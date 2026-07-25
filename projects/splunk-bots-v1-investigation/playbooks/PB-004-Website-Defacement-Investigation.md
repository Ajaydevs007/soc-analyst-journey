# Playbook: Website Defacement Investigation

## Playbook ID

PB-004

---

# Objective

Investigate suspected website defacement incidents by identifying malicious downloads, compromised files, and attacker activity.

---

# Trigger

- Website appearance changed
- Unexpected files in web directory
- Outbound connections to malicious domains
- User reports website defacement

---

# Investigation Procedure

## Step 1 - Confirm Defacement

- Verify affected website
- Identify modified content
- Determine first observed time

---

## Step 2 - Review Network Activity

Identify outbound HTTP connections.

Determine:

- Source IP
- Destination IP
- Downloaded resources

---

## Step 3 - Review Firewall Logs

Look for:

- Malicious Websites
- Blocked requests
- Downloaded files

---

## Step 4 - Review Endpoint Logs

Investigate:

- File Creation
- Process Creation
- PowerShell
- CMD
- IIS Logs

---

## Step 5 - Determine Scope

Identify:

- Modified files
- Additional compromised systems
- Persistence mechanisms

---

# Containment

- Disconnect compromised server
- Remove malicious files
- Block attacker IPs
- Block malicious domains

---

# Recovery

- Restore website from backup
- Patch vulnerable applications
- Reset credentials
- Verify file integrity

---

# Lessons Learned

Improve monitoring for:

- Website changes
- File downloads
- Web shell uploads
- Outbound malicious connections