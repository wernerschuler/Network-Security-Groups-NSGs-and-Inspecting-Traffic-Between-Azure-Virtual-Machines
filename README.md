
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

**6. Open Wireshark and filter for ICMP traffic only**
 - Start --> Enter Wireshark --> Click 'Start capturing packets' (image below)

<img src="https://i.imgur.com/21jquHi.png" height="40%" width="60%" alt="how to capture packets in wireshark"/>

 - In the filter bar enter 'icmp' then click Enter on your keyboard

<img src="https://i.imgur.com/kVuhxoY.png" height="40%" width="60%" alt="how to capture packets in wireshark"/>

**7. Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM**
 - From your local computer go to portal.azure.com --> Virtual machines --> Click the name of your Ununtu VM --> Scroll down until you see the private IP address

<img src="https://i.imgur.com/dPLUuqq.png" height="40%" width="60%" alt="Virtual machines private IP address"/>

 - In the VM --> Start --> PowerShell --> Enter ping [private IP address]
 - Observe the reply and request in Wireshark

<img src="https://i.imgur.com/XAn7y7g.png" height="80%" width="100%" alt="Virtual machines private IP address"/>

**8. From The Windows 10 VM, open command line or PowerShell and attempt to ping a public website (such as www.amazon.co.uk) and observe the traffic in WireShark**
 - Can see from Wireshark the private IP address of the Windows 10 VM is 10.0.0.4, which is sending a request to www.amazon.co.uk and gets a reply with the IP address 18.66.174.162

<img src="https://i.imgur.com/5slkufo.png" height="80%" width="100%" alt="Virtual machines private IP address"/>

**9. Initiate a perpetual ping from your Windows 10 VM to your Ubuntu VM**
 - To clear Wireshark click the green icon at the top left of the screen (image below)

<img src="https://i.imgur.com/CqXFLco.png" height="50%" width="60%" alt="Virtual machines private IP address"/>


