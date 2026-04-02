# Part 9B - DNS Troubleshooting and DHCP Setup

## Overview
In this section, we cover two fundamental networking skills for IT help desk professionals — DNS troubleshooting and DHCP configuration. We run common DNS diagnostic commands from the Windows 11 workstation and set up a DHCP server on Windows Server 2022 to automatically assign IP addresses to devices on the network.

---

## Environment
| Machine | Role | IP Address |
|---|---|---|
| Windows Server 2022 VM (Ceres-DC-01) | Domain Controller / DNS / DHCP Server | 192.168.1.10 |
| Windows 11 VM (frankie-vm1) | Domain-joined Workstation / DHCP Client | 192.168.1.100–200 (DHCP) |
| Domain | example.com | — |

---

## Part 1 — DNS Troubleshooting

### What is DNS?
DNS (Domain Name System) translates human-readable names into IP addresses. For example:
```
frankie-vm1     → 192.168.1.x
example.com     → 192.168.1.10
Ceres-DC-01     → 192.168.1.10
```
When DNS breaks, users cannot reach shared folders, websites, or domain resources by name — even if the network itself is working fine. DNS troubleshooting is one of the most common Level 1 help desk tasks.

---

### Running DNS Commands

> 💡 **Important:** Always open CMD by right-clicking → **"Run as Administrator"**. Some commands require elevation and will return an error otherwise.

#### Command 1 — Check Current DNS Settings
```cmd
ipconfig /all
```
Shows all network adapter details including the DNS server currently assigned. Confirm the DNS server shows your domain controller IP.

---

#### Command 2 — Test DNS Resolution
```cmd
nslookup example.com
```
Tests whether DNS can resolve your domain name to the correct IP address. Should return `192.168.1.10`.

---

#### Command 3 — Test Reverse DNS Lookup
```cmd
nslookup 192.168.1.10
```
Tests whether DNS can resolve an IP address back to a hostname. Should return `Ceres-DC-01.example.com`.

---

#### Command 4 — Flush DNS Cache
```cmd
ipconfig /flushdns
```
Clears all locally cached DNS entries. Used when a user can reach something by IP but not by name, or when DNS records have recently changed and old entries are being cached.

Expected output:
```
Successfully flushed the DNS Resolver Cache
```

---

#### Command 5 — Re-register DNS
```cmd
ipconfig /registerdns
```
Forces the machine to re-register itself with the DNS server. Useful when a machine is not showing up in Active Directory or cannot be found by name on the network.

> 💡 **Note:** This command requires elevation. Always run CMD as Administrator or you will receive "The requested operation requires elevation" error.

---

#### Command 6 — Check Specific Host Resolution
```cmd
nslookup Ceres-DC-01
```
Tests whether a specific hostname resolves correctly. Should return `192.168.1.10`.

---

### DNS Troubleshooting Quick Reference

| Command | When to Use |
|---|---|
| `ipconfig /all` | Check what DNS server the machine is using |
| `nslookup <name>` | Test if a hostname resolves to the correct IP |
| `nslookup <ip>` | Test reverse lookup from IP to hostname |
| `ipconfig /flushdns` | Clear stale DNS cache entries |
| `ipconfig /registerdns` | Re-register machine with DNS server |
| `ipconfig /renew` | Request a new IP from DHCP server |

---

## Part 2 — DHCP Server Setup

### What is DHCP?
DHCP (Dynamic Host Configuration Protocol) automatically assigns IP addresses to devices when they join the network. Without DHCP, every device would need a manually configured static IP address — not practical in an enterprise with hundreds of devices.

When a device joins the network:
```
Device joins → contacts DHCP server → gets assigned:
  - IP Address (e.g. 192.168.1.105)
  - Subnet Mask (255.255.255.0)
  - Default Gateway (192.168.1.1)
  - DNS Server (192.168.1.10)
```

---

### Step 1 — Install the DHCP Role via GUI

1. On **Ceres-DC-01** open **Server Manager**
2. Click **"Add Roles and Features"**
3. Click **Next** until you reach **Server Roles**
4. Check **"DHCP Server"** → click **Add Features**
5. Click **Next** → **Next** → **Install**
6. Wait for installation to complete
7. Click **"Complete DHCP Configuration"** in the notification flag at the top of Server Manager
8. Click **Next** → **Commit** → **Close**

---

### Step 2 — Authorize DHCP Server in Active Directory

DHCP servers must be authorized in Active Directory before they can assign IP addresses. This prevents unauthorized DHCP servers from disrupting the network.

Open **PowerShell as Administrator** on Ceres-DC-01:

```powershell
Add-DhcpServerInDC -DnsName "Ceres-DC-01.example.com" -IPAddress 192.168.1.10
```

Verify authorization:
```powershell
Get-DhcpServerInDC
```

Should return your server name and IP address ✅

> 💡 **Note:** You may see a warning about RPC server unavailability after authorization in a single DC lab environment. This is a cosmetic warning and does not affect DHCP functionality as long as the authorization command confirms success.

