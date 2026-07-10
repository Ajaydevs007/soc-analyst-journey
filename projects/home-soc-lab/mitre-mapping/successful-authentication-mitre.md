# MITRE ATT&CK Mapping — Successful Authentication Using Valid Credentials

---

# 1. Technique Information

| Field | Value |
|--------|-------|
| Technique ID | T1078 |
| Technique Name | Valid Accounts |
| Tactic | Initial Access |

---

# 2. Why This Technique Applies

Following a successful password guessing attack, the attacker authenticated to the Windows SMB service using valid credentials for the local account `socuser`.

Windows Security generated Event ID **4624 (Successful Logon)**, confirming that Windows accepted the supplied credentials and granted network access.

Unlike the previous case study, the focus is no longer on obtaining credentials but on the use of valid credentials to access the target system.

This behavior aligns with **MITRE ATT&CK Technique T1078 – Valid Accounts**, where legitimate account credentials are used to gain or maintain access.

---

# 3. Supporting Evidence

## Evidence Sources

- Hydra terminal output
- Windows Security Event ID 4624
- Event Viewer
- Splunk Enterprise

---

### Evidence Summary

The investigation confirmed:

- Successful authentication using the account `socuser`.
- Authentication originated from `10.10.10.10`.
- Workstation identified as `KALI`.
- Authentication Package: NTLM.
- Logon Type: 3 (Network Logon).
- Splunk successfully correlated the authentication event.

These findings demonstrate the successful use of valid credentials to access the Windows endpoint.

---

# 4. Observed Telemetry

| Source | Evidence |
|---------|----------|
| Windows Security Log | Event ID 4624 (Successful Logon) |
| Windows Security Log | Logon Type 3 (Network) |
| Windows Security Log | NTLM Authentication Package |
| Windows Security Log | Source IP Address |
| Windows Security Log | Workstation Name |
| Splunk Enterprise | Successful authentication correlation |
| Hydra | Successful credential use |

---

# 5. Detection Opportunities

Successful authentication should be investigated when:

- The source IP address is unusual.
- The workstation has not previously authenticated.
- Authentication follows repeated Event ID 4625 failures.
- Administrative or privileged accounts are involved.
- Authentication occurs outside normal operating hours.
- Authentication originates from unexpected network segments.

Analysts should correlate Event ID 4624 with surrounding authentication activity before determining legitimacy.

---

# 6. Detection Limitations

Several factors may limit detection:

- Event ID 4624 is generated for both legitimate and malicious logons.
- Authentication events alone cannot determine attacker intent.
- Internal authentication using valid credentials may appear normal.
- Additional telemetry is required to determine post-authentication behavior.

Correlation with process creation, network activity, and account history is recommended.

---

# 7. Defensive Recommendations

To improve detection and response:

- Monitor successful authentication from unusual hosts.
- Correlate Event ID 4624 with previous Event ID 4625 events.
- Review authentication trends for privileged accounts.
- Investigate successful network logons from unfamiliar workstations.
- Forward Windows Security logs to Splunk Enterprise.
- Correlate authentication with endpoint telemetry such as Sysmon Event ID 1.

---

# References

## MITRE ATT&CK

- T1078 – Valid Accounts
- Initial Access Tactic

---

## Technical References

- Microsoft Windows Security Auditing Documentation
- Microsoft Event ID 4624 Documentation
- Splunk Documentation

---

Template Version: 2.0

Last Updated: 2026-07-09

Author: Ajaydev S