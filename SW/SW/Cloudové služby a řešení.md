#Azure
- **pronájem výpočetních služeb**
	- #Virtualní_počítač - [[Virtualizace a Kontejnerizace]] ( #Virtualizace_serverů  )
	- úložiště
	- databáze
	- #IoT
	- #AI / #ML
- rychlé a jednoduché rozšíření stávající infrastruktury
- vysoká dostupnost
	- garantována ve smlouvě #SLA - **Service-level agreement**
	- *99.9999---%*
- spolehlivost - schopnost systému zotavit se při výskytu chyby
- předpověditelnost - předpovídání budoucího výkonu či nákladů
	- výkon - budoucí potřeby prostředků
	- náklady - budoucí náklady na provoz
		- nástroje #TCO (**Total Cost of Ownership**) nebo *Pricing Calculator*
- zabezpečení
	- soulad s předpisy a zásadami - #governance
	- ochrana před kyberútoky
- velmi flexibilní - rychlé škálování
	- #vertikální_škálování - přidání hardwaru (RAM, CPU) do stávajícího počítače
	- #horizontální_škálování - přidání dalších celých virtuálních počítačů
![[shared-responsibility.svg]]
- #SaaS - *Software as a Service*
	- zákazník si pronajímá plně funkční aplikaci
	- spravuje pouze účty apod.
	- nejméně flexibilní - zákazník je pouze uživatel
- #PaaS - *Platform as a Service*
	- poskytovatel spravuje infrastrukturu, zabezpečení, konektivitu, operační systém, middleware a nástroje
	- zákazník řeší pouze svůj software
	- ideální pro vývojové prostředí
- #IaaS - *Infrastructure as a Service*
	- poskytovatel spravuje hardware, konektivitu a údržbu
	- zákazník zodpovídá za vše ostatní (instalace, databáze, aplikace, konfigurace)
	- nejflexibilnější, snadná migrace z #On-Premise
- #On-Premise - vlastní (lokální) řešení

### Privátní cloud
- soukromé datacentrum
- jedna firma

### Veřejný cloud
- Vytvářen a spravován poskytovatelem

### Hybridní cloud
- propojení veřejného cloudu a privátní infrastruktury

### Multicloud
- firma využívá více cloudových služeb

### Typy nákladů
- kapitálové náklady - Koupě vlastní infrastruktury
- provozní náklady - Cloudové služby #OpEx

# Nástroje pro správa cloudu (Azure)
- *Webový portál, (Azure) CLI, API, PowerShell*
- automatické škálování
- nasazování prostředků pomocí šablon
- monitorování
- zasílání varování
- ### Azure portal
	- webový portál  pro správu
- ### Azure Cloud Shell
	- CLI v prostředí webového prohlížeče
	- PowerShell, Bash
- ### Azure PowerShell
	- pracuje s tzv. cmdlety -> volá REST API
- ### Azure CLI - (Bash shell)


# Komponenty architektury Azure
- sada nástrojů pro vytváření, správu a nasazování aplikací
- využívání vyžaduje předplatné
	- vytvořené při založení účtu
	- lze mít více předplatných v rámci jednoho účtu
- v předplatném lze vytvářet #skupiny_prostředků a v nich poté jednotlivé #prostředky
	- #skupiny_prostředků - seskupení #prostředky (základní komponenta - počítač, databáze atd.)
		- může být navázána jen na jedno předplatné
			- jeden účet může mít více předplatných
		- nelze je vnořovat
		- některé #prostředky lze přesouvat mezi skupinami
![[rbac-scope-no-label.png]]

## Fyzická infrastruktura Azure
- Fyzická infrastruktura a Infrastruktura pro správu
- #Datacentrum
	- seskupené do #oblast nebo #zóna_dostupnosti
		- #oblast - jedno nebo více datacenter blízko sebe s nízkou latencí mezi sebou
		- #zóna_dostupnosti - #oblast s datacentry s různými poskytovateli
			- tvořena jedním nebo více datacentry s nezávislým napájením, chlazením a sítí -> redundance
			- propojené vysokorychlostními privátními optickými sítěmi
