- **Open Shortest Path First**
	- Dijkstrův algoritmus - *Shortest Path First (SPF)*
	- #OSPFv2 -> IPv4
	- #OSPFv3 -> [[IPv6]]
		- **Support for Address Families in OSPFv3** -> podpora i pro IPv4
- Automatický [[Routing]] -> tvorba [[Routovací tabulka]]
- Použití v rozlehlejších síťích s více routery a koncovými sítěmi
- Adaptivní -> *reakce na změny topologie sítě* (výpadky)
- Nástupce #RIP
	- nebere v potaz rychlost linky
	- pouze počet routerů
	- automatické přeposílání routovací tabulky dalším routerům
	- pomalé
- #Síťová vrstva -> *packety*
- #classless - nerespektuje třídy adres (velikost masky -> /0 - /32)
	- podpora #VLSM (*Variable Length Subnet Mast*) a #CIDR (*Classless Inter Domain Routing*)
- #Link_state dynamický routovací protokol
	- duplex
	- speed

- #Autonomní_systém 
	- část #WAN sítě se stejnou routovací politikou
	- většinou spravuje jeden subjekt -> ISP

- #Interior_gateway - routing uvnitř oblasti
- #Exterior_gateway - routing mezi oblastmi

```bash
show ip ospf neighbor
show ip protocols
show ip ospf
show ip ospf interface
show ip route
show ip ospf interface brief
```
### Konvergence
- #Konvergovaná_síť - všechny routery vědí všechno o všech ostatních routerech
	- #Rychlost_konvergence - doba od uplynutí zapnutí prvků do dosažení konvergentní sítě 
	- #OSPF *nejrychlejší* -> *nejpoužívanější*

1. Posílá #Hello_packet - *Establish Neighbour Adjectencies* 
	-  kontrola sousedních routerů
	-  info o sousedech
		- metriky, directly connected linky
	- všechny routery mají info o všech ostatních routerech
2. Výměna #Link_state informací - *Exchange Link-State*
	- #Link_state *Advertisement*
3. Vytvoření mapy sítě - *Build the Topology Table*
	-  ukládá do #LSDB
	- všechny informace o routerech a linkách
4. Dijkstrův algoritmus pro hledání nejlepší cesty
	- Na základě #LSDB
5. Vytvoří  [[Routovací tabulka]]
	- ponechává statické cesty

- #area - #Autonomní_systém routerů navzájem sdílejících #Link_state informace
	- #Backbone_area - area 0

- ## Multi Area [[OSPF]] 
	- #area rozdělená na více celků se společnou oblastí pro [[Routing]] mezi nimi
	- větší #Rychlost_konvergence
	- menší routovací tabulky
	- menší #Link_state provoz
	- méně časté přepočty
	- routery ve více oblastech
		- hraniční routery #ABR #Area_Bordered_Routers propojené #Backbone_area
		-  více #LSDB (pro každou #area)

## Struktura dat
- #Neighbor_table / #Adjectency_database - informace o sousedech
	- informace prostředím #Hello_packet
	- všechny routery mají identickou databázi
	- `do show ip ospf neighbor`
		- interface pro odesílání
		- propagované sítě
- #Link_state database / #LSDB - mapa sítě (topologie)
	- informace o všech routerech v oblasti
	- všechny routery mají identickou databázi
	- info prostřednictvím #Link_state *update* 
	- `do sh ip ospf database`
		- Link ID - IP adresa cílové sítě
		- ADV router - odesílající router
- #Forwardning_database - podklady pro vytvoření routovací tabulky
	- každý router má jinou databázi
	- `do show ip route`
	- výpočet nejoptimálnější cesty (Dijkstrův algoritmus)
		- cena cesty - #Metrika

## Routing protocol Messages
- #Hello_packet 
	- #Multicast (`224.0.0.5` `FF02::5`)
	- komunikace mezi sousedy každých 10 vteřin -> představení a kontrola stavu linky
		- #Router_ID - IPv4 adresa (**I pro #OSPFv3 !!**)
		- #Area_ID
		- #Priorita
		- #Hello_interval - výchozí 10 vteřin (musí být schodné na obou stranách linky)
		- #Dead_interval - po uplynutí prohlašuje linku za neaktivní - 4x #Hello_interval
	- #Designated_router **DR** / #Backup_designated_router **BDR** - vyřizuje veškerou [[OSPF]] #Multicast komunikaci
		- výběr dle nejvyšší #Router_ID a #Priorita (default 1 -> manuální nastavení)
	- #Active_neighbour
		- potvrzení příchozího #Hello_packet -> IP adresa druhého routeru
- #Database_description packet - posílá informace o sobě a všech sousedech - **DBD**
	- zjednodušená #LSDB
