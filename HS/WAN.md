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