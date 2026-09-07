# Playbook: Joomla Brute Force Investigation

## Playbook ID

PB-014

---

# Objective

Investigate repeated authentication attempts against a Joomla administrator interface.

---

# Procedure

1. Identify the targeted Joomla endpoint.
2. Search HTTP POST authentication requests.
3. Extract username and password parameters.
4. Sort authentication attempts chronologically.
5. Identify the source IP.
6. Count unique passwords attempted.
7. Determine whether a valid password was eventually discovered.
8. Correlate successful authentication with subsequent activity.

---

# Containment

- Block malicious source IP.
- Disable compromised account if necessary.
- Reset affected credentials.
- Review web server logs.
- Investigate post-authentication activity.

---

# Recovery

- Remove unauthorized access.
- Review Joomla administrator accounts.
- Patch vulnerable components.
- Enable stronger authentication controls.
- Implement rate limiting.
```

# `reports/REP-014-Brute-Force.md`

````markdown
# Incident Report

## Report ID

REP-014

---

# Executive Summary

Analysis of HTTP authentication traffic identified a brute-force attack against the Joomla administrator interface.

The first password attempted was:

```text
12345678
```

---

# Findings

| Item | Value |
|---|---|
| Source IP | `23.22.63.114` |
| Destination IP | `192.168.250.70` |
| Target | Joomla Administrator |
| Endpoint | `/joomla/administrator/index.php` |
| First Password | `12345678` |

---

# Assessment

The repeated HTTP POST authentication attempts are consistent with automated password brute-forcing.

---

# Recommendations

- Block the attacking source IP.
- Reset affected credentials.
- Review successful authentication events.
- Investigate post-compromise activity.
- Implement authentication rate limiting.