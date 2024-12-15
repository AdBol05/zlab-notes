- **Access Control Lists** -> #Packet_filter 
	- jednotlivá pravidla -> #ACE **Access Controll Entries**
- #Inboud #ACL - příchozí, před routováním
- #Outbound #ACL - odchozí, před předáním driverům síťovky
```bash
access-list <access-list-number> {deny | permit | remark} <source> [source-wildcard] [log]

int g0/0
ip access-group <name/id> <out/in>

show ip int g0/0/0
show access-list # ukazuje počet odbavení
```
- ==Ve výchozím stavu nejsou aplikovavané žádné ACE a výchozí nastavení je deny any==
	- expilcitní povolování
- Pravidla se aplikují v zadávaném pořadí -> první zadané, první odbavené
- #Standard_ACLs - filtr pouze dle zdrojové IP adresy, většinou na #Inboud 
	- `access-list 10 permit 192.168.30.0 0.0.0.255` -> `1`-`99` #Standard_ACLs 
	- ==Neaplikují se na packety vznikající na routeru==
	- automaticky se seřadí, aby se jako první odbavovali host plravidla
- #Extended_ACLs - filtr dle dalších atributů (cílová a zdrojová IP adresa, protokol, TCP/UDP port)
	- `access-list 103 permit tcp 192.168.30.0 0.0.0.255 any eq 80` -> `100`-`199` #Extended_ACLs 
	- `access-list 114 permit tcp 192.168.20.0 0.0.0.255 any eq telnet`

- #Numbered_ACLs - čísla pravidel
- #Named_ACLs - definovaná názvem, vlastní konfigurační rozhraní
```bash
ip access-list extended FTP filter
permit tcp 192.168.10.0 0.0.0.255 any eq ftp
permit tcp 192.168.10.0 0.0.0.255 any eq ftp-data
```
# Firewall
- Systém (software / hardware) monitorující a řídící provoz a přístup k síťovým prostředkům
	- [[LAN Security]]
	- #Packet_filter - filtruje dle hlavičky packetu
		- nižší  [[Vrstvy OSI - ISO modelu]] ( #Síťová  #Transportní )
		- pravidla pro přístup k aplikacím a službám -> blokace/propouštění packetů
		- filtrace provozu dle typu provozu
		- povolení/zakázání přístupu k službám pro jednotlivé uzly
		- omezení provozu s cílem zvýšit výkon sítě
		- řízení toku dat
		- nastavení priority dle tříd provozu
	- #Content_filter - filtruje dle obsahu packetu

## Wildcard mask v ACL
- filtrace individuálních IP adres a skupin adres
	- `0` -> výsledná hodnota odpovídá hodnotě bitu v IP adrese
		- `0.0.0.0` -> explicitně povolená jen jedna adresa
	- `1` -> odpovídající bit v IP adrese je ignorován
- zkratky
	- host - `0.0.0.0` -> vyhoví jen jedna adresa
	- any - `255.255.255.255` -> vyhoví libovolná adresa

```bash
ip access-list standard PERMIT-ACCESS
remark ACE permits host 192.168.10.10
permit host 192.168.10.10
int s0/0/0
ip access-group PERMIT-ACCESS out
```

```bash
access-list 20 deny host 192.168.10.10
access-list 20 permit 192.168.10.0 0.0.0.255
interface g0/0/0
ip access-group 20 in

ip access-list standard LAN2-FILTER
permit host 192.168.10.10
deny any
int g0/0/1
ip access-group LAN2-FILTER out
```

## Standard ACL pro bezpečný terminálový přístup
- přístup pouze z management [[VLAN]]
- SSH, lokální uživatel
```bash
line vty 0 15
login local
transport input ssh
access-class 21 in
exit
access-list 21 permit 192.168.10.0 0.0.0.255 # management VLAN
access-list 21 deny any
```
