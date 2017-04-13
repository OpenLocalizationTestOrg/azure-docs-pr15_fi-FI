<properties 
    pageTitle="Windows Phone Silverlight SDK yleiskatsaus" 
    description="Yleistä Windows Phone Silverlight SDK: n Azure Mobile välitys"                     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Silverlight SDK yleiskatsaus Azure Mobile välitys varten

Saat tietoja siitä, miten voit integroida Azure Mobile välitys Windows Phone Silverlight-sovelluksessa, Aloita tästä. Jos haluat, kokeile ensin, varmista, että olet suorittanut tämän [15 minuutin opetusohjelma](mobile-engagement-windows-phone-get-started.md).

Napsauttamalla saat näkyviin [SDK sisältö](mobile-engagement-windows-phone-sdk-content.md)

##<a name="integration-procedures"></a>Integrointi ohjeita

1. Aloita tästä: [miten voit integroida Mobile Windows Phone Silverlight-sovelluksen välitys](mobile-engagement-windows-phone-integrate-engagement.md)

2. Ilmoitukset: [miten voit integroida Reach (ilmoitusten) Windows Phone Silverlight-sovelluksessa](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Tunnisteen suunnitelman käyttöönoton: [Lisäasetukset Mobile-välitys tunnisteita API Windows Phone Silverlight-sovelluksen käyttäminen](mobile-engagement-windows-phone-use-engagement-api.md)

##<a name="release-notes"></a>Julkaisutiedot

###<a name="330-04192016"></a>3.3.0 (04-19-2016)
Osa *MicrosoftAzure.MobileEngagement* nuget pakata **v3.4.0**

-   Lisätty "TestLogLevel" API ottaminen käyttöön tai poistaminen käytöstä tai Suodata konsolin lokit lähettämän SDK.

Aiemman version, katso [täydellinen julkaisutiedot](mobile-engagement-windows-phone-release-notes.md)

##<a name="upgrade-procedures"></a>Päivityksen ohjeita

Jos jo on vanhempi versio, tutustu SDK on integroitu sovellukseen, sinun on ottaa huomioon seuraavat seikat, kun päivitetään SDK.

Saatat joutua noudata useita ohjeita, jos vastattu SDK useita versioita. Valmis [Päivittäminen ohjeita](mobile-engagement-windows-phone-upgrade-procedure.md)kohdassa. Esimerkiksi jos voit siirtää 0.10.1 0.11.0, sinun on ensin noudattamalla "lähettäjä, 0.10.1 0.9.0" sitten "lähettäjä, 0.11.0 0.10.1" kuvatulla tavalla.

###<a name="from-200-to-330"></a>Valitse 2.0.0 3.3.0

####<a name="test-logs"></a>Testaa lokit

SDK tuottamat konsolin lokeja voidaan nyt käytössä/poissa käytöstä/suodatettu. Voit mukauttaa tätä Päivitä ominaisuus `EngagementAgent.Instance.TestLogEnabled` johonkin käytettävissä arvo `EngagementTestLogLevel` luettelointi, esimerkiksi:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Vanhempien versioiden päivittäminen

Katso [ohjeita päivittäminen](mobile-engagement-windows-phone-upgrade-procedure.md)
 
