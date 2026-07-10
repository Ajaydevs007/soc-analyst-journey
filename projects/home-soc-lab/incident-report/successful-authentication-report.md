# Incident Report

---

# 1. Incident Information

| Field | Value |
|--------|-------|
| Incident ID | HSL-IR-2026-003 |
| Severity | Medium |
| Status | Closed |
| Date | 09 July 2026 |
| Analyst | Ajaydev S |

---

# 2. Executive Summary

A successful Windows network authentication was detected for the local account `socuser` following a controlled password guessing exercise performed using Hydra over SMB2.

Windows Security generated Event ID **4624 (Successful Logon)** after valid credentials were supplied. Splunk Enterprise successfully collected and correlated the authentication event, allowing verification of the authenticated user, source IP address, workstation name, authentication package, and logon type.

The investigation confirmed that the activity was part of an authorized Home SOC Lab exercise and was classified as **Benign Security Testing**.

---

# 3. Incident Overview

| Field | Value |
|--------|-------|
| Attack Phase | Initial Access |
| ATT&CK Technique | T1078 – Valid Accounts |
| Source IP | 10.10.10.10 |
| Target | Windows 10 (`socuser`) |
| Detection Source | Windows Security Log & Splunk Enterprise |

---

# 4. Timeline

| Time | Activity |
|------|----------|
| 12:24:51 | Hydra successfully authenticated using valid credentials. |
| 12:24:51 | Windows generated Event ID 4624. |
| 12:24:52 | Splunk indexed the authentication event. |
| 12:25:00 | Authentication investigation completed. |

---

# 5. Investigation Summary

The investigation confirmed that the account `socuser` successfully authenticated from the Kali workstation (`10.10.10.10`) using NTLM over SMB.

Analysis of Event ID 4624 verified:

- Successful network authentication.
- Logon Type 3 (Network).
- Authentication Package: NTLM.
- Workstation: KALI.
- Source IP: 10.10.10.10.

Correlation with previous Event ID 4625 activity confirmed that the successful authentication followed repeated password guessing attempts.

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

Successful authentication using valid credentials demonstrates how attackers can transition from credential access to post-authentication activity.

Although this exercise was authorized, similar authentication events in production environments should be investigated immediately.

---

# 7. Containment & Response

No containment actions were required because the activity was part of an authorized security exercise.

The following actions were completed:

- Verified Event ID 4624.
- Confirmed authentication metadata.
- Correlated successful authentication with previous failed logons.
- Verified source IP and workstation.
- Closed the investigation as Benign Security Testing.

---

# 8. Recommendations

- Investigate successful logons following repeated failed authentication attempts.
- Monitor successful network logons from unfamiliar workstations.
- Correlate authentication events with endpoint telemetry.
- Continue forwarding Windows Security logs to Splunk.
- Review authentication activity for privileged accounts.

---

# 9. Lessons Learned

- Event ID 4624 requires contextual analysis.
- Successful authentication alone does not confirm legitimate activity.
- Correlation with previous authentication events significantly improves detection accuracy.
- Authentication metadata provides valuable investigative context.

---

# 10. Related Documents

| Document | Reference |
|----------|-----------|
| Investigation | `logs-analysis/successful-authentication-analysis.md` |
| MITRE Mapping | `mitre-mapping/successful-authentication-mitre.md` |
| Detection Rule | `splunk-detections/successful-authentication-detection.md` |
| Playbook | `playbooks/successful-authentication-playbook.md` |

---

Template Version: 2.0

Last Updated: 2026-07-09

Author: Ajaydev S