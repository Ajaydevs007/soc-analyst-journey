# MITRE ATT&CK Mapping — Command Execution

---

# 1. Technique Information

| Field | Value |
|--------|-------|
| Technique ID | T1059 |
| Technique Name | Command and Scripting Interpreter |
| Tactic | Execution |

---

# 2. Why This Technique Applies

The investigation confirmed that commands were executed through the Windows Command Prompt and PowerShell, resulting in Sysmon Event ID 1 (Process Creation).

Processes including `whoami.exe`, `hostname.exe`, `ipconfig.exe`, and `powershell.exe` were created and recorded by Sysmon.

This activity aligns with **MITRE ATT&CK Technique T1059 – Command and Scripting Interpreter**, which describes adversaries executing commands through command-line interpreters or scripting environments to interact with a compromised system.

Although the commands executed during this investigation were benign and part of an authorized Home SOC Lab exercise, the same telemetry would be generated if an attacker executed commands after gaining access to a system.

---

# 3. Supporting Evidence

## Evidence Sources

- Windows Command Prompt
- Sysmon Event ID 1
- Event Viewer
- Splunk Enterprise

---

### Evidence Summary

The investigation confirmed:

- Command execution through Windows Command Prompt.
- Process creation recorded by Sysmon Event ID 1.
- Parent-child relationship between `cmd.exe` and child processes.
- Process execution under the user account `DESKTOP-MMAR6A0\ajayd`.
- Successful ingestion of telemetry into Splunk Enterprise.

---

# 4. Observed Telemetry

| Source | Evidence |
|---------|----------|
| Sysmon | Event ID 1 – Process Creation |
| Sysmon | Image |
| Sysmon | Parent Image |
| Sysmon | Command Line |
| Sysmon | User |
| Sysmon | Current Directory |
| Sysmon | Integrity Level |
| Splunk Enterprise | Indexed Sysmon Event |

---

# 5. Detection Opportunities

Process execution should be investigated when:

- PowerShell launches unexpectedly.
- Command Prompt executes suspicious commands.
- Parent-child process relationships appear abnormal.
- Administrative utilities execute from unusual locations.
- LOLBins are executed.
- Command-line arguments indicate malicious behavior.

Correlating process creation with authentication and network telemetry provides stronger detection capabilities.

---

# 6. Detection Limitations

Several limitations should be considered:

- Many administrative commands are legitimate.
- Process creation alone does not indicate malicious intent.
- User context must be evaluated.
- Parent-child relationships require contextual analysis.
- Additional telemetry is required to determine post-execution activity.

---

# 7. Defensive Recommendations

To improve endpoint visibility:

- Deploy Sysmon with a well-maintained configuration.
- Forward Sysmon logs to Splunk Enterprise.
- Monitor command-line arguments.
- Build baselines for common administrative processes.
- Detect suspicious parent-child process relationships.
- Correlate process creation with authentication events and network activity.

---

# References

## MITRE ATT&CK

- T1059 – Command and Scripting Interpreter
- Execution Tactic

---

## Technical References

- Sysinternals Sysmon Documentation
- Microsoft Windows Documentation
- Splunk Documentation

---

Template Version: 2.0

Last Updated: 2026-07-10

Author: Ajaydev S