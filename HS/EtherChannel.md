![[EtherChannel 2024-01-29 10.41.52.excalidraw]]
### [[STP]] zablokuje jednu linku
-> stále rychlost pouze jednoho rozhraní

#EtherChannel
- **Link aggregation**
- Cisco technologie
	- 802.3ad a 802.1ax na základě Cisco #EtherChannel
- Spojení více fyzických linek do jedné virtuální (logický kanál) -> #port_channel_interface
	- -> #LoadBalancing - rovnoměrné rozložení zátěže
	- -> [[STP]] neblokuje redundantní linky
- Spojení pouze portů stejného typu (nelze G0/0 + F0/0)
	- musí mít identickou konfiguraciě
		- [[VLAN]]
		- Duplex
		- Rychlost
	- Max 8 portů dohromady
		- #FastEthernet max 800 Mbps
		- #GigabitEthernet 8 Gbps
- Na Cisco switchích max 6x #EtherChannel
- Protokoly pro automatickou konfiguraci
	- #PAgP - **Port Aggregation Protocol** (Cisco)
	- #LACP - **Link Aggregation Control Protocol** (IEEE)


#PAgP 
- Vykomunikuje nastavení linky
- Každých 30 sekund kontroluje a řeší problémy
- 3 stavy portů
	- #Desirable - aktivně vyjednává, posílá #PAgP packety
	- #Auto - pasivně přijímá #PAgP packety
	- #On - #EtherChannel je vytvořen ručně

| S1        | S2               | Channel Establishment |
| --------- | ---------------- | --------------------- |
| ON        | ON               | Yes                   |
| ON        | DESIRABLE / AUTO | NO                    |
| DESIRABLE | DESIRABLE        | YES                   |
| DESIRABLE | AUTO             | YES                   |
| AUTO      | DESIRABLE        | YES                   |
| AUTO      | AUTO             | NO                    |


#LACP 
- IEEE 802.1ax
- Podobné #PAgP 
- Dynamické vytvoření #EtherChannel
- 3 stavy portů
	- #Active - aktivně vyjednává, posílá #LACP packety
	- #Passive - pasivně přijímá #LACP packety, řídí se jimi, sám nic neinicializuje
	- #On - nastaven ručně

| S1 | S2 | Channel Establishment |
| ---- | ---- | ---- |
| ON | ON | Yes |
| ON | ACTIVE / PASSIVE | NO |
| ACTIVE | ACTIVE | YES |
| ACTIVE | PASSIVE | YES |
| PASSIVE | ACTIVE | YES |
| PASSIVE | PASSIVE | NO |

## Konfigurace
- Všechny aggregované porty musí podporovat #EtherChannel 
- Stejné konfigurace portu
	- [[VLAN]]
	- Duplex
	- Rychlost
- Nelze kombinovat #LACP a #PAgP na jedné lince

```
interface range fa0/1 - 5
shut
speed 100
duplex full
sw mo tr
sw nonegotiate
channel-group 1 mode [auto/desirable]
no shut

int port-channel 1
switchport trunk encapsulation dot1q
switchport mode trunk
swtitchport native vlan 1000
switchport trunk allowed vlan 110,120,190
no shut
```

```
show interfaces port-channel <number>
show etherchannel port-channel
show interfaces etherchannel
show etherchannel summary
show interfaces <port> etherchannel
```

## Load Balancing
- #LoadBalancing - rovnoměrné rozložení zátěže
- na základě MAC adresy, IP adresy nebo čísla portu
	- `dst-ip` - Destination IP
	- `dst-mac` - Destination MAC address
	- `dst-port` - Destination TCP/UDP port
	- `src-ip` - Source IP address
	- `src-mac` - Source MAC address
	- `src-port` - Source TCP/UDP port
	- ...
- data s identifikátorem -> jedno fyzické rozhraní