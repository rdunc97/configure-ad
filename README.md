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
- Make sure Domain Controller's NIC IP is set to static, and note the IP
- Set the Client VM's DNS settings to the Domain Controller's static IP address
- Verify using Powershell in the Client VM that the DNS settings are correct

---

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
<img src="https://i.imgur.com/GX68T5u.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
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
<img src="https://i.imgur.com/7XQaEBN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
🖥️ Step 2: Set Up the Client VM (Client-1)

1. **Create a new Virtual Machine**:
   - **Name**: `Client-1`
   - **OS**: Windows 10
   - **Username**: `yourchoice`
   - **Password**: `yourchoice`
   - **Region & VNet**: Use the **same region** and **Virtual Network** as `DC-1`.

2. **Update DNS Settings**:
   - Go to `Client-1` → Networking → Network Interface → DNS Servers.
   - Set **DNS to custom** and input **DC-1’s Private IP Address**. In this case above in the picture it was 10.0.0.4

BEFORE DNS CHANGE
<p>
<img src="https://i.imgur.com/d2MjRg0.png" height="45%" width="45%" alt="Disk Sanitization Steps"/>
</p>

AFTER DNS CHANGE
<p>
<img src="https://i.imgur.com/vp6cywx.png" height="45%" width="45%" alt="Disk Sanitization Steps"/>
</p>

3. **Restart Client-1** from the Azure Portal to apply DNS changes.

---

</p>
<br />


<p>
🧪 Step 3: Test the Connection

1. **Login to Client-1 via RDP**
2. **Test Network Connectivity**:
   - Open **Command Prompt**.
   - Run:
     ```bash
     ping <DC-1-Private-IP>
     ```
   - Ensure you get replies.
<p>
<img src="https://i.imgur.com/UFrOppc.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>

3. **Verify DNS Settings**:
   - Open **PowerShell** and run:
     ```powershell
     ipconfig /all
     ```
   - Confirm that the DNS server listed matches **DC-1’s Private IP address**.

<p>
<img src="https://i.imgur.com/CdPWRVx.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>

---

</p>
<br />



<h1> 🏢 Active Directory Deployment Part 2: Domain Setup and Client Join </h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.

---
<br />



<p>
🛠️ Step 1: Install Active Directory on DC-1

1. Log into your **DC-1** VM.
2. Open **Server Manager**.
3. Click **Manage > Add Roles and Features**.
4. Install **Active Directory Domain Services (AD DS)**.
5. After installation, click the yellow warning icon in Server Manager to **Promote this server to a domain controller**.
6. Choose **"Add a new forest"** and enter:
   ```
   mydomain.com
   ```
   *(You can use any name, just remember it)*

7. Complete the wizard and let the server **restart**.
8. After reboot, log in using:
   ```
   mydomain.com\labuser
   ```

---
</p>

<br />



<p>
👤 Step 2: Create a Domain Admin User

1. Open **Active Directory Users and Computers (ADUC)**.
2. Right-click your domain name → **New > Organizational Unit (OU)**:
   - Name: `_EMPLOYEES`
3. Create another OU:
   - Name: `_ADMINS`

4. Inside `_ADMINS`, right-click → **New > User**:
   - Full Name: `Jane Doe`
   - Username: `jane_admin`
   - Password: `Cyberlab123!`

5. After creating the user:
   - Right-click `jane_admin` → **Add to a group**
   - Type: `Domain Admins` → Click OK

6. Log out of `labuser` and log in using:
   ```
   mydomain.com\jane_admin
   ```

> ✅ Use `jane_admin` for all admin tasks going forward.
<p> <img src="https://i.imgur.com/ZSnFNdN.png" height="45%" width="45%" alt="Disk Sanitization Steps"/> </p>

---
</p>

<br />



<p>
🖥️ Step 3: Join Client-1 to the Domain

1. Log into **Client-1** using:
   ```
   labuser / Cyberlab123!
   ```
2. Open **System Settings > About > Rename this PC (advanced)**.
3. Click **Change** and set:
   - **Member of Domain**: `mydomain.com`
4. Provide credentials when prompted:
   - Username: `mydomain.com\jane_admin`
   - Password: `Cyberlab123!`

5. After restart, the computer will be domain-joined.
<p> <img src="https://i.imgur.com/dUXTOLx.png" height="45%" width="45%" alt="Disk Sanitization Steps"/> </p>

---
</p>

<br />



<p>
✅ Step 4: Verify and Organize in ADUC

1. Back on **DC-1**, open **ADUC** again.
2. Verify that **Client-1** appears under **Computers**.
3. Create a new OU:
   - Name: `_CLIENTS`
4. Drag `Client-1` into the `_CLIENTS` OU.
<p> <img src="https://i.imgur.com/H7bgCzJ.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> </p>

---
</p>

<br />


