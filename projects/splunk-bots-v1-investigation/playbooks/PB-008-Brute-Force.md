# Playbook: Web Application Brute Force Investigation

## Playbook ID

PB-008

---

# Objective

Investigate suspected brute force attacks against web application login portals.

---

# Procedure

1. Identify repeated POST requests.
2. Locate authentication endpoints.
3. Count login attempts per source IP.
4. Review HTTP status codes.
5. Check for successful authentication.
6. Correlate with firewall and endpoint logs.
7. Block malicious IP if confirmed.

---

# Containment

- Block attacker IP.
- Enable rate limiting.
- Enforce MFA.
- Review compromised accounts.