# PB-029 — Cerber File Encryption Investigation

## Trigger

Suspected ransomware file encryption on an endpoint.

## Step 1 — Identify Host

we8105desk

## Step 2 — Identify User Profile

C:\Users\bob.smith.WAYNECORPINC\

## Step 3 — Search Sysmon

Focus on EventCode=2.

## Step 4 — Filter File Type

Target:

*.txt

## Step 5 — Count Unique Files

Use:

stats dc(TargetFilename)

## Step 6 — Validate

Correlate file modification events with:

- Cerber process execution
- File extension changes
- Ransom notes
- Network activity
- Encryption timeline

## BOTS v1 Finding

406 distinct .txt files were encrypted.