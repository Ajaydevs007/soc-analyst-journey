# Playbook: Brute Force Password Analysis

## Playbook ID

PB-015

---

# Objective

Identify interesting passwords within a brute-force attack and correlate them with external information.

---

# Procedure

1. Identify brute-force HTTP requests.
2. Extract the `passwd` value.
3. Calculate password length.
4. Filter according to the investigation requirement.
5. Compare candidates against relevant wordlists or threat-intelligence data.
6. Identify the matching password.
7. Determine whether the password was subsequently successful.
8. Investigate post-authentication activity.

---

# Investigation Query

```spl
index=botsv1
sourcetype="stream:http"
http_method=POST
form_data="*username*passwd*"
| rex field=form_data "passwd=(?<password>[^&]+)"
| eval password_length=len(password)
| where password_length=6
| stats count by password
```

---

# Enrichment

Compare extracted password candidates against:

- Known password lists
- Application-specific information
- Public information
- Relevant wordlists
- Song/title lists where appropriate

---

# Containment

If a password is confirmed as successful:

- Reset the compromised credential.
- Investigate the affected account.
- Review authentication history.
- Investigate subsequent attacker activity.