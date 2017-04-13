<properties 
    pageTitle="Tasaiset Streaming Windows-kaupan sovelluksen opetusohjelma | Microsoft Azure" 
    description="Opettele Azure Media-palveluiden avulla voit luoda toisto tasainen virta XML MediaElement ohjausobjektin C# Windows-kaupan sovelluksen sisällön." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>



#<a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Miten voit luoda sujuvat, Streaming Windows-kaupan sovellus

Tasainen Streaming asiakkaan SDK for Windows 8 kehittäjät voivat luoda Windows-kaupan sovelluksia, joka pystyy toistamaan tarvittaessa ja live tasainen mediavirtaa. Lisäksi basic toistoa tasainen Streaming sisällön SDK sisältää ominaisuuksia, kuten Microsoft PlayReady suojaus, tason rajoituksen Live DVR äänenlaadun virtauttaa siirtymistä, kuuntele tilapäivitykset (kuten laadun tason muutokset) ja virhetapahtumia ja niin edelleen. Lisätietoja tuetuista ominaisuuksia on artikkelissa [julkaisutiedot](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Lisätietoja on artikkelissa [Player Framework Windows 8](http://playerframework.codeplex.com/). 

Tässä opetusohjelmassa on neljä opitut:

1. Tavallinen tasainen Streaming Store-sovelluksen luominen
2. Voit hallita Media edistymisen liukusäädin lisääminen
3. Valitse tasainen Streaming virtaa
4. Valitse tasainen Streaming raidat

##<a name="prerequisites"></a>Edellytykset

- Windows 8 32-bittisen tai 64-bittinen. Saat [Windows 8 yrityksen arvioinnin](http://msdn.microsoft.com/evalcenter/jj554510.aspx) MSDN.
- Visual Studio 2012 tai Visual Studio Express 2012 (tai sitä uudempi versio). Saat kokeiluversion [täältä](http://www.microsoft.com/visualstudio/11/downloads).
- [Microsoft tasainen Streaming asiakkaan SDK Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).


Kunkin oppitunnin valmis ratkaisu voi ladata MSDN Developer MALLIKOODEJA (koodi valikoima): 

- [Harjoitus 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) – yksinkertainen Windows 8-tasainen Streaming Media Player-ohjelmassa 
- [Harjoitus 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) – yksinkertainen Windows 8 tasainen Streaming Media Playerin liukusäädin hallinta 
- [Harjoitus 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - Streaming Media Playerin Stream yhteydessä Windows 8-tasainen  
- [Harjoitus 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) – Windows 8-tasainen Streaming Media Player-ohjelmassa, ja seuraa-valinta.

##<a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Harjoitus 1: Basic tasainen Streaming Store-sovelluksen luominen

Tämä Harjoitus luodaan Windows-kaupan sovelluksen ja toistaa tasainen virta MediaElement ohjausobjektin sisältö.  Käynnissä olevan sovelluksen näyttää seuraavanlaiselta:

![Tasainen virtautetun median Windows-kaupan sovelluksen Esimerkki][PlayerApplication]
 
Saat lisätietoja Windows-kaupan sovelluksen [kehittää hyvien sovelluksia Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). Tämä Harjoitus sisältää seuraavasti:

1.  Windows-kaupan projektin luominen
2.  Suunnittele käyttöliittymän (XAML)
3.  Muokkaa koodia tiedoston taakse
4.  Käännä ja testata sovellusta

**Windows-kaupan projektin luominen**

1.  Suorita Visual Studio 2012 tai uudempi versio.
2.  **Tiedosto** -valikosta **Uusi**ja valitse sitten **Projekti**.
3.  Valitse uusi projekti-valintaikkunan Kirjoita tai valitse seuraavat arvot:

Nimi|Arvo
---|---
Malliryhmä|Asennettuna, mallit ja Visual C# / Windows-kaupan
Malli|Tyhjästä sovelluksesta (XAML)
Nimi|SSPlayer
Sijainti|C:\SSTutorials
Ratkaisun nimi|SSPlayer
Luo kansio ratkaisu|(valittu)

4.  Valitse **OK**.

**Jos haluat lisätä tasainen Streaming asiakkaan SDK viittaus**

1.  Ratkaisu Explorerista **SSPlayer**hiiren kakkospainikkeella ja valitse sitten **Lisää hakemisto**.
2.  Kirjoita tai valitse seuraavat arvot:

Nimi|Arvo
---|---
Viittausryhmä|Windowsin-osassa
Viittaus|Valitse Microsoft tasainen Streaming asiakkaan SDK Windows 8: ssa ja Microsoft Visual C++ Runtime-paketti
    
3.  Valitse **OK**. 

Kun olet lisännyt viittauksia, sinun on valittava kohdennettujen ympäristö (x64 tai x86), minkä tahansa suorittimen ympäristön määritys ei toimi lisääminen viittauksia.  Napsauta ratkaisunhallinnassa näet keltainen varoitus merkki näiden lisätty viittauksia.

**Voit suunnitella player-käyttöliittymä**

1.  Ratkaisu Explorerista Kaksoisnapsauta **MainPage.xaml** Avaa raportti rakennenäkymään.
2.  Etsi ** &lt;ruudukon&gt; ** ja ** &lt;/Grid&gt; ** tunnisteet XAML-tiedosto ja liitä seuraava koodi kaksi tunnisteiden väliin:

        <Grid.RowDefinitions>
            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
        </Grid.RowDefinitions>
        
        <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
        </StackPanel>

        <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
        </StackPanel>

        <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
        </StackPanel>

    MediaElement ohjauksella toisto tietovälineelle. Nimetty sliderProgress liukusäädin käytetään seuraavan Harjoitus ohjaamaan media edistymisen.

3.  Tallenna tiedosto **CTRL + S** -näppäinyhdistelmällä.

MediaElement-ohjausobjekti ei tue tasainen Streaming sisällön ulos valmiin. Tasainen Streaming-tuen ottaminen käyttöön, sinun on rekisteröitävä tasainen Streaming-byte stream käsittelijä tiedostotunnisteen ja MIME-tyypin mukaan.  Voit rekisteröidä, voit käyttää Windows.Media nimitilan MediaExtensionManager.RegisterByteStremHandler-menetelmää.

XAML tämän tiedoston joitakin tapahtumien käsittely liittyvät ohjausobjektit.  Sinun täytyy määrittää kyseiset tapahtuman tiedostokäsittelijöitä.

**Voit muokata tiedostoa pohjana olevan koodin**

1.  Ratkaisu Explorerista **MainPage.xaml**hiiren kakkospainikkeella ja valitse sitten **Näytä koodi**.
2.  Tiedoston yläosassa Lisää seuraava lauseella:

        using Windows.Media;

3.  **MainPage** luokan alussa, Lisää seuraava tieto:

        private MediaExtensionManager extensions = new MediaExtensionManager();

4.  **MainPage** konstruktoria lopussa Lisää seuraavat kaksi riviä:

        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
        
5.  Lopussa **MainPage** -luokan aiempia seuraava koodi:

        #region UI Button Click Events
        private void btnPlay_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Play();
            txtStatus.Text = "MediaElement is playing ...";
        }
        
        private void btnPause_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Pause();
            txtStatus.Text = "MediaElement is paused";
        }
        
        private void btnSetSource_Click(object sender, RoutedEventArgs e)
        {
            sliderProgress.Value = 0;
            mediaElement.Source = new Uri(txtMediaSource.Text);
        
            if (chkAutoPlay.IsChecked == true)
            {
                txtStatus.Text = "MediaElement is playing ...";
            }
            else
            {
                txtStatus.Text = "Click the Play button to play the media source.";
            }
        }
        
        private void btnStop_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Stop();
            txtStatus.Text = "MediaElement is stopped";
        }
        
        private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            txtStatus.Text = "Seek to position " + sliderProgress.Value;
            mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
        }
        #endregion

    SliderProgress_PointerPressed tapahtumakäsittelijä määritetään tässä.  Lisää works voit tehdä, ennen kuin se toimii, jotka ovat tässä opetusohjelmassa seuraava Harjoitus on.
6.  Tallenna tiedosto **CTRL + S** -näppäinyhdistelmällä.

Valmiit takana tiedoston koodi on näyttää tältä:

![Visual Studio, tasainen Streaming Windows-kaupan sovelluksen Codeview][CodeViewPic]

**Käännä ja sovelluksen testaaminen**

1.  Napsauta **MUODOSTA** -valikossa **Määritysten hallinta**.
2.  Voit muuttaa **aktiivinen ratkaisu ympäristö** vastaamaan kehittäminen omaa ympäristöäsi.
3.  Paina **F6** koota projektin. 
4.  Painamalla **F5** sovelluksen käyttämiseen.
5.  Sovelluksen yläreunassa Käytä oletusarvoa tasainen Streaming URL-osoite tai kirjoittaa jonkin muun. 
6.  Valitse **Määritä lähde**. Koska **Automaattinen toistaminen** on käytössä oletusarvoisesti, mediatiedostojen toistaminen automaattisesti.  Voit hallita media käyttämällä **Toista**, **tauko** ja **Lopeta** -painikkeet.  Voit hallita media äänenvoimakkuuden liukusäätimellä pystysuunnassa.  Kuitenkin vaakasuora liukusäädin ohjaukseen media edistymisen ei täysin tueta vielä. 

Olet suorittanut lesson1.  Tämä Harjoitus käytetään toisto tasainen mediavirtaa MediaElement ohjausobjektiin.  Seuraava Harjoitus lisätään liukusäädin ohjaamaan tasainen mediavirtaa etenemisen.


##<a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Harjoitus 2: Lisää ohjaamaan Media edistymisen liukusäädin
Harjoitus 1 luomasi Windows-kaupan sovelluksen toistamisen tasainen Streaming mediasisältöä MediaElement XAML-ohjausobjektissa.  On joitakin basic media-funktioita, kuten aloitus- ja Lopeta sekä Keskeytä.  Tämä Harjoitus palkin liukusäädin Lisää sovellukseen.

Tässä opetusohjelmassa Käytämme ajastin päivittämään liukusäädin kohtaan MediaElement ohjausobjektin nykyisen sijainnin perusteella.  Liukusäädin alkamis- ja päättymispäivät aikaa myös tarvetta sisällön Jos päivittäminen kestää.  Tämä voi paremmin ratkaista säädön tietolähteen päivitys tapahtuma.

Media tietolähteet ovat objekteja, Luo media tiedot.  Lähde-tulkintatoiminnon tulee URL-osoite tai tavu virta ja luo tarvittavat tietolähdettä sisältö.  Lähde-tulkintatoiminnon on vakio sovellusten luominen media lähteistä. 

Tämä Harjoitus sisältää seuraavasti:

1.  Rekisteröi tasainen Streaming-käsittelijä 
2.  Lisää mukautuvat tietolähteen hallinta tason tapahtumien käsittely
3.  Lisää mukautuvat lähde-tason tapahtumien käsittely
4.  Lisää MediaElement tapahtumien käsittely
5.  Lisää Aiheeseen liittyvät viivakoodi liukusäädin
6.  Käännä ja testata sovellusta

**Rekisteröi tasainen Streaming-tavuinen stream käsittelijä ja välittää propertyset**

1.  Ratkaisu Explorerista **MainPage.xaml**napsauttamalla hiiren kakkospainikkeella ja valitse sitten **Näytä koodi**.
2.  Tiedoston alkuun, Lisää seuraava lauseella:

        using Microsoft.Media.AdaptiveStreaming;

3.  MainPage luokan alussa, Lisää seuraavat tiedot jäsenet:

        private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
        private IAdaptiveSourceManager adaptiveSourceManager;
    
4.  Sisällä **MainPage** -konstruktoria Lisää seuraava koodi **jälkeen tämä. Alusta Components();** rivi- ja Edellinen Harjoitus kirjoitetut rekisteröinti koodin rivit:
    
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
    
5.  Sisällä **MainPage** -konstruktoria, muokata kaksi RegisterByteStreamHandler tapaa, jos haluat lisätä tuotua parametrit:

        // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
        // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
        // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
        propertySet);

