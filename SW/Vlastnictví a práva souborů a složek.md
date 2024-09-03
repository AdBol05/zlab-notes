## Typy oprávnění #NTFS
- #explicitni_opravneni 
	-  přiděleno přímo složce nebo souboru
- #zdedena_opravneni 
	- přiděleno nadřazené složce (rodičovský objekt)
	- přenáší se na objekty v nadřazené složce (potomky)
- #efektivni_opravneni 
	- výsledná kombinace  (explicitní + zděděná)
	- povolení nebo odepření přístupu
	- odepření > povolení

## Šifrování
#encryption
- zašifrovaný soubor nečitelný
- dešifrováním se převede do čitelného formátu
- šifrování pomocí klíčů (certifikáty - podepsaný veřejný klíč)
#EFS
- **Encrypting File System**
- Ukládání šifrovaných souborů na #NTFS svazcích
- pro přístup je třeba šifrovací klíč
- není třeba pro čtení ručně dešifrovat

## Kopírování a přesun souborů
- soubor/složka získává oprávnění disku/složky, kam byl zkopírován
	- v rámci jednoho svazku
		- kopie si ponechává stejná oprávnění
	- do jiného svazku
		- získává oprávnění disku, kam byl zkopírována

## Vlastnictví souboru a složky
- Vlastník nastavuje oprávnění objektu
- Oprávnění pro převzetí vlastnictví mají automaticky všichni správci