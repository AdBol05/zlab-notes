- Automatický [[Routing]], tvorba [[Routovací tabulka]]
- Nástupce #RIP
	- nebere v potaz rychlost linky
	- pouze počet routerů
	- automatické přeposílání routovací tabulky dalším routerům
	- pomalé
- #Link_state
	- duplex
	- speed
- #Neighbor_table
- #LSDB
- #Forwardning_table

1. Posílá #Hello_messagess
2. Přidá si sousedící routery podle odpovědi
3. Předává informace o linkách -> ukládá do #LSDB
4. Dijkstrův algoritmus pro hledání nejlepší cesty
5. Vytvoří  [[Routovací tabulka]]
6. Při změně posílá #LSU - *Link State Update*
7. Přepočítání