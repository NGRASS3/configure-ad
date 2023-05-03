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
- Set DC-1s NIC Private IP address to be static
- Step 3
- Step 4

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
