# INV-026 — File Server IP Address

## Question

Bob Smith's workstation (we8105desk) was connected to a file server
during the ransomware outbreak. What is the IP address of the file server?

## Answer

192.168.250.20

## Workstation

we8105desk

## Workstation IP

192.168.250.100

## File Server

we9041srv.waynecorpinc.local

## Investigation

The investigation focused on SMB traffic originating from Bob Smith's
workstation.

SMB is the Windows protocol used for network file sharing, making it the
appropriate data source for identifying the remote file server.

## SPL

index=botsv1
sourcetype="stream:smb"
src_ip="192.168.250.100"
| stats count by dest_ip
| sort - count

## Result

The dominant destination IP was:

192.168.250.20

The associated file server hostname was:

we9041srv.waynecorpinc.local

## Conclusion

The IP address of the file server was:

192.168.250.20

## Evidence Type

Direct BOTS v1 Splunk evidence.