# Command Execution Investigation Playbook

---

# Playbook Information

| Field | Value |
|--------|-------|
| Playbook ID | PB-004 |
| Playbook Name | Windows Command Execution Investigation |
| Severity | Medium |
| ATT&CK Technique | T1059 – Command and Scripting Interpreter |
| Author | Ajaydev S |

---

# Objective

Provide a structured investigation workflow for Windows process creation events recorded by Sysmon Event ID 1. The objective is to determine whether command execution represents legitimate administrative activity or potentially malicious behavior.

---

# Trigger

Initiate this playbook when:

- Sysmon Event ID 1 is generated.
- Command Prompt (`cmd.exe`) is executed.
- PowerShell (`powershell.exe`) is launched.
- Administrative utilities are executed.
- Unusual parent-child process relationships are detected.
- Suspicious command-line arguments are observed.

---

# Initial Triage

Collect the following information:

- Executable image
- Parent process
- Command line
- Executing user
- Current directory
- Integrity level
- Process ID
- Parent Process ID
- Process GUID
- Execution time

---

# Investigation Steps

## Step 1 — Review Sysmon Event ID 1

Verify:

- Executable image
- Parent image
- Command line
- User
- Current directory
- Integrity level
- Process identifiers

---

## Step 2 — Validate Process Context

Determine:

- Is the process expected?
- Does the parent process make sense?
- Is the command line normal?
- Is the executing user authorized?
- Is the execution location expected?

---

## Step 3 — Review Parent-Child Relationship

Confirm whether the process hierarchy is expected.

Examples:

Expected:

```text
explorer.exe
    └── cmd.exe
            └── whoami.exe
```

Potentially Suspicious:

```text
winword.exe
    └── powershell.exe
```

```text
excel.exe
    └── cmd.exe
```

---

## Step 4 — Review Additional Telemetry

Correlate with:

- Windows Security Log
- Previous authentication events
- PowerShell Operational Log
- Network connections (Sysmon Event ID 3)
- File creation events
- Registry modifications

---

## Step 5 — Determine Legitimacy

Classify the activity as:

- Expected user activity
- Administrative activity
- Authorized security testing
- Suspicious process execution
- Confirmed malicious execution

---

# Response Actions

If malicious activity is confirmed:

- Isolate the endpoint if required.
- Preserve Sysmon logs.
- Review additional process creation events.
- Investigate child processes.
- Review network activity.
- Escalate to Incident Response.

If activity is authorized:

- Document the executed commands.
- Record supporting evidence.
- Close the investigation.
- Update investigation notes.

---

# Evidence to Collect

- Sysmon Event ID 1
- Event Viewer screenshots
- Splunk search results
- Process metadata
- Parent-child relationship
- Command line
- User context
- Timeline reconstruction

---

# Escalation Criteria

Escalate when:

- PowerShell executes encoded commands.
- Parent-child relationships are abnormal.
- LOLBins are executed unexpectedly.
- Administrative utilities launch from unusual processes.
- Process execution is followed by persistence or network activity.

---

# Detection References

Primary Detection

- Sysmon Event ID 1

Supporting Information

- Image
- Parent Image
- Command Line
- User
- Integrity Level
- Process GUID
- Parent Process ID

---

# Related Documents

- `logs-analysis/command-execution-analysis.md`
- `mitre-mapping/command-execution-mitre.md`
- `splunk-detections/command-execution-detection.md`
- `incident-report/command-execution-incident-report.md`

---

# Lessons for Analysts

- Process creation events provide valuable endpoint visibility.
- Parent-child process relationships are essential for identifying suspicious execution chains.
- Command-line arguments often reveal attacker intent.
- Process creation should always be correlated with authentication, network, and persistence telemetry.
- Context is critical when distinguishing legitimate administration from malicious activity.

---

Playbook Version: 2.0

Last Updated: 2026-07-10

Author: Ajaydev S