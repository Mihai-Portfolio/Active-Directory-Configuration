<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>u

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Connect Domain Controller to a Virtual Machine Representing a User
- Install Active Directory
- Create Administrator and User Accounts
- Connect the User Virtual Machine to the Domain
- Create Additional Users Using Powershell ICE and Manage Their Usernames and Passwords

<h2>Deployment and Configuration Steps</h2>

<p>
  
![image](https://github.com/user-attachments/assets/976fa096-a147-4f93-90f6-a6e417377dca)
![image](https://github.com/user-attachments/assets/c78a2267-f08d-49cf-a63a-3828a6948732)
![image](https://github.com/user-attachments/assets/baa28369-96ee-4322-9b5b-8c5cbb257c40)

</p>
<p>
Start out by creating two virtual machines in Azure. The first virtual machine will be our domain controller which we will name Domain-Controller. Central Canada is the region chosen, however, you may choose a different region depending on availability. Select Windows Server 2022 Datacenter: Azure Edition as the image, and make sure your Domain Controller has at least 2 vCPUs and create. Next, we will create our client's virtual machine. We will name this virtual machine Client, and we will use Windows 10 Pro as the image and give it at least 2 vCPUs. Be sure that the Client virtual machine is connected to the same resource group, region and virtual network. In this case, the resource group is Active-Directory-Tutorial, the region is Canada Central, and the virtual network is Domain-Controller-Virtual-Network. 
</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/8789917f-2505-42d7-b872-281f76b34874)
![image](https://github.com/user-attachments/assets/9b012cb0-36ea-4bdf-95b9-558ea6eab159)


</p>
<p>
Once both virtual machines are created, we are going to change the Domain-Controller's Network Interface Card (NIC) Private IP Address to Static. To do this, go to virtual machines, select Domain-Controller, select network setting, and click Network Interface / IP Configuration towards the top of the screen. On the next screen, select ipconfig1 to open Edit IP Configuration. Change allowcation to static instead of dynamic and click save. 
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/c96699f2-52d0-48f3-b307-8a971b5721bd)
![image](https://github.com/user-attachments/assets/d75c13fe-1ff3-4fbb-98ef-20f09c6601a1)
![image](https://github.com/user-attachments/assets/d30022b8-6f44-4d52-bce5-46e71c0fc6ea)
![image](https://github.com/user-attachments/assets/03c8686d-54b4-49f2-83e1-32e68e314383)


</p>
<p>
Log in to the Client virtual machine with the username and password you created and ping the Domain-Controller to test connectivity. In Azure, find the Domain-Controller's private IP address in the network section and use the command ping -t to preform a perpetual ping. In this case, the command would be ping "10.0.0.5 -t". Once you ping the Domain-Controller, you will notice the request times out after every attempt. To fix this, we are going to log into the Domain-Controller with the username and password we created when making the virtual machine. Once logged in, type "wf.msc" in the search bar at the bottom to open Windows Defender Firewall with Advanced Security. Once opened, select Inbound Rules, filter the list by protocol, look for ICMPv$ protocols in the middle of the screen, and enable both Core Networking Diagnostics inbound rules. Once those two rules are enabled, go back to the Client virtual machine and see the ping succeed. 
</p>
<br />


Next, we are going to install Active Directory Domain Services on the Domain-Controller. Open Server Manager in your Domain-Controller and click Add roles and features. Select role-based or feature based installation and click next, then check that your Domain-Controller and its private IP address is chosen for the destination server and click next. On the next page, select Active Directory Domain Services and add features. No need to change anything in the next 3 sections, just click next and then install. Once installed, click the notification icon on the top right corner of the screen and select Promote this server to a domain controller. Add a new forest name it "tutorialdomain.com". Create a password for the Directory Services Restore Mode password and click next, next, and on the following page, wait for your domain name to populate in The NetBIOS domain name section, then click next, next, and install. Once installed, the Domain-Controller virtual machine will log off automatically, and you'll have to log back in using the public IP address, username and password you created. 


Once logged back into Domain-Controller, open Server Manager, go to tools in the top right corner, and select users and computers. 
