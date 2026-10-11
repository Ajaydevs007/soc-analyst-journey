# 01 — Lab Setup

This phase establishes the isolated virtual network and the three virtual machines used throughout the Detection Lab.

## Lab Components

| Machine | Operating System | Role | Internal IP |
|---|---|---|---:|
| `SOC-LAB-DC` | Windows Server 2022 Standard Evaluation | Domain Controller / AD DS | `10.10.10.10` |
| `SOC-LAB-WIN10` | Windows 10 Pro 22H2 | Windows Endpoint | `10.10.10.20` |
| Kali Linux | Kali Linux | Attacker / Security Testing | `10.10.10.30` |

### Virtualization

- Hypervisor: VirtualBox
- Internal Network: `soc-lab-net`
- Internal Subnet: `10.10.10.0/24`
- Default Gateway on internal network: None

---

## Windows Server VM

The Windows Server virtual machine was created as the future Active Directory Domain Controller.

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

---

## Windows Server Network Configuration

The Server VM uses two network adapters.

### Adapter 1 — NAT

The NAT adapter provides outbound Internet connectivity for the lab.

```text
IPv4 Address: 10.0.2.15
Subnet Mask: 255.255.255.0
Default Gateway: 10.0.2.2
```

Internet connectivity was verified with:

```text
ping 8.8.8.8
```

Result:

```text
4 packets sent
4 packets received
0% packet loss
```

### Adapter 2 — Internal Network

The second adapter connects the server to the isolated VirtualBox network:

```text
Network: soc-lab-net

IPv4 Address: 10.10.10.10
Subnet Mask: 255.255.255.0
Default Gateway: None
```

This interface is used for communication between the Domain Controller, Windows 10 endpoint, and Kali Linux machine.

---

## Windows 10 VM

A Windows 10 virtual machine was created as the lab endpoint.

### VM Configuration

| Setting | Configuration |
|---|---|
| VM Name | `SOC-LAB-WIN10` |
| OS | Windows 10 Pro 22H2 |
| RAM | 4096 MB |
| CPU | 2 vCPUs |
| Virtual Disk | 60 GB |
| Hypervisor | VirtualBox |

Internal network configuration:

```text
IPv4 Address: 10.10.10.20
Subnet Mask: 255.255.255.0
Default Gateway: None
DNS: 10.10.10.10
```

---

## Kali Linux VM

Kali Linux was configured as the attacker and security-testing machine.

Internal network configuration:

```text
IPv4 Address: 10.10.10.30
Subnet Mask: 255.255.255.0
Default Gateway: None
```

The Kali internal interface was connected to:

```text
soc-lab-net
```

---

## Three-Machine Network

The isolated SOC lab network was successfully configured using the VirtualBox Internal Network `soc-lab-net`.

```text
                         Internet
                            │
                           NAT
                            │
              ┌─────────────┼─────────────┐
              │             │             │
            Kali          DC          Windows 10
         10.0.2.15      10.0.2.15      10.0.2.15
              │             │             │
              └──────── soc-lab-net ─────┘
                       10.10.10.0/24
              │             │             │
        10.10.10.30   10.10.10.10   10.10.10.20
              │             │             │
           Attacker    Domain Controller   Endpoint
```

The internal network does not use a default gateway.

---

## Connectivity Verification

Connectivity between all three machines was tested.

### Windows 10 → Domain Controller

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

### Domain Controller → Windows 10

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

### Kali → Domain Controller

```text
4 packets transmitted
4 packets received
0% packet loss
Average RTT: 0.710 ms
```

### Kali → Windows 10

```text
4 packets transmitted
4 packets received
0% packet loss
Average RTT: 0.650 ms
```

### Result

All three lab machines successfully communicated over the isolated `soc-lab-net` network.

---

## Windows Firewall Troubleshooting

During initial network testing, Windows 10 could not ping the Windows Server VM while Windows Firewall was enabled.

### Problem

Initial test:

```text
Windows 10 → 10.10.10.10

Request timed out
100% packet loss
```

The communication worked when Windows Firewall was temporarily disabled. This confirmed that the VirtualBox networking and IP configuration were functioning correctly.

### Investigation

The Windows network profiles were checked using:

```powershell
Get-NetConnectionProfile
```

The isolated lab interface was identified as a Public/Unidentified network.

Because the network was classified as Public, Windows Firewall was blocking inbound ICMP traffic.

### Resolution

Instead of leaving Windows Firewall disabled, a narrowly scoped ICMPv4 rule was created.

Example rule on the Domain Controller:

```powershell
New-NetFirewallRule `
  -DisplayName "SOC-LAB Allow ICMPv4 Echo" `
  -Direction Inbound `
  -Protocol ICMPv4 `
  -IcmpType 8 `
  -LocalAddress 10.10.10.10 `
  -RemoteAddress 10.10.10.0/24 `
  -Action Allow `
  -Profile Any
```

A corresponding rule was created on Windows 10 using its internal address:

```text
10.10.10.20
```

### Final Verification

Windows Firewall remained enabled.

```text
Windows 10 → Windows Server
Sent = 4
Received = 4
Lost = 0
```

```text
Windows Server → Windows 10
Sent = 4
Received = 4
Lost = 0
```

### Result

The Windows Server and Windows 10 machines successfully communicated over the isolated lab network while Windows Firewall remained enabled.

This confirmed that the original connectivity issue was caused by Windows Firewall policy rather than VirtualBox networking or IP configuration.

---

## Evidence

Screenshots from this phase are stored under:

```text
../screenshots/01-lab-setup/
```

Relevant evidence includes:

- Server network configuration
- Windows Server installation
- Windows 10 network configuration

---

## Status

- [x] Windows Server VM created
- [x] Windows Server 2022 installed
- [x] Renamed to `SOC-LAB-DC`
- [x] NAT connectivity verified
- [x] Internal network configured
- [x] Windows 10 VM created
- [x] Windows 10 installed
- [x] Renamed to `SOC-LAB-WIN10`
- [x] Kali Linux VM connected
- [x] `soc-lab-net` configured
- [x] Windows Server ↔ Windows 10 connectivity verified
- [x] Kali → Windows Server connectivity verified
- [x] Kali → Windows 10 connectivity verified
- [x] Windows Firewall remained enabled
- [x] Scoped ICMP firewall rules configured
- [x] Three-machine network verified