# Investigation: OSINT Attribution of Po1s0n1vy Infrastructure

## Investigation ID

INV-007

---

# Objective

Identify the email address most likely associated with the Po1s0n1vy APT group using historical Open Source Intelligence (OSINT).

---

# Question

> Based on the data gathered from this attack and common open source intelligence sources for domain names, what is the email address that is most likely associated with Po1s0n1vy APT group?

---

# Initial Information

Previous investigations identified the attacker infrastructure:

Domain

```
prankglassinebracket.jumpingcrab.com
```

Resolved IP

```
23.22.63.114
```

The challenge now requires using OSINT sources rather than Splunk logs.

---

# Data Sources

- Splunk Enterprise (previous investigations)
- Historical WHOIS
- Historical Passive DNS
- Domain Reputation Services
- Public Threat Intelligence Sources

---

# Investigation Steps

## Step 1 – Identify the Malicious Domain

From previous investigations:

```
prankglassinebracket.jumpingcrab.com
```

---

## Step 2 – Perform OSINT

The domain was searched using multiple OSINT platforms, including:

- WHOIS
- SecurityTrails
- VirusTotal
- Threat intelligence platforms

### Observation

Current OSINT sources no longer contain the historical registration details associated with the domain.

---

## Step 3 – Historical Attribution

According to historical OSINT data used when the BOTS v1 dataset was created, the attacker infrastructure was associated with the email address:

```
lillian.rose@po1s0n1vy.com
```

---

# Findings

Historical OSINT attributed the attacker-controlled infrastructure to the email address above.

Current public OSINT services no longer expose this information due to record expiration, privacy regulations (such as GDPR), and changes in data retention.

---

# Evidence Collected

| Artifact | Value |
|----------|-------|
| Domain | prankglassinebracket.jumpingcrab.com |
| IP Address | 23.22.63.114 |
| Associated Email | lillian.rose@po1s0n1vy.com |

---

# Investigation Limitations

This attribution could not be independently verified using current public OSINT services because the historical records are no longer publicly available.

The documented email address is the accepted historical attribution for the Splunk BOTS v1 challenge.

---

# Conclusion

Historical OSINT associated the attacker infrastructure with:

```
lillian.rose@po1s0n1vy.com
```

This satisfies the attribution requirement for the challenge while acknowledging the limitations of reproducing historical OSINT evidence today.