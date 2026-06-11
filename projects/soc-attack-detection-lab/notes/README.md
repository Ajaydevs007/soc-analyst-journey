## Splunk Configuration

Configured Splunk Enterprise to receive forwarded telemetry on TCP port 9997 from remote endpoints.

## Connectivity Validation

Validated end-to-end connectivity between the Windows victim VM and Splunk Enterprise host over TCP port 9997 before deploying Splunk Universal Forwarder.

## Sysmon Preparation

Downloaded Sysmon from Microsoft Sysinternals and extracted the binaries on the Windows victim VM.

Downloaded the SwiftOnSecurity Sysmon configuration template and placed the configuration file inside the Sysmon directory to enable enhanced telemetry collection.

## Sysmon Installation

Installed Sysmon on the Windows victim VM using the SwiftOnSecurity configuration template.

Verified successful installation of:
- Sysmon service
- Sysmon driver
- telemetry configuration

## Sysmon Service Verification

Verified that the Sysmon service was successfully running after installation on the Windows victim VM.

## Sysmon Event Verification

Verified that Sysmon Operational logs were successfully being generated inside Event Viewer after installation.

Observed multiple Sysmon Event ID 1 (Process Creation) events confirming active telemetry collection.

## Initial Telemetry Generation

Executed basic Windows enumeration commands to intentionally generate Sysmon process creation telemetry for validation and later Splunk ingestion testing.

Commands executed:
- whoami
- hostname
- ipconfig

## Sysmon Process Creation Investigation

Investigated Sysmon Event ID 1 logs generated from manually executed Windows commands.

Observed:
- Process Image
- CommandLine
- ParentImage
- User context

This validated that Sysmon was successfully capturing detailed process execution telemetry.

## Splunk Universal Forwarder Preparation

Downloaded Splunk Universal Forwarder on the Windows victim VM to enable forwarding of Windows and Sysmon telemetry to Splunk Enterprise.

## Splunk Universal Forwarder Installation

Installed Splunk Universal Forwarder on the Windows victim VM.

Configured the forwarder to send telemetry to the Splunk Enterprise host at:
192.168.29.237:9997

## Splunk Forwarder Service Verification

Verified that the Splunk Universal Forwarder service was successfully running on the Windows victim VM after installation.

## Splunk Log Collection Configuration

Configured Splunk Universal Forwarder to collect:
- Windows Application logs
- Security logs
- System logs
- Sysmon Operational logs

All logs were configured to forward into the Splunk "main" index.

## Splunk Forwarder Restart

Restarted the Splunk Universal Forwarder service to apply the updated Windows and Sysmon log collection configuration.

## Sysmon Telemetry Validation in Splunk
Verified that Sysmon Operational logs were successfully reaching Splunk after configuring the Universal Forwarder.

Query used:

index=windows_lab source="WinEventLog:Microsoft-Windows-Sysmon/Operational"

Observed multiple Sysmon events inside Splunk confirming successful telemetry ingestion from the Windows victim VM.

Event ID 1 - Process Creation

Executed several processes manually from Command Prompt to generate Sysmon Process Creation events.

Commands executed:

cmd
notepad
calc
mspaint

Splunk query used:

index=windows_lab EventCode=1 ParentImage="*cmd.exe"
| table _time Image CommandLine ParentImage

Observed parent-child relationships between cmd.exe and the spawned processes.
