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

