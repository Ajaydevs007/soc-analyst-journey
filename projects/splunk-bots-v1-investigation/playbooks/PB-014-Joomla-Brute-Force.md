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
