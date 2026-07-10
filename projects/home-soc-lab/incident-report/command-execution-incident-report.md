# Incident Report

---

# 1. Incident Information

| Field | Value |
|--------|-------|
| Incident ID | HSL-IR-2026-004 |
| Severity | Medium |
| Status | Closed |
| Date | 10 July 2026 |
| Analyst | Ajaydev S |

---

# 2. Executive Summary

Windows Sysmon Event ID 1 (Process Creation) was generated after several commands were executed on the Windows endpoint during an authorized Home SOC Lab exercise.

Commands including `whoami`, `hostname`, `ipconfig`, and PowerShell generated detailed process creation telemetry. Sysmon recorded the executable image, parent process, command line, user context, integrity level, and process identifiers. Splunk Enterprise successfully collected and indexed these events, allowing centralized endpoint investigation.

The activity was confirmed as authorized security testing and classified as **Benign Security Testing**.

---

# 3. Incident Overview

| Field | Value |
|--------|-------|
| Attack Phase | Execution |
| ATT&CK Technique | T1059 – Command and Scripting Interpreter |
| Endpoint | Windows 10 |
| User | DESKTOP-MMAR6A0\ajayd |
| Detection Source | Sysmon Event ID 1 & Splunk Enterprise |

---

# 4. Timeline

| Time | Activity |
|------|----------|
| Replace with actual time | User executed `whoami`. |
| Replace with actual time | Sysmon generated Event ID 1. |
| Replace with actual time | Splunk indexed the process creation event. |
| Replace with actual time | Additional commands executed (`hostname`, `ipconfig`, `powershell`). |

---

# 5. Investigation Summary

The investigation confirmed that command execution occurred from an interactive Windows Command Prompt session.

Sysmon Event ID 1 recorded:

- Image: `C:\Windows\System32\whoami.exe`
- Parent Image: `C:\Windows\System32\cmd.exe`
- User: `DESKTOP-MMAR6A0\ajayd`
- Command Line: `whoami`
- Current Directory: `C:\Users\ajayd\`
- Integrity Level: Medium

Splunk successfully centralized the telemetry, enabling detailed review of the process execution and execution context.

---

# 6. Impact Assessment

## Systems Affected

- Windows 10 Endpoint

---

## Business Impact

No business impact occurred.

The activity was performed during an authorized Home SOC Lab exercise.

---

## Security Impact

The investigation demonstrated how Sysmon Event ID 1 provides detailed visibility into process execution on Windows endpoints.

Although the commands executed were legitimate, similar telemetry could indicate malicious command execution in a production environment and should be investigated within the surrounding context.

---

# 7. Containment & Response

No containment actions were required because the activity was part of an authorized security exercise.

The following actions were completed:

- Verified Sysmon Event ID 1.
- Reviewed parent-child process relationships.
- Confirmed command-line arguments.
- Verified executing user.
- Correlated telemetry within Splunk.
- Closed the investigation as Benign Security Testing.

---

# 8. Recommendations

- Deploy Sysmon across monitored endpoints.
- Forward Sysmon logs to Splunk Enterprise.
- Monitor command interpreters and PowerShell activity.
- Investigate unusual parent-child process relationships.
- Correlate process creation with authentication and network activity.

---

# 9. Lessons Learned

- Sysmon Event ID 1 provides rich endpoint telemetry.
- Parent-child relationships are valuable for endpoint investigations.
- Command-line logging enhances forensic visibility.
- Process creation events should be correlated with surrounding activity before determining intent.

---

# 10. Related Documents

| Document | Reference |
|----------|-----------|
| Investigation | `logs-analysis/command-execution-analysis.md` |
| MITRE Mapping | `mitre-mapping/command-execution-mitre.md` |
| Detection Rule | `splunk-detections/command-execution-detection.md` |
| Playbook | `playbooks/command-execution-playbook.md` |

---

Template Version: 2.0

Last Updated: 2026-07-10

Author: Ajaydev S