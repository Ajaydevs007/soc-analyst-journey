# PB-031 — Suspected Steganographic Payload

## Trigger

An image file is suspected of containing hidden malicious content.

## Investigation Steps

### 1. Identify the downloaded file

Example:

`mhtr.jpg`

### 2. Identify the source

Determine where the file originated.

Example:

`solidaritedeproximite.org`

### 3. Correlate with endpoint telemetry

Search for activity immediately after the download:

- Process creation
- Script execution
- PowerShell
- WScript
- Cmd
- Rundll32
- Other suspicious processes

### 4. Validate the file type

Do not rely only on the file extension.

Compare:

- File extension
- MIME type
- File header/magic bytes
- File size
- File structure

### 5. Analyze the file safely

If the file is available:

- Calculate hashes.
- Analyze it in an isolated malware-analysis environment.
- Inspect for hidden/embedded content.
- Compare against known malware intelligence.

### 6. Containment

If malicious activity is confirmed:

- Isolate the endpoint.
- Block associated infrastructure.
- Preserve the artifact.
- Search for the same artifact across the environment.
- Investigate additional compromised systems.

## Escalation

Escalate to L2/IR when:

- Malware execution is confirmed.
- Ransomware encryption is detected.
- Multiple endpoints are affected.
- Credential compromise is suspected.