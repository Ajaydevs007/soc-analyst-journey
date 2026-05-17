# SOC141 - Phishing URL Detected | LetsDefend Walkthrough

## Overview

This repository contains my investigation and analysis of the **SOC141 - Phishing URL Detected** alert from the LetsDefend SOC platform.

The investigation focused on:

- Phishing URL analysis
- Threat intelligence validation
- IOC investigation
- Endpoint containment
- Incident response workflow
- Alert classification

The alert was ultimately classified as a **True Positive** because an internal user successfully accessed a confirmed malicious phishing URL that was not blocked by security controls.

---

# Alert Information

| Field | Value |
|---|---|
| Alert Name | SOC141 - Phishing URL Detected |
| Platform | LetsDefend |
| Category | Phishing |
| Severity | High |
| Classification | True Positive |
| Status | Closed |
| Event Time | Mar 22, 2021 - 09:23 PM |

---

# Alert Details

| Field | Value |
|---|---|
| Source IP | 172.16.17.49 |
| Destination IP | 91.189.114.8 |
| Hostname | EmilyComp |
| Username | ellie |
| Request URL | http://mogagrocol.ru/wp-content/plugins/akismet/fv/index.php?email=ellie@letsdefend.io |
| Device Action | Allowed |
| User-Agent | Mozilla/5.0 (Windows NT 6.1; Win64; x64) Chrome/79.0.3945.88 |

---

# Investigation Process

## Step 1 - Initial Alert Review

The alert indicated that a user attempted to access a suspicious phishing URL.

### Important Findings
- The request was allowed
- The user successfully reached the phishing page
- The URL contained an email parameter
- External domain appeared suspicious

---

## Step 2 - URL Analysis

### Suspicious URL

```text
http://mogagrocol.ru/wp-content/plugins/akismet/fv/index.php?email=ellie@letsdefend.io
```

### Suspicious Indicators

- The domain mimicked a legitimate WordPress plugin path
- URL contained an email parameter
- HTTP was used instead of HTTPS
- Potential credential harvesting behavior identified

---

## Step 3 - Threat Intelligence Investigation

The URL was checked using:
- VirusTotal
- Threat intelligence sources

### VirusTotal Result

```text
13/93 security vendors flagged the URL as malicious
```

This confirmed malicious activity associated with the destination.

---

## Step 4 - Log Investigation

Log Management confirmed:

| Question | Finding |
|---|---|
| Was the URL accessed? | Yes |
| Was the request blocked? | No |
| Internal host affected? | Yes |
| Source Hostname | EmilyComp |
| Username | ellie |

The phishing page was successfully reached by the user.

---

## Step 5 - Endpoint Risk Assessment

### Additional Observations

- Windows NT 6.1 indicates Windows 7
- Chrome version 79 is outdated
- Older systems may have increased exposure to malicious content

Although credential submission could not be confirmed, the event was treated as a potential credential compromise.

---

## Step 6 - Containment

The affected endpoint was isolated using EDR containment procedures.

### Action Taken

```text
Endpoint containment performed successfully
```

---

# Final Verdict

```text
TRUE POSITIVE
```

The alert was classified as a True Positive after confirming that an internal user successfully accessed a malicious phishing URL that was not blocked by security controls.

Although credential submission could not be verified, the event was treated as a potential credential compromise requiring immediate response and containment actions.

---

# Indicators of Compromise (IOCs)

## Malicious URL

```text
http://mogagrocol.ru/wp-content/plugins/akismet/fv/index.php?email=ellie@letsdefend.io
```

## Malicious Domain

```text
mogagrocol.ru
```

## Source IP

```text
172.16.17.49
```

## Destination IP

```text
91.189.114.8
```

## Hostname

```text
EmilyComp
```

## Username

```text
ellie
```

---

# MITRE ATT&CK Mapping

| Technique | ID |
|---|---|
| Phishing | T1566 |
| Spearphishing Link | T1566.002 |
| Valid Accounts (Potential Follow-on Activity) | T1078 |

---

# Actions Taken

- Investigated malicious URL
- Performed threat intelligence analysis
- Reviewed log activity
- Confirmed phishing access attempt
- Contained affected endpoint
- Added artifacts
- Completed analyst notes
- Executed playbook
- Closed alert as True Positive

---

# Key Learnings

- Phishing URLs often mimic legitimate application paths
- Allowed traffic can still be malicious
- Threat intelligence validation is critical during investigations
- Email parameters inside URLs may indicate credential harvesting
- Potential credential compromise should be treated seriously
- Rapid containment helps reduce organizational risk

---

# SOC Workflow Completed

- Alert Triage: Completed
- IOC Analysis: Completed
- Threat Intelligence Validation: Completed
- Endpoint Investigation: Completed
- Containment: Completed
- Analyst Notes Added: Completed
- Artifacts Added: Completed
- Playbook Executed: Completed
- Alert Classification: True Positive
- Incident Status: Closed

---

# Connect With Me

## LinkedIn

```
www.linkedin.com/in/ajaydev-s
```
