# Investigation: Malicious Executable Upload

## Investigation ID

INV-009

---

# Objective

Identify the executable file uploaded by the attacker (Po1s0n1vy) during the compromise of the Joomla web server.

---

# Question

> What is the name of the executable uploaded by Po1s0n1vy? Please include the file extension.

---

# Initial Information

Previous investigations confirmed:

- Joomla compromise
- Brute force attack against the administrator portal
- Attacker infrastructure
- Malicious domains

The next objective was to identify the executable uploaded to the compromised server.

---

# Data Source

- Index: `botsv1`
- Sourcetype: `stream:http`

---

# Investigation Steps

## Step 1 – Initial Hypothesis

Initially, the investigation focused only on the previously identified attacker IP address.

```spl
index=botsv1 sourcetype=stream:http
src_ip=23.22.63.114
http_method=POST
```

### Observation

The expected executable upload was not identified.

---

## Step 2 – Re-evaluate the Assumption

Restricting the search to a single source IP could exclude relevant events.

Instead of assuming every malicious action originated from the same IP, the investigation shifted to identifying **file upload behavior**.

---

## Step 3 – Search for Executable Uploads

```spl
index=botsv1 sourcetype=stream:http
dest_ip="192.168.250.70"
http_method=POST
multipart/form-data
*.exe
```

### Observation

The search identified an HTTP POST request uploading the executable:

```
3791.exe
```

---

# Findings

HTTP analysis confirmed that an executable file named:

```
3791.exe
```

was uploaded to the compromised web server.

The upload was identified by analyzing HTTP POST requests containing multipart form data rather than restricting the investigation to a previously known attacker IP.

---

# Investigation Flow

```
Compromised Web Server
        │
        ▼
HTTP POST Requests
        │
        ▼
Multipart Form Data
        │
        ▼
Executable Upload
        │
        ▼
3791.exe
```

---

# Evidence Collected

| Artifact | Value |
|----------|-------|
| Target Host | 192.168.250.70 |
| HTTP Method | POST |
| Upload Type | multipart/form-data |
| Uploaded File | 3791.exe |

---

# Lessons Learned

Initially, the investigation assumed that every malicious action originated from the previously identified attacker IP.

After broadening the search to focus on the upload behavior instead of the source IP, the malicious executable was successfully identified.

This reinforces an important investigation principle:

> Follow the evidence, not the assumption.

---

# Conclusion

Analysis of HTTP POST requests identified the uploaded executable as:

```
3791.exe
```