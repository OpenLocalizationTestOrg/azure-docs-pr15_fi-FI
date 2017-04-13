<properties 
    pageTitle="Yhteyden suojatusti App palvelun ympäristön Taustajärjestelmä resurssit" 
    description="Lue lisää yhdistämisestä suojatusti Taustajärjestelmä resurssien App palvelun ympäristön." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>   

# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Yhteyden suojatusti App palvelun ympäristön Taustajärjestelmä resurssit #

## <a name="overview"></a>Yleiskatsaus ##
Koska App palvelun ympäristö luodaan aina- **joko** virtual Azure Resurssienhallinta-verkko- **tai** classic käyttöönoton malli [VPN][virtualnetwork], lähtevät yhteydet App palvelun ympäristön Taustajärjestelmä muita resursseja voi flow yksinomaan virtual verkon kautta.  Tehty kesäkuussa 2016 viimeaikaisesta muutoksesta kanssa ASEs myös voidaan ottaa huomioon virtual verkkoja, jotka käyttävät julkisen osoitealueet tai RFC1918 osoite välilyöntejä (eli yksityisten osoitteiden).  

Esimerkiksi voi olla käytössä näennäiskoneiden porttiin 1433 lukittu klusterin SQL Server.  Päätepisteen voi olla ACLd Salli vain saman virtual verkon käyttää muista resursseista.  

Toinen esimerkki luottamuksellisia päätepisteet voi suorittaa paikallisen ja voidaan yhdistää Azure toiseen [Sivustoon sivuston] kautta[ SiteToSite] tai [Azure ExpressRoute] [ ExpressRoute] yhteydet.  Tuloksena vain resurssien virtual verkkojen sivusto sivusto tai ExpressRoute tunneleita voivat käyttää paikallisen päätepisteet.

Kaikki nämä skenaariot sovellukset App Service-ympäristössä käytössä voi yhdistää suojatusti eri palvelimiin ja resursseja.  Lähtevän tietoliikenteen sovelluksista käynnissä yksityinen päätepisteet virtual samassa verkostossa App Service-ympäristössä (tai saman virtual verkkoyhteys) näkyy vain työnkulku virtual verkon kautta.  Yksityinen päätepisteet liikenteen ei ohjaa julkisen Internetin välityksellä.

Yksi caveat koskee lähtevän tietoliikenteen App palvelun ympäristön päätepisteiden virtual verkossa.  Sovelluksen palvelun ympäristöissä saavuta päätepisteet näennäiskoneiden sijaitsevat **saman** aliverkon kuin sovelluksen Service-ympäristössä.  Tämä yleensä tulee ongelma, kunhan App palvelun ympäristöissä on otettu käyttöön varattu käyttöönsä App palvelun ympäristön aliverkon kyselyjä.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="outbound-connectivity-and-dns-requirements"></a>Lähtevien yhteyksien ja DNS-vaatimukset ##
Sovelluksen palvelun ympäristöön toimii oikein se vaatii eri päätepisteet lähtevän pääsyä. Täydellinen luettelo Ietokannan käyttämän ulkoisen päätepisteet on [Verkon määritysten ExpressRoute varten](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) on artikkelissa "Pakollinen verkkoyhteyden"-osassa.

Sovelluksen palvelun ympäristöissä edellyttävät kelvollinen DNS-infrastruktuurin määritetty virtual verkkoon.  Jos DNS-määrityksiä muutetaan jostakin syystä ympäristössä palvelun sovelluksen luomisen jälkeen, kehittäjät voit pakottaa sovelluksen palvelun ympäristössä, jos haluat jatkaa uuden DNS-määrityksiä.  Käynnistävä juokseva ympäristön uudelleenkäynnistyksen "Uudelleenkäynnistys" kuvakkeella portaalissa App palvelun ympäristön hallinta-sivu yläosassa aiheuttaa ympäristön Nosta uusi DNS-määrityksiä.

