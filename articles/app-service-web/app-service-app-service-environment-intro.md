<properties 
    pageTitle="Johdanto App Service-ympäristöön" 
    description="Lisätietoja sovelluksen palvelun ympäristön ominaisuus, joka on suojattu, VNet liittyneet, erillinen aikayksikön kaikki sovelluksesi suorittamista varten." 
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

# <a name="introduction-to-app-service-environment"></a>Johdanto App Service-ympäristöön

## <a name="overview"></a>Yleiskatsaus ##
Sovelluksen Service-ympäristössä on [Premium] [ PremiumTier] palvelun suunnitelman Azure-sovelluksen palvelun, joka tarjoaa täysin erillään ja erillinen ympäristössä käynnissä suojatusti Azure App palvelun sovellukset hyvin mittakaava, mukaan lukien [Web Apps]-vaihtoehdon[WebApps], [Mobile-sovellusten][MobileApps], ja [API sovellusten][APIApps].  

Sovelluksen palvelun ympäristöissä soveltuvat erinomaisesti edellyttävän sovelluksen toiminnoista:

- Erittäin suuri asteikko
- Eristystaso ja turvallinen Verkkokäyttö

Asiakkaat, voit luoda useita sovelluksen palvelun ympäristöissä yksittäisen Azure alueella ja useita Azure alueiden välillä.  Näin sovelluksen palvelun ympäristöissä erinomaisesti skaalauksen vaakasuunnassa tilan pienempi sovelluksen tasoa hyvin RPS toiminnoista, jotka tukevat.

Sovelluksen palvelun ympäristöissä eristetään vain yhteen asiakas-sovellusten ja ovat aina käyttöön virtual verkostoon.  Asiakkaiden on sekä saapuvan ja lähtevän tietoliikenteen tarkasti rajattuja hallintaoikeutta ja sovellusten muodostaa nopea suojattuja yhteyksiä paikallisen yrityksen resurssien virtual verkoissa.

Kaikki artikkelit ja miten-, Muokkaa tietoja sovelluksen palvelun ympäristöissä ovat käytettävissä [sovelluksen palvelun ympäristöissä Lueminut-tiedosto](../app-service/app-service-app-service-environments-readme.md).

Yleiskatsaus siitä, miten sovelluksen palvelun ympäristöissä käyttöön high asteikko ja sen suojaamista verkkoyhteys on artikkelissa [AzureCon perinpohjaisesti käsittelevään artikkeliin] [ AzureConDeepDive] -sovelluksen palvelun ympäristöissä!

Ohjaamme-skaalauksen vaakasuunnassa käyttämällä useita sovelluksen Service-ympäristössä artikkelista määrittäminen [geo distributed app vaatiman tallennustilan][GeodistributedAppFootprint].

Nähdäksesi AzureCon perinpohjaisesti käsittelevään artikkeliin näkyvät suojausarkkitehtuuri määrityksistä, on artikkelissa yksityiskohtaisista [tasoilla suojausarkkitehtuuri](app-service-app-service-environment-layered-security.md) App palvelun ympäristöjen kanssa.

Sovellukset App Service-ympäristössä käytössä voi olla heillä on käyttöoikeudet seuraavat laitteet, kuten web-sovelluksen palomuurit (WAF) kautta.  [Sovelluksen palvelun ympäristössä WAF](app-service-app-service-environment-web-application-firewall.md) määrittämisestä on artikkelissa kerrotaan tässä skenaariossa. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="dedicated-compute-resources"></a>Erillinen Laske-resurssit ##
Kaikki sovelluksen palvelun ympäristössä Laske-resurssit on varattu yksinomaan yhden tilauksen ja sovelluksen Service-ympäristössä voidaan määrittää enintään 50 (50) Laske resurssit käyttöönsä yhden sovelluksen.

Sovelluksen Service-ympäristössä koostuu edusta Laske resurssivarannon sekä 3 työntekijä Laske resurssin jakavat. 

Edusta ryhmä sisältää Laske resurssien vastuussa SSL-tilauksen kuin sekä automaattisen kuormituksen tasaamisen, sovelluspyyntöjen sovelluksen ympäristössä. 

Kunkin työntekijän resurssivarantoon sisältää Laske resursseja kohdistettu [App palvelun suunnitelmien][AppServicePlan], jotka sisältävät puolestaan vähintään yksi Azure App Service-sovellus.  Koska App palvelun ympäristössä voi olla jopa kolme eri työntekijä jakavat, voit halutessasi valita eri Laske kunkin työntekijän varannon resursseja.  

