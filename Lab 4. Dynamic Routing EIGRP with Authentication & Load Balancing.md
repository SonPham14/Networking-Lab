## Lab 4. Dynamic Routing EIGRP with Authentication & Load Balancing
### Topology

<img src=https://github.com/user-attachments/assets/ce04f578-12f2-4879-8bbd-5a114dca05b5 width="100%">

### Interface config

```
Router(config)#hos R2
R2(config)#int fa0/0
R2(config-if)#ip address 192.168.2.2 255.255.255.0
R2(config-if)#no shut
R2(config-if)#ex

R2(config)#int fa0/1
R2(config-if)#ip address 192.168.3.2 255.255.255.0
R2(config-if)#no shut
R2(config-if)#ex

R2(config)#int loopback 2
R2(config-if)#ip address 172.16.2.1 255.255.255.0
R2(config-if)#ex
```
*Note: R0 and R1 are the same*

<img src=https://github.com/user-attachments/assets/126d6040-b040-4c76-b0ac-162fa26165c5 width="100%">

### EIGRP config

```
R0(config)#router eigrp 100
R0(config-router)#network 172.16.0.1 0.0.0.0
R0(config-router)#network 192.168.1.0 0.0.0.255
R0(config-router)#network 192.168.3.0 0.0.0.255
R0(config-router)#ex
```
*Note: R1 and R2 are the same*

<img src=https://github.com/user-attachments/assets/dd266284-ff2a-4def-b60c-55cb4e1a46aa width="100%">

### Authentication

```
R0(config)#key chain R0Chain
R0(config-keychain)#key 1
R0(config-keychain-key)#key-string R0Key1
R0(config-keychain-key)#accept-lifetime 09:00:00 Apr 30 2024 infinite
R0(config-keychain-key)#send-lifetime 09:00:00 Apr 30 2024 09:01:00 Apr 30 2024
R0(config-keychain-key)#ex
R0(config-keychain)#key 2
R0(config-keychain-key)#key-string R0Key2
R0(config-keychain-key)#accept-lifetime 09:00:00 Apr 30 2024 infinite
R0(config-keychain-key)#send-lifetime 09:00:00 Apr 30 2024 infinite

R0(config)#int range fa0/0 - 1
R0(config-if)#ip authentication mode eigrp 100 md5
R0(config-if)#ip authentication key-chain eigrp 100 R0Chain
```

<img src=https://github.com/user-attachments/assets/391a6ffe-42c9-4f3e-afdc-dff2e46fb0bc width="100%">

### Load Balancing

...continue...
