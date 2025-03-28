## Lab 6. VLAN, NAT, DHCP

### Topology

<img src=https://github.com/user-attachments/assets/3ec1149e-348a-4816-a8ea-276e80d30d81 width="100%">

### Switch0

```
Switch(config)#vlan 10
Switch(config-vlan)#name vlan10
Switch(config-vlan)#ex
Switch(config)#vlan 20
Switch(config-vlan)#name vlan20
Switch(config-vlan)#ex

Switch(config)#int range fa0/1 - 10
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 10

Switch(config)#int range fa0/11 - 20
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 20

Switch(config)#int fa0/24
Switch(config-if)switchport mode trunk
```

### RouterGW

#### Interface config

```
Router(config)#hostname RouterGW
RouterGW(config)#int fa0/1
RouterGW(config-if)#ip address 192.168.1.1 255.255.255.0
RouterGW(config-if)#no shut
RouterGW(config-if)#ip nat inside
RouterGW(config-if)#ex

RouterGW(config)#int fa0/0
RouterGW(config-if)#ip address dhcp
RouterGW(config-if)#ip nat outside
RouterGW(config-if)#ex
```

#### Sub-Interface config

```
Router(config)# int f0/1.10
RouterGW(config-subif)#encapsulation dot1Q 10
RouterGW(config-subif)#ip add 192.168.10.1 255.255.255.0
RouterGW(config-subif)#no shutdown

Router(config)# int f0/1.20
RouterGW(config-subif)#encapsulation dot1Q 20
RouterGW(config-subif)#ip add 192.168.20.1 255.255.255.0
RouterGW(config-subif)#no shutdown
```

#### DHCP Pool config

```
RouterGW(config)#ip dhcp pool vlan10
RouterGW(dhcp-config)#network 192.168.10.0 255.255.255.0
RouterGW(dhcp-config)#default-router 192.168.10.1
RouterGW(dhcp-config)#ex

RouterGW(config)#ip dhcp pool vlan20
RouterGW(dhcp-config)#network 192.168.20.0 255.255.255.0
RouterGW(dhcp-config)#default-router 192.168.20.1
RouterGW(dhcp-config)#ex

Router(config)#ip dhcp excluded-address 192.168.10.1 192.168.10.100
Router(config)#ip dhcp excluded-address 192.168.20.1 192.168.20.100
```

#### Access list config

```
RouterGW(config)#access-list 1 permit 192.168.1.0 0.0.0.255
RouterGW(config)#access-list 1 permit 192.168.10.0 0.0.0.255
RouterGW(config)#access-list 1 permit 192.168.20.0 0.0.0.255
RouterGW(config)#ip nat inside source list 1 int f0/0 overload
```

*Note: using static route as same as Lab 5*
