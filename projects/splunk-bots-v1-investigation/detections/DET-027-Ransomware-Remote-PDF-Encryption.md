# DET-027 — Ransomware Remote PDF Encryption

## Objective

Identify PDF files encrypted by ransomware on a remote Windows file server.

## Detection Logic

1. Identify the infected workstation.
2. Identify the remote file server.
3. Search Windows Security Event ID 5145.
4. Filter for PDF files.
5. Extract Relative_Target_Name.
6. Count distinct files.
7. Validate that the events represent write/encryption activity.

## SPL

index=botsv1
sourcetype="WinEventLog:*"
"*.pdf"
dest="we9041srv.waynecorpinc.local"
Source_Address="192.168.250.100"
| stats dc(Relative_Target_Name) as encrypted_pdfs

## Finding

257 distinct PDF files were encrypted.

## Analyst Note

File access does not automatically mean encryption. Write-related access
should be correlated with ransomware execution and the incident timeline.