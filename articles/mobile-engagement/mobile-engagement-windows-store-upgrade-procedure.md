<properties 
    pageTitle="Yleinen sovellukset Windows SDK päivittäminen ohjeita" 
    description="Windows Azure Mobile välitys yleinen Apps SDK päivityksen ohjeet"                     
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

#<a name="windows-universal-apps-sdk-upgrade-procedures"></a>Yleinen sovellukset Windows SDK päivittäminen ohjeita

Jos valmiiksi välitys vanhempi versio on asennettu sovellukseen, sinun on ottaa huomioon seuraavat seikat, kun päivitetään SDK.

Saatat joutua noudata useita ohjeita, jos vastattu SDK useita versioita. Esimerkiksi jos voit siirtää 0.10.1 0.11.0, sinun on ensin noudattamalla "lähettäjä, 0.10.1 0.9.0" sitten "lähettäjä, 0.11.0 0.10.1" kuvatulla tavalla.

##<a name="from-330-to-340"></a>Valitse 3.3.0 3.4.0

### <a name="test-logs"></a>Testaa lokit

Konsolin lokit SDK tuottamat voi nyt käytössä/poissa käytöstä/suodatettu. Voit mukauttaa tätä Päivitä ominaisuus `EngagementAgent.Instance.TestLogEnabled` johonkin käytettävissä arvo `EngagementTestLogLevel` luettelointi, esimerkiksi:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Resurssit

Kerros Reach on parannettu. Se on osa SDK NuGet paketin resurssit.

Kun päivittäminen uutta versiota SDK, voit valita, haluatko säilyttää aiemmin luotujen tiedostojen resurssien kerros-kansiosta tai olla:

* Jos edellisen kerros toimii puolestasi tai ovat integrointi `WebView` manuaalisesti sitten voit päätät sulkeminen tiedostojen osia, se toimii edelleen. 
* Jos haluat päivittää uusi kerros korvata kokonaisuudessaan `overlay` kansion resurssien uudella SDK-paketista (UWP sovellukset: päivityksen jälkeen, voit hakea uuteen kerros-kansioon % USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Uusi kerros käyttämällä korvaa edellisen version tehdyt mukautukset.

##<a name="from-320-to-330"></a>Valitse 3.2.0 3.3.0

### <a name="resources"></a>Resurssit
Tämä vaihe koskee vain mukautettuja resursseja. Jos olet mukauttanut SDK (html, kuvat, päällekkäin) myöntämä resurssien käytössäsi, voit varmuuskopioida ne ennen päivittämistä ja ottaminen uudelleen käyttöön tekemäsi mukautukset päivitetyn resursseja.

##<a name="from-310-to-320"></a>Valitse 3.1.0 3.2.0

### <a name="resources"></a>Resurssit
Tämä vaihe koskee vain mukautettuja resursseja. Jos olet mukauttanut SDK (html, kuvat, päällekkäin) myöntämä resurssien käytössäsi, voit varmuuskopioida ne ennen päivittämistä ja ottaminen uudelleen käyttöön tekemäsi mukautukset päivitetyn resursseja.

### <a name="webview-integration"></a>Näkymä-integrointi
Tässä versiossa on tuotu joitakin parannuksia vastaamaan toisen laitteen lomakkeen tekijät. Varmista, että näkymä liittäminen vastaavat seuraavasti:

Valitse oman XAML sivun ():

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

Liitetty .cs- tiedostossa:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
            
              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }
              
              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif
            
              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }
            
              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

##<a name="from-200-to-300"></a>Valitse 2.0.0 3.0.0

### <a name="resources"></a>Resurssit
Tämä vaihe koskee vain mukautettuja resursseja. Jos olet mukauttanut SDK (html, kuvat, päällekkäin) myöntämä resurssien käytössäsi, voit varmuuskopioida ne ennen päivittämistä ja ottaminen uudelleen käyttöön tekemäsi mukautukset päivitetyn resursseja.

##<a name="from-111-to-200"></a>Valitse 1.1.1 2.0.0

Seuraavassa kerrotaan, miten SDK-integroinnin siirtäminen tarjoaman Capptain SAS sovellukseen Azure Mobile välitys tarjoaa Capptain palvelun. 

> [Azure.IMPORTANT] Capptain ja Mobile välitys eivät ole samoja palveluja ja alla kuvatulla tavalla korostaa vain siirtäminen asiakas-sovellus. Siirtyminen SDK-sovelluksessa ei siirretä tietojen Capptain-palvelimien Mobile välitys-palvelimiin

