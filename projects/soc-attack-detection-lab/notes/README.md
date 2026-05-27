## Splunk Configuration

Configured Splunk Enterprise to receive forwarded telemetry on TCP port 9997 from remote endpoints.

## Connectivity Validation

Validated end-to-end connectivity between the Windows victim VM and Splunk Enterprise host over TCP port 9997 before deploying Splunk Universal Forwarder.

## Sysmon Preparation

Downloaded Sysmon from Microsoft Sysinternals and added the SwiftOnSecurity Sysmon configuration template to enable enhanced Windows telemetry collection for SOC monitoring.
