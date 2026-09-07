# Investigation: First Brute Force Password

## Investigation ID

INV-014

---

# Objective

Identify the first password attempted during Po1s0n1vy's brute-force attack against the Joomla administrator login.

---

# Question

> What was the first brute force password used?

---

# Background

Previous investigations identified a brute-force attack against the Joomla administrator interface of the compromised web server.

Known attacker IP:

```text
23.22.63.114
```

Target IP:

```text
192.168.250.70
```

Target authentication endpoint:

```text
/joomla/administrator/index.php
```

---

# Data Source

- Index: `botsv1`
- Sourcetype: `stream:http`
- Protocol: HTTP

---

# Investigation Steps

## Step 1 - Identify Authentication Requests

The investigation focused on HTTP POST requests containing both username and password parameters.

```spl
index=botsv1
sourcetype="stream:http"
http_method=POST
form_data="*username*passwd*"
```

These events represent authentication attempts against the Joomla login interface.

---

## Step 2 - Extract Username and Password

The `form_data` field contains the submitted credentials.

Regular expressions were used to extract the username and password.

```spl
index=botsv1
sourcetype="stream:http"
http_method=POST
form_data="*username*passwd*"
| rex field=form_data "username=(?<username>\w+)"
| rex field=form_data "passwd=(?<password>\w+)"
| table _time username password
| sort _time
```

---

## Step 3 - Sort Chronologically

The events were sorted by `_time` so that the earliest authentication attempt appeared first.

The first password observed was:

```text
12345678
```

---

# Findings

The first password used during the brute-force attack was:

```text
12345678
```

The attack consisted of repeated HTTP POST requests against the Joomla administrator login endpoint.

---

# Investigation Flow

```text
Po1s0n1vy
     |
     v
Brute Force Attack
     |
     v
HTTP POST
     |
     v
Joomla Administrator Login
     |
     v
form_data
     |
     +------ username
     |
     +------ passwd
               |
               v
        Sort by _time
               |
               v
          12345678
```

---

# Evidence

| Field | Value |
|---|---|
| Attacker IP | `23.22.63.114` |
| Target IP | `192.168.250.70` |
| HTTP Method | `POST` |
| Authentication Endpoint | `/joomla/administrator/index.php` |
| First Password | `12345678` |

---

# Conclusion

Analysis of HTTP authentication requests showed that the first password attempted during the brute-force attack was:

```text
12345678
```