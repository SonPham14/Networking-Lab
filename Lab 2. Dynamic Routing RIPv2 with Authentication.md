## Lab 2. Dynamic Routing RIPv2 with Authentication

### Topology

<img src=https://github.com/user-attachments/assets/f946e918-64b8-48f1-ae1b-3f45116ae804 width="100%">

### Interface config

- Router R1

```
Router(config)#hos R1
R1(config)#int fa0/0
R1(config-if)#ip address 192.168.1.1 255.255.255.252
R1(config-if)#no shut
R1(config-if)#ex

R1(config)#int fa0/1
R1(config-if)#ip address 192.168.3.2 255.255.255.252
R1(config-if)#no shut
R1(config-if)#ex

R1(config)#int loopback 1
R1(config-if)#ip address 172.16.1.1 255.255.255.0
R1(config-if)#no shut
R1(config-if)#ex
```

<img src=https://github.com/user-attachments/assets/49e70271-a605-4db9-97c6-53624fd58033 width="100%">


- Router R2

```
Router(config)#hos R2
R2(config)#int fa0/0
R2(config-if)#ip address 192.168.1.2 255.255.255.252
R2(config-if)#no shut
R2(config-if)#ex

R2(config)#int fa0/1
R2(config-if)#ip address 192.168.2.1 255.255.255.252
R2(config-if)#no shut
R2(config-if)#ex

R2(config)#int loopback 2
R2(config-if)#ip address 172.16.2.1 255.255.255.0
R2(config-if)#no shut
R2(config-if)#ex
```

<img src=https://github.com/user-attachments/assets/dcd5f620-ffb4-4435-9988-7c5bb124cf65 width="100%">

- Router R3

```
Router(config)#hos R3
R3(config)#int fa0/0
R3(config-if)#ip address 192.168.3.1 255.255.255.252
R3(config-if)#no shut
R3(config-if)#ex

R3(config)#int fa0/1
R3(config-if)#ip address 192.168.2.2 255.255.255.252
R3(config-if)#no shut
R3(config-if)#ex

R3(config)#int loopback 3
R3(config-if)#ip add
R3(config-if)#ip address 172.16.3.1 255.255.255.0
R3(config-if)#no shut
R3(config-if)#ex
```

<img src=https://github.com/user-attachments/assets/c5825af6-b079-4a71-bbc8-bd6154130e67 width="100%">

### RIPv2 Config

- Router R1

```
R1(config)#router rip
R1(config-router)#version 2
R1(config-router)#network 192.168.1.0
R1(config-router)#network 192.168.3.0
R1(config-router)#network 172.16.1.0
R1(config-router)#ex
```

- Router R2

```
R2(config)#router rip
R1(config-router)#version 2
R2(config-router)#network 192.168.1.0
R2(config-router)#network 192.168.2.0
R2(config-router)#network 172.16.2.0
R2(config-router)#ex
```

- Router R3

```
R3(config)#router rip
R1(config-router)#version 2
R3(config-router)#network 192.168.3.0
R3(config-router)#network 192.168.2.0
R3(config-router)#network 172.16.3.0
R3(config-router)#ex
```

### Show ip route rip

<img src=https://github.com/user-attachments/assets/072f21c0-ed36-43c1-9ded-7fc462c03b52 width="100%">
<p></p>
<img src=https://github.com/user-attachments/assets/bcf0e6c9-3a61-4c7b-b866-fbbe600774a5 width="100%">
<p></p>
<img src=https://github.com/user-attachments/assets/8770c1af-e9be-447b-952b-d57986bda97c width="100%">

### Show ip rip database
 
<img src=https://github.com/user-attachments/assets/b44125dd-746a-4f6b-8c02-5356e6eb57c9 width="100%">
<p></p>
<img src=https://github.com/user-attachments/assets/78ff658c-9370-4bb8-ac4c-3bfa7c4d9c98 width="100%">
<p></p>
<img src=https://github.com/user-attachments/assets/9a1f653a-3967-4898-845d-4e82c67642c7 width="100%">

### RIPv2 Authentication

- Using Plain Text (Option 1)

```
R1(config)#key chain keyset
R1(config-keychain)#key 1
R1(config-keychain-key)#key-string cisco
R1(config)#int range fa0/0 - 1
R1(config-if)#ip rip authentication key-chain keyset
```
*Note: Router R2 and R3 as the same*

<img src=https://github.com/user-attachments/assets/7aea2856-77d1-407c-b724-273a768bf8f1 width="100%">

- Using MD5 Encrypt (Option 2)

```
R1(config)#key chain keyset 
R1(config-keychain)#key 1
R1(config-keychain-key)#key-string cisco         
R1(config)#int range fa0/0 - 1
R1(config-if)#ip rip authentication key-chain keyset
R1(config-if)#ip rip authentication mode md5
```
*Note: Router R2 and R3 as the same*

<img src=https://github.com/user-attachments/assets/d2ebf317-504b-426a-8767-35f422c9d52c width="100%">
