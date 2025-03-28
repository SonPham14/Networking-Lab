## Lab 5. NAT Overload vs DHCP

### Topology

<img src=https://github.com/user-attachments/assets/706860d6-d2e9-4ce5-89b3-d5032c757ce1 width="100%">

### Interface configuration

```
Router(config)#hos RouterGW
RouterGW(config)#int fa0/1
RouterGW(config-if)#ip address 192.168.1.1 255.255.255.0
RouterGW(config-if)#no shut
RouterGW(config-if)#ip nat inside
RouterGW(config-if)#ex

RouterGW(config)#int fa0/0
RouterGW(config-if)#no shut
RouterGW(config-if)#ip address dhcp
RouterGW(config-if)#ip nat outside
RouterGW(config-if)#ex
```

### DHCP pool conffiguration

```
RouterGW(config)#ip dhcp pool dhcp_ip
RouterGW(dhcp-config)#network 192.168.1.0 255.255.255.0
RouterGW(dhcp-config)#default-router 192.168.1.1
RouterGW(dhcp-config)#ex

RouterGW(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.100
```

```
Internet(config)#ip dhcp  pool dynamic_ip_internet
Internet(dhcp-config)#net
Internet(dhcp-config)#network 172.16.1.0 255.255.255.0
Internet(dhcp-config)#def
Internet(dhcp-config)#default-router 172.16.1.1
Internet(dhcp-config)#exit
```
#### DHCP Request successful

<img src=https://github.com/user-attachments/assets/38aaa902-56da-48df-8ff6-6ea9cb62c2e9 width="100%">
<p></p>

### NAT config

- Static route
```
RouterGW(config)#ip route 0.0.0.0 0.0.0.0 fa0/0
Internet(config)#ip route 0.0.0.0 0.0.0.0 fa0/0
```

// The two commands below use NAT Overload to translate the entire range of private IP addresses into a single public IP address, allowing the entire LAN to access external networks.

```
RouterGW(config)#access-list 1 permit 192.168.1.0 0.0.0.255
RouterGW(config)#ip nat inside source list 1 interface fa0/0 overload
```

### Show IP NAT translate

`RouterGW(config)#do show ip nat static`

<img src=https://github.com/user-attachments/assets/1c92d0f9-df14-496f-9dee-5d2d78945abc width="100%">

`RouterGW(config)#do show ip nat trans`

<img src=https://github.com/user-attachments/assets/d27af107-e865-4d0f-ac78-dbab07934ae4 width="100%">
