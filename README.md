<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

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

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Setup Resource Group</h2>

<h2>Create Resource Group</h2>
![image](https://github.com/kavismith/active-directory/assets/143667203/5651d677-6c33-46e9-ad6a-33e19631263f)


</p>
<p>
First, create your Resource droug called "RG-AD". 

![image](https://github.com/kavismith/active-directory/assets/143667203/1d857697-f2ca-43fa-985a-8bdac62a78ba)
![image](https://github.com/kavismith/active-directory/assets/143667203/b9a05dbe-8390-4494-9ac5-1185cc64f0ad)

<h2>Create VM "DC-1"</h2>
Then create a Virtual machine Named "DC-1"(Domain Controller), make sure that your virtual machines region is the same as the resource group. The image needs to be "Windows server 2022", the size should be 2 vcpus and then create you a username and password that you can remember. Go to netwok and make sure DC-1 create its own virtual netork.

![image](https://github.com/kavismith/active-directory/assets/143667203/ab6d1884-9871-4cfe-b6fa-68fdcea74f79)
![image](https://github.com/kavismith/active-directory/assets/143667203/be9b9e07-c8f4-42de-92da-47d6c772072a)
![image](https://github.com/kavismith/active-directory/assets/143667203/9bc74283-731e-4251-94e5-f85b5c59eb35)

Last, create another virtual machine named "Client-1", with the same region as the resource group and DC-1 virtual machine. The image needs to be "Windows 10 pro" and  size for Client-1 should be 2 vcpu. You can create a different username and password but for this practice, the same uusername and password from DC-1 was used for Client-1 virtual Machine . Don't forget to chech the license box for Client-1. Go to networking and make sure the virtual network for Client-1 is DC-1 because they need to be working on the same network.
</p>
<br />

<h2>Change Domain Controller to Static</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/4a7b2bba-a20b-49b5-994a-3d421c12cdda)
</p>
<p>
Set Domain Dontroller's NIC Private IP address to be static. GO back to virtual machines and select DC-1. Select networking under settings.

![image](https://github.com/kavismith/active-directory/assets/143667203/00b9c5a9-9f4f-4a77-9c4f-c70c6734d6a4)

Click on the Network Interface(NIC)

![image](https://github.com/kavismith/active-directory/assets/143667203/9b3240db-4162-4a2d-935d-43e75ce0a61c)

Then select IP configuration
![image](https://github.com/kavismith/active-directory/assets/143667203/2745c1ba-2ee3-460b-9ebc-6bc4c1ab1d26)
![image](https://github.com/kavismith/active-directory/assets/143667203/28b30cc4-eeca-425d-b741-9cf4538892b8)

Click on the Dynamic IP addess and switch it to static. By changing the ip address to static, the ip address will never change even if you cut off the computer for a long time. 
</p>
<br />
<h2>Ensure Connectivity between Client-1 and Domain Controller</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/d3e5c3fd-0891-4934-98dc-eb63c24da259)
![image](https://github.com/kavismith/active-directory/assets/143667203/f7aa72dd-604c-4f95-b710-a2add9d4145d)


Login into Client-1 through remote desktop

![image](https://github.com/kavismith/active-directory/assets/143667203/16d4cead-b304-411c-bfb0-b9282e35b610)
In sthe Search bar, search for Command Propmt

![image](https://github.com/kavismith/active-directory/assets/143667203/e8a7a052-cb89-41ed-931e-ecd04d7d1e61)

  While CLient-1 is Remote desktop is connecting, get the Domain Controller's pprivate IP address taht we need to ping. Go to virtual machine and select the Domain Controller adn copy DC-1 privvate IP address

![image](https://github.com/kavismith/active-directory/assets/143667203/86a003d3-a381-4247-b09f-1c1617d95b7b)
</p>
<p>
 Then Ping DC-1's private IP address with "Ping -t <ip address>(perpetual ping), which will probably fail because DC-1 firewall is blocking ICMP traffic.

  
![image](https://github.com/kavismith/active-directory/assets/143667203/1abb093f-ba0e-4e73-9d3a-01d4b90a3481)
![image](https://github.com/kavismith/active-directory/assets/143667203/296c3c17-8c9d-4ccf-822b-f728c8ac4e76)


Log into the Domain Controller

![image](https://github.com/kavismith/active-directory/assets/143667203/ee2fed51-2202-4bf1-8b93-a4590e45b328)

Enable ICMPv4 on the local windows firewall by searching in the search bar fpor wf.msc and then select Microsoft Common Console Document. 

![image](https://github.com/kavismith/active-directory/assets/143667203/4d2f3aa0-a890-43de-8ef0-f0cfe75874a0)
Once windows firewall Defender is open select Inbound Rules.

![image](https://github.com/kavismith/active-directory/assets/143667203/dd7baca7-0bb0-4ae9-b111-c420394a2870)

At the top ypu will see different groups and click on Protocol to oragnice all protocols. Select all Core Networking Destination then right click and select Enable Rules


![image](https://github.com/kavismith/active-directory/assets/143667203/040603f4-3fe0-4f07-bb46-f82f9e0b5a24)

GO back to Client-1, ypu should see client-1 ping to DC-1, start to reply.
</p>
<br />


<h2>Install Active Directory</h2>

![image](https://github.com/kavismith/active-directory/assets/143667203/7dbe73e3-8e99-45bd-ad85-fc4c148e86f9)
![image](https://github.com/kavismith/active-directory/assets/143667203/2f19d350-60da-46fc-bc06-9f7d0276daec)
![image](https://github.com/kavismith/active-directory/assets/143667203/6e76ae91-c288-4953-961d-7b275c1dab80)
![image](https://github.com/kavismith/active-directory/assets/143667203/be95e565-3dc9-476a-b194-d9ee8f27afb8)



</p>
<p>
Log into Domain Control if you are not already logged in. GO to search and search Server Manager if its not already loaded from when you first logged into Domanin Controller. Then select Add rolles and Features.Click next, for the install type section click next, and when you get to server selection make sure the server pool say DC-1 private ip address and then click next. For the server role make sure to click on Active Directory Domain Services and click on add features. Select next until you get to confirmation section and then click install an now Achtive Directory is installing
</p>
<br />


![image](https://github.com/kavismith/active-directory/assets/143667203/4798da25-2971-4092-8686-8716b30cebd1)

![image](https://github.com/kavismith/active-directory/assets/143667203/41f3e910-c3d4-4121-8dc1-051370f4a611)
</p>
<p>
click on the flag next to the exclamation mark to you right hand corner and select promote this server to a domain controller. This is the final steps to installing active directory and the server will turn into a Domain Controller. Select add a new forest in the Deployment Configuration section. Now, name the private domain to kavidomain.com. Set your password. Click next until you get to the install section, then press install. 

</p>
<br />


![image](https://github.com/kavismith/active-directory/assets/143667203/157d7772-0e62-4491-b820-3c9efec8b2bf)

</p>
<p>
Afte ryou have successfully install Active Directory, your Remote desktop will restart. You would then need to sign back in using kavidomain.com/labuser and the password you set.

</p>
<br />


![image](https://github.com/kavismith/active-directory/assets/143667203/5ceeb292-3d05-4652-a136-e99569ef9ef8)
</p>
<p>

Once you have successfully signed in, Server Manager will automatically display on the screen but if it do not, just type in the search bar server manager and you can pull it up that way. Select tools from the  Server Manager and then select Actove Directory Users and Computers or you can click start and search for Active Directory that way as well.

</p>
<br />

![image](https://github.com/kavismith/active-directory/assets/143667203/ca906a93-3ede-4f4c-afb3-dfcb5bb385f1)
![image](https://github.com/kavismith/active-directory/assets/143667203/dbfd57ef-3da2-435e-8628-259eda1ed6b1)
![image](https://github.com/kavismith/active-directory/assets/143667203/122816c9-601d-473d-a147-cc52dc86be51)
![image](https://github.com/kavismith/active-directory/assets/143667203/1c5b4a78-b44f-4333-b4a9-d1d8a88c40b1)

</p>
<p>

We are going to create a few organizational Units which are like folders. Right click on your domain, select new and then select organizational unit. Create 2 Organizational Units called "_EMPLOYEES" & "_ADMINS"

</p>
<br />

![image](https://github.com/kavismith/active-directory/assets/143667203/63618c18-0139-4ee4-b902-6d46e8e9158f)
![image](https://github.com/kavismith/active-directory/assets/143667203/089d8416-833b-4514-a1f3-7442c1281382)
![image](https://github.com/kavismith/active-directory/assets/143667203/c568d1a9-53ed-4b36-8d34-161c609796cd)



</p>
<p>

Now we are going to log out of labuser and create and Admin account and sign into that account. Click on _ADMINS, then right click on the empty space select new and then click on user. 
- First name: Tiffany
- Last name: Smith
- Username: tiffany_admin
- Set your Password

</p>
<br />

![image](https://github.com/kavismith/active-directory/assets/143667203/ec0e2c2e-f900-4e56-9190-0c7fc93e89c9)
![image](https://github.com/kavismith/active-directory/assets/143667203/83f9defe-9b60-4504-b5ca-511d95c3df01)
![image](https://github.com/kavismith/active-directory/assets/143667203/ce3d8b59-8df0-4647-b55b-1b7d688e46ab)

</p>
<p>

Next we are going to move the admin to the admin group to make it an actual admin domain. Right click on the tiffany smith user, then go to properties and select member of. Click add. In the domain group box, type "Domain" and click check and select "Domain Admin" click apply and then ok.
</p>
<br />

![image](https://github.com/kavismith/active-directory/assets/143667203/defa1ebc-d424-4b4d-8e5c-2ead1a959809)
![image](https://github.com/kavismith/active-directory/assets/143667203/4832fc75-7ee3-4603-acdf-7245fd364489)

</p>
<p>

logoff to log in as "tiffany_admin". To check to make sure you are conncted properly, go to start and search for "CMB" and select "Command Prompt". Type in "whoami" and you should see the domain ypu are conncted to and then type in "hostname" and it should read DC-1


![image](https://github.com/kavismith/active-directory/assets/143667203/157d7772-0e62-4491-b820-3c9efec8b2bf)

</p>
<p>
In Azure Portal, set client-1's DNS settings to the DC-1's private ip address. Go to DC-1 and copy its private ip address. Go to virtual machines and click on Client-1. Select networking and select the NIc(Network Interface). Click on DNS servers. Change the DNS server to custom and paste DC-1 private ip address. Select save. Then restart Client-1 from Azure Portal.
</p>
<br />

![image](https://github.com/kavismith/active-directory/assets/143667203/796f4d73-f0a8-46bb-bc6b-25c275eed164)
![image](https://github.com/kavismith/active-directory/assets/143667203/736eec5c-a1d0-4e92-837a-7543ff5768a2)
![image](https://github.com/kavismith/active-directory/assets/143667203/3a283446-83aa-4a83-a45a-002c2d0d1ffd)
![image](https://github.com/kavismith/active-directory/assets/143667203/d6f830e4-6911-4dcf-8089-9a8ab5bfdb90)


</p>
<p>

First, sign back into Client-1 with its new DNS settings. Log into it using (labuser). Now, join it to the domain. To joing the domain right clickon start and select system. Select "rename this PC(advanced)". Select Domain and type in your domain name "kavidomain.com" and select ok. A new login window will show up to change the domain name. Enter the domain name and admin login you set up in DC-1. For this practice I used "kavidomain.com\tiffany_admin and entered my password I set in DC-1. Select ok and now Client-1 will restart. 

![image](https://github.com/kavismith/active-directory/assets/143667203/2deabe7f-7ae5-4dd4-bf75-b6730d759709)
![image](https://github.com/kavismith/active-directory/assets/143667203/94ba7cad-5fa4-4317-8117-9af09ddaaa7d)
![image](https://github.com/kavismith/active-directory/assets/143667203/bc5fde52-8b8b-4abd-8c28-d0f6ddf27e86)
![image](https://github.com/kavismith/active-directory/assets/143667203/39bbfa60-124a-49b2-98cb-db93ec5e46d8)
![image](https://github.com/kavismith/active-directory/assets/143667203/0f4a1d1d-ac42-43de-8b7b-f83364c7c731)

</p>
<p>

Sign back into Client-1 using the domain name "kavidomain.com\tiffany_admin and password you created. Right click on start and go to systems. Click on "Remote Deskop" and then click "select users that can remotely access this PC". Select Add. Enter "omain Users and click check names, you will then see Domain Users have been selected. Click okay. Now ou will be abe to the domain in the box


![image](https://github.com/kavismith/active-directory/assets/143667203/2a03bd60-416f-413b-b90d-ebe4aeb74025)
![image](https://github.com/kavismith/active-directory/assets/143667203/5f710d70-76e7-4b54-b99f-f13322dd95b8)
![image](https://github.com/kavismith/active-directory/assets/143667203/986c6644-7b65-4338-91ae-d1eeec5742a5)
![image](https://github.com/kavismith/active-directory/assets/143667203/2ef33021-8905-417b-875c-792626f4d04c)
![image](https://github.com/kavismith/active-directory/assets/143667203/9f7d3903-dd4d-4de1-bb7f-91f8a3151c0e)


</p>
<p>

Log back into DC-1 as tiffany_admin. open powershell_ise as an administrator. Then create a file and place the content of script in it. To your left hand corner click on the document with a star on it and paste the script in it and select runwhich is the green arrow button This script is gping to create accounts in the _EMPLOYEES FOLD. It will create accounts for 10,000 users that have random names.

![image](https://github.com/kavismith/active-directory/assets/143667203/182898f6-71a8-419e-b3d4-9c536fa63df3)
![image](https://github.com/kavismith/active-directory/assets/143667203/3e1a3be6-2016-413b-9118-0472b1c8bbbf)
![image](https://github.com/kavismith/active-directory/assets/143667203/793f8e66-a795-4666-8d4a-7958b1001bed)
![image](https://github.com/kavismith/active-directory/assets/143667203/7ee5ebbe-d7d7-419b-b975-4105593246b3)
![image](https://github.com/kavismith/active-directory/assets/143667203/5d2cc212-5c3a-4f52-84ba-1d4dd04d1ad2)


</p>
<p>

After selecting "Run", you should be able to see all the users random names. Now, open up active directory users and computers. Select domain. Click on file "_EMPLOYEES" and the users names will appear. Now, try to log into Client-1 using one of the users name. Alll the users profiles have a default password "Passord1".Now, Select a random user "cem.tecox" and try to sign in with using "Password1". Log in was successful. To check to make usre you are logged in correctly open up command prompt and type in "whoami" and it should bring up domain kavidomain\cem.tecox
