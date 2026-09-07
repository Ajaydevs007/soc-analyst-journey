# INV-020 — we8105desk IP Address

## Question

What was the most likely IP address of we8105desk on 24AUG2016?

## Answer

192.168.250.100

## Target Host

we8105desk

## Date

24 AUG 2016

## Investigation

The investigation began by searching the BOTS v1 dataset for events
associated with the hostname `we8105desk`.

The source IP addresses were then aggregated and sorted by frequency.

## SPL

index=botsv1 host=we8105desk earliest="08/24/2016:00:00:00" latest="08/24/2016:23:59:59"
| stats count by src_ip
| sort - count

## Alternative LDAP Query

index=botsv1 sourcetype="stream:ldap" host=we8105desk
| stats count by src_ip
| sort - count

## Result

The most likely IP address associated with we8105desk was:

192.168.250.100

## Conclusion

The workstation `we8105desk` was using 192.168.250.100 on
24 AUG 2016.

## Evidence Type

Direct BOTS v1 Splunk evidence.