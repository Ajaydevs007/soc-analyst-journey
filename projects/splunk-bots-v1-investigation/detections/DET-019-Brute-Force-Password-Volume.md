# DET-019 — Brute-Force Password Volume

## Objective

Determine the number of unique password values used during a suspected
brute-force authentication attack.

## Detection Logic

1. Identify HTTP POST authentication requests.
2. Extract the password from form_data.
3. Count distinct password values.
4. Correlate with source IP and user agent.

## SPL

index=botsv1
sourcetype="stream:http"
http_method=POST
form_data="*username*passwd*"
| rex field=form_data "passwd=(?<password>\w+)"
| stats dc(password) as unique_passwords

## Finding

Unique passwords attempted:

412

## Analyst Note

A high number of distinct password attempts against a single account or
application is a strong indicator of brute-force activity.