<properties
    pageTitle="Azure AD-Cordova aloittaminen | Microsoft Azure"
    description="Miten voit luoda Cordova-sovellus, joka integroituu Azure AD kirjauduttaessa ja soittaa Azure AD suojattu ohjelmointirajapinnan käyttäminen OAuth."
    services="active-directory"
    documentationCenter=""
    authors="vibronet"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="vittorib"/>

# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Integroi Azure AD Apache Cordova app

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova avulla voit kehittää HTML5/JavaScript-sovellukset, jotka voidaan suorittaa mobiililaitteissa täydellisiksi alkuperäisen sovelluksina.
Azure AD-ja voit lisätä yrityksen arvosana todennus ominaisuuksia Cordova sovellukset. Rivitys Azure AD alkuperäisen SDK: T Android- ja Windows-kaupan Windows Phone-, iOS Cordova-laajennuksen ansiosta voit parantaa sovelluksen tuki Kirjaudu käyttäjien AD-tilien kanssa ja käyttää Office 365: ssä ja Azure API suojaa myös omia mukautettuja verkko-Ohjelmointirajapinnan puhelut.

Tässä opetusohjelmassa Käytämme Apache Cordova laajennus, Active Directory käyttöoikeuksien kirjaston (ADAL) voit parantaa yksinkertaisen sovelluksen seuraavia ominaisuuksia:

-   Koodin muutamalla viivoilla AD-käyttäjä todennetaan ja hanki soitettavien Azure AD-kaavio-Ohjelmointirajapinnan tunnuksen.
-   Kyseisen tunnuksen avulla voit käynnistää Graph-Ohjelmointirajapinnan kyselyn kansioon ja tulosten näyttäminen  
-   Hyödyntää ADAL suojaustunnuksen välimuistissa käyttäjän todennusta kysyy pienentämistä.

Jotta voit tehdä tämän, sinun on:

2. Voit rekisteröidä sovelluksen Azure AD
2. Koodin lisääminen sovelluksesi pyynnön tunnusten
3. Lisää käytettävä tunnuksen kyselyt Graph-Ohjelmointirajapinnan koodi ja tulosten esittämistä varten.
4. Luo Cordova käyttöönotto-projektin kaikissa ympäristöissä, jonka haluat kohdentaa ja Cordova ADAL laajennus ja esikatsella ratkaisun emulointeja.

## <a name="0--prerequisites"></a>*0. edellytykset*

Tässä opetusohjelmassa, sinun on suoritettava:

- Jos olet määrittänyt tilin sovelluksen kehittämisen oikeuksilla Azure AD-vuokraajan
- Määritetty käyttämään Apache Cordova kehitysympäristö  

Jos kumpikin on jo määritetty, siirry suoraan kohtaan vaiheessa 1.

Jos sinulla ei ole Azure AD-vuokraajan, voit etsiä [ohjeita siitä, miten voit hankkia sen tästä](active-directory-howto-tenant.md).

Jos sinulla ei ole Apache Cordova määrittäminen tietokoneeseen, asenna seuraavasti:

