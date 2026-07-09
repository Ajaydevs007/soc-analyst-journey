# Windows Brute Force Authentication Playbook

---

# Playbook Information

| Field | Value |
|--------|-------|
| Playbook ID | PB-002 |
| Playbook Name | Windows Brute Force Authentication Response |
| Severity | Medium |
| ATT&CK Technique | T1110.001 – Password Guessing |
| Author | Ajaydev S |

---

# Objective

Provide a structured investigation and response process for Windows brute-force authentication alerts generated from repeated failed logon attempts followed by a successful authentication.

---

# Trigger

This playbook should be initiated when:

- Multiple Windows Security Event ID **4625** events are detected.
- The same user account is repeatedly targeted.
- The attempts originate from the same source IP address.
- A successful Event ID **4624** follows the failed attempts.

---

# Initial Triage

Collect the following information:

- Username
- Source IP address
- Workstation name
- Target host
- Authentication package
- Logon type
- Number of failed attempts
- Time window
- Successful authentication (Yes/No)

---

# Investigation Steps

## Step 1 — Review Failed Authentication Events

Identify:

- Target account
- Source IP
- Workstation
- Logon Type
- Authentication Package
- Failure Status
- SubStatus

Relevant Event ID:

- 4625

---

## Step 2 — Check for Successful Authentication

Determine whether Event ID **4624** occurred after the failed attempts.

If successful:

- Confirm username
- Confirm source IP
- Confirm workstation
- Confirm logon type

---

## Step 3 — Correlate Authentication Events

Determine:

- How many failed attempts occurred?
- Were all attempts against the same account?
- Did the source IP remain the same?
- How much time elapsed between attempts?
- Was the attack successful?

---

## Step 4 — Determine Scope

Identify whether:

- One account was targeted.
- Multiple accounts were targeted.
- One endpoint was targeted.
- Multiple endpoints were targeted.

---

## Step 5 — Validate Activity

Determine whether the authentication activity is:

- Authorized security testing.
- User error.
- Administrative activity.
- Malicious password guessing.

---

# Response Actions

If malicious activity is confirmed:

- Notify the SOC Lead.
- Disable or lock the affected account if required.
- Reset compromised credentials.
- Block the attacking IP address where appropriate.
- Review additional authentication activity.
- Investigate post-authentication behavior.

If authorized:

- Document the activity.
- Close the investigation.
- Record lessons learned.

---

# Evidence to Collect

- Windows Security Events (4625, 4624)
- Splunk search results
- Source IP address
- Username
- Workstation name
- Hydra terminal output (lab)
- Timeline reconstruction

---

# Escalation Criteria

Escalate when:

- Multiple user accounts are targeted.
- Administrative accounts are targeted.
- Authentication succeeds unexpectedly.
- Suspicious post-authentication activity is observed.
- Similar attacks are detected across multiple systems.

---

# Detection References

Primary Detection

- Windows Security Event ID 4625
- Windows Security Event ID 4624

Supporting Information

- Logon Type
- Authentication Package
- Source Network Address
- Workstation Name
- Status
- SubStatus

---

# Related Documents

- `logs-analysis/brute-force-analysis.md`
- `mitre-mapping/brute-force-mitre.md`
- `splunk-detections/brute-force-detection.md`
- `incident-report/brute-force-incident-report.md`

---

# Lessons for Analysts

- Always correlate failed and successful authentication events.
- Review Status and SubStatus values to understand why authentication failed.
- Validate authentication metadata before escalating.
- Investigate successful authentication immediately following repeated failures.
- Correlate authentication events with later endpoint activity.

---

Playbook Version: 2.0

Last Updated: 2026-07-09

Author: Ajaydev S