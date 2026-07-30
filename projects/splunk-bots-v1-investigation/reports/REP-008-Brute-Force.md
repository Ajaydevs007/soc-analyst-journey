# Incident Report

## Report ID

REP-008

---

# Executive Summary

HTTP analysis identified a brute force password attack targeting the Joomla administrator portal.

---

# Findings

| Item | Value |
|------|-------|
|Attacker IP|23.22.63.114|
|Target|imreallynotbatman.com|
|Login Endpoint|/joomla/administrator/index.php|
|Attempts|412|

The attacker repeatedly submitted credentials within approximately 90 seconds, indicating automated password guessing.

---

# Risk

High

---

# Recommendations

- Block attacker IP.
- Enable account lockout.
- Enable MFA.
- Monitor administrator accounts.