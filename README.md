# Lab 2: Configuring Active Directory Users and Groups in Azure
**Created by:** Terrence Edwards

---

## Overview
This lab demonstrates how to configure **Active Directory (AD) Users and Groups** in a Windows Server VM deployed in Azure.

### Objectives
- Create Organizational Units (OUs)
- Create users
- Create security and distribution groups
- Add users to groups
- Verify membership using GUI and PowerShell

> **Note:** This lab assumes a Domain Controller is already running in Azure (from Lab 1).

---

## Phase 1: Preparation

### Step 1: Connect to Your Windows Server VM
- Go to the VM overview in Azure Portal
- Select **Bastion**
- Enter your credentials
- Click **Connect**

![Step 1 Screenshot](https://github.com/user-attachments/assets/14d3f570-bb71-47e9-a3fd-88154acb8bf4) 
![Step 1 Screenshot](https://github.com/user-attachments/assets/2b3ce811-6754-43bb-b51d-b0d558d1aef5)


---

### Step 2: Open Active Directory Users and Computers (ADUC)
- Click **Start → Server Manager → Tools → Active Directory Users and Computers**

![Step 2 Screenshot]

---

## Phase 2: Lab Steps

### Step 3: Create Organizational Units (OUs)
**GUI**
- Right-click your domain → **New → Organizational Unit**
- Name the OU (Example: `IT_Dept`) → Click **OK**

**PowerShell**
```powershell
New-ADOrganizationalUnit -Name "IT_Dept" -Path "DC=yourdomain,DC=com"

