# Detection: Malicious Executable Upload

## Detection ID

DET-009

---

# Objective

Detect executable files uploaded through HTTP POST requests.

---

# Data Source

- stream:http

---

# SPL Detection Rule

```spl
index=botsv1 sourcetype=stream:http
http_method=POST
multipart/form-data
("*.exe" OR "*.dll" OR "*.scr" OR "*.bat")
| table _time src_ip dest_ip uri_path request
```

---

# Detection Logic

Monitor HTTP POST requests containing multipart form-data where executable files are uploaded to a web application.

---

# MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Ingress Tool Transfer | T1105 |

---

# Severity

High

---

# Analyst Response

- Validate uploaded filename.
- Identify upload endpoint.
- Confirm whether the file was executed.
- Isolate affected host if malicious.