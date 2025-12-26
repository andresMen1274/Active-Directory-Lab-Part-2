# Active-Directory-Lab-Part-2
In this part of the active directory lab I will install and configure sysmon and splunk onto the windows target machine and windows server.

First we want to configure a NAT network so all machines are able to connect to the internet. To do this first we want to navigate to tools, select network, and NAT networks. Select create and enter in a name for the network and enter the IP address of the network. Then for each of the virtual machines we are going to navigate to the network settings. then change the defalut NAT to the NAT network option that we just created. 

<img width="563" height="292" alt="image" src="https://github.com/user-attachments/assets/a2f3ce2d-d7bb-4ae3-9991-51b68bc6c573" />

<img width="957" height="592" alt="image" src="https://github.com/user-attachments/assets/3bb08ae4-cb7b-4c72-abbe-df47fec6975b" />

Then we are going to open the splunk server and use the command ip a. Then we should see that the splunk server is connected to the NAT network and has an ip in the range.

<img width="826" height="97" alt="image" src="https://github.com/user-attachments/assets/4af71222-eea7-4e2b-8f72-2f8e730b6c3d" />

Then we want to set a static IP address for the splunk server. To do this we enter the command sudo nano /etc/netplan/50-cloud-init.yaml and change the file to look like this.

<img width="277" height="197" alt="image" src="https://github.com/user-attachments/assets/44f549d6-c0df-4c62-a468-0e75f8f41708" />

Then we save the file and clear the screen. Enter in the command sudo netplan apply and this will apply the new changes. Then we are going to enter the command ip a and confirm that the ip address is 192.169.1.10 as shown below.

<img width="813" height="101" alt="image" src="https://github.com/user-attachments/assets/a0cac7f9-2a93-49d7-b763-ecda35a9b654" />

Then we will ping google.com to make sure that the file is configured correctly.

<img width="761" height="81" alt="image" src="https://github.com/user-attachments/assets/43dcda54-48dc-4a22-88d1-0a69f1a39f17" />

Next we want to install the splunk server to do this we will go to splunk.com and create an account. After that has been done naviagte to splunk.com and select splunk enterprise then download. When the download has been completed we open the ubuntu server and enter the command sudo apt-get install virtualbox then tab to view the options. We want the guest additions iso therefore, we will enter that in. 

<img width="912" height="83" alt="image" src="https://github.com/user-attachments/assets/ac7a5a8a-ee80-4446-9fd6-4daf458b5a88" />

When the download has been completed we are going to create a folder named shared folder and put in the downloaded splunk server DEB file. Then in virtualbox we will go to settings then shared folder and select add. Then select all of the boxes below and select ok. 

<img width="372" height="311" alt="image" src="https://github.com/user-attachments/assets/f9348fbb-e32c-43fc-9d63-706a32534024" />

Reboot the server with the command sudo reboot and login. We are going to add a new user to the vboxsf and we get the prompt vboxsf doesn't exist. we are going to again enter the command sudo apt=get install virtualbox with a tab to view the installations. We will select the -guest-utils.

<img width="920" height="192" alt="image" src="https://github.com/user-attachments/assets/73645394-96d4-4cf5-bded-bac7df80b1db" />

Then run the command sudo adduser andres vboxsf and then make a shared directory with the command mkdir share. Now we want to mount the shared folder to the share directory and to do this run the command sudo mount -t vboxsf -o uid=1000,gid=1000 <shared folder name> share/. Then cd into the share directory and la -la and we should be able to see the splunk download file. Run the command sudo dpkg -i <splunk server name>. 

<img width="1050" height="357" alt="image" src="https://github.com/user-attachments/assets/aeab454f-01f8-46ab-a388-d9e73a0a65b7" />

Now we want to check that splunk has been configured correctly. To do this we will cd into the /opt/splunk directory and the use ls -la to check the contents. 

<img width="798" height="372" alt="image" src="https://github.com/user-attachments/assets/d4ee2e87-5087-4c8b-8a34-d7450f739e5e" />

We can see that all of the files are owned by the splunk user, so we will switch to that user by using the command sudo -u splunk bash. To run the installer we will cd into the bin directory then run the command ./splunk start to start the installer. Now we want splunk to run everytime the virtual machine so we will exit. Cd into the bin directory and then run the command sudo ./splunk enable boot-start -user splunk. Now the splunk server will start.

Now we want to install the universal forwarder on the Windows target machine we will start the virtual machine. Then we will search splunk.com and navigate to the splunk universal forwarder then download the Windows 64-bit version. 

<img width="1022" height="765" alt="image" src="https://github.com/user-attachments/assets/1a676eb4-8323-4bd0-a09f-27d700bd13ce" />

Open the file when it has finished downloading and select next until you reach recieveing indexer. Then enter in the IP address of the splunk server that we previously configured and enter in 9997 for the other field. Now we will download Sysmon search sysmon and then download it from the Microsoft website. 

<img width="1022" height="765" alt="image" src="https://github.com/user-attachments/assets/7ba87eb7-8928-4e51-aada-93fdd69d29ef" />

Then we want to use the olaf configuration of sysmon, so we are going to find this github repository and select sysmonconfig.xml.

<img width="1018" height="767" alt="image" src="https://github.com/user-attachments/assets/aabbb09e-fdef-4ece-b234-5020ad775811" />

After selecting the file we select raw and save it as an xml file. Next we navigate to fiel explorer and then select the sysmon zip file and click extract all. When it has finished extracting the contents then copy the file path and open powershell with administrative privlages. Now we are going to cd into the directory where sysmon was configured in and then enter the command shown below.

<img width="507" height="98" alt="image" src="https://github.com/user-attachments/assets/a8f195ac-17e4-433c-b85a-3de3bd3ea374" />

Now we are going to tell our splunk forwarder what to forward to the splunk server. To do this we go to the program files that are used for the splunk universal forwarder and create a input.conf file that is in the local directory. We open notepad in administrative mode and then enter whats shown below into the file. 

<img width="751" height="517" alt="image" src="https://github.com/user-attachments/assets/1dec5edc-640a-4fa2-a7ec-366ac74e1111" />

Then save it in the local directory as input.conf 

<img width="608" height="472" alt="image" src="https://github.com/user-attachments/assets/203f5e04-d8c0-45fd-9636-f35881198e11" />

Since we modified the inputs now we have to reset the universal forwarder and we do this by going to services and run as administrator. Then we find splunk forwarder and navigate to log on and change it to local system account. Right click the splunk forwarder and then select restart.  

<img width="801" height="586" alt="image" src="https://github.com/user-attachments/assets/59bda40a-fe6a-40d5-a9b4-46f3c403d6a8" />

This error screen appeared we will just select start service. Now we will look up the IP address of the splunk server and will login with out splunk server credentials. Then go to settings and indexes. We need to create a new index, so out input.conf file will start sending logs to the Splunk server. 

<img width="1018" height="850" alt="image" src="https://github.com/user-attachments/assets/c985bfdd-586a-46be-9fe9-4cc5bac7809e" />

Select new Index and name it endpoint and save. Then we navigate to settings again and then select forwarding and recieving. Then select configure recieving and new recieving port. Enter the 9997 default port. 
The configureation for the server is the same as the configuration of the target machine. Therefore, it will not be repeated or this will seem very redundant.

Now I will configure the active directory and promote domain controller on the Windows server. Which will allow the Windows PC to join the domain.

First we want to add role based features for our Windows server. To do this we naviagte to server manager and select manage, role based features, next, and select role-based or feature-based installation. Chose the Winodws server that we have configured and select Active Directory Domain Services as well as add features. 

<img width="1022" height="768" alt="image" src="https://github.com/user-attachments/assets/b4bdc922-b277-4abf-9e54-05eab64b8809" />

Keep selecting next until the installation button apears and select install. When finished you will recieve this prompt. 

<img width="787" height="561" alt="image" src="https://github.com/user-attachments/assets/66170b41-0709-48e7-8d18-46829959cfe2" />

close out the pop up screen and select the flag that has a yellow icon next to it. Then select promote this server to a domain controller.

<img width="1021" height="767" alt="image" src="https://github.com/user-attachments/assets/5d7729c1-572f-4ebc-a78a-c3c4d9a35fa1" />

Then select new forrest and enter a name of your choice followed by a . and a another name. Then select next until the installation button appears. When the installation has been completed it will tell the user that the computer will restart.

<img width="1023" height="767" alt="image" src="https://github.com/user-attachments/assets/1beb5965-789b-4ad9-96e3-e63474355575" />

Then login to the Windows server again. Go to the server manager and select Active Directory Users and Computrers. Now I want to create a new organizational unit and to do this I right click the name of the server and select new organizational unit. Then I will name it IT and select ok. 

<img width="750" height="527" alt="image" src="https://github.com/user-attachments/assets/cbd07f53-6440-4fe8-a810-f7d3bf7607da" />

Then we can add a user to the newly created organization by selecting new and user. give them a name and password and add them to the system.

<img width="431" height="377" alt="image" src="https://github.com/user-attachments/assets/ca2d4092-9276-4e34-82b0-343824b72ff2" />

Now we want to add the Windows machine to the domain, so login to the Windows 10 machine. Then search this PC and properties. Select advanced system settings and the computer name tab. Then select domain and eneter in the domain name of the server. 

<img width="797" height="632" alt="image" src="https://github.com/user-attachments/assets/850e66e8-9321-48c3-a164-17dda37fb911" />

We get this error because our target machine does not know how to resolve ACTIVEDIRECTORY.LOCAL. We are going to fix this be heading to the network adapter and change adapter options. 
