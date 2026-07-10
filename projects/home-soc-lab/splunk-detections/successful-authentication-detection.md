# Successful Authentication Detection Rule

---

# Detection Information

| Field | Value |
|--------|-------|
| Detection Name | Suspicious Windows Successful Authentication |
| Detection ID | DET-003 |
| Severity | Medium |
| Status | Active |
| Author | Ajaydev S |

---

# Objective

Detect successful Windows network authentication events that may indicate unauthorized access using valid credentials.

The detection focuses on identifying successful Event ID 4624 logons that require analyst validation based on authentication context.

---

# MITRE ATT&CK Mapping

| Field | Value |
|--------|-------|
| Tactic | Initial Access |
| Technique | T1078 – Valid Accounts |

---

# Data Sources

Primary Telemetry

- Windows Security Log
- Splunk Enterprise

Supporting Telemetry

- Sysmon
- Windows Firewall
- Active Directory Security Logs (Domain Environments)

---

# Detection Logic

Generate an alert when:

- Windows Security Event ID **4624** is generated.
- Logon Type equals **3 (Network Logon)**.
- Authentication originates from an unexpected source IP or workstation.
- Authentication follows repeated failed logon attempts.
- Authentication occurs outside expected operational patterns.

---

# Splunk Searches

## Successful Network Logons

```spl
source="WinEventLog:Security"
EventCode=4624
Logon_Type=3
| table _time Account_Name Source_Network_Address Workstation_Name Authentication_Package_Name Logon_Process
```

---

## Successful Logons Following Failed Authentication

```spl
source="WinEventLog:Security"
(EventCode=4624 OR EventCode=4625)
Account_Name=*
| sort 0 _time
| table _time EventCode Account_Name Source_Network_Address Workstation_Name Logon_Type
```

---

## Successful Logons by Source IP

```spl
source="WinEventLog:Security"
EventCode=4624
| stats count by Source_Network_Address Account_Name Workstation_Name
```

---

# Expected Alert

Alert Title

```
Suspicious Successful Windows Network Authentication
```

---

Alert Description

```
Windows recorded a successful network authentication using valid credentials.

Analysts should verify whether the authentication originated from an expected source and determine whether additional post-authentication investigation is required.
```

---

# Alert Metadata

| Field | Value |
|--------|-------|
| Severity | Medium |
| Confidence | Medium |
| ATT&CK Technique | T1078 |
| Trigger | Successful Network Authentication |
| Response | Validate Authentication Context |

---

# Investigation Steps

1. Review Event ID 4624.
2. Verify the authenticated account.
3. Review Source IP Address.
4. Verify Workstation Name.
5. Confirm Logon Type.
6. Review Authentication Package.
7. Check for previous Event ID 4625 activity.
8. Determine whether authentication is expected.
9. Investigate post-authentication activity.

---

# False Positives

Legitimate successful authentication may occur due to:

- Normal user logons.
- Administrative remote access.
- Scheduled automation tasks.
- Backup software.
- Approved service accounts.

Successful authentication should always be evaluated within the surrounding authentication context.

---

# Tuning Recommendations

- Baseline normal authentication behavior.
- Exclude approved management hosts.
- Monitor privileged account logons separately.
- Correlate successful authentication with process creation events.
- Investigate successful logons from unfamiliar workstations.

---

# Validation

Lab Validation

- Hydra successfully authenticated using SMB2.
- Windows generated Event ID 4624.
- Splunk successfully indexed the authentication event.
- Authentication metadata matched the attack source.

---

# Related Documents

- `logs-analysis/successful-authentication-analysis.md`
- `mitre-mapping/successful-authentication-mitre.md`
- `incident-report/successful-authentication-incident-report.md`
- `playbooks/successful-authentication-playbook.md`

---

Template Version: 2.0

Last Updated: 2026-07-09

Author: Ajaydev S