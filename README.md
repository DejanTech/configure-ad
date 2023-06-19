<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Active Directory's General Purpose</h2> 

Active Directory simplifies network management, enhances security, and streamlines user and resource administration in Windows-based networks. It provides a structured and organized approach to managing resources, ensuring efficient network operations.






<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Setup Resources in Azure
- Step 2: Ensure Connectivity between the client and Domain Controller
- Step 3: Install Active Directory
- Step 4: Create an Admin and Normal User Account in AD
- Step 5: Join Client-1 to the domain (dejab.com)
- Step 6: Setup Remote Desktop for non-administrative users on Client-1
- Step 7: Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Step 1: Setup Resources in Azure</h2>

<p>
<img src="https://i.imgur.com/A1PLq1r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
1. Create the Domain Controller VM (Windows Server 2022) named “DC-1.” Take note of the Resource Group and Virtual Network (Vnet) that
  get created at this time. 
2. Set Domain Controller’s NIC Private IP address to be static: Click DC-1> Networking> Network Interface> IPconfig> Ipconfig1 >static >Save
3. Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
4. Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

</p>
<br />
<h2>Step 2: Ensure Connectivity between the client and Domain Controller</h2>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
1. Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
2. Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall:
  click start >search: Windows Defender Firewall with Advanced Security >Inbound Rules >protocol -ICMPv4 (protocol ping uses)- right click Core Network Diagnostic-ICMP Echo Request (ICMPv4-in) >Enable.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
