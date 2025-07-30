<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Active Directory Logo" width="200"/>
</p>

# Azure-Based Active Directory Lab

This repository walks through building a complete Active Directory environment inside Microsoft Azure using virtual machines. The guide covers deploying a Domain Controller, configuring domain services, joining a client machine to the domain, and managing users in a cloud-hosted lab that mirrors traditional on-premises setups.

---

##  Technologies and Services

- **Microsoft Azure** – Resource Groups, Virtual Networks, Virtual Machines  
- **Active Directory Domain Services (AD DS)**  
- **Remote Desktop Protocol (RDP)**  
- **Windows PowerShell**  

---

## Operating Systems

- **Windows Server 2022** (Domain Controller)  
- **Windows 10 21H2** (Client Workstation)  

---

## Lab Deployment Steps

### Step 1: Configure and Promote the Domain Controller (DC-1)
- **Access DC-1 via RDP**
  - Log in with the admin credentials created during VM deployment.
- **Install AD DS Role**
  - Open *Server Manager* → Add Roles → Active Directory Domain Services.
- **Promote to Domain Controller**
  - Create a new forest (e.g. `corp.local`).
  - Complete configuration and restart the server.
- **Log in with Domain Account**
  - Sign back into DC-1 using the new domain administrator context.

📸 *[Add screenshot of AD DS installation wizard]*

---

### Step 2: Create Admin Accounts and Organizational Units
- **Open ADUC (Active Directory Users and Computers)**
- **Organize Structure**
  - Create Organizational Units such as:
    - `_USERS`
    - `_ADMINS`
- **Add a Domain Admin**
  - Create a user (e.g. `admin.jane`) and assign to the *Domain Admins* security group.
- **Use New Admin**
  - Sign out and back into DC-1 using `corp.local\admin.jane`.



---

### Step 3: Deploy and Join Client-1 to the Domain
- **Provision Windows 10 VM**
  - Place the client in the same Virtual Network and subnet as DC-1.
- **Point DNS to DC-1**
  - Set Client-1's NIC DNS server to the static IP of DC-1.
- **Join to Domain**
  - Access *System Properties* → Change Domain → Enter `corp.local`.
  - Restart Client-1 to finalize the domain join.
- **Verify in ADUC**
  - Confirm Client-1 appears under *Computers* or move it into a `_CLIENTS` OU.



---

### Step 4: Enable Remote Access for Domain Users on Client-1
- **Sign in as a Domain Admin**
- **Modify Remote Desktop Settings**
  - Allow the `Domain Users` group to access RDP.
- **Test Access**
  - Sign into Client-1 with a standard user account to confirm permissions.


---

### Step 5: Automate User Creation with PowerShell
- **Open PowerShell ISE on DC-1**
- **Run a User Creation Script**
  - Use a script to bulk-create test accounts inside `_USERS`.
- **Validate Accounts**
  - Check ADUC to verify users were generated correctly.
- **Test Logins**
  - Attempt to log into Client-1 with one of the new domain accounts.




---

## Objective

This project replicates a full Active Directory domain environment inside Azure for training, testing, or proof-of-concept deployments. By following these steps, you create a scalable cloud-based lab that mirrors real-world enterprise domain structures and authentication flows.

---
