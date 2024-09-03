#FHRP
- **First Hop Redundancy Protocol**
- Více fyzických routerů jako jeden virtuální
- Jedna IP adresa, jedna MAC adresa -> redundance (dynamická)
- Konvergentní

![[FHRP 2024-02-26 11.09.52.excalidraw]]


#Forwardning_Router
- Hlavní router
- Posílá #Hello_messages

#Standby_Router
- Záložní router
- Pokud nedostává #Hello_messages přebere IP a MAC adresu hlavního routeru

## Implementace
#HSRP
- **Hot Standby Router Protocol**
- Cisco proprietární implementace
- #HSRP_for_IPv6
- Priorita 0 - 255 (100 výchozí)
	- Vyšší priorita = důležitější
	- Election -> výběr #Forwardning_Router
- #HSRP_preemption
	- Vypadne hlavní, nasadí se záložní
	- Záložní zůstává aktivní i po opětovném startu aktivního -> přepnout se musí ručně
- #HSRP_states
	- #Initial - původní, při změně konfigurace
	- #Learn - Router ještě neobdržel #Hello_messages a neví IP virtuálního routeru
	- #Listen - Router zná IP virtuálního routeru
	- #Speak - Posílá #Hello_messages účastí se election
	- #Standby - neaktivní, záloha, posílá #Hello_messages 

#VRRP_v2
- **Virtual Router Redundancy Protocol v2**
- Otevřený standard

#VRRP_v2 
- **Virtual Router Redundancy Protocol v3**
- podpora [[IPv6]]

#IRDP
- **ICMP Router Discovery Protocol**
- Původní #FHRP