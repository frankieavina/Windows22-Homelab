# Part 3 - Join Machine to Domain and Creating Active Directory Users

## Overview
In this section, we configure two VMs running side by side in Unraid to communicate with each other over the same network bridge. We then join the Windows 11 VM to the Active Directory domain and create domain user accounts on Windows Server 2022.

---

## Environment
| Machine | Role | IP Address |
|---|---|---|
| Windows Server 2022 VM | Domain Controller / AD DS / DNS | 10.0.0.10 |
| Windows 11 VM | Domain-joined Workstation | 10.0.0.x |
| Domain | example.com | — |
| Unraid Network Bridge | br0 (default) | — |

---

## Part 1 — Connecting Both VMs in Unraid

### Why This Works
Both VMs are connected to the same **br0 network bridge** in Unraid, which acts like a virtual network switch. This puts both VMs on the same subnet as the physical LAN, allowing them to communicate with each other just like two physical machines on the same network.

### Step 1 — Verify Both VMs Are on br0

1. In the **Unraid WebGUI**, go to the **VMs tab**
2. Click each VM → **Edit**
3. Confirm **Network Bridge** is set to `br0` for both VMs
4. Confirm **Network Model** is set to `virtio-net` or `e1000`

### Step 2 — Assign a Static IP to Windows Server 2022

A static IP is required so the Windows 11 VM always knows where to find the Domain Controller and DNS server.

1. On **Windows Server 2022**, open **Network & Internet Settings → Change adapter options**
2. Right-click the NIC → **Properties → IPv4** → enter the following:

```
IP Address:      10.0.0.10
Subnet Mask:     255.255.255.0
Default Gateway: 10.0.0.1
DNS Server:      127.0.0.1
```

> 💡 The DNS server is set to `127.0.0.1` (localhost) because the Server itself runs the DNS role for the domain.

### Step 3 — Point Windows 11 DNS to the Server

For Windows 11 to find the domain, its DNS must point to the Windows Server VM.

On **Windows 11**, open **PowerShell as Administrator** and run:

```powershell
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.10
```

Verify DNS is resolving correctly:

```powershell
nslookup example.com
```

Expected result: returns `10.0.0.10` ✅

### Step 4 — Test Connectivity Between VMs

On **Windows 11**, open CMD and run:

```cmd
ping 10.0.0.10
```

On **Windows Server**, open CMD and run:

```cmd
ping 10.0.0.x
```

Both should return successful replies. If ping fails, enable the firewall rule on Windows Server:

```powershell
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=Yes
```

### Step 5 — Join Windows 11 to the Domain

On **Windows 11**, open **PowerShell as Administrator** and run:

```powershell
Add-Computer -DomainName "example.com" -Credential "example\Administrator" -Restart
```

Enter the domain admin password when prompted. The machine will automatically restart.

**or**

1. Right-click **`This PC`** → Scroll down to **Domain or Workgroups** → Click **Change**
2. Fill in the domain section: **example.com**
3. Press **Okay**
4. Fill in the domain information:

| Field | Example |
|---|---|
| Username | Administrator |
| Password | 123 |

### Step 6 — Verify Domain Join

After reboot, open CMD on **Windows 11** and run:

```cmd
systeminfo | findstr /i "domain"
```

Expected result:

```
Domain: example.com
```

To log in with a domain account, click **Other User** on the login screen and enter:

```
Username: example\Administrator
Password: <your domain admin password>
```

**or** 

1. Click **`Tools`** on Server Manger → Scroll down to **Active Directory Users and Computers** → Click **Change**
2. Click **example.com** dropdown → Click **Computers**
3. You should see Desktop joined
<img width="1281" height="942" alt="Screenshot 2026-03-10 at 4 41 24 PM" src="https://github.com/user-attachments/assets/aa194189-f6c6-466c-816a-cba46239370d" />

---

## Part 2 — Creating Active Directory Users

### Step 1 — Open Active Directory Users and Computers

1. On **Windows Server 2022**, open **Server Manager**
2. Click **Tools** → **Active Directory Users and Computers**
3. In the left panel, expand **`example.com`**

### Step 2 — Create a New User

1. Right-click **`Users`** → **New** → **User**
2. Fill in the user information:

| Field | Example |
|---|---|
| First Name | John |
| Last Name | Smith |
| User Logon Name | jsmith |

3. Click **Next**
4. Set a password and choose password options:
   - ✅ **Password never expires** (recommended for lab use)
5. Click **Next** → **Finish**

> 💡 **Note:** In a production environment you would enforce password expiration and complexity policies. For this lab, disabling expiration keeps the focus on learning AD structure and administration.

### Step 3 — Verify the User Was Created

To confirm the user exists, run this in **PowerShell** on Windows Server:

```powershell
Get-ADUser -Filter * | Select Name, SamAccountName
```

<img width="1273" height="946" alt="Screenshot 2026-03-09 at 8 50 31 PM" src="https://github.com/user-attachments/assets/0810f945-f6a2-4257-b463-065988969b73" />


---

## Summary

By the end of this section you should have:

- ✅ Verified both VMs are connected to the `br0` network bridge in Unraid
- ✅ Assigned a static IP to Windows Server 2022 (`10.0.0.10`)
- ✅ Pointed Windows 11 DNS to the domain controller
- ✅ Confirmed network connectivity between both VMs via ping
- ✅ Successfully joined the Windows 11 VM to `example.com`
- ✅ Created domain user accounts in Active Directory Users and Computers

---

## What's Next — Part 4

In the next part we will:
- Create shared folders on Windows Server 2022
- Assign folder permissions to AD users and groups
- Map network drives on Windows 11 so users connect to their shares automatically at login
