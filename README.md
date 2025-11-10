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
### Step 2: Promoting the Server to a Domain Controller (DC)
- First we need to open **Server Manager**: Note- you may need to search for **Server Manager** in the bottom left-hand corner manually if it **Does Not** pop up automically
    - Refer to the picture below:
    - ![Step 2 Screenshot](https://github.com/user-attachments/assets/1aca8a62-37ae-4239-b9d7-89a9c9ceb0c1)

- Upon opening Server Manager, we will need to **Click AD DS** in the upper left corner
    - Refer to the picture below:
    - ![Step 2 Screenshot Cont.](https://github.com/user-attachments/assets/83a88c5c-c2ce-4008-ba8c-63af951a9a15)
- After we will see at the top of the page it will say **configuration required for Active Directory domain services at lab-vm** -> **Click on MORE**
    - Refer to the picture below:
    - ![Step 2 Screenshot Cont.](https://github.com/user-attachments/assets/7487f214-2a49-4548-a19a-7f0be2f244fe)
-Next, we will have to deploy the configuration. By doing this, the action will promote this server to a domain controller. To do this, we will need to click on Promote this server to domain controller under the action tab. Refer to the picture below:
    - ![Promoting Server to DC 2nd (Step 2)](https://github.com/user-attachments/assets/db42dd32-4328-43a2-9e64-643d90982b32)
- Then **Click Add a new Forest**
    -In the **Root domain name** -> Enter in **lab.local**
    -Upon clicking next for the page -> **enter in the same login password as your Virtual Machine**
      -Refer to the picture below:
      -![Enter PW (Step 2)](https://github.com/user-attachments/assets/6bdcaf08-600b-4f56-95e7-42876de65669)
-Click next until you are on the prerequisites screen, then go ahead and **Click install**
    - Refer to the picture below for reference:
![Install of DC (Step 2)](https://github.com/user-attachments/assets/6472d6cf-6827-4faa-8b1d-a154257b9f0b)

> After completing the Domain Controller installation, the VM will automatically restart. This usually takes a couple of minutes.

**Importance of Restarting the Server:**

Restarting a server after installing roles, features, or security updates is **critical in IT** for several reasons:

1. **Apply Security Updates:** Many patches and security updates require a restart to fully integrate with the operating system. Without a restart, the system may remain vulnerable.  
2. **Initialize New Services:** Roles like Active Directory Domain Services only start and configure properly after a reboot. This ensures that all dependencies and services are running correctly.  
3. **Prevent System Instability:** Some configurations or updates may leave the server in a partially applied state until a restart occurs. Rebooting ensures a clean and stable environment.  
4. **Verify Successful Installation:** Restarting confirms that the system can boot with the new roles, updates, or features applied, reducing the risk of failure in production environments.  

> 💡 In IT, routine reboots after updates or configuration changes are considered **best practice** to maintain security, stability, and reliability.

### Step 3: Open Active Directory Users and Computers (ADUC)
- Click **Start → Server Manager → AD DS (Upper left-hand corner)**
    -For reference, refer to the picture below
    -![Active Directory (ADUC) (Step 3)](https://github.com/user-attachments/assets/ee01c696-199d-4157-9f6f-45d1a45a3b7f)
    -This is how our **Active Directory** should look: Refer to the picture below
    -![An Actual Look of Active Directory (ADUC) (Step 3)](https://github.com/user-attachments/assets/e80425c7-4ee5-483a-9a8a-357f7ccc1c42)

---

## Phase 2: Lab Steps

### Step 4: Create Organizational Units (OUs)
**GUI**
- Right-click our domain → **New → Organizational Unit**
- Name the OU (Example: *Branch 1*) → Click **OK**
    -Refer to the picture for reference:
    -![Creating an OU (Step 3)](https://github.com/user-attachments/assets/b65f085e-c815-4e3a-b0ee-5069afce7883)
    -![Creating an OU Branch 1(Step 3)](https://github.com/user-attachments/assets/39fcc5ea-baf9-4699-ad65-d71c56c75b11)


**PowerShell**
```powershell
New-ADOrganizationalUnit -Name "IT_Dept" -Path "DC=yourdomain,DC=com"

