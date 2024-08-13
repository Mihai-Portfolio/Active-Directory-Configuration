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
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log in to the Client virtual machine with the username and password you created and ping the Domain-Controller to test connectivity. In Azure, find the Domain-Controller's private IP address in the network section and use the command ping -t to preform a perpetual ping. In this case, the command would be ping "10.0.0.5 -t". 
</p>
<br />
