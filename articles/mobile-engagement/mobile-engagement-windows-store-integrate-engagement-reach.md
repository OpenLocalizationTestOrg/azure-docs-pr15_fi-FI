<properties 
    pageTitle="Yleinen Windows-sovellusten saavuttaa SDK-integrointi" 
    description="Miten voit integroida Azure Mobile välitys Reach yleinen Windows-sovellusten kanssa"
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

# <a name="windows-universal-apps-reach-sdk-integration"></a>Yleinen Windows-sovellusten saavuttaa SDK-integrointi

Sinun on noudatettava integrointi kuvattua [Windows yleinen välitys SDK integrointi](mobile-engagement-windows-store-integrate-engagement.md) ennen seuraavan tässä oppaassa.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>Välitys saavuttaa SDK upottaminen Windows yleinen Project-ohjelmaan

Sinulla ei ole mitään Lisää. `EngagementReach`viittaukset ja resurssit ovat jo projektin.

> [AZURE.TIP] Voit mukauttaa sijaitsevat kuvia `Resources` kansion projektin erityisesti brändiä kuvaketta (välitys-kuvaketta, että oletusarvo). Yleinen sovelluksista voit myös siirtää `Resources` kansioon jaetun projektin voit jakaa sen sisällön sovellukset, mutta se on säilytettävä `Resources\EngagementConfiguration.xml` tiedoston oletussijaintiin, kun se on riippuvainen ympäristö.

## <a name="enable-the-windows-notification-service"></a>Ota käyttöön Windows-ilmoituspalvelu

### <a name="windows-8x-and-windows-phone-81-only"></a>Windowsin 8.x ja Windows Phone 8.1 vain

Jotta voit käyttää **Windows-ilmoituspalvelu** (kutsutaan WNS) että `Package.appxmanifest` tiedoston `Application UI` valitsemalla `All Image Assets` robotti vasemmanpuoleisessa ruudussa. Oikealla puolella olevaa ruutua `Notifications`, muuta `toast capable` - `(not set)` , `(Yes)`.

### <a name="all-platforms"></a>Kaikissa ympäristöissä

Haluat synkronoida sovelluksen Microsoft-tilisi ja välitys-ympäristössä. Tämän ominaisuuden tarvitset Luo tili tai Kirjaudu sisään [windows keskihajonta keskelle](https://dev.windows.com). Tämän jälkeen uuden sovelluksen luominen ja Etsi SUOJAUSTUNNUS ja salausavaimen. Siirry välitys edusta-sovelluksen-asetus- `native push` ja tunnistetiedot liittäminen. Napsauta projektin, valitse sen jälkeen `store` ja `Associate App with the Store...`. Haluat sovellus, ennen kuin olet luonut synkronoida sen.

## <a name="initialize-the-engagement-reach-sdk"></a>Välitys Reach SDK alusta

Muokkaa `App.xaml.cs`:

-   Lisää `EngagementReach.Instance.Init` jälkeen `EngagementAgent.Instance.Init` -että `InitEngagement` menetelmää:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
          EngagementReach.Instance.Init(e);
        }

    `EngagementReach.Instance.Init` Erillinen viestiketjun suoritetaan. Sinun ei tarvitse tehdä sen itse.

