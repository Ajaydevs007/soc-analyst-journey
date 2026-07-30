# Investigation: Po1s0n1vy Pre-Staged Infrastructure

## Investigation ID

INV-006

---

# Objective

Identify the IP address associated with the malicious domains pre-staged by the attacker (Po1s0n1vy) to target Wayne Enterprises.

---

# Question

> What IP address has Po1s0n1vy tied to domains that are pre-staged to attack Wayne Enterprises?

---

# Initial Information

The previous investigation (INV-005) identified the malicious FQDN:

```
prankglassinebracket.jumpingcrab.com
```

The next objective was to determine the IP address associated with this attacker-controlled infrastructure.

---

# Data Source

- Index: `botsv1`
- Sourcetype: `stream:dns`

---

# Investigation Steps

## Step 1 – Search for the Malicious Domain

Query the DNS logs for the identified malicious domain.

```spl
index=botsv1 sourcetype=stream:dns
name="prankglassinebracket.jumpingcrab.com"
| table _time name host_addr reply_code
```

### Observation

The DNS event showed that the domain successfully resolved with a `NoError` response.

---

## Step 2 – Review the Resolved Address

The `host_addr` field contained the resolved IP address for the domain.

```
host_addr

23.22.63.114
```

---

# Findings

Analysis of the DNS logs confirmed that the attacker-controlled domain:

```
prankglassinebracket.jumpingcrab.com
```

resolved to:

```
23.22.63.114
```

This IP address represents the attacker infrastructure that was pre-staged for operations against Wayne Enterprises.

---

# Investigation Flow

```
Known Malicious Domain
        │
        ▼
Search DNS Logs
        │
        ▼
Review host_addr
        │
        ▼
Identify Associated IP
```

---

# Evidence Collected

| Artifact | Value |
|----------|-------|
| Domain | prankglassinebracket.jumpingcrab.com |
| Resolved IP | 23.22.63.114 |
| DNS Reply | NoError |

---

# Conclusion

DNS analysis confirmed that the malicious infrastructure associated with Po1s0n1vy resolved to the IP address:

```
23.22.63.114
```

This IP was used as part of the attacker's infrastructure targeting Wayne Enterprises.