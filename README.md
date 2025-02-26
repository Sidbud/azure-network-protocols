<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Inspecting Traffic Between Azure Virtual Machines</h1>
In this lab, I observe different kinds of network traffic to and from Azure virtual machines with Wireshark as well as experiment with network security groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Connection
- Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 Pro (21H2)
- Ubuntu Server 20.04

<h2>The Set-Up</h2>

Within Azure, I created two VMs within the same virtual network to ensure that they are able to communicate with each other. One VM will have Windows 10 Pro while the other uses Ubuntu. The Windows VM will connect to the other via the command line/PowerShell. 

<h2>Actions and Observations</h2>


![image](https://github.com/user-attachments/assets/a6e7d565-9158-4d27-b2d7-3880f7005bdb)

<p>
Using Remote Desktop Connection, I connect to the Windows VM using its public IP address. From there, I installed Wireshark in order to begin inspecting traffic. 
</p>
<br />


![step 2](https://github.com/user-attachments/assets/8a5909f4-ca73-4f2e-a332-91d75d18ff1c)
<p>
Within Wireshark, I filtered for ICMP (Internet Control Message Protocol) traffic and opened PowerShell to execute a command called ping. Ping utilizes ICMP, which is used by devices in a network to communicate problems within data transmition. I used ping to see if I can communicate with the Ubuntu VM using its private IP address and with google.com. Afterwards, I used a perpetual ping to the Ubuntu VM in order to see how network security groups work. I executed the perpetual ping with the command: ping -t (ip address).
</p>
<br />

![image](https://github.com/user-attachments/assets/5f0573aa-4d2b-48d9-9b08-b7eceaf7333c)

<p>
Within the Azure portal, I opened the networking settings for the Ubuntu VM and added an inbound security rule to block ICMP traffic. I make sure to have the priority higher than SSH (300) to ensure the rule applies first. 
</p>
<br />

![image](https://github.com/user-attachments/assets/e0587cc8-fc05-4b10-bbd9-8969da5ae6ae)

<p>
Upon returning to the Windows VM, I notice that the ICMP traffic is blocked now that the inbound security rule is in place. After changing the rule to allow traffic again, the perpetual ping resolves without timing out. 
</p>
<br />

![image](https://github.com/user-attachments/assets/7e14f082-7c60-408a-9dbd-4e1549acd6c2)

<p>
Next, I chose to examine SSH traffic. I logged in to the Ubuntu server via PowerShell with the ssh command (ssh user@private IP address). With Wireshark, I filtered the traffic with ssh. While logged into the Ubuntu server, my session is logged in Wireshark with each command I use.
</p>
<br />

![image](https://github.com/user-attachments/assets/9f13215c-08c0-41f7-86ca-8f83b5bf04e8)

<p>
After examining SSH traffic, I exited the Ubuntu server in order to filter for DHCP traffic. To see it in action, I decided to attempt to issue a new IP address from my VM. The command ipconfig /renew will attempt to issue the new IP address and will temporarily disconnect me for a few seconds. After reconnecting, the resulting traffic is shown in Wireshark.
</p>
<br />

![image](https://github.com/user-attachments/assets/7497acf2-12e2-49b9-83ea-4968f0dd1bcf)

<p>
To observe DNS traffic, I used the DNS filter (aka udp.port == 53) and the command "nslookup" just to simply see the results from looking up Youtube.com and Netflix.com. 
</p>
<br />

![image](https://github.com/user-attachments/assets/99267657-7225-47dc-9821-9ed2c317ffab)

<p>
To finish my lab, I decided to observe RDP traffic. The filter for Wireshark is tcp.port == 3389. There is non-stop traffic because RDP is constantly showing me a live stream from one computer to another (in my case, my computer accessing the VM that is hosted on Azure) and thus traffic is always transmitted. 
</p>
<br />

<h2>Lessons Learned </h2>

The purpose of this lab was for me to see how different protocols and ports are utilized in a network between devices. While this lab does not exactly allow me to troubleshoot, it still serves a purpose to gather information and learn. While troubleshooting, I need to utilize different tools like Wireshark and the command line to see how traffic flows in a network through ports and protocols. As someone that is very curious about the inner workings of things, this was a very fun and eye opening exercise.
