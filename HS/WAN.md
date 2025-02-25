- #WAN propojení #LAN sítí

# Soukromé a veřejné WANs
- #Privátní #WAN 
	- Garantovaná kvalita přenosu dat
	- Vyhrazená, nesdílená šířka pásma
	- Bezpečnost - k dedikované lince nemá přístup nikdo jiný
- #Veřejná #WAN 
	- standardně provozována #ISP nebo telekomunikačním operátorem
	- Kvalita přenosu se může měnit
	- Není garantovaná bezpečnost


- ## Topologie
	- #point-to-point -  privátní linky
	- #hub-and-spoke - *point to multipoint*
		- centrální hub router
		- spokes připojené na hub jedním interfacem na hubu (sdílí jedno rozhraní hubu)
			- komunikace mezi spoky pouze prostřednictvím hubu
	- #dual-homed - redundance pomocí dvou hub routerů
		- přístupové sítě
		- velké množství koncových routerů a koncových sítí
	- #fully-meshed - všechny routery propojeny s každým
		- největší odolnost
	- #partially-meshed - skoro všechny routery propojené s každým

- ### Standardizační organizace
	- #TIA/EIA - Telecommunications Industry Association
	- #ISO
	- #IEEE

# Terminologie
- #DTE - *Data Terminal Equipment*


- #Local_Loop - *Lokální smyčka* 


## Sériová komunikace
- Téměř veškerá síťová komunikace
- Přenáší bity postupně po jednom komunikačním kanále
## Paralelní komunikace
- Současný přenos několika bitů pomocí více kanálů
-> s větší vzdáleností se narušuje synchronizace kanálů

## Circuit Switched Networks
- #Circuit_switched
- v síti ISP se vytvořil vyhrazený okruh mezi odesílatelem a příjemcem před zahájením komunikace
- #PSTN
## Packet Switched Networks
- #Packet_switched
- Přenášená data rozdělena do packetů směrované k cíli
- současné využití
- #MPLS 
- Etherner #WAN 

## DWDM
- #DWDM - Dense Wawelength Division Multiplexing
	- více datových toků na různých vlnových délkách
		- Standardní pásma *C* (1530-1565 nm) a *L* (1565-1625 nm)
		- rozšířené pásmy *S - SWIR - short band* (1400-3000 nm) a *E - LWIR - extended band* (8000-14000 nm)
- Optická vlákna -> velké vzdálenosti, rychlost, spolehlivost, malé rušení
- Úplný odraz -> paprsek se odráží uvnitř jádra vlákna

# Připojení k WAN
- ## Tradiční druhy připojení
	- Dedikované linky - Leased line, #broadband
	- Switched - #Circuit_switched #Packet_switched 
- ## Moderní WAN technologie
	- Dedikované linky - #broadband - vyhrazené optické vlákno
	- #Packet_switched - #MPLS, Ethernet [[WAN]], #EoMPLS
	- Internet based - #broadband VPN 
		- *Wired* - #xDSL, #Cable, optické vlákno
		- [[Wireless]] - #WiMAX, #Cellular, #Satellite, #WiFi 