> [AZURE.NOTE] Jos käytössäsi on push-ilmoitukset muualla sovelluksessa sinun on välitys saavuttaa [jakaminen push-kanava](#push-channel-sharing) .

## <a name="integration"></a>Integrointi

Välitys voit lisätä sovelluksen Reach sovelluskohtaisesta nauhan ja kudosten näkymät ilmoitukset ja kyselyiden kahdella tavalla: kerros-integroinnin ja web näkymien manuaalinen integrointi. Ei olisi yhdistettävä sekä lähestymistapa samalla sivulla.

Kaksi integrointi valita on koottu näin:

-   Voit halutessasi kerros-integrointi, jos sivujen perii jo agentti `EngagementPage`, se on vain ohjeisiin ja korvaaminen `EngagementPage` mukaan `EngagementPageOverlay` ja `xmlns:engagement="using:Microsoft.Azure.Engagement"` mukaan `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` sivuilla.
-   Voit halutessasi web-näkymien integroinnin manuaalisesti Jos haluat sijoittaa tarkasti sisällä sivujen saavuttaa-Käyttöliittymän tai jos et halua periytyminen toisen tason lisääminen. 

### <a name="overlay-integration"></a>Kerros-integrointi

Välitys kerros Lisää dynaamisesti käyttöliittymäelementit, joita käytetään näyttää Reach Kampanjat sivulle. Jos kerros ei rakenne kannattaa web-näkymien manuaalinen integrointi sijaan.

.Xaml tiedoston muuttuvat `EngagementPage` viittaus`EngagementPageOverlay`

-   Lisää nimitilan ilmoitukset:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

-   Korvaa `engagement:EngagementPage` kanssa `engagement:EngagementPageOverlay`:

**Kanssa EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
        
            <!-- Your layout -->
        </engagement:EngagementPage>

**Kanssa EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">
        
            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Tunnisteen sivullesi .cs tiedostossa `EngagementPageOverlay` sijaan `EngagementPage` ja tuoda `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

-   Korvaa `EngagementPage` kanssa `EngagementPageOverlay`:

**Kanssa EngagementPage:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**Kanssa EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


Lisää välitys kerros `Grid` osan päälle sivun asettelu ja kaksi koostuva `WebView` elementit yksi nauhan ja toinen kudosten näkymän.

Voit mukauttaa suoraan kerros-osat `EngagementPageOverlay.cs` tiedosto.

### <a name="web-views-manual-integration"></a>Web-näkymien manuaalinen integrointi

Saatavilla haun sivuille kahden `WebView` elementtejä, jotka ovat vastuussa nauhan ja kudosten näkymän näyttäminen. Riittää, sinun on tehtävä lisätään nämä kaksi `WebView` osia toiseen sivuilla, tässä on esimerkki:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


Tässä esimerkissä `WebView` osat ovat sovitettu niiden säilö, joka sisältää automaattisesti uudelleen koon ne näytön kierto- tai ikkunan koon muutoksista.

> [AZURE.WARNING] On tärkeää pitää samassa naming `engagement_notification_content` ja `engagement_announcement_content` varten `WebView` osat. Saatavilla on tunnistaa ne niiden nimen mukaan. 

## <a name="handle-datapush-optional"></a>Käsittele datapush (valinnainen)

Jos haluat, että sovelluksesi voi vastaanottaa Reach tietojen työntää, on toteuttaa EngagementReach luokan kaksi tapahtumaa.

Lisää App.xaml.cs App() konstruktorissa:

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

## <a name="customize-ui-optional"></a>Mukauta Käyttöliittymä (valinnainen)

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

Aseta sisällön `EngagementReach.Instance.Handler` Mukautettu-objekti kenttään oman `App.xaml.cs` luokan sisällä `App()` menetelmää.

**Esimerkki koodi:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [AZURE.NOTE]Välitys käyttää oletusarvon mukaan oma soveltaminen `EngagementReachHandler`.
> Sinun ei tarvitse luoda omia ja jos teet näin, sinun ei tarvitse ohittaa kaikki menetelmää. Oletusarvoisen toimintatavan aloitetaan välitys perusosoitteen objektia.

### <a name="web-view"></a>Web-näkymä

Oletusarvon mukaan Reach käyttämällä DLL upotetun resurssien ilmoitukset ja sivujen näyttäminen.

Antaa koko mukauttaminen mahdollisuuksia Käytämme vain web-näkymä. Jos haluat mukauttaa asetteluja, ohittaa suoraan resurssit-tiedostojen `EngagementAnnouncement.html` ja `EngagementNotification.html`. Välitys on kaikki koodi `<body></body>` voidaan suorittaa oikein. Mutta voit lisätä tunnisteen ulompi `engagement_webview_area`.

Voit päättää kuitenkin käyttää omaa resursseja.

Voit ohittaa `EngagementReachHandler` menetelmiä oman aliluokka onko välitys asettelun, mutta huolehdi upotettu välitys-järjestelmä:

**Esimerkki koodi:**
            
            // In your subclass of EngagementReachHandler
            
            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Oletusarvon mukaan AnnouncementHTML on `ms-appx-web:///Resources/EngagementAnnouncement.html`. Suunnittele push-viestin (tekstin ilmoitus, Web anoucement ja kysely-ilmoitus) sisältöä html-tiedosto on. AnnouncementName on `engagement_announcement_content`. Se on xaml-sivun näkymä mallin nimi.

NotfificationHTML on `ms-appx-web:///Resources/EngagementNotification.html`. Suunnittele ilmoituksen push viestin html-tiedosto on. NotfificationName on `engagement_notification_content`. Se on xaml-sivun näkymä mallin nimi.

### <a name="customization"></a>Mukauttaminen

Voit mukauttaa ilmoituksen ja web-näkymä on voit Jos säilytät välitys objektin ilmoitus. Tutustu huolellisesti, että näkymä objekti on kuvattu kolme kertaa - että xaml ensimmäistä kertaa, toisen kerran .cs "setwebview()"-menetelmällä tiedostoon ja kolmannen kerran html-tiedoston.

-   Kirjoita oman xaml kuvaus nykyisen graafisen asettelun näkymä-osan.
-   Voit määrittää "setwebview()", jossa voit määrittää kahden näkymä (ilmoitus, ilmoitus) dimension .cs-tiedostossa. On erittäin tehokkaasti sovelluksen muutettaessa.
-   Välitys html-tiedosto on kuvaus, näkymä-sisältöä, rakenne ja keskenään osat haluamaasi järjestykseen.

### <a name="launch-message"></a>Avaa viesti

Käyttäjän napsauttaessa järjestelmän ilmoituksen (ilmoitukseen) välitys käynnistää sovelluksen ladata push-viestien sisältö ja avaa vastaavan markkinointikampanja-sivu.

Ei viiveen sovelluksen käynnistys- ja näyttämisen sivu (mukaan verkon nopeuden).

Jos haluat määrittää käyttäjälle jotain ladataan, anna visual tiedot, kuten tilanneilmaisin tai tilanneilmaisin. Välitys ei voi käsitellä, että itse, mutta se on muutama käsittelytoimintoja puolestasi.

Toteuttamaan takaisinkutsu "Julkisen App() {}" App.xaml.cs Lisää:

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

Voit määrittää takaisinkutsu "Julkisen App() {}"-maksutavan oman `App.xaml.cs` tiedoston mieluiten ennen `EngagementReach.Instance.Init()` Soita.

> [AZURE.TIP] Kunkin käsittelijä kutsuu Käyttöliittymän säikeen. Ei tarvitse huolehtia käytettäessä MessageBox tai julkaiseminen Käyttöliittymän liittyvät.

##<a id="push-channel-sharing"></a>Push-kanavan jakaminen

Jos käytät push-ilmoitukset toiseen tarkoitukseen sovelluksessa sinun on käytettävä välitys SDK muun push-kanava. Näin voit välttää vastaamattomat push.

- Voit kirjoittaa omia push kanavan välitys saavuttaa alustus. SDK käyttää sitä pyytää uuden sijaan.

Päivitä välitys saavuttaa alustus push-kanavassa `InitEngagement` menetelmää `App.xaml.cs` tiedoston:
    
    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

- Jos haluat vain tarjoaman push-kanavan, kun Reach alustus jälkeen voit määrittää takaisinkutsun välitys saavuttaa saat push-kanavan kerran sen vaihtoehtoisesti on luotu SDK mukaan.

Määrittää oman takaisinsoiton, milloin tahansa paikka **jälkeen** Reach alustus seuraavasti:

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Mukautetun värimallin Vihje

Annamme mukautetun värimallin käyttöä. Voit lähettää erityyppisiä URI-välitys edusta, jota käytetään välitys-sovelluksessa. Oletus-malli, kuten `http, ftp, ...` ovat hallinta Windows-ikkuna tulee kehote jos ne eivät ole asennettu oletussovellus. Voit myös luoda oman mukautetun värimallin sovelluksen.

Voit määrittää mukautetun värimallin sovelluksessa helppo tapa on Avaa oman `Package.appxmanifest` Siirry- `Declarations` Ohjauspaneelin. Valitse `Protocol` käytettävissä ilmoitukset Vieritä ruutuun ja lisää se. Muokkaa `Name` uusi protokolla-kenttään haluamasi nimi.

Nyt voit käyttää Tämä protokolla, muokata oman `App.xaml.cs` kanssa `OnActivated` -menetelmä Älä unohda alustaa välitys tähän myös:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);
            
              //TODO design action to do when app is launch
            
              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;
            
                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion
 
