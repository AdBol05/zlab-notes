![[Drawing 2023-09-14 10.56.37.excalidraw]]

1. Nejrychlejší
	- #Cut-throught
	- Stačí cílová MAC
	- prvních 14B (preamble + zdroj + cíl)

2. Mezi
	- #Fragment-free
	- Minimální délka (64B)

3. Nejspolehlivější
	- #Store-and-forward
	- Čeká na checkum na konci rámce
	- Celý rámec

![[Switching 2023-09-18 11.01.40.excalidraw]]

## MAC tabulka
| MAC adresa | číslo portu | číslo VLAN | Timestamp |
|-----|-----|-----|-----|
- dynamický vyprší za 5 minut
- statický (nastavený správcem) nevyprší

#Broadcast_Domény
- "LANs"
- meze dosahu broadcastů

#Kolizní_Domény
- Half-Duplex
- Hrozí kolize
- FastEthernet, Wireless

#ingress
- incoming port

#egress
- outgoing port

#SVI 
- Switch Virtual Interface
- logické (virtuální, simulované) rozhraní switche -> management [[VLAN]]
- Může mít IP parametry