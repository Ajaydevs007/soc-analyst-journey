# 01 — Lab Setup

## Windows Server VM

Created the Windows Server virtual machine that will later become the Active Directory Domain Controller.

### VM Configuration

| Setting | Configuration |
|---|---|
| VM Name | `SOC-LAB-DC` |
| OS | Windows Server 2022 Standard Evaluation |
| Installation | Desktop Experience |
| RAM | 4096 MB |
| CPU | 2 vCPUs |
| Virtual Disk | 60 GB |
| Hypervisor | VirtualBox |

## Network Configuration

The Server VM uses two network adapters.

### Adapter 1 — NAT

Used for outbound Internet connectivity.

```text
IPv4: 10.0.2.15
Subnet Mask: 255.255.255.0
Gateway: 10.0.2.2

Internet connectivity was verified using:

ping 8.8.8.8

Result:

4 packets sent
4 packets received
0% packet loss



### Adapter 2 — Internal Network

Connected to the isolated VirtualBox internal network:

Network: soc-lab-net

Static IPv4 configuration:

IPv4: 10.10.10.10
Subnet Mask: 255.255.255.0
Default Gateway: None

This interface will be used for communication between the Domain Controller, Windows 10 workstation, and Kali Linux attacker.


Current Architecture


                 Internet
                    │
                  NAT
                    │
             10.0.2.15
                    │
             ┌─────────────┐
             │ SOC-LAB-DC  │
             │ Windows     │
             │ Server 2022 │
             └──────┬──────┘
                    │
              10.10.10.10
                    │
              soc-lab-net
                    │
          ┌─────────┴─────────┐
          │                   │
       Kali Linux          Windows 10
       Attacker            Workstation




Evidence

screenshots/01-lab-setup/server-network-configuration.png
screenshots/01-lab-setup/windows-server-installed.png


Status
[x] Windows Server VM created
[x] Windows Server 2022 installed
[x] Computer renamed to SOC-LAB-DC
[x] NAT connectivity verified
[x] Internal lab network configured
[ ] Active Directory Domain Services





## Windows Firewall Troubleshooting

During initial network testing, Windows 10 could not ping the Windows Server VM while Windows Firewall was enabled.

### Problem

Initial test:

```text
Windows 10 → 10.10.10.10
Request timed out
100% packet loss

The same communication worked when Windows Firewall was temporarily disabled, confirming that VirtualBox networking and the IP configuration were functioning correctly.

Investigation

The network profiles were checked using:

Get-NetConnectionProfile

The soc-lab-net interface was identified as:

InterfaceAlias   : Ethernet 2
NetworkCategory  : Public
IPv4Connectivity : LocalNetwork

Because the isolated lab network was classified as a Public/Unidentified network, Windows Firewall was blocking inbound ICMP traffic.

Resolution

Instead of disabling the firewall, a narrowly scoped ICMPv4 rule was created on both Windows machines.

The rule allows ICMP Echo Requests only within the SOC lab subnet:

10.10.10.0/24

Example rule:

New-NetFirewallRule -DisplayName "SOC-LAB Allow ICMPv4 Echo" `
-Direction Inbound `
-Protocol ICMPv4 `
-IcmpType 8 `
-LocalAddress 10.10.10.10 `
-RemoteAddress 10.10.10.0/24 `
-Action Allow `
-Profile Any

A corresponding rule was created on Windows 10 for 10.10.10.20.

Final Verification

Windows 10 → Windows Server:

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

Windows Server → Windows 10:

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
Result

The Windows Server and Windows 10 VMs can now communicate successfully over the isolated soc-lab-net network while Windows Firewall remains enabled.

This confirmed that the issue was firewall policy rather than VirtualBox networking or IP configuration.


Also update the **Status** section at the bottom of that file:

```markdown
- [x] Windows Server VM created
- [x] Windows Server 2022 installed
- [x] Renamed to SOC-LAB-DC
- [x] NAT connectivity verified
- [x] Internal lab network configured
- [x] Windows 10 VM created
- [x] Windows 10 installed
- [x] Renamed to SOC-LAB-WIN10
- [x] Windows Server ↔ Windows 10 connectivity verified
- [x] Firewall rules configured without disabling Windows Firewall
- [ ] Active Directory Domain Services



## Three-Machine Network Verification

The isolated SOC lab network was successfully configured using the VirtualBox
Internal Network `soc-lab-net`.

### Lab Network

| Machine | Internal IP | Purpose |
|---|---:|---|
| SOC-LAB-DC | 10.10.10.10 | Domain Controller / AD DS |
| SOC-LAB-WIN10 | 10.10.10.20 | Windows endpoint |
| Kali Linux | 10.10.10.30 | Attacker / security testing |

Subnet:

```text
10.10.10.0/24

The internal network does not use a default gateway.

Connectivity Tests
Windows 10 → Domain Controller
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
Domain Controller → Windows 10
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
Kali → Domain Controller
4 packets transmitted, 4 received, 0% packet loss
Average RTT: 0.710 ms
Kali → Windows 10
4 packets transmitted, 4 received, 0% packet loss
Average RTT: 0.650 ms
Network Status

The three lab machines can communicate successfully over the isolated
soc-lab-net network.

Windows Firewall remains enabled on the Windows machines, with narrowly
scoped ICMP rules allowing communication within the lab subnet.

Current Architecture
                 NAT / Internet
                       │
          ┌────────────┼────────────┐
          │            │            │
        Kali       SOC-LAB-DC   SOC-LAB-WIN10
     10.0.2.15      10.0.2.15      10.0.2.15
          │            │            │
          └────── soc-lab-net ──────┘
                 10.10.10.0/24

       Kali          DC           Windows 10
   10.10.10.30   10.10.10.10    10.10.10.20


Status
[x] Three lab machines created
[x] Internal network configured
[x] Kali connected to soc-lab-net
[x] Windows Server connected to soc-lab-net
[x] Windows 10 connected to soc-lab-net
[x] Kali → DC connectivity verified
[x] Kali → Windows 10 connectivity verified
[x] DC → Windows 10 connectivity verified
[x] Windows 10 → DC connectivity verified
[ ] Active Directory Domain Services