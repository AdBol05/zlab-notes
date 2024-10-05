- 128bitů
-  hexadecimální soustava - *xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx*
- 8 hextetů - 16bitů / 4 hex hodnoty
- *fe80::972b:99ac:7e55:71ea*/64

#### Koexistence IPv4 a IPv6
- #Dual_stack - IPv4 a IPv6 současně
- #Tunneling - IPv6 packet zapouzdřen do IPv4 packetu
- #Translation - NAT64, překlad podobný NAT
- *Povolení IPv6 na Switchi* -> `sdm prefer dual-ipv4-and-ipv6 default`
- *Povolení IPv6 unicast routování* -> `ipv6 unicast-routing`
#### Pravidla formátování
- Odebrání nul na začátku
	- 00b4 -> b4
- Dvojitá dvojtečka - nahrazení nul 
	-  fe80:0000:0000:0000:972b:99ac:7e55:71ea -> fe80::972b:99ac:7e55:71ea

### Unicast, Multicast, Anycast
- #Unicast - unikátní IPv6 adresa
	- #GUA - *Global Unicast Adres*
		- první tři bity *2000::/3*
		- #Global_routing_prefix (48b) + #Subnet_ID (16b) + #Interface_ID (64b)
			- #Interface_ID 
				- **EUI-64** - využití MAC adresy
				- **Random generated** - náhodně generuje operační systém
		- celosvětově unikátní
		- globálně routovatelné
	- #LLA - *Link-Local Address*
		- musí mít každé zařízení s povolenou IPv6
		- komunikace uvnitř jedné LAN
		- jeden spoj, nejsou globálně routovatelné
		- v LAN je default gateway
	- #Unique_local - *fc00::/7* až *fdff::/7*
		- [[Routing]] mezi [[VLAN]]
		- nejsou globálně routovatelné
- #Multicast - různé destinace (skupiny)
- #Anycast - adresa na více zařízeních, packet směrován k nejbližšímu
- **Nemá broadcast** -> multicast všech uzlů

### Prefix
- nahrazuje masku
- síťová + hostitelská část (prefix + #Interface_ID)


# Konfigurace
### Switch
```sh
sdm prefer dual-ipv4-and-ipv6 default
do copy run start
reload

int vlan 90
ipv6 add fe80::2 link-local
ipv6 2001:acad:abc:90::2/64

ipv6 route ::/0 2001:db8:3::1  # default gateway
```

### Router
```sh
ipv6 unicast-routing

int g0/0.10
ipv6 add fe80::1 link-local
ipv6 add 2001:acad:abc:10::1/64

int g0/0.20
ipv6 add fe80::1 link-local
ipv6 add 2001:acad:abc:20::1/64

int g0/0.90
ipv6 add fe80::1 link-local
ipv6 add 2001:acad:abc:90::1/64
```