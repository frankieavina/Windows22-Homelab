# IT Help Desk Home Lab — Windows Server 2022 on Unraid

## Overview
This repository documents a hands-on IT help desk home lab built from scratch using Unraid as the virtualization host. The lab simulates a real enterprise IT environment including Active Directory, Group Policy, file sharing, patch management, ticketing, and remote support — all running on virtualized infrastructure.

The goal of this project is to demonstrate practical, job-ready IT help desk skills by building, configuring, and troubleshooting a fully functional domain environment.

---

## Lab Environment

| Machine | Role | OS |
|---|---|---|
| Unraid Server | Virtualization Host | Unraid |
| Ceres-DC-01 | Domain Controller / File Server | Windows Server 2022 |
| frankie-vm1 | Domain-joined Workstation | Windows 11 |

- **Domain:** example.com
- **Network Bridge:** br0 (both VMs on the same subnet)
- **Virtualization:** KVM/QEMU via Unraid

---

## Skills Demonstrated

- Deploying and configuring Windows Server 2022 in a virtualized environment
- Installing and managing Active Directory Domain Services (AD DS)
- Building enterprise-style OU, Security Group, and User structures
- Configuring and enforcing Group Policy (password policy, account lockout)
- Creating and managing SMB shared folders with NTFS permissions
- Patch management and endpoint reporting using Action1 RMM
- Help desk ticketing and documentation using Jira
- Remote desktop support using RDP
- DNS troubleshooting and DHCP configuration
- PowerShell scripting for AD administration
- Mapping network drives and managing user access

---

## Technologies Used

| Tool | Purpose |
|---|---|
| Unraid | Virtualization host (KVM/QEMU) |
| Windows Server 2022 | Domain Controller, DNS, File Server |
| Windows 11 | Domain-joined workstation |
| Active Directory Domain Services | User, group, and policy management |
| Group Policy Management Console | Password and lockout policy enforcement |
| Action1 RMM | Patch management and endpoint reporting |
| Jira | Help desk ticketing and documentation |
| PowerShell | Scripting and AD administration |
| Remote Desktop Protocol (RDP) | Remote machine access and support |
| SMB / NTFS | File sharing and permission management |

---

## Project Structure

| Part | Topic |
|---|---|
| [Part 1 – Infrastructure & Windows Server Deployment](docs/part-1-infrastructure-and-windows-server-deployment.md) | Setting up Unraid, creating VMs, installing Windows Server 2022 |
| [Part 2 – Active Directory Domain Services Deployment](docs/part-2-active-directory.md) | Installing AD DS, promoting server to Domain Controller, configuring DNS |
| [Part 3 – Connecting VMs and Adding Users](docs/part-3-configuring-and-Adding-Users.md) | Networking both VMs via br0, joining Windows 11 to the domain, creating AD users |
| [Part 4 – Group Policy](docs/part-4-group-policy-password-and-lockout.md) | Password policy, account lockout threshold, enforcing GPO, verifying with gpresult |
| [Part 5 – Patch Management and Reporting](docs/part-5-patch-mngmt-reporting.md) | Action1 RMM setup, deploying missing updates, generating audit reports |
| [Part 6 – Network Shares and Security Groups](docs/part-6-shared-files-security-groups.md) | Creating SMB shares, NTFS permissions, security groups, mapping network drives |
| [Part 7 – Enterprise Active Directory with OUs](docs/part-7-Enterprise-AD.md) | Building OU structure, department security groups, bulk user creation, department shares |
| [Part 8 – Help Desk Ticketing with Jira](docs/part-8-Help-Desk-Tickets.md) | Jira Kanban board setup, 5 real-world tickets created and resolved |
| [Part 9A – Remote Desktop Protocol (RDP)](docs/part-9a-rdp.md) | Enabling RDP, connecting remotely from Server to Windows 11, troubleshooting |
| Part 9B – DNS & DHCP *(coming soon)* | DNS troubleshooting, DHCP server setup and IP management |
| Part 9C – PowerShell Scripting *(coming soon)* | Automating AD tasks, bulk user management, password resets via script |
| Part 9D – Azure AD *(coming soon)* | Connecting on-premise AD to Azure, hybrid identity management |

---

## Key Scenarios Practiced

These are real-world help desk scenarios worked through in this lab:

| Scenario | Skill |
|---|---|
| User account locked out | Unlock AD account, verify Group Policy lockout settings |
| New employee onboarding | Create user, assign to OU and Security Group, provision share access |
| Forgot password | Reset AD password with forced change at next logon |
| Employee offboarding | Disable account, remove from security groups, revoke access |
| Network drive not reconnecting | Remap with correct credentials and reconnect at sign-in |
| Remote troubleshooting | RDP into user machine and resolve issue remotely |

---

## Purpose

This lab was built to develop and demonstrate practical IT help desk skills in a safe, isolated environment. Every configuration, ticket, and troubleshooting step documented here reflects real tasks performed by IT support professionals in enterprise environments.


  
