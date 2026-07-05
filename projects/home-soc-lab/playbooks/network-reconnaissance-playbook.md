# Network Reconnaissance Investigation Playbook

---

# 1. Purpose

This playbook provides a standardized investigation procedure for alerts indicating potential network reconnaissance activity.

Its objective is to help SOC analysts validate, investigate, and respond to suspected network service discovery attempts before an attacker progresses to later stages of the attack lifecycle.

---

# 2. Alert Information

| Field | Value |
|--------|-------|
| Alert Name | Network Reconnaissance Detected |
| Severity | Low |
| MITRE ATT&CK | T1046 – Network Service Discovery |
| ATT&CK Tactic | Reconnaissance |

---

# 3. Investigation Objectives

- Confirm whether reconnaissance activity occurred.
- Identify the source and destination systems.
- Determine which services were targeted.
- Verify whether defensive controls blocked the activity.
- Assess whether additional malicious activity followed.

---

# 4. Required Telemetry

Primary Sources

- Windows Defender Firewall
- Splunk Enterprise

Supporting Sources

- Windows Security Log
- Sysmon Operational Log
- Nmap output (if available)

---

# 5. Investigation Workflow

## Step 1 — Validate the Alert

Verify:

- Source IP
- Destination IP
- Destination Port
- Timestamp
- Number of connection attempts

---

## Step 2 — Review Windows Firewall Logs

Check for:

- DROP TCP entries
- Repeated inbound connection attempts
- Multiple destination ports
- Repeated activity from the same source IP

Example:

```text
DROP TCP 10.10.10.10 10.10.10.20 40738 445 RECEIVE
```

---

## Step 3 — Review Windows Security Logs

Determine whether authentication activity occurred.

Relevant Event IDs:

- 4624
- 4625

If no authentication events exist, the activity may have remained in the reconnaissance phase.

---

## Step 4 — Review Sysmon

Verify whether Sysmon generated:

- Event ID 3 (Network Connection)

If Event ID 3 is absent:

- Confirm whether the connection was blocked before a TCP session was established.

---

## Step 5 — Review Splunk

Example searches:

```spl
sourcetype=windows:firewall
```

```spl
source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
```

Look for:

- Firewall DROP events
- Related Sysmon events
- Additional activity from the same source IP

---

## Step 6 — Correlate Events

Correlate:

- Source IP
- Destination IP
- Destination Port
- Time Window

Determine whether:

- Multiple ports were scanned.
- Multiple hosts were targeted.
- Later attack stages occurred.

---

# 6. Investigation Decision

## Benign

Examples:

- Authorized vulnerability scans
- Internal security testing
- Home SOC Lab exercises

---

## Suspicious

Examples:

- Unknown external IP
- Multiple targeted ports
- Repeated scans over time

---

## Malicious

Examples:

- Reconnaissance followed by:
  - Brute-force attacks
  - Successful authentication
  - Command execution
  - Persistence activity

---

# 7. Escalation Criteria

Escalate when:

- Multiple destination ports are scanned.
- Multiple systems are targeted.
- Authentication failures occur after reconnaissance.
- Successful authentication follows reconnaissance.
- Command execution is observed after initial scanning.

---

# 8. Containment Recommendations

Possible actions include:

- Block the source IP.
- Review firewall rules.
- Restrict exposed administrative services.
- Increase monitoring of the affected system.
- Continue monitoring for follow-on attack activity.

---

# 9. False Positives

Possible benign sources include:

- Vulnerability scanners
- Asset discovery tools
- Authorized penetration tests
- Internal network inventory tools

Always verify whether the source IP belongs to an authorized security tool.

---

# 10. Lessons Learned

- Reconnaissance is often the first observable stage of an attack.
- Windows Firewall provides valuable visibility into blocked inbound network activity.
- Sysmon Event ID 3 may not be generated for blocked inbound connection attempts.
- Correlating multiple telemetry sources provides greater investigative confidence.

---

# Related Documents

- Investigation: `logs-analysis/recon-analysis.md`
- MITRE Mapping: `mitre-mapping/recon-mitre.md`
- Detection Rule: `splunk-detections/recon-detection.md`
- Incident Report: `incident-report/recon-incident-report.md`

---

Playbook Version: 1.0

Last Updated: 2026-06-26

Author: Ajaydev S