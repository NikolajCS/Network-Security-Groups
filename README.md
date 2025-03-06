# Network-Security-Groups


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

- Windows 11 Pro (24H2)
- Ubuntu Server 20.04

<h2> Basic Steps</h2>

- We need 2 Virtual Machines. I will use one Windows vm and one Linux vm
- We will use Remote Desktop to connect to them
- Step 3
- Step 4


Step 1 is to create 2 virtual machines in Microsoft Azure and make sure they are both in the same virtual network/subnet and ressource group. 
We will then connect to the Windows Virtual Machine via Remote Dekstop. Find the public IP adress of your vm in Azure and paste it into Remote Dekstop

![image](https://github.com/user-attachments/assets/98eedc60-a17d-4154-9a8d-d9acfc5d1954)

We will then install a protocol analyzer to observe network traffic in this tutorial we will use Wireshark. > Log in to the Windows vm > open a browser > https://www.wireshark.org/ > Download > follow the install configuration, you can just use all the default configurations

![image](https://github.com/user-attachments/assets/0f6d199c-f940-4cc2-b372-0eaff02ef3d0)

Open WhireShark > Ethernet should be highlighted > click on the shark icon on the top left corner that says "start capturing packets" 

You can now see all the network traffic happening

![image](https://github.com/user-attachments/assets/019f0f9a-aea4-400c-828e-f1c10be56da7)

We will now ping the Linux vm from within the Windows vm and observe the traffic via Whireshark. So first we need to find our Linux virtual machines private IP address > Go to Portal.Azure.com > Virtual Machines > click on your Linux vm > Look for the private IP adresse> Now open your windows virtual machine > Open Whireshark and filter to icmp traffic > Simply type icmp in the top collum in Whireshark > Open Powershell as an admin > ping the Linux vm's private IP address, command: Ping "IP address"  
![image](https://github.com/user-attachments/assets/35825717-f93f-42f8-9133-2859ae87b4de)

Now we will setup a firewall within the Linux-vm, which will block the ping requests from the Windows vm > Go to Azure Portal > Virtual Machines > Your Linux VM > Networking > Network settings > On the right side click on > Network Security Group: Linux-vm-nsg 
![image](https://github.com/user-attachments/assets/86d6f88f-cead-4ff9-9c1d-7f6898ac77e0)

In "Linux-vm-nsg > Settings > Inbound security rules > add > Put "Destination port ranges" to * > Protocol to "ICMPv4" > Action: Deny > Priority: 290 > Add

![image](https://github.com/user-attachments/assets/e795d45f-28c4-4df3-8e13-0e853315a8b3)

If we go back into the Windows VM, and use the ping command we will get a "Request timed out" and in Whireshark we will get no reply from the Linux VM. The firewall is now blocking the ping request from the Windows VM.


![image](https://github.com/user-attachments/assets/06b1933f-d9d1-411d-8939-a973497a099d)



<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
