# INV-017 — Average Password Length

## Question

What was the average password length used in the password brute forcing attempt?

## Answer

6

## Investigation

The brute-force attack against the Joomla administrator interface generated
HTTP POST requests containing password attempts in the `form_data` field.

Each attempted password was extracted and its character length calculated.

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

Average password length:

6.174334140435835

Rounded to the nearest whole number:

6

## Conclusion

The average password length used during the brute-force attack was:

6

## Evidence Type

Direct BOTS v1 Splunk evidence.