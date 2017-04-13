<properties 
    pageTitle="Web Application palomuurin (WAF) määrittäminen sovelluksen Service-ympäristössä" 
    description="Opettele määrittämään web sovelluksen palomuuri eteen App Service-ympäristössä." 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="naziml"/>    

# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Web Application palomuurin (WAF) määrittäminen sovelluksen Service-ympäristössä

## <a name="overview"></a>Yleiskatsaus ##
Web-sovelluksen palomuurit, kuten [Barracuda WAF Azure varten](https://www.barracuda.com/programs/azure) , joka on suojattu web-sovellusten poistamiseksi saapuva web-liikenne paikalliseen estä SQL kertaa, sivustojenvälisen komentosarjan, haittaohjelmia latauksia ja sovelluksen WWW.microsoft.com-sivustoa kohtaan ja muut kalastelu [Azure Marketplacesta](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) auttaa käytettävissä. Se myös tarkastaa taustatietokantaan WWW-palvelimien vastaukset tietojen menetyksen estäminen (DLP). Yhdistää eristystaso ja muita skaalaus myöntämä App palvelun ympäristöissä, tämä on paras mahdollinen ympäristö host business kriittinen web sovelluksia, jotka kestävät haittaohjelmien pyyntöjä ja määrää liikenne on.

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Asetukset ##
Olemme määrittää asiakirjan Microsoftin App palvelun ympäristön takana useita kuormituksen saapuva Barracuda WAF esiintymät, niin, että vain WAF tietoliikenteen kätevästi ottaa sovelluksen Service-ympäristön ja ei voi käyttää Jos kyseessä. On myös on Azure liikenteen hallinta eteen lataamaan saldo Azure tietojen keskikohdan mukaan- ja alueiden välillä sekä Barracuda WAF esiintymät. Korkean tason kaavion määritykset on tehty näyttäisi samanlaiselta kuin mitä näkyy alla.

![Arkkitehtuuri][Architecture] 

> Huomautus: Myötä [ILB tukea sovelluksen Service-ympäristössä](app-service-environment-with-internal-load-balancer.md), voit määrittää ei ole käytettävissä DMZ ja vain mahdollista yksityisverkon Ietokannan. 

## <a name="configuring-your-app-service-environment"></a>Sovelluksen palvelun ympäristön määrittäminen ##
Voit määrittää sovelluksen Service-ympäristössä käyttämisestä [sekä ohjeet](app-service-web-how-to-create-an-app-service-environment.md) aiheesta. Kun sovellus palvelun ympäristö luodaan, voit luoda [Web Apps -sovelluksista](app-service-web-overview.md), [API-sovellukset](../app-service-api/app-service-api-apps-why-best-platform.md) ja [Mobile-sovellusten](../app-service-mobile/app-service-mobile-value-prop.md) tämän ympäristössä, joka kaikki suojataan takana on määritettävä seuraavan osion WAF.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Barracuda WAF Cloud-palvelun määrittäminen ##
Barracuda on [yksityiskohtaiset artikkelissa](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) sen WAF virtual Azure-koneeseen ottamisesta käyttöön. Mutta koska kopiointia haluat redundancy ja sisältämiin yksittäisen kohdan virhe, haluat ottaa käyttöön vähintään 2 WAF esiintymän VMs saman pilvipalvelussa, kun seuraamalla näitä ohjeita.

### <a name="adding-endpoints-to-cloud-service"></a>Päätepisteet cloud palvelun lisääminen ###
Kun 2 tai WAF AM esiintymiä on pilvipalvelussa sijaitsevaan voit [Azure portal](https://portal.azure.com/) Lisää HTTP- ja HTTPS päätepisteet, joita käytetään sovelluksen alla olevassa kuvassa esitetyllä tavalla.

![Määritä päätepisteen][ConfigureEndpoint]

Jos sovellus käyttää muita päätepisteet, varmista, että voit lisätä ne tähän luetteloon. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Barracuda WAF kautta sen hallinta-portaalin määrittäminen ###
Barracuda WAF käyttää TCP-portin 8000 määritysten hallinta-portaalin sen kautta. Koska on WAF VMs, toista nämä ohjeet AM jokaiselle esiintymälle on useita kertoja. 


> Huomautus: Kun olet saanut WAF kokoonpanossa, poista TCP/8000 päätepisteen kaikki oman WAF VMs voit suojata oman WAF.

Lisää hallinta päätepiste, voit määrittää Barracuda WAF alla olevassa kuvassa esitetyllä tavalla.

![Lisää hallinta päätepiste][AddManagementEndpoint]
 
Siirry pilvipalvelussa sijaitsevaan hallinta päätepiste selaimen avulla. Pilvipalvelussa sijaitsevaan kutsutaan test.cloudapp.net, jos käyttää tämän päätepisteen http://test.cloudapp.net:8000 selaamalla. Näkyviin tulee kirjautumissivun, kuten alla, että kirjaudut sisään käyttämällä WAF AM määritys vaiheessa määritetyt tunnistetiedot.

![Kirjaudu sisään hallintasivu][ManagementLoginPage]

Kerran voit kirjautuminen pitäisi näkyä Raporttinäkymät-ikkunan alla olevassa kuvassa jota, jotka näyttävät WAF suojauksen basic tilastoja.

![Hallinnan Raporttinäkymää][ManagementDashboard]

Valitsemalla palvelut-välilehden avulla voit määrittää oman WAF se suojaa Services. Lisätietoja määrittäminen Barracuda WAF tietojen kysyminen [niiden ohjeissa](https://techlib.barracuda.com/waf/getstarted1). Azure Web App-sovelluksessa alla olevassa esimerkissä käsittelevä liikenne HTTP- ja HTTPS on määritetty.

![Hallinnan Lisää palveluita][ManagementAddServices]

> Huomautus: Sen mukaan, miten sovellukset on määritetty ja mitä ominaisuuksia käytetään sovelluksen ympäristössäsi, sinun on edelleenlähetys liikenne TCP portit 80 ja 443, esimerkiksi jos käytössäsi on verkkosovellukseen IP SSL-asetukset. Verkkoportit, joita käytetään sovelluksen palvelun ympäristöissä luetteloon Tutustu [ohjausobjektin saapuva liikenne dokumentaatio](app-service-app-service-environment-control-inbound-traffic.md) verkon portit-osassa.

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Microsoft Azure liikenteen hallinta (valinnainen) määrittäminen ##
Jos sovellus on käytettävissä useita alueilla kannattaa ladata saldo ne takana [Azure liikenteen hallinta](../traffic-manager/traffic-manager-overview.md). Tarvittaessa voit lisätä päätepisteen [Azure perinteinen portal](https://manage.azure.com) käyttämällä omaa WAF pilvipalvelussa nimi liikenteen hallinta profiilin alla olevassa kuvassa esitetyllä tavalla. 

![Päätepisteen liikenteen hallinta][TrafficManagerEndpoint]

Jos sovellus edellyttää todennusta, varmista sinulla on joitakin resurssi, joka ei edellytä mitään todennusta varten liikenteen hallinta ping-sovelluksen käytettävyyden. Voit määrittää URL-Osoitteen määrittäminen-osassa [Azure perinteinen portal](https://manage.azure.com) alla kuvatulla tavalla.

![Määritä liikenteen hallinta][ConfigureTrafficManager]

Voit lähettää liikenteen hallinta Testaa yhteyttä WAF-sovelluksen, joudut määritys sivuston käännökset Barracuda WAF-liikenne lähettää edelleen sovelluksesi, kuten alla olevassa esimerkissä.

![Sivuston käännökset][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Suojaaminen liikenne App Service-ympäristöön verkon suojausryhmien (NSG) avulla##
Noudata on [ohjausobjektin saapuva liikenne dokumentaatio](app-service-app-service-environment-control-inbound-traffic.md) estäminen liikenne App Service-ympäristöön WAF vain käyttämällä pilvipalvelussa sijaitsevaan VIP-osoitteeseen. Tässä on esimerkki Powershell-komentoa, saat tästä tehtävästä portin TCP 80.


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Korvaa SourceAddressPrefix oman WAF pilvipalvelussa Virtual IP-osoite (VIP).

> Huomautus: Pilvipalvelussa sijaitsevaan VIP muuttuvat, kun poistat ja luo uudelleen pilvipalvelussa. Varmista, että päivitettävä verkon resurssiryhmä IP-osoite, kun olet tehnyt. 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
