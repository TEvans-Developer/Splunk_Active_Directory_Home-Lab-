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


</br>10.) We now will create a knew directory called "share", then mount the shared folder onto the directory called share with this command. 

</br> sudo mount -t vboxsf -o uid=1000,gid=1000 <shared_folder_name> share/

</br>![Screenshot (205)](https://github.com/user-attachments/assets/eae659ca-050b-4eb6-a78a-64c200337539)


</br>11.) After we will cd into the share folder we created and ls -la to see all the shared folder including the splunk .deb package. We will then use the command to install the splunk package into the machine. " sudo dpkg -i splunk <tab> to do the rest. Hit enter and wait until it is completed.

</br>![Screenshot (207)](https://github.com/user-attachments/assets/74672a0b-e5b2-4f42-9706-cff6ddf340d8)

</br> splunk will be located in the /opt/splunk directory, cd the and ls -la to see that the user and group belongs to splunk. Change into the user splunk by typing sudo -u splunk bash. We will cd into bin directory and used the binary command "./splunk start". Hit Q and Y to accept user agreements and input a username and password. 

</br> When installation is complete use this command to make sure splunk starts up evertime VM reboots. We want to exit from the splunk user "exit" command. We will then "cd bin" as our user into our bin and then type "sudo ./splunk enable boot-start -user splunk".


</br>1.) We will open our target windows machine and rename the PC to target-pc, by type pc in search bar and clicking properties. We will then "Rename this pc" to target-pc, next and restart the vm. 

</br>2) Go to cmd to check ip, ipconfig and change IP for this machine as needed. We will need to change the machines IP by right clicking the network icon, click open net. and intenet setting, ethernet, change adaptor options, right click adaptor, click properties, and double click the internet protocol version 4 or properties.

</br> click "use the following IP address and input the assigned IP we made in the diagram. IP of 192.168.10.100 A subnet of 255.255.255.0 and a defualt gateway of 192.168.10.1, DNS server 8.8.8.8 and hit okay. Re-check IP. 

