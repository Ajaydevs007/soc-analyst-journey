# Internal Network — VirtualBox

## What is an Internal Network?

An **Internal Network** creates a **private network between VMs**.

Example:

```text
Kali VM
10.10.10.20
     │
     │  SOC-Lab
     │
Windows VM
10.10.10.10
```

Both VMs are connected to the same Internal Network.

## Configuration

In VirtualBox:

```text
Attached to: Internal Network
Name: SOC-Lab
```

Use the **same Internal Network name** on the VMs that need to communicate.

Example:

```text
Kali VM     → SOC-Lab
Windows VM  → SOC-Lab
```

## IP Address

Unlike normal NAT, you commonly configure the IP addresses **inside the VMs**.

Example:

### Kali

```text
IP: 10.10.10.20
Mask: 255.255.255.0
```

### Windows

```text
IP: 10.10.10.10
Mask: 255.255.255.0
```

Now they can communicate privately.

## Purpose

Internal Network is useful for:

* Cybersecurity labs
* Attack/defense simulations
* Kali → Windows testing
* AD labs
* SOC labs
* Isolated VM-to-VM communication

## Easy Way to Remember

> **Internal Network → Private communication between VMs.**

### NAT vs Internal Network

```text
NAT
 ↓
Internet access

Internal Network
 ↓
Private VM-to-VM communication
```
