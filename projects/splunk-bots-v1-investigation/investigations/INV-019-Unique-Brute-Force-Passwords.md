# INV-019 — Unique Brute-Force Passwords

## Question

How many unique passwords were attempted in the brute force attempt?

## Answer

412

## Investigation

The brute-force attack against the Joomla administrator interface generated
HTTP POST requests containing password attempts in the form_data field.

The password value was extracted using rex and the distinct count was
calculated using the Splunk dc() function.

## SPL

index=botsv1
sourcetype="stream:http"
http_method=POST
form_data="*username*passwd*"
| rex field=form_data "passwd=(?<password>\w+)"
| stats dc(password) as unique_passwords

## Result

unique_passwords = 412

## Conclusion

The brute-force attack attempted 412 unique passwords.

## Evidence Type

Direct BOTS v1 Splunk evidence.