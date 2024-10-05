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