[[Windows server]]

#SSL #TSL
- **Secure Sockets Layer**
- #TCP port 443 -> #HTTPS
- #encryption pomocí digitálního certifikátu
- asymetrické šifrování
	- #public_key (tajný klíč) - zašifrování
	- #private_key (veřejný klíč) - dešifrování
- certifikát X.509

### Webový server
- #TCP port 80
- zabezpečení #SSL

#### Složky webového serveru
- default `%SystemDrive\inetpub\wwwroot%`
- složka odpovídající kořeni webu
- #virtual_directory
	- virtualní adresář
	- odpovídá fyzickému adresáři kdekoli na serveru nebo webu
- #Application_pool
	- soubor prostředků webu nebo aplikace (činný proces)
	- paměťová oblast webu
	- oddělení jednotlivých aplikaci


### FTP
#FTP
- **File Transfer Protocol**
- #TCP 20 a 21
- ověření pomocí hesla nebo anonymně
- samostatný nešifrovaný
	- nutná podpora #SSL
		-> #SFTP
		-  **SSH File Transfer Protocol**
		-> #FTPS 
		- **FTP over SSL**

### SMTP
#SMTP
- **Simple Mail Transfer Protocol**
- e-mail - odesílání
	- pro příjem #POP3 nebo #IMAP
- #TCP port 25

### IIS
- **Internet Information Services**
- Webový/aplikační server od Microsoftu
	- #FTP
	- #FTPS 
	- #SMTP 
	- #HTTP
	- #HTTPS
- Více webů na jednom serveru
	- #host_headers
		-  použití jedné IP a jednoho portu na vice stránkách
		- nastavení názvu, na který bude web odpovídat
	- IP adresa
	- port
- Ověřování
	- Anonymous
	- (ASP.NET Impesronation)
	- Basic authentication
	- (Digest Authentication)
	- Windows authentication
	- AD Client Certificate Authentication
- Výchozí soubory
	- `Default.htm`
	- `Default.asp`
	- `Index.htm`
	- `Index.html`
	- `Isstart.htm`
	- `Default.aspx`
- Zabezpečení
	- omezení přístupu (IP adresy, TCP porty)
	- firewall
	- ověřování uživatele
	- šifrování obsahu
	- autorizační pravidla
		- nastavení přístupů ke konkrétním webům, aplikacím atd.
#ASP 
- **Active Server Pages**
- jazyk pro tvorbu webů od microsoftu


### RAS
#RAS **Remote access server**
- připojení k síti vzdáleně
- #RD vzdálená plocha ( #RDP - **Remote Desktop Protocol** )
- #RemoteApp - připojení přímo ke vzdálené aplikaci

### VPN
#VPN **Virtual private network**
- propojení dvou počítačů po #WAN 
- zapouzdřená a zašifrovaná komunikace
- protokoly
	- #PPTP Point-to-Point Tunneling protocol
		- tunel i šifrování
		- zastaralý
	- #L2TP Layer 2 Tunneling Protocol
		- tunel bez šifrování
	- #SSTP Secure Socket Tunneling Protocol
		- tunel i šifrování
- #Split_tunneling
	- zakáže používat bránu ve vzdálené síti


### Virtuální servery
#Virtualní_počítač 
- Několik OS na jednom PC
- Oddělení služeb, testovací prostředí
	- #Hyper-V
		- #DEP - Data Execution Prevention
			- Intel -> #XD - eXecuted Disable
			- AMD -> #NS - No eXecute
			- Oddělení paměti pro instrukce a data
	- #QUEMU
	- #ESXi
- #Konsolidace
	- sloučení několika fyzických serverů do jednoho, kde běží virtuální servery
	- #P2V -  physical to virtual
	- #VVM - Microsoft System Center Virtual Machine Manager