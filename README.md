<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
In this tutorial we will be learning how deploy and configure Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

**Step 1: Setting up Resources in Microsoft Azure Portal**
  - Create 2 Virtual machines for domain controller (Windows server 2022) and the Client (Windows 10): The client (Client 01) will be under the same Resource Group and vnet as the Domain controller.
  - Set the Domain controller NIC's IP Address to be static:
    Go to Network settings -> Select the NIC of domain controller (starts in dc in this case) -> ip configurations -> ipconfig1 -> select static -> Save

![image](https://github.com/user-attachments/assets/5402d94a-5fc4-45d5-8153-29a4af099411)


**Step 2: Test conenctivity between the client and Domain Controller**
 - Log in to Client 01 and go to windows command line.
 - Ping the Domain controller (DC-01)'s Private IP through the command: ping -t. This allows a continous ping between the devices. Press Cmd+C to stop.

 Notice how the request timed out. This means there is an issue with the domain controller where the firewall may be blocking us:

  ![image](https://github.com/user-attachments/assets/1b428f77-4f40-4125-9dd1-a5738cbaa921)


  - Log in to DC- 01 and enable all ICMPv4 protocol on the local windows firewall. Now the ping works just fine!
  - Go to Windows Defender Firewall --> Advanced -> Click protocol to filter -> Right click and enable on all ICMPv4 Core Networking Diagnostics rules

    ![image](https://github.com/user-attachments/assets/718f8372-015c-41d6-9ed8-bd2590d91fc7)

- Go back to client 01 and try the ping -t command again. It works now!

![image](https://github.com/user-attachments/assets/8ae811e7-56e3-49ca-b8c5-ee3933066aa2)

**Step 3: Install Active Directory**
 - In rhw server manager in DC-1: Go to Add Roles and features --> Server roles -> Active Directory Domain Services --> Add features --> Install
   
   ![image](https://github.com/user-attachments/assets/539e7f16-8821-4dea-b4d5-841ed4e80f83)

 - Promote as a Domain controller: Setup a new forest as mydomain.com (We'll call ours corp.AD-Root.com)
   
![image](https://github.com/user-attachments/assets/b4ca04d9-f7f0-4702-a942-a578e07f4bf8)

![image](https://github.com/user-attachments/assets/a5c395a5-d68a-488d-866d-5ca677731874)

- Click next and set password on Domain controller options. Keep clicking next then install once validatioin finishes.

- Restart DC-1 as use and then log back in as user: username@corp.AD-root.com

**Step 4: Create an Admin and user account in Active Directory**
-  Go to active directory users and computers from search box
-  Create a new Organizational unit (OU) named "ADMINS". Right click on corp.AD-root.com and select organizational unit.
  
![image](https://github.com/user-attachments/assets/a53cc283-be1b-47c7-a97c-cbcaadf7aba8)

  
-  We will create a new admin employee named James King with the username of james_admin and set their password.
-  Add james_admin to the "Domain Admins" Security group: Right clock on the user --> Properties --> Click add on member of --> Type Domain admins and click Check names --> Click ok then apply then ok. 

  ![image](https://github.com/user-attachments/assets/a9ed6515-22af-4063-8f02-2c13cb1e483c)

-  Test the admin account by logging out of DC-1 then logging in again as james_admin@corp.AD-root.com This will serve as our admin account for the rest of this exercise.

  
**Step 5: Join Client-1 to your AD domain (corp.AD-root.com)**
 - From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address. Restart Client-01 after changing the DNS settings.
 - Remote into Client-01 as the original local admin (labuser in my case) and join it to the domain (computer will restart)
 - Login to the Domain Controller and verify Client-1 shows up in Active Directory Users and Computers  inside the “Computers” container on the root of the domain
 - Create a new OU named “CLIENTS” and drag Client-1 into there

  **Step 6: Setup Remote Desktop for Non-Admin users**
  
