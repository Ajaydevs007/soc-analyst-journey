# DET-026 — Ransomware File Server Connection

## Objective

Identify network file servers accessed by an infected workstation during
a ransomware incident.

## Detection Logic

1. Identify the infected workstation.
2. Filter for SMB traffic.
3. Identify destination IPs.
4. Count SMB connections per destination.
5. Investigate the dominant destination.
6. Correlate the destination with hostname and file activity.

## SPL

index=botsv1
sourcetype="stream:smb"
src_ip="192.168.250.100"
| stats count by dest_ip
| sort - count

## Finding

Source:

192.168.250.100

Destination:

192.168.250.20

File Server:

we9041srv.waynecorpinc.local

## Analyst Note

Ransomware accessing a remote SMB server is significant because encryption
may extend beyond the local endpoint to network shares.