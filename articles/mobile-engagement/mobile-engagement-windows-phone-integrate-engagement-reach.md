<properties 
    pageTitle="Windows Phone Silverlight Reach SDK-integrointi" 
    description="Miten voit integroida Azure Mobile välitys Reach kanssa Windows Phone Silverlight-sovellukset"                    
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

#<a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlight Reach SDK-integrointi

Sinun on noudatettava integrointi kuvattua [Windows Phone Silverlight välitys SDK integrointi](mobile-engagement-windows-phone-integrate-engagement.md) ennen seuraavan tässä oppaassa.

##<a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>Välitys saavuttaa SDK upotat Windows Phone Silverlight-projektiin

Sinulla ei ole mitään Lisää. `EngagementReach`viittaukset ja resurssit ovat jo projektin.

> [AZURE.TIP]  Voit mukauttaa sijaitsevat kuvia `Resources` kansion projektin erityisesti brändiä kuvaketta (välitys-kuvaketta, että oletusarvo).

##<a name="add-the-capabilities"></a>Lisää ominaisuuksia

Välitys saavuttaa SDK on joitakin muita ominaisuuksia.

Avaa oman `WMAppManifest.xml` tiedoston ja varmista, että seuraavia ominaisuuksia ilmoitetaan:

-   `ID_CAP_PUSH_NOTIFICATION`
-   `ID_CAP_WEBBROWSERCOMPONENT`

Ensimmäisen vaiheen käytetään MPNS-palvelu, jotta ilmoitusruudun näyttö. Toinen käytetään selaimen tehtävän upotat SDK.

Muokkaa `WMAppManifest.xml` tiedoston ja lisätä sisällä `<Capabilities />` tunniste:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

##<a name="enable-the-microsoft-push-notification-service"></a>Ota käyttöön Microsoft Push-ilmoituspalvelu

Jotta voit käyttää **Microsoft Push-ilmoituspalvelu** (kutsutaan MPNS) oman `WMAppManifest.xml` tiedostossa on oltava `<App />` tunnisteen kanssa `Publisher` määrite asettaminen projektin nimen.

##<a name="initialize-the-engagement-reach-sdk"></a>Välitys Reach SDK alusta

### <a name="engagement-configuration"></a>Välitys määritys

Välitys-määritys on keskitetty `Resources\EngagementConfiguration.xml` projektin tiedosto.

Muokkaa tiedostoa reach kokoonpanon määrittäminen:

-   *Valinnainen*alkuperäisen push (MPNS) on aktivoitu vai ei ole välillä `<enableNativePush>` ja `</enableNativePush>` tunnisteet (`true` oletusarvoisesti).
-   *Valinnainen*push kanavan nimi `<channelName>` ja `</channelName>` tunnisteita, anna sama, että sovelluksesi voi tällä hetkellä käyttää tai jätä se tyhjäksi.

Jos haluat määrittää sen suorituksen aikana, voit soittaa ennen välitys agentti alustus seuraavalla tavalla:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */
    
    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [AZURE.TIP] Voit määrittää sovelluksen MPNS push-kanavan nimi. Oletusarvon mukaan välitys luo nimen perusteella sovelluksen tunnus. Sinulla ei tarvitse määrittää nimen itse, paitsi jos aiot käyttää push-kanavan välitys ulkopuolella.

### <a name="engagement-initialization"></a>Välitys valmistelu

Muokkaa `App.xaml.cs`:

-   Voit lisätä oman `using` lauseet:

        using Microsoft.Azure.Engagement;

-   Lisää `EngagementReach.Instance.Init` jälkeen `EngagementAgent.Instance.Init` - `Application_Launching` :

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

-   Lisää `EngagementReach.Instance.OnActivated` - `Application_Activated` menetelmää:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

> [AZURE.IMPORTANT] `EngagementReach.Instance.Init` Erillinen viestiketjun suoritetaan. Sinun ei tarvitse tehdä sen itse.

##<a name="app-store-submission-considerations"></a>App store lähetyksen huomioon otettavia seikkoja

Microsoft joutuu sääntöjä, kun käytät push-ilmoitukset:

Microsoft- [Sovelluksen määrittäminen] -asiakirjat-kohdassa 2.9:

