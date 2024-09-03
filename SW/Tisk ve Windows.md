- Fyzická tiskárna #tiskove_zarizeni
- Logická tiskárna #tiskárna
	- softwarové rozhraní mezi fyzickou tiskárnou a aplikacemi
	- vyžaduje #driver
		- klient i server

## Přidání tiskárny
- číslo portu, kam je tiskárna připojena (COM)
	- síťová tiskárna -> TCP/IP 9100 

## Oprávnění tiskárny
- tisknout
- spravovat tiskárnu
- spravovat tiskovou frontu (tiskové úlohy)
	- #Printer_Spooler -> služba zařazování tisku
	- `C:\Windows\System32\Spool\Printers`

### Fondy tiskáren
#Printer_Pools
- skupina tiskáren na jednom tiskovém serveru
	- tváří se jako jedna logická tiskárna
	- rozložení tiskových prací