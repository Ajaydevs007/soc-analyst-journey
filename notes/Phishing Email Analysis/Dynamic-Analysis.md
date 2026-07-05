# Dynamic Analysis

## Overview

Dynamic Analysis is the process of executing a suspicious file or visiting a suspicious website inside an isolated environment (sandbox) to observe its behavior.

Unlike Static Analysis, which examines files without execution, Dynamic Analysis allows SOC analysts to understand what the file actually does when it runs.

---

# Why is Dynamic Analysis Important?

Static Analysis can tell us:

- File Name
- Extension
- Hash
- Metadata
- Reputation

However, it cannot answer questions like:

- Does the file create new files?
- Does it contact a Command and Control (C2) server?
- Does it steal credentials?
- Does it encrypt files?
- Does it establish persistence?

Dynamic Analysis answers these questions by executing the file in a safe environment.

---

# Dynamic Analysis Definition

Dynamic Analysis is:

> Executing suspicious files or visiting suspicious websites inside a controlled and isolated environment to observe their behavior.

---

# Static Analysis vs Dynamic Analysis

| Static Analysis | Dynamic Analysis |
|----------------|------------------|
| Does not execute the file | Executes the file safely |
| Examines metadata | Observes runtime behavior |
| Faster | Slower |
| Safer | Performed inside a sandbox |
| Collects static indicators | Collects behavioral indicators |

---

# What Can Be Dynamically Analyzed?

## Files

Examples:

- EXE
- DLL
- DOCX
- PDF
- XLSM
- ZIP
- Scripts

Purpose:

Observe what the file does after execution.

---

## URLs

Instead of opening suspicious URLs on a personal computer, analysts open them inside sandbox browsers.

Purpose:

Observe:

- Redirections
- Credential harvesting
- Drive-by downloads
- Browser exploits

---

## Websites

Monitor:

- Network connections
- Downloads
- JavaScript execution
- Malicious behavior

---

# Sandbox

## What is a Sandbox?

A sandbox is an isolated environment where suspicious files or websites can be safely executed without affecting the analyst's computer or the production network.

Think of it as a secure laboratory for malware analysis.

---

# Why Use a Sandbox?

Executing malware on a personal computer may:

- Infect the system
- Steal credentials
- Encrypt files
- Spread through the network

A sandbox isolates the malware so analysts can safely observe its behavior.

---

# What Does a Sandbox Monitor?

## File Activity

- File creation
- File deletion
- File modification

---

## Process Activity

- Parent process
- Child processes
- PowerShell execution
- CMD execution
- Process injection

---

## Registry Activity (Windows)

- Registry key creation
- Registry modification
- Persistence mechanisms

---

## Network Activity

- DNS queries
- HTTP/HTTPS requests
- IP addresses contacted
- Command and Control (C2) communication

---

## System Changes

- Scheduled tasks
- Services
- Startup entries
- Persistence techniques

---

# Common Sandbox Platforms

- ANY.RUN
- Hybrid Analysis (Falcon Sandbox)
- VMRay
- Joe Sandbox

---

# Dynamic Analysis Workflow

Receive Suspicious Email

↓

Perform Static Analysis

↓

Still Suspicious?

↓

Execute Inside Sandbox

↓

Observe Behavior

↓

Collect Indicators of Compromise (IOCs)

↓

Determine if the file or URL is malicious

---

# Information Gathered During Dynamic Analysis

A SOC analyst may collect:

- Created files
- Deleted files
- Registry modifications
- Running processes
- Network connections
- Downloaded payloads
- Command and Control (C2) servers
- Domains contacted
- IP addresses contacted
- Persistence mechanisms
- Mutexes
- Indicators of Compromise (IOCs)

---

# Important Considerations

## Never execute suspicious files on:

- Personal computer
- Work laptop
- Production server

Always use:

- Sandbox
- Isolated Virtual Machine (VM)
- Malware Analysis Environment

---

## Browser Isolation

Instead of visiting suspicious URLs directly:

Use:

- Browserling
- ANY.RUN
- Hybrid Analysis

This reduces the risk of browser exploitation and malware infection.

---

## URL Parameters

Before opening a suspicious URL, inspect its parameters.

Example:

https://example.com/login?email=user@example.com

Potential Risk:

The attacker can identify valid email addresses simply when the victim visits the page, even if credentials are never entered.

Sensitive parameters should be removed or sanitized before analysis whenever possible.

---

## Malware Evasion

Some malware intentionally delays its malicious activity to avoid detection.

Example:

- Waits several minutes
- Sleeps before executing
- Waits for user interaction

Do not immediately conclude that a file is safe if no malicious activity appears during the first few seconds of execution.

---

## Images Can Be Malicious

An email without URLs or attachments is not necessarily safe.

Attackers may:

- Hide payloads inside images
- Use image-based phishing
- Deliver malware using steganography

Always evaluate the email as a whole.

---

# SOC Investigation Workflow

Suspicious Email

↓

Perform Static Analysis

↓

Analyze:

- URLs
- Attachments
- Headers

↓

Dynamic Analysis Required?

↓

Execute in Sandbox

↓

Observe:

- File Activity
- Process Activity
- Registry Changes
- Network Traffic
- Persistence

↓

Collect Indicators of Compromise (IOCs)

↓

Determine Malicious Behavior

---

# Key Interview Points

- Dynamic Analysis executes suspicious content in a safe environment.
- Dynamic Analysis focuses on behavior rather than metadata.
- Sandboxes isolate malware from production systems.
- Dynamic Analysis helps identify persistence, C2 communication, registry modifications, and file system changes.
- Never execute suspicious files on a personal or production system.
- Malware may delay execution to evade sandbox detection.
- URLs should be analyzed carefully before visiting, especially if they contain user-specific parameters.

---

# Common Interview Questions

### What is Dynamic Analysis?

### What is the difference between Static Analysis and Dynamic Analysis?

### What is a sandbox?

### Why should malware be executed inside a sandbox?

### What information can be collected during Dynamic Analysis?

### Why should suspicious URLs not be opened on a personal computer?

### Why do some malware samples delay their execution?

### Name some commonly used malware sandbox platforms.

### Can an email without attachments or URLs still be malicious?

### Why should URL parameters be inspected before opening a phishing website?