# Lab 2: Configuring Active Directory Users and Groups in Azure
**Created by:** Terrence Edwards  

---

## 🧩 Overview
This lab demonstrates how to configure **Active Directory (AD) Users and Groups** in a Windows Server VM deployed in Azure.

### Objectives
- Create Organizational Units (OUs)
- Create users
- Add users to groups

> **Note:** This lab assumes a Domain Controller is already running in Azure (from Lab 1).

---

## Step 1: Connect to Your Windows Server VM

1. Go to your VM **Overview** in the Azure Portal.  
2. Select **Bastion**.  
3. Enter your credentials and click **Connect**.  

![Step 1 Screenshot](https://github.com/user-attachments/assets/14d3f570-bb71-47e9-a3fd-88154acb8bf4)  
![Step 1 Screenshot](https://github.com/user-attachments/assets/2b3ce811-6754-43bb-b51d-b0d558d1aef5)

---

## Step 2: Promote the Server to a Domain Controller (DC)

1. Open **Server Manager**.  
   > 💡 You may need to search for *Server Manager* in the Start Menu if it does not open automatically.  
   ![Server Manager](https://github.com/user-attachments/assets/1aca8a62-37ae-4239-b9d7-89a9c9ceb0c1)

2. Click **AD DS** in the upper-left corner.  
   ![Step 2 Screenshot](https://github.com/user-attachments/assets/83a88c5c-c2ce-4008-ba8c-63af951a9a15)

3. At the top, click **More** next to “Configuration required for Active Directory Domain Services at lab-vm.”  
   ![Step 2 Screenshot Cont.](https://github.com/user-attachments/assets/7487f214-2a49-4548-a19a-7f0be2f244fe)

4. Under the **Action** tab, click **Promote this server to a domain controller.**  
   ![Promoting Server to DC](https://github.com/user-attachments/assets/db42dd32-4328-43a2-9e64-643d90982b32)

5. Select **Add a new forest**, and set your **Root domain name** to `lab.local`.  
   Enter the same password you used for your VM login.  
   ![Enter PW](https://github.com/user-attachments/assets/6bdcaf08-600b-4f56-95e7-42876de65669)

6. Click **Next** until you reach the prerequisites screen, then click **Install**.  
   ![Install of DC](https://github.com/user-attachments/assets/6472d6cf-6827-4faa-8b1d-a154257b9f0b)

> After completing the Domain Controller installation, the VM will automatically restart. This usually takes a couple of minutes.

---

### 💡 Importance of Restarting the Server

Restarting a server after installing roles, features, or updates is **critical in IT** because it:

1. **Applies Security Updates:** Patches require a reboot to secure the system fully.  
2. **Initializes New Services:** Roles like AD DS only start properly after restart.  
3. **Prevents Instability:** Ensures all updates are cleanly applied.  
4. **Verifies Configuration:** Confirms the server boots and runs with new roles correctly.

> Routine reboots after configuration changes are **best practice** for maintaining stability, reliability, and security.

---

## Step 3: Open Active Directory Users and Computers (ADUC)

1. Click **Start → Server Manager → AD DS**.  
   ![ADUC](https://github.com/user-attachments/assets/ee01c696-199d-4157-9f6f-45d1a45a3b7f)

2. Open **Active Directory Users and Computers**.  
   ![ADUC View](https://github.com/user-attachments/assets/e80425c7-4ee5-483a-9a8a-357f7ccc1c42)

---

## Step 4: Create Organizational Units (OUs)

1. Right-click your domain → **New → Organizational Unit**.  
2. Name the OU (example: `Branch1`) → Click **OK**.  
   ![Create OU](https://github.com/user-attachments/assets/b65f085e-c815-4e3a-b0ee-5069afce7883)  
   ![Branch1 OU](https://github.com/user-attachments/assets/39fcc5ea-baf9-4699-ad65-d71c56c75b11)

3. Repeat this process to add:
   - **Users**
   - **Computers**
   - **Groups**

---

## Step 5: Create a User in Active Directory

1. Right-click on **Users** under your OU (e.g., Branch1).  
2. Click **New → User**.  
   ![Create User](https://github.com/user-attachments/assets/d5b52fbc-096d-447c-a066-5ece45d1150b)

3. Fill out the user info and click **Next**.  
   ![User Info](https://github.com/user-attachments/assets/4e3a4f6f-1301-4c26-9d89-caf51620e170)

4. Enter a password and confirm.  
   ![Password Setup](https://github.com/user-attachments/assets/52bbed9f-cebd-4481-87f5-325482ed9e9e)

5. Click **Finish** — your new user (e.g., *Tez Dough*) should appear under **Users**.  
   ![First User](https://github.com/user-attachments/assets/e183b16d-0099-45ce-8ead-ac3632255b73)

---

## Step 6: Understanding the Default “Computers” Container

When you join a Windows device to your domain (like `yourdomain.com`), the computer object must be stored somewhere in Active Directory.  
By default, AD places new computers in a built-in container called **Computers** — not inside an OU.

yourdomain.com
├── Builtin

├── Computers ← Default container

├── Domain Controllers

├── Users

└── [Your custom OUs]

### 🧠 Key Notes
- The **Computers** container is not a true OU — you **cannot** link Group Policies to it.
- In **hybrid environments** (on-prem + Azure AD), Azure AD Connect only syncs selected OUs.

If your computers remain in the default container and it’s **not selected for sync**, those devices will **not appear in Azure AD** — meaning no Intune or Conditional Access management.

---

### ✅ Best Practice: Move Computers into a Custom OU

Create your own OU structure for devices:

yourdomain.com
└── OU=Devices

├── OU=IT_Computers

├── OU=Faculty_Computers

└── OU=Student_Computers


Move computers out of the default **Computers** container into your custom OUs manually or via PowerShell:
Powershell example below: (I will over this in future labs)
```powershell
Move-ADObject -Identity "CN=PC01, CN=Computers, DC=lab, DC=local" -TargetPath "OU=IT_Computers, OU=Devices, DC=lab, DC=local"

🌐 Summary (Hybrid Context)

| Location          | Description                                       | Syncs to Azure AD?                |
| ----------------- | ------------------------------------------------- | --------------------------------- |
| **CN=Computers**  | Default container for new domain-joined computers | ❌ Not by default                  |
| **OU=Devices**    | Custom OU you create for computers                | ✅ If selected in Azure AD Connect |
| **OU=Syncing OU** | OU chosen during Azure AD Connect setup           | ✅ Actively synced to Azure        |

💡 In a hybrid AD setup, always move or redirect computers to a syncing OU to ensure device management, GPOs, and Azure sync all function correctly.

🧾 Lab Completion

You have now:

Promoted a server to a Domain Controller

Configured OUs and Users

Understood the function of the default “Computers” container

Learned hybrid syncing best practices

🧹 Reminder: Always delete your resource group in Azure when finished to avoid unnecessary charges.

BONUS: PowerShell commands
I will cover this in a future lab

Step 7: PowerShell Verification (Users & OUs)

After creating your users and OUs, use PowerShell to verify their existence in Active Directory.
# List all Organizational Units
Get-ADOrganizationalUnit -Filter * | Select-Object Name, DistinguishedName

# List all users in the domain
Get-ADUser -Filter * | Select-Object Name, SamAccountName

# List users within a specific OU (e.g., Branch1)
Get-ADUser -SearchBase "OU=Users,OU=Branch1,DC=lab,DC=local" -Filter * | Select-Object Name, Enabled

# View which groups a specific user belongs to
Get-ADUser -Identity "Tez Dough" -Properties MemberOf | Select-Object -ExpandProperty MemberOf

🧠 Using PowerShell for verification provides a faster and more scalable way to confirm your configuration across environments.

🧾 Lab Completion

You have now:

Promoted a server to a Domain Controller

Configured OUs and Users

Understood the function of the default “Computers” container

Verified AD objects using PowerShell

Learned hybrid syncing best practices

🧹 Reminder: Always delete your resource group in Azure when finished to avoid unnecessary charges.



    
