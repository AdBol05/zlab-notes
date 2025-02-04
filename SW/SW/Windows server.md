#Role
-> určuje "účel" server
-> Má určité **funkce** (upřesňuje a rozšiřuje role)
- Database server (MariaDB, MongoDB, ScyllaDB)
- File server (FTP, SFTP, SMB) - [[Sdílení složek a tiskáren]]
- Print server
- Web server (HTTP, HTTPS)
- Mail server (IMAP, POP3)
- [[NAS, SAN]]

#Hardware
- Redundance a spolehlivost

- CPU
	- Rychlost -> GHz
	- vícejádrové procesory -> multi-core
	- Instrukce -> matematické a logické operace
	- Intel a AMD
	- architektura (64bit, 32bit)
		- AMD64 -> 64bit
		- RISC-V
		- ARM64
	- program -> kompilace -> instrukce (machine code) -> RAM/cache -> ALU

- RAM
	- valotilní -> potřebuje napájení pro udržení dat 
	- instrukce pro procesor
	- ECC -> error correction code
	- Statická X Dynamická
		- Statická - 6 tranzistorů (dražší)
		- Dynamická - 1 tranzistor a kondenzátory (vybíjí se -> potřebuje obnovy)

- Úložiště
	- HDD -> rotační, magnetické, mechanické
	- SSD -> čipy NAND, rychlejší, dražší

- Základová deska
	- BUS
		- North bridge -> CPU, RAM, PCIe
		- South bridge -> SATA, USB, PCI, BIOS
		![[Drawing 2023-09-15 09.26.41.excalidraw]]

- Zdroj, Skříň, Chlazení

#BIOS-UEFI
- Basic Input Output System
- Unified Extensible Firmware Interface
	- **BIOS**
		- 2TB ma disk size
		- #MBR
		- bootování dle MBR
	- **UEFI**
		- max disk size 
		- #GPT 
		- nemá na disku bootovací záznam
- Firmware
- Základní funkce (ventilátory, stavové LED)
- Zavádí bootloader, který zavádí systém
- Setup (CMOS) -> bootovací sekvence

#Subsystémy
- CPU
- Paměť
- Úložiště
- Síť
Každý subsystém může být úzkým hrdlem a pokud jeden selže, havaruje celý server.

![[Drawing 2023-09-15 09.20.44.excalidraw]]


#Porty
- Seriové
	- USB
	- Ethernet
	- UART

- Paralelní
	- DB25

- Digitální
	- HDMI

- Analogové
	- VGA

#Virtualizace_serverů
- více OS na jednom fyzickém počítačí
- Oddělení jednotlivých služeb
- Efektivní využití hardwaru
- Softwarová redundance

#Software_OS
- Hostitelský systém určen dle role serveru (Web, DB, VPS, SMB, FTP)
	- TrueNAS (FreeBSD, Ubuntu)
	- Proxmox (Ubuntu)
	- Unraid (Linux Slackware)
	- Windows Server 2012, 2016, 2019, 2022 (64bit)
		- Datacentrum - Datacentra, cloud, virtualizace vázané na počet jader(==$6.155==)
		- Standard - Fyzická nebo minimálně virtualizovaná prostředí, vázané na počet jader (==$972==)
		- Essentials - 25 uživatelů, 50 zařízení, vázané na server (==$501==)
		-> Plná verze  (Desktop experience) - GUI
		-> Server Core (Power Shell) - CLI
	- Ubuntu
	- Debian
	- ~~CentOS~~
	- FreeBSD
- čistá instalace vs upgrade
	- čistá instalace - smaře všechna předchozí data (nutnost unstalace a konfigurace všeho znovu)
	- upgrade - nechává předchozí soubory a konfigurace (riziko konfliktů)

#Sysprepr 
- Oprava po klonování disku
	- Odstranění duplicit
	- Opravení konfliktů v síťi

#Bezopslužná_instalace
- odpovědi na otázky installeru uložené v souboru xml
	- `autounattend.xml`
	- server image manager

#WDS
- Windows Deployment Services
- síťová instalace Windows
- automatizace pomocí skriptů
- instalační soubory ve formátu WIM (Windows Image Format)

#WSUS
- Windows server update services
- Vlastní server pro aktuakizace windows na místní síťi
	- Menší zátěž internetové přípojky
	- Kontrola aktualizací

#MECM
- Microsoft Endpoint Configuration Manager
- Distribuce aktualizací z jednoho počítače na ostatní v lokální síťi

![[Windows server 2023-09-22 09.05.38.excalidraw]]

#MMC
- Microsoft Management Console
- Sada nástrohů pro správu
	- zásady skupin