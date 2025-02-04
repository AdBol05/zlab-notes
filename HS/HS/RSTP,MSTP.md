### Rapid STP
#RSTP
- 802.1w
- Rychlejší konvergence
- Plně kompatibilní s 802.1D ([[STP]])

#PortFast  `spanning-tree portfast`
- pouze na #Access portech
- port se **okamžitě** přepne do #Forwardning stavu
- na #Trunk může způsobovat problémy (smyčky)

#BPDU_Guard `spanning-tree portfast trunk`
- na #PortFast by neměl přijít #BPDU rámec
	- #Access je připojený na jiný switch 
	- port se přepne do #errdisabled stavu
	- špatné zapojení nebo zneužití [[STP]] pro útok
	- je třeba ručně zapnout
#### Port type navíc
#Backup_port
- podobné #Alternate_port 
- záložní port pro sdílené medium
- záložní port pro cestu do stejného prvku


#### 802.1D-2004
- Aktualizace původního 802.1D-1998
- Přejetí 802.1w => původní [[STP]] obsolete

### MSTP
- Multiple [[STP]]
- Inspirace Cisco MISTP
- Mapuje více VLAN do jedné STP instance
