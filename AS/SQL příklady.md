PK - primary key (unikátní pro každý záznam -> identifikace)
FK - foreign key (pro relace)
FTI - Full text index (pro vyhledávání)

# Správa databáze
```sql
CREATE TABLE IF NOT EXISTS <tablename> (
	<column> <datatype>,
	...
)
```

#### Počet ulic s názvem začínající písmenem "P"
```sql
SELECT COUNT (ulice_kod) AS pocet FROM ulice WHERE nazev LIKE "P%";
```

**Seznam okresů ve Středočeském kraji (Seřazeno podle abecedy, třetí záznam)**
```sql
SELECT * FROM okres WHERE kraj_kod == 27;

SELECT okres.nazev FROM okres
INNER JOIN kraj ON okres.kraj_kod = kraj.kraj.kod
WHERE kraj.nazev LIKE "Středočeský"
ORDER BY okres.nazev ASC;

SELECT okres.nazev FROM okres
INNER JOIN kraj ON okres.kraj_kod = kraj.kraj.kod
WHERE kraj.nazev LIKE "Středočeský"
ORDER BY okres.nazev ASC
LIMIT 1 OFFSET 2;
```

**Existuje-li ulice Jiráskova v Boskovicích**
```sql
SELECT * FROM ulice
JOIN obec ON ulice.obec_kod = obec.obec_kod
WHERE obec.nazev LIKE "Boskovice" AND ulice.nazev LIKE "Jiráskova";
```

**Najít všechna prostranství pojmenována po Jiráskovi (v Praze)**
```sql
SELECT DISTINCT nazev FROM ulice WHERE nazev LIKE "%Jirásk%";

SELECT ulice.nazev FROM ulice
JOIN obec ON ulice.obec_kod = obec.obec_kod
WHERE ulice.nazev LIKE "%Jirask%" AND obec.nazev LIKE "Praha%"
ORDER BY ulice.nazev ASC;
```

**Počet ulic v Praze (Ve Středočeském kraji)**
```sql
SELECT COUNT(ulice.nazev) FROM ulice
INNER JOIN obec ON ulice.obec_kod = obec.obec_kod
WHERE obec.nazev LIKE "Praha%"

SELECT COUNT(ulice.nazev) FROM ulice
JOIN obec on ulice.obec_kod = obec.obec_kod
JOIN okres on obec.okres_kod = okres.okres_kod
JOIN kraj on okres.kraj_kod = kraj.kraj_kod
WHERE kraj.nazev LIKE 'Středočeský'
```

**Seznam zaměstnanců s názvem oddělení, ve kterém pracují, seřazený vzestupně**
```SQL
SELECT E.FIRST_NAME, E.LAST_NAME, D.DEPARTMENT_NAME FROM HR.EMPLOYEES E  
LEFT JOIN HR.DEPARTMENTS D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID  
ORDER BY LAST_NAME ASC, FIRST_NAME ASC;
```

**Počet zaměstnanců pracujících v jednotlivých odděleních**
```SQL
SELECT D.DEPARTMENT_NAME, COUNT(E.EMPLOYEE_ID) AS "count_emp" FROM HR.DEPARTMENTS D   
LEFT JOIN HR.EMPLOYEES E ON D.DEPARTMENT_ID = E.DEPARTMENT_ID  
GROUP BY D.DEPARTMENT_NAME  
ORDER BY "count_emp" DESC;
```

**Seznam manažerů se jmény podřízených, seřazený vzestupně**
```sql
SELECT M.FIRST_NAME AS "MANAGER_FIRST_NAME", M.LAST_NAME AS "MANAGER_LAST_NAME", E.EMPLOYEE_ID, E.MANAGER_ID, E.FIRST_NAME, E.LAST_NAME FROM HR.EMPLOYEES E
INNER JOIN HR.EMPLOYEES M ON E.MANAGER_ID = M.EMPLOYEE_ID
ORDER BY M.LAST_NAME ASC;

```

**Seznam pracovních pozic s průměrnou mzdou seřazeno sestupně**
```sql
SELECT J.JOB_TITLE, AVG(E.SALARY) AS "AVG_SALARY" FROM HR.EMPLOYEES E 
INNER JOIN HR.JOBS J ON E.JOB_ID = J.JOB_ID
GROUP BY J.JOB_TITLE
ORDER BY "AVG_SALARY" DESC;
```

**Seznam tabulek, které mají sloupec s datovým typem DATE**
```sql
SELECT DISTINCT TABLE_NAME FROM ALL_TAB_COLS WHERE OWNER = 'HR' AND DATA_TYPE = 'DATE' ORDER BY DATA_TYPE;
```

**Seznam primárních klíčů**
```sql
SELECT ACC.COLUMN_NAME FROM ALL_CONSTRAINTS AC, ALL_CONS_COLUMNS ACC WHERE AC.OWNER = 'HR' AND AC.CONSTRAINT_TYPE = 'P' AND AC.CONSTRAINT_NAME = ACC.CONSTRAINT_NAME;

SELECT ACC.COLUMN_NAME FROM ALL_CONSTRAINTS AC INNER JOIN ALL_CONS_COLUMNS ACC ON AC.OWNER = 'HR' AND AC.CONSTRAINT_TYPE = 'P' AND AC.CONSTRAINT_NAME = ACC.CONSTRAINT_NAME;

```

**Seznam procesů zabírající více než 5Mib paměti**
```sql
SELECT * FROM V$PROCESS WHERE PGA_ALLOC_MEM > 5 * 1024 * 1024 ORDER BY pga_alloc_mem DESC;
```

**Seznam aktivních uživatelů**
```sql
SELECT * FROM V$SESSION WHERE TYPE = 'USER' AND STATUS = 'ACTIVE' ORDER BY STATUS DESC;
```

**Seznam nezálohovaných tablespaces**
```sql
SELECT * FROM V$TABLESPACE WHERE INCLUDED_IN_DATABASE_BACKUP = 'NO';
```