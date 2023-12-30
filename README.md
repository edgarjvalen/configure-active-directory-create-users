# install-active-directory-create-users

<p align="center">
<img src="https://i.imgur.com/Q3YpjDD.png" height="40%" width="40%"alt="AD Logo"/>
</p>

<h1>Installing Active Directory and Creating Users</h1>
This is a tutorial on how to install Active Directory using Azure Virtual Machines. If you need to learn how to create Resource groups or Virtual Machines in Azure, go to the following links for my tutorials.

https://github.com/jarrettm98/azure-crash-course

https://github.com/jarrettm98/azure-compute-and-networking

<h2>Technology Used</h2>

- Microsoft Azure
- Remote Desktop
- Active Directory

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10

<h2>Prerequisites</h2>

- Create Domain Controller and Client Virtual Machines
- Enable ICMPv4 rules with Windows Defender

<h2>Step 1: Setup</h2>

First, using Azure, create a Resource Group. Now, create 2 Virtual Machines(VMs). One will be the Domain Controller and the other will be the Client. To create the Domain Controller, give the VM a name as well as assign it to the Resource Group created before. 

<img src="https://i.imgur.com/uYfHMQG.png" height="70%" width="70%" alt="9"/><br />

Now for the image use Windows Server 2022. It is recommended for the size to use 2 vcpus.

<img src="https://i.imgur.com/FNoA7m0.png" height="70%" width="70%" alt="9"/><br />

Give the admin log in credentials that can be remembered or just write them down in notepad. Now, click "Next" until reaching the "Networking" tab. Take note of the "Virtual Network" created. This will be important when creating the Client VM. Check the box under Licensing then "Review and create" the VM.

<img src="https://i.imgur.com/NxXFK16.png" height="60%" width="60%" alt="9"/><br />

Now, create the Client VM. Same thing as the first one except the image should be using Windows 10.

<img src="https://i.imgur.com/2PvUCJN.png" height="70%" width="70%" alt="9"/><br />

Click, Next until reaching the Networking tab. Make sure the Virtual Network is the same as the Domain Controller. Now Review and create.

Now it's time to set the Domain Controller's NIC Private IP to static. Go to the Domain Controller and click on the "Networking" tab. Next, click on the "Network Interface."

<img src="https://i.imgur.com/6W2WZTA.png" height="50%" width="50%" alt="9"/><br />

Now, go the "IP configurations" tab and click on the IP configuration. 

<img src="https://i.imgur.com/0R53K7r.png" height="40%" width="40%" alt="9"/><br />

Now, change the Allocation from "Dynamic" to "Static." Then click Save.

<img src="https://i.imgur.com/JAGBZtk.png" height="40%" width="40%" alt="9"/><br />

Now, using the user and password created before, login to the Client with it's IP address in Remote Desktop Connection. 

<img src="https://i.imgur.com/28TrmKg.png" height="45%" width="45%" alt="9"/><br />

Now, using Command Prompt, ping the Domain Controller with it's Private IP Address. Type in "ping (Your DC Private IP) -t" to perpetually ping. For now it will time out.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/5acef08a-c31f-4261-9f9a-218c28777b35)

Next its time to enable ICMPv4. First, login to the Domain Controller VM then open "Windows Defender Firewall with Advanced Security" 

<img src="https://i.imgur.com/bYkAEwk.png" height="85%" width="85%" alt="9"/><br />

Click on "Inbound Rules" and Sort by "Protocol". Look for the rules with "Core Networking Diagnostics - ICMP Echo Request(ICMPv4-In)" There will be two of them (Both on the bottom of the image below)

<img src="https://i.imgur.com/dmGfcyH.png" height="85%" width="85%" alt="9"/><br />

Right-click and Enable both rules. Now go back to the Client VM and check on the command prompt. It should now be properly pinging the Domain Controller.

<img src="https://i.imgur.com/ENb2KyF.png" height="50%" width="50%" alt="9"/><br />

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/ccd209de-9c2a-4eeb-a5d8-0b2addd974e2)


<h2>Step 2: Installing Active Directory</h2>

Now time to Install Active Directory. Go to the Domain Controller. In "Server Manager" click on "Add roles and features."

<img src="https://i.imgur.com/0BcdJpW.png" height="80%" width="80%" alt="9"/><br />

Click "Next" until reaching the "Server Roles" section. Now, check the box next to "Active Directory Domain Services" then "Add Features."

<img src="https://i.imgur.com/K5oTmkD.png" height="80%" width="80%" alt="9"/><br />

Click Next until reaching the "Confirmation" tab then click "Install." It may take a while to install. Once it says "Configuration required. Installation succeeded on (Your DC name here). Click "Close"

Towards the top-right corner of the Server Manager window, there will be a flag and a yellow triangle with a "!" symbol. Click on that then "Promote the server to a domain controller"

<img src="https://i.imgur.com/D8p1wU9.png" height="40%" width="40%" alt="9"/><br />

A window will pop up for a Configuration Wizard. Check the bubble next to "Add a new forest" then give it a domain (Example in image below) Click next.

<img src="https://i.imgur.com/BefHqfW.png" height="80%" width="80%" alt="9"/><br />

Give it a DSRM password (Required but wont be used in this tutorial) Click next.

<img src="https://i.imgur.com/TYXfTrJ.png" height="80%" width="80%" alt="9"/><br />

Next, the NETBIOS domain will be made. This may take a moment. Once it is made, Click next until reaching the "Prerequisites Check" tab. This process will take a moment. Now click "Install"

