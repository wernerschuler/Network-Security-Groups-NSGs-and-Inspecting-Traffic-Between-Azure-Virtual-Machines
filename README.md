
 <h1>Network-Security-Groups-NSGs-and-Inspecting-Traffic-Between-Azure-Virtual-Machines</h1>

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.

Terms
-- 
- Wireshark
- ICMP traffic

Steps
-- 
**1. Create a Resource Group**
  - Go to portal.azure.com --> Resource groups --> Create
  - Enter a name for the Resource group --> Set your Region
  - Review + create --> Create

<img src="https://i.imgur.com/Iy6XCZ2.png" height="40%" width="60%" alt="image showing resource group being created"/>

**2. Create a Windows 10 Virtual Machine (VM)**
 - portal.azure.com --> Virtual machines --> Create --> Azure virtual machine
 - Select the Resource group you created in the previous step
 - Enter a name for the VM
 - Select the same Region as the previous step
 - Select Windows 10 for the Image
 - Set the Size to 2 vcpus or more
 - Enter a username and password
 - Tick the box indicating 'I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights.'
 - Review + create --> Create

<img src="https://i.imgur.com/s9TjhXv.png" height="40%" width="60%" alt="image showing a virtual machine being created"/>
   
 - Make a note of the Virtual network that was created for this VM
   - Go to Virtual machines --> Click the name of the VM ---> Look under 'Virtual network/subnet'
  
<img src="https://i.imgur.com/66okQZV.png" height="40%" width="60%" alt="image showing an overview of the virtual machine"/>

**3. Create a Linux (Ubuntu) VM**
 - Virtual machines --> Create --> Azure virtual machine
 - Select the same Resource group as the previous step
 - Enter a name for the VM
 - Select the same Region as the previous step
 - Select Ubuntu Server 20.04 LTS for the Image
 - Set the Size to 2 vcpus or more
 - For Authentication type select Password
 - Enter a username and password
 - Click Next until you get to the Networking page
   - Select the Virtual network that was created in the previous step
 - Review + create --> Create

<img src="https://i.imgur.com/KAjE99T.png" height="40%" width="60%" alt="image showing a virtual machine being created"/>

**4. Use Remote Desktop to connect to your Windows 10 Virtual Machine**
 - Copy the public IP address of the Windows 10 VM
   - Virtual machines --> Name of VM --> Look under Public IP address

 <img src="https://i.imgur.com/QufRysh.png" height="40%" width="60%" alt="overview of a virtual machine"/>
    
 - Start --> Enter 'Remote desktop connection'
 - Paste the VM's IP address --> More choices --> Use a different account
 - Enter the username and password for the VM

<img src="https://i.imgur.com/FSaV9n6.png" height="40%" width="60%" alt="remote desktop connection login screen"/>

**5. Within your Windows 10 Virtual Machine, Install Wireshark**
 - Enter 'Download Wireshark' into Google --> Windows installer --> Once download is complete go to Wireshark
 - In the Wireshark setup, install Wireshark with it's default settings

