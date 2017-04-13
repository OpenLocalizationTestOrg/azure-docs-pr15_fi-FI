<properties 
    pageTitle="Sovelluksen Service-ympäristön luomiseen" 
    description="Sovelluksen palvelun ympäristöissä luominen työnkulku kuvaus" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2016" 
    ms.author="ccompy"/>

# <a name="how-to-create-an-app-service-environment"></a>Sovelluksen Service-ympäristön luomiseen #

### <a name="overview"></a>Yleiskatsaus ###

Sovelluksen Service-ympäristössä (Ietokannan) ovat Premium service-vaihtoehto Azure App palvelun, joka tarjoaa parannetun asetusten-ominaisuus, joka ei ole käytettävissä usean vuokraajan niihin.  Ietokannan ominaisuus tunnistus Azure App palvelun olennaisesti asiakkaan virtual verkostoon.  Sovelluksen palvelun ympäristöissä lukea, [mitä sovelluksen Service-ympäristössä] tarjoamia ominaisuuksia suurempi ymmärtämisen saada[ WhatisASE] ohjeissa.

### <a name="before-you-create-your-ase"></a>Ennen kuin voit luoda oman Irjainkoko ###

On tärkeää tietää kohteita ei voi muuttaa.  Näitä ei voi muuttaa tietoja oman Ietokannan sen luomisen jälkeen ominaisuuksia ovat seuraavat:

- Sijainti
- Tilauksen
- Resurssiryhmä
- Käyttää VNet
- Käyttää aliverkon 
- Aliverkon kokoa

Valitsemalla VNet ja määrittämällä aliverkon, varmista, että se on riittävän suuri, minkä tahansa tulevaa tarvetta.  

### <a name="creating-an-app-service-environment"></a>Sovelluksen Service-ympäristössä luominen ###

Käyttää Ietokannan luominen Käyttöliittymän kahdella eri tavalla.  Voi olla löytyy hakemalla Azure Marketplace- ***sovelluksen Service*** -ympäristössä tai siirtymällä kautta uusi Web -> + Mobile -> App Service-ympäristössä.  Voit luoda Ietokannan seuraavasti:

1. Anna käyttäjän Ietokannan nimi.  Nimi, joka on määritetty Ietokannan käytetään luotu Ietokannan sovellukset.  Jos Ietokannan on appsvcenvdemo alitoimialueen nimen olisi. *appsvcenvdemo.p.azurewebsites.net*.  Jos loit sovelluksen nimeltä *mytestapp* , se on käytettävissä osoitteessa *mytestapp.appsvcenvdemo.p.azurewebsites.net*näin.  Kirjoita oman Ietokannan nimi ei voi käyttää tyhjää aluetta.  Jos käytät ISOLLA kirjoitetut merkit nimeä, toimialuenimi on samanniminen yhteensä pienikirjaiminen versio.  Jos käytössäsi ILB sitten Ietokannan nimi ei käytetä oman alitoimialue, mutta on sen sijaan määritetty tarkasti Ietokannan luonnin aikana

    ![][1]

2. Valitse tilauksen.  Oman Ietokannan käytettävä tilaus on myös sähköpostiosoitteen loppuosa kyseisen Ietokannan kaikkien sovellusten luodaan.  Et voi sijoittaa oman Ietokannan VNet, joka on toiseen tilaukseen

3. Valitse tai määritä uusi resurssiryhmä.  Oman Ietokannan käytettäviä resurssiryhmä on oltava sama, jota käytetään oman VNet.  Jos valitset olemassa VNet vastaavaksi oman VNet päivitetään oman Ietokannan resurssi-ryhmän valinta.

    ![][2]

