# Detection: Threat Intelligence Enrichment

## Detection ID

DET-007

---

# Objective

Enrich malicious domains and IP addresses with external threat intelligence to improve investigations.

---

# Data Sources

- DNS Logs
- HTTP Logs
- Threat Intelligence Platforms
- WHOIS
- Historical Passive DNS

---

# Detection Logic

When a suspicious domain or IP is identified:

1. Collect indicators from internal logs.
2. Enrich indicators using trusted threat intelligence.
3. Document any historical attribution.
4. Validate before using attribution for incident response.

---

# Severity

Medium

---

# MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Gather Victim Network Information | T1590 |
| Acquire Infrastructure | T1583 |