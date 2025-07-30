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

<img width="830" alt="Screenshot 2025-01-23 at 3 39 35 PM" src="https://github.com/user-attachments/assets/4e576c4c-b6dd-4aa6-bf85-6070571682bf" />
<img width="1223" alt="Screenshot 2025-01-23 at 3 40 40 PM" src="https://github.com/user-attachments/assets/f4febafe-d041-48c5-a1e3-6f2f18458b75" />
<img width="421" alt="Screenshot 2025-01-23 at 3 42 14 PM" src="https://github.com/user-attachments/assets/1378c546-4afc-4e90-891e-c95eea98e73e" />
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

<img width="692" alt="Screenshot 2025-01-23 at 3 45 08 PM" src="https://github.com/user-attachments/assets/52a900c0-8050-413f-a57b-39d41995e8e1" />
<img width="664" alt="Screenshot 2025-01-23 at 3 45 52 PM" src="https://github.com/user-attachments/assets/b6c24ef3-1556-4afd-9721-bdca545410e5" />
<img width="651" alt="Screenshot 2025-01-23 at 3 46 30 PM" src="https://github.com/user-attachments/assets/ef0c6e97-061e-4331-8cd3-462490184e8d" />

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

<img width="921" alt="Screenshot 2025-01-23 at 3 50 00 PM" src="https://github.com/user-attachments/assets/bed85172-e5dc-4956-8762-eea8e07e5ade" />
<img width="421" alt="Screenshot 2025-01-23 at 3 51 12 PM" src="https://github.com/user-attachments/assets/1c0b9cad-2fc9-4446-babe-8ec74a4ffe5a" />
<img width="629" alt="Screenshot 2025-01-23 at 3 52 24 PM" src="https://github.com/user-attachments/assets/62af8808-9966-4ab2-a3f2-7138c65dd800" />

---

### Step 4: Enable Remote Access for Domain Users on Client-1
- **Sign in as a Domain Admin**
- **Modify Remote Desktop Settings**
  - Allow the `Domain Users` group to access RDP.
- **Test Access**
  - Sign into Client-1 with a standard user account to confirm permissions.

<img width="454" alt="Screenshot 2025-01-23 at 4 05 59 PM" src="https://github.com/user-attachments/assets/665b22bd-58b9-4127-a7f6-8667cb8e7b4c" />
<img width="843" alt="Screenshot 2025-01-23 at 4 06 36 PM" src="https://github.com/user-attachments/assets/1bb20155-8432-4e58-aec5-6d21b284fb1e" />

---

### Step 5: Automate User Creation with PowerShell
- **Open PowerShell ISE on DC-1**
- **Run a User Creation Script**
  - Use a script to bulk-create test accounts inside `_USERS`.
- **Validate Accounts**
  - Check ADUC to verify users were generated correctly.
- **Test Logins**
  - Attempt to log into Client-1 with one of the new domain accounts.

<img width="463" alt="Screenshot 2025-01-23 at 4 08 49 PM" src="https://github.com/user-attachments/assets/baaf9830-96e3-41ac-83d4-3d921678e598" />
<img width="1056" alt="Screenshot 2025-01-23 at 4 10 56 PM" src="https://github.com/user-attachments/assets/6d001fd1-8a35-4b2f-95c8-f7e307deff3d" />
<img width="785" alt="Screenshot 2025-01-23 at 4 12 32 PM" src="https://github.com/user-attachments/assets/a27ebddb-84e5-48a0-961f-b3e9cef46f2c" />

---

## Objective

This project replicates a full Active Directory domain environment inside Azure for training, testing, or proof-of-concept deployments. By following these steps, you create a scalable cloud-based lab that mirrors real-world enterprise domain structures and authentication flows.

---
