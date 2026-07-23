# Investigation Methodology

## Purpose

This repository documents a complete end-to-end investigation of the Splunk Boss of the SOC (BOTS) v1 dataset using a structured SOC analyst investigation methodology.

Rather than simply answering challenge questions, every phase of the attack is investigated as a real-world security incident. The objective is to understand attacker behavior, correlate evidence across multiple log sources, develop detection opportunities, map activity to the MITRE ATT&CK framework, and produce professional incident response documentation.

---

# Investigation Workflow

Every investigation follows the same structured workflow.

```
Question / Alert
        │
        ▼
Understand the Investigation Objective
        │
        ▼
Identify Relevant Log Sources
        │
        ▼
Develop Investigation Hypothesis
        │
        ▼
Write SPL Queries
        │
        ▼
Collect Evidence
        │
        ▼
Interpret Results
        │
        ▼
Correlate Events
        │
        ▼
Build Attack Timeline
        │
        ▼
MITRE ATT&CK Reasoning
        │
        ▼
Identify Detection Opportunities
        │
        ▼
Document Findings
```

---

# Investigation Process

## 1. Understand the Investigation Objective

Clearly define what needs to be investigated before searching logs.

Questions considered include:

- What is being investigated?
- Why is this activity suspicious?
- What information is required?
- What evidence is expected?

---

## 2. Identify Relevant Log Sources

Determine which log sources are likely to contain evidence.

Examples include:

- Web Server Logs
- Windows Security Logs
- Sysmon
- DNS Logs
- Firewall Logs
- Proxy Logs
- Authentication Logs
- Endpoint Logs

---

## 3. Develop an Investigation Hypothesis

Before writing queries, develop an initial hypothesis.

Examples:

- Was a web application exploited?
- Did an attacker upload a webshell?
- Was credential theft performed?
- Did lateral movement occur?

The hypothesis is continuously refined as new evidence is discovered.

---

## 4. Write SPL Queries

Develop Splunk Search Processing Language (SPL) queries to locate relevant events.

The investigation starts with broad searches before narrowing to specific indicators.

---

## 5. Collect Evidence

Gather supporting evidence including:

- Source IP addresses
- Destination IP addresses
- Usernames
- Hostnames
- URLs
- File names
- Commands executed
- Process execution
- HTTP requests
- Authentication events

Relevant screenshots are captured throughout the investigation.

---

## 6. Interpret Results

Analyze collected evidence to determine:

- What occurred?
- When did it occur?
- Who performed the activity?
- Which systems were affected?
- Was the activity malicious or legitimate?

---

## 7. Correlate Events

Correlate evidence across multiple data sources to reconstruct attacker activity.

Questions include:

- What happened immediately before this event?
- What happened immediately after?
- How does this activity relate to previous findings?

---

## 8. Build the Attack Timeline

Each confirmed event is added to the overall attack timeline.

The timeline reconstructs the complete attack chain from initial access through final objectives.

---

## 9. MITRE ATT&CK Reasoning

Rather than memorizing ATT&CK techniques, attacker behavior is analyzed by asking:

- What was the attacker trying to accomplish?
- Which ATT&CK tactic fits the objective?
- Which technique best describes the observed behavior?

---

## 10. Identify Detection Opportunities

For every attack phase, identify opportunities to improve detection.

This includes:

- SPL detection searches
- Correlation rules
- Alert logic
- Threat hunting queries
- SOC recommendations

---

## 11. Document Findings

Each completed investigation produces:

- Investigation Report
- Timeline Update
- MITRE ATT&CK Mapping
- Detection Rule
- IOC Documentation
- Incident Report Update
- SOC Investigation Playbook

---

# Investigation Principles

This project follows several core principles:

- Evidence-driven investigations
- Correlation before conclusions
- Validate assumptions using logs
- Document every significant finding
- Focus on attacker behavior rather than challenge answers
- Produce reusable detection content
- Follow professional SOC investigation practices

---

# Expected Deliverables

By the completion of this project, the repository will contain:

- Complete attack timeline
- Phase-by-phase investigations
- Splunk SPL queries
- Detection engineering content
- MITRE ATT&CK mappings
- IOC documentation
- SOC playbooks
- Executive incident report
- Professional GitHub documentation