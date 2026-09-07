# PB-027 — Remote File Encryption Investigation

## Trigger

Ransomware activity is suspected of affecting files on a network share.

## Step 1 — Identify Infected Workstation

we8105desk

192.168.250.100

## Step 2 — Identify Remote File Server

we9041srv.waynecorpinc.local

192.168.250.20

## Step 3 — Search File-Share Events

Use Windows Security Event ID 5145.

## Step 4 — Filter File Type

Focus on:

*.pdf

## Step 5 — Extract Unique Files

Use:

stats dc(Relative_Target_Name)

## Step 6 — Validate Encryption

Check access fields for write/modify activity and correlate with the
ransomware execution timeline.

## BOTS v1 Finding

PDF files observed:

258

PDF files actually encrypted:

257