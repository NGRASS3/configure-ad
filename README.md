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


<br>
<p>
Now we will join Client-1 to the domain (mydomain.com). From the azure portal we will change client-1's DNS settings to the DC's Private IP address. We can then restart Client-1 to flush the cache. 
</p>

![image](https://user-images.githubusercontent.com/111653930/236004716-a5adea24-2f82-4877-8822-023fde84f37b.png)


<br>
<p>
We can now log back into Client-1 and use the command line to run ipconfig /all to check that the DNS is now using DC-1s Private IP.
</p>

![image](https://user-images.githubusercontent.com/111653930/236006676-6ab580a9-6468-4c69-88c9-298f81697243.png)


<br>
<p>
Once we verify the proper DNS we can right click the start menu -> System -> Rename this PC -> Change -> Domain and type in mydomain.com. From here use the username mydomain.com\jane_admin. This will prompt a restart and now Client-1 will be officially joined to the Domain. Now when Client-1 comes back online we will be able to login to it using jane_admin because Client-1 is a member of the domain, and jane_admin is a member of that domain.</p>

![image](https://user-images.githubusercontent.com/111653930/236007980-0c1c0466-1f41-4be0-bc42-869e5c6f6c33.png)


<br>
<p>
Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. You can now log into Client-1 as a normal user.</p>

![image](https://user-images.githubusercontent.com/111653930/236009998-ef8f7127-9bf0-4eb2-8d2d-5908fa3dcb9e.png)


<br>
<p>
Now to test if non-admin users can log into Client-1 we will use Powershell to run a script that creates a large amount of random users. To start we login to DC-1 as jane_admin. We then open Powershell ISE -> create a new folder -> paste the contents of <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">this script</a> and run the file. Once completed we can check the "_EMPLOYEES" file and see all the newly created users.</p>

![image](https://user-images.githubusercontent.com/111653930/236014739-43f522bf-3c57-4455-86f5-8104be20544a.png)
![image](https://user-images.githubusercontent.com/111653930/236015256-e9b7fb56-db75-459c-ae0b-2a173075a607.png)


<p> Now we can select any randomly generated user, in this case "mit.rad" and login to Client-1 using "mydomain.com/mit.rad" with the password assigned in the script. We have successfully setup AD and allowed admin and non-admin users access.
</p>

![image](https://user-images.githubusercontent.com/111653930/236017028-24759f96-2588-484a-8e87-b39b562e289b.png)
![image](https://user-images.githubusercontent.com/111653930/236017065-9a782d4b-5911-4647-8135-79d42f87914c.png)




