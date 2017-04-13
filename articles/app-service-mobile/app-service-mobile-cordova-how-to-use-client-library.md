<properties
    pageTitle="Opi käyttämään Apache Cordova laajennuksen for Azure-mobiilisovellukset"
    description="Opi käyttämään Apache Cordova laajennuksen for Azure-mobiilisovellukset"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Miten voit käyttää Apache Cordova asiakkaan kirjaston Azure Mobile-sovellukset

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Tämän oppaan avulla voit suorittaa yleisiä tilanteita, joissa uusimman [Apache Cordova ‑laajennuksen Azure Mobile-sovellusten]avulla. Jos ole ennen käyttänyt Azure Mobile-sovellusten, ensimmäinen valmis [Azure Mobile sovellusten Pika-aloitus] luominen taustassa-taulukon luominen ja lataa valmiita Apache Cordova projektin. Tässä oppaassa on keskittyä asiakkaan Apache Cordova laajennus.

## <a name="supported-platforms"></a>Tuetut käyttöympäristöt

Tämä SDK tukee Apache Cordova v6.0.0 ja uudempi iOS, Android ja Windows laitteet.  Käyttöympäristön tuki on seuraavanlainen:

* Android-Ohjelmointirajapinnan 19-24 (KitKat Nougat kautta)
* iOS 8.0 ja uudemmat versiot.
* Windows Phone 8.0
* Windows Phone 8.1
* Yleinen Windows-ympäristössä

##<a name="Setup"></a>Asennus- ja edellytykset

Tässä oppaassa oletetaan, että olet luonut taustassa taulukosta. Tässä oppaassa oletetaan, että taulukossa on samaan rakenteeseen taulukoina näiden Opetusohjelmissa. Myös tässä oppaassa oletetaan, että olet lisännyt koodin Apache Cordova laajennus.  Jos et ole vielä tehnyt sitä, voit lisätä Apache Cordova laajennus projektiin komentorivillä:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Lisätietoja [ensimmäisen Apache Cordova]-sovelluksen niiden ohjeissa.

[AZURE.INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]


##<a name="auth"></a>Toimintaohje: todentaa käyttäjät

Azure App palvelu tukee käyttöoikeudet ja myöntää sovelluksen käyttäjille käyttämällä eri ulkoisen tunnistetietojen toimittajat: Facebook, Google, Microsoft-Account ja Twitter. Voit määrittää käyttöoikeudet vain todennetut käyttäjät toiminnoista käytön taulukot. Voit käyttää myös todennettujen käyttäjien käyttäjätietojen toteuttavien palvelimen komentosarjojen käyttöoikeuksien myöntämissääntöjä. Lisätietoja on kohdassa [todentaminen käytön aloittaminen] -opetusohjelma.

Käytettäessä todennusta Apache Cordova sovelluksessa seuraavat Cordova-laajennukset on oltava käytettävissä:

* [cordova-laajennus-laite]
* [cordova-laajennus-inappbrowser]

Kaksi todennus kulkee tuetaan: server-kulun sekä asiakas-työnkulku.  Palvelin-työnkulku on helpoin todennus-toiminto, kun se on riippuvainen tarjoajan web todennus-liittymän. Asiakas-työnkulun avulla tarkempaa integroinnissa laitekohtaiset ominaisuuksia, kuten single-Sign, kun se on riippuvainen toimittajakohtaiset laitekohtaiset SDK: T.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Toimintaohje: Mobile sovelluksen-palvelun ulkoinen Uudelleenohjaus URL-osoitteiden määrittäminen.

Usean tyyppisiä Apache Cordova sovellusten silmukka-ominaisuuden avulla voit käsitellä OAuth-Käyttöliittymän kulkee.  Valitse localhost kulkee OAuth-Käyttöliittymän aiheuttaa ongelmia, koska Todentamispalvelu vain tietää, miten voit hyödyntää palvelun oletusarvoisesti.  Esimerkkejä ongelmallinen OAuth-Käyttöliittymän kulkee:

- Aaltoilu emulaattorin.
- Reaaliaikainen Lataa Ionic kanssa.
- Käytössä mobile Taustajärjestelmä paikallisesti
- Käynnissä mobile Taustajärjestelmä eri kuin yksi tarjoavat todennus Azure App palvelu.

Noudata seuraavia ohjeita voit lisätä paikallisten asetusten määritykset:

1. Kirjaudu sisään [Azure portal]
2. Valitse **kaikki resurssit** tai **Sovelluksen palvelujen** valitsemalla Mobile-sovelluksen nimi.
3. Valitse **Työkalut**
4. Valitse **resurssi explorer** KÄYTTÄJÄTIETUEISSA-valikossa ja valitse sitten **Siirry**.  Avaa uuden ikkunan tai välilehden.
5. Laajenna **config**, **authsettings** solmujen sivuston vasemmanpuoleisesta siirtymispalkista.
6. Valitse **Muokkaa**
7. Etsi "allowedExternalRedirectUrls"-elementti.  Se voi määrittää tyhjäarvon tai matriisin arvoista.  Muuta arvo seuraaviin arvoihin:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Korvaa URL-osoitteet palvelun URL-osoitteet.  Esimerkkejä tällaisista tiedostoista ovat "http://localhost:3000" (palvelulle Node.js esimerkki) tai "http://localhost:4400" (palvelulle aaltoilu).  Näitä URL-osoitteet on kuitenkin esimerkkejä - tilanteen mukaan lukien olevassa esimerkissä tarkoitetut palvelut saattavat olla erilaiset.
8. Napsauta näytön oikeassa yläkulmassa **Kirjoitus** -painiketta.
9. Valitse vihreä **valitseminen** -painiketta.

Tässä vaiheessa tallennetaan asetukset.  Sulje selaimen ikkunan, ennen kuin olet lopettanut asetusten tallentaminen.
Lisää nämä silmukan URL-osoitteet myös sovelluksen palvelun CORS asetuksia:

1. Kirjaudu sisään [Azure portal]
2. Valitse **kaikki resurssit** tai **Sovelluksen palvelujen** valitsemalla Mobile-sovelluksen nimi.
3. Asetukset-sivu avautuu automaattisesti.  Jos näin ei ole, valitse **Kaikkia asetuksia**.
4. Valitse **CORS** API-valikossa.
5. Anna URL-osoite, johon haluat lisätä ruutuun annettu ja paina Enter-näppäintä.
6. Kirjoita URL-osoitteita, tarpeen mukaan.
7. Valitse Tallenna asetukset **Tallenna** .

Kestää noin 10-15 sekunnin ajan uudet asetukset tulevat voimaan.

##<a name="register-for-push"></a>Toimintaohje: Rekisteröidy Push-ilmoitukset

Asenna [phonegap-laajennus-push] käsittelemään push-ilmoitukset.  Laajennus voi helposti lisätä avulla `cordova plugin add` komennon komentorivillä tai Visual Studion Git laajennuksen asennuksen kautta.  Seuraava koodi Apache Cordova sovelluksen Rekisteröi laitteesi push-ilmoitukset:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Ilmoitus keskittimet SDK avulla voit lähettää push-ilmoitukset-palvelimesta.  Älä lähetä push-ilmoitukset suoraan asiakkaille. Tällöin voidaan käyttää käynnistettävän palvelunestohyökkäykselle ilmoituksen keskittimet tai PNS vastaan.  PNS voi kieltää tietoliikenteestä tällaisten tuloksena.

<!-- URLs. -->
[Azure portal]: https://portal.azure.com
[Azure Mobile sovellusten Pika-aloitus]: app-service-mobile-cordova-get-started.md
[Käyttöoikeuksien käytön aloittaminen]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Apache Cordova laajennuksen for Azure-mobiilisovellukset]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[ensimmäinen Apache Cordova sovelluksen]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap-laajennus-push]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova-laajennus-laite]: https://www.npmjs.com/package/cordova-plugin-device
[cordova-laajennus-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
