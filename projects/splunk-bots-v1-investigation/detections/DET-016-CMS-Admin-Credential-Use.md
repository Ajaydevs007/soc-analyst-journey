# DET-016 — CMS Admin Credential Use

## Objective

Detect successful or suspicious authentication attempts against the Joomla
administrator interface.

## Target

imreallynotbatman.com

## Detection Logic

Monitor HTTP POST requests to:

/joomla/administrator/index.php

Extract:

- username
- password
- source IP
- user agent

Flag authentication attempts where:

- username = admin
- source IP differs from the known brute-force source
- credentials are reused
- authentication follows a brute-force sequence

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

## Key IOC

Successful credential observed:

Username: admin
Password: batman
Source IP: 40.80.148.42

## Analyst Note

Credentials should not normally be stored or exposed in production detections.
This detection is specific to the historical BOTS v1 training dataset.