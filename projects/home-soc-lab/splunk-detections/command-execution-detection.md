# Command Execution Detection Rule

---

# Detection Information

| Field | Value |
|--------|-------|
| Detection Name | Windows Process Creation Monitoring |
| Detection ID | DET-004 |
| Severity | Medium |
| Status | Active |
| Author | Ajaydev S |

---

# Objective

Detect process creation events on Windows endpoints using Sysmon Event ID 1 to identify potentially suspicious command execution and post-authentication activity.

The detection focuses on monitoring process creation, parent-child relationships, command-line arguments, and execution context.

---

# MITRE ATT&CK Mapping

| Field | Value |
|--------|-------|
| Tactic | Execution |
| Technique | T1059 – Command and Scripting Interpreter |

---

# Data Sources

## Primary Telemetry

- Sysmon Event ID 1
- Splunk Enterprise

---

## Supporting Telemetry

- Windows Security Log
- PowerShell Operational Log
- Windows Defender Logs

---

# Detection Logic

Generate an alert when:

- Sysmon Event ID 1 is generated for command interpreters.
- PowerShell executes unexpectedly.
- Command Prompt launches suspicious child processes.
- Command-line arguments indicate administrative or reconnaissance activity.
- Parent-child process relationships deviate from expected behavior.

---

# Splunk Searches

## Process Creation Events

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
EventCode=1
| table _time Image ParentImage CommandLine User
```

---

## PowerShell Execution

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
EventCode=1
Image="*powershell.exe"
| table _time User CommandLine ParentImage
```

---

## Command Prompt Execution

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
EventCode=1
Image="*cmd.exe"
| table _time User ParentImage CommandLine
```

---

# Expected Alert

## Alert Title

```
Suspicious Windows Command Execution Detected
```

---

## Alert Description

```
Sysmon detected the execution of a command interpreter or administrative utility.

Review the process, command line, parent process, and execution context to determine whether the activity is expected or requires further investigation.
```

---

# Alert Metadata

| Field | Value |
|--------|-------|
| Severity | Medium |
| Confidence | Medium |
| ATT&CK Technique | T1059 |
| Trigger | Sysmon Event ID 1 |
| Response | Investigate Process Execution |

---

# Investigation Steps

1. Review Sysmon Event ID 1.
2. Verify the executable image.
3. Review the parent process.
4. Examine the command line.
5. Identify the executing user.
6. Check the integrity level.
7. Determine whether the process is expected.
8. Investigate any follow-on activity.

---

# False Positives

Legitimate process creation may occur due to:

- Normal user activity.
- Administrative tasks.
- Software updates.
- Scheduled maintenance.
- IT support operations.

Analysts should evaluate process execution within the broader system context.

---

# Tuning Recommendations

- Baseline normal process execution.
- Monitor privileged user activity separately.
- Alert on uncommon parent-child relationships.
- Detect encoded PowerShell commands.
- Correlate process creation with authentication events.

---

# Detection Validation

## Lab Validation

- Executed `whoami`, `hostname`, `ipconfig`, and `powershell`.
- Sysmon generated Event ID 1 for each process.
- Splunk successfully indexed the events.
- Process metadata matched the executed commands.

---

# Related Documents

- `logs-analysis/command-execution-analysis.md`
- `mitre-mapping/command-execution-mitre.md`
- `incident-report/command-execution-incident-report.md`
- `playbooks/command-execution-playbook.md`

---

Template Version: 2.0

Last Updated: 2026-07-10

Author: Ajaydev S