4. Tee haluamasi valinnat VPN ja sijainti.  Voit luoda uuden VNet tai valitse olemassa VNet.  Jos valitset uuden VNet voit määrittää nimen ja sijainnin. Uusi VNet on osoite alueen 192.268.250.0/23 ja nimeltä **oletusarvon** , jotka on määritetty 192.168.250.0/24 aliverkon.  Voit valita myös yksinkertaisesti olemassa perinteinen tai resurssien hallinnan VNet.  VIP tyypin valinta määrittää, jos oman Ietokannan niitä voi käyttää suoraan Internetistä (ulkoinen) tai jos se on käytössä sisäinen kuormituksen tasauspalvelun (ILB).  Saat lisätietoja niitä lukea [millä sisäinen kuormituksen, sovellus-palvelun ympäristössä][ILBASE].  Jos valitset VIP tyypin ulkoisten voit valita kuinka monta ulkoisen IP-osoitteiden järjestelmä on luotu IPSSL tarkoituksiin.  Jos valitset sisäinen voit joutua määrittämään lisäämäsi alitoimialue oman Ietokannan käyttää.  ASEs voidaan ottaa käyttöön kyselyjä, jotka käyttävät *joko* julkinen osoite-alueiden *tai* RFC1918 osoite välilyöntejä (eli virtual verkoissa yksityisten osoitteiden).  Jotta voit käyttää virtual verkon julkinen osoite, sinun on etuajassa VNet luomiseen.  Kun valitset olemassa VNet sinun on luoda uusi aliverkon Ietokannan luonnin aikana.  **Et voi käyttää valmiiksi luotuja aliverkon portaalissa.  Voit luoda Ietokannan olemassa aliverkon kanssa, jos luot oman Ietokannan resurssien hallinnan mallin avulla.**  Ietokannan luominen mallin käyttäminen tietojen tässä [luominen sovelluksen Service-ympäristön mallista] [ ILBAseTemplate] ja tässä kohdassa [luominen mallista ILB sovelluksen palvelun ympäristön][ASEfromTemplate].

### <a name="details"></a>Tiedot ###

Ietokannan luodaan 2 etu päättyy ja 2 työntekijöiden kanssa.  Postikortin etupuoli lopettaa HTTP/HTTPS-päätepisteet edustajana ja liikenne lähettää työntekijöille, joka on roolia, jotka isännöivät sovelluksia.   Voit muuttaa määrä Ietokannan luonnin jälkeen ja voit myös näiden resurssien jakavat Automaattinen skaalaus sääntöjen määrittäminen.  Lisätietoja ympärille manuaalinen skaalaus hallintaan ja valvontaan App Service-ympäristössä, tutustu artikkeliin: [App palvelun ympäristön määrittäminen][ASEConfig] 

Vain yksi Ietokannan voidaan säilyttää Ietokannan käyttämä aliverkon.  Aliverkon ei voi käyttää mitään Ietokannan kuin

### <a name="after-app-service-environment-creation"></a>Sovelluksen palvelun ympäristön luomisen jälkeen ###

Ietokannan luomisen jälkeen voit muuttaa:

- Postikortin etupuoli päättyy määrä (vähintään: 2)
- Työntekijöiden määrä (vähintään: 2)
- IP-osoitteiden käytettävissä IP SSL määrä
- Laske resurssin koot eteen päättyy tai työntekijöiden käyttämän (edusta vähimmäiskoko on P2)


Lisätietoja manuaalinen skaalauksen, hallinta ja seuranta App palvelun ympäristöissä tähän ympärillä on: [App palvelun ympäristön määrittäminen][ASEConfig] 

Lisätietoja automaattisen skaalauksen poistaminen on tässä oppaassa: [Automaattinen skaalaus sovelluksen-palvelun ympäristön määrittäminen][ASEAutoscale]

On muita riippuvuuksia, jotka eivät ole käytettävissä, kuten tietokannan ja tallennustilaa mukauttamiseen.  Nämä ovat Azure käsittelemien ja järjestelmän mukana.  Järjestelmän tallennustila tukee enintään 500 Gigatavua koko sovelluksen Service-ympäristön ja tietokannan sovitetaan mukaan Azure tarpeen mukaan järjestelmä asteikon.


## <a name="getting-started"></a>Käytön aloittaminen
Kaikki artikkelit ja miten-, Muokkaa sovelluksen palvelun ympäristöissä ovat käytettävissä [sovelluksen palvelun ympäristöissä Lueminut-tiedosto](../app-service/app-service-app-service-environments-readme.md).

Aloita sovelluksen palvelun ympäristöissä, Lue lisää [ohjeaiheista johdanto App palvelun ympäristöissä][WhatisASE]

Saat lisätietoja Azure App Service-ympäristö [Azure App palvelun][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