</br>![Screenshot (208)](https://github.com/user-attachments/assets/142ad808-d8f9-4d16-bd14-f62cf55570f8)

</br>![Screenshot (210)](https://github.com/user-attachments/assets/7d1c60e4-dfe5-4436-b907-020f2dc989f6)



</br> Go to the internet browser to go to our splunk server listening on 192.168.10.10:8000 (splunk server must be up)

</br> We will install splunk universial forwarder on the target vm. Go to splunk.com on the vm and download the Splunk Universal Forwarder as a Windows-64 bit package ( must be signed into account). 

</br> Double click the download packages and agree to the license agreement, make sure "An on-premises Splunk Enterprise instance" box is selected, hit next > user name left as admin and random generated password selected. Skip deploying server, receiving indexer should be the splunk server 192.10.10.10 : 9997,  then install. 

</br> We will download sysmon from micrsoft, with sysmon configuration by olaf on GitHub, find sysmonconfig.xml, click raw, save as, and save in download directory on vm machine. Go to downloads and extract all from Sysmon zip folder. 

</br> https://github.com/olafhartong/sysmon-modular

</br>Open and copy the file explorer bar of the extract folder and go to Powershell (run as admin). Cd into the copied file and type .\Sysmon64.exe -i ..\sysmonconfig.xml. Hit agree to install sysmon.

</br>![Screenshot (211)](https://github.com/user-attachments/assets/5a150081-a3d4-4bb9-bf48-a6e0be222898)


</br> We will then instruct splunk forworder on what we want to send over to our splunk server. Configure file called "inputs.conf" found at "This PC" , Local C drive, program files, splunk universal forwarder, etc, system, default. Copy the inputs.conf file, make a new file under the local directory of system files . Do not edit the inputs.conf under the default directory. 

</br> Open notepad as admin, from the github  https://github.com/MyDFIR/Active-Directory-Project , copy and paste the "inputconfig"  file into your notepad. Take note of the " index=endpoint ". Save as "inputs.conf" under the profile files > universal forwarders > etc, system, local as a "all files" for the "Save as type". Save

</br>![Screenshot (212)](https://github.com/user-attachments/assets/659407bd-a5f0-4675-a703-0991cb536413)

</br> We will now restart splunks universal forwarders service. Search up services, run as admin, type "s" to find "splunk forwarder services". When this is found you should see NT Service under the splunk forwarder services tab "Log on As". Click on the splunk forwarder service. Select local system account, then hit "okay" after the message appears. You will now notice the "Log on As" is now " local system" on splunk service. You will also notice the sysmon64 is running under it. 

</br> Right-click splunk forwarder, click restart, clcik okay after services message, then click start to start the services. 

</br>![Screenshot (213)](https://github.com/user-attachments/assets/ffaa713a-50e0-474f-9df0-2fbf240d8abf)

</br>![Screenshot (214)](https://github.com/user-attachments/assets/0617a36e-7af1-48cc-8147-5d45d258e648)

</br>![Screenshot (215)](https://github.com/user-attachments/assets/fb843392-1dbb-4556-882c-ef6669d4a511)

</br> We will not go to our splunk enterprise via the browser on the Windows Machine at 192.168.10.10:8000 and login using the same credentials to log in with our splunk server. We then navigate to the settings tab and click on "Indexes". This is where we will create the index "endpoint" for the input.conf file to send indexes to. 

</br> Click New index on the top right. Then type "endpoint" as the Index Name and save. Double check by scrolling down.

</br>![Screenshot (216)](https://github.com/user-attachments/assets/b9c908b3-fb4a-42b4-bc57-0e38d136f5d5)


</br>![Screenshot (217)](https://github.com/user-attachments/assets/75e2e816-7d42-4be7-b536-d431d10bcdff)

</br> We will now make sure the splunk server receives the data. Settings, Forwarding and Receiving, configure receiving, new recieving port, and type 9997 and save. Once set up, click app to on the top left nav bar. Go to search and reporting , skip, skip tour and type in the Search bar , " index=endpoint" with a time frame of "last 24 hours". You will see different events happening on the system that have been recorded.

</br>![Screenshot (218)](https://github.com/user-attachments/assets/278f66cf-38ed-4875-bdef-4a7b86940174)


</br>You will see in the host tab we have our "target-pc"

</br>![Screenshot (219)](https://github.com/user-attachments/assets/0d5ac2c2-3c2a-4436-a1ed-31e9bbfffeb9)


</br>You will also see the sources and source type, where our inputs.conf and its specified security, system, application and sysmon data. 

</br>![Screenshot (220)](https://github.com/user-attachments/assets/95f06fcc-220a-4cc0-88e8-02bce23a698b)


</br>We will do the same steps that we did on the windows machine onto the windows server.  Hint( If you try to connect to the splunk server and it is not listening, go the the server an input the command , "systemctl status <nameofservice>" to check the status, if services status is "-1/FAILURE" we need to type in the command, "sudo systemctl restart <nameofservice>" to restart the service, re-check the status, it should now be running)

</br>![Screenshot (222)](https://github.com/user-attachments/assets/8d5ce4de-6347-4bf1-af4a-1ea25bd337aa)

<h2> Install and Configure Active Directory; Promote Domain Controller</h2>

</br>We will log into our windows server machine and click on the server manager. Here we will click on manage at the top right. We will go through the prompt to click next on the "Before you begin page, enable "Role-based or feature-based installation" then click next, select the ADDC (should only be one), click next, We want to select the Active directory Domain Services" button and continue to move forward click next until it is installed ( Configuration required installion seucceded on ADDC01) when finished.

</br>![Screenshot (223)](https://github.com/user-attachments/assets/a4e6723f-3662-449b-9e5c-02c4888f711f)

</br>Go back to the service manager and click the flag icon. Here you will notice "promote this server to a domain controller", click this option. 

</br> We will then click "Add a new forest" and give our Domain a name with a top level domain. (e.g ADHomeLab.local). Continue through prompt, provide a password, next>, NetBIOS domain name will appear, click the empty bar and continue. In the "Paths" part, it will show paths to store database files (NTDS , SYSVOL). These are common files that can be a clear indicator a system is compromised . Click next until  prerequisites check out and installation is completed. Server should restart and our new sign in screen should show a back slash indicating we installed ADDC and promoted server to a DC. 

</br>![Screenshot (224)](https://github.com/user-attachments/assets/b174310b-34fd-4922-9ae2-75b4e7b30870)

</br>![Screenshot (226)](https://github.com/user-attachments/assets/60a44149-4e50-4251-9c8b-86a789fe8a53)

</br>![Screenshot (227)](https://github.com/user-attachments/assets/885163c7-7185-4a7f-9712-577c3e6cd486)

</br>![Screenshot (228)](https://github.com/user-attachments/assets/adaad8df-ac80-47bd-9ee8-14c582218fb8)

</br> We will now create users, log into server and go to service manager. We will click on tools on the top right corner and select "active directory users and computers". Click the domain, go to builtin and right click on the right hand side, click new, click group and give the group a name. Click OK. Double Click the test group that was just made then click member of and click add. We will now add administrators then click okay. 

</br> Right click domain > new > click "Organizational Unit" to create a department called "IT"

</br>![Screenshot (229)](https://github.com/user-attachments/assets/05b381e8-3da1-4ffb-8f1c-352402d36558)


</br>![Screenshot (230)](https://github.com/user-attachments/assets/43129498-8e7a-4b28-ab83-ab2424c93301)

</br> We will now right click the OU to create a new user and name them Jenny Smith and username will be jsmith. We will also uncheck the "user must change password at next logon" because this is simply a lab environment. Give the user a simple password then hit next, then finish. 

</br>We will continue the same steps to create new OU (HR) and new user (Terry Smith, tsmith and a password for the user).

</br>![Screenshot (231)](https://github.com/user-attachments/assets/c56c44d9-9fe6-4bee-92d5-8816dbc7aef3)


</br>![Screenshot (232)](https://github.com/user-attachments/assets/46ff4e15-6ce9-46ac-ace6-f0517b0e4482)

</br> We will now go to the windows target machine and join it to our new domain we created and authenticate it with jenny smith's account. 

</br> On the windows 10 machine go to pc > properties > advance system settings >computer name >change > select domain > and type in you domain name. If presented with error go to network and internet setting, change adaptor options,right click adaptor, select properties and click IPv4 and change our DNS to our Domain contoller (192.168.10.7). Then click ok.

</br>![Screenshot (233)](https://github.com/user-attachments/assets/dba78761-73e0-4717-87dc-0bcc08c56805)

</br>![Screenshot (234)](https://github.com/user-attachments/assets/366dded3-e298-4926-bf85-36345c2bab72)

</br> Return back to naming the domain and click ok. We then will input the administrator and password of the server which will have the proper permissions. A Welcome message should appear and a restart should appear. 

</br>We will now login with jenny smith. select other user and restart, make sure the domain is the one we created. We will use jsmith and password and we will be able to login. 


