# INV-023 — First Suspicious Domain

## Question

What was the first suspicious domain visited by we8105desk on 24AUG2016?

## Answer

solidaritedeproximite.org

## Host

we8105desk

## IP

192.168.250.100

## Date

24 AUG 2016

## Investigation

The workstation IP identified in Q20 was used to investigate network
activity on 24 AUG 2016.

Suricata HTTP and DNS telemetry was examined to identify domains visited
by the workstation.

Normal domains and infrastructure-related lookups were excluded, including
Windows, Microsoft, and Wayne Enterprises domains.

The remaining suspicious domain identified as the first malicious/suspicious
website visited was:

solidaritedeproximite.org

## SPL

index=botsv1
sourcetype=suricata
src_ip="192.168.250.100"
event_type=http
| dedup http.hostname
| table _time http.hostname
| sort _time

## Alternative DNS Query

index=botsv1
sourcetype="stream:dns"
src_ip="192.168.250.100"
"query_type{}"=A
NOT query IN ("*windows*", "*waynecorpinc*", "*microsoft*")
| sort _time
| table _time query

## Finding

First suspicious domain:

solidaritedeproximite.org

## Conclusion

The first suspicious domain visited by we8105desk on 24 AUG 2016 was:

solidaritedeproximite.org

## Evidence Type

Direct BOTS v1 Splunk evidence with threat-intelligence validation.