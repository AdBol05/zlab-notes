![[RAID 2023-11-02 09.18.06.excalidraw]]
# RAID 0
- prokládání
- dva a více disků
- vysoká rychlost a kapacita
- žádná redundance


# RAID 1
- zrcadlení
- rychlejší čtení
- dva a více disků
- odolnost


# Hybridní RAID (RAID 10)
- RAID 1+0
	- zrcadlená sada, která se prokládá
- RAID 0+1
	- prokládaná sada, která se zrcadlí


# RAID 5
- minimálně tři disky
- jeden disk pro ukládání #parity
- při selhání se ztracená data dopočítají z #parity



#parity 
- kontrolní součet
- prokládá se na všechny disky v RAID array
- dopočítání poškozených dat

#hot_spare
- záložní zapojený disk
- nepoužívaný dokud nedojde k selhání jiného disku

#dynamicky_disk
- možnost změny velikosti a formátování bez nutnosti restartu
