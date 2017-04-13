<properties 
    pageTitle="Miten sovellus App Service-ympäristössä" 
    description="Sovelluksen skaalaus App Service-ympäristössä" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="ccompy"/>

# <a name="scaling-apps-in-an-app-service-environment"></a>Skaalauksen sovellukset App Service-ympäristössä #

Azure-sovelluksen-palvelussa liittyy voi skaalata tavallisesti kolme asiaa:

- hinnat suunnitelma
- työntekijän kokoa 
- esiintymien määrän.

Ietokannan on ei tarvita, voit valita tai muuttaa hinnoittelu suunnitelma.  Ominaisuuksista se on jo Premium, hinnat ominaisuuksien taso.  

Työntekijän kokoja, jotka koskevat Ietokannan järjestelmänvalvoja määrittää Laske resurssi, jota käytetään kunkin työntekijän resurssivarantoon kokoa.  Siis voi olla työntekijä resurssivarantoon 1 P4 suorittaminen tiedoilla ja P1 työntekijä resurssivarantoon 2 Laske resurssien tarvittaessa.  Hän ei ole koon järjestyksessä.  Saat tiedot koon sekä niiden hinnoittelu asiakirjan tässä artikkelissa [Azure App palvelun hinnat][AppServicePricing].  Tämä jättää web Apps-sovellusten ja sovelluksen palvelun suunnitelmien skaalausasetukset olevan sovelluksen Service-ympäristössä:

- työntekijän resurssivarantoon valinta
- kerrat

Muuttaminen joko kohteen tehdään näkyy, että Ietokannan isännöidään App palvelun suunnitelmien tarvittavat Käyttöliittymän avulla.  

![][1]

Ei skaalaudu määrittäminen ASP lisäksi käytettävissä Laske resurssien työntekijä sarjassa ASP oleva määrä.  Jos tarvitse laskea kyseisen työntekijän varannon resursseja haluat saada Ietokannan-järjestelmänvalvoja, voit lisätä ne.  Varten tietojen ympärillä määrittäminen uudelleen oman Ietokannan lukeminen tiedot tähän: [App Service-ympäristön määrittäminen][HowtoConfigureASE].  Voit myös käyttää Lisää kapasiteetin perusteella aikataulun tai arvot Ietokannan Automaattinen skaalaus-ominaisuuksia.  Saat lisätietoja määrittäminen Automaattinen skaalaus Ietokannan ympäristön itse Katso [määrittäminen Automaattinen skaalaus App Service-ympäristöön][ASEAutoscale].

Voit luoda useita sovelluksen palvelusopimusten vaihtoehdot käyttämällä Laske resurssien eri työntekijän jakavat tai voit käyttää samaa työntekijän resurssivarantoon.  Jos esimerkiksi (10) käytettävissä Laske resurssit ovat työntekijä resurssivarantoon 1, voit luoda yhden sovelluksen-palvelusopimus (6) Laske resurssien käyttäminen ja toinen sovellus-palvelun suunnittelu, joka käyttää (4) Laske resurssit.

### <a name="scaling-the-number-of-instances"></a>Skaalauksen esiintymien määrän ###

Kun luot web app sovelluksen Service-ympäristössä se käynnistyy 1 esiintymää.  Voit sitten skaalata voit uusia esiintymiä antamaan muita Laske resursseja, kun sovellus.   

Jos yhteyttä Ietokannan on riittävästi sitten tämä on melko helppoa.  Voit siirtyä oman sovelluksen palvelun suunnittelu, joka sisältää sivustoja haluat skaalata ja valitse asteikko.  Tämä avaa Käyttöliittymä, jossa voit manuaalisesti mittakaavan ASP tai Automaattinen skaalaus sääntöjen määrittäminen ASP.  Manuaalisesti näytöstä sovelluksen yksinkertaisesti Aseta ***mittakaavaksi mukaan*** ***esiintymä-määrä, jotka kirjoitan manuaalisesti***.  Tässä vetämällä liukusäädintä haluttu määrä tai kirjoita se liukusäätimen vieressä olevaan ruutuun.  

![][2] 

Automaattinen skaalaus säännöt Ietokannan ASP toimii samalla tavalla kuin normaalisti.  Voit valita ***Suorittimen prosentin*** ***mukaan*** skaalaus ja luo Automaattinen skaalaus säännöt suorittimen prosentteina ASP tai voit luoda monimutkaisia sääntöjä ***aikataulun ja suorituskyvyn sääntöjen***avulla.  Katso lisätietoja tiedot määrittämisestä oppaan Automaattinen skaalaus käyttäminen tähän [skaalata Azure App Service-sovelluksen][AppScale]. 


### <a name="worker-pool-selection"></a>Työntekijän resurssivarantoon valinta ###

Aiemmin mainittuja työntekijä resurssivarantoon-valinta on käytettävissä ASP-Käyttöliittymä.  Avaa sivu, jonka haluat skaalata ja valitse työntekijä vähäisen ASP.  Näet kaikki työntekijä-jakavat, jonka olet määrittänyt sovelluksen ympäristössäsi.  Jos käytössäsi on vain yksi työntekijä resurssivarantoon sitten vain näet yhteen resurssivarantoon luettelossa.  Mitä työntekijä resurssivarantoon ASP on muutettava valitset riittää, että haluat, että sovelluksen palvelun aiot siirtää työntekijä resurssivarantoon.  

![][3]

Ennen kuin ASP siirtäminen yhden työntekijä varannon toiseen on tärkeää varmistaa, että sinulla on riittävät kapasiteetin ASP.  Paitsi työntekijä sovellussarjan nimi näkyy työntekijä jakavat luettelo, mutta voit myös tarkistaa, kuinka monta työntekijää on käytettävissä työntekijä kyseiseen resurssivarantoon.  Varmista, että ole riittävästi käytettävissä sisältävät sovelluksen-palvelu-suunnitelma esiintymät.  Jos käytettävissäsi on enemmän Laske haluat siirtää työntekijä varannon resursseja, pyydä Ietokannan-järjestelmänvalvoja, voit lisätä ne.  

> [AZURE.NOTE] ASP-sovellusten siirtäminen yhden työntekijä varannon ASP aiheuttaa kiinnostunut käynnistyy.  Tämä voi aiheuttaa pyynnöt toimia hitaasti, sovelluksen on kylmän aloittaa uuden Laske resurssit.  Kylmän aloitus voidaan välttää [sovelluksen kiinnostunut ominaisuuksien määrittäminen] käyttämällä[ AppWarmup] Azure sovelluksen-palvelussa.  Sovelluksen alustus moduulin kuvattu artikkelissa toimii myös kylmän alkaa koska alustusprosessin myös käynnistyy, kun sovellukset on kylmän aloittaa uuden Laske resurssit. 

## <a name="getting-started"></a>Käytön aloittaminen

Aloita sovelluksen Service-ympäristössä, katso [Siitä, miten voit luoda App palvelun ympäristössä][HowtoCreateASE]

Saat lisätietoja Azure App Service-ympäristö [Azure App palvelun][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
