## NAT Web

### Topology

<img src=https://github.com/user-attachments/assets/769a2f08-85a0-4fda-a3aa-97894022eec0 width=100%>
<p></p>
 
*NOTE: Config VTP, DHCP, Sub-Interface,… as same as Lab 7*

### 1. Web_Server config

```
hostname Web_Server
ip name-server 8.8.8.8
username dtu privilege 15 secret 5 123
interface FastEthernet0/0
	ip address 192.168.100.100 255.255.255.0
	no shut
ip http server
ip http authentication local
ip http secure-server
ip domain-lookup
ip route 0.0.0.0 0.0.0.0 192.168.100.254
```

### 2. RouterGW

```
hostname RouterGW
ip name-server 8.8.8.8
interface FastEthernet0/0
	ip address dhcp
	ip nat outside
interface FastEthernet0/1
	ip address 192.168.1.254 255.255.255.0
	ip nat inside
interface FastEthernet0/1.10
	encapsulation dot1Q 10
	ip address 192.168.10.254 255.255.255.0
	ip helper-address 192.168.1.100
interface FastEthernet0/1.20
	encapsulation dot1Q 20
	ip address 192.168.20.254 255.255.255.0
	ip helper-address 192.168.1.100
interface FastEthernet0/1.100
	encapsulation dot1Q 100
	ip address 192.168.100.254 255.255.255.0
	ip nat inside
access-list 1 permit 192.168.1.0 0.0.0.255
access-list 1 permit 192.168.10.0 0.0.0.255
access-list 1 permit 192.168.20.0 0.0.0.255
access-list 1 permit 192.168.100.0 0.0.0.255
ip nat inside source list 1 interface FastEthernet0/0 overload
ip nat inside source static tcp 192.168.100.100 80 192.168.114.147 80 extendable
```

### 3. Internet

In virtual machine setting, the first virtual network card is VMNET8, so the type of network will be choose Cloud0 in this topology.

<img src=https://github.com/user-attachments/assets/de74b870-6917-4d0c-b186-e1697682c011 width=100%>
<p></p>
<img src=https://github.com/user-attachments/assets/e4330d5e-fcb1-4170-9824-834cc3488e1b width=100%>

Finally, on your local computer, open the browser and go to 192.168.114.147 to access the web server.
