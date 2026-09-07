# PB-030 — Cerber Cryptor Download Investigation

## Trigger

Detection of suspicious HTTP activity involving a potentially malicious payload.

## Investigation Steps

### 1. Identify the source host

Determine which internal endpoint initiated the HTTP request.

Example:

`192.168.250.100`

### 2. Identify the destination

Determine the external domain/IP contacted.

Example:

`solidaritedeproximite.org`

### 3. Identify the requested file

Inspect the URI/path.

Example:

`/mhtr.jpg`

### 4. Review surrounding network activity

Check for:

- DNS lookups
- HTTP requests
- Additional downloads
- Connections to related IP addresses
- Suricata alerts

### 5. Check endpoint activity

Correlate the download timestamp with:

- Sysmon process creation
- File creation
- Script execution
- PowerShell/cmd activity
- Suspicious parent-child process relationships

### 6. Determine payload behavior

If available, submit the file to approved malware-analysis infrastructure and determine whether the apparent image file contains executable/malicious content.

### 7. Containment

If the host is confirmed compromised:

- Isolate the endpoint.
- Block malicious domains/IPs.
- Preserve relevant logs.
- Identify additional affected hosts.
- Begin ransomware-response procedures.

## Expected IOC

`mhtr.jpg`