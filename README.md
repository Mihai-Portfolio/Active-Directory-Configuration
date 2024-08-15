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
  
![image](https://github.com/user-attachments/assets/c57d03de-2674-4d9a-bbb3-372c27b600e7)

![image](https://github.com/user-attachments/assets/3f860bda-ef7b-412a-9577-b9985b199d76)

![image](https://github.com/user-attachments/assets/e9c77b9a-c704-429e-86ae-f1a1c86db86e)


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

![image](https://github.com/user-attachments/assets/9b299287-ee59-4e52-82d7-30adfb3d6e1b)
![image](https://github.com/user-attachments/assets/02c60a43-b83d-4502-a64a-a3b9ad437357)
![image](https://github.com/user-attachments/assets/b3ab14bb-247e-418a-8e34-5fa17aedbd4c)
![image](https://github.com/user-attachments/assets/54e16b2d-01ff-4020-b7d2-b06b4d97e484)
![image](https://github.com/user-attachments/assets/6aa015cb-58e1-4936-a76b-fa3afa21ba2d)


Next, we are going to install Active Directory Domain Services on the Domain-Controller. Open Server Manager in your Domain-Controller and click Add roles and features. Select role-based or feature based installation and click next, then check that your Domain-Controller and its private IP address is chosen for the destination server and click next. On the next page, select Active Directory Domain Services and add features. No need to change anything in the next 3 sections, just click next and then install. Once installed, click the notification icon on the top right corner of the screen and select Promote this server to a domain controller. Add a new forest name it "tutorialdomain.com". Create a password for the Directory Services Restore Mode password and click next, next, and on the following page, wait for your domain name to populate in The NetBIOS domain name section, then click next, next, and install. Once installed, the Domain-Controller virtual machine will log off automatically, and you'll have to log back in using the public IP address, username and password you created. 

![image](https://github.com/user-attachments/assets/6c165219-0e6c-42ea-9bbe-959bd0754efa)
![image](https://github.com/user-attachments/assets/439277e4-86e3-430e-8cc9-9e5b7108740b)
![image](https://github.com/user-attachments/assets/40e14396-a795-4bc8-b186-fad4b6966ba6)


Next, we will be creating two organizational units. Once logged back into Domain-Controller, open Server Manager, go to tools in the top right corner, and select users and computers. In Active Directory Users and Computers, right click tutorialdomain.com and add a new organizational unit. We will add in 2 organizational units, one named Employees and one named Administrators. 


![image](https://github.com/user-attachments/assets/010b65c1-9dd1-4eb7-a7e3-e4b252f40f7a)
![image](https://github.com/user-attachments/assets/dc64bdd4-2f02-48ba-9dcb-1106e2c45231)
![image](https://github.com/user-attachments/assets/b8732663-5958-42ea-8f19-5e707645c4a2)


Next, we will create an administator. To do so, right click your newly created organizational unit for Administrators, click new, and select user. Name the user Jane Doe, and make her username Jane-Doe.b I recommend using an easy password for the tutorial. Once Jane is created, right click her account, select properties, and add her to the domain admins group. Log out of the Domain-Controller virtual machine and log back in as Jane Doe with the username and password you created. 

![image](https://github.com/user-attachments/assets/75ae873c-1274-4ff6-bd66-af96037473ef)
![image](https://github.com/user-attachments/assets/04e7e753-5f0d-47e5-980e-b6a9fb6a7c84)
![image](https://github.com/user-attachments/assets/f8374a46-9475-4adf-ad9f-5fd320d36a2b)
![image](https://github.com/user-attachments/assets/a35e13af-9e04-4010-b8e8-090ed1005b78)

Next, we will configure DNS settings and join the domain. To do this, open the Client virtual machine in Azure, click network settings, and select Network interface / IP configuration. Under settings, click DNS server, change the setting to custom, enter the Domain-Controller's privat IP address under DNS server, and hit save, and restart your Client virtual machine. Log back in to your Client virtual machine, go to settings, about, rename this PC (advanced), and select change. Enter tutorialdomain.com under domain, and enter the username and password of Jane Doe to allow it permission to join the domain. The computer will restart itself to update the changes. In the Domain-Controller virtual machine, open Active Directory Users and Computers, drop down tutorialdomain.com, select computers, and notice how our Client virtual machine now shows. 

![image](https://github.com/user-attachments/assets/a8ca0341-1ccc-4c29-8231-91920366b26c)
![image](https://github.com/user-attachments/assets/983bab42-48ce-473b-a447-5efc9b2fbd36)


Next, we will set up Remote Desktop for non-administrartive users for our Client virtual machine. Log in to the Client virtual machine as Jane Doe, open settings, select about, and select Remote Desktop. Click Select users that can remotely access this PC, click add, and add in domain users. Now you can log in to the Client virtual machine as a normal non-administrative user. 

![image](https://github.com/user-attachments/assets/b9d40606-ca3f-40a5-b7b6-0f68b79e3aff)


Next, we are going to create additional users and manage their accounts. Log into the Domain-Controller virtual machine, open Windows Powershell ISE as an administrator, and select new script on the top left corner. Copy and Paste the following code and click run to have your computer create 10,000 random users - https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1. Go back to Active Directory Users and Computers, expand tutorialdomain.com and select Employees. You will notice users added in as employees. The code alternated vowels and consonance to make everyone's names, and set everyone's password to Password1.

![image](https://github.com/user-attachments/assets/48bd6012-4166-4949-821e-619ace60b5ef)
![image](https://github.com/user-attachments/assets/a9238ce2-e732-4e99-ba9d-903f2b004782)
![image](https://github.com/user-attachments/assets/f258d2e0-c3c8-468c-93e8-c8158ef5a1e8)
![image](https://github.com/user-attachments/assets/95b678c2-a61d-4ce8-8d57-c5c13d59a534)


Lastly, we are going to log in to the Client virtual machine with a random user. I chose the user fon.win, and logged into the Client virtual machine using the password Password1. Let's imagine fon.win forgot their password and needs a password reset. Log out of your Client virtual machine and open your Domain-Controller virtual machine. Right click the user needing a password reset, in this case it's fon.win, make their new password Password2 and unlock their account if needed.  Log back into the Client virtual machine as your selected user with Password 2. 

This concludes our Active Directory Tutorial!

***Be sure to delete your virtual machines in Azure at the end of the tutorial so you don't have to pay for something you're not using***
