### Domain name -> IP address
**Domain name system**
- Kořen (root) `google`
- TLD `.com .cz`

- Služba [[Windows server]]

![[DNS 2023-12-08 08.58.31.excalidraw]]

## Zóny, typy záznamů
- dopředná -> A, CNAME
	- A - host
	- NS - Name server
	- SOA - Start Of Authority
- zpětná -> PTR

`ServerManager > Manage > Add Roles and Features`
`ServerManager > Tools > DNS > WS2022 > New Zone...`
![[MicrosoftTeams-image 1.png]]



Přenos záznamů mezi servery
**TCP/UDP port 53**
- primární & sekundární
	-> redundance

`HOSTS`
- textový soubor
- názvy uzlů v síti (IP adresy)
- spojení s DNS

#WINS
- Windows Internet Service
- zastaralá
- lokální síť
- není hierarchický
- soubor `LMHOSTS`  (podobný `HOSTS`)


### Suffix
- přípona pro DNS zónu
![[MicrosoftTeams-image.png]]