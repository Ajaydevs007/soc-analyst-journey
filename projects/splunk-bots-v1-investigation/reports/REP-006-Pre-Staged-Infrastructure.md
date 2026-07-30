# Incident Report

## Report ID

REP-006

---

# Executive Summary

The investigation confirmed that attacker-controlled infrastructure associated with Po1s0n1vy resolved to the IP address **23.22.63.114**.

---

# Findings

| Item | Value |
|------|-------|
| Threat Actor | Po1s0n1vy |
| Domain | prankglassinebracket.jumpingcrab.com |
| IP Address | 23.22.63.114 |

DNS analysis confirmed that the malicious domain resolved to the identified IP address, linking the attacker infrastructure used during the campaign.

---

# Risk

High

---

# Recommendations

- Block the domain.
- Block the IP address.
- Investigate hosts that queried the domain.
- Continue monitoring DNS activity.