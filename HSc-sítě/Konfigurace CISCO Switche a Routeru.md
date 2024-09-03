```
do <command> -> příkazy pro exec v privileged režimu

-------------------

do sh hi -> historie
do sh ip int br | in Vlan -> list nastavení adres VLAN

-------------------

en
conf t
line co 0
logging synchronous -> lepší výpis (informace se nemýchají s příkazy)

--------------------

no ip domain-name lookup -> vypíná pomalé vyhledávání domén

--------------------

en
conf t
interface <interface>
description <descr>

--------------------

en
show interfaces          -> info o rozhraních
show ip interface brief  -> tabulka informací o rozhraních 
                         (STATUS - povolen?, PROTOCOL - spojení?)
show interface (status/summary)

--------------------    -> vypnutí DTP

int range f0/4-24, g0/1-2
sw mo acc
sw nonegotiate

int range f0/1-3
sw nonegotiate

--------------------
```

#hostname
```
enable <password>
conf term
hostname <hostname>
```

#banner
```
enable <password>
conf term
banner motd #<banner>#
```

#password
- **Privileged exec mode**
```
enable <password-default>
conf term
enable (secret/password) <password-new>
```

- **Console/VTY**
```
enable <password-enable>
conf term

line con 0
password <password-console>
login

line vty 0 15
password <password-vty>
login
```

- **Users**
```
enable <password-enable>
conf term

username <name> secret <pasword-user>

line <line>
login local
```

#speed_duplex
```
enable <password>
conf t
interface <interface>

speed (auto/10/100/1000) -> auto (Auto-MDIX nastavuje rychlost a duplex)
duplex (auto/full/half)
```

#IP_address
```
en <password>
conf t

interface <port/vlan/loopback> <number>
ip address <IP> <mask>
ipv6 address <IPv6 + suffix>

no shutdown
exit

ip default-gateway <IP>
```

#SSH
```
en
conf t
interface vlan1
ip address <IP> <mask>
exit

line vty 0 15
transport input ssh
login local

exit

ip domain-name <domain.name>
crypto key generate rsa
	> 1024
ip ssh version 2
```