After Installing the VM will reboot. Once it is rebooted, Log back into the Domain Controller with the domain name and the username. Example below.

<img src="https://i.imgur.com/nT5uFiT.png" height="40%" width="40%" alt="9"/><br />

<h2>Step 3: Creating a Domain Admin</h2>

Once logged in, using Server Manager click on tools in the top-right corner. Next click on "Active Directory Users and Computers."

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/8fe68c25-2b8d-44de-9ebe-4fbc4ff1df84)

In the Domain container, create a new "Organizational Unit"

<img src="https://i.imgur.com/9DGmBMS.png" height="80%" width="80%" alt="9"/><br />

Name the OU "_EMPLOYEES", then click OK. Next create another OU and name it "_ADMINS."

In the "_EMPLOYEES" tab, create a new "User"

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/eb70e5e0-0729-4ba7-af6e-a20de74b1e85)

Name this anything. Just remember the user and password. Uncheck the box that is next to "User must change password at next logon." This wont be necessary. Click next then click Finish.

<img src="https://i.imgur.com/VIJLhXf.png" height="50%" width="50%" alt="9"/><br />
<img src="https://i.imgur.com/slz2BBbf.png" height="50%" width="50%" alt="9"/><br />

Now add this user to the "Domain Admins" security group. Right-click on the user create, then click "Properties." Click on the "Members of" tab, then click "Add." 

<img src="https://i.imgur.com/3MooGCr.png" height="80%" width="80%" alt="9"/><br />

Type "domain" in the box under "Enter the object names to select:" then click "Check Names" 

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/a0d8e4e1-30df-4ed4-8bde-15e08577f71c)

Choose the "Domain Admins" option then click OK

<img src="https://i.imgur.com/eHOKSWT.png" height="80%" width="80%" alt="9"/><br />

Now, click "Apply." The user has successfully been added to the Domain Admins security group. Click OK. Now logout of the Domain controller and re-log as the user just created.

<img src="https://i.imgur.com/oECi1Rd.png" height="50%" width="50%" alt="9"/><br />

<h2>Step 4: Setting Client DNS Settings to Domain Controller Private IP Address</h2>

First, on Azure go to the Client VM. Next, go to the Networking tab and click on the Network Interface.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/b3b0df1b-dea6-488c-9ed5-b799fea5cb1f)

Next, go to the "DNS Servers tab and create a custom DNS Server. Add a custom server using the Domain Controller's Private IP address. Example Below.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/a3ea6e36-b4f5-4d53-876d-972bb532a6d8)

Now click "Save" Next go back to the Client and click "Restart in the "Overview" tab 

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/e6b59225-f50e-4d47-b972-d1d5837ec222)

Once the Client is restarted, login to the client with Remote Desktop as the original admin created with the VM.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/041739ae-3f18-4529-b78a-0c536d63e4ce)

Once logged in go to Settings>System>About and click on "Rename this PC(advanced)"

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/2b814352-f7aa-4f26-9aae-e0920e321f1b)

Now Click on "Change..."

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/0927adfe-69e8-4183-bdd3-48d547948f47)

Now check the bubble next to "Domain" then type in the domain name (Your own domain name)

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/ca0ee0d4-a1ec-4b76-9a5b-1dbbd1d2d634)

There should be window that pops up for a login. Use the admin previously created to login. Example below:

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/42dbb73b-9038-4ef6-a19b-8124bb2822eb)

The VM will now restart after a short period.

<h2>Step 5: Setting up Remote Connection for Domain Users</h2>

Now, log into the Domain Controller. Go back to Server Manager>Tools>Active Directory Users and Computers. Under the Domain container, go to the "Computers" tab. It should show that the client has been added to the list.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/ffaaa5bd-4f80-4f5a-a664-6ac578783ccc)

Now, log into the Client as the admin user created and go to System Settings>Remote Desktop. Click on "Select users that can remotely access this PC" Next click Add.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/6e69f3bb-be67-42b2-aea0-47ed4ee39822)

In the box at the bottom, type in "Domain Users" and Check Names. Next click OK.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/61d6da6a-b8e0-4c45-953f-aad9ae0b582b)

<h2>Step 6: Creating Domain Users</h2>

In the Domain Controller, open "Windows PowerShell ISE." Make sure to open it as Administrator. Click "New File" in the top left corner.

![powershell ise new file](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/e11f1de8-276e-4664-827c-caf7cafec0b1)

Next, copy and paste the script from this link into the text editor. 

https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1

Reccomendation: Before running it, change "NUMBER_OF_ACCOUNTS_TO_CREATE" from 10000 to 100.

Now, click the Run button to run the script. Example below:

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/7cd52fba-92de-44af-933d-87b8aa7e5e0c)

This will start creating domain users with usernames and passwords (The Password for these users will be "Password1") 

Go to Server Manager>Tools>Active Directory Users and Computers. Under the "_EMPLOYEES" tab, look at all of the users created from the script.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/eec199e2-2e9e-436a-a210-d835cb232f1d)

These names are all randomly generated. Choose one and log into the Client VM with the username it is assigned. (Remember the password is "Password1)

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/ef04a5bb-0f60-42e2-ae99-78ef7c229af8)

Congrats! You completed this tutorial!

Click on this link for a tutorial on giving users files shares and permissions.

https://github.com/jarrettm98/network-file-sharing-and-permissions-with-active-directory
