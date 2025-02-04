- half duplex
- kolize -> #Kolizní_Domény
	- #CSMA/CA
		- vysílající uzel za ticha zasílá #RTS (*Request To Send*) rámec určený pro #AccessPoint
			- žádá o právo vysílat
			- informuje o délce vysílání
			- zachytávají i ostatní uzly
			- přijímá #CTS (*Clear To Send*) - "rezervace" času na vysílání
		- #AccessPoint odpovídá #CTS (*Clear To send*) rámcem
			- povoluje vysílání
			- informuje o délce vysílání
			- zachytávají i ostatní uzly
		- ostatní uzly čekají na konec vysílání
- rušení - interference
- elektromagnetické vlny 
	- anténa 
		- #směrová_anténa
		- #všesměrová_anténa
		- ~~polosměrová~~
		- #MIMO - *Multiple Inputs Multiple Outputs*
			- konfigurace antén předchozích typů
			- více přijímacích/nebo odesílajících kanálů - stále *half-duplex*
	- vysílač + přijímač
- #AccessPoint / #WiFi router -> [[Routing]] mezi drátovou a bezdrátovou sítí
	- #Autonomous
		- každý nakonfigurován jednotlivě
		- pracuje nezávisle na ostatních
	- #Controller-based
		- centralizovaná správa
- #WiFi Síťová karta

## Výhody a nevýhody
### Výhody
- mobilita
- flexibilita
- cena
### Nevýhody
- Nižší rychlost
- Nižší bezpečnost ( [[LAN Security]] )
- Rušení

- # Wireless Personal Area Network 
	- IEEE 802.15
	- #WPAN 
	- Low power
	- Short-range ( 10 m)
	- #Bluetooth, #ZigBee
		- #BLE - *Bluetooth Low Energy*
			- snímače, čidla
			- nízká spotřeba
		- #BR/ERD - *Bluetooth Basic Rate / Enhanced Rate*
			- #point-to-point topologie
			- optimalizovaný pro audio
	- IEEE 802.15
	- 2.4GHz

- # Wireless Local Area Network 
	- #WLAN
	- medium-size (100 m)
	- IEEE 802.11
	- 2.4GHz
		- 14 kanálů po 22MHz (2.412GHz - 2.484GHz)
		- pro více #AccessPoint jsou vhodné kanály 1, 6, 11
			- nejmenší překrývání
	- 5GHz
		- 24 kanálů po 20MHz
		- pro více #AccessPoint  jsou vhodné kanály 36, 48, 60
			- nejmenší překrývání
	- #WiFi

| Standard | Frekvence     | Popis             |
| -------- | ------------- | ----------------- |
| 802.11   | 2.4GHz        | 2Mb/s             |
| 802.11a  | 5GHz          | 54Mb/s            |
| 802.11b  | 2.4GHz        | 11MB/s            |
| 802.11g  | 2.4GHz        | 54Mb/s            |
| 802.11n  | 2.4GHz & 5GHz | 150 - 600Mb/s     |
| 802.11ac | 5GHz          | 450Mb/s - 1.3Gb/s |
| 802.11ax | 2.4GHz & 5GHz | HEW, 1GHz + 7GHz  |
- ### 802.11 Topologie (módy)
	- #Infrastructure
		- centrální #AccessPoint
		- bridge mezi bezdrátovou a drátovou sítí ( [[Routing]] )
		- "*převod ethernet rámců na elektromagnetické vlny*"
	- #IBSS / #AdHoc 
		- *peer-to-peer* - #point-to-point
		- bez centrálního #AccessPoint
		- #tethering -> osobní hotspot z mobilního zařízení, sdílení připojení
	- #Mesh
		- propojení více #AccessPoint
		- pokrytí velké plochy
		- více cest -> *redundance*

![[WiFiFrame.png]]
- Address 1 - #RA -> Receiving **radio** MAC
- Address 2 - #TA -> Transmitting **radio** MAC
- Address 3 - #DA -> Final destination MAC
- Address 4 - #SA ->Original sender MAC 

- ### Asociace klienta s AP
	- Objevit #AccessPoint -> #Authentication -> #Asociace s #AccessPoint 
	- Požaduje:
		- #SSID -> *jméno* sítě
		- #Password -> heslo pro přístup
		- #Network_mode -> podporovaný 802.11 standard
		- #Security_mode -> bezpečnostní nastavení ( #WEP #WPA #WPA2 #WPA3 )
		- #Channel_settings -> komunikační kanály
	- #### Způsoby objevení #AccessPoint 
		- #Passive 
			- klient přijímá #Beacon_frame -> #AccessPoint je pravidelně vysílá
				- broadcast
				- informace o #SSID , standardech a protokolech
		- #Active
			- klient musí znát #SSID hledaného #AccessPoint
			- klient zasílá #Probe_request_frame na více kanálů, správný #AccessPoint odpovídá

- ### CAPWAP
	- #CAPWAP - *Control and Provisioning of Wireless Access Points*
	- Umožňuje #WLC (*Wireless LAN Controller*) bezpečně spravovat mnoho #AccessPoint a #WLAN
	- Vychází z #LWAPP
		- #DTLS - *Datagram Transport Layer Security* -> zabezpečení komunikace
			- přidává bezpečnostní hlavičku k odesílaným datům -> zabezpečený tunel
				- port #UDP 5246 - komunikace mezi #WLC a #AccessPoint
				- port 5247 - komunikace s klienty
	- rozděluje funkcionalitu každého #AccessPoint na MAC vrstvě
		- #AP_MAC_funkce 
			- správa #Beacon_frame a #Probe_request_frame
			- potvrzování, přeposílání a prioritizace packetů
			- řazení rámců a šifrování a dešifrování na MAC vrstvě
		- #WLC_MAC_funkce
			- #Authentication #Asociace a #Deasociace klientů pohybujících se mezi různými #AccessPoint
			- překlad rámců mezi protokoly
			- ukončování 802.11 komunikace

- ### Typy #Authentication
	- #Open_system
	- #Shared_key
	- #WPA

- ### Modulační techniky
	- #AM - Amplitude modulation
	- #FM - Frequency modulation
	- #PM - Phase modulation

- ### #FlexConnect
	- bezpečná konfigurace a správa #AccessPoint přes #WAN síť

- #  Wireless Metropolitan Area Network
	- #WMAN
	- velká rozloha - města
	- licencované frekvence

- # Wireless Wide Area Network 
	- #WWAN
	- národní & globální
	- licencované frekvence

- # Worldwide Interoperability for Microwave Access
	- #WiMAX
	- **Nevyužívá se - nejsou volné frekvence**
	- IEEE 802.16
	- dosah až 50 km
	- #point-to-multipoint
	- přístupová síť providerů
	- podobný #WiFi pro více uživatelů na větší vzdálenost

- # Cellular Broadband - Mobilní síť
	- data i hlas
	- nutná SIM karta
	- telefony, tablety atd.
	- #směrová_anténa s rozptylem 120˚
		- výkon 100W na #všesměrová_anténa
		- výkon 500W na #směrová_anténa
	- generace/technologie
		- #0G
		- #2G
		- #3G
		- #4G (10x rychlejší než #3G)
		- #5G (100x rychlejší než #4G)
		- #6G (100x rychlejší než #5G)
	- Typy:
		- Global System of Mobile ( #GSM )
		- Code Division Multiple Access ( #CDMA )

![[Cellular.png]]

- # Satellite
	- #směrová_anténa na geostacionární družici
	- odlehlé lokality - bez jiné možnosti připojení
	- **Starlink**