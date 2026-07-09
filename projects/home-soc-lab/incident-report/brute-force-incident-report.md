# Incident Report

---

# 1. Incident Information

| Field | Value |
|--------|-------|
| Incident ID | HSL-IR-2026-002 |
| Severity | Medium |
| Status | Closed |
| Date | 09 July 2026 |
| Analyst | Ajaydev S |

---

# 2. Executive Summary

A controlled brute-force authentication attack was performed against the Windows 10 endpoint using Hydra over SMB2 to evaluate authentication logging and detection capabilities.

The attacker attempted multiple passwords against the local account `socuser`. Windows Security auditing generated Event ID 4625 for each failed authentication attempt and Event ID 4624 after successful authentication using the correct password.

Splunk Enterprise successfully collected and correlated the authentication events, allowing the complete attack sequence to be reconstructed.

The activity was determined to be **Benign Security Testing** performed within the Home SOC Lab.

---

# 3. Incident Overview

| Field | Value |
|--------|-------|
| Attack Phase | Credential Access |
| ATT&CK Technique | T1110.001 – Password Guessing |
| Source IP | 10.10.10.10 |
| Target | Windows 10 (`socuser`) |
| Detection Source | Windows Security Log & Splunk Enterprise |

---

# 4. Timeline

| Time | Activity |
|------|----------|
| 12:24:51 | Hydra initiated SMB authentication attempts. |
| 12:24:51 | Five failed authentication attempts generated Event ID 4625. |
| 12:24:51 | Successful authentication generated Event ID 4624. |
| 12:24:52 | Splunk ingested the authentication events. |
| 12:25:00 | Authentication attack confirmed and investigation completed. |

---

# 5. Investigation Summary

The investigation confirmed that Hydra performed a password guessing attack against the `socuser` account over SMB2.

Five failed authentication attempts generated Windows Security Event ID 4625 before the correct password was identified. A successful Event ID 4624 immediately followed, confirming successful authentication.

Splunk correlated the complete authentication sequence using Windows Security telemetry.

---

# 6. Impact Assessment

## Systems Affected

- Windows 10 Endpoint

---

## Business Impact

No business impact occurred.

The activity was performed as part of an authorized Home SOC Lab exercise.

---

## Security Impact

The attack successfully authenticated using valid credentials after repeated password guessing.

Although this occurred in a controlled environment, the observed behavior demonstrates how compromised credentials can lead to unauthorized network access if password attacks are not detected.

---

# 7. Containment & Response

No containment actions were required because the activity was part of an authorized security exercise.

The following actions were performed:

- Verified Windows Security authentication events.
- Correlated Event IDs 4625 and 4624 in Splunk.
- Confirmed the source IP and workstation.
- Validated authentication metadata.
- Closed the investigation as Benign Security Testing.

---

# 8. Recommendations

The investigation identified the following recommendations:

- Monitor repeated Event ID 4625 events.
- Correlate failed authentication attempts with Event ID 4624.
- Configure threshold-based alerts for password guessing.
- Enable account lockout policies where appropriate.
- Continue forwarding Windows Security logs to Splunk.

---

# 9. Lessons Learned

- Windows Security auditing provides comprehensive authentication telemetry.
- Multiple failed logons followed by a successful logon strongly indicate password guessing activity.
- Correlation of authentication events significantly improves detection confidence.
- Dedicated local test accounts simplify repeatable authentication testing.

---

# 10. Related Documents

| Document | Reference |
|----------|-----------|
| Investigation | `logs-analysis/brute-force-analysis.md` |
| MITRE Mapping | `mitre-mapping/brute-force-mitre.md` |
| Detection Rule | `splunk-detections/brute-force-detection.md` |
| Playbook | `playbooks/brute-force-playbook.md` |

---

Template Version: 2.0

Last Updated: 2026-07-09

Author: Ajaydev S