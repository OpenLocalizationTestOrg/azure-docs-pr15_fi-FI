<properties
   pageTitle="Azure verkon suojauksen yleiskatsaus | Microsoft Azure"
   description=" Tässä artikkelissa helppoa ymmärtää, mitä Microsoft Azure uusista ominaisuuksista verkkosuojaus-alueella. Annamme basic selitykset core verkon suojauksen käsitteitä ja vaatimukset ja tietoja, mitä Azure uusista ominaisuuksista kussakin alueilla. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-network-security-overview"></a>Azure verkon suojauksen yleiskatsaus

Microsoft Azure sisältää tehokkaat verkkoinfrastruktuuria tukea sovelluksen ja palvelun verkkoyhteyksien vaatimukset. Verkkoyhteys on mahdollista paikallisen Azure-välissä resurssien välillä ja Azure isännöidään resursseja, ja voit ja Internetistä ja Azure.

Tässä artikkelissa on helpompi ymmärtää, mitä Microsoft Azure uusista ominaisuuksista verkkosuojaus-alueella. Tähän annamme core verkon suojauksen käsitteitä ja vaatimukset basic selitykset. On myös antaa sinun tietoja Azure on tarjota kussakin alueilla. On monia linkkejä muuhun sisältöön, joiden avulla saat tarkempaa tietoa alueet, jotka haluat muuttaa.

Azure verkon suojauksen yleiskatsaus tässä artikkelissa keskitytään seuraavasti:

- Azure verkko
- Verkon käytönhallinta
- Suojattu käyttö ja paikallisen-etäyhteyksien
- Käytettävyys
- Lokiin kirjaaminen
- Nimenselvitys
- DMZ-arkkitehtuuri
- Azure Tietoturvakeskus

## <a name="azure-networking"></a>Azure verkko

Näennäiskoneiden on verkkoyhteys. Vaatimus tukemaan Azure edellyttää näennäiskoneiden on muodostettava yhteys Virtual Azure-verkkoon. Virtuaalinen Azure-verkko on looginen rakenteen laadittuihin fyysinen Azure verkko-kangasta päälle. Kunkin loogisen Azure Virtual verkon eristetään kaikki muut Azure Virtual verkot. Näin voit varmistaa, että verkkoliikennettä käyttöönoton ei ole käytettävissä muissa Microsoft Azure-asiakkaille.

Opi lisää:

