<properties
    pageTitle="Aloittaminen välittää sisältöä käyttämällä .NET pyydettäessä | Azure"
    description="Tässä opetusohjelmassa käydään läpi vaiheet käyttöönoton sovelluksen on demand-sisällön toimittamisen käyttämällä .NET Azure Media-palvelujen kanssa."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/17/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Välittää sisältöä käyttämällä .NET SDK pyydettäessä käytön aloittaminen

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

>[AZURE.NOTE]
> Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](/pricing/free-trial/?WT.mc_id=A261C142F). 
 
##<a name="overview"></a>Yleiskatsaus 

Tässä opetusohjelmassa esitellään käyttöönoton käyttämällä .NET Azure Media Services (AMS) SDK videotilauspalveluiden (VoD) sisällön toimitus-sovelluksen ohjeita.


Opetusohjelman esitellään basic Media Services-työnkulku ja yleisimmät ohjelmoinnin objektit ja Media-palveluiden kehittämiseen tarvittavat tehtävät. Opetusohjelman päätyttyä osaat käyttää tai Lataa asteittain, joka on ladattu, koodattu ja ladata media mallitiedosto.

## <a name="what-youll-learn"></a>Opit

Opetusohjelman esitetään, kuinka voit tehdä seuraavat toimet:

1.  Luo Media Services-tilin, (joko Azure portaalin).
2.  Määritä streaming päätepisteen (joko Azure portaalin).
3.  Luo ja määritä Visual Studio-projekti.
5.  Muodosta yhteys Media Services-tilin.
6.  Luo uusi resurssi ja lataa videotiedoston.
7.  Koodata lähdetiedosto mukautuvat bittitaajuus MP4-tiedostot joukkoon.
8.  Julkaista kohteen ja poistaa URL-osoitteet streaming ja asteittain ladattavaksi.
9.  Testaa toistaminen sisältöä.

## <a name="prerequisites"></a>Edellytykset

Seuraavassa on suorittamiseen tarvittava opetusohjelman.

- Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. 
    
    Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](/pricing/free-trial/?WT.mc_id=A261C142F). Saat hyvitykset, jonka avulla voit kokeilla maksettu Azure services. Senkin jälkeen, kun hyvitykset on käytetty, voit säilyttää sen tilin ja käyttää Azure palveluiden ja -ominaisuuksia, kuten Azure App palvelun Web Apps-toiminnon.
- Käyttöjärjestelmät: Windows 8 tai uudempi Windows 2008 R2, Windows 7: ssä.
- .NET framework 4.0 tai uudempi
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate tai Express) tai uudempi versio.


##<a name="download-sample"></a>Lataa malli

