<properties
    pageTitle="Windowsin Universal Advanced MobileApps välitys raportointi"
    description="Miten voit integroida Azure Mobile välitys yleinen Windows-sovellusten kanssa"                  
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Laajennettu Windows yleinen sovellusten välitys SDK-raportointi

> [AZURE.SELECTOR]
- [Yleinen Windows](mobile-engagement-windows-store-advanced-reporting.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-laitteeseen](mobile-engagement-android-advanced-reporting.md)

Tässä ohjeaiheessa kerrotaan Lisää raportoinnin skenaarioita yleinen Windows-sovelluksessa. Näissä tilanteissa ovat, voit käyttää sovelluksen [Käytön aloittaminen](mobile-engagement-windows-store-dotnet-get-started.md) -opetusohjelma luotu.

## <a name="prerequisites"></a>Edellytykset

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on suoritettava [Käytön aloittaminen](mobile-engagement-windows-store-dotnet-get-started.md) -opetusohjelma, joka on tahallisesti suora ja yksinkertainen. Tässä opetusohjelmassa kerrotaan, voit valita lisäasetuksia.

## <a name="specifying-engagement-configuration-at-runtime"></a>Välitys määritysten suorituksen määrittäminen

Välitys-määritys on keskitetty `Resources\EngagementConfiguration.xml` tiedoston projektista, joka on, jossa se on määritetty [Käytön aloittaminen](mobile-engagement-windows-store-dotnet-get-started.md) -artikkelissa.

Mutta voit myös määrittää suorituksen: Voit soittaa ennen välitys agentti alustus seuraavalla tavalla:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Suositeltu tapa: ylikuormituksen oman `Page` luokat

Kaikki käyttäjät, istunnot, aktiviteetteja, kaatuu ja teknisistä tiedoista laskemiseen välitys vaatii lokit raportointia aktivoimaan kaikkien oman `Page` aliraportti luokat perivät `EngagementPage` luokat.

Tässä on esimerkki sivun sovelluksesi. Tee sama kaikille sivuille sovelluksesi.

### <a name="c-source-file"></a>C#-lähdetiedosto

Muokkaa sivun `.xaml.cs` tiedosto:

-   Voit lisätä oman `using` lauseet:

        using Microsoft.Azure.Engagement;

-   Korvaa `Page` kanssa `EngagementPage`:

**Ilman välitys:**

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Välitys: kanssa**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Jos sivun ohittaa `OnNavigatedTo` -menetelmä Varmista, että puhelu `base.OnNavigatedTo(e)`. Muussa tapauksessa tehtävä on ei ilmoiteta ( `EngagementPage` puhelut `StartActivity` sisällä sen `OnNavigatedTo` menetelmää).

### <a name="xaml-file"></a>XAML-tiedosto

Muokkaa sivun `.xaml` tiedosto:

-   Lisää nimitilan ilmoitukset:

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Korvaa `Page` kanssa `engagement:EngagementPage`:

**Ilman välitys:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Välitys: kanssa**

        <engagement:EngagementPage
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

### <a name="override-the-default-behaviour"></a>Ohita oletusarvon käyttäytyminen

Oletusarvon mukaan tehtävän nimen, jossa ei ole ylimääräisiä ilmoitetaan sivun luokkanimi. Jos luokan käytetään "Sivu" jälkiliite, välitys poistaa sen.

Voit ohittaa nimi oletusominaisuuksien lisäämällä koodi:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Voit ilmoittaa ylimääräisten tietojen kanssa toimiessasi tämän koodin lisääminen:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Näistä tavoista kutsutaan-toimintoa `OnNavigatedTo` sivusi-menetelmää.

### <a name="alternate-method-call-startactivity-manually"></a>Vaihtoehtoinen menetelmä: soita `StartActivity()` manuaalisesti

Jos et voi tai et halua ylikuormituksen oman `Page` luokkia, voit aloittaa toimintojen sijaan soittamalla `EngagementAgent` menetelmiä suoraan.

Suosittelemme, että puhelut `StartActivity` sisällä oman `OnNavigatedTo` sivusi-menetelmää.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Varmista, että voit lopettaa istunnon oikein.
>
> Windows SDK: Yleinen automaattisesti soittaa `EndActivity` silloin, kun sovellus suljetaan. Näin se on **erittäin** suositeltavaa Soita `StartActivity` menetelmää aina, kun tehtävä käyttäjän muuttaminen ja **ei koskaan** Soita `EndActivity` menetelmää. Tämä menetelmä ilmoittaa välitys palvelimen nykyinen käyttäjä on poistunut hakemus, jossa tehdyt muutokset vaikuttavat kaikkiin sovelluksen lokit.

## <a name="advanced-reporting"></a>Laajennettu raportointi

Vaihtoehtoisesti voit raportoida sovelluksen kielikohtaiset tapahtumia, virheitä ja voit tehdä töitä, käytä muita menetelmiä löytyvät `EngagementAgent` luokka. Välitys Ohjelmointirajapinnan sallii kaikkien välitys lisäominaisuuksia käyttö.

Lisätietoja Katso [Lisäasetukset Mobile-välitys tunnisteita API Windows yleinen-sovelluksen käyttämisestä](mobile-engagement-windows-store-use-engagement-api.md).