- #Region_pair - páry oblastí
	- spárované oblasti s minimální vzdáleností 300 mil
	- *West US - East US*
- #Sovereign_regions - Výsostná oblast
	- instance Azure oddělené od hlavní instance
	- oddělené z politických nebo zákonných důvodů
	- *US Gov Virginia, China, ..*

# Výpočetní služby Azure
- #Virtualní_počítač
	- #IaaS řešení
		- plná kontrola nad operačním systémem
		- vlastní software a služby
		- vlastní konfigurace
		- #NSG - **Network Security Group**
			- Povolení/zakázaní portů
			- "Firewall" virtuálního počítače
	- vytváření podle #šablona
		- JSON soubor s definicí stroje
- #Škálovací_sada
	- skupina #Virtualní_počítač se stejnou konfigurací -> #Load_Balancing 
		- rozložení zátěže na více strojů
- #Skupina_dostupnosti 
	- seskupení do #Update_domain nebo #Fault_domain -> #Load_Balancing 
		- #Update_domain - seskupení počítačů k instalaci aktualizací
			- možný restart kvůli aktualizaci bez výpadku služby
				- rozložení na počítače v jiné #Update_domain 
		- #Fault_domain - skupiny počítačů připojené na jinou infrastrukturu
			- redundance v případě výpadku např. switche nebo napájení
- #Virtual_desktop
	- Windows OS běžící v cloudu (Windows 10, WIndows 11)
	- #multisession - možnost současného přihlášení více uživatelů
- #Azure_container 
	- Instance  #PaaS umožňující načtení spouštění #kontejner 
	- #mikroslužby - jednotlivá řešení rozdělena do menších (na sobě nezávislých) celků
- #Azure_functions 
	- Bezserverové řešení řízené událostmi -> Naprogramované funkce se spouští v reakci na události
	- Automatické škálování
	- Platba za čas CPU potřebný pro běh funkce
	- #Stateless (bezstavové) - při každé reakci se spouští nezávisle na přechozím stavu
	- #Stateful (stavové) - při reakci se funkci předává kontext předchozích aktivit
- #Azure_app_service
	- vytváření a provozování webových aplikací
		- #API aplikace -> #REST_API
		- webové aplikace
		- WebJobs
		- Mobilní aplikace
	- automatické škálování a vysoká dotupnost
	- podpora WIndows, #Linux 
	- automatizované nasazování z GitHubu, #Azure DevOps, Git ...

# Virtuální sítě
- Zajištění komunikace mezi prostředky #Azure a s klientskými počítači v #On-Premise prostředí
	-> rozšíření stávajícího #On-Premise 
	- Izolace a segmentace
	- komunikace na internetu
	- komunikace mezi prostředky #Azure 
	- komunikace s prostředky #On-Premise 
		- #point-to-site - VPN z počítače mimo firmu do firemní (cloudové) sítě
		- #site-to-site - VPN z #On-Premise brány k #Azure_VPN_Gateway 
		- #Azure_ExpressRoute - vyhrazené připojení k #Azure (nejde přes intenet - privátní linky)
	- routing - mezi virtuálními i #On-Premise sítěmi
	- filtrování síťového provozu
	- propojení virtuálních sítí
- Integrovaná podpora koncových bodů veřejných i privátních IP adres 
	-> možnost privátních i veřejných IP adres 
- #Azure_VPN
	 - #policy_based VPN - výběr tunelu na základě IP adresy (pravidla)
	 - #route_basde VPN - definice rozhraní, definované routingem
		- #On-Premise -> propojení virt. sítí, #point-to-site 
	- odolnost #Azure_VPN_Gateway 
		- #active/standby instance -> aktivní a záložní gateway
		- #active/active instance -> dvě instance, dvě IP adresy, dvě připojení
		- #Azure_ExpressRoute failover -> #Azure_VPN_Gateway jako záloha při výpadku
		- #Zone_redundant gateway -> #Azure_VPN_Gateway v #zóna_dostupnosti 

# Azure DNS
- překlad prostřednisctvím infrastruktury #Azure 
- spolehlivý, výkonný, bezpečný (marketing intensifies)
- interní i externí prostředky, privátní domény, aliasy

