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