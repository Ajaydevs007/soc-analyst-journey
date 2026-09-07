# Detection: Joomla Brute Force Authentication

## Detection ID

DET-014

---

# Objective

Detect repeated HTTP authentication attempts against the Joomla administrator interface.

---

# Data Source

- `stream:http`

---

# SPL Detection

```spl
index=botsv1
sourcetype="stream:http"
http_method=POST
uri="*/administrator/index.php"
form_data="*username*passwd*"
| rex field=form_data "username=(?<username>[^&]+)"
| rex field=form_data "passwd=(?<password>[^&]+)"
| stats count dc(password) as unique_passwords
    values(username) as usernames
    by src_ip dest_ip
| where count > 20
```

---

# Detection Logic

Detect repeated HTTP POST requests containing Joomla username and password parameters.

A high number of authentication attempts combined with many different passwords is indicative of brute-force activity.

---

# MITRE ATT&CK

| Technique | ID |
|---|---|
| Brute Force | T1110 |
| Password Guessing | T1110.001 |

---

# Severity

High

---

# Analyst Response

1. Identify source IP.
2. Identify targeted account.
3. Extract attempted passwords where appropriate for investigation.
4. Determine whether authentication eventually succeeded.
5. Check subsequent activity from the source IP.
6. Block or contain the attacker if appropriate.