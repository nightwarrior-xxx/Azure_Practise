## Hosting Django App in VM
- Make a network security group
- Make a virtual network
- Make a subnet
- Make a VM with a virtual network and that subnet
- Allow http, https, ssh and port 8000 in nsg
- Add ip of vm and 0.0.0.0:8000 in setting.py 
- runserver 0.0.0.0:8000
