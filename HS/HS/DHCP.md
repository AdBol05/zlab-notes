- #DHCP **Dynamic Host Configuration Protocol**
- #Aplikační protokol (server - klient)
- automatické poskytnutí základních parametrů pro plnohodnotné fungování sítě
	- IP adresa uzlu
	- maska sítě
	- IP adresa výchozí brány (default gateway)
	- IP adresa DNS serveru
- "pronájem" adresy na určitou dobu
	- je nutný *Lease Renewal* po vypršení adresy ( #DHCPREQUEST + #DHCPACK )
- při selhání DHCP (server neodpoví) si uzel nastaví #APIPA adresu 169.254.x.x

![[DHCP.png]]
1. #DHCPDISCOVER - hledá #DHCP server, *broadcast*
2. #DHCPOFFER - nabídka IP adresy od serveru, *broadcast/unicast* na MAC adresu
3. #DHCPREQUEST - příjem IP adresy, *broadcast*
4. #DHCPACK - potvrzení, *broadcast/unicast*

## Konfigurace DHCP serveru na Cisco L3 switchi
- výčet #excluded adres
- #address_pool - pojmenování rozsahu adres
- síťový rozsah s maskou
- adresa default gateway
- adresa #DNS
- (jméno domény)

```
service dhcp
no service dhcp
```

```
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp excluded-address 192.168.10.254
ip dhcp pool LAN-POOL-1

network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 192.168.11.5
domain-name example.com
end
```

```
show running config | section dhcp
show ip dhcp binding
show ip dhcp server statistics
```

## DHCPv4 Relay
- server se nachází v jiné síti (linkovém segmentu nebo [[VLAN]])
- možnost více rozsahů pro více [[VLAN]]
```
int g0/0/0
ip helper-address 192.168.11.6 # pomocná adresa z druhé sítě
end
```

## DHCP klient na Cisco prvcích

```
interface g0/1
ip address dhcp
no shutdown
```


# DHCPv6
- #DHCPv6 - [[IPv6]]
- uzel potřebuje
	- #GUA a adresu DNS serveru
		- ručně
		- #SLAAC
		- #Stateless_DHCPv6 - jen *DNS* ( #GUA od #SLAAC, zbytek od #DHCPv6)
		- #Stateful_DHCPv6 - vše krom *default gateway* od #DHCPv6 serveru

		 - (M flag = 0, O flag = 0) -> #SLAAC only
		 - (M flag = 0, O flag = 1) -> #DHCPv6 + #SLAAC 
		 - (M flag = 1) -> #Stateful_DHCPv6 

		- #DAD - kontrola duplicity
	- default gateway - získá od hraničního routeru (router se sám oznamuje)

#SLAAC 
 - **Stateless Address Autoconfiguration**
 - posílá prefix a délku prefixu klientovi
 - nemusí být k dispozici server
 - #Router_solicititaion 
	 - multicast na *FF02:2* - multicast všech routerů v síti
 - #Router_advertisement 
	 - router periodicky rozesílá defaultně každých 200s
	 - jako odpověď na #Router_solicititaion
	 - na IPv6 multicast všech *FF02::1* - multicast všech uzlů v síti ->"broadcast"
	 - prefix + délka prefixu

## Konfigurace DHCPv6
- ### Klient - Switch
```sh
int vlan 90
ipv6 add dhcp # Stateful
ipv6 add autoconfig # Staless
```

- ### Server - Router
```sh
# Stateless
ipv6 unicast-routing
ipv6 dhcp pool IPV6-STATELESS
domain-name example.com
exit

interface g0/0.10
description Link to LAN
ipv6 address fe80::1 link-local
ipv6 address 2001:acad:abc:10::1/64
ipv6 nd other-config-flag
ipv6 dhcp server IPV6-STATELESS
no shut
exit

interface g0/0.90
description Link to LAN
ipv6 address fe80::1 link-local
ipv6 address 2001:acad:abc:90::1/64
ipv6 nd other-config-flag
ipv6 dhcp server IPV6-STATELESS
no shut
exit

# Stateful
ipv6 dhcp pool IPV6-STATEFUL
address prefix 2001:acad:abc:1::/64
domain-name example.com

interface g0/0.20
description Link to LAN
ipv6 address fe80::1 link-local
ipv6 address 2001:acad:abc:20::1/64
ipv6 nd managed-config-flag
ipv6 nd prefix default no-advertise
ipv6 dhcp server IPV6-STATEFUL
no shut
```