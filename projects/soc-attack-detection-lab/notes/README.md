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
