<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Inspecting Traffic Between Azure Virtual Machines</h1>
In this lab, I use Wireshark to observe various network traffic (ICMP, SSH, DHCP, DNS, RDP) and configure Network Security Group to block ICMP traffic between VMs. <br />



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

<p>
  
![image](https://github.com/user-attachments/assets/dd1bdba1-ce2b-4b22-b9b7-d9f264442569)

</p>
<p>
Using Remote Desktop Connection, I connect to the Windows VM using its public IP address. From there, I installed Wireshark in order to begin inspecting traffic. 
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/c9b806ff-389b-4538-9994-3e260007197c)

</p>
<p>
Within Wireshark, I filtered for ICMP (Internet Control Message Protocol) traffic and opened PowerShell to ping to see if I can communicate with the Ubuntu VM using its private IP address and with google.com. Afterwards, I used a perpetual ping with the command: ping -t (private ip address of ubuntu VM).
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/cd6d8e63-4560-44b9-a097-629014cfa407)

</p>
<p>
Within the Azure portal, I opened the networking settings for the Ubuntu VM and added an inbound security rule to block ICMP traffic. I make sure to have the priority higher than SSH (300) to ensure the rule applies first. 
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/5e0accb8-7e02-4ddd-bc83-24d4ab296d9d)

</p>
<p>
Upon returning to the Windows VM, I notice that the ICMP traffic is blocked now that the inbound security rule is in place. After changing the rule to allow traffic again, the perpetual ping resolves without timing out. 
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/5eef08bb-3842-47e2-a434-235c09340e23)

<p>
Next, I examined SSH traffic. I logged in to the Ubuntu VM via PowerShell with the ssh command. With Wireshark, I filtered the traffic with tcp.port == 22. 
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/bbc318d4-d7d8-4e0a-a7e1-7f12bcb80b0a)

</p>
<p>
After examining SSH traffic, I exited the Ubuntu server in order to filter for DHCP traffic. To see it in action, I used ipconfig /renew to issue the new IP address. After reconnecting, the resulting traffic is shown in Wireshark.
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/eec3822f-f7b6-455f-a87a-543c86fa6fb0)

<p>
To observe DNS traffic, I used the filter udp.port == 53 and the command nslookup. I wanted to see the results that are from looking up google.com and disney.com, two very popular sites. 
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/1c811d95-ed88-4db1-81d7-83597d20b074)

</p>
<p>
To finish my lab, I decided to observe RDP traffic. The filter for Wireshark is tcp.port == 3389. 
</p>
<br />

