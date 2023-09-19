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

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Setup Resource Group</h2>

<h2>Create Resource Group</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/5651d677-6c33-46e9-ad6a-33e19631263f)


</p>
<p>
First, create your Resource Group called "RG-AD". 

<h2>Create VM "DC-1"</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/1d857697-f2ca-43fa-985a-8bdac62a78ba)

Begin by creating a Virtual Machine with the following specifications:

- Name the virtual machine "DC-1" and designate it as a Domain Controller.
- Ensure that the region for your virtual machine matches the resource group's region.
- Choose the "Windows Server 2022" image.
- Set the virtual machine size to have 2 vCPUs.
- Create a username and password that you can easily remember.

![image](https://github.com/kavismith/active-directory/assets/143667203/b9a05dbe-8390-4494-9ac5-1185cc64f0ad)

After configuring the virtual machine details, proceed to the network settings by clicking "Next." In the network configuration, make sure that "DC-1" creates its own virtual network. By following these steps, you will create a Virtual Machine named "DC-1," running Windows Server 2022, with the specified resources and network configuration, ready for use as a Domain Controller.

<h2>Create VM "Client-1"</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/ab6d1884-9871-4cfe-b6fa-68fdcea74f79)

Begin by setting up the virtual machine with the following specifications:

- Name the virtual machine "Client-1."
- Ensure that the chosen region matches the region of both the resource group and the DC-1 virtual machine.
- Select the "Windows 10 Pro" image as the operating system.
- Allocate 2 vCPUs for the Client-1 virtual machine.
- You can either create a different username and password for Client-1, or for practice purposes, use the same username and password that was used for DC-1.

