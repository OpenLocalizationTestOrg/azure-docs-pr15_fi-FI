<properties 
    pageTitle="Sovelluksen palvelun ympäristön | Microsoft Azure" 
    description="Mikä on Azure App Service-ympäristössä? Johdanto App Service-ympäristöön." 
    keywords="Azure app service-ympäristössä virtual verkon suojatun verkko"
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

# <a name="app-service-environment-documentation"></a>Sovelluksen palvelun ympäristön ohjeet

Sovelluksen Service-ympäristössä on [Premium] [ PremiumTier] palvelun suunnitelman Azure-sovelluksen palvelun, joka tarjoaa täysin erillään ja erillinen ympäristössä käynnissä suojatusti Azure App palvelun sovellukset hyvin mittakaava, mukaan lukien [Web Apps]-vaihtoehdon[WebApps], [Mobile-sovellusten][MobileApps], ja [API sovellusten][APIApps].  

Sovelluksen palvelun ympäristöissä soveltuvat erinomaisesti edellyttävän sovelluksen toiminnoista:

- Erittäin suuri asteikko
- Eristystaso ja turvallinen Verkkokäyttö

Asiakkaat, voit luoda useita sovelluksen palvelun ympäristöissä yksittäisen Azure alueella ja useita Azure alueiden välillä.  Näin sovelluksen palvelun ympäristöissä erinomaisesti skaalauksen vaakasuunnassa tilan pienempi sovelluksen tasoa hyvin RPS toiminnoista, jotka tukevat.

Sovelluksen palvelun ympäristöissä eristetään vain yhteen asiakas-sovellusten ja ovat aina käyttöön virtual verkostoon.  Asiakkaiden on tarkasti rajattuja sekä saapuvan ja lähtevän tietoliikenteen [verkon suojausryhmien]avulla hallita[NetworkSecurityGroups].  Sovellusten voit myös luoda nopean suojattuja yhteyksiä virtual verkoissa paikallisen yrityksen resurssit.

Sovellukset on usein käyttää yrityksen resurssit, kuten sisäinen tietokannat ja WWW-palvelut.  Sovellukset App Service-ympäristössä käytössä voi käyttää resursseja voi käyttää [Sivuston sivuston] kautta[ SiteToSite] VPN ja [Azure ExpressRoute] [ ExpressRoute] yhteydet.

* [Mikä on sovelluksen Service-ympäristössä?](../app-service-web/app-service-app-service-environment-intro.md)
* [Sovelluksen Service-ympäristössä luominen](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Sovellusten luominen sovelluksen Service-ympäristössä](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Luominen ja käyttäminen sisäinen kuormituksen App palvelun ympäristöjen kanssa](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Sovelluksen Service-ympäristön määrittäminen](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Skaalaus sovellukset App Service-ympäristössä](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Verkkosuojaus ja arkkitehtuuri](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Miten 's

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]


## <a name="videos"></a>Videot
[AZURE.VIDEO azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps]

[AZURE.VIDEO microsoft-ignite-2015-running-enterprise-web-and-mobile-apps-on-azure-app-service]


<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
