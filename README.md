<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 Pro (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>

<p>
</p>
<p>
First, you will need to create two VMs on Azure. One machine will be a Linux machine and the other will be a Windows 10 machine. Both will have two CPUs and they must be on the same VNET. Once that is done go on the Windows machine and download Wireshark. I will attach a link to the Wireshark download. https://www.wireshark.org/download.html Once installed open Wireshark and filter for ICMP Traffic only. ICMP is a network layer protocol that relays messages concerning network connection issues. Ping uses this protocol, and ping tests connectivity between hosts. When we filter Wireshark to only capture ICMP packets and ping the private IP address of our Linux machine we can visually see the packets on Wireshark. 
</p>
<br />
<p>
<img src="https://i.imgur.com/IIUShxp.png" height="80%" width="80%" alt="Wireshark"/>
</p>
<p>
We can inspect each individual packet and see the actual data that is being sent in each ping. the picture below demonstrates just that. 
</p>
<br />
<p>
<img src="https://i.imgur.com/GLxSIG3.png" height="80%" width="80%" alt="Hash"/>
</p>
<p>
In the next portion of the lab, we will perpetually ping the Linux machine with the command ping -t. This will continually ping the machine until we decide to stop it, while the Windows machine is pinging the Linux machine, we will go to the Linux machine and block inbound ICMP traffic on its firewall. Once we do that, we will stop receiving echo replies from the Linux machine. We will block ICMP by creating a new Network Security Group on the Linux machine that will be set to block ICMP. We can allow the traffic by allowing ICMP on the Linux Network Security Groups page on Azure. 
</p>
<br />
<img src="https://i.imgur.com/5vXO75R.png" height="80%" width="80%" alt="Wireshark cmd"/>
</p>
<img src="https://i.imgur.com/Asl80tN.png" height="80%" width="80%" alt="Azure network security group"/>
<p>
Next, we will use our Windows machine to SSH to the Linux machine. SSH has no GUI it just gives the user access to the machine’s CLI. We will set the Wireshark filter to capture SSH packets only. When we ssh into the Linux machine with the command prompt "ssh labuser@10.0.0.5" we can see that Wireshark starts to immediately capture SSH packets.
</p>
<br />
<img src="https://i.imgur.com/zteR41r.png" height="80%" width="80%" alt="Wireshark shell"/>
</p>
<p>
Now we will use Wireshark to filter for DHCP. DHCP is the Dynamic Host Configuration Protocol this works on ports 67/68. It is used to assign IP addresses to machines. We will request a new IP address with the command "ipconfig /renew". Once we enter the command Wireshark will capture DHCP traffic.
</p>
<br />
<img src="https://i.imgur.com/vU8fpQf.png" height="80%" width="80%" alt="Wireshark ifconfig shell"/>
</p>
<p>
Time to filter DNS traffic. We will set Wireshark to filter DNS traffic. We will initiate DNS traffic by typing in the command "nslookup www.google.com" this command essentially asks our DNS server what is Google’s IP address.
</p>
<br />
<img src="https://i.imgur.com/VMcwmsO.png" height="80%" width="80%" alt="Wireshark cmd nslookup"/>
</p>
<p>
Lastly, we will filter for RDP traffic. When we enter tcp.port==3389 traffic is spammed non-stop because we are using Remote Desktop Protocol to connect to our Virtual Machine. 
</p>
<br />
<img src="https://i.imgur.com/VxXGv6X.png" height="80%" width="80%" alt="Wireshark tcp port inspection"/>
</p>
<p>

