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

</br>3.) Logging into the splunk server you will notice a that the IP for the server is not true to the diagram that was draw for the lab. We should change the IP of the server by writing the command, "ip addr" to see our IP for the machine, then "sudo nano /etc/netplan/<tab>". Tab will bring you to the file which will allow you to deactiviate DHCP and write a static IP for the machine.  

</br>![Screenshot (198)](https://github.com/user-attachments/assets/f8a86359-30b0-4aac-9ce3-9c5258606eb8)

<br>4.)The DHCP should be removed, replacing "True" with "no". This lets the file know you do not want a dynamic IP address given to the server. 
</br> Next we should align the "addresses" configuration with the dhcp4 and give it our static IP from the diagram [192.168.10.10/24].
</br> Next the "nameservers" should be align under our static ip address, we will then tab 4 spaces under our nameservers: and list the hosting ip address we want our domain to point to. [8.8.8.8] (Goolge).
</br> Then align the "routes:" with the dhcp4, addresses: and nameservers:... We will then put where we want our traffic to go "-to", which will be "via" our gateway address. 
</br> Ctrl + x , "Y" and hit "Enter" to save the configuration

</br>![Screenshot (200)](https://github.com/user-attachments/assets/a1c98386-c0d0-47d1-ac13-c844d1631897)


</br>5.) Lastly type in the command "sudo netplan apply" and then check the new static ip address and try pinging to google.com to ensuring network is connecting. 


</br>![Screenshot (201)](https://github.com/user-attachments/assets/5bb6c7bc-c362-40ed-a6e5-7293dfb27a7a)



</br>6.) Go to Splunk's website to creat an account and go to products / free downloads to download the .deb enterprise addition (linux) for a splunk server.

</br>7. In the splunk server the command "sudo apt-get install virtualbox" <tab> was type to display options. We are looking for the "virtualbox-guest-additions-iso
. Type Y for yes, then hit enter.

</br>![Screenshot (202)](https://github.com/user-attachments/assets/132094e3-96a3-474f-a539-e8285c8232e8)

</br>8.) On the top of the splunk servers vm , click "Devices" > "Shared Folders" > "Shared Folders settings" > click the add folder icon. We then will find the path to where we downloaded our splunk .deb  and leave the name of the folder as is. Then check all boxes and hit okay. After we will type in the splunk server sudo reboot to reboot the server and log back in. 

</br>9.) We will not input the command "sudo adduser <username> vboxsf. Note.. If "The group 'vboxsf' does not exist" appear. Type in  sudo apt-get install virtualbox <tab> , then find "sudo apt-get install virtualbox-guest-utils"  and install it. Then after installation 'sudo reboot' then log on to re add user again

</br>![Screenshot (202)](https://github.com/user-attachments/assets/8e3e1474-6291-4394-95dd-9a8e893ae032)


</br>![Screenshot (203)](https://github.com/user-attachments/assets/4890183a-5030-4626-b9ae-5126adb2c035)


</br>![Screenshot (204)](https://github.com/user-attachments/assets/7e9cae7e-0feb-464c-a011-a4201f466bcf)