6.  Tallenna tiedosto **CTRL + S** -näppäinyhdistelmällä.

**Voit lisätä mukautuvat tietolähteen hallinta tason tapahtumakäsittelijä**

1.  Ratkaisu Explorerista **MainPage.xaml**napsauttamalla hiiren kakkospainikkeella ja valitse sitten **Näytä koodi**.
2.  **MainPage** -luokan sisällä lisätään seuraava tieto:

        private AdaptiveSource adaptiveSource = null;

3.  Lisää seuraavat tapahtumakäsittelijä **MainPage** luokan lopussa:
    
        #region Adaptive Source Manager Level Events
        private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
        }
        #endregion Adaptive Source Manager Level Events

4.  **MainPage** konstruktoria päättyessä Lisää seuraava rivi tilata mukautuvat lähde open-tapahtumaa:
    
    adaptiveSourceManager.AdaptiveSourceOpenedEvent += uusi AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);

5.  Tallenna tiedosto **CTRL + S** -näppäinyhdistelmällä.

**Jos haluat lisätä mukautuvat lähde tason tapahtumien käsittely**

1.  Ratkaisu Explorerista **MainPage.xaml**napsauttamalla hiiren kakkospainikkeella ja valitse sitten **Näytä koodi**.
2.  **MainPage** -luokan sisällä lisätään seuraava tieto:
    
        private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
        private Manifest manifestObject;
    
