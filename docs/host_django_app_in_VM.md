## Hosting Django App in VM
To hosting a django app in a VM you need to follow the following basics steps:-

- Make a network security group
    Network Security Group (NSG) provides a complete security to your app. You can set the inbound and outbound rules for incoming and outgoing traffic.

- Make a virtual network
    Virtual Network helps you to communicate with on-premise network, internet and virtual machines. 
- Make a subnet
    Also called as subdivision of an IP network.
- Make a VM with a virtual network and that subnet

- Allow http, https, ssh and port 8000 in Network Security Group
    You have to allow port 22, 443 and 80 for ssh, https and http respectively for running internet on the VM also so that you can ssh into the VM.

- Allow Host
    Go to your django app settings and add IP of your VM , localhost and 0.0.0.0 to the allowed hosts list. 

- Running the server
  ```
  python3 manage.py runserver 0.0.0.0:8000
  ```
