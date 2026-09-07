# DET-017 — Brute-Force Password Length Analysis

## Objective

Analyze password characteristics used during a brute-force attack.

## Detection Logic

1. Identify HTTP POST requests against the Joomla administrator interface.
2. Extract the attempted password.
3. Calculate password length.
4. Calculate statistical averages.

## SPL

index=botsv1
sourcetype="stream:http"
dest_ip="192.168.250.70"
http_method=POST
form_data="*username*passwd*"
| rex field=form_data "passwd=(?<brutePassword>\w+)"
| eval password_length=len(brutePassword)
| stats avg(password_length) as avg_password_length
| eval answer=round(avg_password_length,0)

## Result

Raw average:

6.174334140435835

Rounded:

6

## Analyst Note

Password-length statistics can help characterize brute-force or password-
spraying activity, although password length alone should not be treated as
evidence of malicious activity.