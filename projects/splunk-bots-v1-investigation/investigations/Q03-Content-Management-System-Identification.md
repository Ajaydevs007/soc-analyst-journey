# BOTS v1 - Question 03

## Question

What content management system is imreallynotbatman.com likely using?

---

# Objective

Identify the Content Management System (CMS) used by the target web application.

---

# Investigation

The web server hosting `imreallynotbatman.com` was analyzed by reviewing HTTP request paths.

## SPL Query

```spl
index=botsv1 sourcetype=stream:http site="imreallynotbatman.com"
| stats count by uri_path
| sort -count
```

To verify the suspected CMS:

```spl
index=botsv1 sourcetype=stream:http
site="imreallynotbatman.com"
uri_path="*joomla*"
| stats count by uri_path
| sort -count
```

---

# Findings

Over **16,000** HTTP requests referenced Joomla-specific directories and resources, including:

- `/joomla/`
- `/joomla/index.php`
- `/joomla/administrator/`

These paths are characteristic of the Joomla Content Management System.

---

# Evidence

- Joomla directory structure observed
- Joomla administrator interface accessed
- Joomla-specific resources requested

---

# MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Active Scanning | T1595 |
| Gather Victim Host Information | T1592 |

---

# Analyst Notes

Identifying the CMS is an important step because it allows defenders to assess application-specific vulnerabilities and understand why attackers target particular endpoints.

---

# Final Answer

**Joomla**