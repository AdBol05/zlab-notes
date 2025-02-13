<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSJM_B52nhqR-QrvDzp-2V0GCwmyG_0DWmsJA&usqp=CAU">
![[Pasted image 20241208170825.png]]
#Fyzická
- Koncová zařízení (PC, server)
- Zprostředkující zařízení (router, switch)
- Přenosové médium (metalika, optika, elektromagnetické záření)
- kódování/dekódování
- přenos na přenosové médium
- [[WAN]]
	- #DWDM 

#Linková (spojová)
- Propojení sousedních uzlů
- Rámce (64B - 1518B)
- [[Switching]]
- LAN
- Adresace, Encapsulation, Error checking
- MAC adresy (`90:E2:BA:1C:8B:57`)
	- 48 bitů
	- 3 Bity Organization Unique Identifier ==OUI==
	- 3 Bity Vendor assigned ==EUI==
	- Broadcast: `FF:FF:FF:FF:FF:FF`
	- Multicast: `33:33:.:.:.:.` (IPv6), `01:00:5E:.:.:.` (IPv4)
- Podvrstvy
	- MAC - Media Access Control
		- přístup na přenosové medium
	- LLC - Logical Link Control
		- #CSMA/CA - Collision Avoidance
		- #CSMA/CD - Collision Detection
- [[WAN]]
	- #Metro-ethernet & Ethernet #WAN 
		- sada IEEE standardů
		- většinou optika nebo twinaxiální kabely
	- širokopásmové standardy #DSL 
		- kroucená dvoulinka
	- "kabelovka" - #Cable
		- koax
	- #MPLS - *Multiprotocol Label Switching*
		- L2,5 - Tag mezi hlavičkou #Linková a #Síťová vrstvy
		- #EoMPLS - *Ethernet over MPLS*


#Síťová
- Propojuje LAN
- Pakety
- [[Routing]]
- IP adresy (172.16.1.54)
	- 32 bitů
	- síťová a hostitelská část (určuje prefix)
	
#Transportní
- může být spolehlivá
- porty (IP + port = socket)
- TCP
	- spolehlivá
	- segment
- UDP
	- nespolehlivá
	- datagram

#Relační

#Prezentační

#Aplikační






![[Drawing 2023-09-08 10.29.59.excalidraw]]