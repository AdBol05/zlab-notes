- **Open Shortest Path First**
	- Dijkstrův algoritmus - *Shortest Path First (SPF)*
	- #OSPFv2 -> IPv4
	- #OSPFv3 -> [[IPv6]]
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
	- většinou spravuje jeden subjekt -> 

- #Interior_gateway - routing uvnitř oblasti
- #Exterior_gateway - routing mezi oblastmi

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

- # Multi Area [[OSPF]] 
	- #area rozdělená na více celků se společnou oblastí pro [[Routing]] mezi nimi
	- větší #Rychlost_konvergence
	- menší routovací tabulky
	- menší #Link_state provoz
	- méně časté přepočty
	- routery ve více oblastech
		- hraniční routery #ABR #Area_Bordered_Routers propojené #Backbone_area
		-  více #LSDB (pro každou #area)

# Struktura dat
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

# Routing protocol Messages
- #Hello_packet 
	- #Multicast (`224.0.0.5` `FF02::5`)
	- komunikace mezi sousedy každých 10 vteřin -> představení a kontrola stavu linky
		- #Router_ID
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

# [[OSPF]] States
