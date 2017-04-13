<properties
    pageTitle="Windows SDK yleinen-integrointi"
    description="Windowsin yleinen integrointi SDK for Azure Mobile välitys"                                     
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

#<a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Windows Azure Mobile välitys yleinen SDK-integrointi

Tässä asiakirjassa kerrotaan kaikki integrointi- ja määrityspalveluja varten Azure Mobile välitys Windows yleinen SDK-paketissa.

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on suoritettava tässä [15 minuutin opetusohjelma](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="advanced-features"></a>Lisäominaisuudet

### <a name="reporting-features"></a>Raportointiominaisuuksien
Voit lisätä nämä ominaisuudet:

1. [Raportoinnin Lisäasetukset](mobile-engagement-windows-store-advanced-reporting.md)

2. [Kokoonpanon Lisäasetukset](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Ilmoitukset

[Miten voit integroida Reach (ilmoitusten) Windows yleinen-sovelluksessa](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Tunnisteen suunnitelman toteutus:

[Voit käyttää tunnisteita API Windows yleinen-sovelluksen lisäasetusten Mobile-välitys](mobile-engagement-windows-store-use-engagement-api.md)

##<a name="release-notes"></a>Julkaisutiedot

###<a name="340-04192016"></a>3.4.0 (04-19-2016)

-   Saavuttaa kerros parannukset.
-   Lisätty "TestLogLevel" API ottaminen käyttöön tai poistaminen käytöstä tai Suodata konsolin lokit lähettämän SDK.
-   Kiinteä-aktiviteetin ilmoitukset kohdistamisen ensimmäiseen toimintoon sovellus ei näy Käynnistä-painiketta.

Aiemmissa versioissa Katso [täydellinen julkaisutiedot](mobile-engagement-windows-store-release-notes.md)

##<a name="upgrade-procedures"></a>Päivityksen ohjeita

Jos valmiiksi välitys vanhempi versio on asennettu sovellukseen, sinun on ottaa huomioon seuraavat seikat, kun päivitetään SDK.

Jos vastattu SDK useita versioita, voit joutua useita ohjeita noudattamalla. Valmis [Päivittäminen ohjeita](mobile-engagement-windows-store-upgrade-procedure.md)kohdassa. Esimerkiksi jos voit siirtää 0.10.1 0.11.0, sinun on ensin noudattamalla "lähettäjä, 0.10.1 0.9.0" sitten "lähettäjä, 0.11.0 0.10.1" kuvatulla tavalla.

###<a name="from-330-to-340"></a>Valitse 3.3.0 3.4.0

####<a name="test-logs"></a>Testaa lokit

Konsolin lokit SDK tuottamat voi nyt käytössä/poissa käytöstä/suodatettu. Jos haluat mukauttaa, Päivitä ominaisuus `EngagementAgent.Instance.TestLogEnabled` johonkin käytettävissä arvot `EngagementTestLogLevel` luettelointi, esimerkiksi:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

####<a name="resources"></a>Resurssit

Kerros Reach on parannettu. Se on osa SDK NuGet paketin resurssit.

Siirtyessäsi SDK: N uusi versio, voit valita, haluatko säilyttää aiemmin luotujen tiedostojen resurssien kerros-kansiosta tai olla:

* Jos edellisen kerros toimii puolestasi tai ovat integrointi `WebView` osat manuaalisesti, valitse voit päättää pitämään yhteyttä sulkeminen tiedostoja, se toimii edelleen.
* Päivittämään Uusi kerros korvata kokonaisuudessaan `overlay` kansion resurssien uudella SDK-paketista (UWP sovellukset: päivityksen jälkeen, voit hakea uuteen kerros-kansioon % USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Uusi kerros käyttämällä korvaa edellisen version tehdyt mukautukset.

### <a name="upgrade-from-older-versions"></a>Vanhempien versioiden päivittäminen

Katso [ohjeita päivittäminen](mobile-engagement-windows-store-upgrade-procedure.md)
