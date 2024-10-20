#proxy - prostředník mezi klientem a serverem
#cache - součást web serveru, uchovává poslední data pro případ opětovných dotazů
## Apache - [[Unix-like systémy]]
- `/etc/apache2/apache.conf` nebo `/etc/httpd/conf/httpd.conf`
- služba `apache` nebo `httpd`
- podpora pro více webů
- podpora #SSL pomocí certifikátů
	- genorované příkazem `openssl` - balíček `mod_ssl`
- #Direktiva - atribut konfigurace
- #Direktiva #WebRoot - adresář pro uložení webových stránek
	- `DocumentRoot "/var/www/webroot"`
- #Direktiva #Listen - definice portů či adres, na kterých server 