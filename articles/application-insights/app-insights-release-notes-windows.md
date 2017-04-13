<properties 
    pageTitle="Hakemuksen tiedot Windows julkaisutiedot" 
    description="Windows-kaupan SDK uusimmat päivitykset." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/12/2016" 
    ms.author="joshweb"/>
 
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>Hakemuksen tiedot SDK for Windows Phone julkaisutiedot ja tallentaa

Hakemuksen tiedot SDK lähettää telemetriatietojen live sovellukseen liittyviä [Hakemuksen tiedot](https://azure.microsoft.com/services/application-insights/), jossa voit analysoida sen käyttöä ja suorituskykyä.


## <a name="version-111"></a>Version 1.1.1

### <a name="windows-sdk"></a>Windows SDK-paketissa

- Korjaa jumittua kaatua aikana, Windows Phone-Silverlight SDK käytettäessä. Tämän muutoksen jälkeen kaatumisen, joka tapahtuu myöhemmin noin 2 sekunnin jälkeen WindowsAppInitialier.InitializeAsync(...) puhelu samanlainen levylle ja seuraavan lähetetään, kun sovellus käynnistetään. Jos kaatumisen tapahtuu ennen noin 2 sekunnin jälkeen puhelun, se ohitetaan.  
- Määritä NuGet riippuvuudet tietyn Core ja Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3)-version.   

### <a name="core-sdk"></a>Core SDK-paketissa

- Core hallitaan github. [Github](http://github.com/Microsoft/ApplicationInsights-dotnet/releases) löytyvät tulevien Core SDK: n julkaisutiedot

## <a name="version-11"></a>Version 1.1

### <a name="core-sdk"></a>Core SDK-paketissa

- SDK esitellään nyt uusi telemetriatietojen tyyppi ```DependencyTelemetry``` sisältää tietoja sovelluksesta puhelun riippuvuus
- Uusi menetelmä ```TelemetryClient.TrackDependency``` avulla voit lähettää riippuvuuden puhelut tietoja sovelluksesta

## <a name="version-100"></a>Version 1.0.0

### <a name="windows-app-sdk"></a>Windows-sovelluksen SDK

- Windows-sovellusten uusi alustus. Uusi `WindowsAppInitializer` luokan kanssa `InitializeAsync()` mahdollistaa automaattiseen käynnistykseen SDK sivustokokoelman alustus. Tämä muutos Salli tarkemmin ja merkittäviä app alustus suorituskykyparannukset edellisen ApplicationInsights.config tekniikka päälle.
- DeveloperMode määritetään enää automaattisesti. Voit muuttaa DeveloperMode toimintaa, sinun on määritettävä koodi.
- NuGet paketti injects enää ApplicationInsights.config. Suosittelemme käyttämään uuden WindowsAppInitializer lisääminen manuaalisesti NuGet paketti.
- ApplicationInsights.config vain lukee `<InstrumentationKey>`, kaikki muut asetukset ohitetaan ensisijaisuus WindowsAppInitializer asetukset.
- Säilön Market on automaattisen kerännyt SDK.
- Runsaasti virheenkorjauksia, vakauteen ja liittyvät suorituskykyparannukset.

### <a name="core-sdk"></a>Core SDK-paketissa

- ApplicationInsights.config tiedosto ei ole enää requiered. Ja ei ole lisätty NuGet paketti. Määritys voi määrittää täysin koodi.
- NuGet paketin enää Lisää kohteet tiedoston ratkaisuun. Tämä poistaa DeveloperMode automaattinen asetus virheenkorjaus muodosta aikana. DeveloperMode on määritettävä manuaalisesti koodi.

## <a name="version-017"></a>Version 0.17

### <a name="windows-app-sdk"></a>Windows-sovelluksen SDK

- Windows-sovelluksen SDK tukee nyt yleinen Windows sovellukset, jotka on luotu Windows 10 technical Preview'n ja ja 2015 RC: n kanssa.

### <a name="core-sdk"></a>Core SDK-paketissa

- TelemetryClient oletusarvo InMemoryChannel valmistelu.
- Uusi lisätty-Ohjelmointirajapinnan `TelemetryClient.Flush()`. Kohdistus Tämä menetelmä käynnistää heti eston lataus, kaikki telemetriatietojen kirjautunut asiakkaalle. Näin ennen prosessin Sammuta Lataa manuaalinen käynnistävä.
- NuGet paketin lisätty .net 4.5 kohde. Tämä kohde ei ole ulkoisia riippuvuuksia (poistetaan BCL ja EventSource riippuvuudet).

## <a name="version-016"></a>Version 0,16 

2015-04-28 esikatselu

- SDK tukee nyt DNX kohdeympäristö käyttöön [.NET Core framework](http://www.dotnetfoundation.org/NETCore5) -sovellusten valvonta.
- Esiintymät ```TelemetryClient``` Tallenna enää Instrumentation-näppäintä. Nyt, jos instrumentation avain ei ole määritetty ```TelemetryClient``` erikseen ```InstrumentationKey``` palauttavat null. Se korjaa ongelman, kun määrität ```TelemetryConfiguration.Active.InstrumentationKey``` päätyttyä joitakin telemetriatietojen jo allekirjoitukseen, telemetriatietojen moduulit, kuten riippuvuuden kerääminen web pyynnöt tietojen kerääminen ja suorituskyvyn laskureita kerääminen käyttää instrumentation uusi avain.

## <a name="version-015"></a>Version 0,15

- Uuden ominaisuuden ```Operation.SyntheticSource``` nyt käytettävissä ```TelemetryContext```. Voit nyt telemetriatietojen kohteiden merkitseminen "ei todellinen käyttäjä-liikenne" ja määrittää, miten tämä liikenne luotiin. Esimerkkinä tämän ominaisuuden avulla voit erottaa liikenne testi-automaatio kuormituksen testi liikenteen.
- Kanavan logiikan on siirretty erillisessä NuGet kutsutaan Microsoft.ApplicationInsights.PersistenceChannel. Kanavan oletusarvo on nyt nimeltään InMemoryChannel
- Uusi menetelmä ```TelemetryClient.Flush``` tyhjentämään telemetriatietojen kohteet-puskuri synkronoidusti avulla

## <a name="version-013"></a>Version 0,13

Vanhemmat versiot käytettävissä ei ole julkaisutiedot. 
