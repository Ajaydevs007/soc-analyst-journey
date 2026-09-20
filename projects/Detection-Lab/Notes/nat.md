# NAT — VirtualBox

## What is NAT?

**NAT = Network Address Translation**

NAT allows a **VM to access the Internet through the host computer**.

```text
Internet
    ↓
Host PC
    ↓
VirtualBox NAT
    ↓
VM
```

## Configuration

In VirtualBox:

```text
Attached to: NAT
```

Normally, you **do not manually configure the IP range**.

VirtualBox automatically provides network configuration to the VM.

Example:

```text
VM IP: 10.0.2.15
```

## Purpose

NAT is mainly used when the VM needs:

* Internet access
* Windows updates
* Downloading tools
* Installing packages

## Easy Way to Remember

> **NAT → Internet access for the VM through the host.**
