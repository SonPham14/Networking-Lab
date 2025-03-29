## Lab 9. VPN Site to Site

### Topology

![image](https://github.com/user-attachments/assets/3e714c49-c519-4450-8bd6-25e14011e59b)

### Configure the IP addressing as illustrated in the topology

```
VPN_GW_NVL(config)#interface Loopback0
VPN_GW_NVL(config-if)#ip address 10.1.1.1 255.255.255.0
VPN_GW_NVL(config)#interface FastEthernet0/0
VPN_GW_NVL(config-if)#ip address 14.0.0.1 255.255.255.0
VPN_GW_NVL(config-if)#no shutdown
VPN_GW_NVL(config)#ip route 0.0.0.0 0.0.0.0 14.0.0.4

VPN_GW_HMT(config)#interface Loopback0
VPN_GW_HMT(config-if)#ip address 10.2.2.2 255.255.255.0
VPN_GW_HMT(config)#interface FastEthernet0/0
VPN_GW_HMT(config-if)#ip address 24.0.0.2 255.255.255.0
VPN_GW_HMT(config-if)#no shutdown
VPN_GW_HMT(config)#ip route 0.0.0.0 0.0.0.0 24.0.0.4

Internet(config)#interface e0/0
Internet(config-if)#ip address 14.0.0.4 255.255.255.0
Internet(config-if)#no shut
Internet(config)#interface e0/1
Internet(config-if)#ip address 24.0.0.4 255.255.255.0
Internet(config-if)#no shut
```

### Configure NAT translation to translate the LAN networks

```
VPN_GW_NVL(config)#ip access-list extended NAT-ACL
VPN_GW_NVL(config-ext-nacl)#deny ip 10.1.1.0 0.0.0.255 10.2.2.0 0.0.0.255
VPN_GW_NVL(config-ext-nacl)#permit ip 10.1.1.0 0.0.0.255 any
VPN_GW_NVL(config)#ip nat inside source list NAT-ACL interface e0/0 overload

VPN_GW_HMT(config)#ip access-list extended NAT-ACL
VPN_GW_HMT(config-ext-nacl)#deny ip 10.2.2.0 0.0.0.255 10.1.1.0 0.0.0.255
VPN_GW_HMT(config-ext-nacl)#permit ip 10.2.2.0 0.0.0.255 any
VPN_GW_HMT(config)#ip nat inside source list NAT-ACL interface e0/0 overload
```

### Enable the NAT on Lo0 (inside) and fa0/0 (outside) interfaces

```
VPN_GW_NVL(config)#interface loopback 0
VPN_GW_NVL(config-if)#ip nat inside
VPN_GW_NVL(config)#interface e0/0
VPN_GW_NVL(config-if)#ip nat outside

VPN_GW_HMT(config)#interface loopback 0
VPN_GW_HMT(config-if)#ip nat inside
VPN_GW_HMT(config)#interface e0/0
VPN_GW_HMT(config-if)#ip nat outside
```

### Configure Interesting Traffic

```
VPN_GW_NVL(config)#ip access-list extended VPN-TO-VPN_GW_HMT
VPN_GW_NVL(config-ext-nacl)#permit ip 10.1.1.0 0.0.0.255 10.2.2.0 0.0.0.255

VPN_GW_HMT(config)#ip access-list extended VPN-TO-VPN_GW_NVL
VPN_GW_HMT(config-ext-nacl)#permit ip 10.2.2.0 0.0.0.255 10.1.1.0 0.0.0.255
```

### Configure Phase 1 ISAKMP

```
VPN_GW_NVL(config)# crypto isakmp policy 1
VPN_GW_NVL(config-isakmp)# encr aes
VPN_GW_NVL(config-isakmp)# hash sha
VPN_GW_NVL(config-isakmp)# authentication pre-share
VPN_GW_NVL(config-isakmp)# group 1

VPN_GW_HMT(config)# crypto isakmp policy 1
VPN_GW_HMT(config-isakmp)# encr aes
VPN_GW_HMT(config-isakmp)# hash sha
VPN_GW_HMT(config-isakmp)# authentication pre-share
VPN_GW_HMT(config-isakmp)# group 1
```

### Define the pre-shared key for authentication

```
VPN_GW_NVL(config)# crypto isakmp key cisco address 24.0.0.2

VPN_GW_HMT(config)# crypto isakmp key cisco address 14.0.0.1
```

### Configure Phase 2 IPsec

```
VPN_GW_NVL(config)#crypto ipsec transform-set TS-TO-VPN_GW_HMT esp-aes esp-sha-hmac

VPN_GW_HMT(config)#crypto ipsec transform-set TS-TO-VPN_GW_NVL esp-aes esp-sha-hmac
```

### Configure a crypto map

```
VPN_GW_NVL(config)#crypto map VPNMAP 1 ipsec-isakmp
VPN_GW_NVL(config-crypto-map)#set peer 24.0.0.2
VPN_GW_NVL(config-crypto-map)#set transform-set TS-TO-VPN_GW_HMT
VPN_GW_NVL(config-crypto-map)#match address VPN-TO-VPN_GW_HMT

VPN_GW_HMT(config)#crypto map VPNMAP 1 ipsec-isakmp
VPN_GW_HMT(config-crypto-map)#set peer 14.0.0.1
VPN_GW_HMT(config-crypto-map)#set transform-set TS-TO-VPN_GW_NVL
VPN_GW_HMT(config-crypto-map)#match address VPN-TO-VPN_GW_NVL
```

### Attach the crypto map above to interface

```
VPN_GW_NVL(config)#interface e0/0
VPN_GW_NVL(config-if)#crypto map VPNMAP

VPN_GW_HMT(config)#interface e0/0
VPN_GW_HMT(config-if)#crypto map VPNMAP
```

### To verify the VPN Tunnel between VPN_GW_NVL and VPN_GW_HMT

```
VPN_GW_NVL#show crypto session
VPN_GW_NVL#show crypto isakmp sa
VPN_GW_NVL#show crypto isakmp sa detail
VPN_GW_NVL#show crypto ipsec sa
VPN_GW_NVL#show crypto ipsec sa detail
VPN_GW_NVL#show crypto ipsec sa | s local | remote | pkts
```
