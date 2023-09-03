<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
In this lab we will create two VMs in the same VNET. One will be a Domain Controller, the other will be a Client machine. Because the DC is providing Active Directory services to the client workstation, we will switch it to a static IP. The client computer will join the domain. We will have control over the client computer's DNS configuration, and the client computer will use the DC as its DNS server.<br />




- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Resource Group setup
- Virtual Machine setup
- Create Domain Controller VM (Windows Server) named "DC 1"
- Set the NIC Private IP to Static
- Create Client VM (Named Client 1)
- Ensure connectivity between Client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users



<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.gyazo.com/fc0610532047ba3d384af1156502344a.png" height="80%" width="80%" alt="Resource Group Setup"/>
</p>
<p>

</p>
</br>




<p>
<img src="https://i.gyazo.com/9eea4c8e97a169a91c643f45a2878bc6.png" height="80%" width="80%" alt="Resource Group Setup"/>
</p>
<p>
Enter the Azure portal and search for Resource Group. 
</p>
</br>

<p>
<img src="https://i.gyazo.com/4d71396d56ec8a80d5c77e3d6ffef3d8.png" height="80%" width="80%" alt="Resource Group setup settings"/>
</p>
<p>
Set up your resource group based on your region. 
</p>
</br>
<p>
<img src="https://i.gyazo.com/40d46ab39d08d4c6e2abf351a30ce432.png" height="80%" width="80%" alt="Virtual Machine setup"/>
</p>
<p>
Now go into the Azure portal, search for Virtual machines, and click create your Virtual machine. 
</p>
</br>

<p>
<img src="https://i.gyazo.com/f6a8f1c61a1d716f3a5d9952594d46d8.png" height="80%" width="80%" alt="Virtual Machine setup"/>
</p>
<p>
Here are the settings for my Virtual Machine, again adjust the region setting based on your location.
</br>

<p>
<img src="https://i.gyazo.com/befd2b6e509ecdd2fa76827b288afbb1.png" height="80%" width="80%" alt="Virtual Machine setup"/>
</p>
<p>
Proceed to Disk (settings left to default, then to Networking.) In the Network tab (Settings also left to default), your Virtual network will automatically be created by Azure, this is also where you will see your public and private IP. You can then click Review+Create, Azure will validate your settings and you can now create your Virtual Machine. You will then see "Deployment in Progress" and below that resources will be created which include: Virtual Network, a Public IP, a Network Security group (Essentially a Firewall your VM will use), an OS disk, and a Virtual Network Interface Card.Repeat the process and in the second network tab, select the same virtual network as your first VM. 
</p>
</br>


<img src="https://i.gyazo.com/85c0a5471c38fe5f82b8c6a384135c59.png" height="80%" width="80%" alt="Set IP to Static p"/>
</p>
<p>
DC-1 has to have a static Private IP Address. Client one will connect to DC-1 to ensure connectivity we will try to ping DC-1 from Client-1. At first, the ping will not work correctly.
  
So Go back into the first VM and go to the networking tab. Here we will click on the Network Interface > IP configurations 
</p>

</br>

<p>
  
<img src="https://i.gyazo.com/c796465ab4ae4859cd928f7a3481d4c5.png" height="80%" width="80%" alt="Setting Dynamic IP to Static"/>

</p>
<p>
  
Set the Dynamic IP to Static. 
  
</p>

</br>
<p>
  
<img src="https://i.gyazo.com/cacc2981608f6536c6dc182950bffdb1.png" height="80%" width="80%" alt="RDP Set up"/>

</p>
<p>

Now go to Client 1 and connect using Remote Desktop Connection and the public IP address. Once connected, we will use the private IP address of DC-1 and ping using command prompt. 

</p>

</br>

<p> 
  
<img src="https://i.gyazo.com/1838fce31608d9a33f30e280db656162.png" height="80%" width="80%" alt="Ping Test"/>

</p>
<p>

Our ping has failed because Windows Firewall is blocking ICMP traffic, we have to enable ICMPv4 on the firewall on DC-1. so we will do the same remote desktop connection process with DC-1. 
</p>

</br>

<p> 
<img src="https://i.gyazo.com/9c9e95e3f28c15b886276edb21508ba7.png" height="80%" width="80%" alt="ICMP Set up"/>

</p>
<p>

Search for firewall and click Windows Firewall with Advanced Security. Then we will go to inbound rules, sort by protocol, find ICMPv4, and enable "Core Networking Diagnostics - ICMP Echo Request". If we go back to Client 1 we will now see that the ping succeeded. 

</p>

</b>

<p>
  
<img src="https://i.gyazo.com/b412092248d8decf3c328845b37f5d2f.png" height="80%" width="80%" alt="RDP Set up"/>

</p>
<p>

