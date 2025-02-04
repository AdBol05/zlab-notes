## ACID
- #Atomicity -> transakce - nedělitelná jednotka
- #Consistency -> zachování konzistence databáze
- #Isolation -> transakce nesmí ovlivňovat navzájem své výsledky
- #Durability -> změny po potvrzení trvalé

### Dirty reads
- nepotvrzené údaje jsou viditelné

### Non-repeatable reads
- opakované vykonávání dotazu může vrátit rozdílné údaje záznamů

### Phantom reads
- opakované vykonávání dotazu může vrátit rozdílné sady záznamů 

![[Pasted image 20240610125501.png]]


# Transakce v Oracle
- Více verzí v danou chvíli
- Nepodporuje režim *Read Uncommitted*
- Explicitní řízení transakcí