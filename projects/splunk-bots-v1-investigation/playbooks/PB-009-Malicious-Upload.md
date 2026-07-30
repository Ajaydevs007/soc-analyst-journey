# Playbook: Malicious File Upload Investigation

## Playbook ID

PB-009

---

# Objective

Investigate suspicious executable uploads to a web server.

---

# Procedure

1. Review HTTP POST requests.
2. Identify multipart form-data uploads.
3. Extract uploaded filename.
4. Verify file creation on the endpoint.
5. Determine whether the file was executed.
6. Collect malware sample if available.
7. Block malicious activity.

---

# Containment

- Remove uploaded file.
- Isolate compromised server.
- Reset compromised credentials.
- Review additional uploaded files.

---

# Recovery

- Restore clean application files.
- Patch exploited vulnerabilities.
- Enable file upload restrictions.