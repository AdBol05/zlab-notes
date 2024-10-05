- #FreeBSD
- #Linux - [[Open source]]
	- #Debian 
		- #Ubuntu
	- #Fedora
	- #RedHat - Komerční (placená podpora a služby)
### Dokumentace
- `--help` - nápověda od programu
- `man` - manuál programu
- `/usr/share/doc` - dokumentace
- `info` - podobné `man`
	- nejpodrobnější

## Správa balíčků
- #Low-level_tools
	- #Debian - #dpkg
		- `dpkg -i ./file.deb`
	- #CentOS / #RedHat- #rpm
		- `rpm -Uvh file.rpm`
- #High-level_tools
	- Metadata
		- verze
		- závislosti
	- Automaticky instaluje závislosti
	- #Debian - #apt / #apt-get
		- `apt install <package>`
		- `apt-get install <package>`
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
		- `-h` - Human readable
	- `lsblk` - vypsání blokových zařízení -> disky, oddíly, velikost, mountpoint
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
	- #### Formátování a správa oddílů
		- `fdisk` - #MBR oddíly (zastaralý)
		- `gdisk` - #GPT oddíly
		- Formátování
			- `mkfs -t <filesystem> -L <popisek> <oddíl>` - make filesystem
				- #ext2 
				- #ext3 
				- #ext4
			- `mkswap <addíl>` - make swap
				- #swap - alternativa pagefile.sys
					- `swapon` / `/etc/fstab`
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
		- `mount -t <filesystem> <oddíl> <mountpoint> -o <options>`
		- `umount <oddíl>`
```sh
# <odkud> <kam> <typefs> <options> <záloha metadat> <kontrola fs>

/dev/sdb10 /mnt/ext2 ext2 defaults 0 1
#hrozí změna cesty disku! (přiřazuje BIOS)

/dev/mapper/i3x-swap none swap sw 0 0
#logický oddíl -> nehrozí změna cesty
```

- ### RAM
	- `free -h`
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
`kill -9 PID`

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

# RAID
- #RAID0 - prokládání
- #RAID1 - zrcadlení
- #RAID5 - prokládání s paritou
- `mdadm -create -verbose /dev/md0 level=stripe -raid-devices=2 /dev/sdb1 /dev/sdc1`
- `cat /proc/mdstat`, `mdadm -detail /dev/md0` - stav oddílu
- `mdadm --detail --scan --asemble` - najde a sestaví RAID

# LVM
- logické oddíly
	- cesta vždy stejná na rozdíl od fyzických
- `/dev/mapper`
```bash
sudo pvcreate /dev/sdb{3,4}                    # vytvoření physical volume
sudo vgcreate "nerad" /dev/sdb{3,4}            # vytvoření volume group
sudo lvcreate --name=data --size=150M "nerad"  # vytvoření logických oddílů
```
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
- `adduser` `useradd` -> vytvoření uživatele (`adduser` interaktivní) 
	- `cat /etc/passwd` -> seznam uživatelů
		- `[username]:[x]:[UID]:[GID]:[Comment]:[HomeDir]:[Shell]`
			- `[x]` -> heslo uložené v `/etc/shadow`
	- `/home/<user>` -> domovská složka uživatele
		- `~/.bash_logout` `~/.bash_profile` `~/.bashrc`
- `userdel --remove` -> smazání účtu
- `groupadd` -> vytvoření skupiny
	- `cat /etc/group` -> seznam skupin
		- `[groupname]:[grouppassword]:[GID]:[members]`
			- `[x]` -> skupina bez hesla
- `usermod` -> upravení nastavení uživatele
	- `--expiredate` -> vypršení účtu
	- `-aG / --append --groups` -> přidat uživatele do skupiny
	- `-d / --home` -> nastavení domovského adresáře
	- `--shell` -> nastavit shell

`chown user:group file` - změna vlastníka souboru
`chgrp new_group file` - změna vlastnící skupiny

## Oprávnění
- `chmod 777 file.txt` - nastavení oprávnění
	- `chmod u+r,g+r,o+r file.txt`
- `chown user file.txt` - nastavení vlastníka
- `rwxrwxrwx` - (`user` `group` `other`)
	- `-rwxr--r--` -> `744` (`4+2+1` `4+0+0` `4+0+0`) - osmičková soustava
	- `r` - read
	- `w` - write
	- `x` - execute
### Speciální oprávnění
- #SETUID
	- při spuštění souboru se spustí s právy vlastníka
	- `chmod u+s file.txt` `chmod 4755 file.txt`
- #SETGID 
	- umožňuje uživateli přístup k souboru s právy vlastnící skupiny
	- `chmod g+s directory` `chmod 2755 directory`
- #Sticky_bit
	- u adresářů povoluje uživatelům mazat jen jejich vlastní soubory
	- `chmod o+t file.txt` `chmod 1755 file.txt`

### chattr
- `chattr +i file` - soubor je *immutable* 
	- -> nelze přejmenovat, přesunout ani smazat (ani root)
- `chattr +a file` - soubor je *append only*
	- -> lze pouze přidávat obsah
- `lsattr` - zobrazí speciální oprávnění

# Služby
- #služba - proces běžící na pozadí
- `systemctl restart služba
	- `start` / `stop`
	- `stop`
	- `enable` / `disable`
	- `status`
## DNS
- #DNS - překlad jmen na IP adresy a zpět
- nástupce `/etc/hosts` - staticky nastavené IP adresy a jména
- služba `bind`
	- `/etc/named.conf`
```bash
options {
	listen-on port 53 { 127.0.0.1; 192.168.0.18};
	allow-query {localhost; 192.168.0.0/24;};
	recursion yes;
	forwarders {
		8.8.8.8;
		8.8.4.4
	};
}

zone "prodej.firma.com" IN {
	type master;
	file "/var/named/prodej.firma.com.zone";
};

zone "0.168.192.in-addr.arpa" IN {
	type master;
	file "/var/named/0.168.192.in-addr.arpa.zone";
};
```

#zóna - část #DNS stromu -> doména
- konfigurace v `/var/name/prodej.firma.com.zone`
```bash
$TTL 604800
@ IN SOA prodej.firma.com. root.prodej.firma.com. (
	2016051101 ; Serial
	10800 ; Refresh
	3600 ; Retry
	604800 ; Expire
	604800) ; Negative TTL
;
@ IN NS prodej.firma.com.
dns IN A 192.168.0.18
web1 IN A 192.168.0.29
mail1 IN A 192.168.0.28
mail2 IN A 192.168.0.30
@ IN MX 10 mail1.prodej.firma.com.
@ IN MX 20 mail2.prodej.firma.com.
www.web1 IN CNAME web1
```
`systemctl start named`
## DHCP
- `/etc/dhcp/dhcp.conf`
```bash
option domain-name "firma.lan";
option domain-name-servers ns1.firma.lan, ns2.firma.lan;
default-lease-time 3600;
max-lease-time 7200;
authoritative;

subnet 192.168.1.0 netmask 255.255.255.0 {
	option routers 192.168.1.1;
	option subnet-mask 255.255.255.0;
	option domain-search "firma.lan";
	option domain-name-servers 192.168.1.1;
	range 192.168.1.10 192.168.1.100;
	range 192.168.1.110 192.168.1.200;
}
```
`systemctl start isc-dhcp-server`
`ufw allow 67/udp`