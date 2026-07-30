# Investigation: Brute Force Password Attack Against Joomla Administrator

## Investigation ID

INV-008

---

# Objective

Identify the source IP address responsible for performing a brute force password attack against the Joomla administrator portal hosted on **imreallynotbatman.com**.

---

# Question

> What IP address is likely attempting a brute force password attack against imreallynotbatman.com?

---

# Initial Information

A brute force attack is characterized by repeated authentication attempts against a login page within a short period of time.

The objective was to identify the client repeatedly submitting login credentials to the Joomla administrator portal.

---

# Data Source

- Index: `botsv1`
- Sourcetype: `stream:http`

---

# Investigation Steps

## Step 1 – Identify Systems Communicating with the Web Server

```spl
index=botsv1 sourcetype="stream:http"
dest_ip=192.168.250.70
| stats count by src_ip
```

### Observation

| Source IP | Requests |
|-----------|---------:|
|192.168.2.50|214|
|23.22.63.114|1429|
|40.80.148.42|17546|

Although **40.80.148.42** generated the highest number of requests, request volume alone does not indicate a brute force attack.

---

## Step 2 – Filter Only HTTP POST Requests

```spl
index=botsv1 sourcetype="stream:http"
dest_ip=192.168.250.70
http_method=POST
| stats count by src_ip
```

### Observation

| Source IP | POST Requests |
|-----------|--------------:|
|23.22.63.114|412|
|40.80.148.42|12844|

Further investigation was required because POST requests may represent many different actions.

---

## Step 3 – Identify Login Attempts

```spl
index=botsv1 sourcetype="stream:http"
dest_ip=192.168.250.70
http_method=POST
form_data="*username*passwd*"
| stats count by src_ip
```

### Observation

| Source IP | Login Attempts |
|-----------|---------------:|
|23.22.63.114|412|
|40.80.148.42|1|

This indicates that **23.22.63.114** repeatedly submitted login credentials.

---

## Step 4 – Verify the Targeted Login Page

```spl
index=botsv1 sourcetype="stream:http"
src_ip=23.22.63.114
http_method=POST
form_data="*username*passwd*"
| table _time request
| sort -_time
```

### Observation

- 412 POST requests
- Same destination endpoint

```
POST /joomla/administrator/index.php HTTP/1.1
```

- Time Window

```
2016-08-11 03:15:21

↓

2016-08-11 03:16:51
```

Approximately **412 login attempts within 90 seconds**.

---

## Step 5 – Review HTTP Response Codes

```spl
index=botsv1 sourcetype="stream:http"
src_ip=23.22.63.114
uri_path="/joomla/administrator/index.php"
| stats count by status
```

### Observation

| HTTP Status | Count |
|------------|------:|
|200|823|
|303|411|

The HTTP responses indicate the server successfully processed the requests and issued redirects where appropriate. HTTP status codes alone do not confirm successful authentication, but they support the observed login activity.

---

# Findings

Analysis of HTTP logs identified repeated authentication attempts against the Joomla administrator login page.

The activity consisted of:

- 412 credential submissions
- Same login endpoint
- Same source IP
- Completed within approximately 90 seconds

This behavior is consistent with a brute force password attack.

---

# Investigation Flow

```
Web Server
      │
      ▼
HTTP Logs
      │
      ▼
Filter POST Requests
      │
      ▼
Filter Login Forms
      │
      ▼
Identify Source IP
      │
      ▼
Verify Repeated Authentication Attempts
      │
      ▼
Confirm Brute Force Activity
```

---

# Evidence Collected

| Artifact | Value |
|----------|-------|
|Target Host|192.168.250.70|
|Target Application|Joomla Administrator|
|Login Endpoint|/joomla/administrator/index.php|
|Source IP|23.22.63.114|
|Login Attempts|412|
|Time Window|03:15:21 – 03:16:51|

---

# Conclusion

The HTTP logs confirm that the IP address **23.22.63.114** repeatedly submitted login credentials to the Joomla administrator portal over a short period of time.

The observed behavior is consistent with a brute force password attack.

**Answer**

```
23.22.63.114
```