# Služby úložiště Azure
- #Azure_storage -> úložiště přístupné odkudkoliv (**HTTP, HTTPS**)
- dostupnost, škálovatelnost atd.
- různé typy účtů -> služby a možnosti redundance
- ==vyžaduje storage account==
- Endpoint pro účet úložiště definovaný kombinací názvu účtu a endpointu služby #Azure_storage 
	- `https://<account>.<type>.core.windows.net`
	- #Blob storage
	- #Data_lake storage gen2
	- Azure files
	- #Qeue storage
	- Table storage
- uložení několik kopií -> #redundance
	- **Redundance v primární oblasti**
		- #LRS - **Locally Redundant Storage**
			- tři kopie v jednom datacentru (v primární oblasti)
			- ochrana před výpadkem disku nebo serveru
			- ==výchozí nastavení==
		- #ZRS - **Zone Redundant Storage**
			- využití #zóna_dostupnosti 
			- synchronní replikace dat přes tři #zóna_dostupnosti 
			- ochrana i před výpadkem celého datacentra nebo #oblast 
	- **Redundance v sekundátrní oblasti**
		- kopie do oblasti vzdálené několik set kilometrů od primární oblasti
			- ochrana před havárií primární oblasti
		- #GRS - **Geo-redundant storage**
			- asynchronní replikace #LRS do sekundární oblasti
		- #GZRS - **Geo-zone-redundant storage**
			- #ZRS s trojitou kopií do jednoho datacentra v sekundární oblasti
		- Standardně se přístup k sekundární oblasti povolí až po selhání primární oblasti
			- Možnost manuálního povolení vytvořením #RA-GRS nebo #RA-GZRS - *Read-access ...*
- #Azure_Blob
	- Univerzální, nestrukturované
	- Binární, textová data
	- Podpora big data
	- Přístup:
		- #Hot_access - velmi častý přístup
		- #Cool_access - velmi málo častý přístup
		- #Archive_access - velmi málo častý přístup, levné uložení, drahý přístup
- #Azure_Files
	- Sdílení souborů pomocí #SMB nebo #NFS
	- #On-Premise i cloud
	- #Azure_FileSync Možnost sdílené prostředky uložit do cache serverů [[Windows server]]
	- podpora skriptování
- #Azure_Qeue 
	- Úložiště (asynchronní) pro zprávy zasílaně mezi komponentami aplikací
	- max 64 KB
	- přístup HTTP, HTTPS
	- Využíváno #Azure_functions 
- #Azure_Disks
	- Blokové úložiště pro práci s #Virtualní_počítač 
	- Běžné disky s odolností a dostupností

## Možnosti migrace dat
- #Azure_Migrate - pomocník pro migraci z #On-Premise do cloudu
	- #On-Premise i #Virtualní_počítač 
- #Azure_DataBox - fyzický přesun velkých objemů dat (80 TB)
	- zákazník obdrží fyzické úložiště, které se následně převeze do datacentra
	- možnost i onovy z cloudu

## Možnost přesunu souborů
- #AzCopy - CLI nástroj pro kopírování souborů nebo #Blob 
- #Azure_Storage_explorer - frontend pro #AzCopy 
- #Azure_FileSync - centralizace sdílených souborů v #Azure_Files 
	- synchonizace s [[Windows server]] -> tvorba #CDN - **Content Delivery Network**

# Adresářová služba Azure
- Microsoft #Entra Domain Services
	- #Entra_Connect možnost propojení s #On-Premise #Active_Directory 
		-  z #Entra ID do domain services
	- přihlášení ke cloudovým i vlastním aplikacím
	- spravované doménové služby
	- monitorování podezřelých pokusů o přihlášení
	- #SSO **Single Sign On** - přístup k více aplikacím po jednom úspěšném ověření
