Zabezpečení #LAN
## Útoky
- #DDoS 
	- **Distributed Denial of Service**
	- zahlcení cíle requesty z infikovaných zařízení v botnetu - "zombies"
- #Data_Breach
	- únik citlivých dat
- #Malware
	- zařízení napadené škodlivým softwarem
		- #Ransomware - zašifrování dat a požadování platby za rozšifrování
		- #Spyware - sbírá citlivá data na pozadí a odesílá je
		- #Worm - kopíruje sám sebe a připojuje se k souborům -> zahlcení disku

# Bezpečnostní zařízení
- #VPN-Enabled_Router
	- Poskytuje #VPN server
- #NGFW - **Next-Generation Fire Wall**
	- kontrola packetů
	- #NGIPS - **Next-Generation Intrusion Prevention System**
	- #AMP - **Advanced Malware Protection**
	- URL filtering
- #NAC - **Network Access Control**
	- #AAA - **Authentiation, Authorization, Accounting**
	- #ISE - **Cisco Identity Services Engine** 
- Cisco #ESA - **Email Security Appliance**
	- monitoring #SMTP
	- real-time updaty z Talos databáze (update každých 3 - 5 minut)
	- blokuje známé hrozby, šifruje odchozí zprávy
- Cisco #WSA - **Web Security Appliance**
	- bandwidth limity
	- blokuje zakázané stránky
	- malware scan
	- šifruje a dešifruje webovou komunikaci
- #HIPSs - **Host-based Intrusion prevention systems**

# Access control
- #console
```
line vty 0 4
password cisco
login
```

- #ssh
```
ip domain-name zlab.cz
crypto key generate rsa general-keys modulus 2048
username Admin secret password
ip ssh version 2
line vty 0 4
transport input ssh
login local
```

#AAA 
- #Authentication #Authorization #Accounting
- Local #AAA 
	- přístupové údaje přímo na prvku
- Server-based #AAA
	- přístupové údaje na centrálním serveru
	- prvek se dotazuje serveru
		- #RADIUS - **Remote Authentication Dial-In User Service**
		- #TACACS - **Terminal Access Controller Access Control System**

### 802.1X
- zabezpečuje přístup k LAN portu
- server ověří každou připojenou stanici před udělením přístupu ke službám v síti
- role:
	- #Supplicant (klient) - zařízení s klientskou aplikací 802.1X
	- #Authenticator (switch)
	- #Authentication_server ( #RADIUS server ) 


# Bezpečnostní hrozby na L2 
- #Linková vrstva
- ### útoky na MAC tabulku
	- zahlcení tabulky -> komunikace na všechny porty
	- **Ochrana** #port_security
- ### útoky na [[VLAN]]
	- vynucení #Trunk -> defaultně povolené veškeré [[VLAN]]
	- dvojité falešné tagy -> odebere se pouze jeden -> komunikace do jiné [[VLAN]]
		- útočník musí být v nativní [[VLAN]]
	- **Ochrana:** #port_security, vypnutí #DTP, nestandardní native [[VLAN]]
- ### útoky prostřednictvím [[DHCP]]
	- #DHCP_starvation -> vyčerpání #address_pool pomocí #DHCPDISCOVER žádostí
	- #DHCP_spoofing -> falešný [[DHCP]] server -> falešné údaje (default gateway, DNS server)
		- **Ochrana:** #DHCP_snooping 
- ### útoky prostřednictvím ARP
	- #ARP_spoofing a #ARP_poisoning
		- útočník zasílá nevyžádané ARP reply s falešnou MAC adresou
			- **Ochrana**: #DAI - *Dynamic ARP Inspection*
				- Zabraňuje předávání neplatných ARP odpovědí na ostatní porty [[VLAN]]
				- Odchytává ARP požadavky
				- Prověřuje platnost MAC + IP
				- Loguje odpovědi s neplatným záznamem
				- Deaktivuje port při překročení limitu počtu odpovědí (error-disabling)
				- `ip arp inspection VLAN 10`
				- `ip ardp inspection trust` - důvěřovaný port
				- **Povolit #DHCP_snooping (v daných [[VLAN]] )
				- Kontrola adres
					- Destination MAC
					- Source MAC
					- IP address
						- podezřelé IP adresy - *0.0.0.0* *255.255.255.255*
						- `ip arp inspection validate {[src-mac] [dst-mac] [ip]}`
		- nastavení jiné MAC adresy na síťové kartě -> obcházení MAC filtrů
			- **Ochrana**: #port_security nebo #IPSG (*IP Source Guard*)
