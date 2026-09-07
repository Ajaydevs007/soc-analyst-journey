# Detection: Brute Force Password Pattern

## Detection ID

DET-015

---

# Objective

Detect repeated authentication attempts and extract attempted passwords for investigation.

---

# SPL

```spl
index=botsv1
sourcetype="stream:http"
http_method=POST
form_data="*username*passwd*"
| rex field=form_data "passwd=(?<passwd>[^&]+)"
| stats count dc(passwd) as unique_passwords values(passwd) as attempted_passwords by src_ip dest_ip
| where count > 20