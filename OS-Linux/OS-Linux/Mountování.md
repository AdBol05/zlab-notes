![[Mountování 2023-09-19 11.55.43.excalidraw]]


```bash
fdisk /dev/sdb # Správa oddílů
> p # výpis tabulky oddílů
> n # vytvoření nového oddílu
> o # vytvoření MBR tabulky
> g # vytvoření GPT tabulky
> w # odejít a uložit
> q # odejít a neukládat

mkfs.ext4 /dev/sdb1 # Vytvoření filesystem
mkfs.btrfs
mkfs.vfat

mount /dev/sdb1 /mnt/ext4 # Připojení oddílu (co kam)
umount /dev/sbd1 # Odpojení oddílu
umount /mnt/ext4

mount -o ro /dev/sdb1 /mnt/ext4 # Připojení jen pro čtení

sudo mount -a
# pokusí se připojit nepřipojené oddíly z /etc/fstab

sudo falocate -l <velikost> <umístění> # vytvoří prázdný soubor s danou velikostí

blkid <oddíl> # vypíše UUID oddílu
```

`/etc/fstab`
- mountování oddílů při startu
```bash
# <odkud>       <kam>    <typefs> <options>  <záloha metadat>   <kontrola fs>

/dev/sdb10    /mnt/ext2    ext2    defaults         0                 1
#hrozí změna cesty disku! (přiřazuje BIOS)

/dev/mapper/i3x-swap none swap sw 0 0
#logický oddíl -> nehrozí změna cesty

LABEL="250MiB_ext2"     /mnt/ext2 ext2 defaults 0 1  
LABEL="oddil19"         /mnt/ro   ext4 ro       0 1  
UUID="64161A773DC7FF66" /mnt/ntfs ntfs defaults 0 1

UUID="SKJDHF563J654JHF"     none swap sw 0 0  # SWAP oddíl
/mnt/swapcointains/swap.swp none swap sw 0 0
LABEL="testswap"            none swao sw 0 0
```

zapsání filesystemu
- cesta -> "neměla" by se používat (písmena založena na času "ozvání se" disku)
- UUID -> unikátní identifikátor, vzniká při vytvoření filesystemu
- LABEL -> vytváří uživatel a ručí za unikátnost
```bash
sudo e2label /dev/sdb2 "nazev"                 # LABEL pro ext filesystémy
sudo btrfs filesystem label /dev/sdb21 "nazev" # LABEL pro btrfs filesystémy
sudo swaplabel -L "swaplabel" /dev/sdb31       # LABEL pro swap oddíly
```

Vytvoření swap
```bash
sudo falocate -l <veliost> <soubor> # vytvoření souboru s danou velikostí

mkswap <oddíl/soubor> # převod souboru nebo oddílu na swap

swapon -a # připojí všechny swapy z /etc/fstab
swapoff

sudo chmod 0600 /swap.swp # oprava oprávnění pro swap
```

# Logical volume management
```bash
sudo pvcreate /dev/sdb{3,4}                    # vytvoření physical volume
sudo vgcreate "nerad" /dev/sdb{3,4}            # vytvoření volume group
sudo lvcreate --name=data --size=150M "nerad"  # vytvoření logických oddílů
```
 - Umístění logických oddílů:
	 - `/dev/mapper/`
	 - cesta vždy stejná narozdíl od fyzických oddílů

# Šifrování oddílů
```bash
sudo cryptsetup luksFormat /dev/sdb40
#zašifruje oddíl s heslem

sudo cryptsetup open /dev/sdb40 crypt_sdb40 
#vytváří "bránu" do šifrovaného oddílu v /dev/mapper


#dd vytváří bytové kopie
#/dev/urandom generuje náhodné byty
sudo dd if=/dev/urandom of=/root/mujklicek bs=1k count=4
#vygeneruje 4096 náhodných bytů -> klíč pro šifrování

#použití klíče
sudo cryptsetup luksFormat --key-file=/root/mujklicek /dev/sdb41
sudo cryptsetup open --key-file=/root/mujklicek /dev/sdb41 crypt_sdb41

#přidání klíče nebo hesla
sudo cryptsetup luksAddKey /dev/sdb46 /root/klicek            #přidá klíč
sudo cryptsetup luksAddKey --key-file=/root/klicek /dev/sdb46 #přidá heslo
```

`/etc/crypttab`
- dešifrování disků při startu
- pokud v možnosti `key file` je hodnota `none` zeptá se při startu na heslo
```bash
# <target name>     <source name>         <key file>         <options>
crypt_sdb45 UUID="KJFUZRTVIUTGFJTZI5465R"   none               luks
    ..........                              /root/mujklicek    luks
```