3.  **MainPage** luokan lopussa Lisää seuraavat tapahtumien käsittely:

        #region Adaptive Source Level Events
        private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
        }
        
        private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
        {
            adaptiveSourceStatusUpdate = args;
        }
        
        private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
        {
            txtStatus.Text = "Error: " + args.HttpResponse;
        }
        #endregion Adaptive Source Level Events

4.  **MediaElement AdaptiveSourceOpened** menetelmä lopussa Lisää tilaa tapahtumat seuraava koodi:
    
        adaptiveSource.ManifestReadyEvent +=
                    mediaElement_ManifestReady;
        adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 
            mediaElement_AdaptiveSourceStatusUpdated;
        adaptiveSource.AdaptiveSourceFailedEvent += 
            mediaElement_AdaptiveSourceFailed;
    
5.  Tallenna tiedosto **CTRL + S** -näppäinyhdistelmällä.

Saman tapahtumat ovat käytettävissä säädön tietolähteen hallinta tasolla sekä, jonka avulla voidaan ratkaista kaikkien Mediaelementit sovelluksessa yhteiset toiminnot. Kunkin AdaptiveSource sisältää omassa tapahtumien ja valitse AdaptiveSourceManager suoritetaan myös kaikki AdaptiveSource tapahtumat.

**Jos haluat lisätä Media elementti tapahtumien käsittely**

