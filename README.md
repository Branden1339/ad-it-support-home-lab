Active Directory and IT Support Home Lab

A simulated small company network built in VMware Workstation to practice the day to day work of an IT support and help desk technician: managing users and permissions in Active Directory, enforcing Group Policy, and handling tickets in a service desk system.

Status: In progress. Last updated November 2026.

Goals
Build and manage a Windows domain from scratch
Practice identity and access management: users, groups, least privilege
Troubleshoot like a service desk: isolate the problem layer by layer
Run a ticketing system and document each issue from report to resolution
Architecture
Machine	Role	OS	Notes
LAB-DC01	Domain controller, DNS	Windows Server 2022 Standard Evaluation	Domain lab.local, static IP 192.168.89.10
LAB-CLIENT01	Employee workstation	Windows 11 Pro	Joined to the domain
LAB-GLPI01	Ticketing system	Ubuntu Server 26.04 LTS	GLPI and MariaDB in Docker

All three machines sit on one private VMware NAT network (192.168.89.0/24).

What is built

Active Directory and DNS

Domain lab.local with DNS and a forwarder for internet name resolution
Organizational units: Sales, HR, IT
Five user accounts split across those OUs
Security groups: SalesTeam, HRTeam, ITTeam

File shares and permissions

SMB shares for Sales, HR, and IT
NTFS permissions set with icacls: inheritance removed, each group gets Modify on its own folder only, Administrators and SYSTEM keep Full Control
Tested: a Sales user can open the Sales share and is denied the HR share

Group Policy

Policy restricting access to Control Panel and PC settings, linked to the Sales OU
Account lockout policy with a threshold of 3 failed attempts, tested by locking and unlocking an account

Ticketing (GLPI)

GLPI and MariaDB running as Docker containers on a separate Linux VM
Default accounts removed and the administrator password changed
Technician account and employee accounts that match the Active Directory users
Ticket categories: Account and Password, Access and Permissions, Network and DNS, Hardware, Software, Onboarding and Offboarding
Troubleshooting method
Confirm the symptom and what changed
Check the account: enabled, locked out
Check group membership
Check share and NTFS permissions
For network problems, test in layers: gateway, then an internet IP, then name lookup
Fix, verify, and document in the ticket
Tickets

See docs/ticket-log.md for every ticket worked, with cause, fix, and verification.

Some incidents were recreated on purpose so the full workflow could be practiced. Those are labeled simulated in the log.

Tools and skills practiced

VMware Workstation, Windows Server 2022, Active Directory Domain Services, DNS, Group Policy, SMB and NTFS permissions, PowerShell, Ubuntu Server, SSH, Docker and Docker Compose, GLPI, MariaDB, ticket triage and documentation.

Screenshots

Stored in screenshots/. No passwords or tokens are included anywhere in this repository.

Next steps
Password reset, onboarding, and offboarding tickets
Static IP for the GLPI server
Optional: import Active Directory users into GLPI, WSUS patching, BitLocker
