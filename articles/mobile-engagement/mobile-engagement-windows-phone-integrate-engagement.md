<properties 
    pageTitle="Windows Phone Silverlight välitys SDK-integrointi" 
    description="Miten voit integroida Azure Mobile välitys kanssa Windows Phone Silverlight-sovellukset"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlight välitys SDK-integrointi

> [AZURE.SELECTOR] 
- [Windowsin Universal](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android-laitteeseen](mobile-engagement-android-integrate-engagement.md) 

Tässä kuvataan helpoin tapa Aktivoi Azure Mobile välitys analyysin ja seuranta-funktioiden Windows Phone Silverlight-sovelluksen.

Seuraavat vaiheet ovat riittävän Aktivoi lokit tarpeen laskea tilastoja kaikki käyttäjät, istunnot, aktiviteetteja, kaatuu ja Technicals raportti. Lokit tarpeen laskea muiden tilastotietoja, kuten tapahtumia, virheitä ja töiden raportti on tehtävä manuaalisesti välitys-Ohjelmointirajapinnan käyttäminen (Katso alla [Lisäasetukset Mobile-välitys tunnisteita API Windows Phone Silverlight-sovelluksen käyttämisestä](mobile-engagement-windows-phone-use-engagement-api.md) ) koska tilastotiedot sovelluksen riippuva.

##<a name="supported-versions"></a>Tuetut versiot

Mobile välitys SDK for Windowsin Silverlight voidaan yhdistää vain sovellusten kohdistamisen:

-   Windows Phone 8.0
-   Windows Phone 8.1 Silverlight

> [AZURE.NOTE] Jos Windows Phone 8.1 (-Silverlight) kohdennat viittaa [Windows yleinen integrointi ohjeiden mukaisesti](mobile-engagement-windows-store-integrate-engagement.md).

##<a name="install-the-mobile-engagement-silverlight-sdk"></a>Asenna Silverlight SDK Mobile välitys

Mobile välitys SDK for Windowsin Silverlight on käytettävissä kutsutaan *MicrosoftAzure.MobileEngagement*Nuget pakettina. Voit asentaa sen Visual Studio Nuget paketin Managerista. 

##<a name="add-the-capabilities"></a>Lisää ominaisuuksia

Välitys SDK on joitakin ominaisuuksia jotta toimi oikein Windows Phone Silverlight SDK-paketissa.

Avaa oman `WMAppManifest.xml` tiedoston ja varmista, että seuraavia ominaisuuksia on määritetty `Capabilities` Ohjauspaneelin:

-   `ID_CAP_NETWORKING`
-   `ID_CAP_IDENTITY_DEVICE`

##<a name="initialize-the-engagement-sdk"></a>Välitys SDK alustaminen

### <a name="engagement-configuration"></a>Välitys määritys

Välitys-määritys on keskitetty `Resources\EngagementConfiguration.xml` projektin tiedosto.

Voit määrittää tämän tiedoston muokkaaminen:

-   Sovelluksen yhteysmerkkijono tunnisteiden väliin `<connectionString>` ja `<\connectionString>`.

Jos haluat määrittää sen suorituksen aikana, voit soittaa ennen välitys agentti alustus seuraavalla tavalla:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

Sovelluksen yhteysmerkkijonon näkyy perinteinen Azure-portaalissa.

### <a name="engagement-initialization"></a>Välitys valmistelu

Kun luot uuden projektin `App.xaml.cs` tiedosto luodaan. Tähän luokkaan perii `Application` ja sisältää paljon tärkeitä menetelmiä. Sitä käytetään myös alustaminen välitys SDK-paketissa.

Muokkaa `App.xaml.cs`:

-   Voit lisätä oman `using` lauseet:

        using Microsoft.Azure.Engagement;

-   Lisää `EngagementAgent.Instance.Init` - `Application_Launching` menetelmää:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
          EngagementAgent.Instance.Init();
        }

-   Lisää `EngagementAgent.Instance.OnActivated` - `Application_Activated` menetelmää:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
        }

> [AZURE.WARNING] On erittäin avaavan voi lisätä välitys alustus sovelluksesta toiseen paikkaan. Kuitenkin huomioon, `EngagementAgent.Instance.Init` menetelmä suoritetaan erillinen viestiketjun sekä ei Käyttöliittymän viestiketjun.

##<a name="basic-reporting"></a>Tavallinen raportointi

### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Suositeltu tapa: ylikuormituksen oman `PhoneApplicationPage` luokat

Kaikki käyttäjät, istunnot, aktiviteetteja, kaatuu ja teknisistä tiedoista laskemiseen välitys vaatii lokit raportin aktivoimista yksinkertaisesti tehdä kaikki oman `PhoneApplicationPage` aliraportti luokat perivät `EngagementPage` luokat.

Tässä on esimerkki siitä, miten voit tehdä tämän sovelluksen sivulle. Tee sama kaikille sivuille sovelluksesi.

#### <a name="c-source-file"></a>C#-lähdetiedosto

Muokkaa sivun `.xaml.cs` tiedosto:

-   Voit lisätä oman `using` lauseet:

        using Microsoft.Azure.Engagement;

-   Korvaa `PhoneApplicationPage` kanssa `EngagementPage` :

