# Windows Server & Active Directory Labs

Hands-on labs covering the deployment and administration of a Windows Server 2025 domain environment. The labs begin with Windows Server deployment and baseline configuration, then progress through Active Directory Domain Services, DNS, user and computer management, DHCP, Group Policy, and domain-based file services.

| Lab | Description | 
|---|---|
| [Windows Server Deployment & Baseline Configuration](01-server-deployment-baseline/) | Deployed a Windows Server 2025 VM, configured the virtual network and server baseline, installed updates, verified connectivity and Remote Desktop access, and created a pre-AD DS snapshot |
| [AD DS & DNS Deployment](02-ad-ds-dns-deployment/) | Installed Active Directory Domain Services and DNS, created a new forest, promoted `DC01` to a domain controller, and verified the domain and DNS configuration |
| [Organizational Units, Users & Security Groups](03-ous-users-security-groups/) | Created an organizational unit structure, domain user accounts, and security groups for centralized identity and access management |
| [Windows 11 Domain Join & Client Verification](04-windows-11-domain-join/) | Joined a Windows 11 client to the domain, verified domain authentication and DNS resolution, and confirmed the computer account in Active Directory |
| [DHCP Server Deployment & Client Configuration](05-dhcp-server-deployment/) | Installed and authorized DHCP, configured a scope and network options, and verified lease assignment on the domain-joined Windows 11 client |
| [Group Policy Administration](06-group-policy-administration/) | Created and linked Group Policy Objects, configured user and computer settings, and verified policy application on the Windows 11 client |
| [File Services, Permissions & Recovery](07-file-services-permissions-recovery/) | Created SMB shares, assigned share and NTFS permissions through Active Directory security groups, tested user access, and backed up and restored server data |

[← Back to IT-Support](../README.md)