---

### Step 3 — Create a DHCP Scope via GUI

A scope defines the range of IP addresses DHCP will hand out to devices.

1. Open **Server Manager → Tools → DHCP**
2. Expand **Ceres-DC-01 → IPv4**
3. Right-click **IPv4** → **New Scope**
4. The **New Scope Wizard** opens → click **Next**

#### Scope Name
| Field | Value |
|---|---|
| Name | Lab Network |
| Description | DHCP Scope for Lab Environment |

#### IP Address Range
| Field | Value |
|---|---|
| Start IP Address | 192.168.1.100 |
| End IP Address | 192.168.1.200 |
| Subnet Mask | 255.255.255.0 |

#### Add Exclusions
Exclude static IPs already in use so DHCP never assigns them to another device:

| Field | Value |
|---|---|
| Start IP | 192.168.1.1 |
| End IP | 192.168.1.99 |

Click **Add** → **Next**

#### Lease Duration
Leave as **8 days** for lab use → click **Next**

#### Configure DHCP Options
Select **"Yes I want to configure these options now"** → click **Next**

#### Default Gateway
| Field | Value |
|---|---|
| Router | 192.168.1.1 |

Click **Add** → **Next**

#### DNS Server
| Field | Value |
|---|---|
| Parent Domain | example.com |
| IP Address | 192.168.1.10 |

Click **Add** → **Next**

#### WINS Server
Leave blank → click **Next**

#### Activate Scope
Select **"Yes I want to activate this scope now"** → click **Next** → **Finish**
<img width="1023" height="657" alt="Screenshot 2026-04-01 at 5 02 05 PM" src="https://github.com/user-attachments/assets/8b11c26d-602e-4a45-a94a-f079b71f6f46" />

---

### Step 4 — Verify the Scope in DHCP Manager

After completing the wizard your DHCP Manager should look like this:

```
Ceres-DC-01
└── IPv4
      └── Scope [192.168.1.0] Lab Network  ✅
            ├── Address Pool      → 192.168.1.100 - 192.168.1.200
            ├── Address Leases    → devices that received an IP
            ├── Reservations      → static assignments by MAC address
            └── Scope Options     → gateway, DNS, domain name
```

---

### Step 5 — Test DHCP on Windows 11

1. On **Windows 11** go to **Network Adapter Settings**
2. Set IPv4 to **"Obtain an IP address automatically"**
3. Open **PowerShell as Administrator** and run:

```powershell
# Release current IP
ipconfig /release

# Request new IP from DHCP
ipconfig /renew

# Verify assigned IP
ipconfig /all
```

The assigned IP should fall between `192.168.1.100` and `192.168.1.200` ✅

> 💡 **Note:** A restart may be required for the machine to pick up the new DHCP lease. This is normal behavior when a new DHCP server is deployed on an existing network.

---

### Checking DHCP Leases

To see all devices that have been assigned an IP by your DHCP server:

1. Open **DHCP Manager** on Ceres-DC-01
2. Expand **Scope → Address Leases**
3. You will see each device listed with:
   - IP address assigned
   - Device hostname
   - Lease expiration date/time

> 💡 **Interview Tip:** If an interviewer asks how you would find what IP address a specific computer was assigned — go to **DHCP Manager → Address Leases**. This is the fastest way to look up any device's current IP in an enterprise environment.

---

## GUI vs PowerShell

Both methods accomplish the same result. As a help desk professional it is valuable to know both:

| Task | GUI Method | PowerShell Method |
|---|---|---|
| Install DHCP role | Server Manager → Add Roles | `Install-WindowsFeature -Name DHCP` |
| Authorize DHCP | DHCP Manager → Authorize | `Add-DhcpServerInDC` |
| Create scope | New Scope Wizard | `Add-DhcpServerv4Scope` |
| Set exclusions | Scope Wizard → Exclusions | `Add-DhcpServerv4ExclusionRange` |
| Set DNS/Gateway | Scope Wizard → Options | `Set-DhcpServerv4OptionValue` |

> 💡 **Note:** At Level 1 help desk, GUI knowledge is expected and sufficient. PowerShell becomes more important at Level 2 and Sysadmin levels where automation and scripting are required.

---

## Summary

By the end of this section you should have:

- ✅ Run all DNS troubleshooting commands from Windows 11 as Administrator
- ✅ Flushed DNS cache and re-registered the machine with the DNS server
- ✅ Installed the DHCP Server role via Server Manager GUI
- ✅ Authorized the DHCP server in Active Directory
- ✅ Created a DHCP scope (192.168.1.100 — 192.168.1.200) via the New Scope Wizard
- ✅ Set exclusion range to protect static IPs
- ✅ Configured scope options (gateway, DNS, domain)
- ✅ Verified Windows 11 received an IP address from the DHCP scope

---

## What's Next — Part 9C

In the next part we will cover:
- PowerShell scripting for common AD administration tasks
- Automating password resets, user creation, and group management
- Writing reusable scripts for help desk scenarios