On myös suositeltavaa, että kaikki mukautetut DNS-palvelimet vnet määritettävä etuajassa ennen luominen sovelluksen Service-ympäristössä.  Jos virtual verkon DNS-määritys muutetaan kesken App palvelun ympäristö luodaan, joka johtaa sovelluksen palvelun ympäristön luominen prosessin epäonnistumista.  Vastaavat alueilla: Jos mukautettujen DNS-palvelin on olemassa VPN-yhdyskäytävän toinen pää- ja DNS-palvelin ei ole käytettävissä tai poissa käytöstä, sovelluksen palvelun ympäristön luontiprosessi myös epäonnistuu.

## <a name="connecting-to-a-sql-server"></a>Yhteyden muodostaminen SQL Server
Yleisiä SQL Server-määritys on päätepisteen kuuntele porttia 1433:

![SQL Server-palvelimen päätepisteen][SqlServerEndpoint]

Käytettävissä on kaksi tapaa rajoittaa liikenne tämän päätepisteen:


- [Verkon käyttöoikeusluettelot] [ NetworkAccessControlLists] (verkon käyttöoikeusluettelot)

- [Verkon käyttöoikeusryhmät][NetworkSecurityGroups]


## <a name="restricting-access-with-a-network-acl"></a>Verkon Käyttöoikeusluettelon käytön rajoittaminen

Porttia 1433 voidaan suojata verkon käyttöoikeusluettelo.  Whitelists asiakkaan alla olevassa esimerkissä osoitteet sisältyy virtual verkon peräisin ja estää pääsyn kaikki asiakkaat.

![Esimerkki verkon käytön hallinta-luettelosta][NetworkAccessControlListExample]

Kaikki sovellukset App Service-ympäristössä käytössä virtual samassa verkossa kuin SQL Server voivat muodostaa yhteyden SQL Server-esiintymän käyttämällä SQL Server-virtuaalikoneen **VNet sisäinen** IP-osoite.  

Alla olevassa esimerkissä yhteysmerkkijonon viittaa SQL Server-tietokantaan sen yksityinen IP-osoite.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Vaikka virtuaalikoneen on sekä julkinen päätepisteen, yhteys yritykset julkiseen IP-osoitetta hylätään verkon Käyttöoikeusluettelon vuoksi. 

## <a name="restricting-access-with-a-network-security-group"></a>Käyttöoikeusryhmän verkon käytön rajoittaminen
Vaihtoehtoinen menetelmä, käytön on verkon käyttöoikeusryhmän.  Verkon käyttöoikeusryhmät voi suojata yksittäisen näennäiskoneiden tai aliverkon, joka sisältää näennäiskoneiden.

Verkon käyttöoikeusryhmän on luotava ensin:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Käytön rajoittaminen vain VNet sisäisen tietoliikenteen on kuvattu yksinkertainen verkon käyttöoikeusryhmän kanssa.  Verkon käyttöoikeusryhmän oletusarvon säännöt Salli käyttää vain muiden verkkoasiakkaiden virtual samassa verkostossa.

Tuloksena lukitus yhteyden SQL Server on pelkästään verkon käyttöoikeusryhmän sen oletusarvo sääntöjen soveltaminen joko näennäiskoneiden SQL Server tai joka sisältää näennäiskoneiden aliverkon.

Alla esimerkki koskee verkon käyttöoikeusryhmän sisältävän aliverkon:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
Lopputulos on joukko suojaussäännöt, jotka estävät ulkoisen käytön, samalla kun VNet sisäiseen käyttöön:

![Oletusarvoinen verkon suojaussäännöt][DefaultNetworkSecurityRules]


## <a name="getting-started"></a>Käytön aloittaminen
Kaikki artikkelit ja miten-, Muokkaa sovelluksen palvelun ympäristöissä ovat käytettävissä [sovelluksen palvelun ympäristöissä Lueminut-tiedosto](../app-service/app-service-app-service-environments-readme.md).

Aloita sovelluksen palvelun ympäristöissä artikkelissa [Johdanto App Service-ympäristöön][IntroToAppServiceEnvironment]

Lisätietoja ympärille App Service-ympäristöön saapuvan liikenteen hallinta on artikkelissa [sovelluksen palvelun ympäristöön saapuvan liikenteen hallinta][ControlInboundASE]

Saat lisätietoja Azure App Service-ympäristö [Azure App palvelun][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
