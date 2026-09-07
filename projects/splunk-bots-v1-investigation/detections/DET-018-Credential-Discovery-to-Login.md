# DET-018 — Credential Discovery to Compromised Login

## Objective

Identify the time interval between discovery of a valid credential during
brute-force activity and subsequent use of that credential.

## Detection Logic

1. Extract password attempts from HTTP POST requests.
2. Identify a password appearing multiple times.
3. Sort the events chronologically.
4. Calculate the time delta between attempts.
5. Investigate the second use as a potential compromised login.

## SPL

index=botsv1
sourcetype="stream:http"
dest_ip="192.168.250.70"
http_method=POST
| rex field=form_data "passwd=(?<brutePassword>\w+)"
| search brutePassword="batman"
| sort 0 _time
| delta _time as time_diff
| table _time brutePassword time_diff

## Finding

Credential:

batman

Elapsed time:

92.17 seconds

## Analyst Note

A short interval between automated credential discovery and subsequent
credential use can provide useful evidence of an automated attack followed
by manual or separate-session access.