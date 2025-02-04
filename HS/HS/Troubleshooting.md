
![[Screenshot_20231207-104549.png]]

```
do del vlan.dat
do erase startup-config
```

```
interface Serial0/0/0
ip address 172.31.1.197 255.255.255.252
ipv6 address 2001:DB8:4::1/64
clock rate 2000000
```