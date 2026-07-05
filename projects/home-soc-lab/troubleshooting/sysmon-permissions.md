## Troubleshooting 1 — Sysmon Events Missing in Splunk

### Problem

Sysmon events were visible in Event Viewer but did not appear in Splunk.

The following logs were successfully ingested:

- Security
- System
- Application

However, Sysmon Operational logs were missing.

### Initial Assumption

The Universal Forwarder configuration might be incorrect.

### Investigation

Verified:

```cmd
splunk list inputstatus
```

Observed:

```text
splunk-winevtlog.exe
```

running successfully.

Examined:

```cmd
findstr /i "Microsoft-Windows-Sysmon" splunkd.log
```

Found:

```text
Could not subscribe to Windows Event Log channel
errorCode=5
```

### Root Cause

The SplunkForwarder service was running under:

```text
NT SERVICE\SplunkForwarder
```

This account could read standard Windows Event Logs but lacked permission to access:

```text
Microsoft-Windows-Sysmon/Operational
```

### Resolution

Changed the SplunkForwarder service account to:

```text
Local System
```

Restarted the service.

### Result

Sysmon events immediately began reaching Splunk.

### Lesson Learned

Always investigate `splunkd.log` before changing configurations.

Missing logs may be caused by permission issues rather than incorrect inputs.


## Troubleshooting 3 — WinEventLog Inputs Not Visible in splunk list monitor

### Problem

Running:

```cmd
splunk list monitor
```

did not display:

- WinEventLog://Security
- WinEventLog://System
- WinEventLog://Application
- WinEventLog://Microsoft-Windows-Sysmon/Operational

### Initial Assumption

Inputs were not configured correctly.

### Investigation

Executed:

```cmd
splunk list inputstatus
```

Observed:

```text
splunk-winevtlog.exe
```

actively reading Windows Event Logs.

### Root Cause

`splunk list monitor` primarily displays file-based monitors.

Windows Event Logs are handled separately by:

```text
splunk-winevtlog.exe
```

### Resolution

Used:

```cmd
splunk list inputstatus
```

instead of:

```cmd
splunk list monitor
```

to verify Event Log ingestion.

### Lesson Learned

Different Splunk input types require different verification methods.



