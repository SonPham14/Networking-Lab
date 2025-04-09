## Topology

<img src=https://github.com/user-attachments/assets/69d21a8a-4a1e-4b32-90b7-8a372ac336e7 width=100%>

## Configuration

### 1. Cấu hình IP cho các PC & Các interface VLAN trên các Switch

<img src=https://github.com/user-attachments/assets/256b8f4d-96d4-419d-997d-08716e867b33 width=100%>

#### Switch10
```
Switch10(config)#interface vlan 1
Switch10(config-if)#no shutdown
Switch10(config-if)#ip address 192.168.10.10 255.255.255.0
Switch10(config-if)#exit
```

### Switch20
```
Switch>enable
Switch#
Switch#conf t
Switch(config)#hostname Switch20
Switch20(config)#interface vlan 1
Switch20(config-if)#no shutdown
Switch20(config-if)#ip address 192.168.1.20 255.255.255.0
Switch20(config-if)#exit
```
### SWCore1
```
Switch>enable
Switch#conf t
Switch(config)#hostname SWCore1
SWCore1(config)#interface vlan 1
SWCore1(config-if)#no shutdown
SWCore1(config-if)#ip address 192.168.1.1 255.255.255.0
SWCore1(config-if)#exit
```

//Tạo VLAN 10 & VLAN 20 trước khi cấu hình IP cho các Interface VLan 10 & 20

### 2. Cấu hình VTP trên SWCore1 & các switch còn lại

#### SWCore1
```
SWCore1(config)#vtp mode server
SWCore1(config)#vtp domain dtu.vn
SWCore1(config)#vtp password dtu@123
```

#### Switch10
```
Switch10(config)#vtp mode client
Switch10(config)#vtp domain dtu.vn
Switch10(config)#vtp password dtu@123
```

#### Switch20
```
Switch20(config)#vtp mode client
Switch20(config)#vtp domain dtu.vn
Switch20(config)#vtp password dtu@123
```

### 3. Cấu hình các cổng nối giữa các Switch là trunk port

#### SWCore1
```
SWCore1(config)#interface range G0/1 - 2
SWCore1(config-if-range)#switchport trunk encapsulation dot1q
SWCore1(config-if-range)#switchport mode trunk
SWCore1(config-if-range)#exit
```

#### Switch10
```
Switch10(config)#interface G0/1
Switch10(config-if)#switchport mode trunk
Switch10(config-if)#exit
```

#### Switch20
```
Switch20(config)#interface G0/2
Switch20(config-if)#switchport mode trunk
Switch20(config-if)#exit
```

### 4. Tạo VLAN 10 & 20 trên SWCore

```
SWCore1(config)#vlan 10
SWCore1(config-vlan)#name TaiChinh
SWCore1(config-vlan)#exit

SWCore1(config)#vlan 20
SWCore1(config-vlan)#name KeToan
SWCore1(config-vlan)#exit
```

Kiểm tra thông tin VLAN trên các switch:

