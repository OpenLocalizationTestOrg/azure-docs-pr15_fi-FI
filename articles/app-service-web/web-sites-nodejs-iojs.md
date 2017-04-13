<properties 
    pageTitle="Voit käyttää io.js Azure palvelun Web sovellukset" 
    description="Opettele käyttämään verkkosovellukseen Azure-sovelluksen palvelun io.js kanssa." 
    services="app-service\web" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016"
    ms.author="robmcm" />

# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>Voit käyttää io.js Azure palvelun Web sovellukset

Suositut solmu rinnakkainen [io.js] -välilehdessä eri erot Joyent's Node.js projektiin, mukaan lukien paremmin hallintomalli, nopeampi julkaisuprosessin ja nopeampi antamista uusien ja koe JavaScript-ominaisuuksia.

[Azure App palvelun](http://go.microsoft.com/fwlink/?LinkId=529714) verkkosovelluksissa ei esiasennettuna monta Node.js-versiota, se sallii myös käyttäjän antamaa Node.js binaariluvuksi. Tässä artikkelissa käsitellään io.js sovelluksen palvelun Web Apps-käytön aloittamiseen kahdella tavalla: laajennettu käyttöönotto-komentosarjan, joka määrittää automaattisesti Azure käyttämään käytettävissä io.js uusin sekä io.js, joka on binaarinen Manuaalinen lataaminen käyttö. 

<a id="deploymentscript"></a>
## <a name="using-a-deployment-script"></a>Käyttöönotto-komentosarjan avulla

Yhteydessä Node.js sovelluksen käyttöönotto-palvelun Web sovellukset suorittaa useita pieni komennot, joilla varmistetaan, että ympäristö on määritetty oikein. Käytä käyttöönoton komentosarja, tämä prosessi voi mukauttaa sisältämään lataaminen ja io.js määrittäminen.

[Io.js käyttöönoton komentosarja](https://github.com/felixrieseberg/iojs-azure) on käytettävissä GitHub. Jotta io.js web-sovellukseen, kopioida vain **.deployment**, **deploy.cmd** ja **IISNode.yml** sovelluksen-kansion ja ota käyttöön verkkosovelluksissa.  

Ensimmäisen tiedoston **.deployment**ohjaa Web Apps-sovellusten suorittamiseen **deploy.cmd** käyttöönoton yhteydessä. Tämä komentosarja suoritetaan tavallista vaiheet Node.js-sovelluksen, mutta lataa myös io.js uusimman version. Lopuksi **IISNode.yml** määrittää Web Apps-sovellusten, voit käyttää vain ladatut io.js, binaarinen esiasennettu Node.js binaarinen sijaan.

> [AZURE.NOTE] Päivitä käytetyt io.js binaarinen, käyttöön vain sovelluksesi - komentosarja Lataa io.js uuden version yksittäisen aina sovellus otetaan käyttöön.

<a id="manualinstallation"></a>
## <a name="using-manual-installation"></a>Manuaalinen asennus käyttäminen

Manuaalinen asennus mukautetun io.js-versio sisältää vain kaksi vaihetta. Lataa ensin **win x64** binaarinen suoraan [io.js jakauman]. Kaksi tiedostot - **iojs.exe** ja **iojs.lib**on pakollinen. Molempien tiedostojen on tallennettava koodiin, kuten **bin/iojs**-kansiossa.

Voit määrittää verkkosovelluksissa käytettävä **iojs.exe** esiasennettu solmu-version sijaan, **IISNode.yml** -tiedoston luominen sovelluksen ylimmällä tasolla ja lisää seuraava rivi.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>
## <a name="next-steps"></a>Seuraavat vaiheet

Tämän artikkelin oppinut käyttämään io.js kanssa App palvelun Web Apps-käyttämällä molemmat annettu käyttöönoton komentosarjoja sekä manuaalinen asennus. 

> [AZURE.NOTE] IO.js on paljon kehittämisen ja päivittää useammin kuin Node.js. Useita Node.js moduulit eivät ehkä toimi io.js - Ota consult [io.js GitHub-] vianmääritystä varten.

## <a name="whats-changed"></a>Mikä on muuttunut
* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

[IO.js]: https://iojs.org
[jakauman IO.js.]: https://iojs.org/dist/
[Valitse GitHub IO.js]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
 