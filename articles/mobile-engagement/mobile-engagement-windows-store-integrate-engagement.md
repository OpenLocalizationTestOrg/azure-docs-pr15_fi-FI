<properties 
    pageTitle="Windowsin yleinen sovellusten välitys SDK-integrointi" 
    description="Miten voit integroida Azure Mobile välitys yleinen Windows-sovellusten kanssa"                  
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
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="windows-universal-apps-engagement-sdk-integration"></a>Windowsin yleinen sovellusten välitys SDK-integrointi

> [AZURE.SELECTOR] 
- [Yleinen Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android-laitteeseen](mobile-engagement-android-integrate-engagement.md) 

Tässä kuvataan helpoin tapa Aktivoi välitys 's Analytics seurantaan ja valvontaan Funktiot yleinen Windows-sovelluksessa.

Seuraavat vaiheet ovat riittävän Aktivoi lokit tarpeen laskea tilastoja kaikki käyttäjät, istunnot, aktiviteetteja, kaatuu ja Technicals raportti. Lokit tarpeen laskea muiden tilastotietoja, kuten tapahtumia, virheitä ja töiden raportti on tehtävä manuaalisesti välitys-Ohjelmointirajapinnan käyttäminen (katso [Lisäasetukset Mobile-välitys tunnisteita API Windows yleinen-sovelluksen käyttämisestä](mobile-engagement-windows-store-use-engagement-api.md) , koska nämä tilasto on sovellusta riippuvaisten.

## <a name="supported-versions"></a>Tuetut versiot

Mobile välitys SDK for Windowsin yleinen sovellukset voidaan yhdistää vain Windows Runtimen ja Yleinen Windows-ympäristön sovellusten kohdistamisen:

-   Windows 8: ssa
-   Windows 8.1
-   Windows Phone 8.1
-   Windows 10: ssä (Työpöytä- ja mobiilisovelluksen perheille)

> [AZURE.NOTE] Windows Phone Silverlight-kohdistus Valitse noudata [Windows Phone Silverlight integrointi toimet](mobile-engagement-windows-phone-integrate-engagement.md).

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>Asenna Mobile välitys yleinen Apps SDK

### <a name="all-platforms"></a>Kaikissa ympäristöissä

Mobile välitys SDK for Windowsin yleinen sovellus on käytettävissä kutsutaan *MicrosoftAzure.MobileEngagement*Nuget pakettina. Voit asentaa sen Visual Studio Nuget paketin Managerista.

### <a name="windows-8x-and-windows-phone-81"></a>Windowsin 8.x ja Windows Phone 8.1

NuGet ottaa automaattisesti käyttöön SDK resurssit `Resources` kansion sovelluksen projektin ylimmällä tasolla.

### <a name="windows-10-universal-windows-platform-applications"></a>Windows 10 yleinen Windows-ympäristön sovellusten

NuGet automaattisesti Ota UWP sovelluksen vielä SDK-resursseja. Sinun on tehtävä manuaalisesti, kunnes käyttöönottaminen on jälleen NuGet:

1.  Avaa tiedosto Resurssienhallinta.
2.  Siirry seuraavaan sijaintiin (**x.x.x** on versio, oletko asentamassa välitys): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3.  Vedä ja pudota **resurssit** -kansiossa Resurssienhallinnasta Visual Studiossa projektin pääkansioon.
4.  Valitse Visual Studio projektin ja aktivoi päällä **Ratkaisunhallinnassa** **Näytä kaikki tiedostot** -kuvaketta.
5.  Projektin ei sisällytetä tiedostoja. Voit tuoda ne kerralla napsauttamalla hiiren kakkospainikkeella **resurssit** -kansiossa, **projektin pois** sitten toiseen napsauttamalla **resurssit** -kansiossa, **Sisällytä projektin** koskemaan koko kansion uudelleen. **Resurssit** -kansion kaikki tiedostot nyt sisältyvät projektin.

## <a name="add-the-capabilities"></a>Lisää ominaisuuksia

Välitys SDK on joitakin ominaisuuksia Windows SDK jotta toimi oikein.

Avaa oman `Package.appxmanifest` tiedoston ja varmista, että seuraavia ominaisuuksia ilmoitetaan:

-   `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>Välitys SDK alusta

### <a name="engagement-configuration"></a>Välitys määritys

Välitys-määritys on keskitetty `Resources\EngagementConfiguration.xml` projektin tiedosto.

Voit määrittää tämän tiedoston muokkaaminen:

-   Sovelluksen yhteysmerkkijono tunnisteiden väliin `<connectionString>` ja `<\connectionString>`.

Jos haluat määrittää sen suorituksen aikana, voit soittaa ennen välitys agentti alustus seuraavalla tavalla:
          
          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

Sovelluksen yhteysmerkkijonon näkyy perinteinen Azure-portaalissa.

### <a name="engagement-initialization"></a>Välitys valmistelu

Kun luot uuden projektin `App.xaml.cs` tiedosto luodaan. Tähän luokkaan perii `Application` ja sisältää paljon tärkeitä menetelmiä. Sitä käytetään myös alustaa välitys SDK-paketissa.

Muokkaa `App.xaml.cs`:

-   Voit lisätä oman `using` lauseet:

        using Microsoft.Azure.Engagement;

-   Määritä välitys alustaminen kerran kaikki kutsut jakamaan menetelmää:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
        
          // or
        
          EngagementAgent.Instance.Init(e, engagementConfiguration);
        }
        
-   Soita `InitEngagement` - `OnLaunched` menetelmää:

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
          InitEngagement(e);
        }

-   Kun sovellus käynnistetään mukautetun värimallin, toiseen sovellukseen tai komentorivin myös `OnActivated` menetelmää kutsutaan. Sinun on myös aloittaa välitys SDK-paketissa, kun sovellus on aktivoitu. Voit tehdä ohittaa `OnActivated` menetelmää:

        protected override void OnActivated(IActivatedEventArgs args)
        {
          InitEngagement(args);
        }

> [AZURE.IMPORTANT] On erittäin avaavan voi lisätä välitys alustus sovelluksesta toiseen paikkaan.

## <a name="basic-reporting"></a>Tavallinen raportointi

### <a name="recommended-method-overload-your-page-classes"></a>Suositeltu tapa: ylikuormituksen oman `Page` luokat

Kaikki käyttäjät, istunnot, aktiviteetteja, kaatuu ja teknisistä tiedoista laskemiseen välitys vaatii lokit raportin aktivoimista riittää, että annat kaikki oman `Page` aliraportti luokat perivät `EngagementPage` luokat.

Tässä on esimerkki siitä, miten voit tehdä tämän sovelluksen sivulle. Tee sama kaikille sivuille sovelluksesi.

#### <a name="c-source-file"></a>C#-lähdetiedosto

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

> [AZURE.IMPORTANT] Jos sivun ohittaa `OnNavigatedTo` -menetelmä Varmista, että puhelu `base.OnNavigatedTo(e)`. Muussa tapauksessa tehtävän ei ilmoiteta ( `EngagementPage` puhelut `StartActivity` sisällä sen `OnNavigatedTo` menetelmää).

#### <a name="xaml-file"></a>XAML-tiedosto

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

#### <a name="override-the-default-behaviour"></a>Ohita oletusarvon käyttäytyminen

Oletusarvon mukaan tehtävän nimen, jossa ei ole ylimääräisiä ilmoitetaan sivun luokkanimi. Jos luokan käytetään "Sivu" jälkiliite, välitys myös poistaa sen.

Jos haluat ohittaa oletusarvon käyttäytyminen nimi, Lisää tämä koodi:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Jos haluat raportin muutamia ylimääräisiä informations toimiessasi kanssa, voit lisätä koodin:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Näistä tavoista kutsutaan-toimintoa `OnNavigatedTo` sivusi-menetelmää.

### <a name="alternate-method-call-startactivity-manually"></a>Vaihtoehtoinen menetelmä: soita `StartActivity()` manuaalisesti

Jos et voi tai et halua ylikuormituksen oman `Page` luokkia, voit aloittaa toimintojen sijaan soittamalla `EngagementAgent` menetelmiä suoraan.

On suositeltavaa Soita `StartActivity` sisällä oman `OnNavigatedTo` sivusi-menetelmää.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Varmista, että voit lopettaa istunnon oikein.
> 
> Windows SDK: Yleinen automaattisesti soittaa `EndActivity` silloin, kun sovellus suljetaan. Näin, se on **erittäin** suositeltavaa Soita `StartActivity` menetelmä aina, kun tehtävä käyttäjän muuttaminen ja **ei koskaan** Soita `EndActivity` -menetelmä Tämä menetelmä lähettää välitys palvelimeen, että käyttäjällä on jättää sovelluksen, tämä toiminto vaikuttaa kaikkiin sovelluksen lokit.

## <a name="advanced-reporting"></a>Laajennettu raportointi

Halutessasi voit halutessasi raportin sovelluksen tiettyjä tapahtumia, virheet ja töitä, voit tehdä käyttämällä muita menetelmiä löytyvät `EngagementAgent` luokka. Välitys Ohjelmointirajapinnan avulla voit käyttää kaikkia välitys 's lisäominaisuuksia.

Lisätietoja Katso [Lisäasetukset Mobile-välitys tunnisteita API Windows yleinen-sovelluksen käyttämisestä](mobile-engagement-windows-store-use-engagement-api.md).

##<a name="advanced-configuration"></a>Määritysten Lisäasetukset

### <a name="disable-automatic-crash-reporting"></a>Poista Automaattinen virheraportointi

Voit poistaa automaattisen kaatumisen raportoinnin välitys ominaisuus. Valitse käsittelemättömän poikkeuksen vuoksi voi esiintyä myös silloin, kun välitys ei tehdä mitään.

> [AZURE.WARNING] Jos aiot poistaa tämän toiminnon käytöstä, Huomaa, että sovelluksen esiintyy käsittelemättömän kaatumisen, kun välitys ei lähetä kaatumisen **ja** ei Sulje istunto ja työt.

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

### <a name="burst-mode"></a>#Pursketila

Oletusarvon mukaan välitys raportit kirjaa reaaliaikaisesti. Jos sovellus ilmoittaa lokit hyvin usein, on parempi puskurin lokit ja raportoida ne kaikki kerralla Normaali aika perus (eli "purskeen tilassa").

Voit tehdä kutsua menetelmää:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argumentti on arvo **millisekunteina**. Milloin tahansa, jos haluat aktivoida reaaliaikainen kirjaaminen, vain kutsua menetelmää ilman mitään parametri, tai arvon 0.

Purskeen tila on hieman Suurenna varauksen aika, mutta se on vaikutusta välitys näytöltä: kaikki istuntoa ja töiden kesto pyöristetään purskeen raja (siis istunnot ja töiden lyhyempi kuin purskeen raja-arvo ei ehkä näy). On suositeltavaa käyttää burst-kynnysarvo enää-kuin 30000 (30s). Sinun on otettava huomioon lokit tallennettuja on oikeutettu enintään 300 kohdetta. Jos lähettäminen on liian pitkä Menetät joitain lokit.

> [AZURE.WARNING] Purskeen raja-arvo ei voi määrittää ajan, joka on pienempi kuin 1s. Jos yrität kiellä, SDK näyttää virheen jäljityksen ja automaattisesti palauttaa oletusarvoon, eli 0s. Tämä käynnistää SDK ilmoittamaan lokit reaaliajassa.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
 
