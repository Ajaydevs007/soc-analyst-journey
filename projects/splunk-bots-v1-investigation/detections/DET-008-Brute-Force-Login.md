# Detection: Joomla Administrator Brute Force

## Detection ID

DET-008

---

# Objective

Detect repeated login attempts against the Joomla administrator interface.

---

# Data Source

- stream:http

---

# SPL Detection Rule

```spl
index=botsv1 sourcetype="stream:http"
http_method=POST
uri_path="/joomla/administrator/index.php"
form_data="*username*passwd*"
| bucket _time span=1m
| stats count by _time src_ip
| where count > 20
```

---

# Detection Logic

Alert when a single source IP submits more than 20 login attempts within one minute against the Joomla administrator login page.

---

# MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Brute Force | T1110 |

---

# Severity

High

---

# Analyst Response

- Verify repeated authentication attempts.
- Check whether authentication succeeded.
- Block offending IP if malicious.
- Investigate follow-on activity.