- [VPN-yleiskatsaus](../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Verkon käytönhallinta
Verkon käytönvalvonta on act rajoittamisen connectivity ja sieltä pois tiettyjä laitteita tai aliverkosta Virtual Azure-verkossa. Verkon käytönhallinta lähinnä varmistaaksesi, että näennäiskoneiden ja palvelut ovat käytettävissä vain käyttäjille ja laitteille, johon haluat ne käytettävissä. Access-ohjausobjektit perustuvat Salli tai estä päätökset yhteyksiä, ja virtuaalikoneen tai-palvelusta.

Azure tukee useita verkon käytönhallinta. Näitä ovat:

- Verkon kerroksen hallinta
- Reitittää ohjausobjekti ja pakotettu tunneling
- Suojaus VPN-laitteille

### <a name="network-layer-control"></a>Verkon kerroksen hallinta
Minkä tahansa suojatun käyttöönotto edellyttää, että jotkin mitataan verkon käytönhallinta. Verkon käytönhallinta lähinnä varmistaaksesi, että näennäiskoneiden ja käynnissä olevat näiden näennäiskoneiden verkkopalveluita voivat viestiä vain muiden verkkolaitteiden, että he tarvitsevat yhteydenpito ja kaikki muut yhteydenmuodostamisyritykset on estetty.

Jos tarvitset perusverkko käyttöoikeustaso ohjausobjektin (perustuu IP-osoite ja TCP tai UDP-protokollat), voit käyttää verkon käyttöoikeusryhmät. Voit hallita [5-monikon](https://www.techopedia.com/definition/28190/5-tuple)perusteella verkon suojauksen ryhmän (NSG) on basic tilallisten paketin suodatus palomuuri. NSGs ei ole sovelluksen kerroksen tarkastus tai todennettu access-ohjausobjektit.

Opi lisää:

- [Verkon käyttöoikeusryhmät](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Reitittää ohjausobjekti ja pakotettu Tunneling
Voit hallita reititys Azure Virtual verkoissa on kriittinen verkon turvallisuus- ja ohjausobjektin ominaisuus. Jos reititys on määritetty virheellisesti, sovellusten ja palveluiden isännöimät virtuaalikoneen voi muodostaa yhteyttä laitteiden piilotettavien he voivat muodostaa yhteyden, mukaan lukien laitteet omistaja ja ylläpitäjä mahdollisia hyökkääjiä.

Azure verkko tukee mahdollisuus mukauttaa Azure lopettaminen Virtual verkkoliikenteen reititys toiminnan. Voit muuttaa reititys taulukon oletusarvot Azure Virtual verkossa. Reititys toiminta voit varmistaa, että kaikki liikenne laitteelta tai laitteet-ryhmän Lisää tai jättää Azure Virtual verkon kautta tiettyyn kohtaan ohjausobjekti.

Esimerkiksi voi olla VPN-suojauksen laitteen Azure Virtual verkossa. Haluat varmistaa, että kaikki liikenne ja sieltä pois Azure Virtual verkon käy läpi virtual suojaus, että laitteen. Voit tehdä tämän määrittämällä [Käyttäjän määrittämät tiet](../virtual-network/virtual-networks-udr-overview.md) Azure-tietokannassa.

[Pakotettu tunneling](https://www.petri.com/azure-forced-tunneling) on järjestelmä, jonka avulla voit varmistaa, että palvelujen ei sallita laitteet Internet-yhteyden muodostamiseen. Huomaa, että kyseessä on erilainen kuin hyväksymällä saapuvia yhteyksiä ja vastata niihin. Edusta-WWW-palvelimien täytyy kokouspyyntöön Internet-isäntien ja siten Internet Orpotermit liikenne sallitaan saapuva nämä WWW-palvelimien ja WWW-palvelimien voivat vastata.

Mitä et halua sallia on lähtevän pyynnön aloittaminen edusta-WWW-palvelin. Pyynnöt voivat edustaa tietoturvariskin, koska nämä yhteydet voidaan ladata haittaohjelmia. Myös silloin, kun haluat aloittaa lähtevä pyynnöt Internet nämä edusta-palvelimiin, haluat ehkä Pakota yleisöä menee läpi paikallisen web-välityspalvelimet niin, että voit hyödyntää URL-osoite, suodattaminen ja lokiin kirjaaminen.

Sen sijaan haluat pakotettu tunneling avulla voit estää tämän. Kun otat pakotetun tunneling, kaikki Internet-yhteyksien pakotetaan paikallisen yhdyskäytävän kautta. Voit määrittää pakotettu tunneling mukaan käyttäjän määrittämät tiet hyödyntämistä.

Opi lisää:

- [Mitä käyttäjän määrittämät tiet, ja IP-osoitteen välityksen](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Suojaus VPN-laitteille
Kun verkko-käyttöoikeusryhmät, käyttäjän määrittämät tiet ja pakotettu tunneling antaa sinulle taso [verkkosegmenttiä](https://en.wikipedia.org/wiki/OSI_model)verkko- ja kerrokset suojaus, voi olla ajat, kun haluat ottaa käyttöön suurempia verkon suojaus.

Esimerkiksi suojausvaatimuksia voivat kuulua muun muassa:

- Todennus- ja ennen sovelluksen pääsyn salliminen
- Tunkeutumisen tunnistaminen ja tunkeutumisen vastaus
- Sovelluksen kerroksen tarkastus korkean tason protokollia
- URL-suodatus
- Verkon tason virusten ja antimalware
- Anti robotti suojaus
- Sovelluksen käyttöoikeuksien hallinta
- Lisää WWW.microsoft.com-sivustoa kohtaan suojaus (edellä WWW.microsoft.com-sivustoa kohtaan suojaus annettu Azure kangasta itse)

Voit käyttää näitä verkon suojaustoiminnot Azure kumppanin-ratkaisun avulla. Voit etsiä uusimmat Azure kumppanin verkon security-ratkaisujen ohjesisältöä [Azure Marketplacesta](https://azure.microsoft.com/marketplace/) ja etsimällä "suojaus" ja "verkkosuojaus".

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Suojatun etäkäyttöpalvelimen ja Cross paikallisen yhteydet
Asetukset, määrittäminen ja hallinta Azure resurssit tarpeitasi tehtävä etäyhteyden välityksellä. Lisäksi voit [hybrid IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) -ratkaisujen, joiden osat paikalliseen käyttöön ja Azure julkisen pilveen. Näissä tilanteissa edellyttävät suojatun etäkäyttö.

Azure verkko tukee suojatun etäkäyttöpalvelimen seuraavissa tilanteissa:

- Yksittäisten työasemien yhdistäminen Virtual Azure-verkossa
- Yhdistä paikallisen verkon Azure Virtual verkkoon näennäisen yksityisverkon
- Yhdistä paikallisen verkon Azure Virtual verkkoon erillinen WAN-linkin avulla
- Azure Virtual verkostojen yhdistäminen toisiinsa

### <a name="connect-individual-workstations-to-an-azure-virtual-network"></a>Yksittäisten työasemien muodostaa Azure Virtual verkkoon
Voi olla ajat, kun haluat ottaa käyttöön yksittäisiä kehittäjät tai toimintojen henkilökunnan näennäiskoneiden ja Azure palveluiden hallinta. Esimerkiksi tarvitset virtual koneeseen Azure Virtual verkossa ja suojauskäytäntö ei salli RDP- tai SSH yksittäisiä näennäiskoneiden etäkäyttö. Tässä tapauksessa voit pisteen sivuston VPN-yhteyden.

Pisteen sivuston VPN-yhteyden käyttää [SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx) -protokollaa, jotta voit muodostaa yhteyden yksityisyyttäsi ja suojata käyttäjän ja Azure Virtual verkon välillä. Kun VPN-yhteys on muodostettu, käyttäjä saa oikeuden RDP tai SSH kyselyjä virtual koneen Azure Virtual verkossa VPN-linkin kautta (olettaen, että käyttäjä voi todentaa ja henkilöstön oikeudet).

Opi lisää:

- [PowerShellin Virtual verkon pisteen sivuston yhteyden asetusten määrittäminen](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-vpn"></a>Paikallisen verkon yhdistäminen Azure Virtual verkon näennäisen yksityisverkon
Haluat ehkä koko yrityksen verkkoon tai sen osiin muodostaa Virtual Azure-verkkoon. Tämä on yleinen hybrid IT skenaariot missä yhtiöiden [laajentaa käyttäjän paikallisen palvelinkeskukseen Azure kyselyjä](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). Monissa tapauksissa yritysten isännöi palvelun paikallisessa Azure ja osia, kuten kun ratkaista sisältää Azure ja taustatietokantaan tietokantojen paikallisten edusta-WWW-palvelimien osat. Tällaisia "paikallisen rajat-yhteyksien tehdä myös Azure sijaitsevat resurssien Lisää suojata ja ottaa käyttöön skenaarioiden, kuten laajentaminen kyselyjä Azure Active Directory-toimialueen ohjaimet.

Yksi tapa tehdä tämä on käyttää [sivuston sivuston VPN-yhteyttä](https://www.techopedia.com/definition/30747/site-to-site-vpn). Sivuston sivuston VPN ja pisteen sivuston VPN välinen ero on, pisteen sivuston VPN yhdistetään yhteen laitteeseen Azure Virtual Network samalla, kun sivusto sivusto VPN yhdistää koko verkko (kuten paikallista verkkoa) Virtual Azure-verkkoon. Sivuston sivuston VPN-yhteydet Virtual Azure-verkkoon käyttää suojatuissa IP-Tunnelointitila VPN-protokollaa.

Opi lisää:

- [Luo Resurssienhallinta VNet sivusto sivusto VPN-yhteyden Azure-portaalissa](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Suunnittelun ja rakenteen VPN-yhdyskäytävän](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-dedicated-wan-link"></a>Paikallisen verkon yhdistäminen Azure Virtual Verkostossayammer erillinen WAN-linkin
Pisteen sivuston ja sivuston sivuston VPN-yhteydet ovat voimassa paikallisen-yhteyden ottaminen käyttöön. Joissakin organisaatioissa voit kuitenkin niissä on seuraavat haitoista:

- VPN-yhteydet tietojen siirtäminen Internetin välityksellä – tämä paljastaa asiakasyhteydet tietoturvariskin liittyvistä tietojen siirtämistä julkisen verkon kautta. Lisäksi luotettavuutta ja käytettävyyttä Internet-yhteyksien ei voida taata.
- VPN-yhteydet Azure Virtual verkkoihin pidetään kaistanleveyden rajoitettu Jotkin sovellukset ja varten siinä muodossa kuin ne enimmäismäärä, on 200Mbps ympärille.

Organisaatioissa, joissa on yleensä parhaan mahdollisen tietoturvan ja käytettävyyden niiden paikallisen-yhteyksien erillinen WAN linkkien avulla voit muodostaa yhteyden etäkohteeseen. Azure on voi käyttää oma WAN-linkin, joiden avulla voit muodostaa paikallisen verkon Virtual Azure-verkkoon. Tämä on otettu käyttöön Azure ExpressRoute.

Opi lisää:

- [ExpressRoute teknisiä tietoja](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-to-each-other"></a>Azure Virtual verkostojen yhdistäminen toisiinsa
On mahdollista, voit käyttää useita Azure Virtual verkkojen käyttöönoton. On useita syitä, miksi tämä kannattaa tehdä. Voit yksinkertaistaa hallintaa, voi olla jokin syyt toisen ehkä tietoturvasyistä. Riippumatta siitä, motivointi tai perusteet julkaisemisen resurssien eri Azure Virtual verkkoihin voi olla halutessasi resurssien kunkin verkkojen muodostaa yhteys toisiinsa.

Yksi vaihtoehto on yksi Azure Virtual verkossa muodostaa palveluihin toisessa Azure Virtual verkossa "takaisin toistuminen-palveluiden Internetin kautta. Yhteys Käynnistä yhdessä Azure Virtual verkossa, siirry Internetin välityksellä ja sitten toinen käyttäjä palaa lomaltaan kohteeseen Azure Virtual Network. Tämä vaihtoehto paljastaa perusominaisuuksiin minkä tahansa Internet-pohjaisten viestintä artikkelissa yhteys.

Parempi vaihtoehto voi olla luominen Azure Virtual verkon Azure Virtual verkon sivusto sivusto VPN-yhteyttä. Tämä Azure Virtual verkon Azure Virtual verkon sivusto sivusto VPN käyttää samaa [Tunnelointitila IP](https://technet.microsoft.com/library/cc786385.aspx) -protokollaa paikallisen-sivusto sivusto VPN-yhteyden edellä mainituista.

Käytön Azure Virtual verkon Azure Virtual verkon sivusto sivusto VPN etuna on, että VPN-yhteys on muodostettu Azure verkon; kangasta päälle se ei ole Internetin välityksellä. Tämä on ylimääräisiä layer Security verrattuna sivusto sivusto VPN-yhteydet, jotka Internetin välityksellä.

Opi lisää:

- [VNet VNet yhteysasetusten Azure Resurssienhallinta ja PowerShellin avulla](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Käytettävyys
Käytettävyys on minkä tahansa suojaus-ohjelman avaimen osa. Jos käyttäjien ja järjestelmien voi käyttää tarvitsemiaan käyttää verkon kautta, palvelun pidettäviä käsiin. Azure on verkko toiminnot, jotka tukevat seuraavia suuren käytettävyyden-järjestelmiä:

- HTTP-pohjaista kuormituksen hallinta
- Verkon tason kuormituksen hallinta
- Yleinen kuormituksen

Kuormituksen tasaamisen on suunniteltu jakaminen tasaisesti yhteydet leviävät useilla eri laitteilla järjestelmä. Kuormituksen tasaamisen tavoitteet ovat seuraavat:

- Suurenna käytettävyyttä –, kun lataat saldo yhteyksiä eri useilla eri laitteilla, yksi tai useampi laitteet voi olla ei käytettävissä ja palveluita jäljellä online laitteissa voit jatkaa tukemaan huolto sisältöä
- Parantaa suorituskykyä – kun lataat saldo yhteyksiä eri useita laitteita, yhteen laitteeseen ei tarvitse tehdä suoritin osumien. Sen sijaan että sisällön tarjoillaan käsittelyn ja muistin vaatimukset levitä yli useilla eri laitteilla.

### <a name="http-based-load-balancing"></a>HTTP-pohjaista kuormituksen hallinta
Organisaatiot, jotka suoritetaan web-pohjaisten palvelujen usein haluaa on HTTP-pohjaista kuormituksen näiden WWW-palvelut, jotka auttavat varmistaa riittävä tasot suorituskyvyn ja suuren käytettävyyden eteen. Toisin kuin perinteisen verkkopohjainen kuormituksen tasoitusmääritykset HTTP-pohjaista päätökset kuormituksen kuormituksen tasoitusmääritykset perustuvat HTTP-protokolla ei ole Verkko- ja transport layer protokollat-ominaisuuksia.

Anna HTTP-pohjaista kuormituksen tasaamisen verkkopohjaisia palvelujen Azure löytyy Azure sovelluksen yhdyskäytävän. Azure-sovelluksen Gateway tukee:

- HTTP-pohjaista kuormituksen tasaaminen – Lataa Vastatilin päätöksiä tehdään perusteella-ominaisuus määräten HTTP-protokolla
- Evästeiden perustuva istunnon affiniteetti – tätä ominaisuutta varmistetaan, että yhteyksiä takana, kuormituksen palvelimia pysyy muuttumattomana asiakkaan ja palvelimen välillä. Tämä vakuutusyhtiö tapahtumat vakavuus.
- SSL-purku – kun asiakas-yhteys on muodostettu kuormituksen, jossa istunnon asiakkaan ja kuormituksen välillä on salattu käyttämällä HTTPS (SSL /) protokolla. Jotta voit parantaa suorituskykyä jakavien, voit määrittää kuormituksen ja takana kuormituksen tasauspalvelun käyttö (salaamaton) HTTP-protokolla verkkopalvelin välinen yhteys. Tätä kutsutaan "SSL purku" koska web-palvelinten kuormituksen takana ei kohdata liittyvistä salaus suoritin katseltavan ja näin ollen pitäisi palvelupyyntöjä nopeammin.
- URL-pohjaiset sisällön reititys – tämä toiminto tekee kuormituksen tekemään päätöksiä, missä voit lähettää kohteen URL-Osoitteen perusteella yhteyksiä varten. Tämä on usein joustavampi kuin ratkaisuja, jotka ladata Vastatilin päätöksiä IP-osoitteiden perusteella.

Opi lisää:

- [Sovelluksen yhdyskäytävän yleiskatsaus](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Verkon tason kuormituksen hallinta
Toisin kuin HTTP-pohjaista kuormituksen tasaamisen, verkon tason kuormituksen tasaamisen tekee kuormituksen Vastatilin päätöksiä IP-osoite ja portti (TCP tai UDP) numeroiden perusteella.
Voit käyttää verkon tason kuormituksen Azure-tietokannassa käyttämällä Azure ladata tasaustoiminto etuja. Azure kuormituksen joitakin keskeisiä ominaisuuksia ovat seuraavat:

- Verkon tason kuormituksen tasaamisen perusteella IP-osoite ja portin numerot
- Minkä tahansa sovelluksen layer protokollan tuki
- Azuren näennäiskoneiden saldojen lataaminen ja cloud services-roolin esiintymiä
- Voi käyttää Internetiin yhteydessä oleva (ulkoinen kuormituksen tasaamisen) ja -Internet vastakkaisten (sisäinen kuormituksen hallinta)-sovelluksia ja näennäiskoneiden
- Päätepisteen seuranta, jolla voidaan määrittää, jos takana kuormituksen palveluja on poistuvat käytöstä

Opi lisää:

- [Internet-vastakkaisten kuormituksen useita näennäiskoneiden tai palveluiden välillä](../load-balancer/load-balancer-internet-overview.md)
- [Sisäinen kuormituksen tasauspalvelun yleiskatsaus](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Yleinen kuormituksen
Joissakin organisaatioissa haluat parhaan mahdollisen käytettävyys mahdollista. Yksi tapa tämän tavoitteen saavuttamiseksi on yleisesti eri aikajaksoille palvelinkeskusten isännöidä sovelluksia. Kun sovellus isännöidään kaikkialla maailmassa sijaitsevat tiedot keskikohdan mukaan-on mahdollista koko geopoliittisten alueen poistuvat käytöstä ja jatkuu sovelluksen ja käytössä.

Lisäksi voit avata isännöinnin yleisesti eri aikajaksoille palvelinkeskusten sovellusten käytettävyyden edut voit myös avata suorituskyky. Suorituskyvyn hyödyn saadaan toimintoa, joka ohjaa palvelinkeskukseen on lähinnä laitteella, joka tekee pyynnön-palvelun avulla.

Yleinen kuormituksen tasaamisen voi antaa sinulle sekä hyödyn. Azure-tietokannassa voit käyttää edut yleinen kuormituksen Azure liikenteen hallinta-valintaikkunassa.

Opi lisää:

- [Mikä on liikenteen hallinta?](../traffic-manager/traffic-manager-overview.md)

## <a name="logging"></a>Lokiin kirjaaminen
Verkon tasolla kirjaaminen on mikä tahansa verkko-suojauksen skenaarion avaimen funktio. Azure-tietokannassa voit kirjautua verkon käyttöoikeusryhmät saat verkko-tason tietojen tallentaminen saatuja tietoja. NSG kirjaaminen, saat tietoja:

- Valvontalokien – lokitiedostot käytetään Näytä kaikki toiminnot, jotka on lähetetty Azure-tilauksia. Lokitiedostot ovat oletusarvoisesti käytössä, ja se voidaan käyttää Azure portaalissa.
- Tapahtumalokien – lokitiedostot on tietoa siitä, mitä NSG sääntöjä käytetään.
- Laskuri lokit – lokitiedostot ilmoittaa, kuinka monta kertaa NSG niiden sääntöjen käytetty Estä tai Salli liikenne.

[Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/)-tehokas tietojen visualisoinnin-työkalun avulla voit tarkastella ja analysoida lokitiedostot.

Opi lisää:

- [Lokitiedoston analysointitietoja verkon käyttöoikeusryhmät (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)

## <a name="name-resolution"></a>Nimenselvitys
Nimenselvitys on kriittinen funktio kaikissa isännöit Azure-palveluissa. Suojaus, näkökulmasta n nimi tarkkuus-funktiota voi aiheuttaa voi saada uudelleenohjausta pyynnöt sivustoista sivustossa. Suojatun nimenselvitys on kaikki isännöidään cloud Services.

Liittyy nimenselvitys on kahdenlaisia:

- Sisäinen nimenselvitys – sisäinen nimenselvitys käyttämä Azure Virtual lopettaminen ja paikallisen lopettaminen palvelut. Sisäinen nimenselvitys käytettäviä nimet eivät ole käytettävissä Internetin välityksellä. Suojaustapa on tärkeää, että nimenä tarkkuus-malli ei ole käytettävissä ulkoisten käyttäjien.
- Ulkoinen nimenselvitys – ulkoisten nimenselvitys käyttävät henkilöt ja laitteet paikallisen ja Azure Virtual verkkojen ulkopuolella. Nämä ovat nimiä, jotka ovat näkyvissä Internet- ja käytetään ohjaamaan pilvipohjainen palvelujen yhteys.

Sisäinen nimenselvitys sinulla on kaksi vaihtoehtoa:

- Azure Virtual verkon DNS-palvelin – kun luot uuden Azure Virtual verkon, DNS-palvelin on luonut puolestasi. DNS-palvelin voi ratkaista Azure Virtual verkossa sijaitsevat koneet nimet. DNS-palvelin ei voi muuttaa, ja Azure kangasta projektipäällikkö näin henkilöistä suojatun nimi tarkkuus-ratkaisun hallinnasta.
- Tuo DNS-palvelin – voit halutessasi laajennettujen oman valitsemisen Azure Virtual verkon DNS-palvelimeen. DNS-palvelimeen voitu Active Directoryn integroitu DNS-palvelimeen tai Oma DNS server ratkaista myöntämä Azure kumppani, jonka voit hankkia Azure Marketplacesta.

Opi lisää:

- [VPN-yleiskatsaus](../virtual-network/virtual-networks-overview.md)
- [Hallitse DNS-palvelimet käyttämä Virtual Network (VNet)](../virtual-network/virtual-networks-manage-dns-in-vnet.md)

Ulkoiset DNS-ratkaisun sinulla on kaksi vaihtoehtoa:

- Isännöidä omia ulkoisen DNS server-ympäristöön
- Isännöidä palveluntarjoajan ulkoiseen DNS-palvelimeen

Monta suurissa organisaatioissa isännöi omia DNS servers paikallisen. Käyttäjä voi tehdä, koska ne on Verkko osaamisalueet ja yleisen tavoitettavuustietojen tekemään niin.

Useimmissa tapauksissa kannattaa isännöidä DNS nimi tarkkuus palvelujen palveluntarjoajan. Nämä palveluntarjoajien on verkon osaamisalueet ja yleisen tavoitettavuustietojen erittäin suuri käytettävyys nimi tarkkuus palvelujen varmistamiseksi. Käytettävyys on olennainen DNS-palvelut, koska nimi tarkkuus palvelujen epäonnistuu, jos kukaan voitava muodostaa yhteys Internet-palveluiden vastakkaisten.

Azure avulla voit helposti saatavilla ja performant ulkoiseen DNS-ratkaisun Azure DNS-lomakkeessa. Ulkoinen nimi tarkkuus ratkaisu hyödyntää maailmanlaajuisesti Azure DNS-infrastruktuuria. Sen avulla voit isännöidä toimialueen Azure käyttämällä tunnistetiedot-ohjelmointirajapinnan-työkaluilla ja Azure muiden palvelujen kuin laskutus. Osana Azure se perii myös sisäänrakennettu platform vahvoja turvaominaisuudet.

Opi lisää:

- [Azure DNS-yleiskatsaus](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>DMZ-arkkitehtuuri
Useissa avulla DMZs määritetään niiden verkkojen Luo puskurin-alue Internet- ja niiden palveluiden välillä. Verkon DMZ-osa pidetään pieni suojaus-vyöhykkeeseen ja high-arvo ei ole resurssit ovat käytettävissä kyseisen verkko-osiossa. Näkyviin tulee yleensä suojauksen laitteet, joissa on verkkoliittymän DMZ-osiossa ja toiseen verkkoliittymän yhteydessä verkkoon, jossa on näennäiskoneiden ja palvelut, jotka hyväksyvät saapuvia yhteyksiä Internetistä.

Useilla eri versioiden verkon suunnittelu ja käyttöönotto DMZ-päätös ja sitten DMZ, jos haluat käyttää jotakin, mitä perustuu verkon suojausvaatimukset.

Opi lisää:

- [Microsoftin pilvipalveluihin ja verkkosuojaus](../best-practices-network-security.md)

## <a name="azure-security-center"></a>Azure Tietoturvakeskus
Tietoturvakeskus auttaa estämään, haku- ja uhkien vastaaminen ja on lisätty näkyvyys- ja päättää, Azure resurssien suojaus. Siinä on integroitu suojaus seuranta ja käytäntöjen hallinta Azure-tilauksissa, auttaa tunnistamaan uhkien, jotka muuten huomaamatta ja laaja valikoima security-ratkaisujen mediatyökaluja toimii.

Azure Tietoturvakeskus avulla voit optimoida ja seurata verkon suojausta:

- Tarjoaa verkon suojausta koskevia suosituksia
- Verkon suojausasetukset tilan seuranta
- Päätepisteen ja verkon tasoilla uhkien verkosta sekä varoittaa

Opi lisää:

- [Johdanto Azure Tietoturvakeskus](../security-center/security-center-intro.md)