![[Microsoft-Kyle-Blog-1.png]]
### Metody ověřování v Azure
- Jméno a heslo
- #SSO 
- #Multifactor #MFA - vícefaktorové ověření, požadováno více druhů ověření
- Bez použití hesla
	- Biometrie - otisk prstu, sken obličeje ... -> #Windows_Hello
	- Microsoft #Authenticator -> #MFA
	- Bezpečnostní klíče #FIDO2 - *Fast IDentity Online*, externí bezpečnostní klíč (USB, Bluetooth, NFC)

### Externí identita
- Přihlášení uživatelů mimo firmu (není zaměstnanec) do firemní infrastruktury
- #B2B *Collaboration* - externí uživatelé jsou ověřováni třetí stranou (důvěřovanou microsoftem - Google, Facebook...), přihlášení jako #guest 
- #B2B *Direct connect* - vzájemný důvěryhodný vztah dvou firem, vyžaduje #Entra ID v obou firmách
- #Entra #B2C - poskytování aplikací zákazníkům za použití #Azure #Active_Directory

# Zabezpečení v Azure
- ## Podmíněný přístup
	- Povolení / zamítnutí přístupu na základě identity uživatele, místa přihlášení a použitého zařízení
	- Vyžaduje #MFA v nestandardních situacích
	- Při přihlašování se vyhodnocují různé *signály* -> povolení / zamítnutí / požadavek na #MFA
- #RBAC **Role Based Access Control**
	- přístup na základě přiřazených rolí
		- každá role má přiřazená oprávnění
			- #Reader
			- #Contributor
			- #Owner
		- aplikace na #scope -> #prostředky #skupiny_prostředků Management group
		- #dědičnost -> role se propisují na vnořené prostředky
	- princip povolování přístupu
- ## Model Zero trust
	- každý požadavek považuje jako by přicházel z nedůvěryhodné sítě
	- "Nikomu nedůvěřuj"
	- Důsledné ověřování, nejmenší potřebná oprávnění, očekávání incidentů
- ## Model defense-in-depth
	- Ochrana dat před zcizením
	- Ochranné vrstvy, data veprostřed (chráněná okolními vrstvami)
		- Zpomalení útoku -> útočník musí prolomit jednu vrstvu za druhou
		- #Fyzické_zabezpečení - přístup do budovy
		- #Identita_a_přístup - řízení přístupu k infrastruktuře, využití #SSO a #MFA , audit událostí a změn
		- #Perimetr - identifikace a ochrana před síťovými útoky, varování a likvidace, firewall, ochrana před DDoS
		- #Síť - omezení konektivity na minimum
		- Výpočetní #prostředky  - zabezpečení přístupů k #Virtualní_počítač a fyzickým #prostředky , aktualizace a ochrana koncových uzlů
		- #Aplikace - odolávání zranitelností v kódu
		- #Data - ochrana pomocí #prostředky a procesů k zajištění #CIA

- ## Microsoft Defender for Cloud
	- Sledování zabezpečení a ochrana v #Azure 
	- Monitorování cloudu, #On-Premise , Hybridního i multicloudového prostředí
		- rady a upozornění na zlepšení bezpečnosti
	- Nasazení na počítače mimo #Azure pomocí #Azure_Arc v multicloudu pomocí funce #CSPM - **Cloud Security Posture Management**
	- Nativně integrován do služeb #Azure #PaaS, Datových služeb #Azure a sítí
	- Funkce:
		- Sledování stavu zabezpečení a identifikace zranitelností
		- Zabezpečení pomocí zásad zabezpečení v #Azure_Policy

# Náklady v Azure
- Provozní náklady #OpEx
- Ovlivněno
	- Druh a oblast nasazení #prostředky 
	- Využití #prostředky 
	- Správa #prostředky 
	- Zeměpisné umístění
	- Síťový provoz
	- Typ předplatného
- #Azure Marketplace -> možnost zakoupit hotová řešení třetími stranami
- Nástroje
	- Cenová kalkulačka - výpočet odhadu výdajů za služby
	- #TCO - **Total Cost of Ownership** - kalkulačka celkových nákladů na vlastnictví, porovnání #On-Premise a cloudové infrastruktury
	- Nástroj pro správu nákladů - rychlá kontrola nákladů na #prostředky #Azure , varování, rozpočty a automatizace správy, analýza nákladů firmy
		- Upozornění na náklady
			- Budget alerts - překročení rozpočtu
			- Credit alerts - blížící se vyčerpání prostředků
			- Department spending quota alerts - překročení dané kvóty oddělením firmy

