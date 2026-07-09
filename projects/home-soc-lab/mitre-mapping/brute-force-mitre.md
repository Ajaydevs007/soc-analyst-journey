# MITRE ATT&CK Mapping — Brute Force Authentication Attack

---

# 1. Technique Information

| Field | Value |
|--------|-------|
| Technique ID | T1110.001 |
| Technique Name | Password Guessing |
| Tactic | Credential Access |

---

# 2. Why This Technique Applies

The attacker used Hydra to repeatedly attempt authentication against the Windows SMB service using multiple password guesses for a single user account (`socuser`).

Each incorrect password generated Windows Security Event ID 4625 (Failed Logon). After the correct password was supplied, Windows generated Event ID 4624 (Successful Logon), confirming that valid credentials had been obtained through password guessing.

This behavior directly aligns with MITRE ATT&CK Technique **T1110.001 – Password Guessing**, where an attacker repeatedly attempts different passwords until successful authentication is achieved.

---

# 3. Supporting Evidence

## Evidence Sources

- Hydra terminal output
- Windows Security Log
- Splunk Enterprise
- Event Viewer

---

### Evidence Summary

The investigation confirmed that:

- Hydra attempted multiple passwords against the `socuser` account.
- Five failed authentication attempts generated Event ID 4625.
- One successful authentication generated Event ID 4624.
- All authentication attempts originated from the same source IP (`10.10.10.10`) and workstation (`KALI`).
- Authentication occurred using the NTLM authentication package over SMB.

These findings are consistent with a password guessing attack.

---

# 4. Observed Telemetry

| Source | Evidence |
|---------|----------|
| Windows Security Log | Event ID 4625 (Failed Logon) |
| Windows Security Log | Event ID 4624 (Successful Logon) |
| Windows Security Log | Logon Type 3 (Network Logon) |
| Windows Security Log | NTLM Authentication Package |
| Windows Security Log | Status `0xC000006D` |
| Windows Security Log | SubStatus `0xC000006A` |
| Splunk Enterprise | Correlated authentication timeline |
| Hydra | Successful password discovery |

---

# 5. Detection Opportunities

The following behaviors may indicate password guessing attacks:

- Multiple Event ID 4625 events targeting the same account.
- Authentication attempts originating from a single source IP.
- Repeated failures followed immediately by Event ID 4624.
- Network logons (Logon Type 3) using NTLM authentication.
- Authentication attempts occurring within a short time interval.

Correlation of these indicators significantly increases confidence that a brute-force attack is occurring.

---

# 6. Detection Limitations

Several factors may limit detection:

- A small number of failed attempts may not exceed alert thresholds.
- Distributed password attacks from multiple IP addresses may evade simple threshold-based detections.
- Legitimate users occasionally mistype passwords, producing Event ID 4625.
- Authentication events alone cannot determine attacker intent without additional context.

Correlation across multiple events and time windows is recommended.

---

# 7. Defensive Recommendations

To improve detection and response:

- Enable auditing for successful and failed logon events.
- Forward Windows Security logs to Splunk Enterprise.
- Configure alerts for repeated Event ID 4625 events.
- Correlate failed logons with subsequent successful authentication.
- Implement account lockout policies where appropriate.
- Monitor authentication attempts from unusual hosts or workstations.
- Regularly review authentication logs for anomalous behavior.

---

# References

## MITRE ATT&CK

- T1110.001 – Password Guessing
- Credential Access Tactic

---

## Technical References

- Microsoft Windows Security Auditing Documentation
- Microsoft Event ID 4624 Documentation
- Microsoft Event ID 4625 Documentation
- Splunk Documentation
- Hydra Documentation

---

Template Version: 2.0

Last Updated: 2026-07-09

Author: Ajaydev S