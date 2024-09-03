#LDAP
- **Lightweight Directory Access Protocol**
- Přístup k informacím v adresáři
- vyhledávání, správa, uspořádání (svazky, složky, soubory, tiskárny atd.)
- Využívá adresářová služba Active Directory

#Active_Directory
- řadič domény
- využívá #LDAP 
- ověřování #Kerberos a single sign-on authentication
- centrum pro správu sítě a delegování

#RSAT 
- Remote Server Administration Tools

#NSS 
 - enumerace informací o službách a uživatelích
## Logická struktura
- *domény, stromy, lesy*
- vztahy důvěryhodnosti
	- přístupy v jiné doméně

## Fyzická struktura
- *místa (sites)*
	- jedna nebo několik podsítí, navzájem propojeny (zeměpisné umístění)
- *řadiče domény*
	- server ([[Windows server]])
	- údaje o uživatelských účtech a zabezpečení
	- definuje hranice domény

## Nástroje pro správu Active Directory
- Uživatelé a počítače AD (Active Directory Users and computers)
- Domény a vztahy důvěryhodnosti AD (Active Directory Domains and Trusts)
- Místa a služby AD (Active Directory Sites and Services)
- Centrum pro správu AD (Active Directory Administrative Center)
- Konzola pro správu zásad skupiny (Group policy Management Console -> #GPMC)

#Member_Server
- není řadičem domény
- program `dcpromo`

#FSMO
- **Flexible Single Master Operations**
- #Active_Directory používá tzv. replikaci multimaster -> neexistuje hlavní řadič domény

#Delegovani_rizeni
- Přiřazení různých úkolů správy určitým uživatelům/skupinám

## Objekty Active Directory
- sady atributů
	- reprezentují konkrétní prvky v síti
	- jednoznačné a pojmenované
	- *počítače, uživatelé, skupiny, tiskárny*
	- 128bitové  
		- #GUID -> **Globally Unique IDentifier** 
		- #SID -> **Security IDentifier**

## Uživatelské účty a profily
#User_account
- přihlášení do počítače a do domény -> ověření identity a přístupu
	- audit
	- místní a doménové uživatelské účty

#User_profile
- soubor složek a dat
	- plocha
	- nastavení aplikací `NTUSER.DAT` - `regedit`
	- síťová připojení (sdílené složky)
    -> cestovní profil

#Computer_account
-  účty počítačů
	- každý počítač má jedinečný účet
	- audit

#Group
- množina uživatelských účtů
	- stejná práva (práva skupiny)
	- zjednodušená správa (přidělování práv a oprávnění)
	- neukládá žádné údaje týkající se uživatele či počítače -> pouze vypisuje
	- *skupina se zabezpečením* a *distribuční skupina*
	- *místní, globální, univerzální* (doména, strom, les) - rozsah působnosti
	- #AGDLP - **Accounts Global Domain Local Permissions**
		- efektivní používání skupin na přiřazování oprávnění
		- univerzální skupina -> #AGUDLP
	- skupiny integrované v operačním systému (přednastavená práva a oprávnění)
		- Domain Admins
		- Domain Users
		- Account Operators
		- Backup Operators
		- Authenticated Users
		- Everyone

```sh
# local user
net user <name> <password> /add

# domain user
net user <name> <password> /add /domain

# local group domain
dsadd group CN="<name>",CN=Users,DC=Firma,DC=com -scope 1
net localgroup "<name>" /add /domain

# global group
dsadd group CN="<name>",CN=Users,DC=Firma,DC=com -scope g
```

### Přístupy
`A -> G -> DL <- P`
`account -> global group -> Domain local <- priviliges`

### Zásady skupiny
- `gpedit.msc`
- konfigurace prostředí účtů (uživatelů a počítačů)
- centralizovaná správa a konfigurace
- např. *zásady hesla* (požadavky)
- použití
	- místní
	- na úrovni místa
	- na úrovni domény
	- na úrovni organizační jednotky


# LDAP na linuxu
[[Unix-like systémy]]
## Server
### Instalace
```sh
yum install openldap openldap-servers
sudo apt install slapd ldpa-utils

sudo systemctl start slapd
sudo systemctl enable slapd
sudo systemctl status slapd

firewall-cmd --add-services=ldap
sudo ufw allow ldap
```

### Konfigurace serveru
```sh
slappasswd #vytvoření účtu správce OpenLDAP
sudo vim ldaprootpasswd.ldif #soubor pro přidávání záznamů do LDAP
```

`ldaprootpasswd.ldif`
```sh
dn: olcDatabase={0}config,cn=config #název databáze a globální konfigurace
changetype: modify
add: olcRootPW
olcRootPW: {SSHA}PASSWORD_CREATED #hash z vytvoření účtu (slappasswd)
```

```sh
#přidání zázznamu LDAP pomocí URI serveru
sudo ldapadd -Y EXTERNAL -H ldapi:// -f ldaprootpasswd.ldif
```

### Konfigurace databáze
```sh
#nakopírování soubor slapd
sudo cp /usr/share/openldap-servers/DB_CONDFIG.example /var/lib/ldap/DB_CONFIG

#nastavení oprávnění souboru slapd
sudo chown -R ldap:ldap /var/lib/ldap/DB:CONFIG

#restart služby
sudo systemctl restart slapd

#import základních schémat
sudo ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
sudo ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
sudo ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif

#soubor konfigurace domény
sudo vim ldapdomain.ldif
```

`ldapdomain.ldif`
```sh
dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth"
read by dn.base="cn=Manager,dc=example,dc=com" read by * none
dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=example,dc=com
dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=Manager,dc=example,dc=com
dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcRootPW
olcRootPW: {SSHA}PASSWORD
dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcAccess
olcAccess: {0}to attrs=userPassword,shadowLastChange by
dn="cn=Manager,dc=example,dc=com" write by anonymous auth by self write by * none
olcAccess: {1}to dn.base="" by * read
olcAccess: {2}to * by dn="cn=Manager,dc=example,dc=com" write by * read
```

```sh
#přidání domény do databáze LDAP
sudo ldapmodify -Y EXTERNAL -x -D cn=Manager,dc=example,dc=com -W -f ldapdomain.ldif
```

```sh
#soubor konfigurace domény
sudo vim baseldapdomain.ldif
```

`baseldapdomain.ldif`
```sh
dn: dc=example,dc=com
objectClass: top
objectClass: dcObject
objectclass: organization
o: example com
dc: example
dn: cn=Manager,dc=example,dc=com
objectClass: organizationalRole
cn: Manager
description: Directory Manager
dn: ou=People,dc=example,dc=com
objectClass: organizationalUnit
ou: People
dn: ou=Group,dc=example,dc=com
objectClass: organizationalUnit
ou: Groupldif
```

```sh
#přidání domény do databáze LDAP
sudo ldapadd -Y EXTERNAL -x -D cn=Manager,dc=example,dc=com -W -f baseldapdomain.ldif
```

```sh
sudo useradd testuser #vytvoření testovacího uživatele
sudo passwd testuser # nastaveni hesla
```

`ldapgroup.ldif`
```sh
#skupina v LDAP
dn: cn=Manager,ou=Group,dc=example,dc=com
objectClass: top
objectClass: posixGroup
gidNumber: 1005 #group id z /etc/group
```

```sh
#přidání skupiny do adresáře OpenLDAP
sudo ldapadd -Y EXTERNAL -x -W -D "cn=Manager,dc=example,dc=com" -f ldapgroup.ldif
```

`ldapuser.ldif`
```sh
#definice pro uživatele testuser
dn: uid=tecmint,ou=People,dc=example,dc=com
objectClass: top
objectClass: account
objectClass: posixAccount
objectClass: shadowAccount
cn: tecmint
uid: testuser
uidNumber: 1005
gidNumber: 1005
homeDirectory: /home/testuser
userPassword: {SSHA}PASSWORD_HERE
loginShell: /bin/bash
gecos: tecmint
shadowLastChange: 0
shadowMax: 0
shadowWarning: 0
```

```sh
#načtení konfigurace do adresáře LDAP
sudo ldapadd -Y EXTERNAL -x -D cn=Manager,dc.example,dc=com -W -f ldapuser.ldif
```
## Klient
### Instalace
```sh
yum install openldap openldap-clients nss-pam-ldap
sudo apt install libnss-ldap libpam-ldap ldap-utils nscd

getent passwd testuser #otestování záznamů uživatele testuser ze souboru passwd
```
### Konfigurace
- většinu zajišťuje balíček `ldap-auth-config`
	- zadat identifikátor serveru #LDAP, báze vyhledávaní #LDAP, a verzi #LDAP
- `/etc/ldap.conf`
	- přihlašovací údaje uživatele root do #LDAP databáze
	- -> používá balíček `ldap-auth-login`
- vytvoření profilu #LDAP pro #NSS
	- `sudo auth-client-config -t nss -p lac_ldap`
- nastavení přihlášení pomocí #LDAP
	- `sudo pam-auth-udpate`
		- vybrat #LDAP a požadované ověřovací mechanismy

