# Detection: Suspicious Staged Domain WHOIS

## Detection ID

DET-013

---

# Objective

Identify suspicious domains whose historical registration information contains indicators associated with threat-actor infrastructure.

---

# Detection Type

Threat Intelligence / OSINT

---

# Relevant Indicators

Domain:

```text
waynecorinc.com
```

IP:

```text
23.22.63.114
```

Registrant:

```text
LILLIAN ROSE
```

Email:

```text
lillian.rose@po1s0n1vy.com
```

---

# Analyst Action

When investigating suspicious domains:

1. Perform current WHOIS/RDAP lookup.
2. Check historical WHOIS records.
3. Compare registrant information against known attacker infrastructure.
4. Look for reused names, email addresses, organizations, addresses and unusual values.
5. Preserve historical WHOIS evidence because current records may differ.

---

# Important Note

The hexadecimal WHOIS artifacts in this investigation are historical BOTS v1 evidence and should not be treated as a standalone real-time detection rule.