# Incident Report

## Report ID

REP-005

---

# Executive Summary

The investigation identified that the attack used a Dynamic DNS domain to resolve the malicious IP address used during the compromise.

---

# Investigation Summary

## Source Host

192.168.250.20

---

## DNS Server

8.8.8.8

---

## Malicious Domain

```
prankglassinebracket.jumpingcrab.com
```

---

## Resolved IP

```
23.22.63.114
```

---

# Findings

DNS logs confirmed that the internal host queried the domain `prankglassinebracket.jumpingcrab.com`, which resolved to the malicious IP address `23.22.63.114`.

This domain served as the Dynamic DNS infrastructure associated with the attack.

---

# Risk Assessment

Risk Level

High

---

# Recommendations

- Block the malicious domain.
- Block the resolved IP address.
- Review all systems communicating with the domain.
- Investigate additional DNS activity for similar domains.

---

# Conclusion

The Dynamic DNS domain associated with the attack was successfully identified through DNS log analysis.