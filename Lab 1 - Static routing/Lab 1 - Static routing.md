## Lab 1. Static Routing
### Topology

<img src=https://github.com/user-attachments/assets/8d282089-b4b9-4f3d-9aa7-f1262a8d7dd7 width="100%">

#### Router R1
- Interface config
```
Router(config)#hos R1
R1(config)#int fa0/0
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#no shut
R1(config-if)#ex

R1(config)#int fa0/1
R1(config-if)#ip address 10.0.0.1 255.255.255.0
R1(config-if)#no shut
R1(config-if)#ex
```

<img src=https://github.com/user-attachments/assets/828bcfcd-0a87-4edf-81e1-ea15c0977b1a width="100%">

- Static routing

`R1(config)#ip route 172.16.0.0 255.255.255.0 192.168.1.2`

<img src=https://github.com/user-attachments/assets/861944c0-85ee-40a5-8d7b-33daadd36ba7 width="100%">

#### Router R2

- Interface config

```
Router(config)#hos R2
R2(config)#int fa0/0
R2(config-if)#ip address 192.168.1.2 255.255.255.0
R2(config-if)#no shut
R2(config-if)#ex

R2(config)#int fa0/1
R2(config-if)#ip address 172.16.0.1 255.255.255.0
R2(config-if)#no shut
R2(config-if)#ex
```

<img src=https://github.com/user-attachments/assets/c3ad47cf-352a-4945-9210-c51cd244ed44 width="100%">


#### Static routing

`R2(config)#ip route 10.0.0.0 255.255.255.0 192.168.1.1`

<img src=https://github.com/user-attachments/assets/ca9fe045-173d-4c3e-9bf7-6877c4bf9aa0 width="100%">

Pinging from Network 10.0.0.0/24 to 172.16.0.0/24 and vice versa

<img src=https://github.com/user-attachments/assets/479f2ec2-34ce-4939-9326-32a68c14da4f width="100%">

<img src=https://github.com/user-attachments/assets/0c456b53-362c-429b-b4a7-4b9284d5cc49 width="100%">
