## Troubleshooting 2 — Incorrect Sysmon Source Query

### Problem

Sysmon events were present in Splunk when searching:

```spl
index=* EventCode=1
```

However:

```spl
source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
```

returned zero results.

### Initial Assumption

Sysmon logs were not being ingested correctly.

### Investigation

Inspected event metadata inside Splunk.

Observed source:

```text
WinEventLog:Microsoft-Windows-Sysmon/Operational
```

instead of:

```text
XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
```

### Root Cause

Different environments assign different source names.

The issue was with the search query, not with ingestion.

### Resolution

Updated query:

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
```

### Result

Successfully retrieved:

- Event ID 1
- Event ID 3
- Event ID 13
- Event ID 22

### Lesson Learned

Always inspect metadata before assuming ingestion problems.


