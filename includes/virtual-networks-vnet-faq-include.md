## <a name="virtual-network-basics"></a>VPN-perusteet

### <a name="what-is-an-azure-virtual-network-vnet"></a>Mikä on Azure Virtual network (VNet)?

Voit käyttää VNets valmistelu ja hallita näennäinen yksityisverkko (VPN) Azure ja, voit myös linkittää VNets Azure muiden VNets tai oman paikallista IT-infrastruktuurin luominen hybrid tai paikallisen-ratkaisuja. Kunkin VNet, voit luoda on oma CIDR block ja voidaan linkittää VNets ja paikallisen verkoista, kunhan CIDR lohkot ei törmäävät. Käytössä on myös VNets DNS-palvelimen asetusten ohjausobjektit ja VNet Segmentointi aliverkosta kyselyjä.

Käytä VNets avulla:

- Luo oma vain pilvipalveluita näennäinen yksityisverkko

    Joskus ei vaadi ratkaisu paikallisen-määritys. Kun luot VNet, palveluiden ja VMs oman VNet sisällä voit keskenään suoraan ja suojatusti pilveen. Tämä säilyttää liikenne suojatusti VNet sisällä, mutta edelleen avulla voit määrittää päätepisteen yhteydet VMs ja palveluja, jotka edellyttävät Internet-yhteyden osana ratkaisu.

- Laajenna suojatusti data Centerissä

    VNets, jossa voit luoda perinteinen sivusto sivusto (S2S) VPN skaalata suojatusti palvelinkeskuksen kapasiteetin kautta. S2S VPN-yhteydet käyttää IP suojatun yhteyden corporate VPN-yhdyskäytävän ja Azure välillä.

- Ota käyttöön hybrid cloud skenaariot

    VNets antaa joustavuutta tukemaan hybrid cloud skenaarioita. Voit muodostaa suojatusti tyypin paikallista järjestelmän, kuten keskustietokoneita ja Unix-järjestelmiä pilvipohjainen sovellukset.

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>Mistä tiedän, jos tarvitsen virtual verkossa?

Käy [Näennäinen yleistä](../articles/virtual-network/virtual-networks-overview.md) ja tarkastella päätös taulukon, jonka avulla voit päättää paras verkon suunnittelu-vaihtoehto puolestasi.

### <a name="how-do-i-get-started"></a>Miten aloitetaan?