Now we will set up Active Directory. We will use default settings on Before you Begin, Installation Type, and Server Selection. Once you are on the Server Roles tab, click on Active Directory Domain Services and Add Features. Once that is done, you can click next on all the default settings on each tab and once on the confirmation tab we can click Install. 
</p>

</br>

<p>
  
<img src="https://i.gyazo.com/83bac03e39e02da34de7b259af502641.png" height="80%" width="80%" alt="RDP Set up"/>

</p>
<p>

After it finishes installing, go into the Server Manager Dashboard and click the flag with the notification at the top right. Click Promote the Server Controller and run through the installation, nothing needs to be changed from the default.
The DC will need to be restarted, but during remote desktop connection you will need to login with Full Qualified Domain Name (FQDN). Use "Different account" during login and login with "mydomain.com/*****" (Username and password you chose). 

</p>

</br>

<p>
  
<img src="https://i.gyazo.com/4d49133f3bc030a78b6d97619c7b905c.png" height="80%" width="80%" alt="RDP Set up"/>

</p>

<p>

Once restarted, go into the DC and search "Active Directory Users and Computers". From there right click on my domains, go to New > Organizational Unit and set up an EMPLOYEES unit and ADMINS unit. Go to the Admins unit and add New User (can be named whatever you want). 

</p>

</br>

<p>
  
<img src="https://i.gyazo.com/dc4cffa67d85d08f777d18219bc13670.png" height="80%" width="80%" alt="Admin Set up"/>

</p>

<p>

Once created, go to properties > Member of > Add... > Domain Admins > OK > Apply

</p>

</br>

<p>
  
<img src="https://i.gyazo.com/dc4cffa67d85d08f777d18219bc13670.png" height="80%" width="80%" alt="Admin Set up"/>

</p>

<p>

Once created, go to properties > Member of > Add... > Domain Admins > OK > Apply.
From here you will now Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”


</p>

</br>

<p>
  
<img src="https://i.gyazo.com/98201a4b2b5330ffef82f604abfa9c82.jpg" height="80%" width="80%" alt="Admin Set up"/>

</p>
<p>

Go back into the Azure portal to our DC VM, then the Networking tab, and copy the NIC Private IP. Then we will go to C1's > Networking > Click on the virtual NIC next to Network Interface > DNS Servers > Change DNS servers to DC's NIC Private IP address and save. 


</p>

</br>

<p>
  
<img src="https://i.gyazo.com/e8a395b7a2cb093c3cc2d71edc544e14.png" height="80%" width="80%" alt="Admin Set up"/>

</p>

<p>

Restart Client 1 from the Azure portal and login to Client 1 using the new Public IP address listed in the Azure Portal. 

</p>


</br>

<p>
  
<img src="https://i.gyazo.com/c5752d5fcf8ee8ce3c3c925a166b1c1f.jpg" height="80%" width="80%" alt="Admin Set up"/>

</p>

<p>

In Client 1, go to Settings > Rename PC > Change and enter mydomain.com into Member of. This will point to the DC and will know Client 1 can join through the domain itself. A notification should appear welcoming you to the Domain and the Client will restart. Once it has restarted, search Remote Desktop Settings > Select Users that can remotely access to the PC > ADD > Domain users. This will automatically put all users in the security group and click OK. (Normally we would want to do this through Group Policy). 

</p>

</br>

<p>
  
<img src="https://i.gyazo.com/656e1439d4c5ae71151a72e6161d6705.png" height="80%" width="80%" alt="Admin Set up"/>

</p>

<p>

You can see all User Accounts created are added to Domain Users group automatically and should all be able to login to Client 1. You can now login to Client 1 as a normal, non-admin user. Normally we would want to do this through Group Policy. 

</p>

<br>
<p>
  
<img src="https://i.gyazo.com/83640dcdc6ff260fb78a52dd44dd96b1.png" height="80%" width="80%" alt="Admin Set up"/>

</p>

<p>

We can see that we are logged into DC1 as Jane_Admin. Open Powershell_ise as an Admin, Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)


</p>

<br>

<p>
  
<img src="https://i.gyazo.com/86e0c7c0e32c0db98c1ae108f46cb3b4.png" height="80%" width="80%" alt="Admin Set up"/>

</p>

<p>

On line 1, we see the script will create account with the password "Password1" and it will create 100000 scripts. Run the script and watch as accounts are created. 

</p>
<p>

<img src="https://i.gyazo.com/f3548888b3dd84a61b086697a6ff1482.jpg" height="80%" width="80%" alt="Admin Set up"/>

</p>

<p>
  
<img src="https://i.gyazo.com/7b06968b7fa1f6889ae87f841bb5e1ba.jpg" height="80%" width="80%" alt="Admin Set up"/>

</p>

<p>
  
<img src="https://i.gyazo.com/4655768404d8ec06ec54c48ed8916ca5.jpg" height="80%" width="80%" alt="Admin Set up"/>

</p>

<p>

You can use all of these accounts to login to Client 1! 
</p>
