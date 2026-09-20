# Member of: WORKGROUP

## What does "Member of" mean?

**Member of** tells us which **group or domain the computer belongs to**.

If Windows shows:

```text
Member of: WORKGROUP
```

it means:

> The computer is **not joined to a Windows domain**.

`WORKGROUP` is the default/simple setup for a computer that is not domain-joined.

## Important

* `WORKGROUP` does **not** mean Administrator.
* It does **not** contain users for giving special privileges.
* It does **not** provide special permissions.
* It simply indicates that the computer is **not part of a domain**.

## Domain-Joined Computer

If the computer is joined to an Active Directory domain:

```text
Member of: SOC-LAB.LOCAL
```

instead of:

```text
Member of: WORKGROUP
```

## Easy Way to Remember

```text
No Domain
    ↓
WORKGROUP

Joined to Domain
    ↓
Domain Name
```

### Key Point

> **`Member of: WORKGROUP` = This computer is not currently joined to a Windows domain.**
