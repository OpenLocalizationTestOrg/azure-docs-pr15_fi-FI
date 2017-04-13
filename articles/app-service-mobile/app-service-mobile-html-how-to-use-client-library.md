<properties
    pageTitle="Opi käyttämään JavaScript-SDK for Azure-mobiilisovellukset"
    description="Miten Azure Mobile-sovellusten käyttö v"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>Miten voit käyttää JavaScript-asiakas-kirjaston Azure Mobile-sovellukset

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Tämän oppaan avulla voit suorittaa yleisiä tilanteita, joissa uusimman [JavaScript SDK Azure Mobile-sovellusten]avulla. Jos ole aiemmin käyttänyt Azure Mobile-sovellusten, suorita ensin taustassa ja taulukon luominen [Azure Mobile sovellusten Pika-aloitus] . Tässä oppaassa on keskitytään mobile Taustajärjestelmä käyttäminen HTML/JavaScript-verkkosovelluksia.

## <a name="supported-platforms"></a>Tuetut käyttöympäristöt

Olemme rajoittaa selaimen tuen pää selaimet nykyisen ja edellisen versioita: Google Chrome, Microsoft Edge Microsoft Internet Explorerin tai Mozilla Firefox.  Odotamme SDK-funktion suhteellisen Moderni selaimen kanssa.

Paketin on jaettu yleinen JavaScript-moduulissa, jotta se tukee globals AMD- ja CommonJS muodot.

##<a name="Setup"></a>Asennus- ja edellytykset

Tässä oppaassa oletetaan, että olet luonut taustassa taulukosta. Tässä oppaassa oletetaan, että taulukossa on samaan rakenteeseen taulukoina näiden Opetusohjelmissa.

Asentaminen Azure Mobile sovellusten JavaScript-SDK voidaan toteuttaa kautta `npm` komento:

```
npm install azure-mobile-apps-client --save
```

Kun asennettu, kirjasto sijaitsee `node_modules/azure-mobile-apps-client/dist/MobileServices.Web.min.js`.  Kopioi tämä tiedosto web-alueelle.

```
<script src="path/to/MobileServices.Web.min.js"></script>
```

Kirjaston voi käyttää myös ES2015-moduulissa sisällä CommonJS ympäristöissä, kuten Browserify ja Webpack ja AMD-kirjasto.  Esimerkki:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

[AZURE.INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

##<a name="auth"></a>Toimintaohje: todentaa käyttäjät

Azure App palvelu tukee käyttöoikeudet ja myöntää sovelluksen käyttäjille käyttämällä eri ulkoisen tunnistetietojen toimittajat: Facebook, Google, Microsoft-Account ja Twitter. Voit määrittää käyttöoikeudet vain todennetut käyttäjät toiminnoista käytön taulukot. Voit käyttää myös todennettujen käyttäjien käyttäjätietojen toteuttavien palvelimen komentosarjojen käyttöoikeuksien myöntämissääntöjä. Lisätietoja on kohdassa [todentaminen käytön aloittaminen] -opetusohjelma.

Kaksi todennus kulkee tuetaan: server-kulun sekä asiakas-työnkulku.  Palvelin-työnkulku on helpoin todennus-toiminto, kun se on riippuvainen tarjoajan web todennus-liittymän. Asiakas-työnkulun avulla tarkempaa integroinnissa laitekohtaiset ominaisuuksia, kuten single-Sign, kun se on riippuvainen toimittajakohtaiset SDK: T.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Toimintaohje: Mobile sovelluksen-palvelun ulkoinen Uudelleenohjaus URL-osoitteiden määrittäminen.

JavaScript-sovellusten erityyppisiä silmukka-ominaisuuden avulla voit käsitellä OAuth-Käyttöliittymän kulkee.  Nämä ominaisuudet ovat seuraavat:

* Käytössä palvelun paikallisesti
* Live Lataa käyttäminen Ionic Framework
* Uudelleenohjausta App palveluun todennusta varten. 

Käytössä paikallisesti saattaa aiheuttaa ongelmia, koska oletusarvoisesti App todentaminen vain määritetty sallimaan käytöltä Mobile-sovelluksen Taustajärjestelmä. Seuraavien vaiheiden avulla voit ottaa käyttöön suoritettaessa paikallisesti palvelimen todennuksen App Service-asetusten muuttaminen:

1. Kirjaudu sisään [Azure portal]
2. Siirry Mobile-sovelluksen Taustajärjestelmä.
3. Valitse **KEHITYSTYÖKALUT** -valikosta **resurssin explorer** .
4. Valitse resurssi-explorer Mobile-sovelluksen Taustajärjestelmä avaaminen uusi välilehti tai ikkuna, **Siirry** .
5. Laajenna **config** > **authsettings** solmu, kun sovellus.
6. Resurssin muokattavaksi valitsemalla **Muokkaa** .
7. Etsi **allowedExternalRedirectUrls** -elementin, joiden pitäisi olla tyhjä. Lisää URL-osoitteisiin matriisin:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Korvaa URL-osoitteet matriisista palvelua, joka tässä esimerkissä on URL-osoitteet `http://localhost:3000` paikallisen Node.js otoksen palvelulle. Voit myös käyttää `http://localhost:4400` aaltoilu-palvelun tai joitakin muita URL-sovelluksen määrityksistä riippuen.

8. Sivun yläreunassa **Luku-/ kirjoitusoikeudet**valitse sitten Tallenna päivitykset **valitseminen** .

Sinun on myös lisättävä saman silmukan URL-osoitteet CORS whitelist asetukset:

1. Siirry takaisin [Azure portal].
2. Siirry Mobile-sovelluksen Taustajärjestelmä.
3. Valitse **CORS** **API** -valikossa.
4. Kirjoita URL-Osoitteen **Sallittu alkuperän** tyhjään tekstiruutuun.  Luo uusi tekstiruutu.
5. Valitse **Tallenna**
    
Kun taustaan päivittää osaat käyttää uuden silmukan URL-sovelluksen.

<!-- URLs. -->
[Azure Mobile sovellusten Pika-aloitus]: app-service-mobile-cordova-get-started.md
[Käyttöoikeuksien käytön aloittaminen]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure portal]: https://portal.azure.com/
[JavaScript-SDK for Azure-mobiilisovellukset]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx

