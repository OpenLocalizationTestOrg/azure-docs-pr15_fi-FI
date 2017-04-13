<properties 
    pageTitle="Verkon määritysten tiedot Express reitin käsitteleminen" 
    description="Verkon määritysten tiedot sovelluksen Service-ympäristössä käytössä Virtual verkkojen yhdistetty ExpressRoute piiri." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="stefsch"/>   

# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Verkon määritysten tiedot sovelluksen Service-ympäristöissä, joissa ExpressRoute 

## <a name="overview"></a>Yleiskatsaus ##
Asiakkaat voivat liittää [Azure ExpressRoute] [ ExpressRoute] piiri niiden virtual verkkoinfrastruktuuria, ulottuu näin Azure paikalliseen verkkoon.  Sovelluksen Service-ympäristössä voidaan luoda [VPN] aliverkon[ virtualnetwork] infrastruktuuri.  Käynnissä olevat sovelluksen Service-ympäristön sovellusten sitten muodostaa suojattuja yhteyksiä taustatietokantaan resursseja on käytettävissä vain ExpressRoute-yhteyden kautta.  

Sovelluksen Service-ympäristössä voidaan luoda- **joko** virtual Azure Resurssienhallinta-verkko- **tai** classic käyttöönoton malli VPN.  Kanssa tehty kesäkuussa 2016 viimeaikaisesta muutoksesta ASEs myös nyt voidaan ottaa huomioon virtual verkkoja, jotka käyttävät julkisen osoitealueet tai RFC1918 osoite välilyöntejä (eli yksityisten osoitteiden). 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="required-network-connectivity"></a>Pakollinen verkkoyhteyden ##
On verkkoyhteyksien vaatimukset App Service-ympäristössä, joka ei voidaan aluksi täyttää virtual ExpressRoute yhteydessä verkkoon.  Sovelluksen palvelun ympäristöissä tarvitaan toimi seuraavista toimenpiteistä:


