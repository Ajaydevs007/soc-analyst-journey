# REP-027 — Remote PDF Encryption

## Executive Summary

Windows file-share telemetry was analyzed to determine the number of PDF
files encrypted by Cerber ransomware on the remote file server.

## Workstation

we8105desk

## Source IP

192.168.250.100

## File Server

we9041srv.waynecorpinc.local

## File Server IP

192.168.250.20

## Data Source

Windows Security Event ID 5145

## Initial Observation

258 distinct PDF filenames were observed.

## Encryption Finding

257 PDFs had the relevant write/encryption activity.

## Conclusion

Cerber encrypted:

257 distinct PDFs

on the remote file server.