![image](https://github.com/kavismith/active-directory/assets/143667203/be9b9e07-c8f4-42de-92da-47d6c772072a)

Don't forget to check the license box for Client-1 to ensure proper licensing.

![image](https://github.com/kavismith/active-directory/assets/143667203/9bc74283-731e-4251-94e5-f85b5c59eb35)

In the networking configuration, make sure that the virtual network for Client-1 matches that of DC-1, as both virtual machines need to be part of the same network to work together effectively. By following these steps, you will create a virtual machine named "Client-1" with the specified settings, allowing it to work seamlessly with the DC-1 virtual machine on the same network.


</p>
<br />

<h2>Change Domain Controller to Static</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/4a7b2bba-a20b-49b5-994a-3d421c12cdda)
</p>
<p>
To set the Domain Controller's NIC (Network Interface Card) Private IP address to be static, Return to the list of virtual machines, and select "DC-1." Under the virtual machine's settings, locate and select "Networking."

![image](https://github.com/kavismith/active-directory/assets/143667203/00b9c5a9-9f4f-4a77-9c4f-c70c6734d6a4)

Within the "Networking" section, click on the Network Interface (NIC) associated with DC-1.

![image](https://github.com/kavismith/active-directory/assets/143667203/9b3240db-4162-4a2d-935d-43e75ce0a61c)

Once you've accessed the NIC settings, navigate to the "IP Configuration" section.

![image](https://github.com/kavismith/active-directory/assets/143667203/0ebbb141-9ad4-4e50-8cb1-5fe5cce8df7a)

Look for the option labeled "Dynamic IP address" and switch it to "Static."

![image](https://github.com/kavismith/active-directory/assets/143667203/28b30cc4-eeca-425d-b741-9cf4538892b8)

By making this change and setting the IP address to static, you ensure that the IP address assigned to DC-1 will remain constant, even if you shut down the computer for an extended period. This stability is important for the proper functioning of a Domain Controller within a network environment.
</p>
<br />

<h2>Ensure Connectivity between Client-1 and Domain Controller</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/d3e5c3fd-0891-4934-98dc-eb63c24da259)

Connect to Client-1 using Remote Desktop.

![image](https://github.com/kavismith/active-directory/assets/143667203/f7aa72dd-604c-4f95-b710-a2add9d4145d)

Login into Client-1 through Remote Desktop

![image](https://github.com/kavismith/active-directory/assets/143667203/e8a7a052-cb89-41ed-931e-ecd04d7d1e61)

While the Remote Desktop connection to Client-1 is establishing, you need to obtain the Domain Controller's private IP address. To do this, go back to the list of virtual machines, select the Domain Controller (DC-1), and copy its private IP address.

![image](https://github.com/kavismith/active-directory/assets/143667203/16d4cead-b304-411c-bfb0-b9282e35b610)

Once you have DC-1's private IP address, open the Command Prompt on Client-1 by searching for it in the search bar.

![image](https://github.com/kavismith/active-directory/assets/143667203/86a003d3-a381-4247-b09f-1c1617d95b7b)
</p>
<p>
 Ping DC-1's private IP address using the command: ping -t <ip address> (where <ip address> is the IP address you copied). This command will perform a perpetual ping, although it's likely to fail initially due to the DC-1 firewall blocking ICMP traffic.

![image](https://github.com/kavismith/active-directory/assets/143667203/296c3c17-8c9d-4ccf-822b-f728c8ac4e76)


Log into the Domain Controller via remote desktop***

![image](https://github.com/kavismith/active-directory/assets/143667203/ee2fed51-2202-4bf1-8b93-a4590e45b328)

To enable ICMPv4 on the local Windows Firewall, search for "wf.msc" in the search bar on DC-1 and select "Microsoft Common Console Document."

![image](https://github.com/kavismith/active-directory/assets/143667203/4d2f3aa0-a890-43de-8ef0-f0cfe75874a0)

Once Windows Firewall with Advanced Security is open, select "Inbound Rules."

![image](https://github.com/kavismith/active-directory/assets/143667203/dd7baca7-0bb0-4ae9-b111-c420394a2870)

In the top section, you'll find different groups; click on "Protocol" to organize all protocols. Locate "Core Networking Destination," right-click on it, and select "Enable Rule."

![image](https://github.com/kavismith/active-directory/assets/143667203/040603f4-3fe0-4f07-bb46-f82f9e0b5a24)

Return to Client-1, where you initiated the perpetual ping earlier. You should now see Client-1 successfully pinging DC-1, and you'll receive ping responses as the ICMP traffic is now allowed through the firewall on DC-1.
</p>
<br />


<h2>Install Active Directory</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/7dbe73e3-8e99-45bd-ad85-fc4c148e86f9)
Log into Domain Control if you are not already logged in. Go to the search bar and search for Server Manager if it's not already loaded from when you first logged into the Domain Controller. Next, select "Add Roles and Features."

![image](https://github.com/kavismith/active-directory/assets/143667203/2f19d350-60da-46fc-bc06-9f7d0276daec)
Click "Next" for the install type section. When you get to the server selection, ensure that the server pool says "DC-1 private IP address," and then click "Next." For the server role, make sure to click on "Active Directory Domain Services" 

![image](https://github.com/kavismith/active-directory/assets/143667203/6e76ae91-c288-4953-961d-7b275c1dab80)
and click on "Add Features. Select "Next" until you reach the confirmation section. ***

![image](https://github.com/kavismith/active-directory/assets/143667203/be95e565-3dc9-476a-b194-d9ee8f27afb8)
Once the confirmation is completed, click the "Install" button.
</p>
<p>

</p>
<br />


![image](https://github.com/kavismith/active-directory/assets/143667203/f4481222-29b9-4cae-ac56-7b73af0accab)

Begin by clicking on the flag icon located next to the exclamation mark in the top right-hand corner of the screen. From the menu that appears, select "Promote this server to a domain controller." This step represents the final stage of the Active Directory installation, during which the server will be transformed into a Domain Controller. In the "Deployment Configuration" section, opt to "Add a new forest." Provide the desired name for the private domain, which should be "kavidomain.com."

![image](https://github.com/kavismith/active-directory/assets/143667203/9df67453-9c8e-4650-9746-948a977c62cc)

Set the required password for this process.

![image](https://github.com/kavismith/active-directory/assets/143667203/537979bd-dfb5-45d4-96b6-15e8f67e79e6)

Continue by clicking "Next" until you reach the "Install" section. Finally, press the "Install" button to initiate the Active Directory installation. By following these steps, you will successfully configure the server as a Domain Controller, completing the installation of Active Directory with the specified domain name, "kavidomain.com."
</p>
<p>

</p>
<br />


![image](https://github.com/kavismith/active-directory/assets/143667203/157d7772-0e62-4491-b820-3c9efec8b2bf)

</p>
<p>
After you have successfully installed Active Directory, your Remote Desktop will restart, and you will need to sign back in using the credentials "kavidomain.com/labuser" and the password you previously set.

</p>
<br />


![image](https://github.com/kavismith/active-directory/assets/143667203/5ceeb292-3d05-4652-a136-e99569ef9ef8)
</p>
<p>

Once you have successfully signed in, Server Manager will automatically appear on the screen; however, if it does not, you can simply type "Server Manager" into the search bar to access it. From there, select "Tools" within Server Manager, and then choose "Active Directory Users and Computers." Alternatively, you can also click the "Start" menu and search for "Active Directory" to access it.
</p>
<br />

<h2>Create An Admin and Normal User Account In AD</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/ca906a93-3ede-4f4c-afb3-dfcb5bb385f1)
We will create several Organizational Units (OUs) that function as folders within Active Directory. To do this, right-click on your domain, choose "New," and then select "Organizational Unit."

![image](https://github.com/kavismith/active-directory/assets/143667203/dbfd57ef-3da2-435e-8628-259eda1ed6b1)

Create one Organizational Units named "_EMPLOYEES" 

![image](https://github.com/kavismith/active-directory/assets/143667203/122816c9-601d-473d-a147-cc52dc86be51)

Create two Organizational Units named "_ADMINS" 

![image](https://github.com/kavismith/active-directory/assets/143667203/1c5b4a78-b44f-4333-b4a9-d1d8a88c40b1)

</p>
<p>

</p>
<br />

![image](https://github.com/kavismith/active-directory/assets/143667203/63618c18-0139-4ee4-b902-6d46e8e9158f)

Navigate to the "_ADMINS" Organizational Unit (OU). Right-click on the empty space within the "_ADMINS" OU and select "New," then click on "User."

![image](https://github.com/kavismith/active-directory/assets/143667203/089d8416-833b-4514-a1f3-7442c1281382)

Provide the following user details:
- First Name: Tiffany
- Last Name: Smith
- Username: tiffany_admin

![image](https://github.com/kavismith/active-directory/assets/143667203/c568d1a9-53ed-4b36-8d34-161c609796cd)

Set a password for the user account.


</p>
<p>

</p>
<br />

![image](https://github.com/kavismith/active-directory/assets/143667203/ec0e2c2e-f900-4e56-9190-0c7fc93e89c9)

Next, we will add the "Tiffany Smith" user to the admin group, granting administrative privileges within the domain. Right-click on the "Tiffany Smith" user account. Select "Properties" from the context menu.

![image](https://github.com/kavismith/active-directory/assets/143667203/83f9defe-9b60-4504-b5ca-511d95c3df01)

In the "Properties" window, navigate to the "Member Of" tab. Click the "Add" button to add the user to a group. In the "Enter the object names to select" field, type "Domain" and then click the "Check Names" button to verify and select "Domain Admin."

![image](https://github.com/kavismith/active-directory/assets/143667203/ce3d8b59-8df0-4647-b55b-1b7d688e46ab)

Click "Apply" and then "OK" to save the changes. By following these steps, you will have added the "Tiffany Smith" user to the "Domain Admin" group, effectively granting administrative privileges within the domain.
</p>
<p>

</p>
<br />

![image](https://github.com/kavismith/active-directory/assets/143667203/defa1ebc-d424-4b4d-8e5c-2ead1a959809)

Log out of the "labuser" account. Log in using the "tiffany_admin" account.

![image](https://github.com/kavismith/active-directory/assets/143667203/4832fc75-7ee3-4603-acdf-7245fd364489)

To ensure you are connected correctly, click the "Start" button and search for "CMD" or "Command Prompt." Select "Command Prompt" from the search results. In the Command Prompt window, type in "whoami" and press Enter. You should see the domain to which you are connected. Next, type in "hostname" and press Enter. It should display "DC-1."
</p>
<p>

<h2>Join Client-1 To Your Domain(kavidomain.com)</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/e479e9f4-461b-43cf-9e4e-b2c0546b8a95)

In the Azure Portal, configure Client-1's DNS settings to use DC-1's private IP address. First, obtain DC-1's private IP address. Then, navigate to the "Virtual Machines" section, select "Client-1," access its network settings, choose the relevant Network Interface (NIC).

![image](https://github.com/kavismith/active-directory/assets/143667203/1bd8b21b-a021-4563-b66e-220554635553)

Click on "DNS servers 

![image](https://github.com/kavismith/active-directory/assets/143667203/146186cf-8883-4fad-b76c-9effc3e1b5cb)

Set the DNS server to "Custom," paste DC-1's private IP address, save the changes

![image](https://github.com/kavismith/active-directory/assets/143667203/5c2c227e-a3a0-4b54-818d-697ab983d3a7)

And finally, restart Client-1 from the Azure Portal.

</p>
<p>
</p>
<br />

![image](https://github.com/kavismith/active-directory/assets/143667203/796f4d73-f0a8-46bb-bc6b-25c275eed164)

Sign back into Client-1 using its new DNS settings, logging in as "labuser."

![image](https://github.com/kavismith/active-directory/assets/143667203/736eec5c-a1d0-4e92-837a-7543ff5768a2)

Right-click on the "Start" button and select "System."

![image](https://github.com/kavismith/active-directory/assets/143667203/3a283446-83aa-4a83-a45a-002c2d0d1ffd)

 In the System settings, choose "Rename this PC (advanced)."
 
![image](https://github.com/kavismith/active-directory/assets/143667203/d6f830e4-6911-4dcf-8089-9a8ab5bfdb90)

Select the "Domain" option and enter your domain name as "kavidomain.com." Click "OK." A new login window will appear for changing the domain name. Enter the domain name and the admin login credentials you set up in DC-1. In this practice, it's "kavidomain.com\tiffany_admin," and enter the password set in DC-1. Click "OK," and Client-1 will restart.

</p>
<p>

<h2>Set Up Remote Desktop For Non-Administrative Users On Client-1</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/2deabe7f-7ae5-4dd4-bf75-b6730d759709)

Sign back into Client-1 using the domain name "kavidomain.com\tiffany_admin" and the password you previously created.

![image](https://github.com/kavismith/active-directory/assets/143667203/94ba7cad-5fa4-4317-8117-9af09ddaaa7d)

Right-click on the "Start" button and select "System."

![image](https://github.com/kavismith/active-directory/assets/143667203/bc5fde52-8b8b-4abd-8c28-d0f6ddf27e86)

Click on "Remote Desktop" in the system settings.

![image](https://github.com/kavismith/active-directory/assets/143667203/39bbfa60-124a-49b2-98cb-db93ec5e46d8)

Next, click on "Select users that can remotely access this PC." Select the "Add" button to add users who should have remote access. Enter "Domain Users" and click "Check Names." You will see that "Domain Users" has been selected. Click "OK."

![image](https://github.com/kavismith/active-directory/assets/143667203/0f4a1d1d-ac42-43de-8b7b-f83364c7c731)

Now, users from the domain will have permission to remotely access Client-1 using Remote Desktop.
</p>
<p>

<h2>Create Different Users & Log In With One User</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/2a03bd60-416f-413b-b90d-ebe4aeb74025)

Log back into DC-1 as "tiffany_admin." Open PowerShell ISE as an administrator.

![image](https://github.com/kavismith/active-directory/assets/143667203/5f710d70-76e7-4b54-b99f-f13322dd95b8)

Within PowerShell ISE, create a new script file and paste the content of the script into it. In the top left-hand corner of PowerShell ISE, you'll find a document icon with a star on it. Click on it to save the script.

![image](https://github.com/kavismith/active-directory/assets/143667203/986c6644-7b65-4338-91ae-d1eeec5742a5)

Go to the script and right click and copy.

![image](https://github.com/kavismith/active-directory/assets/143667203/2ef33021-8905-417b-875c-792626f4d04c)

After saving the script, click the green arrow button, which represents "Run." This will execute the script.

![image](https://github.com/kavismith/active-directory/assets/143667203/9f7d3903-dd4d-4de1-bb7f-91f8a3151c0e)

The script you're running will create accounts for 10,000 users with random names in the "_EMPLOYEES" folder. After clicking "Run" in PowerShell ISE, you should see the random names of all the users generated by the script. 

</p>
<p>


![image](https://github.com/kavismith/active-directory/assets/143667203/182898f6-71a8-419e-b3d4-9c536fa63df3)

Open "Active Directory Users and Computers." Select your domain. Click on the folder "_EMPLOYEES," and you will see the list of user names that were generated.

![image](https://github.com/kavismith/active-directory/assets/143667203/3e1a3be6-2016-413b-9118-0472b1c8bbbf)

To test the user accounts, try logging into Client-1 using one of the user names. Keep in mind that all user profiles have a default password of "Password1."

![image](https://github.com/kavismith/active-directory/assets/143667203/793f8e66-a795-4666-8d4a-7958b1001bed)

Select a random user, such as "cem.tecox," and attempt to sign in using the password "Password1."

![image](https://github.com/kavismith/active-directory/assets/143667203/7ee5ebbe-d7d7-419b-b975-4105593246b3)

 If successful, you will log in.
 
![image](https://github.com/kavismith/active-directory/assets/143667203/5d2cc212-5c3a-4f52-84ba-1d4dd04d1ad2)

To verify that you are logged in correctly, open the Command Prompt and type in "whoami." It should display "kavidomain\cem.tecox," confirming that you are logged in under that user account.
</p>
<p>

