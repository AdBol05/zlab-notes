#Azure
- **pronájem výpočetních služeb**
	- #Virtualní_počítač - [[Virtualizace a Kontejnerizace]]
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
- provozní náklady - Cloudové služby

## Správa cloudu
- *Webový portál, (Azure) CLI, API, PowerShell*
- automatické škálování
- nasazování prostředků pomocí šablon
- monitorování
- zasílání varování

# Komponenty architektury Azure
- sada nástrojů pro vytváření, správu a nasazování aplikací
- využívání vyžaduje předplatné
	- vytvořené při založení účtu
	- lze mít více předplatných v rámci jednoho účtu
- v předplatném lze vytvářet #skupiny_prostředků a v nich poté jednotlivé #prostředky
	- #skupiny_prostředků - seskupení #prostředky (základní komponenta - počítač, databáze atd.)
		- může být navázána jen na jedno předlatné
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