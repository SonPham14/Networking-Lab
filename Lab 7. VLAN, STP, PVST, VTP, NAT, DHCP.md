## Lab 7. VLAN, STP, PVST, VTP, NAT, DHCP

### Topology

<img src=https://github.com/user-attachments/assets/82911a2d-4ed0-4e03-a2a8-0c556d767d10 width=100%>

### 1. Switch SW1

#### 1.1. VTP config

```
SW1(config)#vtp version 2
SW1(config)#vtp domain dtu
SW1(config)#vtp mode server
SW1(config)#vtp password 123
```

<img src=https://github.com/user-attachments/assets/f8bda09a-8a0c-472b-b02c-dbbd59420b6e width=100%>

#### 1.2. Default VLAN

```
SW1(config)#int vlan 1
SW1(config-if)#ip address 192.168.1.2 255.255.255.0
SW1(config-if)#no shut
SW1(config-if)#ex
```

#### 1.3. Switchport mode Trunk

```
SW1(config)#int range fa0/21 - 22
SW1(config-if-range)#switchport
SW1(config-if-range)#switchport trunk encapsulation dot1q
SW1(config-if-range)#switchport mode trunk
SW1(config-if-range)#ex

SW1(config)#int fa0/24
SW1(config-if)#switchport
SW1(config-if)#switchport trunk encapsulation dot1q
SW1(config-if)#switchport mode trunk
SW1(config-if)#ex
```

<img src=https://github.com/user-attachments/assets/809339e9-b9fc-4720-a73d-b63b16da6b3b width=100%>

#### 1.4. VLAN config

```
SW1(config)#vlan 10
SW1(config-vlan)#name vlan10
SW1(config-vlan)#ex

SW1(config)#vlan 20
SW1(config-vlan)#name vlan20
SW1(config-vlan)#ex

SW1(config)#int range fa0/1 - 10
SW1(config-if-range)#switchport mode access
SW1(config-if-range)#switchport access vlan 10
SW1(config-if-range)#ex

SW1(config)#int range fa0/11 - 20
SW1(config-if-range)#switchport mode access
SW1(config-if-range)#switchport access vlan 20
SW1(config-if-range)#ex
```

<img src=https://github.com/user-attachments/assets/31ff56a7-4d3e-4707-a512-5e2012b860f0 width=100%>

#### 1.5. PVRST+ mode
```
SW1(config)#spanning-tree mode rapid-pvst
SW1(config)#spanning-tree vlan 10 root primary
SW1(config)#spanning-tree vlan 20 root secondary
```

<img src=https://github.com/user-attachments/assets/2abdee3f-6a72-49da-88f4-cb58187222bf width=100%>
<p></p>
<img src=https://github.com/user-attachments/assets/2c65f07c-116a-4205-9a38-19627fff24f5 width=100%>

### 2. Switch SW2

#### 2.1. VTP config

```
SW2(config)#vtp version 2
SW2(config)#vtp mode client
SW2(config)#vtp domain dtu
SW2(config)#vtp password 123
```

<img src=https://github.com/user-attachments/assets/b95fe9da-063f-48eb-b01a-b8d9cf847594 width=100%>
<p></p>
<img src=https://github.com/user-attachments/assets/f8904f95-c52a-406a-8266-c93c70c93f66 width=100%>

#### 2.2. Default VLAN

```
SW2(config)#int vlan 1
SW2(config-if)#ip address 192.168.1.3 255.255.255.0
SW2(config-if)#no shut
SW2(config-if)#ex
```

#### 2.3. VLAN config

```
Switch(config)#hos SW2
SW2(config)#int range fa0/1 - 10
SW2(config-if-range)#switchport mode access
SW2(config-if-range)#switchport access vlan 10
SW2(config-if-range)#ex

SW2(config)#int range fa0/11 - 20
SW2(config-if-range)#switchport mode access
SW2(config-if-range)#switchport access vlan 20
SW2(config-if-range)#ex

SW2(config)#int range fa0/21 - 22
SW2(config-if-range)#switchport
SW2(config-if-range)#switchport trunk encapsulation dot1q
SW2(config-if-range)#switchport mode trunk
SW2(config-if-range)#ex
```

<img src=https://github.com/user-attachments/assets/67fa7651-7c3b-4801-a1fa-99a27ad5a377 width=100%>
<p></p>
<img src=https://github.com/user-attachments/assets/78f24106-3037-42a8-aa5a-d7e7f4c930ee width=100%>

#### 2.4. PVRST+ mode

```
SW2(config)#spanning-tree mode rapid-pvst
SW2(config)#spanning-tree vlan 20 root primary
SW2(config)#spanning-tree vlan 10 root secondary
```

<img src=https://github.com/user-attachments/assets/04962f9b-db46-4c28-81a7-41c95d13ab38 width=100%>
<p></p>
<img src=https://github.com/user-attachments/assets/3de1b9bc-66a4-4c19-8b2e-acf4e7af0e2a width=100%>

### 3. Router R1

#### 3.1. Interface config

```
R1(config)#int fa0/0
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#no shut
R1(config-if)#ip nat inside

R1(config)#int fa0/1
R1(config-if)#no shut
R1(config-if)#ip address dhcp
R1(config-if)#ip nat outside
```

<img src=https://github.com/user-attachments/assets/f59df0d8-b437-41d4-af03-ac51f6bc54e8 width=100%>

#### 3.2. Sub–Interface config

```
R1(config)#int fa0/0.10
R1(config-subif)#encapsulation dot1Q 10
R1(config-subif)#ip address 192.168.10.1 255.255.255.0
R1(config-subif)#no shut

R1(config)#int fa0/0.20
R1(config-subif)#encapsulation dot1Q 20
R1(config-subif)#ip address 192.168.20.1 255.255.255.0
R1(config-subif)#no shut
```

#### 3.3. IP helper

```
R1(config)#int fa0/0.10
R1(config-subif)#ip helper-address 192.168.1.100
R1(config)#int fa0/0.20
R1(config-subif)#ip helper-address 192.168.1.100
```

#### 3.4. Access list & NAT config

```
R1(config)#access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)#access-list 1 permit 192.168.10.0 0.0.0.255
R1(config)#access-list 1 permit 192.168.20.0 0.0.0.255
R1(config)#ip nat inside source list 1 interface fa0/1 overload
```

*NOTE: Config DHCP server on Windows Server to solve this lab*