- #Link_state_request packet - **LSR**
	- upřesnění informací z #Database_description
	- potvrzení updatu
- #Link_state_update packet - **LSU**
	- #Link_state_advertisement - **LSA**
	- oznámení změn v síti a odpověď na **LSR**
- #Link_state_acknowledgment packet - **LSAck**

## [[OSPF]] States
- `do show ip ospf neighbour`
- #Down 
	- nedostal #Hello_packet 
	- posílá #Hello_packet
	- přechází do #Init
- #Init 
	- přijal #Hello_packet od souseda (obsahující #Router_ID odesílajícího routeru)
	- přechází do #Two-way
- #Two-way 
	- obousměrná komunikace mezi sousedy
	- v #MultiAccess síti probíhá volba #Designated_router a #Backup_designated_router
	- přechází do #ExStart
- #ExStart 
	- v #point-to-point síti dva routery rozhodují, kdo odesílá #Database_description packet jako první
		- prvotní číslo packetu
- #Exchange 
	- routery si vyměňují #Database_description packety
	- v případě nutnosti více informací přechází do #Loading jinak přechod do #Full
- #Loading
	- získání nutných informací pomocí #Link_state_request a #Link_state_update
	- výpočet cest pomocí **SPF** algoritmu
	- přechod do #Full
- #Full
	- Synchronizované #LSDB
# Multiaccess sítě
- #MultiAccess
- příliš mnoho sousedů každého s každým -> #Link_state_advertisement flooding
-> vysoký provoz
- volba #Designated_router a #Backup_designated_router - sběrný a distribuční bod
	- ostatní routery #DROTHER

# Point-to-Point sítě
- nastavení pří přímém propojení routerů pomocí ethernetu
- zákaz volby #DR a #BDR -> rychlejší konvergence, menší vytížení routerů
- `int [interface]` 
- `ip ospf network point-to-point`
- `do show ip ospf interface [interface]`
# Konfigurace
`do show ip protocols`
- konfigurační mód `router ospf [process-id]` / `ipv6 router ospf [process-id]`
	- *process-id* - identifikátor OSPF procesu (1 - 65535)
		- více procesů
	1. #Router_ID formát IPv4 adresy `router-id [RouterID]`
		- bez konfigurace si system tvoří sám dle nejvyšší IP adresy rozhraní (loopback, fyzické)
		- jednoznačná identifikace routeru
		- volba #Designated_port a #Backup_designated_router dle nejvyšší #Router_ID 
		- **Změna vyžaduje restart OSPF procesů na všech routerech** - znovunačtení nového #Router_ID
			- `clear ip ospf process`
	2. Propagované sítě 
		- `network [adresa sítě] [inverzní maska] area [area]` 
		- `network [adresa rozhraní] 0.0.0.0 area [area]`
		- `int [interface] \n ip ospf [process-id] area [area]`
	3. Passive interface
		- koncové sítě
		- neodesílá [[OSPF]] zprávy
		- `passive-interface [interface]`
	4. Priorita (0 - 255)
		- výchozí `1`
		- `0` nelze zvolit jako #Designated_router  ani #Backup_designated_router 
		- `ip ospf priority [priority]`
	5. #Metrika - [[OSPF]] #Cost 
		- #Cost = #reference_bandwidth / #interface_bandwidth
			- možní nastavení #reference_bandwidth 
				- default `100,000,000`
				- `auto-cost reference bandwidth [bandwidth]`
			- zaokrouhlená na celé číslo
			-> 10Gbe, Gigabit a 100Mbit stejná #Cost
			- loopback má výchozí #Cost `1`
		- Celková hodnota součtem #Cost na celé cestě
		- ruční nastavení 
			- `int [interface]`
			- `ip ospf cost [cost]`
	6. #Hello_interval a #Dead_interval 
		- výchozí `10`sekund
		- #Dead_interval automaticky vypočítán (4x #Hello_interval)
		- manuální nastavení
			- `int [intarface]`
			- `ip ospf hello-interval [interval]`
			- `ip ospf dead-interval [interval]`

## Propagace default static route v #OSPFv2 
```bash
interface [default interface] # interface do nadřazené sítě
ip address 64.100.0.1 255.255.255.252
exit

ip route 0.0.0.0 0.0.0.0 64.100.0.2 # default route

router ospf 10
default-information originate
```
- automaticky se poupravuje během propagace
- `0*E2 0.0.0.0/0 [110/1] via 10.1.1.6`
	- `O*E2` - default route zděděná z [[OSPF]]

# Možné problémy
- sousedící routery nejsou ve stejné IP síti
- sousedící routery mají různě nastaven #Hello_interval 
- sousedící routery mají různě nastaven typ sítě