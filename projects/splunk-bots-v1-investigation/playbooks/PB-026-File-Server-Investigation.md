# PB-026 — Ransomware File Server Investigation

## Trigger

An infected workstation generates SMB traffic during a ransomware event.

## Step 1 — Identify the Workstation

BOTS v1:

we8105desk

IP:

192.168.250.100

## Step 2 — Search SMB Traffic

Use:

sourcetype="stream:smb"

## Step 3 — Identify Destinations

Aggregate:

stats count by dest_ip

## Step 4 — Identify the File Server

Investigate the destination with significant SMB activity.

## Step 5 — Correlate Hostname

Review SMB paths and DNS/network telemetry to determine the server's
hostname.

## Step 6 — Investigate Impact

Search for:

- Network share access
- File creation
- File modification
- File encryption
- Extension changes
- Windows Event ID 5145
- Suspicious process activity

## BOTS v1 Finding

File server:

we9041srv.waynecorpinc.local

IP:

192.168.250.20