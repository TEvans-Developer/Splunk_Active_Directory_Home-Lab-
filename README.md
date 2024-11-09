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



