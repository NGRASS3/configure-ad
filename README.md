<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create 2 VMs DC-1 (Windows Server 2022) & Client VM (windows 10)
- Ensure Connectivity between client and Domain Controller
- Install Active Directory on Domain Controller
- Create an Admin and Normal User Account in AD

<h2>Deployment and Configuration Steps</h2>

<p>
To get this lab started we will need to create 2 VMs in Azure - a Domain Controller VM (Windows Server 2022) named “DC-1” and a Client VM (Windows 10) named “Client-1”. 
</p>

![image](https://user-images.githubusercontent.com/111653930/235935973-2acf7acf-1734-497b-8d25-1a175b1dcff5.png)


<br>
<p>
Once created we need to set DC-1s NIC Private IP Address to static. While inside Azure navigate to DC-1 VM -> Networking -> Network Interface -> IP Configurations -> ipconfig1 - then change the Assignment from Dynamic to Static. 
</p>

![image](https://user-images.githubusercontent.com/111653930/235938730-70a7adc6-7792-426f-a02e-10a4a1fd0bd0.png)


<br>
<p>
To ensure connectivity between the client and the domain controller we will login to Client-1 and ping DC-1's IP address with ping -t (a perpetual ping). This will timeout due to DC-1's windows firewall blocking ICMPv4 traffic.
</p>

![image](https://user-images.githubusercontent.com/111653930/235984862-80dfabfc-c678-411a-bd38-1afcdf3a4b6e.png)

<br>
<p>
To fix this we login to DC-1 and enable ICMPv4 on the local windows Firewall. Once completed we can re-try pinging DC-1 from Client-1 and this time it's successful. 
</p>

![image](https://user-images.githubusercontent.com/111653930/235986537-4b62be3e-0b32-4bc6-84fc-075a2906052a.png)
![image](https://user-images.githubusercontent.com/111653930/235986600-385429f5-e49f-4e3a-94da-270fffd8645f.png)

<br>
<p>
Now we will install AD on DC-1 and create a new forest called "mydomain.com". 
</p>

![image](https://user-images.githubusercontent.com/111653930/235989735-1e6366b3-6d8a-42d6-8279-678c9305047d.png)


<br>
<p>
Once installed we will restart the remote desktop for DC-1 and log back in. This time we will log in using mydomain\username since DC-1 is now officially a Domain Controller. From here we can run Active Directory and within Active Directory Users and Computers (ADUC), we will create an Organizational Unit (OU) called “_EMPLOYEES” and one named "_ADMINS".
</p>

![image](https://user-images.githubusercontent.com/111653930/235995734-5bd1e6ff-27bf-4a8a-9c56-8e14a09d6f03.png)

<br>
<p>
Next we will create a new user named Jane Doe. To add a new user right click inside the "_ADMINS" OU and click New -> User. Once the user is created we right click her name and select "Member of" to add her to "Domain Admins". This will gve Jane Doe full admin access.
</p>

![image](https://user-images.githubusercontent.com/111653930/236000040-b7590fff-d552-4b72-ba17-82265a092902.png)
![image](https://user-images.githubusercontent.com/111653930/236000096-83992613-64f2-4835-af81-819fef6488aa.png)







