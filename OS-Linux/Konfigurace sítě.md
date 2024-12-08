![[Drawing 2024-09-17 08.01.53.excalidraw]]

## TUI pro konfiguraci sítě
`sudo nmtui`

## Routování
`/etc/sysctl.conf`
- 28. řádek
```
# Uncomment the next line to enable packet forwarding for IPv4
net.ipv4.ip_forward=1
```


# DHCP
`sudo apt install isc-dhcp-server`
- `/etc/dhcp/dhcpd.conf`
```bash
option doamin-name "pepa.koko";
option domain-name-servers 192.168.69.1, 10.0.50.1;

default-lease-time 600;
max-lease-time 7200;
ddns-update-style none;

subnet 10.0.50.0 netmask 255.255.255.0 {}

subnet 192.168.69.0 netmask 255.255.255.0 {
	range 192.168.69.2 192.168.69.253;
	option routers 192.168.69.1;
}

host apache {
	hardware ethernet 08:00:27:e7:f3:1d;
	fixed-address 10.0.50.2;
	option routers 10.0.50.1;
}
```

- `/etc/default/isc-dhcp-server`
```bash
INTERFACESv4="enp0s8 enp0s9"
INTERFACESv6=""
```
`sudo systemctl start isc-dhcp-server`
`ufw allow 67/udp`

# DNS
`sudo apt instal bind9` `sudo apt install systemd-reslve`
- `/etc/bind/named.conf.options`
```bash
options {
	directory "/var/cache/bind";

	forwarders { 10.20.1.12; 10.20.1.13; };

	dnssec-validation no;
	forward only;
	
	allow-query { 192.168.69.0/24; 10.0.50.0/24; };

	listen-on { 192.168.69.1; 10.0.50.1; };
	listen-on-v6 { any; };
};
```

### Definice zón
- `/etc/bind/named.conf.local`
```bash
//dopredny preklad
zone "pepa.koko" {
	type master;
	file "/etc/bind/db.pepa.koko";
};

//zpetny preklad
	//192.168.69.0/24
	//192.168.69
	//69.168.192.in-addr.arpa
zone "69.168.192.in-addr.arpa" {
	type master;
	file "/etc/bind/db.69.168.192.in-addr.arpa";
};

zone "50.0.10.in-addr.arpa" {
	type master;
	file "/etc/bind/db.50.0.10.in-addr.arpa";
};
```

`/etc/bind/db.pepa.koko` - generátor https://pgl.yoyo.org/as/bind-zone-file-creator.php
```bash
; BIND db file for pepa.koko

$TTL 86400

@       IN      SOA     ns.pepa.koko.      hostmaster.example.com. (
                        2024100801	; serial number YYMMDDNN
                        28800           ; Refresh
                        7200            ; Retry
                        864000          ; Expire
                        86400           ; Min TTL
			)

                NS      ns.pepa.koko. 


$ORIGIN pepa.koko.

; záznamy pro router - DNS server
ns A 192.168.69.1
ns A 10.0.50.1

gw CNAME ns
router CNAME ns

; záznamy pro apache (server)
apache A 10.0.50.2
```

- za záznamy nekončící `.` se připojuje `.$ORIGIN`
#### A záznam
```bash
domain_name [value1] [TTL] [CLASS] A ip_address
domain_name [value1] [TTL] IN A ip_address
```
- `IN` je class INTERNET
`ns.pepa.koko. A 192.168.69.1` = `ns A 192.168.69.1`

#### CNAME záznam
- "alias"
```bash
gw CNAME ns
router CNAME ns

server CNAME apache
```

#### PTR záznam
- pointer - překlad na jinou doménu
- využití pro zpětný překlad@updates
```bash
1 PTR ns.pepa.koko  ; za 1 se připojuej $ORIGIN .69.168.192
```

`sudo systemctl start named`


### Ověření
- `sudo apt install dnsutils`
	- `dig @nameserver domain_name` - dopředný překlad
	- `dig @nameserver -x ip_address` - zpětný překlad

# NAT
`sudo apt install systemd-resolved`
`sudo apt install iptables` - **Firewall**

`sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE`

![[Konfigurace sítě 2024-10-22 08.38.00.excalidraw]]

`sudo apt install iptables-persistent`
- Zachování pravidel po restartu
- uložit aktuální nastavení
- `/etc/iptables/rules.v4`
	- obsahuje nastavená pravidla
