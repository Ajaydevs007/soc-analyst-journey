# PB-017 — Brute-Force Password Analysis

## Trigger

Suspected brute-force authentication activity.

## Step 1 — Identify Authentication Traffic

Search HTTP POST requests targeting the authentication endpoint.

## Step 2 — Extract Password Attempts

Use rex to extract the password from form_data.

## Step 3 — Calculate Password Length

Use:

eval password_length=len(brutePassword)

## Step 4 — Calculate Statistics

Use:

stats avg(password_length)

## Step 5 — Correlate

Correlate password characteristics with:

- Source IP
- Number of attempts
- Time interval
- Username
- Successful authentication
- User-Agent

## BOTS v1 Finding

Average password length:

6.174334140435835

Rounded result:

6