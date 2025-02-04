- Virtual #LAN
- Více sítí na jednom switchi
- Zmenšuje #Broadcast_Domény 

```bash
# vytvoření VLAN
vlan 5
name Students
end

# přiřazení portu VLAN
interface f0/5
switchport mode access
switchport access vlan 5

# VOICE VLAN
vlan 150
name VOICE
exit
interface fa0/5
mls qos trust cos
switchport vioce vlan 150 <dot1p, none, untagged>

# TRUNK
interface fa0/5
switchport mode trunk
switchport trunk allowed vlan 10, 20
switchport trunk native vlan 999
switchport nonegotiate

do show vlan brief
int f0/1 switchport
show interfaces trunk
```

- Porty
	- #Trunk
		- více VLAN
		- VLAN tag v rámci -> přepočet CRC checksum
		- propojení switchů
	- #Access
		- Jen jedna datová VLAN + voice VLAN
	- #Dynamic_auto
		- Podřizuje se protějšku
		- Default
	- #Dynamic_desirable
		- Protějšek bude trunk

|                  | ACCESS | TRUNK | DYNAMIC AUTO | DYNAMIC DESIRABLE |
| ---------------- | ------ | ----- | ------------ | ----------------- |
| ACCESS           | A      | /     | A            | A                 |
| TRUNK            | /      | T     | T            | T                 |
| DYNAMIC AUTO     | A      | T     | A            | T                 |
| DYNAMIC DESIRABL | A      | T     | T            | T                 |
![[Drawing 2023-10-23 10.59.42.excalidraw]]

- Druhy VLAN
	- Default = 1 
		- nelze smazat
		- nelze změnit jméno
	- Data
		- nejběžnější
	- Native
		- pokud na #Trunk přijde rámec bez VLAN tagu
	- Management
		- pro správu
		- nemusí existovat -> vytvořená manuálně
	- Voice
		- VoIP
		- QoS
		- nízká latence
		- tagovaná

- Rozsah VLAN
	- `1 - 4095`
	- normal -> `1 - 1005`
		- `vlan.dat` - flash
		- `1` default
		- `1002 - 1005` rezervováno pro FDDI Token Ring
		- `1, 1002 - 1005` nelze smazat
	- extended -> `1006 - 4095`
		- `running-config` -> `startup-config` - NVRAM

# DTP
#DTP
- ==Dynamic Trunking Protocol==
- proprietární cisco protokol
- 2960 a 3650 switche
- automaticky nastaví trunk i na druhém zařízení
- **nenastavuje** číslo VLAN
- defaultně #Dynamic_auto 


![[Drawing 2023-10-02 10.58.03.excalidraw]]