# VPN
- #VPN - **Virtual Private Network**
- Zabezpečená komunikace v Internetu
- (Šifrovaný) privátní tunel napříč veřejnou sítí
	- #IPsec nebo #SSL pro zabezpečení provozu
	- Authentizace připojovaného uživatele
- Bezpečnost, Rozšiřitelnost (závisí na internetové přípojce), Úspory, Kompatibilita

- #Site-to-site - vzájemné propojení sítí (pobočka k centrále), na hraničních routerech
	- #VPN_gateway - šifrovaná brána na obou hraničních routerech -> transparentní pro uživatele (bez authentizace)
- #Remote-access - připojení jednotlvých uživatelů k firemní síti přes internet, klientské aplikace
	- #Clientless - zabezpečené pomocí #SSL připojení webového prohlížeče (integrovaná)
	- #Client-based - využívá klientský software

- #Enterprise-Managed (**Podnikové**) - firma si sama vytváří a spravuje #VPN  
- #Service_Provider-Managed - #ISP  vytváří a spravuje #VPN na #Linková nebo #Síťová  vrstvě -> využití #MPLS 

### GRE tunnel
- #GRE - **Generic Routing Encapsulation**
- ==nešifrovaný== -> lze zapouzdřit do #IPsec pro zabezpečení
- #Site-to-site s podporou #Multicast a broadcast
```
RA(config)# interface tunnel 0
RA(config-if)# ip add 10.10.10.1 255.255.255.252
RA(config-if)# tunnel source s0/0/0
RA(config-if)# tunnel destination 209.165.122.2
RA(config-if)# tunnel mode gre ip
RA(config-if)# no shut

RB(config)# interface tunnel 0
RB(config-if)# ip add 10.10.10.2 255.255.255.252
RB(config-if)# tunnel source s0/0/0
RB(config-if)# tunnel destination 64.103.211.2
RB(config-if)# tunnel mode gre ip
RB(config-if)# no shut

RA(config)# ip route 192.168.2.0 255.255.255.0 10.10.10.2
```