Käy aloittamaan [VPN-ohjeista](https://azure.microsoft.com/documentation/services/virtual-network/) . Tällä sivulla on linkkejä yleisiä määritysvaiheet sekä tiedot, jotka auttavat sinua ymmärtämään asioista, jotka sinun on otettava huomioon virtual verkon suunnitteluun.

### <a name="what-services-can-i-use-with-vnets"></a>Mitä palveluja voin käyttää VNets kanssa?

VNets voidaan käyttää erilaisia eri Azure palveluja, kuten pilvipalveluihin (PaaS), näennäiskoneiden ja verkkosovelluksissa. On kuitenkin joitakin palvelut, joita ei tueta VNet. Tarkista tietyn palvelun, jota haluat käyttää ja varmista, että se on yhteensopiva.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Voinko käyttää VNets ilman paikallisen-yhteyden?

Kyllä. Voit käyttää VNet käyttämättä sivusto yhteys. Tämä on erityisen hyödyllinen, jos haluat suorittaa toimialueohjaimia ja SharePoint-klustereihin Azure-tietokannassa.

## <a name="virtual-network-configuration"></a>Virtuaalinen verkon määrittäminen

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Millä työkaluilla käytän luomiseen VNet?

Voit luoda tai määrittää virtual verkon seuraavia työkaluja:

- Azure Portal (perinteinen varten ja Resurssienhallinta VNets).

- Verkon määritystiedoston (netcfg - perinteinen VNets varten). Artikkelissa [verkon määritysten tiedoston virtual verkon määrittäminen](../articles/virtual-network/virtual-networks-using-network-configuration-file.md).

- PowerShellin (perinteinen varten ja Resurssienhallinta VNets).

- Azure CLI (perinteinen varten ja Resurssienhallinta VNets).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Mitä osoitealueet Omat VNets voidaan käyttää?

Voit käyttää julkiseen IP-osoitealueet ja mikä tahansa IP-osoitealueita [RFC 1918](http://tools.ietf.org/html/rfc1918)määritelty.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Julkisten IP-osoitteiden on oma VNets

Kyllä. Saat lisätietoja julkiseen IP-osoitealueet [julkiseen IP-osoitetilaa Virtual verkon (VNet)](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md). Ota huomioon, että julkiseen IP-osoitteet eivät ole suoraan käytettävissä Internetistä.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>Onko rajoitusta aliverkosta virtual verkon määrään?

Ei ole rajoitettu aliverkosta sisällä VNet määrän. Kaikki aliverkosta täytyy sisältyä täysin VPN-osoitetilaa ja eivät saa olla päällekkäiset toisiinsa.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Mitä rajoituksia käyttämällä näitä aliverkosta IP-osoitteet?

Azure varaa joitakin kunkin aliverkon IP-osoitteet. Protokollan poikkeamisen sekä 3 useita osoitteita käytetään Azure palveluille on varattu aliverkosta etu- ja IP-osoitteet.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Miten pienten ja kuinka suuri voi olla VNets ja aliverkosta?

Sovellus tukee pienin aliverkon on /29 ja suurin on /8, (joko CIDR aliverkon määritelmät).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Voin tuoda Omat näennäislähiverkkojen Azure, käyttämällä VNets?

Ei. VNets ovat Layer 3 päällekkäiset. Azure ei tue minkä tahansa tason 2 semantiikkaan liittyvien.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Mukautettujen reititys käytäntöjen määrittää omat VNets ja aliverkosta

Kyllä. Voit käyttää käyttäjän määrittämät reititys (UDR). Lisätietoja UDR käy [käyttäjän määrittämät tiet, ja IP-osoitteen välityksen](../articles/virtual-network/virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>VNets tukee moni- tai?

Ei. Microsoft ei tue moni- tai.

### <a name="what-protocols-can-i-use-within-vnets"></a>Mitä protokollia sisällä VNets voi käyttää?

Voit käyttää VNets IP-pohjaisen protokollia vakio. Kuitenkin moni, lähetyksen, IP IP encapsulated pakettien ja Yleinen Routing Encapsulation (GRE)-paketit on estetty VNets kuluessa. Vakio protokollia, jotka toimivat ovat seuraavat:

- TCP
- UDP
- ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Oma oletusarvon reitittimen sisällä VNet ping

Ei.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Vianmääritys yhteyden tracert avulla?

Ei.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Voinko lisätä aliverkosta VNet luomisen jälkeen?

Kyllä. Aliverkosta voidaan lisätä VNets milloin tahansa, kunhan aliverkon osoite ei ole toisen aliverkon VNet-osa.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Voi muokata Omat aliverkon koon sen luomisen jälkeen?

Voit lisätä, poistaa, laajenna tai kutista aliverkon, jos ei ole VMs tai palvelut käyttöön sen sisältämät PowerShellin cmdlet-komennot tai NETCFG-tiedoston avulla. Voit myös lisätä, poistaa, laajenna tai Kutista kaikki etuliitteiden, kunhan muutos ei vaikuta aliverkosta, jotka sisältävät VMs tai palveluista.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Voi muokata aliverkosta, kun ne luotu?

Kyllä. Voit lisätä, poistaa ja muokata VNet käyttämä CIDR lohkot.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Voin muodostaa yhteyden Internetiin, jos käytössäni VNet palveluiden?

Kyllä. Kaikki palvelut, jotka on otettu käyttöön VNet sisällä voit muodostaa yhteys Internetiin. Jokaisen pilvipalvelussa käyttöön Azure-tietokannassa on määritetty julkisesti käytettävissä VIP. Sinun on määrittää syötteen päätepisteet PaaS roolien ja päätepisteet näennäiskoneiden käyttöön hyväksymään yhteyksiä internet-palveluun.

### <a name="do-vnets-support-ipv6"></a>VNets tue IPv6: ta

Ei. Et voi käyttää IPv6 VNets tällä hetkellä.

### <a name="can-a-vnet-span-regions"></a>VNet kattavat alueiden?

Ei. VNet on rajoitettu vain yhden alueen.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Voin muodostaa yhteyden toiseen VNet Azure-tietokannassa VNet?

Kyllä. Voit luoda VNet VNet viestintä REST API- tai Windows PowerShellin avulla. Voit myös muodostaa VNets VNet Peering kautta. Lisätietoja peering [tähän.](../articles/virtual-network/virtual-network-peering-overview.md)

## <a name="name-resolution-dns"></a>Nimenselvitys (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Mitä VNets DNS-asetukset ovat?

[Nimenselvitys VMs ja roolin esiintymät](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) sivun päätös-taulukon avulla voit opastaa kaikissa DNS käytettävissä olevat asetukset.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>DNS-palvelimet Määritä VNet varten

Kyllä. Voit määrittää DNS-palvelimien IP-osoitteiden VNet-asetukset. Tämä käytetään kuin oletusarvoinen DNS-palvelin-VNet kaikki VMs.

### <a name="how-many-dns-servers-can-i-specify"></a>Kuinka monta DNS-palvelimet voit määrittää?

Voit määrittää enintään 12 DNS-palvelimet.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Voi muokata DNS-palvelimet, kun verkko on luotu?

Kyllä. Voit muuttaa DNS-palvelinten luettelo oman VNet, milloin tahansa. Jos muutat DNS-palvelinten luettelo, sinun on käynnistämään kunkin VMs oman VNet, jotta ne päiväkodista uusi DNS-palvelimeen.


### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Mikä on antamalla Azure DNS ja se toimii VNets kanssa?

Azure toimitettuja DNS on Microsoftin tarjoama usean vuokraajan DNS-palvelun. Azure Rekisteröi tämä palvelu kaikki VMs ja roolin esiintymät. Tämä palvelu on nimenselvitys hostname VMs ja saman pilvipalvelussa sisältämät roolin esiintymät ja FQDN VMs ja saman VNet-roolin esiintymät.

> [AZURE.NOTE] Ei tällä hetkellä rajat-vuokraajan nimenselvitys antamalla Azure DNS: n avulla saat virtual verkon ensimmäisen 100 pilvipalveluihin rajoitus. Jos käytössäsi on DNS-palvelimeen, tämä rajoitus ei koske.

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Voit ohittaa DNS-asetukset, valitse kohden-AM / palvelun peruste?

Kyllä. Voit määrittää DNS-palvelimet-cloud palvelun välein korvaavat verkon oletusasetukset. Suosittelemme kuitenkin, että käytät koko verkon DNS mahdollisimman hyvin.

### <a name="can-i-bring-my-own-dns-suffix"></a>Voit tuoda oman DNS-liite?

Ei. Et voi määrittää mukautettuja DNS-liite, että VNets.

## <a name="vnets-and-vms"></a>VNets ja VMs

### <a name="can-i-deploy-vms-to-a-vnet"></a>Voin ottaa VMs VNet?

Kyllä.

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>Voin ottaa Linux VMs VNet?

Kyllä. Voit ottaa käyttöön minkä tahansa Linux Azure tukemat distro.

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>Mikä on julkinen VIP ja sisäinen IP-osoite välinen ero?

- Sisäinen IP-osoite on IP-osoite, joka on määritetty kunkin AM sisällä VNet DHCP. Se ei ole julkisten yhteydessä. Jos olet luonut VNet, sisäinen IP-osoite määritetään solualue, jonka olet määrittänyt oman VNet aliverkon asetuksia. Jos sinulla ei ole VNet, sisäinen IP-osoite määritetään silti. Sisäinen IP-osoite pysyy kanssa AM aikana, ellei, AM varaus on poistettu.

- Julkinen VIP on julkiseen IP-osoite, joka on määritetty cloud-palvelua tai kuormituksen tasauspalvelun. Se ei ole määritetty suoraan AM NIC-sivustosta. VIP pysyy pilvipalvelussa, se on määritetty ennen kuin kaikki VMs pilvipalvelussa Varaus poistettu tai poistettu. Tässä vaiheessa se on julkaistu.

### <a name="what-ip-address-will-my-vm-receive"></a>Mitä IP-osoite Omat AM saavat?

- **Sisäinen IP-osoite-** Jos otat käyttöön AM VNet, AM saa sisäinen IP-osoite sisäinen IP-osoitteet, jotka määrität resurssivarantoon. VMs viestiminen käyttämällä sisäisiä IP-osoitteita VNets kuluessa. Vaikka Azure määrittää dynaamisen sisäinen IP-osoite, voit pyytää, että AM staattinen osoite. Lisätietoja staattinen sisäisiä IP-osoitteita, käy [staattinen sisäinen IP määrittämisestä](../articles/virtual-network/virtual-networks-reserved-private-ip.md).

- **VIP-** Oman AM liittyy myös VIP, vaikka VIP koskaan määritetään AM suoraan. VIP on voi määrittää pilvipalvelussa sijaitsevaan julkiseen IP-osoitetta. Voit vaihtoehtoisesti varata VIP cloud-palveluun.

- **ILPIP-** Voit myös määrittää esiintymän tason julkiseen IP-osoite (ILPIP). ILPIPs eivät suoraan liittyvät AM pilvipalvelussa sijaitsevaan sijaan. Saat lisätietoja ILPIPs seuraavasta [Esiintymän tason julkiseen IP-yleiskatsaus](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Sisäinen IP-osoite varata AM, joka luo myöhemmin varten

Ei. Et voi varata sisäinen IP-osoite. Jos sisäinen IP-osoite on käytettävissä se määritetään AM tai rooli esiintymään luokaksi mukaan. Kyseisen AM voi tai ei ehkä ole se, jonka haluat liitetään sisäinen IP-osoite. Voit kuitenkin muuttaa aiemmin luotuja AM sisäinen IP-osoitteen kaikki käytettävissä olevat sisäinen IP-osoitteeseen.

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>Tee sisäinen IP osoitteet muutos VNet VMs?

Kyllä. Sisäisiä IP-osoitteita ovat kanssa AM aikana ellei AM varaus on poistettu. Kun varaus poistettu AM, sisäinen IP-osoite on julkaistu, paitsi että AM määritetyt staattista sisäinen IP-osoite. Jos AM yksinkertaisesti pysäytetty (ja sijoita **pysäytetty (Deallocated)**-tila) IP-osoite säilyy varattuina AM.

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>Voit manuaalisesti osoitettujen IP-osoitteiden VMs NIC?

Ei. Et saa muuttaa VMs käyttöliittymän kaikki ominaisuudet. Muutokset voivat johtaa AM voit mahdollisesti katkaisun.

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>Mitä tapahtuu IP-osoitteet, jos voin Sammuta AM?

Ei mitään. IP-osoitteet (julkisen VIP ja sisäinen IP-osoite) säilyvät pilvipalvelussa tai AM.

> [AZURE.NOTE] Jos haluat vain Sammuta AM, älä käytä hallinta-portaalin tekemään niin. Sammuta tällä hetkellä, poista virtuaalikoneen varaus.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Voit siirtää VMs yhden aliverkosta toiseen aliverkon VNet-ilman käyttöönotto uudelleen?

Kyllä. Voit etsiä lisätietoja [tähän](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Staattisen MAC-osoitteen määrittäminen Omat AM varten

Ei. MAC-osoitetta ei voi määrittää nimipalvelimet.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>MAC-osoite säilyy samana Omat AM, kun se on luotu?

Kyllä, MAC-osoite säilyy samana AM, vaikka AM on pysäytetty (deallocated) ja uudelleen Sender.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Voin muodostaa yhteyden Internetiin VNet-AM?

Kyllä. Kaikki palvelut, jotka on otettu käyttöön VNet sisällä voit muodostaa yhteys Internetiin. Lisäksi jokaisen pilvipalvelussa käyttöön Azure-tietokannassa on määritetty julkisesti käytettävissä VIP. Sinun on määrittää syötteen päätepisteet PaaS roolien ja päätepisteet VMs käyttöön hyväksymään yhteyksiä Internet-palveluun.

## <a name="vnets-and-services"></a>VNets ja -palvelut

### <a name="what-services-can-i-use-with-vnets"></a>Mitä palveluja voin käyttää VNets kanssa?

Voit käyttää vain VNets Laske palveluja. Laske-palvelut ovat rajoitetut Pilvipalveluiden (Internetin kautta tai työntekijän roolit) ja VMs.

### <a name="can-i-use-web-apps-with-virtual-network"></a>Web Apps-sovellusten käyttäminen Virtual verkoston kanssa

Kyllä. Voit ottaa käyttöön verkkosovelluksissa sisällä VNet, käyttämällä Ietokannan (sovelluksen palvelun ympäristössä). Lisääminen, verkkosovelluksissa turvallisesti muodostaa ja käyttää Azure VNet resursseja, jos sinulla on kohta ja sivuston oman VNet määritetty. Lisätietoja on seuraavissa artikkeleissa:


- [Web Apps-sovellusten luominen sovelluksen Service-ympäristössä](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)

- [Web Apps VPN-integrointi](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)

- [VNet integrointi ja Hybrid yhteydet käyttämisestä Web Apps-sovellusten kanssa](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)

- [Integroi verkkosovellukseen näennäinen Azure-verkoston kanssa](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Voit ottaa pilvipalveluihin Internetin kautta tai työntekijä rooleille (PaaS) VNet kanssa?

Kyllä. Voit ottaa käyttöön VNets PaaS palveluja.

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>Miten voin ottaa PaaS roolit VNet?

Voit tehdä tämän määrittämällä VNet nimi ja rooli /subnet yhdistämismääritykset palvelun kokoonpanon verkon määritys-osassa. Sinun ei tarvitse päivittää oman binaaritiedostot.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Voinko siirtää palveluiden ja siitä uloskirjautuminen VNets?

Ei. Et voi siirtää services ja siitä uloskirjautuminen VNets. Sinun on poistaa ja siirtää sen toiseen VNet palvelun uudelleen käyttöön.

## <a name="vnets-and-security"></a>VNets ja suojaus

### <a name="what-is-the-security-model-for-vnets"></a>Mikä on VNets suojausmalli?

VNets eristetään kokonaan keskenään ja muiden palveluiden ylläpidettävä Azure infrastruktuuri. VNet on luota reunaa.

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>Voin määrittää käyttöoikeusluettelot tai NSGs Omat VNets käyttöön?

Ei. Ei voi yhdistää käyttöoikeusluettelot tai NSGs VNets. Käyttöoikeusluettelot voidaan määrittää syötteen päätepisteet VMs, jotka on otettu käyttöön VNets varten, ja NSGs voi liittää aliverkosta tai NIC.

### <a name="is-there-a-vnet-security-whitepaper"></a>Onko VNet suojaus-julkaisu?

Kyllä. Voit ladata sen [tähän](http://go.microsoft.com/fwlink/?LinkId=386611).

## <a name="apis-schemas-and-tools"></a>API, mallit ja työkalut

### <a name="can-i-manage-vnets-from-code"></a>VNets hallinta koodista

Kyllä. REST API avulla voit hallita VNets ja paikallisen-yhteys. Lisätietoja löytyy [tähän](http://go.microsoft.com/fwlink/?LinkId=296833).

### <a name="is-there-tooling-support-for-vnets"></a>Onko sillä tuki VNets?

Kyllä. Voit käyttää eri ympäristöissä PowerShell-ja komentoriviltä. Lisätietoja löytyy [tähän](http://go.microsoft.com/fwlink/?LinkId=317721).