-  Lähtevän verkkoyhteys Azuren tallennustilaan päätepisteet maailmanlaajuisesti molemmat portit 80 ja 443.  Tämä sisältää sijaitsevat sama alue, kun sovellus palvelun ympäristössä sekä tallennustilan päätepisteet sijaitsevien **muiden** Azure alueiden päätepisteet.  Seuraavat DNS-toimialueet-kohdassa Ratkaise Azure tallennustilan päätepisteet: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* ja *file.core.windows.net*.  
-  Lähtevän verkkoyhteyden portti 445 Azure-tiedostot-palveluun.
-  Lähtevän verkkoyhteyden Sql DB päätepisteiden samalla alueella kuin sovelluksen Service-ympäristössä.  SQL DB päätepisteet ratkaista kohdassa toimialueella: *database.windows.net*.  Tämä edellyttää avaaminen porttien 1433 11000 11999 ja 14000 14999.  Lisätietoja on [artikkelissa Sql-tietokannan Vipuventtiili V12 portin käyttö](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
-  Lähtevän verkkoyhteyden Azure hallinta tasossa päätepisteet (ASM ja ARM-päätepisteet).  Tämä sisältää sekä *management.core.windows.net* ja *management.azure.com*lähtevä yhteys. 
-  Lähtevän verkkoyhteyden *ocsp.msocsp.com*, *mscrl.microsoft.com* ja *crl.microsoft.com*.  Tämä on tarpeen tukevat SSL-toimintoja.
-  Virtuaalinen verkon DNS-määritys on pystyttävä ratkaiseminen kaikki päätepisteet ja toimialueiden mainituista aiemmissa pisteinä.  Jos nämä päätepisteet ei voi ratkaista, sovelluksen palvelun ympäristön luominen yritykset epäonnistuu ja olemassa olevan sovelluksen palvelun ympäristöissä merkitään kuin perusasemassa.
-  Lähtevän tietoliikenteen käyttöoikeudet porttiin 53 vaaditaan tietoliikenteen DNS-palvelimet.
-  Jos mukautettujen DNS-palvelin VPN-yhdyskäytävän toinen pää-DNS-palvelin on oltava tavoitettavissa aliverkosta, jotka sisältävät sovelluksen Service-ympäristössä. 
-  Lähtevän verkkopolun ei siirtyvät sisäinen yrityksen välityspalvelimet, eikä se voi olla voimassa tunneloidun paikalliseen.  Tällöin muuttuu tehokkaita NAT osoite lähtevän tietoliikenteen App Service-ympäristössä.  Sovelluksen palvelun ympäristössä 's lähtevän tietoliikenteen NAT-osoitteen vaihtaminen aiheuttaa yhteys epäonnistuu monia päätepisteet yllä olevassa luettelossa.  Tuloksena on epäonnistunut App palvelun ympäristön luominen yritykset sekä aiemmin kunnossa App palvelun ympäristöissä ei merkitä kuin perusasemassa.  
-  Saapuva yhteys tarvittavat portit, sovelluksen palvelun ympäristöissä saa tässä [artikkelissa]kuvatulla tavalla[requiredports].

DNS-vaatimuksista on täytyttävä varmistamalla kelvollinen DNS-infrastruktuuria on määritetty ja ylläpitää virtual verkossa.  Jos DNS-määrityksiä muutetaan jostakin syystä ympäristössä palvelun sovelluksen luomisen jälkeen, kehittäjät voit pakottaa sovelluksen palvelun ympäristössä, jos haluat jatkaa uuden DNS-määrityksiä.  Käynnistävä juokseva ympäristön uudelleenkäynnistyksen "Uudelleen"-kuvakkeen sijaitsevat [Azure portal] -sovellus palvelun ympäristön hallinta-sivu yläreunassa[ NewPortal] aiheuttaa ympäristön Nosta uusi DNS-määrityksiä.

Saapuvien verkon käytön vaatimukset on täytettävä määrittämällä [verkon käyttöoikeusryhmän] [ NetworkSecurityGroups] sovelluksen palvelun ympäristön aliverkon sallimaan tarvittavat käyttöoikeudet, tässä [artikkelissa]kuvatulla tavalla[requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Lähtevän verkkoyhteyden ottaminen käyttöön App Service-ympäristössä##
Oletusarvon mukaan juuri luomasi ExpressRoute piiri mainostaa oletusarvon reitti, joka sallii lähtevät Internet-yhteys.  Tätä kokoonpanoa sovelluksen-palvelun-ympäristöä voi muodostaa yhteyden muiden Azure päätepisteet.

Tavallinen asiakkaan kokoonpano on kuitenkin määrittää omia oletusarvon reitin (0.0.0.0/0) joka pakottaa kulkevan sijaan paikallisen Internet-liikenteen.  Liikenne tämä työnkulku katkaisee sovelluksen palvelun ympäristöissä reaktioita, koska lähtevän tietoliikenteen on joko estettyjen paikallisessa tai NAT yksittäiselle osoitteet, jotka eivät enää toimi kanssa eri Azure päätepisteet tunnistamattomassa joukko.

Ratkaisu on määrittää aliverkon, joka sisältää sovelluksen Service-ympäristössä (vähintään yksi) käyttäjän määrittämät tiet (UDRs).  UDR määrittää aliverkon kielikohtaiset tiet, joka voi käyttää sen sijaan, että oletusarvoinen reitin.

Jos mahdollista kannattaa käyttää asetukset ovat seuraavat:

- ExpressRoute määritykset mainostaa 0.0.0.0/0 ja oletusarvoisesti voimassa tunneloi kaikki lähtevä liikenne paikalliseen.
- Käytetty sisältävän sovelluksen Service-ympäristön aliverkon UDR määrittää 0.0.0.0/0 seuraavan kohteen sisältötyyppi Internet (Tässä esimerkissä on kauemmaksi alaspäin ohjeaiheen).

Yhdistetyn vaikutus näin on, että aliverkon tason UDR ohittavat pakotettu ExpressRoute tunnelointi App Service-ympäristöstä varmistaa lähtevän Internet-yhteyttä.

> [AZURE.IMPORTANT] UDR **on** määritetty tiet olla tarpeeksi ohittavat kaikki tiet määrittämiisi ExpressRoute määrityksestä.  Alla olevassa esimerkissä käyttää laajan 0.0.0.0/0 osoitealueita ja sellaisenaan mahdollisesti voi vahingossa ohittaa reitti-ilmoituksissa käyttämällä tiettyihin osoitealueet.
>
>Sovelluksen palvelun ympäristöissä eivät ole tuettuja ExpressRoute määritykset, **mainostaa rajat ohjautuu julkiseen peering polusta yksityinen peering polku**.  ExpressRoute määritykset, joka on määritetty, julkisen peering saa reitti-ilmoituksissa Microsoft on suuri joukko Microsoft Azure IP-osoitealueet.  Jos nämä osoitealueet rajat määrittämiisi yksityinen peering polulla, lopputulos on se, että kaikki lähtevät verkkopaketit App palvelun ympäristön aliverkosta on voimassa tunneloidun asiakkaan paikalliseen verkkoon infrastruktuuria.  Verkko-työnkulku ei tällä hetkellä tueta App palvelun ympäristöjen kanssa.  Lopeta rajat mainonta ohjautuu julkiseen peering polusta yksityinen peering polku on yksi ratkaista tämän ongelman.

Käyttäjän määrittämät tiet taustatietoja on käytettävissä tässä [Yleiskatsaus][UDROverview].  

Luominen ja määrittäminen käyttäjän määrittämät tiet tiedot on käytettävissä [Käyttöoppaan][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Esimerkki UDR määritysten App Service-ympäristössä ##

**Vaatimukset**

1. Asentaa PowerShellin Azure [Azure-lataussivulta] [ AzureDownloads] (päivätty kesäkuussa 2015 tai uudempi).  "Komentorivin työkalujen" on "Windows PowerShellin", jotka asennetaan PowerShellin cmdlet-komennot "Asenna"-linkkiä.

2. On suositeltavaa, että yksilöllinen aliverkon luodaan käyttöönsä App Service-ympäristössä.  Näin varmistat, että käytetty aliverkon UDRs avautuu vain lähtevän tietoliikenteen App Service-ympäristössä.
3. **Tärkeää**: Älä ota vasta **sen jälkeen** seuraavat määritysvaiheet noudatetaan App Service-ympäristössä.  Näin varmistat, että lähtevän verkkoyhteys on käytettävissä, ennen kuin yrität asentaa sovelluksen Service-ympäristössä.

**Vaihe 1: Nimetyt reititystaulukon luominen**

Seuraavat koodikatkelman Luo nimeltä "DirectInternetRouteTable" Länsi US Azure-alueen reititystaulukon:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Vaihe 2: Luo vähintään yksi tiet reitin taulukkoon**

Tarvitset vähintään yksi tiet lisääminen reittitaulukkoon, jotta lähtevän Internet-yhteyttä.  

Suositeltu tapaa määrittämiseen lähtevä yhteys Internetiin on määrittäminen reitin 0.0.0.0/0 alla kuvatulla tavalla.
  
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Muista, että 0.0.0.0/0 on suurin piirtein osoitealueita ja sellaisenaan ohitetaan tiettyihin osoitealueet ilmoittaa ExpressRoute mukaan.  Uudelleen käydä aiemmissa suositus, UDR 0.0.0.0/0 reitin käytetään yhdessä ExressRoute-määritys, joka ilmoittaa vain 0.0.0.0/0 paikan päällä. 

Vaihtoehtoisesti voit ladata CIDR alueiden Azure käyttää täydellinen ja päivitettyjen luettelo.  Xml-tiedosto, joka sisältää kaikki Azure IP-osoitealueet on käytettävissä [Microsoft Download Centeristä][DownloadCenterAddressRanges].  

Huomaa kuitenkin, että nämä alueet muuttuvat ajan kuluessa, vuoksi käyttäjän määrittämät tiet synkronointiin säännöllisiä manuaalinen päivittäminen.  Myös, koska oletusarvoinen yläraja 100 tiet, valitse yksittäinen UDR, sinun on "yhteenveto" Azure-IP-osoitealueet sopimaan 100 reitin raja-pitää mielessä, että UDR määritetty tiet ei tarvitse olla tarkempi kuin tiet ilmoittaa oman ExpressRoute.  


**Vaihe 3: Liitä reititystaulukon sisältävä sovelluksen Service-ympäristön aliverkon**

Määritysten viimeinen vaihe on liitettävä aliverkon reititystaulukon kohtaa, johon sovelluksen Service-ympäristön otetaan käyttöön.  Seuraava komento liittää "DirectInternetRouteTable" "-ASESubnet", joka sisältää tekstin ympäristössä palvelun sovelluksen avulla.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Vaihe 4: Lopullinen vaiheet**

Kun reitti-taulukko on sidottu aliverkon, kannattaa ensin Testaa ja varmista vaikutus.  Esimerkiksi asentaa virtual machine aliverkon ja varmista, että:


- Lähtevän tietoliikenteen sekä Azure-Azure päätepisteiden mainittiin tämän artikkelin **ei** juoksutus ExpressRoute piiri alaspäin.  On erittäin tärkeää vahvistamiseksi ongelman, koska jos aliverkon lähtevän tietoliikenteen on edelleen pakotettu tunneloidun paikallisen-sovelluksen palvelun ympäristön luominen epäonnistuu aina. 
- DNS-hakuja varten päätepisteet mainittiin ovat kaikki ratkaiseminen oikein. 

Kun vahvistetaan edellä kuvatut toimet, sinun on Poista virtuaalikoneen, koska aliverkon on oltava "tyhjä" sovelluksen Service-ympäristö luodaan aikaan.
 
Valitse jatkaa ympäristössä palvelun sovelluksen luomisessa!

## <a name="getting-started"></a>Käytön aloittaminen
Kaikki artikkelit ja miten-, Muokkaa sovelluksen palvelun ympäristöissä ovat käytettävissä [sovelluksen palvelun ympäristöissä Lueminut-tiedosto](../app-service/app-service-app-service-environments-readme.md).

Aloita sovelluksen palvelun ympäristöissä artikkelissa [Johdanto App Service-ympäristöön][IntroToAppServiceEnvironment]

Saat lisätietoja Azure App Service-ympäristö [Azure App palvelun][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com
 

<!-- IMAGES -->