1) Hyväksy push-ilmoitusten vastaanottotilanteiden käyttäjien on pyydettävä. Lisää sitten asetuksista riippuen tapa poistaa push-ilmoitukset käytöstä.

EngagementReach-objekti sisältää kaksi menetelmää voit hallita suostumuksen antamista-/ suostumuksen antamista-ulos, `EnableNativePush()` ja `DisableNativePush()`. Voi luoda vaihtoehto esimerkiksi kytkin asetuksia MPNS ottaminen käyttöön tai poistaa käytöstä.

Voit myös valita aktivoinnin MPNS välitys-kokoonpanossa\<windows phone-sdk-reach-määritysten\>.

> 2.9.1) sovellus on ensin kuvataan ilmoitukset toimitetaan ja **hankkia käyttäjän express käyttöoikeuden (sisältyy)**ja **Anna järjestelmän, jonka avulla käyttäjä valita ulos push-ilmoitusten vastaanottaminen**. Kaikki ilmoitukset, jotka on annettu käyttämällä Microsoftin Push-ilmoituspalvelu on oltava johdonmukaisia verrattuna käyttäjälle kuvausta ja täytettävä kaikki soveltuvat [Käytännöt]  [ Content Policies] ja [Sovelluksen määrättyjä lisävaatimukset].

2) Älä käytä liikaa push-ilmoitukset. Välitys käsittelee ilmoitusten puolestasi.

> 2.9.2) sovellus ja sen käyttö Microsoftin Push-ilmoituspalvelu on ei kohtuuttoman verkon kapasiteettia tai Microsoftin Push-ilmoituspalvelu kaistanleveyden tai muussa aiheettomasti vaikuttaa Windows Phone-tai Microsoft-laitetta tai liiallinen push-ilmoitukset, määritetyn suositeltavaa kerralla harkintansa Microsoft-palvelun ja on ei vahingoittaa tai minkä tahansa Microsoft-verkoissa tai palvelinten kolmannen osapuolen palvelimiin tai Microsoftin Push-ilmoituspalvelu verkkojen häiritä.

3) MPNS lähettämään criticals tiedot eivät ole riippuvaisia. Välitys käyttää MPNS, joten tämä sääntö koskee myös luotu sisällä välitys edusta kampanjat.

> 2.9.3) Microsoftin Push-ilmoituspalvelu eivät välttämättä ole käyttänyt lähettää ilmoituksia, jotka ovat toiminta-ajatuksen kriittinen tai muuten-syötteestä voi vaikuttaa elinkaaren tai kuoleman asioita, mukaan lukien rajoituksetta kriittinen ilmoitukset liittyvät lääketieteellinen laitteen tai ehto. MICROSOFT NIMENOMAISESTI MYÖNNÄ MITÄÄN TAKUITA, MICROSOFTIN PUSH-ILMOITUSPALVELU KÄYTTÖ- TAI VASTAANOTTOKUITTAUSTA MICROSOFTIN PUSH ILMOITUKSEN PALVELUN ILMOITUSTEN ON KESKEYTYKSETTÄ VIRHE VAPAA TAI MUUSSA TAATA YKSITTÄISEKSI REAALIAIKAINEN VÄLEIN.

**Emme voi taata, että sovelluksesi välittää vahvistusprosessi, jos eivät tue suositukset.**

##<a name="handle-data-push-optional"></a>Käsitellä tietoja push (valinnainen)

Jos haluat, että sovelluksesi voi vastaanottaa Reach tietojen työntää, on toteuttaa EngagementReach luokan kaksi tapahtumaa.

    EngagementReach.Instance.DataPushStringReceived += (body) =>
    {
       Debug.WriteLine("String data push message received: " + body);
       return true;
    };
    
    EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
    {
       Debug.WriteLine("Base64 data push message received: " + encodedBody);
       // Do something useful with decodedBody like updating an image view
       return true;
    };

Näet kunkin tavan takaisinsoitto palauttaa totuusarvon. Välitys lähettää sen taustatietokannan palaute lähetettyään tietojen push. Jos takaisinkutsu palauttaa arvon EPÄTOSI, `exit` on Lähetä palautetta. Muussa tapauksessa se on `action`. Jos ei takaisinsoittoa on määritetty, tapahtumille `drop` palautetta palautetaan välitys.

