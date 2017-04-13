<properties 
    pageTitle="Premium Azure Redis välimuistin VPN-tuki määrittämisestä | Microsoft Azure" 
    description="Lue, miten voit luoda ja hallita Premium taso Azure Redis välimuistin kopioita VPN-tuki" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Virtuaalinen Verkkotuen määrittäminen Premium Azure Redis välimuisti
Azure Redis välimuistin on eri välimuistin tarjouksia, jossa välimuistin kokoa ja ominaisuuksista, kuten uusien Premium tason valinta joustavasti.

Azure Redis välimuistin erikoisominaisuuksia taso sisältää klusterointi pysyvyys ja virtual verkkotuki (VNet). VNet on yksityisverkon pilveen. Kun Azure Redis välimuistin esiintymää on määritetty VNet, se ei ole käytettävissä julkisesti ja niitä voi käyttää vain näennäiskoneiden ja VNet-sovelluksissa. Tässä artikkelissa käsitellään VPN-tuen premium Azure Redis välimuisti-esiintymän määrittäminen.

>[AZURE.NOTE] Azure Redis välimuistin tukee sekä perinteinen ja ARM VNets.

Lisätietoja muiden välimuistin erikoisominaisuuksia on artikkelissa [Johdatus Azure Redis välimuistin Premium-tason](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Miksi VNet?
[Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/) käyttöönoton on parannettu suojaus ja Azure Redis välimuistin, sekä aliverkosta, ohjausobjektin käytäntöjen ja muiden ominaisuuksien käytön rajoittaminen edelleen Azure Redis välimuistin yksinään.

## <a name="virtual-network-support"></a>VPN-tuki
Virtuaalinen verkon (VNet) tuki on määritetty välimuistin luonnin aikana **Uusi Redis välimuisti** -sivu. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Kun olet valinnut premium, hinnat taso, voit määrittää Azure Redis välimuistin VNet integrointi valitsemalla VNet, joka on samassa tilaus ja välimuistin sijainniksi. Jos haluat käyttää uuden VNet, luo se ensin ohjeiden mukaisesti [luoda virtuaalisen verkon Azure-portaalissa](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) tai [luoda virtuaalisia (Klassinen) verkon Azure-portaalissa](../virtual-network/virtual-networks-create-vnet-classic-portal.md) ja palaa sitten voit luoda ja määrittää premium välimuistin **Uusi Redis välimuisti** -sivu.

Jos haluat määrittää uuden välimuistin VNet, **VPN** valitsemalla **Uusi Redis välimuisti** -sivu ja avattavasta luettelosta haluamasi VNet.

![VPN][redis-cache-vnet]

Valitse haluamasi aliverkon **aliverkon** avattavasta luettelosta ja määritä haluamasi **staattinen IP-osoite**. Jos käytät perinteistä VNet **staattinen IP-osoite** -kenttä on valinnainen, ja jos mitään ei määritetä, yksi valitaan valitun aliverkosta.

>[AZURE.IMPORTANT] Kun otat Azure Redis välimuistin ARM-VNet, välimuistin on oltava Kiinteä aliverkon, joka ei sisällä muita resursseja lukuun ottamatta Azure Redis välimuistin esiintymät. Jos yritetään suorittaa Azure Redis välimuistin käyttöön ARM-VNet, aliverkon, joka sisältää muita resursseja, asennus epäonnistuu.

![VPN][redis-cache-vnet-ip]

>[AZURE.IMPORTANT] Aliverkon neljä ensimmäistä osoitteet on varattu eikä sitä voi käyttää. Lisätietoja on artikkelissa [siinä on kaikki nämä aliverkosta IP-osoitteet käyttörajoitukset?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Välimuistin luomisen jälkeen voit tarkastella VNet määrityskohde valitsemalla **VPN** - **asetukset** -sivu.

![VPN][redis-cache-vnet-info]


Yhteyden muodostamisessa Azure Redis välimuistin-esiintymään, kun käytössä VNet, Määritä välimuistin isäntänimi yhteysmerkkijonon seuraavan esimerkin mukaisesti.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Azure Redis.txt välimuistin VNet usein kysytyt kysymykset

Seuraavassa on vastauksia usein kysyttyihin kysymyksiin Azure Redis välimuistin skaalausta.

-   [Mitkä ovat Azure Redis välimuistin ja VNets joitakin yleisiä virheellisen määrityksen ongelmia?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
-   [Voit käyttää VNets vakio- tai basic välimuistin?](#can-i-use-vnets-with-a-standard-or-basic-cache)
-   [Miksi-vaihtoehdon Redis.txt välimuistin luominen epäonnistui joitakin aliverkosta, mutta ei muiden?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
-   [Toimivatko kaikkia välimuistin ominaisuuksia, kun isännöivän VNET-välimuistin?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)


## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Mitkä ovat Azure Redis välimuistin ja VNets joitakin yleisiä virheellisen määrityksen ongelmia?

Kun Azure Redis välimuistin isännöidään VNet, käytetään portit seuraavassa taulukossa. Jos nämä portit on estetty, välimuistia ei ehkä toimi oikein. Pysty vähintään yksi seuraavista porttien estetyt on yleisin virheellisen määrityksen ongelma, Azure Redis välimuistin VNet käytettäessä.

| Portit     | Suunta        | Protokollan | Tarkoitus                                                                           | Etä-IP                           |
|-------------|------------------|--------------------|-----------------------------------------------------------------------------------|-------------------------------------|
| 80, 443     | Lähtevän         | TCP                | Redis riippuvuudet, Azure tallennustilan/PKI (Internet)                                | *                                   |
| 53          | Lähtevän         | TCP/UDP            | Redis riippuvuuksia DNS (Internet/VNet)                                         | *                                   |
| 6379, 6380  | Saapuva          | TCP                | Asiakkaan tietoliikenne Redis.txt, Azure kuormituksen tasaamisen                               | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 8443        | Saapuva ja lähtevä liikenne | TCP                | Toteutuksen jälkeisen Redis.txt yksityiskohdat                                                   | VIRTUAL_NETWORK                     |
| 8500        | Saapuva          | TCP/UDP            | Azure kuormituksen                                                              | AZURE_LOADBALANCER                  |
| 10221 10231 | Saapuva ja lähtevä liikenne | TCP                | Toteutuksen jälkeisen yksityiskohtien Redis.txt (VIRTUAL_NETWORK voit rajoittaa päätepiste) | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 13000 13999 | Saapuva          | TCP                | Asiakkaan tietoliikenne Redis klustereiden, Azure kuormituksen tasaamisen                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 15000 15999 | Saapuva          | TCP                | Asiakkaan tietoliikenne Redis klustereiden, Azure kuormituksen tasaamisen                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 16001       | Saapuva          | TCP/UDP            | Azure kuormituksen                                                              | AZURE_LOADBALANCER                  |
| 20226       | Saapuvien + lähtevä | TCP                | Toteutuksen jälkeisen Redis.txt klustereiden yksityiskohdat                                          | VIRTUAL_NETWORK                     |


On verkkoyhteyksien vaatimukset Azure Redis välimuistin, jotka voivat ei alun perin täytyttävä virtual verkossa. Azure Redis välimuisti edellyttää kaikki seuraavat osat toimivat oikein käytettäessä virtual verkossa oleville.

-  Lähtevän verkkoyhteyden Azuren tallennustilaan päätepisteiden kaikkialla maailmassa. Tämä sisältää sijaitsevat sama alue kuin Azure Redis välimuisti-esiintymän sekä tallennustilan päätepisteet sijaitsevien **muiden** Azure alueiden päätepisteet. Seuraavat DNS-toimialueet-kohdassa Ratkaise Azure tallennustilan päätepisteet: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*ja *file.core.windows.net*. 
-  Lähtevän verkkoyhteyden *ocsp.msocsp.com*, *mscrl.microsoft.com* ja *crl.microsoft.com*. Tämä edellyttää tukevat SSL-toimintoja.
-  Virtuaalinen verkon DNS-määritys on pystyttävä ratkaiseminen kaikki päätepisteet ja toimialueiden mainituista aiemmissa pisteinä. Nämä DNS-vaatimukset on täytettävä varmistamalla kelvollinen DNS-infrastruktuuria on määritetty ja ylläpitää virtual verkossa.



### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Voit käyttää VNets vakio- tai basic välimuistin?

VNets voi käyttää vain premium tallentaa.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Miksi-vaihtoehdon Redis.txt välimuistin luominen epäonnistui joitakin aliverkosta, mutta ei muiden?

Jos asennat Azure Redis välimuistin ARM-VNet, välimuistin on oltava oma aliverkon, joka sisältää ei resurssin laji. Jos yritetään suorittaa Azure Redis välimuistin käyttöön KÄDESSÄ VNet aliverkon, joka sisältää muita resursseja, asennus epäonnistuu. Sinun on poistettava aliverkon aiemmin resurssit, ennen kuin voit luoda uuden välimuistin Redis.txt.

Voit ottaa käyttöön useita resurssityyppiä perinteinen VNet, kunhan sinulla on IP-osoitteiden riittävästi käytettävissä.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Toimivatko kaikkia välimuistin ominaisuuksia, kun isännöivän VNET-välimuistin?

Kun välimuistin kuuluu VNET, vain asiakkaat VNET käyttää välimuistissa. Tämän vuoksi välimuistissa hallintaominaisuudet eivät toimi tällä hetkellä.

-   Redis Console - koska Redis konsoli käyttää isännöimät VMs, jotka eivät kuulu oman VNET Redis.txt cli.exe asiakas, se ei voi muodostaa välimuistin.


## <a name="use-expressroute-with-azure-redis-cache"></a>Azure Redis.txt välimuistin ExpressRoute käyttäminen

Asiakkaiden muodostaa [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) piiri niiden VPN-infrastruktuuria ulottuu näin Azure paikalliseen verkkoon. 

Oletusarvon mukaan juuri luomasi ExpressRoute piiri mainostaa oletusarvon reitti, joka sallii lähtevät Internet-yhteys. Tässä määrityksessä asiakassovelluksissa on pystyttävä muodostamaan yhteys muihin Azure päätepisteet, mukaan lukien Azure Redis välimuistin.

Tavallinen asiakkaan kokoonpano on kuitenkin määrittää omia oletusarvon reitin (0.0.0.0/0) joka pakottaa lähtevän Internet-liikenne paikalliseen flow sijaan paikallisen. Liikenne tämä työnkulku katkaisee yhteyden Azure Redis välimuistin reaktioita, koska lähtevän tietoliikenteen on joko estettyjen paikallisessa tai NAT yksittäiselle osoitteet, jotka eivät enää toimi kanssa eri Azure päätepisteet tunnistamattomassa joukko.

Ratkaisu on määrittäminen (vähintään yksi) käyttäjän määrittämä tiet (UDRs), joka sisältää Azure Redis välimuistin aliverkon. UDR määrittää aliverkon kielikohtaiset tiet, joka voi käyttää sen sijaan, että oletusarvoinen reitin.

Jos mahdollista kannattaa käyttää asetukset ovat seuraavat:

- ExpressRoute määritykset mainostaa 0.0.0.0/0 ja oletusarvoisesti voimassa tunneloi kaikki lähtevä liikenne paikalliseen.
- Käytetty Azure Redis välimuisti aliverkon UDR määrittää 0.0.0.0/0 seuraavan kohteen sisältötyyppi Internet (Tässä esimerkissä on kauemmaksi alaspäin ohjeaiheen).

Yhdistetyn vaikutus näin on, että aliverkon tason UDR ohittaa pakotettu ExpressRoute tunnelointi varmistaa välimuistista Azure Redis lähtevän Internet-yhteyttä.

Vaikka erillisen Azure Redis välimuistin muodostaminen paikallisen-sovelluksesta valitsemalla ExpressRoute ei ole tyypillinen käyttötilanne vuoksi nopeuttamiseksi (Saat parhaan suorituskyvyn Azure Redis välimuistin asiakkaiden on oltava sama alue kuin Azure Redis välimuistin), tässä skenaariossa lähtevä verkkopolun ei siirtyvät sisäinen yrityksen välityspalvelimet, eikä se on voimassa tunneloidun paikalliseen. Tällöin muuttuu tehokkaita NAT osoite lähtevän tietoliikenteen Azure Redis välimuistin. Azure Redis välimuistin NAT-osoitteen vaihtaminen esiintymän 's lähtevän verkkoliikenteen aiheuttaa yhteys epäonnistuu monia päätepisteet yllä olevassa luettelossa. Tuloksena on epäonnistunut Azure Redis välimuistin luominen yritykset.

**Tärkeä:**  UDR **on** määritetty tiet olla tarpeeksi ohittavat kaikki tiet määrittämiisi ExpressRoute määrityksestä. Seuraavassa esimerkissä käyttää laajan 0.0.0.0/0 osoitealueita ja sellaisenaan mahdollisesti voi vahingossa ohittaa reitti-ilmoituksissa käyttämällä tiettyihin osoitealueet.

**Hyvin tärkeä:**  Azure Redis välimuisti ei tueta ExpressRoute määrityksiä kyseinen **väärin rajat-mainos ohjautuu julkiseen peering polusta yksityinen peering polku**. ExpressRoute määritykset, joka on määritetty, julkisen peering saa reitti-ilmoituksissa Microsoft on suuri joukko Microsoft Azure IP-osoitealueet. Jos nämä osoitealueet virheellisesti yksityinen peering polulla rajat määrittämiisi, lopputulos on, että kaikki lähtevät verkkopaketit Azure Redis välimuistin esiintymän aliverkosta ovat väärin, asiakkaan paikallisen verkkoinfrastruktuuria voimassa tunneloidun. Verkko-työnkulku katkaisee Azure Redis välimuistin. Ongelman ratkaisu on lopettavat rajat mainonta ohjautuu julkiseen peering polusta yksityinen peering polku.

Käyttäjän määrittämät tiet taustatietoja sisältyy tämä [Yleiskatsaus](../virtual-network/virtual-networks-udr-overview.md). 

Saat lisätietoja ExpressRoute [ExpressRoute teknisiä tietoja](../expressroute/expressroute-introduction.md)

## <a name="next-steps"></a>Seuraavat vaiheet
Opettele käyttämään Lisää erikoisominaisuuksia välimuistin.

-   [Johdanto Azure Redis välimuistin Premium-taso](cache-premium-tier-intro.md)





  
<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

