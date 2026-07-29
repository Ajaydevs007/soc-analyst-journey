# Detection: Dynamic DNS Domain Resolution

## Detection ID

DET-Q05

---

# Objective

Detect DNS queries resolving dynamic DNS (DDNS) domains that may be used for malware delivery, command-and-control (C2), or malicious infrastructure.

---

# Data Source

- Splunk Enterprise
- Sourcetype: `stream:dns`

---

# SPL Detection Rule

## Identify DNS Queries to Dynamic DNS Domains

```spl
index=botsv1 sourcetype=stream:dns
| search name="*.jumpingcrab.com"
| table _time src_ip dest_ip name host_addr reply_code
```

---

## Identify Domains Resolving to a Known Malicious IP

```spl
index=botsv1 sourcetype=stream:dns
23.22.63.114
| table _time src_ip dest_ip name host_addr
```

---

## Alternative Detection

```spl
index=botsv1 sourcetype=stream:dns
| mvexpand name
| stats count by name host_addr
```

---

# Detection Logic

The detection identifies DNS queries that resolve dynamic DNS domains.

Dynamic DNS providers are frequently abused by attackers because the underlying IP address can change while the domain name remains constant.

---

# MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Dynamic Resolution | T1568.003 |
| Application Layer Protocol: DNS | T1071.004 |

---

# Severity

Medium

---

# Possible False Positives

- Legitimate Dynamic DNS services
- Home lab environments
- Authorized remote access solutions

---

# Analyst Response

1. Review the queried FQDN.
2. Identify the resolved IP address.
3. Determine whether the IP is malicious.
4. Correlate with HTTP, firewall, and endpoint logs.
5. Investigate follow-on network activity.

---

# Evidence

FQDN

```
prankglassinebracket.jumpingcrab.com
```

Resolved IP

```
23.22.63.114
```