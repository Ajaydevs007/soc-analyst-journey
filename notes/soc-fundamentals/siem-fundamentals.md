# SIEM Fundamentals

## SIEM

### Full Form

Security Information and Event Management

---

## Definition

SIEM is a security solution that collects, stores, analyzes, and correlates logs from multiple sources to detect security threats.

---

## Main Functions of SIEM

### 1. Log Collection

Collects logs from:

- Windows Systems
- Linux Systems
- Firewalls
- IDS/IPS
- Applications
- Cloud Services

---

### 2. Log Analysis

Analyzes logs to identify suspicious activity.

#### Example

```text
Multiple Failed Login Attempts
```

---

### 3. Event Correlation

Connects related events to identify potential attacks.

#### Example

```text
Failed Login
+
PowerShell Execution
+
External Network Connection

=
Possible Attack
```

---

### 4. Alerting

Generates alerts when suspicious activities are detected.

#### Example

```text
Brute Force Attack Alert
```

---

### 5. Incident Investigation

Provides security analysts with information needed to investigate threats.

---

## Popular SIEM Tools

- Splunk
- IBM QRadar
- Microsoft Sentinel
- Elastic Security
- LogRhythm

---

## Why SOC Analysts Use SIEM

A SOC analyst cannot manually review millions of logs every day.

SIEM helps by:

- Centralizing logs
- Correlating events
- Generating alerts
- Supporting investigations

---

## How SIEM Works

```text
Devices Generate Events
        ↓
Events Are Recorded As Logs
        ↓
SIEM Collects Logs
        ↓
SIEM Correlates Events
        ↓
Alerts Are Generated
        ↓
SOC Analyst Investigates
        ↓
Incident Response
```

---

## Key Takeaway

SIEM acts as the central platform of a SOC by collecting logs, correlating events, generating alerts, and helping analysts detect and respond to threats.
