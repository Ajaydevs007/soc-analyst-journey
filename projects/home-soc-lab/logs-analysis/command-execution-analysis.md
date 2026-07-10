# Command Execution Detection & Process Creation Analysis

---

# 1. Investigation Overview

## Objective

Investigate Windows Sysmon Event ID 1 (Process Creation) generated after command execution on the Windows endpoint to determine which processes were executed, identify the user responsible, analyze the execution context, and evaluate the visibility provided by endpoint telemetry.

---

## Scope

### Investigation Status

| Field | Value |
|--------|-------|
| Investigation ID | CS-004 |
| Status | Closed |
| Severity | Medium |
| Disposition | Benign Security Testing |
| Analyst | Ajaydev S |

---

### Investigation Trigger

Splunk identified Windows Sysmon Event ID 1 (Process Creation) after commands were executed on the Windows endpoint. Because process creation events provide valuable visibility into endpoint activity, the events required investigation.

---

### Investigation Goals

- Verify the executed processes.
- Identify the executing user.
- Analyze the parent-child process relationship.
- Review the recorded command lines.
- Evaluate Sysmon process creation telemetry.
- Identify detection opportunities for suspicious process execution.

---

### Time Window

10 July 2026

---

### Systems Involved

- Windows 10 Endpoint
- Splunk Enterprise
- Sysmon
- Event Viewer

---

# 2. Lab Environment

| Component | Details |
|-----------|---------|
| Hypervisor | VirtualBox |
| Endpoint | Windows 10 (10.10.10.20) |
| SIEM | Splunk Enterprise |
| Log Forwarder | Splunk Universal Forwarder |
| Endpoint Telemetry | Sysmon (SwiftOnSecurity Configuration) |
| Network | Internal Network (`soc-lab-net`) |

## Telemetry Sources

- Windows Security Log
- Sysmon Operational Log
- Splunk Enterprise

---

# 3. Attack Overview

## Attack Scenario

Following successful authentication during the previous investigation, several Windows commands were executed on the endpoint to generate process creation telemetry.

Commands including `whoami`, `hostname`, `ipconfig`, and PowerShell were executed to simulate common post-authentication activity and evaluate the visibility provided by Sysmon Event ID 1.

The investigation focuses on understanding how process creation events are recorded and how analysts can use them to investigate endpoint activity.

---

## Attack Details

| Field | Value |
|--------|-------|
| Attack Phase | Execution |
| ATT&CK Tactic | Execution |
| ATT&CK Technique | T1059 – Command and Scripting Interpreter |
| User | socuser |
| Host | Windows 10 |
| Telemetry | Sysmon Event ID 1 |
| Tool | Windows Command Prompt / PowerShell |

---

## Attack Execution

### Commands Executed

```cmd
whoami
hostname
ipconfig
powershell
Get-Date
exit
```

---

### Initial Observation

Each executed command generated Sysmon Event ID 1 (Process Creation), recording detailed process metadata including the executable image, parent process, command line, user account, process identifiers, and execution context.

The events were successfully forwarded to Splunk Enterprise, providing centralized visibility into endpoint process activity.

---

# 4. Evidence Collection

Document all evidence collected during the investigation.

---

## 4.1 Command Execution Evidence

### Commands Executed

```cmd
whoami
hostname
ipconfig
powershell
Get-Date
exit
```

---

### Command Output

The commands executed successfully on the Windows endpoint and generated process creation telemetry through Sysmon Event ID 1.

---

### Screenshots

- step56-process-execution-commands.png

---

### Initial Observations

- Multiple Windows processes were successfully created.
- Each command generated a separate Sysmon Event ID 1.
- Process creation telemetry was available for investigation.
- The executed commands represented common administrative and post-authentication activity.

---

## 4.2 Windows Security Evidence

### Relevant Event IDs

No Windows Security Event IDs were directly associated with command execution.

---

### Event Details

Windows Security logs primarily recorded authentication events during the previous investigation. Process execution visibility for this activity was provided by Sysmon rather than native Windows Security logging.

