[[Unix-like systémy]]
1. #POST - **Power On Self Test**
	- kontrola hardwaru
2. Vyhledání a načtení zavaděče #Bootloader 
	- ( #GRUB - **GRand Unified Bootloader**, #Windows_Boot_Manager)
	- #MBR - 512 B na začátku prvního oddílu
	- #BIOS-UEFI - EFI oddíl
3. Načtení komponentu #Bootloader pro zavedení daného operačního systému
4. #Bootloader načítá #kernel a obraz #initrd a #initramfs
	- detekce hardwaru
	- načtení #kernel
	- načtení #kernel modulů
	- připojení kořenového souborového systému
	- spuštění #init / #systemd PID = 1
		- zobrazení UI
		- spouští #služba 

## Run levels
- režimy operačního systému

| Runlevel | popis                                                          | Systemd target |
| -------- | -------------------------------------------------------------- | -------------- |
| 0        | Vypnutí- přechodový stav pro rychlé vypnutí                    | poweroff       |
| 1 / S    | Maintenance - minimum služeb, bez sítě                         | rescue         |
| 2        | Multiuser - #Debian (výchozí, GUI), #RedHat (bez podpory sítě) | -              |
| 3        | #Debian (nepoužívá), #RedHat (multiuser bez GUI)               | multi-user     |
| 4        | Nepoužívaný - vlastní řešení                                   | -              |
| 5        | #Debian (nepoužívá), #RedHat (3 + GUI)                         | graphical      |
| 6        | Restart systému                                                | reboot         |
| 7        | emergency                                                      | emergency      |
## Systemd
- nejpoužívanější #init systém
- používá #target
- default target
		- výchozí stav, kam se má systém dostat
	- spouštění služeb a závisejících položek
	- `systmectl get-dafault`
	- `systemctl set-default multi-user.target`
## Upstart
- náhrada #init s podporou skriptů
	- skripty pro přechod do stavů
- #RedHat distribuce