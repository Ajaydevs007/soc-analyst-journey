# INV-018 — Password Discovery to Compromised Login

## Question

How many seconds elapsed between the time the brute force password scan
identified the correct password and the compromised login?

## Answer

92.17 seconds

## Correct Password

batman

## Investigation

The correct password identified in the previous investigation was `batman`.

Searching the HTTP authentication traffic for this password revealed two
events.

The first event represents the brute-force scanner identifying the correct
password.

The second event represents the subsequent compromised login using the
same credential.

## Relevant Events

First event:

2016/08/10 21:46:33.689

Second event:

2016/08/10 21:48:05.858

## Calculation

21:48:05.858
-
21:46:33.689
=
92.169084 seconds

Rounded to two decimal places:

92.17 seconds

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

## Conclusion

The attacker waited approximately 92.17 seconds between discovering the
correct password and using it for the compromised login.

## Evidence Type

Direct BOTS v1 Splunk evidence.