---

### Observations

Windows Security logs alone do not provide detailed process creation telemetry for this investigation.

---

## 4.3 Sysmon Evidence

### Relevant Event IDs

- Event ID 1 — Process Creation

---

### Event Details

Sysmon generated Event ID 1 for each executed process.

The event captured detailed process metadata including:

- Image
- Parent Image
- Command Line
- User
- Process ID
- Process GUID
- Parent Process GUID
- Current Directory
- Integrity Level
- Hashes

---

### Screenshots

- step57-eventviewer-sysmon-eventid1.png
- step58-eventviewer-process-details.png
- step59-eventviewer-process-xml.png

---

### Observations

Sysmon successfully recorded every executed process, providing comprehensive endpoint telemetry for process creation analysis.

---

## 4.4 Splunk Evidence

### Search Queries

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
EventCode=1
```

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
EventCode=1
| table _time Image ParentImage CommandLine User
```

---

### Search Results

Splunk successfully ingested Sysmon Event ID 1 and indexed the process creation events.

The search results displayed the executed process, parent process, command line, executing user, and execution time, allowing centralized investigation.

---

### Screenshots

- step60-splunk-eventid1-search.png
- step61-splunk-process-details.png
- step62-splunk-process-table.png

---

### Observations

Splunk successfully centralized Sysmon process creation telemetry and enabled efficient process hunting using SPL queries.

---

# 5. Evidence Summary

| Source | Evidence | Value |
|--------|----------|-------|
| Windows Command Prompt | Executed commands | Confirmed command execution |
| Sysmon | Event ID 1 | Confirmed process creation |
| Event Viewer | Process metadata | Detailed execution context |
| Splunk | Indexed Event ID 1 | Centralized investigation |

---

# 6. Investigation

---

## 6.1 Telemetry Analysis

### Windows Command Prompt

The investigation began by reviewing commands executed from the Windows Command Prompt. The executed commands (`whoami`, `hostname`, `ipconfig`, and PowerShell) were intended to simulate common post-authentication activity performed by a user or attacker on the endpoint.

Each command generated a separate Sysmon Event ID 1 (Process Creation), providing detailed visibility into process execution.

---

### Windows Security Log

No Windows Security Event IDs were directly associated with the command execution activity.

This behavior is expected because Windows Security auditing primarily focuses on authentication, authorization, account management, and object access events. Detailed process creation telemetry for this investigation was provided by Sysmon.

---

### Sysmon

Sysmon Event ID 1 successfully recorded the execution of the `whoami.exe` process.

Key process metadata extracted from the event included:

