- #FreeBSD
- #Linux - [[Open source]]
### Dokumentace
- `--help` - nápověda od programu
- `man` - manuál programu
- `/usr/share/doc` - dokumentace
- `info` - podobné `man`

## Správa balíčků
- #Low-level_tools
	- #Debian - #dpkg
		- `dpkg -i ./file.deb`
	- #CentOS - #rpm
		- `rpm -Uvh file.rpm`
- #High-level_tools
	- #Debian - #apt
		- `apt install <package>`
	- #CentOS - #yum
		- `yum install <package>`

## Filesystémy
- #ext2
- #ext3
- #ext4
- #ReiserFS
- #xfs
- #FAT
- #HPFS/NTFS/exFAT
- `tune2fs` - informace o fs
	- počet inodes
	- čas mountingu
	- čas zápisu
- ### Swap
	- `mkswap` - vytvoření swap oddílu
	- `swapon` - připojení swap oddílu ( #mounting )

## Sledování a správa prostředků
- ### Disky
	- `du -h` - disk usage
	- `df -h` - disk free (`df -hi` - volné a obsazené inodes)
	- #### Zálohy
		- kopie disku do img `dd if=/dev/sda | gzip -c > /system_images/sda.img.gz`
		- kopie z img do disku `gzip -dc /system_images/sda.img.gz | dd of=/dev/sda`
		- vytvoření z archivu `tar czf backup.tar.gz file[0-9]`
		- obnovení z archivu `tar xjf backup.tar.gz --directory user_restore --same-permissions`
		- kopie na vzdálený počítač `dd if=/dev/sda | ssh root@remote cat > /dev/sda`
	- #### Synchronizace
		- `rsync -av source destination`
		- vzdálený adresář (SSH) 
			- `rsync -avzhe backups ssh root@remote:/destination`
			- `-a` recursive + odkazy, razítka, oprávnění
			- `-z` compression
			- `-h` human-readable
			- `-e` připojení SSH
			- `-v` verbose
	- #### Správa oddílů
		- `fdisk` - #MBR oddíly
		- `gdisk` - #GPT oddíly
		- Formátování
			- `mkfs -t <filesystem> -L <popisek> <oddíl>` - make filesystem
	- #### Připojování
		-> #mounting 
		- `/etc/fstab` -> automatické připojování oddílů při startu
			- options:
				- #auto - automatické připojení
				- #defaults - `async,auto,dev,exec,nouser,rw,suid`
				- #loop - připojení ISO obrazu
				- #noexec - zákaz spouštění souborů
				- #nouser - smí připojit pouze root
				- #remount - odpojení a připojení již připojeného fs
					- #ro - pouze pro čtení
					- #rw - čtení a zápis
				- #realtime - aktualizuje čas přístupu pokud čas atime nastal dříve než mtime
				- #user_xattr - povoluje nastavování atributů místním i vzdáleným uživatelům
		- #mount_point - bod / adresář připojení
		- `mount -t <filesystem> <zařízebí> <mountpoint> -o <options>`
- ### RAM
	- `free -g`
		- total - celková velikost RAM
		- used - využitá RAM
		- free - volná (total - used)
		- shared - sdílená více procesy
		- buffers - rezervovaná OS
		- cached - naposledy použité soubory
- ### CPU
	- `top` / `htop` - běžící procesy
		- čas a doba běhu
		- přihlášený uživatel
		- průměrné vytížení (1, 5, 15 minut)
		- počet a stav procesů
		- využití RAM, Swap a CPU

### Procesy
- `ps` / `pstree` - výpis běžících procesů
	- vlastník procesu
	- PID
	- využití CPU
	- čas spuštění
	- tty (`?` -> deamon)
	- kumulovaný čas CPU
	- příkaz pro proces
- Záznamy / logy
	- `/var/log` - adresář s logy procesů
	- `dmesg` - problémy s hardwarem 
- #### Signály:

| Signál  | Číslo | Popis                                                       |
| ------- | ----- | ----------------------------------------------------------- |
| SIGTERM | 15    | korektní ukončení                                           |
| SIGINT  | 2     | přerušení procesu, může být ignorován                       |
| SIGKILL | 9     | bezpodmínečné přerušení, nelze ignorovat                    |
| SIGHUP  | 1     | "Hang up", opětovné načtení konfigurace démona bez restartu |
| SIGTSTP | 20    | pozastavit činnost, počkat až bude možné pokračovat         |
| SIGSTOP | 19    | zastavení, nemůže být ignorováno                            |
| SIGCONT | 18    | pokračování pozastaveného procesu                           |


### informace o swap oddílech
- `cat /proc/swaps`


# Připojení síťových souborových systémů
- #NFS - Unix-like klient
	- `nfs-utils`, `nfs-utils-lib`, `nfs-common`
	- `mount //192.168.0.10/NFS-SHARE`
- #SMB - Windows a Unix-like client
	- `samba-client`, `samba-common`, `cifs-utils`
	- vyhledání - `smbclient -L 192.168.0.10`
	- připojení - `smbclient //192.168.0.10/share user%password`

# RAID a LVM
- #RAID0 - prokládání
- #RAID1 - zrcadlení
- #RAID5 - prokládání s paritou
- `mdadm -create -verbose /dev/md0 level=stripe -raid-devices=2 /dev/sdb1 /dev/sdc1`
- `cat /proc/mdstat`, `mdadm -detail /dev/md0` - stav oddílu
- `mdadm --detail --scan --asemble` - najde a sestaví RAID

### Struktura LVM
- #Physical_volume - ( #PV ) celý disk nebo odddíl
- #Volume_group - ( #VG ) jednotka z jednoho nebo více fyzických svazků
- #Logical_volume - vytváří se z #VG, dá se měnit jeho velikost,  chová se jako diskový oddíl


# Konfigurace sítě
- `hostname <name>` -> `/etc/hostname`
- `ip addr add 192.168.0.2/24 brd + dev eth0`
	- nastavení rozhraní eth0 IP adresy a masky
	- broadcast lze nastavit manuálně ( `+` vypočte z masky )
- `ip link set eth0 <up/down>` 
	- aktivování/deaktivování rozhraní
- `ip route add default via 192.168.0.254`
- `ip route list`
- ## DNS
	- `/etc/hosts` `/etc/resolv.conf` -> konfigurační soubory #DNS
```sh
search domain.com #prohledavana domena
nameserver 192.168.0.1 #DNS server
```

- ## DHCP
	- `dhclient` `dhcpd` -> #DHCP klient
	- `/etc/dhclient.conf` -> konfigurační soubor #DHCP klienta


# Uživatelské účty
- `adduser` `useradd` -> vytvoření uživatele
	- `/etc/passwd` -> seznam uživatelů
		- `[username]:[x]:[UID]:[GID]:[Comment]:[HomeDir]:[Shell]`
			- `[x]` -> heslo uložené v `/etc/shadow`
	- `/home/<user>` -> domovská složka uživatele
		- `~/.bash_logout` `~/.bash_profile` `~/.bashrc`
- `userdel --remove` -> smazání účtu
- `groupadd` -> vytvoření skupiny
	- `/etc/group` -> seznam skupin
		- `[groupname]:[grouppassword]:[GID]:[members]`
			- `[x]` -> skupina bez hesla
- `usermod` -> upravení nastavení uživatele
	- `--expiredate` -> vypršení účtu
	- `-aG / --append --groups` -> přidat uživatele do skupiny
	- `-d / --home` -> nastavení domovského adresáře
	- `--shell` -> nastavit shell

`chown user:group file` - změna vlastníka souboru
`chgrp new_group file` - změna vlastnící skupiny
# Speciální oprávnění
- #Sticky_bit
	- u adresářů povoluje uživatelům mazat jen jejich vlastní soubory
	- `chmod o+t file.txt` `chmod 1755 file.txt`
- #SETUID
	- při spuštění souboru se spustí s právy vlastníka
	- `chmod u+s file.txt` `chmod 4755 file.txt`
- #SETGID 
	- umožňuje uživateli přístup k souboru s právy vlastnící skupiny
	- `chmod g+s directory` `chmod 2755 directory`

### chattr
- `chattr +i file` - soubor je *immutable* 
	- -> nelze přejmenovat, přesunout ani smazat (ani root)
- `chattr +a file` - soubor je *append only*
	- -> lze pouze přidávat obsah
- `lsattr` - zobrazí speciální oprávnění