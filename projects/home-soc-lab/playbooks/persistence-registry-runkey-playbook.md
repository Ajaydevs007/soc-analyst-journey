# Registry Run Key Persistence Investigation Playbook

---

# Playbook Information

| Field | Value |
|--------|-------|
| Playbook ID | PB-005 |
| Playbook Name | Registry Run Key Persistence Investigation |
| Severity | High |
| ATT&CK Technique | T1547.001 – Registry Run Keys / Startup Folder |
| Author | Ajaydev S |

---

# Objective

Provide a structured workflow for investigating registry-based persistence detected through Sysmon Event ID 13 and related telemetry. The goal is to determine whether a registry modification is legitimate administrative activity or an unauthorized persistence mechanism.

---

# Trigger

Initiate this playbook when:

- Sysmon Event ID 13 (Registry Value Set) is detected.
- A new value is created under Run or RunOnce registry keys.
- `reg.exe` modifies autorun locations.
- PowerShell modifies registry persistence locations.
- Suspicious registry modifications are observed in Splunk.

---

# Initial Triage

Collect the following information:

- Registry path
- Registry value name
- Configured executable
- Executing process
- Parent process
- Executing user
- Timestamp
- Hostname

---

# Investigation Steps

## Step 1 — Review Sysmon Event ID 13

Verify:

- TargetObject
- Details (configured executable)
- Image
- User
- Process ID
- Timestamp

Determine exactly what registry value was created or modified.

---

## Step 2 — Correlate with Sysmon Event ID 1

Review the related process creation event.

Confirm:

- Executable (`reg.exe`, `powershell.exe`, etc.)
- Parent process
- Command line
- User
- Integrity level

This identifies which process performed the registry modification.

---

## Step 3 — Validate the Persistence Entry

Determine:

- Is the registry location expected?
- Is the executable legitimate?
- Is the executable digitally signed?
- Does the executable reside in a trusted directory?
- Is the persistence entry authorized?

---

## Step 4 — Review Additional Telemetry

Correlate with:

- Authentication events (4624 / 4625)
- PowerShell logs
- Network connections (Sysmon Event ID 3)
- Additional registry modifications
- Process creation events
- Scheduled tasks
- Startup folder activity

---

## Step 5 — Determine Legitimacy

Classify the activity as:

- Expected application configuration
- Software installation
- Authorized administrative activity
- Authorized security testing
- Suspicious persistence
- Confirmed malicious persistence

---

# Response Actions

If malicious persistence is confirmed:

- Preserve registry evidence.
- Export the affected registry key.
- Isolate the endpoint if necessary.
- Remove unauthorized persistence.
- Investigate the originating process.
- Review additional persistence mechanisms.
- Escalate to Incident Response.

If activity is authorized:

- Document the registry modification.
- Record supporting evidence.
- Close the investigation.
- Update investigation notes.

---

# Evidence to Collect

- Registry query output
- Sysmon Event ID 13
- Sysmon Event ID 1
- Event Viewer screenshots
- Splunk search results
- Registry path
- Configured executable
- User context
- Timeline reconstruction

---

# Escalation Criteria

Escalate when:

- Unknown executables are configured for autorun.
- Run or RunOnce keys are modified unexpectedly.
- PowerShell establishes persistence.
- Persistence follows suspicious authentication or command execution.
- Multiple persistence mechanisms are observed.

---

# Detection References

Primary Detection

- Sysmon Event ID 13

Supporting Detection

- Sysmon Event ID 1
- Windows Security Events
- PowerShell Operational Logs

---

# Related Documents

- `logs-analysis/persistence-detection-analysis.md`
- `mitre-mapping/persistence-registry-runkey-mitre.md`
- `splunk-detections/persistence-registry-runkey-detection.md`
- `incident-report/persistence-registry-runkey-incident-report.md`

---

# Lessons for Analysts

- Registry Run Keys are one of the most common Windows persistence mechanisms.
- Sysmon Event ID 13 provides excellent visibility into registry modifications.
- Event ID 1 identifies the process responsible for the change.
- Correlating registry, process, and authentication telemetry provides stronger investigative context.
- Legitimate Windows features should always be evaluated within the broader attack timeline before determining malicious intent.

---

Playbook Version: 2.0

Last Updated: 2026-07-11

Author: Ajaydev S