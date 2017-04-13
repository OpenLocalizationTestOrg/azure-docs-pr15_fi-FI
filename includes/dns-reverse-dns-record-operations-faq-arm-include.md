<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Usein kysytyt kysymykset - käänteinen DNS Azure määritetty IP-osoite

### <a name="how-much-do-reverse-dns-records-cost"></a>Kuinka paljon käänteinen DNS-tietueiden kustannukset?
Ne ovat maksuttomia!  Ei käänteinen DNS-tietueet ja kyselyjen ilman lisäkustannuksia.

### <a name="will-the-reverse-dns-records-for-my-azure-assigned-public-ip-address-resolve-from-the-internet"></a>Ratkaisee oma Azuren määritetty julkiseen IP-osoite käänteinen DNS-tietueet Internetistä?
Kyllä. Kun määrität käänteinen DNS-ominaisuuden julkiseen IP-osoite, Azure hallitsee DNS-delegoinnit ja DNS-vyöhykkeet tarvitse varmistetaan, että käänteinen DNS-tietueen korjaa kaikille internet-käyttäjille.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-public-ip-addresses"></a>Luodaan oletusarvon käänteinen DNS-tietueen Omat julkisten IP-osoitteiden?
Ei. Käänteinen DNS on suostumuksen antamista-ominaisuus. Ei ole oletusarvoisesti käänteinen DNS-tietueet luodaan, jos et halua määrittää niitä.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Mikä on täydellinen toimialuenimi (FQDN)-tiedosto?
Täydelliset toimialuenimet on määritetty eteenpäin järjestyksessä ja on lopetettava piste (esimerkiksi "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Mitä tapahtuu, jos voin olet määrittänyt käänteinen DNS vahvistus etsii epäonnistuu?
Jossa käänteinen DNS-tietojen tarkistus tarkistaa epäonnistumisen-palvelun hallinta-toiminto epäonnistuu. Korjaa käänteinen DNS-arvoa tarpeen mukaan ja yritä uudelleen.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Käänteinen DNS hallinta Azure sivuston
Käänteinen DNS ei tue Azure sivustot. Käänteinen DNS tuetaan Azuren näennäiskoneiden.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-public-ip-address"></a>Useita käänteinen DNS-tietueiden määrittäminen julkisen IP-osoite
Ei. Azure tukee yksittäisen käänteinen DNS-tietue kullekin julkiseen IP-osoite. Jokaisen julkiseen IP-osoitteen kuitenkin voi olla oman käänteinen DNS-tietue.

### <a name="can-i-configure-reverse-dns-records-for-an-ipv6-public-ip-address"></a>Käänteinen DNS-tietueiden määrittäminen IPv6 julkiseen IP-osoite
Ei.  Tällä hetkellä käänteinen DNS-tietueiden tuetaan IPv4 julkisten IP-osoitteiden vain.

### <a name="can-i-configure-a-reverse-dns-record-for-my-public-ip-address-without-having-a-domainnamelabel-specified"></a>Voin määrittää käänteinen DNS-tietueen julkisen IP-osoite ilman, että DomainNameLabel, joka on määritetty?
Ei. Hyödyntää julkiseen IP-osoitteiden käänteinen DNS-tietueita, sinun on määritettävä DomainNameLabel-ominaisuus.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Voit lähettää sähköpostiviestejä ulkoisista toimialueista Azure Laske oma Servicesistä?
Ei. [Azure Laske palvelut eivät tue lähettämisen sähköpostit lähetetään ulkoisista toimialueista](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
