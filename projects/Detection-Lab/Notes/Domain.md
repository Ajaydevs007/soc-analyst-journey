Domain Controller (DC)

A Domain Controller (DC) is a Windows Server that has been promoted after installing AD DS.

Example:

Server: DC01
Domain: SOC-LAB.LOCAL

The Domain Controller provides services such as:

User authentication
Computer authentication
User and group management
Domain policies
Active Directory services
Important

Installing AD DS does not automatically make the server a Domain Controller.

The process is: 

Windows Server
      ↓
Install AD DS
      ↓
Promote Server to Domain Controller
      ↓
DC01
      ↓
SOC-LAB.LOCAL

DC = The server that provides Active Directory domain services for the domain.


Domain Login

When a user logs into a domain-joined Windows computer:

User
  ↓
Enters username + password
  ↓
Windows Workstation
  ↓
Finds Domain Controller
  ↓
DC01
  ↓
AD DS provides authentication services
  ↓
User information is checked against Active Directory
  ↓
Authentication result
  ↓
Login allowed / denied