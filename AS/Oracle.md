- login: `C##BOLEMAD`
- hostname: `lab.uzlabina.cz` / `10.1.130.51`
- port: `10176` / `1521`
- service name: `XEPDB1`
- proxy user
	- Proxy client: `US_BOLEMAD`
	- Password: none

## REDO log
- aktuálně prováděná databázová operace
- circular buffer
- při potvrzení transakce musí být bezpodmínečně zapsány do souborů na diskovém úložišti
- při havárii serveru se užívá pro opětovné vykonání zaznamenaných datových operací
## UNDO log
- historická podoba dat (ORACLE Flashback)
- při odvolání transakce použity pro obnovu původní podoby datových záznamů
- ukládán do datových segmentů v UNDO tablespace