> [AZURE.WARNING] Välitys ei voi vastaanottaa Kerrannaiset palautetta tietojen push varten. Jos aiot määrittää useita tiedostokäsittelijöitä tapahtuman, Huomaa, että palautteen vastaavat viimeiseen jokin lähetetty. Tässä tapauksessa on suositeltavaa palauttaa aina sama Vältä sekava palaute edusta-arvo.

##<a name="customize-ui-optional"></a>Mukauta Käyttöliittymä (valinnainen)

### <a name="first-step"></a>Ensimmäinen vaihe

Olemme avulla voit mukauttaa reach UI.

Tällöin sinun on luotava aliluokka `EngagementReachHandler` luokka.

**Esimerkki koodi:**

    using Microsoft.Azure.Engagement;
    
    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

Aseta sisällön `EngagementReach.Instance.Handler` Mukautettu-objekti kenttään oman `App.xaml.cs` luokan sisällä `Application_Launching` menetelmää.

**Esimerkki koodi:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [AZURE.NOTE] Välitys käyttää oletusarvon mukaan oma soveltaminen `EngagementReachHandler`. Sinun ei tarvitse luoda omia ja jos teet näin, sinun ei tarvitse ohittaa kaikki menetelmää. Oletusarvoisen toimintatavan aloitetaan välitys perusosoitteen objektia.

### <a name="layouts"></a>Asettelut

Oletusarvon mukaan Reach käyttämällä DLL upotetun resurssien ilmoitukset ja sivujen näyttäminen.

Voit päättää kuitenkin käyttää omaa resursseja vastaamaan oman merkki-komponentit.

Voit ohittaa `EngagementReachHandler` oman onko välitys asettelun käyttäminen aliluokan tavoista:

**Esimerkki koodi:**

    // In your subclass of EngagementReachHandler
    
    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetPollUri()
    {
       // return the path of your own xaml
    }
    
    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [AZURE.TIP] `CreateNotification` Menetelmä voi palauttaa null. Ilmoitus ei tule näkyviin ja reach markkinointikampanjan poistetaan.

Helpottaa asettelun käyttöympäristösi annamme myös oman xaml, joka voi olla koodisi perustana. Ne sijaitsevat välitys SDK-arkistoon (/ src/reach /).

> [AZURE.WARNING] Annamme käyttämäsi lähteet ovat täsmälleen saman niistä, Käytämme. Jotta voit halutessasi muokata niitä suoraan Älä unohda muuttaa nimitila ja nimi.

### <a name="notification-position"></a>Ilmoitus sijainti

Oletusarvon mukaan-sovelluksen ilmoituksella näkyy sovelluksen alavasemmalla. Voit muuttaa ongelman ohittaminen `GetNotificationPosition` menetelmää `EngagementReachHandler` objekti.

    // In your subclass of EngagementReachHandler
    
    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Tällä hetkellä, voit valita välillä `BOTTOM` (oletus) ja `TOP` sijaintia.

### <a name="launch-message"></a>Avaa viesti

Käyttäjän napsauttaessa järjestelmän ilmoituksen (ilmoitukseen) välitys Avaa sovellus, ladata push-viestien sisältö ja näyttää vastaavan markkinointikampanjan sivu.

Ei viiveen sovelluksen käynnistys- ja näyttämisen sivu (mukaan verkon nopeuden).

Jos haluat määrittää käyttäjälle jotain ladataan, anna visual tiedot, kuten tilanneilmaisin tai tilanneilmaisin. Välitys ei voi käsitellä, että itse, mutta se on muutama käsittelytoimintoja puolestasi.

Toteuttamaan takaisinkutsu toimi:

    /* The application has launched and the content is loading.
     * You should display an indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };
    
    /* The application has finished loading the content and the page
     * is about to be displayed.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };
    
    /* The content has been loaded, but an error has occurred.
     * You can provide an information to the user.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Voit määrittää takaisinkutsu oman `Application_Launching` maksutavan oman `App.xaml.cs` tiedoston mieluiten ennen `EngagementReach.Instance.Init()` Soita.

> [AZURE.TIP] Kunkin käsittelijä kutsuu Käyttöliittymän säikeen. Ei tarvitse huolehtia käytettäessä MessageBox tai julkaiseminen Käyttöliittymän liittyvät.

[Sovelluksen käytännöt]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[Lisävaatimukset tietylle sovellukselle tyypeissä]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx
 
