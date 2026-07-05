# Investigation Title

> Replace with the investigation name (e.g., Reconnaissance Detection & Telemetry Analysis)

---

# 1. Investigation Overview

## Objective

Describe why this investigation was performed.

Questions to answer:

- What activity is being investigated?
- Why is it important?
- What is the expected outcome?

---

## Scope

### Investigation Status

| Field | Value |
|--------|-------|
| Investigation ID | |
| Status | |
| Severity | |
| Disposition | |
| Analyst | |

---

### Investigation Trigger

Describe what initiated the investigation.

---

### Investigation Goals

List the objectives of the investigation.

---

### Time Window

Specify the investigation period.

---

### Systems Involved

List all systems involved in the investigation.

---

# 2. Lab Environment

| Component | Details |
|-----------|---------|
| Hypervisor | |
| Attacker | |
| Victim | |
| SIEM | |
| Log Forwarder | |
| Endpoint Telemetry | |
| Network | |

## Telemetry Sources

- Windows Security Log
- Windows System Log
- Windows Application Log
- Sysmon Operational Log
- Windows Firewall Log
- Splunk Enterprise

---

# 3. Attack Overview

## Attack Scenario

Describe the simulated attack scenario.

---

## Attack Details

| Field | Value |
|--------|-------|
| Attack Phase | |
| ATT&CK Tactic | |
| ATT&CK Technique | |
| Attacker | |
| Target | |
| Source IP | |
| Destination IP | |
| Tool | |
| Attack Type | |

---

## Attack Execution

### Tool

Describe the tool used.

---

### Command

```bash
# Command executed
```

---

### Command Output

```text
# Paste terminal output
```

---

### Initial Observation

Describe only the immediate observations after the attack.

Do **NOT** explain the root cause yet.

---

# 4. Evidence Collection

Document all evidence collected during the investigation.

---

## 4.1 Attacker Evidence (Kali Linux)

### Commands Executed

### Terminal Output

### Screenshots

### Initial Observations

---

## 4.2 Windows Security Evidence

### Relevant Event IDs

### Event Details

### Observations

---

## 4.3 Windows Firewall Evidence

### Log Entries

### Screenshots

### Observations

---

## 4.4 Sysmon Evidence

### Relevant Event IDs

### Event Details

### Screenshots

### Observations

---

## 4.5 Splunk Evidence

### Search Queries

```spl
# SPL Queries
```

### Search Results

### Screenshots

### Observations

---

# 5. Evidence Summary

| Evidence Source | Summary |
|-----------------|---------|
| Kali Linux | |
| Windows Security Log | |
| Windows Firewall | |
| Sysmon | |
| Splunk | |

---

# 6. Investigation

---

## 6.1 Telemetry Analysis

Analyze each telemetry source individually.

Questions to answer:

- What was observed?
- Which events were generated?
- Which events were expected but absent?
- Why?

---

## 6.2 Event Correlation

Correlate evidence from all telemetry sources.

Describe how the attack progressed.

---

## 6.3 Root Cause Analysis

Determine:

- Why the activity occurred.
- Why certain events appeared.
- Why certain expected events did not appear.
- Whether defensive controls functioned correctly.

---

# 7. Timeline Reconstruction

| Time | Activity | Telemetry Source | Evidence |
|------|----------|------------------|----------|
| | | | |

---

# 8. Findings

---

## Confirmed Findings

List confirmed conclusions supported by evidence.

---

## Supporting Evidence

Reference the evidence supporting each finding.

Example:

- Kali terminal output
- Windows Firewall log
- Sysmon Event Viewer
- Splunk search
- Screenshots

---

# 9. Detection & Telemetry Coverage

| Telemetry Source | Coverage | Notes |
|------------------|----------|------|
| Windows Security Log | | |
| Windows Firewall | | |
| Sysmon | | |
| Splunk | | |

---

# 10. Detection Opportunities

Describe:

- Detection logic
- Correlation opportunities
- Alerting strategy
- Detection improvements

---

# 11. Analyst Reflection

Reflect on the investigation.

Suggested topics:

- Initial assumptions
- Investigation methodology
- Unexpected findings
- Lessons for future investigations
- Improvements to the lab

---

# 12. Lessons Learned

Summarize the most important technical and investigative lessons learned.

Examples:

- Telemetry limitations
- Detection improvements
- Investigation methodology
- Defensive insights

---

# 13. References

## Documentation References

- Microsoft Documentation
- Splunk Documentation
- MITRE ATT&CK
- Sysinternals Sysmon Documentation
- Other technical references used

---

## Investigation Evidence

- Kali terminal output
- Windows Event Viewer
- Windows Firewall log
- Splunk search results
- Project screenshots

---

## Related Project Documents

- MITRE Mapping
- Detection Rule
- Incident Report
- Investigation Playbook

---

Template Version: 2.0

Last Updated: 2026-06-26

Author: Ajaydev S