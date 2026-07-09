# Brute Force Detection Rule

---

# Detection Information

| Field | Value |
|--------|-------|
| Detection Name | Windows Brute Force Authentication Detection |
| Detection ID | DET-002 |
| Severity | Medium |
| Status | Active |
| Author | Ajaydev S |

---

# Objective

Detect repeated failed Windows authentication attempts followed by a successful logon from the same source IP and targeting the same user account.

The goal is to identify password guessing attacks before they can be used for privilege escalation or lateral movement.

---

# MITRE ATT&CK Mapping

| Field | Value |
|--------|-------|
| Tactic | Credential Access |
| Technique | T1110.001 – Password Guessing |

---

# Data Sources

Primary Telemetry

- Windows Security Log
- Splunk Enterprise

Supporting Telemetry

- Windows Firewall (optional)
- Active Directory Security Logs (domain environments)

---

# Detection Logic

Generate an alert when:

- Multiple Event ID **4625 (Failed Logon)** events occur
- The same user account is targeted
- The same source IP performs the attempts
- The attempts occur within a short time window
- A successful Event ID **4624 (Successful Logon)** follows the failed attempts

---

# Splunk Search

## Failed Logon Activity

```spl
source="WinEventLog:Security"
EventCode=4625
| stats count by Account_Name Source_Network_Address Workstation_Name
| where count >= 5
```

---

## Failed Logons Followed by Successful Logon

```spl
source="WinEventLog:Security"
(EventCode=4625 OR EventCode=4624)
Account_Name=*
| sort 0 _time
| table _time EventCode Account_Name Source_Network_Address Workstation_Name Logon_Type Authentication_Package_Name
```

---

# Expected Alert

Alert Title

```
Possible Windows SMB Brute Force Attack
```

---

Alert Description

```
Multiple failed authentication attempts were detected against the same user account from a single source IP, followed by a successful network logon.

This behavior is consistent with a password guessing attack.
```

---

# Alert Metadata

| Field | Value |
|--------|-------|
| Severity | Medium |
| Confidence | High |
| ATT&CK Technique | T1110.001 |
| Trigger | Five or more failed logons followed by one successful logon |
| Response | Investigate authentication activity |

---

# Investigation Steps

1. Identify the targeted account.
2. Review Event ID 4625 details.
3. Identify the source IP address.
4. Confirm the workstation name.
5. Review Event ID 4624 for successful authentication.
6. Verify Logon Type.
7. Review Status and SubStatus values.
8. Determine whether the activity is authorized.
9. Escalate if malicious activity is confirmed.

---

# False Positives

Possible legitimate causes include:

- Users repeatedly entering incorrect passwords.
- Password manager synchronization issues.
- Automated scripts using outdated credentials.
- Misconfigured services repeatedly attempting authentication.

---

# Tuning Recommendations

- Increase thresholds in large environments.
- Exclude approved administrative jump hosts.
- Whitelist known service accounts where appropriate.
- Correlate with account lockout events.
- Correlate with endpoint process creation events after successful authentication.

---

# Validation

Lab Validation

- Hydra SMB2 password attack
- Windows Event ID 4625 generated
- Windows Event ID 4624 generated
- Splunk successfully detected authentication events

---

# Related Documents

- `logs-analysis/brute-force-analysis.md`
- `mitre-mapping/brute-force-mitre.md`
- `incident-report/brute-force-incident-report.md`
- `playbooks/brute-force-playbook.md`

---

Template Version: 2.0

Last Updated: 2026-07-09

Author: Ajaydev S