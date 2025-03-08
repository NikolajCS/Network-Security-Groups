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


Observing SSH traffic: > Log in to your Windows VM > Open Wireshark and filter for "ssh" traffic > Open Powershell as an admin > run the command ssh + Linux VM usersname + its private IP address for example: ssh jokeren@10.2.0.5 > yes > enter password > When you are entering your password nothing will show, that is for security reasons it will still register what you are typing.

![image](https://github.com/user-attachments/assets/70aaa978-3a8a-42e5-8697-67a3a46593b2)

We are now connected to the Linux-vm via ssh. You can type some random stuff in powershell and inspect it in Wireshark. You will see that the data is encrypted so we cannot see what the actual packets contain. This is because SSH is a secure connection that encrypts the data

![image](https://github.com/user-attachments/assets/3c0679ce-b47a-41b5-ae08-08d60aa83f7a)

Exit the connection > Powershell command > "exit" 

![image](https://github.com/user-attachments/assets/8aae2feb-7931-4a1b-942d-e70e281cfbae)

Observing DHCP Traffic: The usual process is: Discover, Offer, Request, Acknowlegde. To observe this we need to make a bat file. Open notepad > Type Ipconfig /release ipconfig /renew

![image](https://github.com/user-attachments/assets/63a59006-e0f2-41d1-8fa8-0c935bf6f3d4)

Now save it on the directory C:\ProgramData > name it dhcp.bat and save as all files 

![image](https://github.com/user-attachments/assets/181e87a8-7119-4149-9daa-523d571633f6)

Open powershell as an administrator > type: cd c:\programdata > enter > type: ls > enter > type: .\dhcp.bat > enter > the command should almost instantantiously use the release and renew command simoultaniously so we do not lose the connection to the VM and in Whireshark we can observe the dchp traffic

![image](https://github.com/user-attachments/assets/2f8ffda8-067a-400e-a8c7-9b560b59d6f4)

Observing dns: This is very simple > In Whireshark filter for "dsn" > Open Powershell > type: nslookup any website > example: nslookup facebook.com or nslookup youtube.com

![image](https://github.com/user-attachments/assets/49c8b652-b55a-4aae-aa4d-457fcd67195d)

Obersving RDP: (Remote Dekstop Protocol) > In Whireshark filter for rdp > There will be a lot of traffic because we are connected through remote desktop

![image](https://github.com/user-attachments/assets/b3f3509f-057b-4ec4-9424-55b103d75411)

This is the end of the tutorial! 