![{FA0D63CC-04B6-4665-8B92-25A4D2D7F93E}](https://github.com/user-attachments/assets/106df531-c8c2-4934-92dc-f77f793b9ec3)

Ta thấy VLAN Database được đổ từ SWCore1 về các switch còn lại.

**Note: Nhớ cấu hình IP cho các interface vlan 10 & 20 sau bước này**

```
SWCore1(config)#interface vlan 10
SWCore1(config-if)#ip address 192.168.10.1 255.255.255.0
SWCore1(config-if)#exit

SWCore1(config)#interface vlan 20
SWCore1(config-if)#ip address 192.168.20.1 255.255.255.0
SWCore1(config-if)#exit
```

### 5. Gán port vào các VLAN

#### Switch10
```
Switch10(config)#interface range f0/1 - 10
Switch10(config-if-range)#switchport mode access
Switch10(config-if-range)#switchport access vlan 10
Switch10(config-if-range)#exit
```

#### Switch20
```
Switch20(config)#interface range f0/1 - 10
Switch20(config-if-range)#switchport mode access
Switch20(config-if-range)#switchport access vlan 20
Switch20(config-if-range)#exit
```

### 6. Kiểm tra cấu hình các switch & kiểm tra thông mạng
```
show running-config
show vlan
show vtp status
show vtp password
show interfaces trunk
show ip interface brief
```

Trước khi kiểm tra thông mạng thì cấu hình lệnh ip routing trên SWCore1

```SWCore1(config)#ip routing```

![{DD3AF057-C8B5-44E4-8AFF-4476898C3102}](https://github.com/user-attachments/assets/243d5478-55cd-4c11-9b6d-dc8cbb225625)

### 7. Thêm 1 Switch SWCore2 vào hệ thống

Cấu hình VTP như SWCore1; nhưng chúng ta tạo nhiều VLAN hơn, mục đích là để cho số Revision Number tăng lên sao cho lớn hơn bên SWCore1 là được. Sau đó nối SWCore2 vào hệ thống và quan sát VLAN Database của các switch còn lại.

```
Switch(config)#hostname SWCore2
SWCore2(config)#vtp mode server
SWCore2(config)#vtp domain dtu.vn
SWCore2(config)#vtp password dtu@123

SWCore2(config)#
SWCore2(config)#vlan 10
SWCore2(config-vlan)#name
SWCore2(config-vlan)#name TC

SWCore2(config-vlan)#vlan 20
SWCore2(config-vlan)#name KT

SWCore2(config-vlan)#vlan 30
SWCore2(config-vlan)#name KD

SWCore2(config-vlan)#vlan 40
SWCore2(config-vlan)#name KyThuat

SWCore2(config-vlan)#vlan 50
SWCore2(config-vlan)#name Kho

SWCore2(config-vlan)#vlan 60
SWCore2(config-vlan)#name IT
SWCore2(config-vlan)#exit
```

Xóa VLAN 10 & 20

```
SWCore2(config)#no vlan 10
SWCore2(config)#no vlan 20
```

Quan sát thông tin **VTP** trên SWCore2

Hiện tại SWCore2 có Revision Number = 14

<img src=https://github.com/user-attachments/assets/f3c45c5b-c1e6-4988-bd78-32867eca2b37 width=100%>

Quan sát thông tin **VLAN** trên SWCore2

![{E18E2A71-47EF-4592-B127-DCB548DF5732}](https://github.com/user-attachments/assets/ab57656d-3df6-4e57-b49b-7da5753d18fd)


Cấu hình cổng G0/1 trên SWCore2 & Switch10 là trunk port & sau đó nối SWCore2 vào Switch20 

```
SWCore2(config)#interface G0/1
SWCore2(config-if)#switchport trunk encapsulation dot1q
SWCore2(config-if)#switchport mode trunk
SWCore2(config-if)#exit
```

```
Switch20(config)#interface G0/1
Switch20(config-if)#switchport mode trunk
Switch20(config-if)#exit
```

![{4A23C6B4-2315-4272-B7D1-7932EA2C20BF}](https://github.com/user-attachments/assets/b01a81c0-91a0-4e3d-8491-4765ef1d0cfd)


Sau khi nối SWCore2 vào hệ thống, chờ vài phút, chúng ta kiểm tra lại thông tin VLAN trên tất cả các Switch & Xem lại thông tin VTP trên SWCore1 & SWCore2.

![{DD03B79E-DBAF-4C71-B1A9-7E8B25AC394F}](https://github.com/user-attachments/assets/a3150e38-0733-487f-aa89-3c07210e6e6d)


![{74113E57-C909-45CD-B729-69892D1C1D34}](https://github.com/user-attachments/assets/011d897b-a78d-4e98-848f-3333a90f9506)


Quan sát Revision Number trên 2 SWCore:

![{B8135866-D04B-49C3-83C3-EBF099203F5D}](https://github.com/user-attachments/assets/2f288e2e-295e-4b2b-a9b3-5f52a538d8da)

SWCore2 có Revision Number lớn hơn của SWCore1 nên hệ thống hiện tại sử dụng VLAN Database của SWCore2.

**Tóm lại, sau LAB này, chúng ta phải hết sức cẩn thận khi thêm 1 switch vào 1 hệ thống mạng đang hoạt động.**
