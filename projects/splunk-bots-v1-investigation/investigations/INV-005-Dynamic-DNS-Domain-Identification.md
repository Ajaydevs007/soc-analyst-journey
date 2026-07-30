# Investigation: Dynamic DNS Domain Identification

## Investigation ID

INV-005

---

# Objective

Identify the Fully Qualified Domain Name (FQDN) associated with the malicious IP address used during the attack.

---

# Question

> This attack used dynamic DNS to resolve to the malicious IP. What fully qualified domain name (FQDN) is associated with this attack?

---

# Initial Information

During the previous investigation, the malicious IP address **23.22.63.114** was identified through firewall logs. The next objective was to determine which domain name resolved to this IP address.

Since DNS logs record domain-to-IP resolutions, they were selected as the primary data source for this investigation.

---

# Data Source

- Index: `botsv1`
- Sourcetype: `stream:dns`

---

# Investigation Steps

## Step 1 – Search for the Malicious IP Address

Search the DNS logs for the known malicious IP address.

```spl
index=botsv1 sourcetype=stream:dns
23.22.63.114
```

### Observation

One DNS event was returned containing the malicious IP address.

---

## Step 2 – Review the DNS Event

Display the important fields.

```spl
index=botsv1 sourcetype=stream:dns
23.22.63.114
| table _time src_ip dest_ip name host_addr reply_code
```

### Observation

| Field | Value |
|-------|------|
| Source Host | 192.168.250.20 |
| DNS Server | 8.8.8.8 |
| Reply Code | NoError |

The DNS response completed successfully, indicating the queried domain was successfully resolved.

---

## Step 3 – Expand Multi-value Fields

The `name` and `host_addr` fields contained multiple values.

After expanding them, the DNS query revealed:

```
name:
prankglassinebracket.jumpingcrab.com
```

The DNS response mapped this domain to:

```
23.22.63.114
```

---

# Findings

The DNS logs confirmed that the host **192.168.250.20** queried the domain:

```
prankglassinebracket.jumpingcrab.com
```

Google Public DNS (**8.8.8.8**) successfully resolved the domain to:

```
23.22.63.114
```

This directly associates the FQDN with the malicious IP address used during the attack.

---

# Investigation Flow

```
Known Malicious IP
        │
        ▼
Search DNS Logs
        │
        ▼
Review DNS Event
        │
        ▼
Expand Multi-value Fields
        │
        ▼
Identify Queried Domain
        │
        ▼
Confirm FQDN
```

---

# Evidence Collected

| Artifact | Value |
|----------|-------|
| Source Host | 192.168.250.20 |
| DNS Server | 8.8.8.8 |
| Reply Code | NoError |
| Malicious IP | 23.22.63.114 |
| FQDN | prankglassinebracket.jumpingcrab.com |

---

# Conclusion

Analysis of the DNS logs confirmed that the malicious IP address **23.22.63.114** was associated with the Fully Qualified Domain Name (FQDN):

```
prankglassinebracket.jumpingcrab.com
```

The investigation successfully answered the challenge by correlating the DNS query with its resolved IP address.