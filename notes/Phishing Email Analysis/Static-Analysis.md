# Static Analysis

## Overview

Static Analysis is the process of examining an email, URL, attachment, domain, IP address, or file **without executing or opening it**.

The goal is to gather as much information as possible while avoiding interaction with potentially malicious content.

In phishing investigations, static analysis is always performed before dynamic analysis.

---

# Why is Static Analysis Important?

Opening a malicious attachment or clicking a phishing URL can:

- Execute malware
- Download ransomware
- Steal credentials
- Establish persistence
- Compromise the analyst's system

Instead of interacting with suspicious content, SOC analysts inspect it safely.

---

# Static Analysis Definition

Static Analysis is:

> Examining suspicious content **without executing it**.

Examples:

✔ Analyze Email Headers

✔ Inspect URLs

✔ Check Domain Reputation

✔ Check IP Reputation

✔ Inspect File Names

✔ Calculate File Hashes

✔ Review Metadata

❌ Do NOT open suspicious attachments

❌ Do NOT click suspicious URLs

---

# Static Analysis vs Dynamic Analysis

| Static Analysis | Dynamic Analysis |
|----------------|------------------|
| Does not execute the file | Executes the file in a controlled environment |
| Safe | Higher risk (performed inside a sandbox) |
| Collects metadata | Observes actual behavior |
| Faster | More time-consuming |

---

# What Can Be Analyzed?

## Email Headers

Examples:

- From
- Reply-To
- Return-Path
- Received
- Authentication-Results

Purpose:

Verify sender authenticity and trace the email route.

---

## URLs

Example:

https://paypal-security-login.com

Checks:

- Domain
- Reputation
- Domain Age
- VirusTotal Detection

---

## Domains

Example:

paypal-security-login.com

Checks:

- WHOIS
- Domain Age
- DNS Records
- Reputation

---

## IP Addresses

Example:

101.99.94.116

Checks:

- Reputation
- Blacklists
- Abuse Reports
- ASN
- Hosting Provider

---

## Attachments

Examples:

- Invoice.pdf
- Resume.docx
- Invoice.zip

Checks:

- File Name
- Extension
- File Type
- Hash
- VirusTotal Detection
- Metadata

---

# SOC Investigation Workflow

Suspicious Email

↓

Do NOT click anything

↓

Perform Static Analysis

↓

Analyze:

- Email Headers
- URLs
- Domains
- IP Addresses
- Attachments

↓

Collect Evidence

↓

Determine if Dynamic Analysis is required

---

# Static Analysis Tools

Common SOC tools include:

- VirusTotal
- Cisco Talos Intelligence
- AbuseIPDB
- WHOIS
- MXToolbox
- Any.Run (Hybrid approach)
- Hybrid Analysis

---

# Static Analysis Objectives

A SOC analyst performs static analysis to answer questions such as:

- Is the sender legitimate?
- Does the URL look suspicious?
- Is the domain newly registered?
- Has the IP been reported for malicious activity?
- Is the attachment suspicious?
- Does the email contain phishing indicators?

---

# What Static Analysis Does NOT Do

Static analysis does NOT:

- Execute attachments
- Open suspicious documents
- Visit phishing websites
- Run malware

These tasks belong to **Dynamic Analysis**.

---

# SOC Analyst Workflow

Receive Suspicious Email

↓

Analyze Email Headers

↓

Inspect URLs

↓

Check Domain Reputation

↓

Check IP Reputation

↓

Inspect Attachments

↓

Gather Evidence

↓

Decide Whether Dynamic Analysis is Required

---

# Key Interview Points

- Static Analysis examines suspicious content without executing it.
- Static analysis is safer than dynamic analysis.
- Email headers are analyzed statically.
- URLs, domains, IP addresses, and attachments can all be statically analyzed.
- Static analysis helps determine whether further investigation is necessary.
- Never click a suspicious URL or open a suspicious attachment during static analysis.

---

# Common Interview Questions

### What is Static Analysis?

### Why is Static Analysis important?

### What is the difference between Static Analysis and Dynamic Analysis?

### What artifacts can be analyzed statically?

### Why should a SOC analyst avoid opening suspicious attachments?

### Name some tools commonly used for Static Analysis.

### Which should be performed first: Static Analysis or Dynamic Analysis?
