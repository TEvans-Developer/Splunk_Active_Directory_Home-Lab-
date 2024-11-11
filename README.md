# Active-Directory-Home-Lab-
Requirments: 
</br> Download, configuration and installation of...
</br> - Windows 10 (VM) 
</br> - Kali Linux (VM)
</br> - Ubuntu Server
</br> - Windows Server 2022
</br> - Virtual Box
</br> - Splunk
</br> - Sysmon


This project displays how to
</br>
<h2>Creating a Logical Diagram:</h2>
I will be using a drawing diagram called draw.io. This will be used to visualize the home lab environment. The home lab will persist of 2 servers, 1 switch, 1 router, 1 target pc, 1 attack machine and a cloud icon to represent the internet and all connection to the appropriate devices. 

</br>Server: Splunk server will have an IP: 192.168.10.10

</br>Server: Active Directory will have an IP: 192.168.10.7 , Splunk Universal Forwarder and Sysmon 

</br>Target: Windows 10 , IP: DHCP, Splunk Universal Forwader, Atomic Red Team and Sysmon

</br>Attacker: Kali Linux, 192.168.10.25

</br>Both Servers, target and attack machine will all connect to the switch, the switch will connect to the router and the router will be connected to the internet. The target machine as well as the Active Directory server will be connected to the Splunk Server to indicate forwarding of logs to splunk ( dotted line without an arrow). 

</br>![Screenshot (195)](https://github.com/user-attachments/assets/c51fadb9-34c2-4146-b503-baa81d3ff662)

<h2>Installation of Virtual Machine</h2>

I installed vitrual box, a Windows 10 (iso), Windows 2022 Server (iso), Ubuntu Server (iso), and Kali Linux onto the virtual box. Using meaningful naming convictions I provided each machine with the appropriate username, machine/ server names and secure passwords (ie. Admin, splunk etc.)

<h2>Install and Configure Sysmon and Splunk</h2>
</br> I installed and configure sysmon and splunk on the Windows Machine (target) and Windows Server so they may began collecting telemontry and send logs to the splunk server

</br>1.) First network settings must be set to NAT network to ensuring the virtual machines can be on the same network still have intenet access. 

</br> This can be done by clicking on the 3 bulletin points next to tool in Virtual Box > select "Network" > select "Create". 

</br>Double click the new NAT network that was created and at the bottom under the "General Options" tab provide the NAT network with a name (e.g. "AD-HomeLab") and the IPv4 Prefix will be the establish IP for our network (e.g. 192.168.10.0/24). "Enable DHCP" should be left on and then apply.

</br>![Screenshot (196)](https://github.com/user-attachments/assets/1bc5770b-bafb-4934-8128-03d9e86a4046)

</br>2.) Now go into each machine and change the current network setting from NAT to the NATnetwork we just created and click okay to apply to each. 
</br>![Screenshot (197)](https://github.com/user-attachments/assets/d2696e26-fbd0-499f-b799-a7febf2e7aa5)

</br>
</br>



