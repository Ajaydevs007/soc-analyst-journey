# Successful Authentication Investigation Playbook

---

# Playbook Information

| Field | Value |
|--------|-------|
| Playbook ID | PB-003 |
| Playbook Name | Successful Windows Authentication Investigation |
| Severity | Medium |
| ATT&CK Technique | T1078 – Valid Accounts |
| Author | Ajaydev S |

---

# Objective

Provide a structured investigation process for successful Windows network authentication events to determine whether the activity represents legitimate user behavior or unauthorized access using valid credentials.

---

# Trigger

Initiate this playbook when:

- Windows Security Event ID **4624** is detected.
- The authentication originates from an unusual source.
- The authentication follows repeated failed logon attempts.
- A privileged account successfully authenticates.
- Authentication behavior deviates from the user's normal activity.

---

# Initial Triage

Collect the following information:

- Username
- Source IP Address
- Workstation Name
- Target Host
- Logon Type
- Authentication Package
- Logon Process
- Authentication Time
- Previous authentication history

---

# Investigation Steps

## Step 1 — Review Event ID 4624

Verify:

- Username
- Logon Type
- Authentication Package
- Source IP
- Workstation
- Target Host

---

## Step 2 — Validate Authentication Context

Determine:

- Is the account expected to access this system?
- Is the source IP recognized?
- Is the workstation trusted?
- Did the authentication occur during normal business hours?
- Is the authentication method expected?

---

## Step 3 — Review Previous Authentication Activity

Search for:

- Previous Event ID 4625 events.
- Account lockout events.
- Authentication attempts from different IP addresses.
- Authentication attempts against other accounts.

---

## Step 4 — Investigate Post-Authentication Activity

Review additional telemetry for:

- Process creation (Sysmon Event ID 1).
- Network connections.
- File access.
- PowerShell activity.
- Scheduled tasks.
- Persistence mechanisms.

---

## Step 5 — Determine Legitimacy

Classify the authentication as one of the following:

- Expected user activity.
- Administrative activity.
- Authorized security testing.
- Suspicious authentication.
- Confirmed unauthorized access.

---

# Response Actions

If malicious activity is confirmed:

- Notify the SOC Lead.
- Disable or isolate the affected account if required.
- Reset compromised credentials.
- Investigate endpoint activity following authentication.
- Review additional authentication events across the environment.
- Escalate for incident response if compromise is confirmed.

If the activity is authorized:

- Document the authentication.
- Record supporting evidence.
- Close the investigation.
- Update investigation notes if necessary.

---

# Evidence to Collect

- Windows Security Event ID 4624
- Previous Event ID 4625 events
- Splunk search results
- Source IP Address
- Workstation Name
- Authentication Package
- Logon Type
- Hydra terminal output (Lab)
- Timeline reconstruction

---

# Escalation Criteria

Escalate when:

- Authentication follows repeated failed logons.
- Administrative accounts are involved.
- Authentication originates from unknown systems.
- Multiple successful logons occur across different endpoints.
- Suspicious activity follows successful authentication.

---

# Detection References

Primary Detection

- Windows Security Event ID 4624

Supporting Information

- Logon Type
- Authentication Package
- Logon Process
- Source Network Address
- Workstation Name
- Previous Event ID 4625 activity

---

# Related Documents

- `logs-analysis/successful-authentication-analysis.md`
- `mitre-mapping/successful-authentication-mitre.md`
- `splunk-detections/successful-authentication-detection.md`
- `incident-report/successful-authentication-incident-report.md`

---

# Lessons for Analysts

- Successful authentication should never be evaluated in isolation.
- Authentication context is essential for determining legitimacy.
- Correlating Event ID 4624 with previous authentication activity improves investigation accuracy.
- Post-authentication activity often provides stronger indicators of compromise than the authentication event itself.
- A successful logon may represent the beginning of an attack rather than its conclusion.

---

Playbook Version: 2.0

Last Updated: 2026-07-09

Author: Ajaydev S