```bash
*filter
:INPUT DROP [0:0]  
:OUTPUT ACCEPT [0:0]  
:FORWARD DROP [0:0]

# povolení přijímání odpovědí
-A INPUT -m state --state=RELATED,ESTABLISHED -j ACCEPT

# povolení SSH, DNS, DHCP
-A INPUT -p tcp -i enp0s8 -m multiport --dports=22,53,67 -j ACCEPT #SSH,DNS,DHCP
-A INPUT -p tcp -i enp0s9 -m multiport --dports=22,53,67 -j ACCEPT

# povolení ICMP odpovědí
-A INPUT -p icmp -i enp0s8 --icmp-type=echo-request -j ACCEPT  
-A INPUT -p icmp -i enp0s9 --icmp-type=echo-request -j ACCEPT

# povolení HTTP, HTTPS, MARIADB
-A FORWARD -p tcp -i enp0s8 -o enp0s9 -m multiport --dports=80,443,3306 -j ACCEPT
COMMIT

# povolení odpovědí ze serveru
-A FORWARD -i enp0s9 -o enp0s8 -m state --state=RELATED,ESTABLISHED -j ACCEPT

# povolení klientovi HTTP a HTTPS ven a odpovědi dovnitř
-A FORWARD -p tcp -i enp0s8 -o enp0s3 -m multiport --dports=80,443 -j ACCEPT  
-A FORWARD -i enp0s3 -o enp0s8 -m state --state=RELATED,ESTABLISHED -j ACCEPT
```
- `sudo iptables-restore < /etc/iptables/rules.v4` - načtení nastavení bez restartu

# SSH
- `~/.ssh/config`
```bash
host R
user fresh
hostname router
port 22
```

- `ssh-keygen`
- `ssh-copy-id <host>`

# Apache
- `sudo apt install apache2 libapache2-mod-php`
- smazání výchozí stránky (překrývá chybové hlášky)
	- `sudo a2addsite 000-default.conf`
	- `sudo rm -rf /var/www/html`
- `/etc/apache2/apache2.conf`
	- přidání adresáře na ~170. řádku
- Přesměrování `gw` na `apache`/`server` 
	- `sudo iptables -t nat -A PREROUTING -p tcp -m multiport --dports=80,443 -d 192.168.69.1 -j DNAT --to-destination=10.0.50.2`
	- `sudo iptables -t nat -A PREROUTING -p tcp -m multiport --dports=80,443 -d 10.0.50.1 -j DNAT --to-destination=10.0.50.2`
## HTTP
- `/etc/apache2/sites-available/pepa.koko.conf`
```xml
<VirtualHost *:80>
	ServerName www.pepa.koko

	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/pepa.koko

	Redirect / https://www.pepa.koko

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
<VirtualHost>
```
- `sudo a2ensite pepa.koko.conf`
- `sudo systemctl restart apache2`

## HTTPS
- `sudo a2enmod ssl`
- `sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/pepa.koko.key -out /etc/ssl/certs/pepa.koko.crt` - 
- `/etc/apache2/sites-available/pepa.koko.ssl.conf`
```xml
<VirtualHost *:443>  
        ServerName www.pepa.koko
        ServerAdmin webmaster@localhost  
        DocumentRoot /var/www/pepa.koko

        ErrorLog ${APACHE_LOG_DIR}/error.log  
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        SSLEngine on  
        SSLCertificateFile      /etc/ssl/certs/pepa.koko.crt  
        SSLCertificateKeyFile   /etc/ssl/private/pepa.koko.key

        <FilesMatch "\.(?:cgi|shtml|phtml|php)$">  
                SSLOptions +StdEnvVars  
		</FilesMatch>  
		<Directory /usr/lib/cgi-bin>  
                SSLOptions +StdEnvVars  
		</Directory>  
</VirtualHost>
```
- `sudo a2ensite pepa.koko.ssl.conf`
- `sudo systemctl restart apache2`

# MariaDB
- `sudo apt install mariadb-server`
- `sudo mysql_secure_installation`
	- vše na `yes` - doporučeno
- `/etc/mysql/mariadb.conf.d/50-server.cnf`
```bash
Bind-address = 127.0.0.1,10.0.50.2
```
`sudo systmectl restart mariadb`
```sql
CREATE DATABASE IF NOT EXISTS pepa_koko;
CREATE USER IF NOT EXISTS 'admin'@'%' identified by 'admin';
GRANT ALL PRIVILEGES ON pepa_koko.* TO 'admin'@'%';
```
