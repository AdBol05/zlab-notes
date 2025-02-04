## Multilayer switch
- L3 funkce na switchi -> místo routeru ( [[Routing]] )
- #SVI pro každou [[VLAN]] (default gateway)
- L3 funkce na úrovni hardwaru
- `ip routing`
### Typy rozhraní
- #SVI 
	- lze nastavit IP adresu
- **Switched ports** 
	- nelze nastavit IP adresa
	- Mohou využívat 802.1Q
	- "rozhraní na switchi"
- **Router ports**
	- lze nastavit IP adresa
	- nelze nastavit access
	- "rozhraní na routeru"
	- neimplementuje 802.1Q
	- `no switchport`

