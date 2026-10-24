# INV-016 — Correct CMS Admin Password

## Question

What was the correct password for admin access to the content management system running "imreallynotbatman.com"?

## Answer

[REDACTED]

## Target

imreallynotbatman.com

## CMS

Joomla

## Administrative Endpoint

/joomla/administrator/index.php

## Investigation

The previous investigations established that Po1s0n1vy conducted a brute-force attack
against the Joomla administrator login.

The brute-force source was:

23.22.63.114

HTTP POST requests containing username and password parameters were examined.

The username targeted was:

admin

A second source IP, 40.80.148.42, submitted credentials to the same administrative
endpoint using a normal browser user agent.

The password submitted by this host was:

[REDACTED]

This distinguishes the successful/legitimate-looking administrative login from the
automated brute-force traffic.

## SPL

index=botsv1
sourcetype="stream:http"
dest_ip="192.168.250.70"
http_method=POST
uri="/joomla/administrator/index.php"
form_data="*username*passwd*"
| rex field=form_data "username=(?<username>[^&]+)"
| rex field=form_data "passwd=(?<password>[^&]+)"
| where username="admin"
| table _time src_ip username password http_user_agent
| sort _time

## Result

40.80.148.42 submitted:

Username: admin
Password: [REDACTED]

## Conclusion

The correct password for administrative access to the Joomla CMS was:

[REDACTED]

## Evidence Type

Direct BOTS v1 Splunk evidence.