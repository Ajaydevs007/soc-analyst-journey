# Detection - Web Application Vulnerability Scanning

## Objective

Detect clients performing automated web application vulnerability scanning.

## Log Source

- stream:http

## SPL

```spl
index="botsv1"
sourcetype="stream:http"
| stats count dc(uri_query) as unique_queries by src_ip uri_path
| where count > 1000 AND unique_queries > 100
| sort -count
```

## Detection Logic

Alert when a client:

- Generates a high volume of HTTP requests
- Targets the same application endpoint repeatedly
- Uses a large number of unique query strings

## Severity

Medium

## MITRE ATT&CK

- T1595 – Active Scanning