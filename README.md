<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
In this tutorial we will be learning how depploy Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

**Step 1: Setting up Resources in Microsoft Azure Portal**
  - Create 2 Virtual machines for domain controller (Windows server 2022) and the Client (Windows 10): The client (Client 01) will be under the same Resource Group and vnet as the Domain controller.
  - Set the Domain controller NIC's IP Address to be static:
    Go to Network settings -> Select the NIC of domain controller (starts in dc in this case) -> ip configurations -> ipconfig1 -> select static -> Save

![image](https://github.com/user-attachments/assets/5402d94a-5fc4-45d5-8153-29a4af099411)


**Step 2: Test conenctivity between the client and Domain Controller**
 - Log in to Client 01 and go to windows command line.
 - Ping the Domain controller (DC-01)'s Public IP through the command: ping -t. This allows a continous ping between the devices. Press Cmd+C to stop.

 Notice how the request timed out. This means there is an issue with the domain controller:
 ﻿﻿﻿﻿ ﻿![image](https://github.com/user-attachments/assets/79ac28cc-4704-442f-b20f-fead6886b966)

  - Log in to DC- 01 and enable all ICMPv4 protocol on the local windows firewall. Now the ping works just fine!
  - Go to Windows Defender Firewall --> Advanced -> Click protocol to filter -> Right click and enable all ICMPv4 echo request rules

    ![image](https://github.com/user-attachments/assets/718f8372-015c-41d6-9ed8-bd2590d91fc7)

- Go back to client 01 and try the ping -t command again. It works now!


    
  
