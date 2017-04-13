<properties
    pageTitle="Azure Mobile välitys Web SDK-integroinnin | Microsoft Azure"
    description="Uusimmat päivitykset ja Azure Mobile välitys Web SDK ohjeet"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="02/29/2016"
    ms.author="piyushjo" />

#<a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Integroi Azure Mobile välitys WWW-sovelluksessa

> [AZURE.SELECTOR]
- [Windowsin Universal](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-laitteeseen](mobile-engagement-android-integrate-engagement.md)

Tässä artikkelissa kuvataan, helpoin tapa Aktivoi analytics ja seurantaa funktioiden Azure Mobile välitys WWW-sovelluksessa.

Aktivoi log-raportteja, jotka tarvitaan laskemiseen kaikki käyttäjät, istuntojen, aktiviteetteja, kaatuu ja technicals tilastotietoja ohjeiden mukaisesti. Sovelluksen riippuva tilastotietoja, kuten tapahtumia, virheitä ja töitä sinun on aktivoitava rajaaminen manuaalisesti Azure Mobile välitys Ohjelmointirajapinnan avulla. Lisätietoja on tietoja [Lisäasetukset Mobile-välitys tunnisteita Ohjelmointirajapinnan web-sovelluksen käyttämisestä](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Johdanto

[Lataa Azure Mobile välitys Web SDK](http://aka.ms/P7b453).
Mobile välitys Web SDK toimitetaan yhden JavaScript-tiedostona, azure engagement.js, joilla sivuston tai web-sovelluksen jokaiselle sivulle viittaavan.

> [AZURE.IMPORTANT] Ennen kuin suoritat tämän komentosarjan, sinun on suoritettava komentosarja tai koodia koodikatkelman, voit kirjoittaa Mobile välitys määrittämiseksi sovelluksen.

## <a name="browser-compatibility"></a>Selaimen yhteensopivuus

Mobile välitys Web SDK käytetään alkuperäisen JSON koodaus ja koodauksen lisäksi toimialueelta AJAX-pyyntöjen (käyttäisit W3C CORS-määritys). On yhteensopiva seuraavissa selaimissa:

* Microsoft Edge 12 +
* Internet Explorer 10 +
* Firefox 3.5 +
* Chrome 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Määritä Mobile välitys

Komentosarja, joka luo yleisenä `azureEngagement` JavaScript-objektin, kuten seuraavassa esimerkissä. Koska sivuston saattavat olla Kerrannaiset sivuja, tässä esimerkissä oletetaan, että tämä komentosarja sisältyy jokaiselle sivulle. Tässä esimerkissä JavaScript-objekti on nimeltä `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

`connectionString` Arvo sovelluksen näkyy Azure-portaalissa.

> [AZURE.NOTE] `appVersionName`ja `appVersionCode` ovat valinnaisia. Suosittelemme, että määrität ne niin, että analytics voit käsitellä versiotiedot.

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Sisällytä Mobile välitys komentosarjojen sivut
Lisää sivujen Mobile välitys komentosarjoja jollakin seuraavista tavoista:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Tai tältä:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias

Kun Mobile välitys Web SDK-komentosarja on ladattu, luo **välitys** alias käyttämään SDK-ohjelmointirajapinnan. Et voi käyttää tätä alias määrittämään SDK-määritys. Tämä alias käytetään viitteenä näitä ohjeita.

Huomaa, että jos oletusarvo-alias menee päällekkäin jonkin toisen Yleinen muuttuja-sivulta kanssa, voit muuttaa sen määritystä seuraavasti ennen kuin olet ladannut Mobile välitys Web SDK:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Tavallinen raportointi

Istunnon tason koskevia lisätietoja, kuten käyttäjien, istuntojen, tehtävät ja kaatuu tilastotietoja Mobile välitys Basic raportoinnin käsitellään.

### <a name="session-tracking"></a>Istunnon seuranta

Mobile välitys istunnon jaetaan toimenpiteiden järjestys, kunkin tunnistaa nimi.

Perinteinen verkkosivustossa on suositeltavaa määritellä eri tehtävän sivuston jokaisella sivulla. Sivuston tai web-sovelluksen missä nykyinen sivu muuttuu koskaan voit seurata toimintojen pienempi skaalaus, kuten sivun.

Kummassakin tapauksessa voit käynnistää tai muuttaa nykyisen käyttäjän toiminnon, soita `engagement.agent.startActivity` funktio. Esimerkki:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

Mobile välitys palvelimen päättyy automaattisesti avointa istuntoa kolme minuuttia sen jälkeen, kun sovellus-sivulla on suljettu.

Vaihtoehtoisesti voit lopettaa istunnon manuaalisesti soittamalla `engagement.agent.endActivity`. Tämä asetus määrittää nykyisen käyttäjän tehtävä "Vapaa."  Istunnon lopettaminen myöhemmin 10 sekuntia paitsi uuden puhelun `engagement.agent.startActivity` säilyy istunto.

Määritä 10 sekunnin viive yleinen välitys kohdetta, seuraavasti:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [AZURE.NOTE] Et voi käyttää `engagement.agent.endActivity` - `onunload` takaisinsoitto koska et voi tehdä tässä vaiheessa AJAX puhelut.

## <a name="advanced-reporting"></a>Laajennettu raportointi

Voit myös halutessasi sovelluksen kielikohtaiset tapahtumat, virheitä ja töiden raportoimaan haluat käyttää Mobile välitys Ohjelmointirajapinnan. Voit käyttää Mobile välitys Ohjelmointirajapinnan kautta `engagement.agent` objekti.

Voit käyttää kaikkia Mobile välitys-Mobile välitys API-lisäominaisuuksia. Ohjelmointirajapinnan on yksityiskohtainen [Lisäasetukset Mobile-välitys tunnisteita Ohjelmointirajapinnan web-sovelluksen käyttämisestä](mobile-engagement-web-use-engagement-api.md)on artikkelissa.

## <a name="customize-the-urls-used-for-ajax-calls"></a>URL-osoitteet AJAX puheluihin mukauttaminen

Voit mukauttaa URL-osoitteet, joka käyttää Mobile välitys Web SDK-paketissa. Uudelleen lokin URL-osoite (SDK päätepisteen kirjaaminen), voit ohittaa määritykset tältä:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Jos URL-funktiot palauttavat merkkijonon, joka alkaa `/`, `//`, `http://`, tai `https://`, oletusarvo-malli ei käytetä. Oletusarvon mukaan `https://` värimallin käytetään näistä URL-osoitteet. Jos haluat mukauttaa oletusarvo-malli, Ohita määritykset, seuraavasti:

    window.azureEngagement = {
      ...
      urls: {
        ...      
        scheme: '//'
      }
    };
