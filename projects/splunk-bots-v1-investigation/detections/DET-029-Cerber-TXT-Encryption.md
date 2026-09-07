# DET-029 — Cerber TXT File Encryption

## Objective

Identify text files encrypted by Cerber ransomware in Bob Smith's
Windows profile.

## Detection Logic

1. Identify the infected workstation.
2. Search Sysmon Event ID 2.
3. Restrict TargetFilename to Bob Smith's profile.
4. Filter for .txt files.
5. Count distinct TargetFilename values.

## SPL

index=botsv1
host=we8105desk
sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
EventCode=2
TargetFilename="C:\\Users\\bob.smith.WAYNECORPINC\\*.txt"
| stats dc(TargetFilename) as encrypted_txt_files

## Finding

406 distinct .txt files were encrypted.

## Analyst Note

Distinct counting is important because multiple telemetry events may exist
for the same file.