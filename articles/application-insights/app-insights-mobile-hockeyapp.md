<properties
    pageTitle="Suorituskyvyn seurantaa mobile web-sovellusten Developer Analytics | Microsoft Azure"
    description="Sovelluksen suorituskykyä ja käytön valvonta mobiilisovelluksen kehittäjille. , työpöytä, web-palvelu ja Taustajärjestelmä sovellusten HockeyApp ja sovelluksen tiedot."
    authors="alancameronwills"
    services="application-insights"
    documentationCenter=""
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="awills"/>

# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>Mobiili Analytics HockeyApp ja hakemuksen tiedot

Seurata suorituskykyä ja beeta-testi ja otettu käyttöön sähköistä tai työpöydän sovellusten [HockeyApp](https://hockeyapp.net/)ja käyttö. Seurata tukevat web-palvelu ja Taustajärjestelmä sovellukset ja [Visual Studio sovelluksen tiedot](app-insights-overview.md). Asiakkaan ja palvelimen sovelluksista Analytics-tietojen analysointia ja näyttää kaavion rinnalla toisiinsa Azure Raporttinäkymät-ikkunan.

Microsoft Developer Analytics ratkaisuja kaksi sovelluksen suorituskyvyn seurantaa varten:

* **HockeyApp mobile** ja työpöydän sovellukset.
 * Jaa valittujen käyttäjien testi versiot.
 * Kaatua analyysi.
 * Käyttöanalyysin mukautetun telemetriatietojen.
* **Sovelluksen tietoa sivustoja** ja palvelut sekä Taustajärjestelmä sovellukset.
 * Suorituskyvyn ja käyttömäärän arvot ja ilmoitukset.
 * Poikkeus raportointi ja diagnostiikan jäljitys.
 * Diagnostiikan Etsi suodatus ja siihen liittyvissä telemetriatietojen linkkejä.

Molemmat vaihtoehdot tarjoavat:

 * Tehokas **[analyyttisten kyselykielen](app-insights-analytics.md)** diagnostiikka-ja analysointia varten.
 * **[Tietojen vieminen](app-insights-export-telemetry.md)** Omat tallennustilan.
 * **Integroitu Raporttinäkymät-ikkunan** Näytä Analyysikaavioiden ja taulukoiden.

## <a name="monitor-your-app-components"></a>Valvoa sovelluksen osat

Noudata näitä sivuja Asenna SDK-koodin ja Käynnistä sovellus seuranta.

### <a name="web-apps---application-insights"></a>Web apps - sovelluksen tiedot

* [ASP.NET web Appissa](app-insights-asp-net.md) 
* [Java web Appissa](app-insights-java-get-started.md)
* [Node.js web Appissa](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Azure pilvipalveluihin](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>Mobiilisovellusten - HockeyApp

* [iOS-sovellukseen](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X-ohjelma](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Android-sovelluksen](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Yleinen Windows-sovellus](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [Windows Phone 8 ja 8.1-sovellus](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Esityksen Windows Foundation-sovellus](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

Windows-työpöytäsovellusten Suosittelemme HockeyApp. Mutta voit myös [lähettää telemetriatietojen sovelluksen havainnollistamisen Windows-sovelluksesta](app-insights-windows-desktop.md). Haluat ehkä tehdä kokeilla hakemuksen tiedot.


## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>Analyysin ja HockeyApp telemetriatietojen vienti

Voit tutkia HockeyApp mukautettu ja kirjaudu analyysin ja jatkuva Vie ominaisuuksista, sovelluksen tiedot määrittämällä [sillan](app-insights-hockeyapp-bridge-app.md)telemetriatietojen.




