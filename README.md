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

<p>
![image](https://github.com/kavismith/active-directory/assets/143667203/4798da25-2971-4092-8686-8716b30cebd1)

.`![image](https://github.com/kavismith/active-directory/assets/143667203/41f3e910-c3d4-4121-8dc1-051370f4a611)
</p>
<p>
click on the flag next to the exclamation mark to you right hand corner and select promote this server to a domain controller. This is the final steps to installing active directory and the server will turn into a Domain Controller. Select add a new forest in the Deployment Configuration section. Now, name the private domain to kavidomain.com. Set your password. Click next until you get to the install section, then press install. 
Name of Private Domain: kavidomain.com
Set up your Password
DNS Options:Click Next
Additional Options: Click Next
Paths: Next
Review Options: Next 



Prerequisites Check: click next and allow the prerequisites check go through and click install

  
</p>
<br />
