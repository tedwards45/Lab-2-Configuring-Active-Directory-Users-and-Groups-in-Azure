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

### Step 4: Create Organizational Units (OUs)

- Right-click our domain → **New → Organizational Unit**
- Name the OU (Example: *Branch 1*) → Click **OK**
    -Refer to the picture for reference:
    -![Creating an OU (Step 4)](https://github.com/user-attachments/assets/b65f085e-c815-4e3a-b0ee-5069afce7883)
    -![Creating an OU Branch 1(Step 4)](https://github.com/user-attachments/assets/39fcc5ea-baf9-4699-ad65-d71c56c75b11)
- Repeat the same process to add the next OU groups:
    - Users
    - Computers
    - Groups

### Step 5: Creating a User in Active Directory

![Creating a User in Active Directory (Step5)](https://github.com/user-attachments/assets/d5b52fbc-096d-447c-a066-5ece45d1150b)
-To create a user, we must first **Right-Click on Users under Branch 1**: Refer to the picture for reference
    -Click **New**: Refer to the picture below for reference:
    -![Creating a User-- Fill out User Info (Step5)](https://github.com/user-attachments/assets/4e3a4f6f-1301-4c26-9d89-caf51620e170)
Then we will need to give our user a password:
    -Note: the password can be anything
    -Refer to the picture below for reference: 
    -![Giving our User a password (Step5)](https://github.com/user-attachments/assets/52bbed9f-cebd-4481-87f5-325482ed9e9e)
    -Finally, click **Finish**
    -The picture below is where we should see our User, in this case, **Tez Dough**, should show up underneath **Users**
    -![First User (Tez Dough)(Step5)](https://github.com/user-attachments/assets/e183b16d-0099-45ce-8ead-ac3632255b73)

I just want to mention as well that there is already a **default computers group**: here is an explanation on why this is:

Here’s a clear, simple breakdown of how the default “Computers” container works — especially when you have custom OUs, syncing OUs, and a hybrid environment (on-prem + Azure AD).

🖥️ What Is the Default “Computers” Container?

When you join a Windows device to your domain (like yourdomain.com), that computer object must be stored somewhere in Active Directory.

By default, Active Directory automatically places new computers in a built-in container called “Computers”, located directly under your domain root — not inside an Organizational Unit (OU).

You can see it here:

yourdomain.com

├── Builtin

├── Computers   ← Default container

├── Domain Controllers

├── Users

└── [Your custom OUs]


🧠 The “Computers” container is not a true OU — it’s a special default container with limited management options (for example, you can’t link Group Policy Objects (GPOs) directly to it).

🧩 Why This Matters in a Hybrid or Syncing OU Setup

If you have Azure AD Connect syncing your on-prem Active Directory to Azure AD, only the OUs you choose during setup will sync to the cloud.

So:

If your devices are still in the default “Computers” container

And that container is not selected for sync
→ Then those computer objects won’t sync to Azure AD.

That means they won’t appear in your Azure Active Directory, and you won’t get features like Intune management, Conditional Access, or cloud-based policies for those devices.

🧠 Best Practice: Move Computers into a Custom OU

To manage your computers properly and ensure they sync to Azure AD, create your own OU structure for devices.

For example:

yourdomain.com

└── OU=Devices

├── OU=IT_Computers
     
├── OU=Faculty_Computers
     
└── OU=Student_Computers


Then, move computers out of the default Computers container and into your custom OUs.

You can do this manually in Active Directory Users and Computers (ADUC)

🌐 Summary (Hybrid Context)
| Location          | Description                                       | Syncs to Azure AD?                |
| ----------------- | ------------------------------------------------- | --------------------------------- |
| **CN=Computers**  | Default container for new domain-joined computers | ❌ Not by default                  |
| **OU=Devices**    | Custom OU you create for computers                | ✅ If selected in Azure AD Connect |
| **OU=Syncing OU** | OU chosen during Azure AD Connect setup           | ✅ Actively synced to Azure        |


💡 Key Takeaway

In a hybrid Active Directory:

The default Computers container is just a temporary holding place.

You should move or redirect computers to a custom, syncing OU.

This ensures device management, GPOs, and Azure sync all work correctly across both environments.

**Remember to always delete your resource group to avoid getting charged a lot of money**


    
