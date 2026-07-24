# Playbook: Joomla Reconnaissance Investigation

## Playbook ID

PB-003

---

# Objective

Investigate alerts indicating reconnaissance against a Joomla-based web application.

---

# Trigger

- Multiple requests to Joomla resources
- Enumeration of administrator pages
- Automated vulnerability scanning
- CMS fingerprinting activity

---

# Investigation Procedure

## Step 1 - Identify Source

Determine:

- Source IP
- Source Country
- User-Agent
- Request Frequency

---

## Step 2 - Identify Target

Determine:

- Target IP
- Website
- Application

---

## Step 3 - Review Requested Resources

Check for access to:

- /joomla/
- /administrator/
- /index.php
- /templates/
- /media/

---

## Step 4 - Check for Exploitation

Review for:

- SQL Injection
- Local File Inclusion
- Directory Traversal
- Command Injection
- Remote Code Execution

---

## Step 5 - Determine Scope

Identify:

- Number of requests
- Time range
- Affected resources

---

# Containment

If malicious:

- Block attacker IP
- Enable WAF rules
- Limit request rate
- Notify web administrators

---

# Recovery

- Patch Joomla
- Update plugins
- Remove unused extensions
- Validate system integrity

---

# Lessons Learned

Improve monitoring for:

- CMS fingerprinting
- Administrator enumeration
- Automated vulnerability scanners