| Field | Observed Value |
|--------|----------------|
| Image | `C:\Windows\System32\whoami.exe` |
| Parent Image | `C:\Windows\System32\cmd.exe` |
| Command Line | `whoami` |
| User | `DESKTOP-MMAR6A0\ajayd` |
| Current Directory | `C:\Users\ajayd\` |
| Integrity Level | Medium |
| Parent Process ID | 5384 |

The telemetry confirmed that `whoami.exe` was launched from an interactive Command Prompt session.

The parent process (`cmd.exe`) indicates that the command was executed manually rather than automatically by another application or scheduled task.

The command line (`whoami`) further confirmed that the process was executed to identify the current security context of the logged-on user.

Because Sysmon records both the child process and its parent process, analysts can reconstruct the process hierarchy and better understand how execution occurred.

---

### Splunk Enterprise

Splunk successfully ingested Sysmon Event ID 1 and indexed the process creation event.

The indexed telemetry included valuable investigative fields such as:

- Image
- Parent Image
- Command Line
- User
- Current Directory
- Process ID
- Parent Process ID
- Integrity Level

Centralizing this information within Splunk enabled efficient endpoint investigation without requiring direct access to the Windows endpoint.

---

## 6.2 Event Correlation

The investigation correlated telemetry collected from Windows Command Prompt, Sysmon, and Splunk Enterprise to reconstruct the command execution activity.

### Step 1 — Command Execution

The user executed the `whoami` command from an interactive Windows Command Prompt session.

---

### Step 2 — Process Creation

Windows created a new process (`whoami.exe`) to execute the command.

Sysmon immediately generated Event ID 1 (Process Creation), recording detailed metadata about the new process.

---

### Step 3 — Parent-Child Relationship

The Sysmon event identified:

- Parent Process: `cmd.exe`
- Child Process: `whoami.exe`

This parent-child relationship confirmed that the process originated from an interactive command shell rather than another executable or background service.

---

### Step 4 — User Context

The process executed under the user account:

`DESKTOP-MMAR6A0\ajayd`

The process ran with **Medium Integrity**, indicating execution within a standard interactive user session.

No evidence suggested privilege escalation or elevated administrative execution.

---

### Step 5 — Centralized Visibility

Splunk successfully indexed the Sysmon event, allowing analysts to review process execution, user context, command line arguments, and process hierarchy from a centralized interface.

---

## 6.3 Root Cause Analysis

The investigation determined that the observed process creation activity resulted from legitimate command execution performed during an authorized Home SOC Lab exercise.

### Root Cause

The user manually executed the `whoami` command from an interactive Command Prompt session.

Windows created the `whoami.exe` process, and Sysmon generated Event ID 1 to record the process creation event.

---

### Why Sysmon Generated Event ID 1

Sysmon Event ID 1 records every newly created process that matches the configured monitoring policy.

When `cmd.exe` launched `whoami.exe`, Sysmon captured:

- Executable image
- Parent process
- Command line
- User account
- Current directory
- Process identifiers
- Integrity level

This provides analysts with detailed visibility into endpoint process execution.

---

### Why the Parent Process Matters

The parent process (`cmd.exe`) provided important investigative context.

Understanding the parent-child relationship allows analysts to determine how a process was started and identify unusual execution chains.

For example:

- `cmd.exe → whoami.exe` is generally expected during interactive administration.
- `winword.exe → powershell.exe` may indicate malicious macro execution.
- `explorer.exe → cmd.exe → powershell.exe` may warrant further investigation depending on context.

Parent-child relationships are therefore a key component of endpoint investigations.

---

### Defensive Control Assessment

Sysmon successfully captured detailed process creation telemetry and forwarded the event to Splunk Enterprise.

The investigation demonstrated that Sysmon Event ID 1 provides significantly richer process execution visibility than standard Windows Security logging.

By correlating process metadata, user context, command lines, and parent-child relationships, analysts can efficiently investigate endpoint activity and identify potentially suspicious behavior.

---

### Investigation Conclusion

The command execution activity was confirmed to be legitimate and part of an authorized Home SOC Lab exercise.

Sysmon Event ID 1 successfully recorded the complete process creation event, while Splunk centralized the telemetry for efficient investigation.

The investigation demonstrated the value of process creation telemetry for endpoint monitoring and highlighted the importance of parent-child process relationships during security investigations.

---

# 7. Timeline Reconstruction

| Time | Activity | Telemetry Source | Evidence |
|------|----------|------------------|----------|
| 11:55:12 | User opened Command Prompt | Windows | Interactive session |
| 11:55:15 | `cmd.exe` launched `whoami.exe` | Sysmon Event ID 1 | Process Creation |
| 11:55:15 | Sysmon generated Event ID 1 | Sysmon | Event Viewer |
| 11:55:16 | Splunk indexed the process creation event | Splunk Enterprise | SPL Search |
| 11:55:30 | Additional commands (`hostname`, `ipconfig`, `powershell`) executed | Sysmon | Process Creation Events |

> Replace the timestamps with the actual times from your Sysmon events.

---

# 8. Findings

## Confirmed Findings

### Finding 1 — Process Creation Successfully Recorded

Sysmon successfully generated Event ID 1 for each executed command, providing detailed process creation telemetry.

---

### Finding 2 — Parent-Child Relationship Confirmed

The investigation confirmed that `whoami.exe` was launched by `cmd.exe`, indicating that the command originated from an interactive command shell.

---

### Finding 3 — Command Line Successfully Captured

Sysmon recorded the executed command (`whoami`), allowing analysts to determine exactly which command was run.

---

### Finding 4 — User Context Identified

The process executed under the user account:

`DESKTOP-MMAR6A0\ajayd`

This provided attribution for the executed process and confirmed the security context.

---

### Finding 5 — Splunk Successfully Centralized Endpoint Telemetry

Splunk successfully indexed Sysmon Event ID 1, enabling centralized investigation of endpoint process execution.

---

## Supporting Evidence

| Finding | Supporting Evidence |
|----------|---------------------|
| Process Creation Confirmed | Sysmon Event ID 1 |
| Parent Process Confirmed | ParentImage (`cmd.exe`) |
| User Attribution | User Field |
| Command Executed | CommandLine |
| Centralized Investigation | Splunk Search Results |

---

# 9. Detection & Telemetry Coverage

| Telemetry Source | Coverage | Notes |
|------------------|----------|------|
| Windows Security Log | ⚠️ Limited | Authentication-focused; no detailed process creation telemetry. |
| Sysmon | ✅ High | Captured executable, parent process, command line, user context, integrity level, and process identifiers. |
| Splunk | ✅ High | Successfully centralized Sysmon process creation events for investigation. |

---

# 10. Detection Opportunities

## Detection Logic

Generate alerts when:

- Unusual processes are executed.
- Suspicious parent-child process relationships are observed.
- Administrative tools are launched unexpectedly.
- PowerShell or scripting engines execute suspicious commands.
- Processes execute from uncommon directories.

---

## Primary Telemetry

- Sysmon Event ID 1
- Splunk Enterprise

---

## Supporting Telemetry

- Windows Security Log
- Windows Defender
- PowerShell Operational Log

---

## Detection Strategy

Correlate:

- Image
- Parent Image
- Command Line
- User
- Integrity Level
- Current Directory
- Process GUID

to identify abnormal process execution patterns.

---

## Detection Improvements

Future improvements include:

- Detect LOLBins (Living-off-the-Land Binaries).
- Monitor suspicious PowerShell execution.
- Detect encoded command lines.
- Build parent-child process baselines.
- Correlate process creation with network connections (Sysmon Event ID 3).

---

# 11. Analyst Reflection

Initially, the investigation focused on verifying that command execution generated endpoint telemetry.

By analyzing Sysmon Event ID 1, I observed how detailed process creation events provide significantly greater visibility than standard Windows Security logging.

The investigation highlighted the importance of parent-child process relationships, command-line arguments, user context, and integrity levels when determining whether process execution is expected or potentially malicious.

This investigation also demonstrated the value of centralized telemetry within Splunk, allowing endpoint activity to be investigated efficiently without relying solely on the local system.

---

# 12. Lessons Learned

- Sysmon Event ID 1 provides comprehensive process creation telemetry.
- Parent-child process relationships are critical for endpoint investigations.
- Command-line logging significantly improves investigative capabilities.
- User context and integrity level help determine execution context.
- Splunk enables efficient process hunting across centralized endpoint telemetry.
- Process creation events are foundational for detecting post-compromise activity.

---

# 13. References

## Documentation References

- Microsoft Windows Security Auditing Documentation
- Sysinternals Sysmon Documentation
- Splunk Documentation
- MITRE ATT&CK Framework

---

## Investigation Evidence

- Windows Command Prompt
- Sysmon Event ID 1
- Event Viewer
- Splunk Search Results
- Project Screenshots

---

## Related Project Documents

- `mitre-mapping/command-execution-mitre.md`
- `splunk-detections/command-execution-detection.md`
- `incident-report/command-execution-incident-report.md`
- `playbooks/command-execution-playbook.md`

---

## Next Investigation

Persistence Detection & Autorun Analysis

---

Template Version: 2.0

Last Updated: 2026-07-10

Author: Ajaydev S