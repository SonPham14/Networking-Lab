## Lab 1. Static Routing
### Topology
[!]
Router R1
Interface config
Router(config)#hos R1
R1(config)#int fa0/0
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#no shut
R1(config-if)#ex

R1(config)#int fa0/1
R1(config-if)#ip address 10.0.0.1 255.255.255.0
R1(config-if)#no shut
R1(config-if)#ex

Static routing
R1(config)#ip route 172.16.0.0 255.255.255.0 192.168.1.2

Router R2
Interface config
Router(config)#hos R2
R2(config)#int fa0/0
R2(config-if)#ip address 192.168.1.2 255.255.255.0
R2(config-if)#no shut
R2(config-if)#ex

R2(config)#int fa0/1
R2(config-if)#ip address 172.16.0.1 255.255.255.0
R2(config-if)#no shut
R2(config-if)#ex

Pinging from Network 10.0.0.0/24 to 172.16.0.0/24 and vice versa
