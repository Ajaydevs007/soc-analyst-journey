# Playbook: Dynamic DNS Investigation

## Playbook ID

PB-005

---

# Objective

Investigate DNS activity involving dynamic DNS infrastructure associated with malicious activity.

---

# Trigger

- DNS query to a Dynamic DNS provider
- Threat intelligence alert
- Suspicious outbound traffic
- Malware investigation

---

# Investigation Procedure

## Step 1 - Review DNS Logs

Identify:

- Source Host
- Queried Domain
- DNS Server
- Resolved IP

---

## Step 2 - Review Network Connections

Determine whether the resolved IP was contacted.

Review:

- HTTP Logs
- Firewall Logs
- Sysmon Network Connections

---

## Step 3 - Review Endpoint Activity

Investigate:

- Process Creation
- File Creation
- PowerShell
- Scheduled Tasks

---

## Step 4 - Threat Intelligence

Determine whether:

- Domain is malicious
- IP has known reputation
- Infrastructure matches previous attacks

---

# Containment

- Block malicious domain
- Block resolved IP
- Isolate affected hosts
- Collect forensic evidence

---

# Recovery

- Remove malware
- Reset compromised credentials
- Patch vulnerabilities
- Continue monitoring

---

# Lessons Learned

Implement monitoring for:

- Dynamic DNS domains
- Newly observed domains
- Suspicious DNS activity