Esimerkiksi tämä avulla voit luoda yhden työntekijä resurssivarantoon heikompi suorittaminen tiedoilla, sovelluksen palvelun suunnitelmien tarkoitettu kehitys- tai sovellukset.  Toista (tai jopa kolmatta) työntekijä resurssivarantoon voi käyttää tehokkaampia Laske resurssien tarkoitettu App palvelun suunnitelmien käynnissä tuotannon sovellukset.

Lisätietoja edusta- ja työntekijä jakavat resursseista Laske määrä, katso, [miten voit määrittäminen sovelluksen Service-ympäristössä][HowToConfigureanAppServiceEnvironment].  

Lisätietoja käytettävissä olevista Laske resurssin koot tuettu App Service-ympäristössä, tarkista [App palvelun hinnat] [ AppServicePricing] sivulle ja tarkista käytettävissä olevat vaihtoehdot App palvelun ympäristössä Premiumin hinnat taso.

## <a name="virtual-network-support"></a>VPN-tuki ##
Sovelluksen Service-ympäristössä voidaan luoda- **joko** virtual Azure Resurssienhallinta-verkko- **tai** classic käyttöönoton malli VPN ([virtual verkoissa lisätietoja][MoreInfoOnVirtualNetworks]).  Koska virtual verkossa ja tarkemmin aliverkon virtual verkoston sisällä on aina sovelluksen Service-ympäristössä, voidaan hyödyntää suojaustoiminnot virtual verkostojen ohjaamaan sekä saapuva ja lähtevä tietoliikenne.  

Sovelluksen Service-ympäristössä voi olla joko Internet vastakkaisten julkiseen IP-osoite tai sisäinen vastakkaisten vain Azure sisäinen kuormituksen tasauspalvelun (ILB) osoitteen.

Voit käyttää [verkon käyttöoikeusryhmät] [ NetworkSecurityGroups] rajoittaa saapuvien verkkoyhteyksien aliverkon ympäristössä App palvelun sijainti.  Voit suorittaa takana seuraavat laitteet ja palveluihin web application palomuurien ja verkon SaaS tarjoajat sovellukset.

Sovellukset on usein myös käyttää yrityksen resurssit, kuten sisäinen tietokannat ja WWW-palvelut.  Yleisiä vaihtoehto on tekemäsi nämä päätepisteet ovat vain juoksutus Azure virtual verkon sisäinen verkkoliikennettä.  Kun sovellus Service-ympäristössä on liitetty virtual samassa verkossa kuin sisäiset palvelut, sovellukset ympäristössä, voit käyttää niitä myös päätepisteet tavoitettavissa [Sivuston sivuston] kautta[ SiteToSite] ja [Azure ExpressRoute] [ ExpressRoute] yhteydet.

Katso lisätietoja sovelluksen palvelun ympäristöissä käyttämisestä virtual verkkojen ja paikallisen verkkojen kanssa kuulla on seuraavissa artikkeleissa- [Verkoston arkkitehtuuri][NetworkArchitectureOverview], [Saapuvan liikenteen hallinta][ControllingInboundTraffic], ja [Liittämisestä turvallisesti Backends][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Käytön aloittaminen

Aloita sovelluksen Service-ympäristössä, katso [Siitä, miten voit luoda App palvelun ympäristössä][HowToCreateAnAppServiceEnvironment]

Kaikki artikkelit ja miten-, Muokkaa sovelluksen palvelun ympäristöissä ovat käytettävissä [sovelluksen palvelun ympäristöissä Lueminut-tiedosto](../app-service/app-service-app-service-environments-readme.md).

Saat lisätietoja Azure App Service-ympäristö [Azure App palvelun][AzureAppService].

Yleiskatsaus sovelluksen palvelun ympäristön verkoston arkkitehtuuri on artikkelissa [Verkoston arkkitehtuuri yleiskatsaus] [ NetworkArchitectureOverview] artikkelissa.

Lisätietoja ExpressRoute-ympäristössä palvelun sovelluksen käyttämisestä on seuraavassa artikkelissa [Express reitin]ja sovelluksen palvelun ympäristöissä[NetworkConfigDetailsForExpressRoute].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 