Jos olet siirtymässä aiemmasta versiosta, tilanteeseesi Capptain sivuston siirtäminen 1.1.1 ensin sitten Käytä seuraavasti

### <a name="nuget-package"></a>Nuget-paketti

Korvaa **Capptain.WindowsPhone** **MicrosoftAzure.MobileEngagement** Nuget paketti.

### <a name="applying-mobile-engagement"></a>Käyttämällä Mobile välitys

SDK käytetään termiä `Engagement`. Sinun täytyy päivittää projektin vastaamaan tämän muutoksen.

Sinun on poistettava nykyisen Capptain nuget paketti. Ota huomioon, että kaikki muutokset Capptain resurssit-kansiossa, poistetaan. Jos haluat säilyttää nämä tiedostot ja kopioida ne.

Tämän jälkeen Asenna uusi Microsoft Azure välitys nuget paketti projektiin. Löydät sen suoraan verkkosivuilla [nuget]. tai tähän indeksi. Tämä toiminto korvaa kaikki välitys käyttämät resurssit-tiedostot ja Lisää uusi välitys DLL projektiviitteet.

Sinulla on siivottava projektiviitteet poistamalla Capptain DLL viittauksia. Jos et tee näin, Capptain versio on ristiriidassa ja virheet tapahtuu.

Jos olet mukauttanut Capptain resursseja, vanha tiedostojen sisältöä kopioida ja liittää ne uusi välitys-tiedostoissa. Huomaa, että xaml- ja cs tiedostot on ehkä päivitettävä.

Kun nämä vaiheet on suoritettu sinun on vain korvaa vanhan Capptain viittaukset uusi välitys viittausten mukaan.

1. Kaikki Capptain nimitilan on päivitettävä.

    Ennen siirtoa:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Kun siirto:
    
        using Microsoft.Azure.Engagement;

2. Kaikki Capptain luokat, jotka sisältävät "Capptain" pitäisi olla "Välitys".

    Ennen siirtoa:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Kun siirto:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Xaml-tiedostot Capptain nimitilan ja määritteet muuttaa myös.

    Ennen siirtoa:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Kun siirto:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Sijoita sivun muuttuu
    > [AZURE.IMPORTANT] Merkintäkerroksen myös muutokset. Uusi nimitilan on `Microsoft.Azure.Engagement.Overlay`. Se on käytettävä xaml- ja cs tiedostoihin. Lisäksi `CapptainGrid` on oltava `EngagementGrid`, `capptain_notification_content` ja `capptain_announcement_content` nimetään `engagement_notification_content` ja `engagement_announcement_content`.
    
    Saat kerros:
    
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
    
    On:
    
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>

5. Muut resurssit, kuten Capptain kuvat ja HTML-tiedostoja Huomaa, että ne myös on nimetty uudelleen käyttämään "Välitys".

### <a name="project-declaration"></a>Project-ilmoitus

Valitse Package.appxmanifest `File Type Associations` on päivitetty:

 -   capptain\_saavuttaa\_sisällön välitys\_saavuttaa\_sisältö
 -   capptain\_lokin\_välitys tiedoston\_lokin\_tiedosto

### <a name="application-id--sdk-key"></a>Sovellustunnus / SDK avain

Välitys käyttää yhteysmerkkijonon. Sinun ei tarvitse määrittää Mobile välitys tunnus ja SDK-näppäintä, vain on määritettävä yhteysmerkkijonon. Voit määrittää sen EngagementConfiguration-tiedoston.

Välitys-määritys voi määrittää oman `Resources\EngagementConfiguration.xml` projektin tiedosto.

Voit määrittää tämän tiedoston muokkaaminen:

-   Sovelluksen yhteysmerkkijono tunnisteiden väliin `<connectionString>` ja `<\connectionString>`.

Jos haluat määrittää sen suorituksen aikana, voit soittaa ennen välitys agentti alustus seuraavalla tavalla:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

Sovelluksen yhteysmerkkijonon näkyy perinteinen Azure-portaalissa.

### <a name="items-name-change"></a>Kohteiden nimen muuttaminen

Kaikkien kohteiden nimeltä *capptain* on nimennyt *välitys*. Lisää vastaavasti varten *Capptain* *välitys*.

Esimerkkejä tavallisimmat Capptain kohteet:

-   Nykyään EngagementConfiguration CapptainConfiguration
-   Nykyään EngagementAgent CapptainAgent
-   Nykyään EngagementReach CapptainReach
-   Nykyään EngagementHttpConfig CapptainHttpConfig
-   Nykyään GetEngagementPageName GetCapptainPageName

Huomaa, että nimeäminen uudelleen myös vaikuttaa ohittaa tavoista.

 
