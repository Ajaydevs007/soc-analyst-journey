# Playbook: Investigating Pre-Staged Infrastructure

## Playbook ID

PB-006

---

# Objective

Investigate systems resolving attacker-controlled domains before or during an attack.

---

# Investigation Steps

1. Review DNS logs.
2. Identify the queried domain.
3. Identify the resolved IP address.
4. Check HTTP and firewall logs for communication.
5. Review endpoint activity.
6. Determine affected hosts.
7. Contain and block malicious infrastructure.

---

# Containment

- Block malicious domain.
- Block malicious IP.
- Isolate affected hosts if necessary.
- Continue monitoring DNS activity.

---

# Recovery

- Remove malicious artifacts.
- Verify no additional communication exists.
- Update detection rules.