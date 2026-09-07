# REP-026 — Ransomware File Server

## Executive Summary

SMB traffic from Bob Smith's workstation was analyzed to identify the
remote file server involved in the Cerber ransomware outbreak.

## Workstation

we8105desk

## Workstation IP

192.168.250.100

## File Server

we9041srv.waynecorpinc.local

## File Server IP

192.168.250.20

## Assessment

The SMB connection indicates that the infected workstation had access to
the remote file server.

This is significant because the ransomware subsequently impacted files
located on the remote server.

## Conclusion

The file server IP address was:

192.168.250.20