1.  Ratkaisu Explorerista **MainPage.xaml**napsauttamalla hiiren kakkospainikkeella ja valitse sitten **Näytä koodi**.
2.  **MainPage** luokan lopussa Lisää seuraavat tapahtumien käsittely:
    
        #region Media Element Event Handlers 
        private void MediaOpened(object sender, RoutedEventArgs e)
        {
            txtStatus.Text = "MediaElement opened";
        }
        
        private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
        {
            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
        }
        
        private void MediaEnded(object sender, RoutedEventArgs e)
        {
            txtStatus.Text ="MediaElement ended.";
        }
        #endregion Media Element Event Handlers

3.  **MainPage** konstruktoria lopussa lisääminen tapahtumat alaindeksi seuraava koodi:
    
        mediaElement.MediaOpened += MediaOpened;
        mediaElement.MediaEnded += MediaEnded;
        mediaElement.MediaFailed += MediaFailed;

4.  Tallenna tiedosto **CTRL + S** -näppäinyhdistelmällä.

**Voit lisätä liukusäätimen liittyvä koodi**

1.  Ratkaisu Explorerista **MainPage.xaml**napsauttamalla hiiren kakkospainikkeella ja valitse sitten **Näytä koodi**.
2.  Tiedoston alkuun, Lisää seuraava lauseella:
    
        using Windows.UI.Core;

3.  Lisää seuraavat tiedot jäsenet **MainPage** -luokan sisällä:
    
        public static CoreDispatcher _dispatcher;
        private DispatcherTimer sliderPositionUpdateDispatcher;
    
4.  **MainPage** konstruktoria päättyessä Lisää seuraava koodi:

        _dispatcher = Window.Current.Dispatcher;
        PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
        sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
    
