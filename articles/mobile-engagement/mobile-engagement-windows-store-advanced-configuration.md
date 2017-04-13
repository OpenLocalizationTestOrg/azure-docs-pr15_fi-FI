<properties
    pageTitle="Lisäasetusten Windows yleinen sovellusten välitys SDK määrittäminen"
    description="Määritysten Lisäasetukset Azure Mobile välitys yleinen Windows-sovellusten kanssa"                    
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Lisäasetusten Windows yleinen sovellusten välitys SDK määrittäminen

> [AZURE.SELECTOR]
- [Yleinen Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-laitteeseen](mobile-engagement-android-advanced-configuration.md)

Tässä kuvataan määrittäminen Azure Mobile välitys Android-sovellusten asetuksia.

## <a name="prerequisites"></a>Edellytykset

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

##<a name="advanced-configuration"></a>Määritysten Lisäasetukset

### <a name="disable-automatic-crash-reporting"></a>Poista Automaattinen virheraportointi

Voit poistaa automaattisen kaatumisen raportoinnin välitys ominaisuus. Tämän jälkeen tapahtuu käsittelemättömän poikkeuksen vuoksi, välitys ei saa aikaan mitään.

> [AZURE.WARNING] Jos tämä ominaisuus poistetaan käytöstä, valitse käsittelemättömän kaatumisen tapahtuessa-sovelluksen välitys ei lähetä kaatumisen **ja** Sulje istunto ja töiden.

Käytöstä Automaattinen virheraportointi, mukauttaa sen mukaan, miten se tietueeksi kokoonpanosi:

#### <a name="from-engagementconfigurationxml-file"></a>Valitse `EngagementConfiguration.xml` tiedosto

Raportin kaatumisen asettaminen `false` välillä `<reportCrash>` ja `</reportCrash>` tunnisteet.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Valitse `EngagementConfiguration` objektin suorituksen aikana

Raportin kaatumisen arvoksi false EngagementConfiguration-objektia.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Reaaliaikaiset poistaminen käytöstä

Oletusarvon mukaan välitys raportit kirjaa reaaliajassa. Jos sovellus ilmoittaa lokit usein, on parempi puskurin lokit ja raportin ne kaikki kerralla säännöllisin väliajoin. Tätä kutsutaan "#pursketila".

Voit tehdä kutsua menetelmää:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argumentti on arvo **millisekunteina**. Aina, kun haluat aktivoida reaaliaikainen kirjaaminen, Soita menetelmä ilman mitään parametrin, tai arvon 0.

Purskeen tila on hieman kasvaa varauksen aika, mutta se on vaikutusta välitys näytöltä: kaikki istuntoa ja töiden kesto pyöristetään purskeen raja (siis istunnot ja töiden lyhyempi kuin purskeen raja-arvo ei ehkä näy). On suositeltavaa käyttää burst-kynnysarvo enää-kuin 30000 (30s). Tallennetut lokit on oikeutettu enintään 300 kohdetta. Jos lähettäminen on liian pitkä, enää joitakin lokitiedot.

> [AZURE.WARNING] Purskeen raja-arvo ei voi määrittää ajanjakson on alle sekunnin. Jos teet näin, SDK näyttää virheen jäljityksen ja palauttaa automaattisesti oletusarvo 0 sekuntia. Tämä käynnistää SDK ilmoittamaan lokit reaaliajassa.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
