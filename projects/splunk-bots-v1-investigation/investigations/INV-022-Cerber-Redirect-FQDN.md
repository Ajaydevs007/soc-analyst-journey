# INV-022 — Cerber Redirect FQDN

## Question

What fully qualified domain name (FQDN) does the Cerber ransomware attempt
to direct the user to at the end of its encryption phase?

## Answer

cerberhhyed5frqa.xmfir0.win

## Victim

we8105desk

## Victim IP

192.168.250.100

## Investigation

The infected workstation identified in Q20 was 192.168.250.100.

DNS traffic originating from this IP was examined to identify suspicious
queries associated with the Cerber ransomware activity.

The DNS query:

cerberhhyed5frqa.xmfir0.win

was identified near the end of the encryption activity.

## SPL

index=botsv1
sourcetype="stream:dns"
src_ip="192.168.250.100"
"query_type{}"=A
| stats count by query
| sort - count

## Focused Query

index=botsv1
sourcetype="stream:dns"
src_ip="192.168.250.100"
query="*cerber*"
| table _time query
| sort _time

## Relevant Timestamp

2016-08-24 17:15:12.668

## Finding

FQDN:

cerberhhyed5frqa.xmfir0.win

## Conclusion

Cerber attempted to direct the user to:

cerberhhyed5frqa.xmfir0.win

## Evidence Type

Direct BOTS v1 Splunk evidence.