# Značky
- #tags - třídění a organizace #prostředky 
- využití
	- správa prostředků
	- optimalizace nákladů
	- správa operací
	- zabezpečení
	- zajištění souladu a správného vedení firmy
	- automatizace a optimalizace pracovních procesů
- Lze spravovat portálem, CLI, REST API
- Název a hodnota

# Azure blueprints
- #blueprint - soubor nastavení a parametrů -> "*šablona*"
- #artifact - komponent #blueprint , který může (ale nemusí) obsahovat přídavné parametry
	- role a zásady
	- skupiny prostředků
	- šablony ARM
	- podpora verzování
- Zajištění standardizace předplatného služeb nebo prostředí pro nasazení
- Klonování nastavení a zásad na nové předplatné

# Microsoft purview
- správa dat v #On-Premise řešení, #SaaS a multicloudu
- automatické objevování dat -> třídění, tvorba "rodokmenu", třídí citlivá data
- funkce Microsoft 365
- mapy, vyhledávání, přehledy, využití, přístupy k datům

# Azure policy
- správa a přiřazování zásad
- #Audit #prostředky 
- vynucení konfigurace v souladu se standardy firmy
	- prověřování -> zákaz vytváření #prostředky bez souladu se standardy
- zásady se dědí na podřízené #prostředky 
- schopnost opravy nekompatibilních zásad
- #iniciativa - skupina navzájem příbuzných zásad - např *Enable Monitoring in Azure Security Center*
	- obsahuje jednotlivé zásady

# Zámky prostředků
- zamknutí prostředků - zabránění nechtěných změn nebo odstranění
	- *delete* - zabraňuje odstranění prostředku
	- *readOnly* - žádné úpravy konfigurace

# Portál Service Trust
- zajištění přístupu k různému obsahu, nástrojům atd.
	- zabezpečení a soukromí -> soulad s normami a standardy (*GDPR ...*)
- Podrobné údaje o mechanismu řízení a procesech pro ochranu služeb a dat zákazníků

# Azure Arc
- monitorování cloudové, multicloudové nebo #On-Premise infratsruktury
- #ARM - *Azure Resource Manager*
	- služba pro nasazování a správu služeb #Azure 
	- vytváření, aktualizace, odstranění atd #prostředky v účtu #Azure 
	- ### Šablony #Azure #ARM 
		- #infrastructure_as_code - nasazení infrastruktury pomocí kódu
		- JSON soubor popisující strukturu
- správa kompletního prostředí, #Virtualní_počítač, [[Cluster]] Kubernetes, SQL servery atd.

# Nástroje pro monitorování
- #Azure #advisor
	- vyhodnocení využívání #prostředky 
	- doporučení ohledně spolehlivosti, zabezpečení, výkonu a nákladů
- #Azure #Service_health
	- sledování stavu #prostředky a celkovýc stav infrastruktury #Azure 
	- #Azure #status - celkový stav #Azure ve všech oblastech -> informace o výpadcích
	- #Service_health - konkrétní pohled na služby #Azure používané zákazníkem -> info o výpadcích a plánovaných odstávkách apod.
	- #Resource_health - informace o konkrétních #prostředky 
- #Azure #Monitor
	- shromažďování a analýza dat
	- přápadné reakce na výsledky analýzy
	- #Azure, #On-Premise, multi-cloud
	- Zobrazení v dashboardu nebo pomocí dotazů Power BI
- #Azure #Log_Analytics
	- podrobné dotazování na data z #Azure #Monitor 
- #Azure #Monitor_Alerts
	- upozornění při překročení zadané hodnoty
	- podmínky vyvolání upozornění
	- #action_groups - více akcí v jedné skupině
- #Application_Insights
	- monitorování webových aplikací
	- sledování výkonu
	- posíláno *syntetických* požadavků -> kontrola stavu během nízkého vytížení