- ### útoky pomocí podvržených MAC a IP adres (Address spoofing)
	- podvržení záznamů v ARP tabulce -> #MitM -> **Man in the middle**
- ### útoky prostřednictvím [[STP]]
	- Změna topologie sítě (výběr #Root_bridge)
	- Přesměrování dat na útočníka -> #MitM
- ### zneužití #CDP **Cisco Discovery Protocol**
	- #CDP_Reconnaissance (průzkum) - broadcast, nešifrované
		- útočník se dozvídá o topologii sítě


# DHCP snooping
#DHCP_snooping 
- `show ip dhcp snooping`
- `shop ip dhcp snooping binding`
- nastavení portů reagující na [[DHCP]] zprávy
- rate limit DHCP requestů na #untrusted portu
- typy portů:
	- #trusted - předává [[DHCP]] zprávy, přímé připojení k serveru
	- #untrusted - přijímá pouze klientské [[DHCP]] zprávy, blokuje serverové
		- #DHCPOFFER, #DHCPACK
- vytváří #DHCP_binding_table 
	- důvěryhodné porty
	- mac dresa + #untrusted porty
	- využíván #DAI
- `ip dhcp snooping trust`
- `ip dhcp snooping limit rate 6`
	- ochrana před #DHCP_starvation 
	- limit serverových zpráv na #trusted portu
- `ip dhcp snooping vlan 5,10,50-52`

# Best practices
- Přesunutí neaktivních portů do neaktivní [[VLAN]] -> #Black_Hole_VLAN
- Vypnutí nepoužívaných portů
- Oddělení management [[VLAN]] od uživatelské
- Nová (nestandardní) nativní [[VLAN]]
- Připojení ke zařízení jen z management [[VLAN]]
- Jen SSH (vypnout telnet)
- Zakázání autonegotiation na režim #Trunk
- Nepoužívat #Dynamic_auto či #Dynamic_desirable na access portech

# Port Security
- #port_security - `show port-security <adresa/interface>`
- Vypnutí nevyužitých portů
- Ochrana před útoky na MAC tabulku
- Limituje počet připojených MAC adres na port <1 - 8192>
	- lze ručně definovat povolené MAC adresy
	- *nastavení počtu adres* - `switchport port-security maximum <number>`
		- standardně 3 -> PC, VoIP, Backup(Management)
- Přiřazení MAC adres
	- *Staticky* 
		- ručně nastavení MAC adresy
		- `switchport port-security mac-address <mac>`
	- *SecureDynamic*
		- nastavení právě připojené 
		- Neukládá se do #startup_config
	- *SecureSticky*
		- Adresa se uloží do #startup_configq
- #port_security_aging - smazání nastavené adresy po určené době
	- `sw port-securtity aging time 10` - 10 minut
	- `sw port-security aging type inactivity`
	- `show port-security fa0/1`
	- *Absolute* - odpočet od nastavení adresy
	- *Inactivity* - smazána po době neaktivity
- ### Port security modes / violation mode
	- *Protected* 
		- zahazuje neznámé rámce
		- nezvyšuje #violation_counter 
	- *Restrict*
		- zahazuje neznámé rámce
		- zvyšuje #violation_counter
	- *Secure-shutdown*
		- výchozí
		- vypíná port po detekci neznámého rámce #error-disabled
			- je třeba manuální zapnutí
		- zvyšuje #violation_counter

| mode     | discard traffic | syslog | violation counter | shut down port |
| -------- | --------------- | ------ | ----------------- | -------------- |
| protect  | Yes             | No     | No                | No             |
| restrict | Yes             | Yes    | Yes               | No             |
| shutdown | Yes             | Yes    | Yes               | Yes            |

### Access porty
- Nastavit ručně
- Vypnout DTP
- Nastavit do #Black_Hole_VLAN 

### Trunk porty
- Pouze kde je potřeba
- Vypnout DTP
- Native [[VLAN]] jinou než 1 nebo data [[VLAN]]

## Syslog
- záznam událostí na systému
- `debug ?` - živý výpis událostí
- obsahuje změny #violation_counter