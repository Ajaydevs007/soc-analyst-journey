# MITRE ATT&CK Mapping — Registry Run Key Persistence

---

# 1. Technique Information

| Field | Value |
|--------|-------|
| Technique ID | T1547.001 |
| Technique Name | Registry Run Keys / Startup Folder |
| Tactic | Persistence |

---

# 2. Why This Technique Applies

The investigation confirmed that a new registry value named **HomeSOCLab** was created under the current user's **Run** registry key.

The registry value points to:

```
C:\Windows\System32\notepad.exe
```

Although Notepad is a legitimate Windows application, attackers commonly abuse the same Windows autorun mechanism to execute malware automatically whenever a user logs on.

Windows processes the **Run** registry key during user logon, making it a widely used persistence mechanism.

The observed activity aligns with **MITRE ATT&CK Technique T1547.001 – Registry Run Keys / Startup Folder**.

---

# 3. Supporting Evidence

## Evidence Sources

- Windows Registry
- Sysmon Event ID 13
- Sysmon Event ID 1
- Event Viewer
- Splunk Enterprise

---

### Evidence Summary

The investigation confirmed:

- Registry Run Key modification.
- Sysmon Event ID 13 detected the registry value creation.
- Sysmon Event ID 1 identified `reg.exe` as the modifying process.
- Splunk successfully centralized the telemetry.
- Persistence was established through the Windows Run registry key.

---

# 4. Observed Telemetry

| Source | Evidence |
|---------|----------|
| Sysmon Event ID 13 | Registry Value Set |
| Sysmon Event ID 1 | Process Creation (`reg.exe`) |
| Registry | Run Key Persistence |
| Splunk Enterprise | Indexed Registry Modification |
| Event Viewer | Registry Event Details |

---

# 5. Detection Opportunities

Persistence should be investigated when:

- Registry Run Keys are modified.
- New autorun values are created.
- `reg.exe` modifies persistence locations.
- PowerShell modifies autorun registry keys.
- Registry modifications follow suspicious authentication or command execution activity.

Correlating registry changes with process creation provides stronger detection capabilities.

---

# 6. Detection Limitations

Registry modifications are not always malicious.

Legitimate software installations, application updates, and system configuration changes frequently modify Windows autorun locations.

Analysts should evaluate:

- Executable path
- Parent process
- User context
- Command line
- Digital signature
- Overall attack timeline

before determining malicious intent.

---

# 7. Defensive Recommendations

To improve persistence detection:

- Deploy Sysmon with registry monitoring enabled.
- Forward Sysmon logs to Splunk Enterprise.
- Monitor Registry Run and RunOnce keys.
- Alert on unexpected autorun entries.
- Correlate registry modifications with process creation and authentication events.
- Baseline common autorun entries to reduce false positives.

---

# References

## MITRE ATT&CK

- T1547.001 – Registry Run Keys / Startup Folder
- Persistence Tactic

---

## Technical References

- Microsoft Registry Documentation
- Sysinternals Sysmon Documentation
- Splunk Documentation

---

Template Version: 2.0

Last Updated: 2026-07-11

Author: Ajaydev S