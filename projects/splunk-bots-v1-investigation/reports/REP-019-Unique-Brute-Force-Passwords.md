# REP-019 — Unique Brute-Force Password Analysis

## Executive Summary

Analysis of HTTP authentication traffic targeting the Joomla CMS identified
412 unique passwords during the brute-force attack.

## Target

imreallynotbatman.com

## Target IP

192.168.250.70

## Attack Type

HTTP-based password brute force.

## Analysis

Password attempts were extracted from HTTP form_data.

The distinct count of the extracted password field was calculated using
Splunk's dc() function.

## Finding

Unique passwords:

412

## Conclusion

The attacker attempted 412 distinct passwords during the brute-force attack.