- [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [NodeJS](https://nodejs.org/download/)
- [Cordova CLI](https://cordova.apache.org/) (voit helposti asentaa kautta NPM pakettien hallinta: `npm install -g cordova`)

Huomaa, että ne toimivat PC ja Mac-tietokoneesta.

Kunkin kohdeympäristö on eri edellytykset.

- Muodosta ja suorita Windows: lehtiö-PC- tai Phone app-versio
    - [Visual Studio 2013 Windows Update-2 tai uudempi](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express tai Office-versio).
- Muodosta ja suorita iOS
    -   Xcode 5.x tai uudempi. Lataa http://developer.apple.com/downloads-tai [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12)
    -   [ios-sim](https://www.npmjs.org/package/ios-sim) – voit käynnistää iOS-sovellusten siirtäminen iOS Simulator komentoriviltä (voidaan helposti asentaa pääte kautta: `npm install -g ios-sim`)

- Voit luoda ja suorittaa Android-sovelluksen
    - Asenna [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) tai uudempi versio. Varmista, että `JAVA_HOME` (ympäristön muuttuva) on määritetty oikein JDK asennuspolku (esimerkiksi C:\Program Files\Java\jdk1.7.0_75) mukaan.
    - Asenna [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) ja lisää `<android-sdk-location>\tools` sijainti (esimerkiksi C:\tools\Android\android-sdk\tools) että `PATH` ympäristön muuttujan.
    - Avaa Android SDK hallinta (esimerkiksi päätteen kautta: `android`) ja asentaminen
    - *Android 5.0.1 (API-21)* platform SDK
    - *Android SDK luontityökalujen* versio 19.1.0 tai uudempi versio
    - *Android tuki säilöön* (Lisäominaisuudet)

  Android sdk ei ole mitään emulaattorin oletusesiintymää. Luo uusi suorittamalla `android avd` pääte ja valitsemalla sitten *Luo...* , jos haluat suorittaa Android-sovelluksen emulointiohjelma. Suositeltu *Api taso* on 19 tai uudempi versio, katso AVD hallinta (http://developer.android.com/tools/help/avd-manager.html) lisätietoja Android emulointikoneen ja luomisen asetukset.


## <a name="1--register-an-application-with-azure-ad"></a>*1. rekisteröidä Azure AD sovellus*

Huomautus: Tämä __vaihe on valinnainen__. Opetusohjelman annettu etukäteen valmistellun arvot, joiden avulla voit tarkastella käytössä otosten tekemättä, kaikki omat vuokraajan valmistelu. Kuitenkin on suositeltavaa, että suorita tätä vaihetta ja olet tutustunut prosessi, kun se on suoritettava kun luot omia sovelluksia.

Azure AD vain antaa tunnusten tunnetut sovelluksia. Ennen kuin voit käyttää Azure AD-sovellukset, sinun on luotava merkinnän sen vuokraajan.  Uusi sovellus rekisteröidään alihallintaan,

-   Kirjaudu sisään [Azure hallinta-portaalissa](https://manage.windowsazure.com)
-   Valitse vasemmalla olevasta siirtymisruudussa **Active Directory**
-   Valitse missä haluat rekisteröidä sovellus vuokraaja.
-   Valitse **sovellukset** -välilehti ja valitse **Lisää** ala-laatikon.
-   Seuraa ohjeita ja luo uusi **Alkuperäisen asiakassovellus** (huolimatta siitä, että Cordova sovellukset ovat HTML-pohjainen, luodaan tähän alkuperäisen asiakassovellus niin `Native Client Application` asetus on valittava, muutoin sovellus ei toimi).
    -   Sovelluksen **nimen** kuvataan sovelluksesi loppukäyttäjille
    -   **Ohjaa URI** on käytetty tunnusten palaa sovelluksen URI. Kirjoita `http://MyDirectorySearcherApp`.

Kun olet suorittanut rekisteröinti, AAD Määritä sovelluksen asiakkaan yksilöllinen tunnus.  Tarvitset tätä arvoa seuraavien osien: löydät sen juuri luomasi sovelluksen **määrittäminen** -välilehdessä.

Jotta voit suorittaa `DirSearchClient Sample`, myöntää juuri luomasi sovelluksen oikeuden kyselyn _Azure AD Graph API_:
-   **Määritä** -välilehden Etsi "Oikeudet ja muut sovellukset"-kohta.  Lisää "Azure Active Directory-sovellus **kirjautunut sisään käyttäjänä hakemiston** käyttöoikeus **Valtuutetun käyttöoikeudet**-kohdassa.  Tämä ottaa käyttöön sovelluksen käyttäjille Graph-Ohjelmointirajapinnan kysely.

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2. Kloonaa opetusohjelman vaaditaan otoksen app säilö*

Käyttöliittymän tai komentorivin Kirjoita seuraava komento:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3. Cordova-sovelluksen luominen*

On useita tapoja luoda Cordova sovellukset. Tässä opetusohjelmassa Käytämme Cordova komento rivin interface (CLI).
Käyttöliittymän tai komentoriviä Kirjoita seuraava komento:


     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

Joka luo kansiorakenne ja rakennustelineet Cordova projektille starter projektin sisällön kopioiminen www-alikansioon.
Siirrä uusi DirSearchClient-kansioon.

    cd .\DirSearchClient

Lisää whitelist-laajennus, tarvittavat Graph-Ohjelmointirajapinnan käynnistämiseen.

     cordova plugin add cordova-plugin-whitelist

Lisää seuraavaksi kaikki ympäristöjen haluat tukea. Tarvitset on työskentelyä otoksen, jotta voit suorittaa vähintään yksi alla olevia komentoja. Huomaa, että olet voi emuloida iOS Windows-tai Windows-tai Windows Phone-, Mac

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

Voit lisätä Cordova laajennuksen ADAL-projektin.

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3. todentaa käyttäjät ja hankkia tunnusten AAD-koodin lisääminen*

Sovellus on kehittäminen Tässä opetusohjelmassa toimi täydellisiä valmisteluja bone hakemisto-hakutoiminnon, jossa peruskäyttäjän tunnus kuka tahansa käyttäjä, kirjoita hakemiston ja visualisointi joitakin basic määritteitä.  Starter projektin sisältää perustiedot käyttöliittymän (-www/index.html) sovelluksen määrityksen ja rakennustelineet, basic app tapahtuman määrittäminen verkkokaapeleita käy käyttäjän käyttöliittymän sidontojen ja tulokset näytön funktioiden (www/js/index.js). Jättää pois, riittää on logiikan käyttöönoton tunnistetietojen tehtävät.

Sinun on suoritettava ensimmäinen vaihe on oma koodissa protokolla-arvoja, jotka käyttävät AAD sovelluksen ja kohdistamista resurssit. Nämä arvot käytetään muodostaa suojaustunnuksen pyynnöt myöhemmin. Lisää alapuolelle koodikatkelman ylimpänä index.js tiedoston.

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

`redirectUri` Ja `clientId` arvojen tulee vastata kuvaava AAD sovelluksen arvoja. Löydät ne määrittäminen-välilehdestä Azure-portaalissa ohjeiden vaiheessa 1 aiemmin tässä opetusohjelmassa mukaisesti.
Huomautus: Jos olet valinnut rekisteröitymisestä ei uuden sovelluksen oman alihallinnan, voit vain liittää esimääritettyjä arvojen perusteella sellaisenaan -, joka mahdollistaa Nähdäksesi malli on käytössä, mutta pitää aina luoda omia tapahtuma tarkoitettu tuotannon sovelluksia.

Seuraavaksi annettava Lisää Todelliset suojaustunnuksen pyyntö-koodi. Lisää seuraavat koodikatkelman välillä `search `ja `renderdata `määritykset.

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Tarkastellaan nyt, jotka toimivat jakamalla, kaksi pääosaa:.
Tässä esimerkissä on suunniteltu toimimaan minkä tahansa vuokraajan voi liittää tietyn sijasta. Tiedostossa käytetään "/ yleisiä" päätepiste, jonka avulla käyttäjä voi syöttää tilien todennus milloin ja ohjaa pyynnön vuokraajan se kuuluu.
Tämä menetelmä alkuosa tarkistaa, onko on jo tallennettu tunnus - ja jos on, käytetään alihallinnat, jotka se peräisin käynnistämisen uudelleen ADAL ADAL välimuistin. Tämä on tarpeen välttää ylimääräisiä kehotteita, "/ yleisiä" Käytä aina tuottaa käyttäjää pyydetään Lisää uusi tili.
```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Menetelmä toisen osan suorittaa ERISNIMI tokewn pyynnön.
`acquireTokenSilentAsync` Menetelmä pyytää ADAL palauttaa määritetyn resurssin tunnus näyttämättä minkä tahansa määritetyn UX Joka voi esiintyä, jos välimuisti on jo tallennettu sopivan käyttöoikeustietue vai onko päivitys-tunnuksen, jonka avulla voidaan saada uuden käyttöoikeustietue shwoing ilman mitään kehote.
Jos, yritys epäonnistuu, on oltava takaisin `acquireTokenAsync` -joka näkyvästi kysyy käyttäjältä tarkistamiseen.
```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Nyt, kun on tunnuksen, emme lopuksi käynnistää Graph-Ohjelmointirajapinnan ja suorittaa haluamme hakukyselyn. Lisää seuraavat koodikatkelman alapuolella `authenticate` määritys.

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Pisteen käynnistetään toimitetun barebone UX tekstikehyksen käyttäjän alias kirjoittamista varten. Tässä menetelmässä arvon voit luoda kyselyn, yhdistää käyttöoikeustietue, Lähetä se kaavio ja jäsentää tulokset. RenderData menetelmää jo ole ensimmäinen piste-tiedostossa on Haluatko esittää tulokset.

## <a name="4-run"></a>*4. Suorita*
Sovellus on lopuksi valmis suorittamaan! Liiketoiminnan se on erittäin yksinkertaisia: kun sovellus käynnistyy, kirjoita tekstiruutuun käyttäjän haluat hakea - ja napsauta sitten painiketta. Kysyy todennusta varten. Käyttöoikeuksien todennus ja onnistuneen haun näkyvät haettavissa käyttäjän määritteet. Seuraavien lohkot suorittaa haun näyttämättä minkä tahansa kehote aiemmin hankittu tunnuksen välimuistin tavoitettavuuden alkuun.
N sovellusta betonin vaiheet vaihtelevat-ympäristössä.

####<a name="windows-10"></a>Windows 10:

   Taulutietokoneiden /:`cordova run windows --archs=x64 -- --appx=uap`

   Matkapuhelin (edellyttää Windows10 mobiililaite PC yhteydessä):`cordova run windows --archs=arm -- --appx=uap --phone`

   __Huomautus__: sinulta pyydetään kirjautumaan developer käyttöoikeuden ensimmäisen käynnistyksen yhteydessä. Saat lisätietoja [Kehittäjä-käyttöoikeus](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-81-tabletpc"></a>Windows 8.1 ja Taulutietokoneiden:

   `cordova run windows`

   __Huomautus__: sinulta pyydetään kirjautumaan developer käyttöoikeuden ensimmäisen käynnistyksen yhteydessä. Saat lisätietoja [Kehittäjä-käyttöoikeus](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-phone-81"></a>Windows Phone 8.1:

   Voit suorittaa yhdistetyn laitteessa seuraavasti:`cordova run windows --device -- --phone`

   Voit suorittaa oletusarvon emulointiohjelma seuraavasti:`cordova emulate windows -- --phone`

   Käytä `cordova run windows --list -- --phone` saat näkyviin kaikki käytettävissä olevat kohteet ja `cordova run windows --target=<target_name> -- --phone` sovelluksen käyttämiseen laitetta tai emulaattorin (esimerkiksi `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

####<a name="android"></a>Android:

   Voit suorittaa yhdistetyn laitteessa seuraavasti:`cordova run android --device`

   Voit suorittaa oletusarvon emulointiohjelma seuraavasti:`cordova emulate android`

   __Huomautus__: Varmista, että olet luonut emulaattorin esiintymän *AVD hallinnan* avulla, kun se on osoittanut *edellytykset* -osassa.

   Käytä `cordova run android --list` saat näkyviin kaikki käytettävissä olevat kohteet ja `cordova run android --target=<target_name>` sovelluksen käyttämiseen laitetta tai emulaattorin (esimerkiksi `cordova run android --target="Nexus4_emulator"`).

####<a name="ios"></a>iOS:

   Voit suorittaa yhdistetyn laitteessa seuraavasti:`cordova run ios --device`

   Voit suorittaa oletusarvon emulointiohjelma seuraavasti:`cordova emulate ios`

   __Huomautus__: Varmista, että sinulla on `ios-sim` paketin emulointiohjelma asennusta. Katso lisätietoja *edellytykset* -osassa.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

Käytä `cordova run --help` on artikkelissa muita luominen ja suorittaminen asetukset.

Viittaus (ilman määritysarvot) Esimerkki valmiista toimitetaan [tähän](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).  Voit nyt siirtää edistynyt (ok, ja mielenkiintoisemmaksi) skenaarioita.  Voit halutessasi kokeilla:

[Suojatun Node.js verkko-Ohjelmointirajapinnan kanssa Azure AD >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
