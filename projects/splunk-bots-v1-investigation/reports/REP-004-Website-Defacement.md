# Incident Report

## Report ID

REP-004

---

# Executive Summary

The investigation identified that the compromised web server downloaded a file named:

```
poisonivy-is-coming-for-you-batman.jpeg
```

The file was retrieved from a domain categorized as a malicious website.

---

# Investigation Summary

## Source Host

192.168.250.70

---

## Destination

23.22.63.114

Hostname:

```
prankglassinebracket.jumpingcrab.com
```

---

## Downloaded Resource

```
/poisonivy-is-coming-for-you-batman.jpeg
```

---

# Findings

The firewall logs recorded multiple outbound requests to the malicious domain.

The downloaded JPEG is associated with the website defacement activity.

---

# Risk Assessment

Risk Level:

High

---

# Recommendations

- Remove malicious files
- Restore website
- Investigate persistence
- Patch vulnerable applications
- Block malicious infrastructure

---

# Conclusion

Evidence indicates the compromised web server retrieved a malicious JPEG used during the website defacement.