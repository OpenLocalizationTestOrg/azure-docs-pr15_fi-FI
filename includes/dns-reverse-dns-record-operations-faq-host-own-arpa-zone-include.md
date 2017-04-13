<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>Usein kysytyt kysymykset - ARPA vyöhykkeen Azure DNS-isännöintipalvelu

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>ARPA vyöhykkeet isännöi Azure DNS-Palveluntarjoajan määritettyjä IP Omat tekstilohkojen
Kyllä. ARPA vyöhykkeet oman IP-alueita Azure DNS-isännöintipalvelu on täysin tuettu.

Vain [luominen Azure DNS-vyöhyke](dns-getstarted-create-dnszone.md)ja valitse Palveluntarjoaja määritettävän [vyöhykkeen](dns-domain-delegation.md)käsitteleminen.  Voit hallita kunkin käänteisen PTR-tietueet sitten samalla tavalla kuin muita tietuetyyppejä.

Voit myös [tuoda aiemmin käänteisen-alueen avulla Azure-CLI](dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>Kuinka paljon isännöinnin Omat ARPA vyöhykkeen kustannukset?
Isännöinnin ARPA vyöhykkeen, Internet-Palveluntarjoajan määrittämä IP estä Azure DNS laskutetaan [Azure DNS normaalin korvauksen](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>IPv4- ja IPv6-osoitteiden Azure DNS-vyöhykkeet ARPA isännöidä
Kyllä.