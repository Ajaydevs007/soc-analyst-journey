# Detection: Joomla Application Discovery

## Detection ID

DET-Q03

---

# Objective

Detect HTTP requests attempting to identify or enumerate Joomla-based web applications.

This detection helps identify reconnaissance activity where attackers attempt to fingerprint a Joomla Content Management System (CMS) before launching exploitation attempts.

---

# Data Source

- Splunk Enterprise
- Sourcetype: `stream:http`

---

# SPL Detection Rule

## Detect Joomla Resource Enumeration

```spl
index=botsv1 sourcetype=stream:http
site="imreallynotbatman.com"
(
    uri_path="*/joomla/*"
    OR uri_path="*/administrator/*"
)
| stats count by src_ip dest_ip site uri_path
| sort -count
```

---

## Alternative Detection

Detect repeated requests to Joomla-specific resources.

```spl
index=botsv1 sourcetype=stream:http
uri_path="*/joomla/*"
| stats count by src_ip site
```

---

# Detection Logic

The detection searches for HTTP requests referencing Joomla-specific directories such as:

- `/joomla/`
- `/administrator/`
- `/index.php`

Repeated access to these locations may indicate reconnaissance or automated vulnerability scanning.

---

# MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Active Scanning | T1595 |
| Gather Victim Host Information | T1592 |

---

# Severity

Low

---

# Possible False Positives

- Legitimate users browsing the website
- Search engine crawlers
- Internal vulnerability assessments
- Authorized penetration testing

---

# Analyst Response

1. Identify the source IP.
2. Determine request frequency.
3. Check for SQL Injection or LFI attempts.
4. Investigate administrator page access.
5. Review whether exploitation followed reconnaissance.

---

# References

- Joomla CMS
- OWASP Web Security Testing Guide