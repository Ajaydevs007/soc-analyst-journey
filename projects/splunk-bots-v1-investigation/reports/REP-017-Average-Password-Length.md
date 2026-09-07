# REP-017 — Brute-Force Password Analysis

## Executive Summary

Analysis of HTTP authentication traffic targeting the Joomla CMS showed that
the average password length used during the brute-force attack was:

6.174334140435835 characters.

Rounded to the nearest whole number:

6

## Target

imreallynotbatman.com

## Target IP

192.168.250.70

## Attack Type

HTTP-based password brute force.

## Analysis

Password attempts were extracted from the HTTP form_data field.

The length of every extracted password was calculated and averaged.

## Finding

Average password length:

6

## Conclusion

The brute-force password attempts had an average length of approximately
6 characters.