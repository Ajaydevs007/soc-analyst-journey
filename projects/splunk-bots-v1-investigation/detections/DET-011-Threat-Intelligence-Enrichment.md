# Detection: Malware Hash Threat Intelligence

## Detection ID

DET-011

---

# Objective

Detect known malicious files using SHA256 threat intelligence.

---

# Data Sources

- EDR
- Sysmon
- Antivirus
- Threat Intelligence Platform

---

# Detection Logic

Alert when a file hash matches a known malicious SHA256 associated with Po1s0n1vy infrastructure.

---

# IOC

**Filename**

```
MirandaTateScreensaver.scr.exe
```

**SHA256**

```
9709473ab351387aab9e816eff3910b9f28a7a70202e250ed46dba8f820f34a8
```

---

# MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Spearphishing Attachment | T1566.001 |
| Ingress Tool Transfer | T1105 |

---

# Severity

Critical