Hae ja suorita otoksen [täältä](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Azure-portaalissa Azure Media Services-tilin luominen

Tämän osan vaiheet näyttää, miten voit luoda AMS tilin.

1. Kirjaudu sisään osoitteessa [Azure portal](https://portal.azure.com/).
2. Valitse **+ Uusi** > **Media + CDN** > **Media-palveluita**.

    ![Media-palveluiden luominen](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Kirjoita **Luo MEDIA SERVICES-tilin** pakolliset arvot.

    ![Media-palveluiden luominen](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Kirjoita **Tilin nimi**-kenttään uuden AMS tilin nimi. Media Services-tilin nimi on kaikki kirjaimiksi numerot tai kirjaimet ilman välilyöntejä, ja 3-24 merkkiä.
    2. Valitse tilaus, eri Azure-tilauksissa, joihin sinulla on pääsy kesken.
    
    2. **Resurssiryhmä**Valitse uusi tai aiemmin luotu resurssi.  Resurssiryhmä on kokoelma resursseja, jotka jakavat elinkaari, käyttöoikeudet ja käytännöt. Lisätietoja [tästä](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Sijainti**Valitse maantieteellisen alueen käytetään tallentaa Media Services-tilin media ja metatiedot-tietueita. Tällä alueella käytetään Käsittele ja median lisääminen. Vain käytettävissä Media Services alueet näkyvät avattavan luetteloruudun. 
    
    3. **Tallennustilan tilin**Valitse tallennustilan tili antamaan Blob-objektien tallennustilaan mediasisällön Media Services-tililtä. Voit valita olemassa olevan tallennustilan tilin saman maantieteellisen alueen Media Services-tiliksi tai voit luoda tallennustilan tilin. Uusi tallennustilan-tili on luotu samalla alueella. Tallennustilan tilin nimi säännöt ovat samat kuin Media Services tilit.

        Lisätietoja tallennustilan [tähän](storage-introduction.md).

    4. Valitse **raporttinäkymät-ikkunan kiinnittäminen** Nähdäksesi tilin käyttöönoton etenemisen.
    
7. Valitse lomakkeen alareunassa **luominen** .

    Kun tili on luotu, tilaksi muuttuu **käynnissä**. 

    ![Media-palveluiden asetukset](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Voit hallita tiliäsi AMS (esimerkiksi videoiden lataaminen, koodata varat, työn edistymistä) **asetukset** -ikkunaa.

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Määritä streaming päätepisteet Azure-portaalissa

Kun käsittelet jokin tilanteissa toimittaa video mukautuvat bittitaajuus streaming kautta asiakkaat Azure Media-palveluita. Media-palveluiden tukee seuraavia mukautuvat bittitaajuus streaming tekniikoita: HTTP Live Streaming (HLS), tasainen Streaming, MPEG VIIVA ja HDS (varten vain Adobe PrimeTime/Access asiakirjamalleja).

Media-palvelut on dynaaminen pakkaus, jonka avulla voit pitää mukautuvat bittitaajuus MP4 koodattu sisällön streaming tukemat Media Services (MPEG VIIVA, HLS, tasainen Streaming, HDS) juuri perustuvan, mukaan eikä sinun tarvitse tallentaa valmiiksi pakattu versioiden kaikkien näiden streaming muotoja.

Jos haluat hyödyntää Dynaaminen pakkaus, seuraavasti:

- Koodata mezzanine (lähde)-tiedoston tuominen säädön bittitaajuus MP4-tiedostot (koodauksen vaiheet ovat esitellään jäljempänä tässä opetusohjelmassa).  
- Luo vähintään yksi streaming yksikkö *tietovirta päätepisteen* , joista aiot toimituksen sisältöä. Alla olevia ohjeita noudattamalla streaming yksiköiden määrän muuttaminen.

Dynaaminen pakkaus, jossa tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostoja ja Media-palveluiden muodostaa ja on oikea vastaus asiakaskoneesta pyyntöjen perusteella.

Voit luoda ja muuttaa streaming varattu yksiköiden määrän, toimi seuraavasti:


1. Valitse **asetukset** -ikkunassa **Virtautetun median päätepisteet**. 

2. Valitse streaming päätepisteen oletus. 

    **Oletus-TIETOVIRTA PÄÄTEPISTEEN tiedot** -ikkuna.

3. Jos haluat määrittää streaming yksiköiden määrän, **Virtautetun median yksiköt** liukusäädintä.

    ![Streaming yksiköt](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Tallenna muutokset valitsemalla **Tallenna** .

    >[AZURE.NOTE]Kaikki uudet yksiköt kohdistus voi kestää jopa 20 minuuttia.

##<a name="create-and-configure-a-visual-studio-project"></a>Luo ja määritä Visual Studio-projekti

1. Luo uuden C# Console-sovelluksen Visual Studio 2013, Visual Studio 2012 tai Visual Studio 2010 SP1. Kirjoita **nimi**, **sijainti**ja **ratkaisun nimeä**ja valitse sitten **OK**.

2. Asenna **Azure Media Services .NET SDK-laajennusten** [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) NuGet paketin avulla.  Media Services .NET SDK-laajennukset on tunniste menetelmien ja helper Funktiot, jotka koodista yksinkertaisempi ja kehittää Media-palvelujen kanssa on helpompaa. Asennuksessa, asentaa **Media Services.NET SDK** myös ja Lisää kaikki tarvittavat riippuvuudet.

3. Lisää System.Configuration kokoonpanon viittaus. Tämän kokoonpanon sisältää **System.Configuration.ConfigurationManager** -luokka, jota käytetään määritysten tiedostoja, esimerkiksi App.config.

4. Avaa App.config-tiedosto (Lisää tiedoston projektiin, jos sitä ei lisätä oletusarvoisesti) ja *appSettings* -osan lisääminen tiedostoon. Määritä Azure Media Services nimi ja tilin tiliavain, kuten seuraavassa esimerkissä. Voit hankkia tilin nimi ja avaintiedoista, siirry [Azure portaaliin](https://portal.azure.com/) ja valitse AMS-tili. Valitse **asetukset** > **avaimet**. Hallitse näppäimet windows näyttää tilin nimi ja ensisijainen ja toissijainen näppäimet tulee näkyviin.

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
          
        </configuration>

5. Korvaa aiemmin **käyttämällä** lauseet alussa Program.cs tiedoston seuraava koodi.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        

6. Luo uusi kansio projektit-kansio ja kopioi koodata ja virtauttaa tai vaiheittain lataamalla haluamaasi .mp4- tai .wmv-tiedostoa. Tässä esimerkissä käytetään "C:\VideoFiles" polku.

##<a name="connect-to-the-media-services-account"></a>Yhteyden muodostaminen Media Services-tilin

Kun Media-palveluiden käyttämisessä .NET, sinun on käytettävä **CloudMediaContext** luokan useimmissa Media Services-ohjelmoinnin tehtävät: yhteyden Media Services-tilin; luominen, päivittäminen, käyttäminen ja poistaminen seuraaville objekteille: kalusto, resurssi-tiedostoja, työt, käytäntöjen, locators jne.

Korvaa oletusarvo-ohjelman luokan seuraava koodi. Koodin esitellään lukeminen yhteyden arvot App.config-tiedostosta ja niiden luomisesta **CloudMediaContext** objektin muodostamiseksi Media-palveluita. Lisätietoja yhteyden muodostamisesta Media Services artikkelissa [yhteyden muodostaminen Media-palvelun .NET Media Services SDK-paketissa](http://msdn.microsoft.com/library/azure/jj129571.aspx).

**Pää** -funktio kutsuu menetelmiä, joilla määritetään edelleen tässä osassa.

    class Program
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];

        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;

        static void Main(string[] args)
        {
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##<a name="create-a-new-asset-and-upload-a-video-file"></a>Luo uusi resurssi ja Lataa videotiedosto

Media-palveluja voit ladata (tai ingest) digitaalisten tiedostojen amerikkalaisen kyselyjä. **Kohteiden** kohde voi olla video, ääni, kuvia, pikkukuvan sivustokokoelmat, tekstin raidat tekstitys tiedostot (ja nämä tiedostot metatietoa.)  Kun tiedostot on ladattu palvelimeen, sisällön tallennetaan Pilvipalvelun myöhempää käsittelyä ja streaming suojatusti. Kohteen tiedostoja kutsutaan **Resurssi-tiedostoiksi**.

**UploadFile** menetelmä jäljempänä puhelut **CreateFromFile** (määritetty .NET SDK laajennuksia). **CreateFromFile** Luo uusi resurssi, johon määritettyä lähdetiedosto on ladattu palvelimeen.

**CreateFromFile** menetelmä tekee **AssetCreationOptions** , jonka avulla voit määrittää yksi resurssi luonti-vaihtoehdoista:

- **Ei mitään** - salausta käytetään. Tämä on oletusarvo. Huomaa, että kun tämä vaihtoehto sisältöä ei ole suojattu salataanko siirrettävät tai muiden tallennustilaan.
Jos haluat pitää MP4 käyttämällä asteittain lataaminen, käytä tätä vaihtoehtoa.
- **StorageEncrypted** - tämän asetuksen voit salata käyttää paikallisesti salaus AES (Advanced Standard)-256 bittistä salausta, jossa Lataa Azure-tallennustilan, johon se on tallennettu Tyhjennä sisältö salataan muiden käyttöön. Varat suojannut tallennustilan salaus automaattisesti salaamattomana ja sijoitetaan salattuja tiedostojärjestelmässä ennen koodaus ja halutessasi uudelleen salataan ennen lataamista takaisin kuin uuden tulostus-resurssi. Tallennustilan salauksen ensisijainen käyttötapaus on, kun haluat suojata laadukkaita mediatiedostojen salauksesta osoitteessa muille käyttäjille levyn.
- **CommonEncryptionProtected** – Käytä tätä vaihtoehtoa, jos lataat sisältöä, joka on jo salattu ja suojattu Yleiset-salausta tai PlayReady DRM (esimerkiksi tasainen Streaming suojattu with PlayReady DRM).
- **EnvelopeEncryptionProtected** – Käytä tätä vaihtoehtoa, jos lataat HLS AES salattu. Huomaa, että tiedostot on koodattu ja salattu Transform hallinta.

**CreateFromFile** -menetelmällä voit myös määritettävä takaisinsoittonumero raportoiminen tiedoston latauksen edistymisestä.

Seuraavassa esimerkissä on määrittää **mitään** resurssi-asetukset.

Lisää ohjelma luokan seuraavalla tavalla.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


##<a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Koodata lähdetiedosto joukkoon mukautuvat bittitaajuus MP4-tiedostot

Kun ingesting varat kyselyjä Media Services, media voi olla koodattu transmuxed, vesileimattu, ja niin edelleen, ennen kuin se toimitetaan asiakkaille. Näitä toimia niin ja suorittamalla eri taustan roolin esiintymissä erinomainen suorituskyky ja käytettävyyden varmistamiseksi. Näitä toimintoja kutsutaan työt ja kunkin projektin koostuu atomisia tehtäviin, jotka tehdä työtä resurssi-tiedoston avulla.

Kun kerrottiin, kun käsittelet Azure Media Services, jokin tilanteissa tarjoaa mukautuvat bittitaajuus toistamisen asiakkaat. Media-palveluiden dynaamisesti voit pakata mukautuvat bittitaajuus MP4 tiedostojoukon johonkin seuraavista muodoista: HTTP Live Streaming (HLS), tasainen Streaming, MPEG VIIVA ja HDS (varten vain Adobe PrimeTime/Access asiakirjamalleja).

Jos haluat hyödyntää Dynaaminen pakkaus, seuraavasti:

- Koodata tai muuttaa oman mezzanine (lähde)-tiedostoksi mukautuvat bittitaajuus MP4-tiedostoja tai mukautuvat bittitaajuus tasainen Streaming tiedostoja joukkoon.  
- Vähintään yhden streaming yksikön streaming päätepisteen, josta aiot toimituksen sisällön hakeminen

Seuraava koodi esitetään, kuinka voit lähettää koodauksen työn. Työn sisältää tehtävän, joka määrittää koodauksen mezzanine tiedoston mukautuvat bittitaajuus MP4s käyttämällä **Media Encoder vakio**joukkoon. Koodin lähettää työn ja odota, kunnes se on valmis.

Kun työ on valmis, voi käyttää omaa resurssi tai ladata asteittain MP4-tiedostot, jotka on luotu koodauksen muuntamisen tuloksena.
Huomaa, että sinun ei tarvitse olla streaming yksiköt enemmän kuin 0, jotta Lataa asteittain MP4-tiedostot.

Lisää ohjelma luokan seuraavalla tavalla.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
    
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.
    
        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);
    
        Console.WriteLine("Submitting transcoding job...");
    
    
        // Submit the job and wait until it is completed.
        job.Submit();
    
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;
    
        Console.WriteLine("Transcoding job finished.");
    
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        return outputAsset;
    }

##<a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Julkaista kohteen ja poistaa URL-osoitteet streaming ja asteittain lataamista varten

Virtauttaa tai ladata sijoituksen, sinun on "julkaisemaan" locator luomalla. Locators on pääsy kohteen olevat tiedostot. Media-palveluiden tukee kahdentyyppisiä locators: OnDemandOrigin locators, mediavirtaa (kuten MPEG VIIVA, HLS tai tasainen Streaming) ja Access-allekirjoitus (SAS) locators käyttää ladattaessa mediatiedostot.

Kun olet luonut locators, voit luoda URL-osoitteet, joiden avulla virtauttaa tai ladata tiedostoja.


Streaming URL-osoite tasainen Streaming on seuraavanlainen:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS streaming URL-osoite on seuraavanlainen:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG VIIVA streaming URL-osoite on seuraavanlainen:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Tiedostojen lataaminen käytetään SAS URL-osoite on seuraavanlainen:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Media Services .NET SDK-laajennukset on kätevä helper menetelmiä, jotka palauttavat muotoiltu julkaistun kohteen URL-osoitteet.

Seuraava koodi käyttää .NET SDK tunnisteet locators luomiseen ja Hae streaming ja asteittain Lataa URL-osoitteet. Koodin näkyvät myös tiedostojen lataamisesta paikalliseen kansioon.

Lisää ohjelma luokan seuraavalla tavalla.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##<a name="test-by-playing-your-content"></a>Testaa sisällön toistaminen  

Kun suoritat määritetty edellisessä osassa ohjelma, URL-osoitteet seuraavankaltaiselta näkyvät konsoli-ikkunassa.

Mukautuvat streaming URL-osoitteet:

Tasainen Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG-VIIVA

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Nousevan Lataa URL (ääni ja video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Lähettää video [Azure mediasoitinta-palvelujen](http://amsplayer.azurewebsites.net/azuremediaplayer.html)avulla

Voit esikatsella asteittain Lataa-Liitä URL-osoite selaimen (esimerkiksi Internet Explorerissa, Chromessa tai Safarissa).


##<a name="next-steps-media-services-learning-paths"></a>Seuraavat vaiheet: Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


### <a name="looking-for-something-else"></a>Etsitkö jotain muuta?

Jos ohjeaihe ei sisällä mitä olet odottanut, puuttuu jotain tai -jollakin muulla tavalla ei vastaa tarpeitasi, anna meille palautetta, käyttäen alla Disqus viestiketjun.


<!-- Anchors. -->


<!-- URLs. -->
  [Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
  [Portal]: http://manage.windowsazure.com/
