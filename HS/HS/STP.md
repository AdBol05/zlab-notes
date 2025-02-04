 ### Spanning tree protocol
- 802.1D
- Zabraňuje tvorbě smyčkám a broadcast storms
- #Linková vrstva
- #BPDU rámce **Bridge Protocol Data Unit**
	- obsahuje data pro všechny potřebná rozhodnutí
		- #Root_bridge ID
		- Root Path Cost
		- Sender Bridge ID
		- Port ID (port na odesílajícím switchi)
		- Message Age
		- Max Age
		- Hello Time
		- Forward Delay

		- BID -> Bridge ID
			- Bridge priority 
				- 0 - 61440 (výchozí 32768)
				- 4b
			- Extended system ID
				- 1 - 4095
				- Identifikace [[VLAN]]
				- 12b
			- MAC address
				- 48b

#STA
- **spanning tree algorithm**
1. STA scenario topology
2. Select the Root Bridge
3. Block redundant paths
4. Loop-Free Topology
5. Link Failure Causes Recalculation

![[STP 2023-12-18 11.00.11.excalidraw]]

### Root path cost
| Link Speed | STP Cost: IEEE 802.1D-1998 | RSTP Cost: IEEE 802.1w-2004 |
| --- | --- | --- |
| 10Gbps | 2 | 2,000 |
| 1Gbps | 4 | 20,000 |
| 100Mbps | 19 | 200,000 |
| 10Mbps | 100 | 2,000,000|

![[STP 2024-01-08 10.57.20.excalidraw]]


![[kdo-se-ptal-kdo.gif]]

### STP timer
- #Hello_timer
	- interval mezi #BPDU rámci
	- 1 - 10 sekund (2 default)
- #Forward_delay_timer
	- mezi #Listening a #Learning stavem
	- 4 - 30 sekund (15 default)
- #Max_age_timer
	- maximální doba před pokusem změny topologie
	- 6 - 40 sekund (20 default)

### STP port state
- #Blocking - jen příjem #BPDU rámců, po vypršení
- #Listening - příjem #BPDU rámců, počítá cest k #Root_bridge 
- #Learning - #Listening + plnění MAC tabulky
- #Forwardning - plná funkce 
- #Disabled - fyzicky vypnutý

![[STP 2024-01-08 11.04.34.excalidraw]]

### Port types
- #Designated_port
	- naproti #Root_port 
	- #Access port
	- *nejlepší port pro příjem*
- #Root_port 
	- nejlepší cesta k #Root_bridge
	- nejnižší
		- #BID souseda
		- #Port_priority (128 default)
		- číslo portu souseda
	- *nejlepší port pro odesílání*
- #Alternate_port
	- zablokovaný, ale zapnutý
	- přijímá pouze #BPDU
	- může se aktivovat, když selže linka k #Root_bridge
	- *záložní cesta k* #Root_bridge