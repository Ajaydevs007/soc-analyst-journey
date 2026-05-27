## Splunk Configuration

Configured Splunk Enterprise to receive forwarded telemetry on TCP port 9997 from remote endpoints.

## Connectivity Validation

Validated end-to-end connectivity between the Windows victim VM and Splunk Enterprise host over TCP port 9997 before deploying Splunk Universal Forwarder.

## Sysmon Preparation

Downloaded Sysmon from Microsoft Sysinternals and extracted the binaries on the Windows victim VM.

Downloaded the SwiftOnSecurity Sysmon configuration template and placed the configuration file inside the Sysmon directory to enable enhanced telemetry collection.
