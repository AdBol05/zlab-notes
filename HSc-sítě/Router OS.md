- default login: `admin` (bez hesla)
- GNU/Linux-based
- Konfigurace 
	- #WebFig - Webové rozhraní
	- #Winbox - GUI klient
	- #SSH / #Telnet / #Console

![[Router OS 2024-09-30 12.45.14.excalidraw]]
# Konfigurace
### IP adresa
- IP -> Addresses -> +
	- Address: `10.14.0.1/30` - *musí mít délku prefixu*
	- Network: `10.0.14.0`
	- Interface: `ether2`
### Smazání konfigurace
- System -> Reset Configuration
	- Do Not Backup - *nevytvářet zálohy na flash*
	- No Default Configuration - *žádná (prázdná) konfigurace*