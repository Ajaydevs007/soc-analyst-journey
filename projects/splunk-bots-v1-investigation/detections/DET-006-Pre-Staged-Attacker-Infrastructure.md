# Detection: Pre-Staged Attacker Infrastructure

## Detection ID

DET-006

---

# Objective

Detect DNS resolutions to attacker-controlled infrastructure that may be used during future attacks.

---

# Data Source

- Splunk Enterprise
- Sourcetype: `stream:dns`

---

# SPL Detection Rule

## Detect DNS Queries to Known Malicious Domain

```spl
index=botsv1 sourcetype=stream:dns
name="prankglassinebracket.jumpingcrab.com"
| table _time src_ip dest_ip name host_addr reply_code
```

## Detect DNS Queries Resolving to Known Malicious IP

```spl
index=botsv1 sourcetype=stream:dns
host_addr="23.22.63.114"
| table _time src_ip name host_addr
```

---

# Detection Logic

Identify DNS queries that resolve to known malicious infrastructure previously associated with attacker Po1s0n1vy.

---

# MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Dynamic Resolution | T1568.003 |
| Application Layer Protocol: DNS | T1071.004 |

---

# Severity

High

---

# Analyst Response

1. Verify the queried domain.
2. Confirm the resolved IP.
3. Correlate with HTTP, firewall, and endpoint logs.
4. Block malicious infrastructure if confirmed.