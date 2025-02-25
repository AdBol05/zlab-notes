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

### Upgrade Router OS
- **Nutná kontrola architektury**
	- mipsbe, ppc, x86, mipsle, tile
- oprava problému, nové funkcionality, vylepšení výkonu
	- informace v changelogu
- stažení balíčku a kopírování do routeru
	- pohled *Files*
	- po restartu se načte nová verze
	- při downgradu je nutné *System* -> *Packages* -> *Downgrade*
- kontrola aktualizací
	- *System* -> *Packages*
- Auto Upgrade
	- *System* -> *Auto Upgrade*
- **Druhy souborů pro upgrade**
	- #ZIP - archiv balíčků, základní OS
	- #NPK - jednotlivé balíčky
- **Upgrade bootloaderu**
	- info **System** -> **RouterBOARD** -> *Upgrade*

## Uživatelské účty
- *System* -> *Users*
	- záložky Users a Groups

## Správa IP služeb
- *IP* -> *Services*
- omezení využívání prostředků, bezpečnostních rizik, přístup na IP adresy
- porty služeb
	- 80 (web)
	- 443 (web-ssl)
	- 8291 (winbox) - IP připojení

## Zálohování
- *Files* -> *Backup*/*Restore*
- Binární záloha
	- kompletní záloha sytému (obsahuje i hesla)
	- kompatibilní jen na zařízení stejného typu
- Export konfigurace - *Terminal* -> `export file=filename`
	- úplná nebo částečná záloha (na základě momentálního kontextu konfigurace)
	- soubor se skriptem nebo export v konzoli

# Wi-Fi
- *Wireless* -> *Wireless*
	- WiFi Interfaces
		- interface -> wlan1
			- mode : přístupový bod (ap bridge) / stanice (station)
			- band: 2GHz-only-N
			- frequency: kanál
			- SSID: název sítě
			- security profil: profil ověřování
			- WPS mode: disabled
			- Frequency mode: regulatory-domain
			- Country: CZ - nastavení správných parametrů dle zákona
			- installation: indoor/outdoor
	- Security Profiles - ověřování
		- mode: dynamic keys
		- types: WPA2 PSK
		- WPA2 Pre-Shared Key: heslo

### Zabezpečení
- Open -> OWE
- WEP
- WPA
- **WPA2**
- **WPA3**
	- EAP (enterprise) - RADUIS, AD
	- PSK (personal)