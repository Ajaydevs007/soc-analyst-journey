# Timeline: Coldplay Password Investigation

## Timeline ID

TL-015

---

| Sequence | Event |
|---|---|
| 1 | Automated brute-force attack targeted Joomla |
| 2 | HTTP POST authentication attempts were identified |
| 3 | Password values were extracted from `form_data` |
| 4 | Six-character passwords were isolated |
| 5 | Password candidates were compared against Coldplay songs |
| 6 | `yellow` was identified as the matching password |

---

# Key Artifact

```text
yellow
```

---

# Summary

The investigation narrowed the brute-force password list to six-character candidates and correlated those candidates with Coldplay song titles. The intended matching password was `yellow`.
```

# `artifacts/ART-015.md`

````markdown
# Investigation Artifacts

## Artifact ID

ART-015

---

## Attacker IP

```text
23.22.63.114
```

---

## Target IP

```text
192.168.250.70
```

---

## Target Endpoint

```text
/joomla/administrator/index.php
```

---

## Password

```text
yellow
```

---

## Associated Song

```text
Yellow
```

---

## Password Length

```text
6
```

---

## Investigation Type

```text
Brute Force + External Enrichment
```