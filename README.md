<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10

<h2>High-Level Deployment and Configuration Steps</h2>

- Configure Domain Controller in Azure
- Configure Client VM in Azure 
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/fnssWjP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
🌐 Step 1: Create the Domain Controller (DC-1)

### ✅ Resource Setup

1. **Create a Resource Group** in your desired Azure region.
2. **Create a Virtual Network** and **Subnet** within that Resource Group.
3. **Create a Virtual Machine**:
   - **Name**: `DC-1`
   - **OS**: Windows Server 2022
   - **Username**: `YourChoice`
   - **Password**: `YourChoice`


</p>
<br />

<p>
<img src="https://i.imgur.com/GX68T5u.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
🔧 Post-Deployment Configuration

1. **Set the Private IP to Static**:
   - Go to `DC-1` → Networking → Network Interface → IP Configuration.
   - Set the private IP address to **Static**.

2. **Log into DC-1 via RDP**
3. **Disable the Windows Firewall** *(for testing only)*:
   - Open Windows Defender Firewall.
   - Click **Turn Windows Defender Firewall on or off**.
   - Disable both Private and Public network firewalls.

---
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