5.  **MainPage** luokan päättyessä Lisää seuraava koodi:
    
        #region sliderMediaPlayer
        private double SliderFrequency(TimeSpan timevalue)
        {
            long absvalue = 0;
            double stepfrequency = -1;
        
            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }
        
            TimeSpan totalDVRDuration = new TimeSpan(absvalue);
        
            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }
        
            return stepfrequency;
        }
        
        void updateSliderPositionoNTicks(object sender, object e)
        {
            sliderProgress.Value = mediaElement.Position.TotalSeconds;
        }
        
        public void setupTimer()
        {
            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
        }

        public void startTimer()
        {
            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderStartTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderEndTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
        }
        #endregion sliderMediaPlayer

    **Huomautus:** CoreDispatcher voidaan tehdä muutoksia ei Käyttöliittymän viestiketjun Käyttöliittymän viestiketjun. Pullonkaula lähettäjä viestiketjun käyttöön, jos developer voit käyttää Käyttöliittymän osa hän aikoo Päivitä myöntämä lähettäjä.  Esimerkki:
    
        await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 
          timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
        double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 
          sliderProgress.Maximum = absvalue; }); 
        

6.  **MediaElement_AdaptiveSourceStatusUpdated** menetelmä päättyessä Lisää seuraava koodi:
    
        setSliderStartTime(args.StartTime);
        setSliderEndTime(args.EndTime);

7.  **MediaOpened** menetelmä päättyessä Lisää seuraava koodi:
    
    sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan); sliderProgress.Width = mediaElement.Width; setupTimer();

8.  Tallenna tiedosto **CTRL + S** -näppäinyhdistelmällä.

**Käännä ja sovelluksen testaaminen**

1. Paina **F6** koota projektin. 
2.  Painamalla **F5** sovelluksen käyttämiseen.
3.  Sovelluksen yläreunassa Käytä oletusarvoa tasainen Streaming URL-osoite tai kirjoittaa jonkin muun. 
4.  Valitse **Määritä lähde**. 
5.  Testaa liukusäädintä.

Harjoitus 2 on suoritettu.  Tämä Harjoitus liukusäätimen lisätään sovelluksen. 

##<a name="lesson-3-select-smooth-streaming-streams"></a>Harjoitus 3: Valitse tasainen Streaming virtaa
Tasainen virtautetun median pystyy stream sisällön useita kieliraidat-ääni, jotka ovat valittavissa katsojille mukaan.  Tämä Harjoitus kokoustyötilaa käyttäjiä valitsemaan virtaa. Tämä Harjoitus sisältää seuraavasti:

1. XAML-tiedoston muokkaaminen
2. Koodin behand tiedoston muokkaaminen
3. Käännä ja testata sovellusta


**Jos haluat muokata XAML-tiedosto**

1. Ratkaisu Explorerista **MainPage.xaml**hiiren kakkospainikkeella ja valitse sitten **Näkymän suunnittelu**.
2. Etsi &lt;Grid.RowDefinitions&gt;, ja muokata RowDefinitions siten, että ne näyttää seuraavalta:

        <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
        </Grid.RowDefinitions>

3. Sisään &lt;ruudukon&gt;&lt;/Grid&gt; tunnisteita, Lisää seuraava koodi määrittämään luetteloruutu-ohjausobjektin, jotta käyttäjät voivat käytettävissä virtaa luettelo ja valitse virtaa:

        <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
        </Grid>

4. Tallenna muutokset **CTRL + S** -näppäinyhdistelmällä.


**Voit muokata tiedostoa pohjana olevan koodin**

1. Ratkaisu Explorerista **MainPage.xaml**hiiren kakkospainikkeella ja valitse sitten **Näytä koodi**.
2. Sisällä SSPlayer nimitilan, Lisää uusi luokka: #region luokan muodossa
    
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
    
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
    
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
    
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream

3. MainPage luokan alussa, Lisää seuraavat muuttujien määritykset:

        private List<Stream> availableStreams;
        private List<Stream> availableAudioStreams;
        private List<Stream> availableTextStreams;
        private List<Stream> availableVideoStreams;

4. MainPage-luokan sisällä Lisää seuraava alue:

        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
        
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
        
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
        
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
        
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
        
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
        
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
        
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
        
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
        
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
        
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
        
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection

5. Etsi mediaElement_ManifestReady-menetelmällä ja liitä seuraava koodi-funktion lopussa:
    
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();

    Kun MediaElement luettelo on valmis, koodi saa käytettävissä virtaa luettelon ja lisää Käyttöliittymän luetteloruudun luettelon kanssa.

6. Etsi MainPage-luokan sisällä Käyttöliittymän painikkeet tapahtumien alue ja lisää sitten seuraavan funktion määritelmän:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Käännä ja sovelluksen testaaminen**

1. Paina **F6** koota projektin. 
2.  Painamalla **F5** sovelluksen käyttämiseen.
3.  Sovelluksen yläreunassa Käytä oletusarvoa tasainen Streaming URL-osoite tai kirjoittaa jonkin muun. 
4.  Valitse **Määritä lähde**. 
5.  Oletuskieli on audio_eng. Kokeile audio_eng ja audio_es välillä. Päivityksen yhteydessä tapahtuvan, valitse uusi stream, näyttöön tulee Lähetä-painiketta.

Harjoitus 3 on suoritettu.  Lisää tämä Harjoitus valitsemaan virtaa toimintoja.

##<a name="lesson-4-select-smooth-streaming-tracks"></a>Harjoitus 4: Valitse tasainen Streaming raidat
Tasainen Streaming esityksen voi olla useita eri laatutaso (nopeuksia) ja ratkaisut koodattu videotiedostot. Tämä Harjoitus otetaan käyttöön, käyttäjät voivat valita seuraa. Tämä Harjoitus sisältää seuraavasti:

1. XAML-tiedoston muokkaaminen
2. Koodin behand tiedoston muokkaaminen
3. Käännä ja testata sovellusta

**Jos haluat muokata XAML-tiedosto**

1. Ratkaisu Explorerista **MainPage.xaml**hiiren kakkospainikkeella ja valitse sitten **Näkymän suunnittelu**.
2. Etsi &lt;ruudukon&gt; tunnisteen nimi **gridStreamAndBitrateSelection**kanssa, Lisää seuraava koodi tunnisteen lopussa:

        <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
        </StackPanel>

3. Tallenna hän muutokset painamalla **CTRL + S**


**Voit muokata tiedostoa pohjana olevan koodin**

1. Ratkaisu Explorerista **MainPage.xaml**hiiren kakkospainikkeella ja valitse sitten **Näytä koodi**.
2. Sisällä SSPlayer nimitilan Lisää uusi luokka:
    
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
    
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
    
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
    
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track

3. MainPage luokan alussa, Lisää seuraavat muuttujien määritykset:
    
        private List<Track> availableTracks;

4. MainPage-luokan sisällä Lisää seuraava alue:
    
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>

        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();

            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;

            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;

                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }

        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }

        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }

        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }

        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection

5. Etsi mediaElement_ManifestReady-menetelmällä ja liitä seuraava koodi-funktion lopussa:

        getTracks(manifestObject);
        refreshAvailableTracksListBoxItemSource();

6. Etsi MainPage-luokan sisällä Käyttöliittymän painikkeet tapahtumien alue ja lisää sitten seuraavan funktion määritelmän:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Käännä ja sovelluksen testaaminen**

1. Paina **F6** koota projektin. 
2.  Painamalla **F5** sovelluksen käyttämiseen.
3.  Sovelluksen yläreunassa Käytä oletusarvoa tasainen Streaming URL-osoite tai kirjoittaa jonkin muun. 
4.  Valitse **Määritä lähde**. 
5.  Oletusarvon mukaan kaikki videovirtaa raidat ovat valittuina. Voit kokeilla bittinen muuttuvat, voit valita käytettävissä pienin siirtonopeuden ja valitse suurin käytettävissä siirtonopeuden. Näyttöön tulee Lähetä jokaisen muutoksen jälkeen.  Näet videon laatu muutokset.

Harjoitus 4 on suoritettu.  Lisää tämä Harjoitus valitsemaan raidat toimintoja.


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="other-resources"></a>Muita resursseja:
- [Miten voit luoda lisäominaisuudet tasainen Streaming 8-Windows JavaScript-sovellukseen](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
- [Tasainen Streaming teknisiä tietoja](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png
 
