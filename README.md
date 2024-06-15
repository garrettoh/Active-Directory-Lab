# Active Directory Lab (WIP)



## Objective
The objective of creating an Active Directory lab is to provide a controlled environment for learning and experimenting with Active Directory configurations, security practices, and administrative tasks. This lab allowed me to gain hands-on experience with directory services, improve my understanding of user and group management, and analyze logs for security events, ultimately enhancing my skills in managing and securing an Active Directory infrastructure.

### Skills Learned
- User and Group Management: Create, modify, and manage user accounts and groups within Active Directory.
- Active Directory Configuration: Set up and configure Active Directory Domain Services, including domain controllers and organizational units.
- Group Policy Management: Implement and manage Group Policies to control user and computer settings across the network.
- Authentication and Authorization: Understand and configure authentication mechanisms and authorization settings to secure resources.
- Backup and Recovery: Perform backup and recovery operations to ensure the availability and integrity of Active Directory data.
- Troubleshooting: Diagnose and resolve common Active Directory issues to maintain a healthy and functional directory service.

### Tools Used
- RSAT to Remotely manage the AD Enviornment from the virtual client machine.
- Powershell to use a script to create the fake user accounts.
- AD Users and Computers to manage user accounts, groups and OUs.
- GPMC to create and edit group policies across the virtual network. 

## Steps
I followed along a youtube video to get the virtual AD enviornment set-up 
### Windows server steps!
 Step 1: Network Diagram ![](https://i.imgur.com/a8KCLM9.png)
 
 Step 2: Download and configure the Virtual Machine with Windows Server 2019 installed.  <img src="Pictures/Windows server Internal adapter.PNG" class="img-responsive" alt=""> <img src="Pictures/Windows server NAT adapter.PNG" class="img-responsive" alt="">
 
 Step 3: Add the server role AD DS (Active Directory and Domain Services.<img src="Pictures/addroles.PNG" class="img-responsive" alt=""> <img src="Pictures/AD DS.PNG" class="img-responsive" alt="">
 
 Step: 4: Go to the Dashboard and click on manage and Promote the server to a DC (Domain controller). <img src="Pictures/domain controller setup.PNG" class="img-responsive" alt="">
 
 Step 5: Open the ADDS Configuration wizard and add a new forest we will call ours mydomain.com.<img src="Pictures/mydomain.PNG" class="img-responsive" alt="">
 
Step 6: We should see that we are now user: MYDOMAIN\Administrator and if we try and login to another user it will be under mydomain.<img src="Pictures/under a domain.PNG" class="img-responsive" alt="">

Step 7: I created my own user in Active Directory users and Computers and made an organizational unit called _ADMINS.<img src="Pictures/create user in OU.PNG" class="img-responsive" alt="">

Step 8: If we want to escalate the privilege of the newly created user we will right click user go properties and then under the "Member Of" tab we will click add and then to add the domain admin privilege we will type in "domain admins" under the field "Enter the object names to select." 
<img src="Pictures/a-graeseadmin.PNG" class="img-responsive" alt="">

Step 9: We now should be able to login to the other user tab on our windows virtual server under the newly created user. <img src="Pictures/login1.PNG" class="img-responsive" alt="">

Step 10: Go to network adapter settings and give the internal adapter a static IP to act as the server IP for the client machine with a DNS Loopback address so that the machine can use itself to resolve the domain names. (127.0.0.1)<img src="Pictures/setting up internal adapter.PNG" class="img-responsive" alt="">

Step 11 (the important one): We are going to set up routing, so were going to go back to add roles and add in remote access and then under the remote access tab were going to click the routing option this will allow us to use the virtual network adapters to actually act as a server for our other windows virtual machine.
<img src="Pictures/setting up routing.PNG" class="img-responsive" alt="">


Configuration > After installing the routing service we are going to og to our DC (local) server and were going to configure it with NAT so that it can act as a NAT adapter to our windows client VM.<img src="Pictures/routing.PNG" class="img-responsive" alt=""> 

After this you will be prompted to choose a network adapter and were going to click the one whos IP we haven't configured or the one that says (DHCP)<img src="Pictures/internet interface.PNG" class="img-responsive" alt=""> 

After that we should be able to right click on our domain controller and click refresh and things should be working. :) ! 
<img src="Pictures/routing-success.PNG" class="img-responsive" alt="">




Step 12: We're now going to add "DHCP Server" as a role so that we can use the windows server as a DHCP server to our internal virtual adapter I plan to make a 100-200 range. (I put mine as a /24 network.) After setting your IP you'll need to set the default gateway now under DHCP we should be able to refresh and see that our dhcp scope has been set. <img src="Pictures/dhcp.PNG" class="img-responsive" alt="">
<img src="Pictures/100-200.PNG" class="img-responsive" alt="">
<img src="Pictures/defaultgateway.PNG" class="img-responsive" alt="">

<img src="Pictures/Ipv4.PNG" class="img-responsive" alt="">


Step 13: WE HAVE NOW COMPLETED THE BASIC SETUP FOR OUR AD ENVIORNMENT !




Step 14: (optional) : From the video he had a powershell script that I ran to create around 1000 user accounts with random names so that we can have fake users to simulate a somewhat realistic enviornment for our lab.<img src="Pictures/pshell script.PNG" class="img-responsive" alt="">

Step 15 (optional):Mess around with some of the features! I messed around with some of the settings and features I used GPMMC to change things in the registries for certain groups to not allow them access to files or folders. I also created shared folders so that every computer in the network could access certain important files. <img src="Pictures/change desktop group policy.PNG" class="img-responsive" alt="">

Step 16 (optional): If your windows machine isn't showing the group policy changes you made through gpmmc you can open cmd as admin and type in gpupdate /force which will force all of the computers to update the group policy adhering to the rules that you have set. <img src="Pictures/gpupdate force.PNG" class="img-responsive" alt="">

### Windows Client Steps 

Step 1: We now need to setup the internal network settings in the virtual machine for our windows client do so by changing network adapter from nat to internal network. <img src="Pictures/inet windows client.PNG" class="img-responsive" alt="">

Step 2: Make sure that you are connected through the windows server to the internet by opening cmd and typing ipconfig /all. <img src="Pictures/windows ipconfig.PNG" class="img-responsive" alt="">

Step 3: Join the domain, Go to system poroperties and where it shows your computer name click change workgroup settings and under member of click Domain and then type in the domain name, In my case it would be mydomain.com. <img src="Pictures/change client to be inside of domain.PNG" class="img-responsive" alt="">

Step 4: you now should be able to login under any account registered within the domain.

<img src="Pictures/domain successful.PNG" class="img-responsive" alt="">


Step 5 Here is me using a random account created by the powershell script- <img src="Pictures/abonivita.PNG" class="img-responsive" alt="">



