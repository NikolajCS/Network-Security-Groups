# Network-Security-Groups


<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>

This is a tutorial where I will be observing and inspecting network traffic between Azure Virtual Machines as well as experiemnt with Network Security Groups. In the tutorial I will be filtering different network protocols such as ICMP, SSH, DHCP, DNS and RDP and I will be observing the traffic. I will go over how to setup a basic firewall to block ICMP traffic and use a couple of network command-line utilities/tools


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 Pro (22H2)
- Ubuntu Server 20.04


<h2> Basic Steps</h2>

- We will need 1 ressource gorup and 2 Virtual Machines. I will use one Windows VM and one Linux VM
- We will use Remote Desktop to connect to them
- We will use Wireshark to observe the network traffic

  <h2> The Tutorial </h2>

The picture below basically shows what I will be doing in this tutorial.

![image](https://github.com/user-attachments/assets/a5a2f4b8-1914-45ef-a29f-d629194a63b4)


First we will create the ressource group and then we will create 2 virtual machines in Microsoft Azure and make sure they are both in the same virtual network/subnet and ressource group. 
Open portal.azure.com in your browser > Create a ressource group > You can name it whatever you like best > I will be naming mine "Network-Activities-RG" > Review + Create > Create

![image](https://github.com/user-attachments/assets/479bf6d4-2f81-447a-89df-b7182941e824)

Next I will create a Virtual Machine > portal.azure.com > Virtual Machines > Create > Make sure to choose the ressource group you just created, so I will choose "Network-Activities-RG > Under image, choose: Windows 10 Pro, version 22h2 -x64 Gen2 > Under Administrator Account > fill out Username + Password > Make sure to check the Licensing box > Review + Create > Create > Azure will automatically create a new Virtual Network for the VM, but if you want to create your own and choose the name yourself, you can do so under "Networking" > Virtual Network > Create new > fill out the information > OK 

![image](https://github.com/user-attachments/assets/d81781bd-9dd1-41e7-9615-f2c9aa7649b6)

Next we will need to create another VM, this will be the Linux VM. > The exact same steps as before apply here, I will name this Virtual Machine Linux-VM2 > make sure to choose the same Ressource group, Region and Virtual Network as the Windows-VM. Choose Ubunto Server 22 under Image > Under "Authentication Type: choose password > fill out username + password > Check licensing > Review + Create > Create - (The VM should automatically be put the in the same Virtual Network that it automatically created before for the Windows-VM, but to double check this > Click Next until you get to "Networking" and double check that Virtual network is the same as the Windows VM.

![image](https://github.com/user-attachments/assets/97005626-e28d-4be6-b98f-9fec75eacb46)



I will then connect to the Windows Virtual Machine via Remote Dekstop. To do that I will get the public IP adress of the Windows vm, which can be found in Microsoft Azure > Virtual Machines > Next to the Virtual Machine, you will see the public IP adresse and paste it into Remote Dekstop

![image](https://github.com/user-attachments/assets/db322e33-9432-49a4-a1f5-bd1891ce9d71)

Open Remote Desktop in Windows. If you are using Mac you can download Remote Desktop in the Mac App Store. All you have to do is paste the Public IP of the Windows VM, and if you want to you can name the VM in Remote Desktop to get a better overview of the different VM's. For example "windows-vm" or "linux-vm". Example of what Remote Desktop can look like below

![image](https://github.com/user-attachments/assets/98eedc60-a17d-4154-9a8d-d9acfc5d1954)

Now we will install a protocol analyzer to observe network traffic. In this tutorial I will be using Wireshark. > Log in to the Windows vm via Remote Dekstop > open a browser in the VM > https://www.wireshark.org/ > Download > follow the install configuration, you can simply use all the default configurations

![image](https://github.com/user-attachments/assets/0f6d199c-f940-4cc2-b372-0eaff02ef3d0)

Open WhireShark > Ethernet should be highlighted > click on the shark icon on the top left corner that says "start capturing packets" 

You can now see all the network traffic happening on the backend.

![image](https://github.com/user-attachments/assets/019f0f9a-aea4-400c-828e-f1c10be56da7)

I will now ping the Linux vm from within the Windows vm and observe the traffic via Whireshark. So first I will find the Linux VM's private IP address > Go to Microsoft Azure > Virtual Machines > click on your Linux vm > Look for the private IP address > Now open your Windows virtual machine > Open Whireshark and filter to icmp traffic > Simply type icmp in the top collum in Whireshark > Open Powershell as an admin > ping the Linux vm's private IP address, command: Ping "IP address"  
![image](https://github.com/user-attachments/assets/35825717-f93f-42f8-9133-2859ae87b4de)

I will now setup a firewall within the Linux-vm, which will block the ping requests from the Windows vm > Go to portal.azure.com > Virtual Machines > Your Linux VM > Networking > Network settings > On the right side click on > Network Security Group: Linux-vm-nsg 
![image](https://github.com/user-attachments/assets/86d6f88f-cead-4ff9-9c1d-7f6898ac77e0)

In "Linux-vm-nsg > Settings > Inbound security rules > add > Put "Destination port ranges" to * > Protocol to "ICMPv4" > Action: Deny > Priority: 290 > Add

![image](https://github.com/user-attachments/assets/e795d45f-28c4-4df3-8e13-0e853315a8b3)

If we go back into the Windows VM, and use the ping command we will get a "Request timed out" and in Whireshark we will get no reply from the Linux VM. The firewall is now blocking the ping request from the Windows VM.


![image](https://github.com/user-attachments/assets/06b1933f-d9d1-411d-8939-a973497a099d)


Observing SSH traffic (Secure Shell) > Log in to your Windows VM > Open Wireshark and filter for "ssh" traffic > Open Powershell as an admin > run the command ssh + Linux VM usersname + its private IP address for example: ssh jokeren@10.2.0.5 > yes > enter password > When you are entering your password nothing will show, that is for security reasons it will still register what you are typing.

![image](https://github.com/user-attachments/assets/70aaa978-3a8a-42e5-8697-67a3a46593b2)

We are now connected to the Linux-vm via ssh. You can type some random stuff in powershell and inspect it in Wireshark. You will see that the data is encrypted so we cannot see what the actual packets contain. This is because SSH is a secure connection and encrypts the data

![image](https://github.com/user-attachments/assets/3c0679ce-b47a-41b5-ae08-08d60aa83f7a)

Exit the connection > Powershell command > "exit" 

![image](https://github.com/user-attachments/assets/8aae2feb-7931-4a1b-942d-e70e281cfbae)

Observing DHCP Traffic (Dynamic Host Configuration Protocol): The DHCP process can be boiled down to -> Discover, Offer, Request, Acknowlegde. To observe this we need to make a bat file. Open notepad > Type Ipconfig /release, ipconfig /renew

![image](https://github.com/user-attachments/assets/63a59006-e0f2-41d1-8fa8-0c935bf6f3d4)

Now save it on the directory C:\ProgramData > name it dhcp.bat and save as all files 

![image](https://github.com/user-attachments/assets/181e87a8-7119-4149-9daa-523d571633f6)

Open powershell as an administrator > type: cd c:\programdata > enter > type: ls > enter > type: .\dhcp.bat > enter > the command should almost instantantiously use the release and renew command simoultaniously so we do not lose the connection to the VM and in Whireshark we can observe the dchp traffic

![image](https://github.com/user-attachments/assets/2f8ffda8-067a-400e-a8c7-9b560b59d6f4)

Observing dns (Donaim Name System): This is very simple > In Whireshark filter for "dns" > Open Powershell > type: nslookup any website > example: nslookup facebook.com or nslookup youtube.com

![image](https://github.com/user-attachments/assets/49c8b652-b55a-4aae-aa4d-457fcd67195d)

Obersving RDP: (Remote Dekstop Protocol) > In Whireshark filter for rdp > There will be a lot of traffic because we are connected through remote desktop

![image](https://github.com/user-attachments/assets/b3f3509f-057b-4ec4-9424-55b103d75411)

This is the end of the tutorial! 

