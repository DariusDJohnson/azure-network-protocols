<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create resources in Azure
- Observe ICMP Traffic
- Configure Firewall (Network Security Group)
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>

<p>
  
![image](https://github.com/user-attachments/assets/ad239e4b-8310-4038-a390-3490ebd30b31) ![image](https://github.com/user-attachments/assets/42501df5-018a-4af8-acbe-c339e84bb225)
</p>
<p>
1.) Create our Virtual Machines:
  
  Within Azure, Create a Resource Group -> Create a Windows 10 Virtual Machine (VM) -> While creating the VM, select the previously created Resource Group -> While creating the VM, allow it to create a new Virtual Network (Vnet) and Subnet -> Create a Linux (Ubuntu) VM -> While creating the VM, select the previously created Resource Group and Virtual Network—the Virtual Network MUST BE THE SAME. (Authentication type: Username/Password) -> Ensure both VMs are in the same Virtual Network / Subnet

</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/8e178aab-6c7d-496a-820a-03df2bd1629a) ![image](https://github.com/user-attachments/assets/95e122b5-dc2d-4388-b9a5-d23973f91771) ![image](https://github.com/user-attachments/assets/b80c221b-92ea-4153-82a7-efb4d173d537)
</p>
<p>
2.) Use Remote Desktop to connect to your Windows 10 Virtual Machine -> Within your Windows 10 Virtual Machine, Install Wireshark via your web browser 
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/05fd4a58-cd37-412b-a156-d2eb2bb66be3) ![image](https://github.com/user-attachments/assets/e1127033-2d47-4e79-ab30-1ee4b52e1bb1) ![image](https://github.com/user-attachments/assets/f3b0ec71-47c2-450d-be1f-129f7912a964) ![image](https://github.com/user-attachments/assets/5c370312-1c1f-4bd4-a489-70aa7d376290)
</p>
<p>
3.) Observe ICMP Traffic:
  
  Within Wireshark, filter for ICMP traffic only -> Retrieve the private IP address of the Ubuntu VM (linux-vm) and attempt to ping it from within the Windows 10 VM -> Observe ping requests and replies within WireShark
</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/e6dad37d-3e8c-4c9a-823f-a6263cbfa590) ![image](https://github.com/user-attachments/assets/58761b68-5bcf-454c-abba-ac32f62e7158) ![image](https://github.com/user-attachments/assets/24fc9662-913c-4996-bd50-d13a0c5e6081) ![image](https://github.com/user-attachments/assets/1c0a8dbc-2007-461f-ae6f-0001bd6dbd6a) ![image](https://github.com/user-attachments/assets/5f56faeb-f148-4e9d-8cb8-3b135348f006)
</p>
<p>
4.) Configuring a Firewall [Network Security Group:
  
  Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM -> Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic -> Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity -> Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is -> Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity (should start working)
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/2ba9f720-c1bc-4db7-b72b-828516091178) ![image](https://github.com/user-attachments/assets/2cbe913b-a46c-496f-9968-39f3c36c09e5) ![image](https://github.com/user-attachments/assets/c236c810-adee-4a1a-9e18-8ce679cb5f7c) ![image](https://github.com/user-attachments/assets/30c391a6-ebf1-451c-a413-fbb5c3b388cb)
</p>
<p>
5.) Observe SSH Traffic:
  
Log back into the windows-vm -> Back in Wireshark, start a packet capture up -> Filter for SSH traffic only -> From your Windows 10 VM, “SSH into” your Ubuntu Virtual Machine (via its private IP address) -> Open PowerShell, and type: ssh username@private IP address -> Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark -> Exit the SSH connection by typing ‘exit’ and pressing [Enter]
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/3dcda90c-7780-418e-bd53-21f37b5f2229) ![image](https://github.com/user-attachments/assets/9352e833-0e1d-4ef7-9390-71a2fc885ad1) 
</p>
<p>
6.) Observe DHCP Traffic:

Back in Wireshark, filter for DHCP traffic only -> From your Windows 10 VM, attempt to issue your VM a new IP address from the command line -> Open PowerShell as admin and run: ipconfig /renew -> Observe the DHCP traffic appearing in WireShark
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/c66b3345-5b3c-4dbb-b2c6-0f28c2ff9768) ![image](https://github.com/user-attachments/assets/0f30f61e-2d74-429e-a56e-7ca33276c707)
</p>
<p>
7.) Observe DNS Traffic:
  
Back in Wireshark, filter for DNS traffic only -> From your Windows 10 VM within a command line, use nslookup to see what disney.com’s IP address is -> Observe the DNS traffic being show in WireShark
</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/20b971d7-ee8a-4186-bf5b-657e1e4fc149)
</p>
<p>
8.) Observe RDP Traffic:
  
  Back in Wireshark, filter for RDP traffic only (tcp.port == 3389) -> Observe the immediate non-stop spam of traffic. The non-stop traffic is due to the RDP (protocol) constantly showing you a live stream from one computer to another, therefor traffic is always being transmitted.
</p>
<br />
