# Incident Report

---

# 1. Incident Information

| Field | Value |
|--------|-------|
| Incident ID | HSL-IR-2026-005 |
| Severity | High |
| Status | Closed |
| Date | 11 July 2026 |
| Analyst | Ajaydev S |

---

# 2. Executive Summary

A Registry Run Key was intentionally created on the Windows endpoint as part of an authorized Home SOC Lab exercise to simulate a common Windows persistence technique.

The registry value **HomeSOCLab** was added under the current user's **Run** registry key and configured to launch `notepad.exe` automatically during user logon.

Sysmon successfully detected the activity through **Event ID 13 (Registry Value Set)** and **Event ID 1 (Process Creation)**. Splunk Enterprise successfully ingested both events, allowing centralized investigation and correlation.

The activity was confirmed as **Authorized Security Testing**.

---

# 3. Incident Overview

| Field | Value |
|--------|-------|
| Attack Phase | Persistence |
| ATT&CK Technique | T1547.001 – Registry Run Keys / Startup Folder |
| Endpoint | Windows 10 |
| User | DESKTOP-MMAR6A0\ajayd |
| Detection Source | Sysmon Event ID 13 & Event ID 1 |

---

# 4. Timeline

| Time | Activity |
|------|----------|
| Replace with actual time | `reg.exe` executed. |
| Replace with actual time | Registry value **HomeSOCLab** created. |
| Replace with actual time | Sysmon generated Event ID 13. |
| Replace with actual time | Splunk indexed the registry modification event. |
| Replace with actual time | Registry value verified using `reg query`. |

---

# 5. Investigation Summary

The investigation confirmed that a new Registry Run Key was created within the current user's autorun location.

### Registry Value

```
HomeSOCLab
```

### Registry Path

```
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

### Configured Executable

```
C:\Windows\System32\notepad.exe
```

Sysmon Event ID 13 recorded the registry value modification, while Event ID 1 identified `reg.exe` as the process responsible for the change.

Splunk successfully centralized the telemetry, allowing analysts to reconstruct the persistence activity.

---

# 6. Impact Assessment

## Systems Affected

- Windows 10 Endpoint

---

## Business Impact

No business impact occurred.

The registry modification was performed as part of an authorized Home SOC Lab exercise.

---

## Security Impact

The investigation demonstrated how registry-based persistence can be established using legitimate Windows functionality.

Although the configured executable (`notepad.exe`) was benign, attackers frequently abuse the same persistence mechanism to automatically execute malicious payloads after user logon.

---

# 7. Containment & Response

No containment actions were required because the activity was authorized.

The following actions were completed:

- Verified Sysmon Event ID 13.
- Correlated Event ID 13 with Event ID 1.
- Confirmed the configured executable.
- Verified the executing user.
- Reviewed the registry path.
- Documented the persistence mechanism.
- Closed the investigation as Authorized Security Testing.

---

# 8. Recommendations

- Enable Sysmon registry monitoring across endpoints.
- Forward Sysmon logs to Splunk Enterprise.
- Monitor Run and RunOnce registry keys.
- Investigate unexpected autorun entries.
- Correlate registry modifications with process creation and authentication events.

---

# 9. Lessons Learned

- Registry Run Keys are a common persistence technique.
- Event ID 13 provides detailed registry modification visibility.
- Event ID 1 identifies the process responsible for registry changes.
- Correlating multiple telemetry sources improves investigation accuracy.
- Legitimate Windows features can be abused for persistence.

---

# 10. Related Documents

| Document | Reference |
|----------|-----------|
| Investigation | `logs-analysis/persistence-detection-analysis.md` |
| MITRE Mapping | `mitre-mapping/persistence-registry-runkey-mitre.md` |
| Detection Rule | `splunk-detections/persistence-registry-runkey-detection.md` |
| Playbook | `playbooks/persistence-registry-runkey-playbook.md` |

---

Template Version: 2.0

Last Updated: 2026-07-11

Author: Ajaydev S