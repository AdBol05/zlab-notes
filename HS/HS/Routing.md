### Výběr směru do jiné sítě
- směr k dalšímu routeru
	- výpočet a zápis optimálního směru k cíli
	- výběr optimálního směru
	- Forwarding packetů
		- #ingress -> cílová adresa -> vhodný záznam v routovací tabulce -> #egress
			- logický součin masky zdrojové adresy a záznamu z routovací tabulky
				- daný záznam
				- 0.0.0.0/0
	- Udržování dat k tomu potřebných

![[Routing 2023-11-20 11.01.56.excalidraw]]

| typ záznamu | adresa      | výhodnost | typ sítě         | čas      | odchozí interface    |
| ----------- | ----------- | --------- | ---------------- | -------- | -------------------- |
| O           | 10.0.4.0/24 | [110/50]  | via 10.0.3.2     | 00:13:29 | Serial 0/1           |
| C           | 10.0.1.0/24 |           | directly to g0/0 |          | GIgabitEthernet0/0/0 |
| L           | 10.0.1.0    |           | directly g0/0    |          | GIgabitEthernet0/0/0 |
#### Priorita:
1. Délka prefixu
2. #Administrative_distance
3. #Metrika (cena cesty)

#### Typ záznamů
- O - #OSPF záznam -> *Remote*
- L - Local -> *Local route interface*
- C - Connected -> *Directly Connected Network*
- S - Static route
- * - Default gateway
- **Priotita:**
	1. L
	2. C
	3. S
	4. O
	5. Default gateway

![[Routing 2024-06-06 10.59.40.excalidraw]]

```
R1
L 172.16.0.1/32  255.255.255.252 directly g0/1
C 172.16.0.0/30  255.255.255.252 directly g0/1
L 192.168.1.1/32 255.255.255.252 directly g0/0
C 192.168.1.0/32 255.255.255.000 directly g0/0
S 192.168.2.0/32 255.255.255.000 via 172.16.0.2 [1/0] g0/0

R2
L 172.16.0.2/32  255.255.255.252 directly g0/1
C 172.16.0.0/30  255.255.255.252 directly g0/1
L 192.168.2.1/32 255.255.255.255 directly g0/0
C 192.168.2.0/24 255.255.255.000 directly g0/0
S 192.168.1.0/24 255.255.255.000 via 172.16.0.1 [1/0] g0/0
```
## Router on a stick
- routování mezi VLAN
- jedna #Trunk linka na router
- router musí podporovat VLAN
- lze rozlišit VLAN
- pro každou VLAN je konfigurován #subinterface na fyzickém rozhraní

## MLS - Fast-switching
[[L3 Switch]]
- Routing klasickou SW cestou 
- Route caching (pokud obdrží packet se stejnou destinací, odesílá na stejné rozhraní)
- Informace o relaci IP adres a odchozího portu se ukládá to paměti rozhraní

## MLS - Process-switching
[[L3 Switch]]
- packet-by-packet
- Kontrola každého packetu
- Pomalé 


#subinterface 
- **Logické** rozhraní pod fyzickým
- **implementace VLAN**
- je ve VLAN narozdíl od fyzického interfacu
- jeden na jednu VLAN
- může mít IP adresu -> default gateway
```sh
#Switch
interface vlan 99  # management VLAN
ip add 192.168.99.2 255.255.255.0
no shut
exit
ip default-gateway 192.168.99.1


#Router
interface G0/1
no shut
interface G0/1.10
encapsulation dot1Q 10
ip add 192.168.10.1 255.255.255.0
exit
...
interface G0/1.20
...
interface G0/1.99
```
![[Routing 2023-11-30 11.00.02.excalidraw]]

[[STP]] go brrrrr
![[Routing 2023-12-14 11.03.44.excalidraw]]

# Statický routing
```sh
ip route 192.168.3.0 255.255.255.0 172.16.0.3  # via 172.16.0.3
ip route 192.168.3.0 255.255.255.0 g0/0        # direct

ip route 192.168.4.0 255.255.255.0 172.16.0.4  
ip route 192.168.3.0 255.255.255.0 172.16.0.3  
ip route 192.168.2.0 255.255.255.0 172.16.0.2  
ip route 10.0.10.0 255.255.255.0 172.16.0.1  
ip route 10.0.20.0 255.255.255.0 172.16.0.1
```
- **Musí být nastavený routing i zpět na druhém routeru**

```sh
ip route 192.168.3.0 255.255.255.0 g0/0     # default administrative distance (6)
ip route 192.168.3.0 255.255.255.0 g0/0 10  # 10 administrative distance
```

```sh
ip route 0.0.0.0 0.0.0.0 g0/0 # default route -> S*
```

#floating_route - záložní statická cesta

# Dynamický routing
-> [[OSPF]]


# IPv6 Routing
[[IPv6]]
```sh
ipv6 unicast-routing

int g0/0.10
encapsulation dot1Q 10
ip add 10.0.10.1 255.255.255.0
ipv6 add fe80::1 link-local 
ipv6 add 2001:acad:abc:10::1/64

int g0/0.20
encapsulation dot1Q 20
ip add 10.0.20.1 255.255.255.0
ipv6 add fe80::1 link-local 
ipv6 add 2001:acad:abc:20::1/64

int g0/0.90
encapsulation dot1Q 90
ip add 10.0.90.1 255.255.255.0
ipv6 add fe80::1 link-local 
ipv6 add 2001:acad:abc:90::1/64
```