**Ilman välitys:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Välitys: kanssa**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.WARNING] Jos sivun perii `OnNavigatedTo` -menetelmä Älä salli `base.OnNavigatedTo(e)` Soita. Muussa tapauksessa tehtävä ei raportoitu. Myös `EngagementPage` soittaa `StartActivity` sisällä `OnNavigatedTo` menetelmää.

#### <a name="xaml-file"></a>XAML-tiedosto

Muokkaa sivun `.xaml` tiedosto:

-   Lisää nimitilan ilmoitukset:

        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

-   Korvaa `phone:PhoneApplicationPage` kanssa `engagement:EngagementPage` :

**Ilman välitys:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Välitys: kanssa**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">
        
            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Ohita oletusasetukset

Oletusarvon mukaan tehtävän nimen, jossa ei ole ylimääräisiä ilmoitetaan sivun luokkanimi. Jos luokan käytetään "Sivu" jälkiliite, välitys myös poistaa sen.

Jos haluat ohittaa oletusominaisuuksien nimi, Lisää tämä koodi:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Jos haluat raportin kanssa toimiessasi ylimääräisiä tietoja, voit lisätä koodin:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Näistä tavoista kutsutaan-toimintoa `OnNavigatedTo` sivusi-menetelmää.

### <a name="alternate-method-call-startactivity-manually"></a>Vaihtoehtoinen menetelmä: soita `StartActivity()` manuaalisesti

Jos et voi tai et halua ylikuormituksen oman `PhoneApplicationPage` luokkia, voit aloittaa toimintojen sijaan soittamalla `EngagementAgent` menetelmiä suoraan.

Suosittelemme, että puhelut `StartActivity` sisällä oman `OnNavigatedTo` oman PhoneApplicationPage-menetelmää.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [AZURE.IMPORTANT] Varmista, että voit lopettaa istunnon oikein.
>
> SDK automaattisesti soittaa `EndActivity` silloin, kun sovellus suljetaan. Näin, se on **erittäin** suositeltavaa Soita `StartActivity` menetelmä aina, kun tehtävä käyttäjän muuttaminen ja **ei koskaan** Soita `EndActivity` menetelmää. Tämä menetelmä lähettää viestin välitys-palvelimeen, että nykyinen käyttäjä on poistunut sovelluksen tämä vaikuttaa kaikkiin sovelluksen lokit.

##<a name="advanced-reporting"></a>Laajennettu raportointi

Halutessasi voit halutessasi raportin sovelluksen tiettyjä tapahtumia, virheet ja töitä, voit tehdä käyttämällä muita menetelmiä löytyvät `EngagementAgent` luokka. Välitys Ohjelmointirajapinnan avulla voit käyttää kaikkia välitys 's lisäominaisuuksia.

Lisätietoja Katso [Lisäasetukset Mobile-välitys tunnisteita API Windows Phone Silverlight-sovelluksen käyttämisestä](mobile-engagement-windows-phone-use-engagement-api.md).

##<a name="advanced-configuration"></a>Määritysten Lisäasetukset

### <a name="disable-automatic-crash-reporting"></a>Poista Automaattinen virheraportointi

Voit poistaa automaattisen kaatumisen raportoinnin välitys ominaisuus. Valitse käsittelemättömän poikkeuksen vuoksi voi esiintyä myös silloin, kun välitys ei tehdä mitään.

> [AZURE.WARNING] Jos aiot poistaa tämän toiminnon käytöstä, Huomaa, että sovelluksen esiintyy käsittelemättömän kaatumisen, kun välitys ei lähetä kaatumisen, **ja** se ei Sulje istunto ja työt.

Käytöstä Automaattinen virheraportointi, mukauttaa sen mukaan, miten se tietueeksi kokoonpanosi:

#### <a name="from-engagementconfigurationxml-file"></a>Valitse `EngagementConfiguration.xml` tiedosto

Raportin kaatumisen asettaminen `false` välillä `<reportCrash>` ja `</reportCrash>` tunnisteet.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Valitse `EngagementConfiguration` objektin suorituksen aikana

Raportin kaatumisen arvoksi false EngagementConfiguration-objektia.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>#Pursketila

Oletusarvon mukaan välitys raportit kirjaa reaaliajassa. Jos sovellus ilmoittaa lokit hyvin usein, on parempi puskurin lokit ja raportoida ne kaikki kerralla Normaali aika perus (eli "purskeen tilassa").

Voit tehdä kutsua menetelmää:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argumentti on arvo **millisekunteina**. Milloin tahansa, jos haluat aktivoida reaaliaikainen kirjaaminen, vain kutsua menetelmää ilman mitään parametrin, tai arvon 0.

Purskeen tila on hieman Suurenna varauksen aika, mutta se on vaikutusta välitys näytöltä: kaikki istuntoa ja töiden kesto pyöristetään purskeen raja (siis istunnot ja töiden lyhyempi kuin purskeen raja-arvo ei ehkä näy). On suositeltavaa käyttää burst-kynnysarvo enää-kuin 30000 (30s). Sinun on otettava huomioon lokit tallennettuja on oikeutettu enintään 300 kohdetta. Jos lähettäminen on liian pitkä Menetät joitain lokit.

> [AZURE.WARNING] Purskeen raja-arvo ei voi määrittää ajan, joka on pienempi kuin sekunnin. Jos yrität tehdä niin SDK näkyy Jäljitä virhe ja muuttaa automaattisesti oletusarvoon, 0 sekuntia. Tämä käynnistää SDK ilmoittamaan lokit reaaliajassa.
 
