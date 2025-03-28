## Lab 3. Dynamic Routing OSPF with Authentication
### Topology

<img src=https://github.com/user-attachments/assets/97c25e69-3aed-4bac-aff3-9060216fb331 width="100%">

### Interface config

```
Router(config)#hos R0
R0(config)#int fa0/0
R0(config-if)#ip address 192.168.0.1 255.255.255.0
R0(config-if)#no shut
R0(config)#int fa0/1
R0(config-if)#ip address 192.168.1.1 255.255.255.0
R0(config-if)#no shut
```

### OSPF Config

```
R0(config)#router ospf 10
R0(config-router)#network 192.168.0.0 0.0.0.255 area 0
R0(config-router)#network 192.168.1.0 0.0.0.255 area 0
R0(config-router)#ex
```
*Note: R1 & R2 as the same*

<img src=https://github.com/user-attachments/assets/b37e4f75-cdfd-471f-a641-1de5a992220c width="100%">
<p></p>
<img src=https://github.com/user-attachments/assets/ab3d950f-234f-441f-992f-9db4d4d2f743 width="100%">
<p></p>
<img src=https://github.com/user-attachments/assets/237ae985-86a1-4ec4-9336-80b989dca556 width="100%">

### Authentication

#### Using Plain text

```
R0(config)#int fa0/0
R0(config-if)#ip ospf authentication-key plainpas
R0(config-if)#ip ospf authentication
R0(config-if)#ex
```

<img src=https://github.com/user-attachments/assets/4f0aae0f-dab9-4fb0-afbd-b07e711686ff width="100%">

#### Using MD5 Encrypt

```
R0(config)#int fa0/0
R0(config-if)#ip ospf message-digest-key 1 md5 keymd5
R0(config-if)#ip ospf authentication message-digest
R0(config-if)#ex
```

<img src=https://github.com/user-attachments/assets/729d36b3-b1b3-400e-9a58-44e2fb70db99 width="100%">
