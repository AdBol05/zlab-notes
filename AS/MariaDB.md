- #### Relační databáze
	- #### Edgar Frank Codd (1923 - 2003)
		- `Relational Model for Database Management`

#DBMS
Database Management System
- MariaDB
- Access
- OracleDB

#živatelské_rozhraní
Přístup do databáze a vizualizace
- PHP MyAdmin
	- `https://lab.uzlabina.cz/pma/index.php`
	- `ASWstudent`
	- `ASWdb`
- Access
- SQL developer
- MySQL Workbench

#MySQL_Workbench
- Method: `Standard TCP/IP`
- Hostname: `lab.uzlabina.cz`
- Port: `3306`
- Password: `PHPmyAdmin Password`
- **Tvorba ER schémat**
	- ==Návrh se musí přejmenovat na username !!==
	- PK -> primary key
	- NN -> not null
	- AI -> auto increment

# Vztahy (relace)
## Kardinalita
- **1:1**
	- většinou může být sloučena do jedné tabulky
- **1:N**
	- nejčastější
- **M:N**
	- nelze vytvořit, musí se obcházet spojovací tabulkou
	- nedoporučuje se (problémová)

![[Pasted image 20231002122739.png]]
