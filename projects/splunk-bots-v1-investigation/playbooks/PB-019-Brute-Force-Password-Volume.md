# PB-019 — Brute-Force Password Volume Analysis

## Trigger

Suspected password brute-force activity.

## Step 1 — Identify Authentication Traffic

Search HTTP POST requests containing username and password parameters.

## Step 2 — Extract Password

Use rex to extract the passwd parameter.

## Step 3 — Count Unique Values

Use:

stats dc(password)

## Step 4 — Correlate

Investigate:

- Source IP
- Username
- User-Agent
- Number of total attempts
- Number of unique passwords
- Time span
- Successful authentication

## BOTS v1 Finding

Unique passwords attempted:

412