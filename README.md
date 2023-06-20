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
- Step 7: Attempt to log into Client-1 with one of the users

<h2>Step 1: Setup Resources in Azure</h2>

<p>
<img src="https://i.imgur.com/A1PLq1r.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
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
Domain Controller
</p>
<img src="https://i.imgur.com/B2poN9i.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Client-1
</p>
<img src="https://i.imgur.com/x41LmgA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
1. Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)

2. Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall:
click start >search: Windows Defender Firewall with Advanced Security >Inbound Rules >protocol -ICMPv4 (protocol ping uses)- right click Core Networking Diagnostic - ICMP Echo Request (ICMPv4-in) >Enable.

3. Check back at Client-1 to see the ping succeed 
</p>
<br />
<h2>Step 3: Install Active Directory</h2>
<p>

<p>
<img src="https://i.imgur.com/AND7Aty.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/bFQKoDr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/QMvpkON.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
1. Login to DC-1 and install Active Directory Domain Services: Click Start >Server Manager >Add Roles and Features (to install AD) >Next >Next >check Acitive 
   Domain Services Features box >Next >Next >Install >Close. 

2. Promote as a DC; Setup a new forest as dejab.com: click the hazard symbol Ppromote this server to a domain controller  >check Add a new forest "dejab.com" >Next >Create password >Next >Next >Install

   *  THIS IS HOW WE FINISH INSTALLING ACTIVE DIRECTORY AND TURN THE SERVER INTO A DOMAIN CONTROLLER.

3. Restart and then log back into DC-1 as user: dejab.com\labuser (FQDN (Fully Qualified Domain Name)) 


<h2>Step 4: Create an Admin and Normal User Account in AD</h2>
</p>
<br />
<p>
<img src="https://i.imgur.com/GBSESIx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/EYQewW7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
Create an Organizational Unit called “_ADMINS" and  "_EMPLOYEES”: 

1. Right click dejab.com >New >Organizational Unit >type "_ADMINS" >Ok (repeat steps to create _EMPLOYEES)
   
2. Create a new employee named “Jane Doe” with the username of “jane_admin”
  *right click inside the empty space *click New >User (enter Name: Jane Doe & User logon name: jane_admin) >Next >Set password *check box: "password never expires" >Next >Finish
   
3. Add jane_admin to the “Domain Admins” Security Group: right click >Properties >Member of >Add: type: domain >Check Name >Domain Admin >Ok >Apply >Ok
   
4. Log out/close the Remote Desktop connection to DC-1 and log back in as “dejab.com\jane_admin”

<h2>Step 5: Join Client-1 to your domain (dejab.com)
</h2>
<p>
<img src="https://i.imgur.com/Jk0CB4h.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
<img src="https://i.imgur.com/F8RI0DK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
<img src="https://i.imgur.com/1MJa51s.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

1. From the Azure portal click: Client-1 >Networking >Networking Interface (private IP address) >DNS Servers >Custom: (paste DC-I IP address-save) click Virtual Machine >Client 1 >Restart. Login to Client-1 (Remote Desktop) as the original local admin (dejab.com\labuser)
   
2. Command Prompt: ipconfig /all (verify DNS server IP address update (DC-1 Private IP address))
   
3. Join Client-1 to the domain: right click >Start >System >Rename this PC (Advance) >Change > check: Domain, type: dejab.com >Ok
 Login dejab.com/jane_admins >Ok (computer will restart)
  
4. Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.

<h2>Step 6: Setup Remote Desktop for Non-administrative users on Client-1
</h2>

<p>
<img src="https://i.imgur.com/nzk52xL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/I7LglvN.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>

1. Log into Client-1 as dejab.com\jane_admins right click: >Start >System >"Remote Desktop”, select > "user that can remotely access the PC" >add, type: domain users >check names >Ok >Ok 
2. Go to DC-1 to see changes in Active Directory Domain Users Properties
   Click >Tools >Active Directory Users & Computers >dejab.com >Users >Domain Users >Members to 

<h2>Step 7: Attempt to log into Client-1 with one of the Users
</h2>

<p>
<img src="https://i.imgur.com/b22pq1K.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
<img src="https://i.imgur.com/UDXyMiH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

1. Login to DC-1 as jane_admin
2. Open PowerShell_ise as an administrator: right click >run, type: Powershell_ise (open as admin), click: >New File, paste the contents of the script into it
3. Run the script and observe the accounts being created
4. Open ADUC and observe the accounts in the appropriate OU
5. Attempt to log into Client-1 with one of the accounts








