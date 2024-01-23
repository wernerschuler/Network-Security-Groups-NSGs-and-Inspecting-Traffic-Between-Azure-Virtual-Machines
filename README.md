
 <h1>Network-Security-Groups-NSGs-and-Inspecting-Traffic-Between-Azure-Virtual-Machines</h1>

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.

Terms
-- 
- Wireshark - A network protocol analyser. It is used to capture, analyse network traffic in real-time.
- ICMP (Internet Control Message Protocol) - Network protocol used to report errors and exchange network status information. One commonly known ICMP message is the "ICMP Echo Request" message, which is used by the "ping" utility to test network connectivity.
- Network security group (NSG) - Is a security feature that acts as a virtual firewall for controlling and govering inbound and outbound traffic.
- SSH (Secure Shell) - A network protocol that provides secure and encrypted communication two devices over a network. It is commonly used to allow remote access to a computer or server.
- DHCP (Dynamic Host Configuration Protocol) - A networking protocol that allows devices to automatically obtain network configuration information, such as IP addresses, subnet masks, and DNS server addresses.
- RDP (Remote Desktop Protocol) - Protocol developed by Microsoft that allow users to connect and control a computer over a network connection.

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

 - To perpetual ping the Ubuntu VM
   - In PowerShell --> Enter ping [private IP address] -t
 - a. Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic
   - From your local computer --> portal.azure.com --> go to Network security groups --> Click the Network security group for the Ubuntu VM
 - Under Settings click 'Inbound security rules'
 - Click Add
  - Source: Any
  - Source port ranges: *
  - Destination: Any
  - Service: Custom
  - Destination port ranges: *
  - Protocol: ICMP
  - Action: Deny
  - Priority: 200
  - Name: DENY_ICMP_PING
 - Add

<img src="https://i.imgur.com/NpOUEIc.png" height="60%" width="80%" alt="Creating a inbound security rule"/>

- b. Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity
  - With the inbound security rule in place to deny ICMP. You can see from the screenshot below, in Wireshark request have been made but no reply has been received. Can also see in PowerShell we get these 'Request timed out'

<img src="https://i.imgur.com/QSaJZ6L.png" height="80%" width="80%" alt="wireshark and powershell not getting a reply"/>

- c. Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using
  - In your local computer --> portal.azure.com --> Network security groups --> Click the Network security group for the Ubuntu VM --> Inbound security rules --> Click the DENY_ICMP_PING rule --> Under 'Action' select Allow --> Save

- d. Go back to your Windows 10 VM and see the ICMP traffic in Wireshark and PowerShell. Should now be getting replies.
- e. Stop the ping
  - Press Ctrl + C in PowerShell

**10. Back in Wireshark filter for SSH traffic**
 - Enter in the filter bar 'ssh'

**11. From your Windows 10 VM, SSH into your Ubuntu VM using it's private IP address**
 - First go to portal.azure.com and make a note of the Ubuntu VM private IP address
   - portal.azure.com --> Virtual machines --> Click the name of the VM --> Scroll down until you see the private IP address
  - In your Windows VM --> In PowerShell --> Enter ssh [username of Ubuntu VM]@[Ubuntu VM's private IP address]
  - 'Are you sure you want to continue connecting': yes --> Enter
  - Enter the password for the Ubuntu VM. **Note** you will not see the characters you enter on the screen, but when you are typing characters it is being entered into the terminal.
  - Once you entered your password --> Press Enter

<img src="https://i.imgur.com/TyH11VZ.png" height="60%" width="80%" alt="wireshark and powershell not getting a reply"/>

 - a. Can see that when a command is entered the SSH traffic appears in Wireshark
 - b. Exit the SSH connection by typing 'exit' and pressing Enter in PowerShell

**12. In Wireshark, filter for DHCP traffic only**
 - Type dhcp into the filter bar then press Enter

**13. From your Windows 10 VM, try to issue your VM a new IP address from PowerShell using 'ipcongig /renew'**
 - After entering this command we can see some DHCP traffic in Wireshark

<img src="https://i.imgur.com/qZuWXmp.png" height="60%" width="80%" alt="wireshark and powershell not getting a reply"/>

**14. Filter for DNS traffic only**
 - Enter dns into the filter bar then press Enter
 - From Windows 10 VM, use command 'nslookup' to see what is the IP address for www.amazon.com
 - Observe the traffic in Wireshark 

**15. Observe RDP traffic**
 - In Wireshark filter for RDP traffic only (tcp.port == 3389)

**16. You can see the traffic is non-stop**
 - This is because RDP is constantly showing a live stream from one computer to another, therefore traffic is always being transmitted.

<img src="https://i.imgur.com/lFuZODg.png" height="60%" width="80%" alt="wireshark and powershell not getting a reply"/>

 



