
 <h1>Network-Security-Groups-NSGs-and-Inspecting-Traffic-Between-Azure-Virtual-Machines</h1>

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.

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

<img src="https://i.imgur.com/s9TjhXv.png" height="40%" width="60%" alt="image showing resource group being created"/>
   
 - Make a note of the Virtual network that was created for this VM
   - Go to Virtual machines --> Click the name of the VM ---> Look under 'Virtual network/subnet'
  
<img src="https://i.imgur.com/66okQZV.png" height="40%" width="60%" alt="image showing resource group being created"/>


