<properties
   pageTitle="Käytä suorituskyvyn laskureita Azure diagnostiikka | Microsoft Azure"
   description="Suorituskyvyn laskureita Azure pilvipalveluihin tai virtuaalikoneen avulla voit etsiä pullonkaulojen ja suorituskyvyn parantamisesta."
   services="cloud-services"
   documentationCenter=".net"
   authors="rboucher"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/29/2016"
   ms.author="robb" />

# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Luominen ja käyttäminen suorituskyvyn laskureita Azure-sovelluksessa

Tässä artikkelissa kuvataan edut ja asettaminen suorituskyvyn laskureita Azure-sovellukseen. Voit käyttää niitä tietojen kerääminen ja löydä pullonkaulojen järjestelmän ja sovelluksen suorituskyvyn parantamisesta.

Suorituskyvyn laskureita käytettävissä Windows Server, IIS ja ASP.NET myös voidaan kerätä ja käyttää Azure web roolit, työntekijä roolit ja näennäiskoneiden terveydentilasta. Voit myös luoda ja käyttää mukautettuja suorituskyvyn laskureita.  

Voit tarkastella suorituskykytietoja laskuri
1. Suoraan isännässä sovellus käyttää etätyöpöydän suorituskyvyn valvonta-työkalulla
2. System Center Operations Manager kanssa käyttämällä Azure Management Pack
3. Muut seurantaa työkaluja, joilla käyttää diagnostiikan tietoja siirretään Azure-tallennustilan. Lisätietoja on kohdassa [tallentaminen ja diagnostiikan tietojen Azuren tallennustilaan](https://msdn.microsoft.com/library/azure/hh411534.aspx) .  

Saat lisätietoja sovelluksen [Azure perinteinen portal](http://manage.azure.com/)suorituskyvyn valvominen Tutustu [näytön pilvipalveluihin](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Ohjeita muita perusteellisempaa kirjaaminen ja jäljityksen määrittäminen ja käyttäminen diagnostiikka- ja muita menetelmiä ongelmien vianmäärityksessä ja optimoi Azure sovellusten luomisesta on artikkelissa [Vianmääritys parhaat käytännöt Azure-sovellusten kehittämisestä](https://msdn.microsoft.com/library/azure/hh771389.aspx).


## <a name="enable-performance-counter-monitoring"></a>Ota käyttöön suorituskyvyn laskuri valvonta

Suorituskyvyn laskureita eivät ole käytössä oletusarvoisesti. Sovelluksen tai käynnistys tehtävä on muokattava sisältämään suorituskykylaskureita, jota haluat seurata roolin jokaiselle esiintymälle oletusarvon diagnostiikka agent-määritys.

### <a name="performance-counters-available-for-microsoft-azure"></a>Suorituskyvyn laskureita käytettäväksi Microsoft Azure

Azure tarjoaa alijoukkoa suorituskyvyn laskureita käytettävissä Windows Server, IIS ja ASP.NET-pino. Seuraavassa taulukossa on lueteltu joitakin kiinnostavissa Azure-sovellusten suorituskykylaskureita.

|Laskuri-luokka: Objektin (esiintymän)|Laskuri-nimi      |Viittaus|
|---|---|---|
|.NET CLR poikkeukset (_Yleiset_)|# Virhe ilmenee / sec Exceps   |Poikkeus suorituskyvyn laskureita|
|.NET CLR-muistia (_Yleiset_)    |Yleisen luettelon aikaa %            |Muistin suorituskykylaskureita|
|ASP.NET                      |Sovellus käynnistyy    |ASP.NET suorituskykylaskureita|
|ASP.NET                      |Pyynnön suoritusaika  |ASP.NET suorituskykylaskureita|
|ASP.NET                      |Yhteys katkaistu pyynnöt   |ASP.NET suorituskykylaskureita|
|ASP.NET                      |Työntekijän prosessin käynnistyessä |ASP.NET suorituskykylaskureita|
|ASP.NET-sovelluksia (__Summa__)|Pyyntöjä yhteensä        |ASP.NET suorituskykylaskureita|
|ASP.NET-sovelluksia (__Summa__)|Pyyntöjä/s          |ASP.NET suorituskykylaskureita|
|ASP.NET-v4.0.30319           |Pyynnön suoritusaika  |ASP.NET suorituskykylaskureita|
|ASP.NET-v4.0.30319           |Pyynnön odotusaika       |ASP.NET suorituskykylaskureita|
|ASP.NET-v4.0.30319           |Pyynnöt nykyinen        |ASP.NET suorituskykylaskureita|
|ASP.NET-v4.0.30319           |Pyyntöjä jonossa         |ASP.NET suorituskykylaskureita|
|ASP.NET-v4.0.30319           |Hylätyt pyynnöt       |ASP.NET suorituskykylaskureita|
|Muisti                       |Megatavuja käytettävissä        |Muistin suorituskykylaskureita|
|Muisti                       |Vahvistettu tavua         |Muistin suorituskykylaskureita|
|Suoritin(_Yhteensä)            |% Suoritinaika        |ASP.NET suorituskykylaskureita|
|TCPv4                        |Epäonnistuneita     |TCP-objekti|
|TCPv4                        |Yhteyksiä |TCP-objekti|
|TCPv4                        |Yhteydet-Palauta       |TCP-objekti|
|TCPv4                        |Lähetettyjä osia/s       |TCP-objekti|
|Verkon Interface(*)         |Vastaanotettuja tavua/s      |Verkko-liittymän objekti|
|Verkon Interface(*)         |Lähetettyjä tavua/s          |Verkko-liittymän objekti|
|Verkko-liittymän (Microsoft virtuaalikoneen Bus verkon sovittimen _2)|Vastaanotettuja tavua/s|Verkko-liittymän objekti|
|Verkko-liittymän (Microsoft virtuaalikoneen Bus verkon sovittimen _2)|Lähetettyjä tavua/s|Verkko-liittymän objekti|
|Verkko-liittymän (Microsoft virtuaalikoneen Bus verkon sovittimen _2)|Tavua yhteensä/s|Verkko-liittymän objekti|

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Luoda ja lisätä mukautettuja suorituskyvyn laskureita sovelluksen

Azure on tuki voit luoda ja muokata mukautettuja suorituskyvyn laskureita web roolit ja työntekijä roolit. Laskureita voidaan seurata ja valvoa sovellusta kielikohtaiset toiminta. Voit luoda ja mukautetun suorituskyvyn laskuri luokat ja määritteet poistaminen käynnistys tehtävän, web-roolin tai työntekijän rooli järjestelmänvalvojan oikeuksilla.

>[AZURE.NOTE] Koodi, joka tekee muutoksia mukautetun suorituskyvyn laskureita on on ylemmän tason käyttöoikeudet suorittamiseen. Jos koodi on web-roolin tai työntekijän rooli, rooli on sisällettävä tunnisteen <Runtime executionContext="elevated" /> roolin alustus ServiceDefinition.csdef-tiedostossa.

Voit lähettää mukautetun suorituskykytietoja laskuri Azure tallennustilan käyttämällä diagnostiikka-agentti.

Vakio suorituskykytietoja laskuri luodaan Azure prosessit. Web-roolin tai työntekijän rooli sovelluksen on luotava mukautettu suorituskykytietoja laskuri. Lisätietoja [Suorituskyvyn tyypit](https://msdn.microsoft.com/library/z573042h.aspx) , jotka voidaan tallentaa mukautettuja suorituskyvyn laskureita tietotyyppien. Katso esimerkki, joka luo ja määrittää mukautetun suorituskykytietoja laskuri-web-roolin [PerformanceCounters malli](http://code.msdn.microsoft.com/azure/) .

## <a name="store-and-view-performance-counter-data"></a>Tallenna ja Näytä laskuri käytettävissä

Azure välimuistiin laskuri suorituskykytietoja vianmääritystiedot muiden kanssa. Nämä tiedot ovat käytettävissä remote seuranta roolin esiintymää on käynnissä työpöydän etäkäyttöpalvelimen avulla voit tarkastella työkaluilla, kuten suorituskyvyn valvonta. Säilyttää tiedot ulkopuolella roolin esiintymän diagnostiikka-agentti on siirrettävä tiedot Azure-tallennustilan. Välimuistiin tallennetut suorituskykytietoja laskuri kokorajoitus voi määrittää monin diagnostiikka-agentti tai se on määritetty osaksi jaetun rajoitukset diagnostiikan tietojen. Saat lisätietoja asetusten puskurikoko [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) ja [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Katso [tallentaminen ja diagnostiikan tietojen Azuren tallennustilaan](https://msdn.microsoft.com/library/azure/hh411534.aspx) yleiskatsaus diagnostiikka-agentti tietojen siirtäminen tallennustilan tilin määrittämisestä.

Määritetty suorituskyvyn laskuri jokaiselle esiintymälle on tallennettu, joka on määritetty ja näytteeksi tiedot siirretään tallennustilan tilin ajoitetun siirto pyyntö, tai tarvittaessa siirtopyynnön. Automaattinen siirrot voidaan ajoittaa niin usein kuin kerran minuutissa. Laskuri suorituskykytietoja diagnostiikka-agentti siirtämien tallennetaan taulukon, WADPerformanceCountersTable-tallennustilan tilin. Tämä taulukko voi käyttää ja kysely vakio Azure-tallennustilan API menetelmillä. Katso esimerkki kyselyt ja suorituskyvyn laskuri WADPerformanceCountersTable-taulukon tiedot näytetään [Microsoft Azure PerformanceCounters malli](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) .

>[AZURE.NOTE] Mukaan diagnostiikka agentti siirto korkojakso ja jonon viive viimeisimmän laskuri suorituskykytietoja tallennustilan tilin voi olla muutaman minuutin ajan tasalla.

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Käytä diagnostiikka määritystiedostoa suorituskyvyn laskureita ottaminen käyttöön

Noudattamalla seuraavia vaiheita voit ottaa käyttöön suorituskyvyn laskureita Azure-sovelluksessa.

## <a name="prerequisites"></a>Edellytykset

Tässä osassa oletetaan, että on tuotu sovelluksen diagnostiikka-näyttö ja diagnostiikka-kokoonpanotiedosto lisätään Visual Studio-ratkaisun (diagnostics.wadcfg SDK 2.4 ja alla tai diagnostics.wadcfgx SDK 2,5 ja edellä). Katso vaiheet 1 ja 2 [Ottaminen käyttöön diagnostiikka Azure Cloud Services-palveluissa ja näennäiskoneiden](./cloud-services-dotnet-diagnostics.md)) lisätietoja.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Vaihe 1: Kerätä ja tallentaa tiedot suorituskyvyn laskureita

Kun olet lisännyt diagnostiikka-tiedoston Visual Studio-ratkaisuun, voit määrittää kerääminen ja suorituskyvyn Laskuritietojen säilyttäminen Azure-sovelluksessa. Tämä tapahtuu lisäämällä suorituskyvyn laskureita diagnostiikka-tiedostoon. Diagnostiikan tiedot, myös suorituskyvyn laskureita kerätään ensimmäisen esiintymän. Tiedot säilytetään sitten WADPerformanceCountersTable taulukkoon Azure-taulukosta-palvelussa, joten sinun on myös Määritä tallennustilan sovelluksen. Jos olet testaat sovelluksen paikallisesti Laske emulaattori, voit myös tallentaa diagnostiikka tietojen paikallisesti tallennustilan emulaattori. Ennen kuin tallennat diagnostiikka tiedot on ensin Siirry [Azure perinteinen portal](http://manage.windowsazure.com/) ja tallennustilaa tilin luominen. Paras käytäntö on paikantaa tallennustilan tilin Azure sovelluksen samaan geo-sijaintiin siten vältät maksaa ulkoisen kaistanleveyden kustannuksia ja vähentää viive.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Lisää suorituskyvyn laskureita diagnostiikka-tiedostoon

Voit käyttää paljon laskureita on. Seuraavassa esimerkissä on useita suorituskyvyn laskureita, on suositeltavaa web ja työntekijän rooli seurantaa.

Avaa diagnostiikka-tiedoston (diagnostics.wadcfg SDK 2.4 ja alla tai diagnostics.wadcfgx SDK 2,5 ja yläpuolella) ja lisää seuraava teksti DiagnosticMonitorConfiguration osan:

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

BufferQuotaInMB-määrite, joka määrittää järjestelmän tiedostosäilön, joka on käytettävissä sivustokokoelman tietotyyppi (esimerkiksi Azure lokit, IIS-lokeja) enimmäismäärän. Oletusarvo on 0. Kun kiintiön on saavutettu, vanhin tiedot poistetaan, koska uudet tiedot lisätään. Kaikki bufferQuotaInMB ominaisuudet summa on oltava suurempi kuin OverallQuotaInMB-määritteen arvon. Yksityiskohtaisempia keskustelun määrittämiseksi, kuinka paljon tallennustilaa on suoritettava diagnostiikka tietojen keräämistä varten on kohdassa asetukset WAD [vianmääritys parhaiden](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx)käytäntöjen Azure-sovellusten kehittämisestä.

ScheduledTransferPeriod-määrite, joka määrittää ajoitetun siirrot väli pyöristettynä lähimpään minuutit. Seuraavissa esimerkeissä se asetetaan PT30M (30 minuuttia). Siirto-ajan asettaminen pieni arvoa, kuten 1 minuutti heikentää sovelluksen suorituskykyä tuotannon, mutta voi olla hyötyä työskentelyä nopeasti, kun käytät pelkkiä Diagnostiikka. Ajoitettu siirto kauden on oltava pieni varmistaa, että diagnostiikkatiedot ei korvata esiintymässä, mutta riittävän suuri, ettei se häiritse sovelluksen suorituskykyä.

CounterSpecifier-määrite määrittää kerääminen suorituskyvyn laskuri. SampleRate-määrite määrittää nopeus, jolla suorituskyvyn laskuri pitäisi olla näyte, tällöin 30 sekuntia.

Kun olet lisännyt suorituskyvyn laskureita, jotka haluat kerätä, Tallenna tekemäsi muutokset diagnostiikka-tiedostoon. Seuraavaksi haluat määrittää tallennustilan-tili, jolla diagnostiikka tiedot säily.

### <a name="specify-the-storage-account"></a>Määritä tallennustilan-tili

Säilyttää tietosi diagnostiikka Azure-tallennustilan tilin määritys (ServiceConfiguration.cscfg)-palvelutiedosto on määritettävä yhteysmerkkijonon.

Azure SDK 2,5 tallennustilan tilin voidaan määrittää diagnostics.wadcfgx-tiedostossa.

>[AZURE.NOTE] Nämä ohjeet koskevat ainoastaan Azure SDK 2.4 ja alapuolella. Azure SDK 2,5 tallennustilan tilin voidaan määrittää diagnostics.wadcfgx-tiedostossa.

Voit määrittää yhteyden merkkijonot seuraavasti:

1. Avaa ServiceConfiguration.Cloud.cscfg tiedoston tuttuja tekstieditorissa ja tallennustilaa yhteysmerkkijono. *AccountName* ja *AccountKey* arvoja löytyy tallennustilan tilin koontinäytön-kohdassa Hallitse näppäimet Azure perinteinen-portaalissa.

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Tallenna tiedosto ServiceConfiguration.Cloud.cscfg.

3. Avaa ServiceConfiguration.Local.cscfg-tiedosto ja varmista, että UseDevelopmentStorage on määritetty tosi.

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
Nyt, kun yhteysmerkkijonot on määritetty, sovelluksen jatkuvat diagnostiikka tallennustilan tilin tiedot, kun sovellus otetaan käyttöön.
4. Tallenna ja luo projektin ja valitse sovelluksen käyttöönotto.

## <a name="step-2-optional-create-custom-performance-counters"></a>Vaihe 2: (Valinnainen) luo mukautettu suorituskyvyn laskureita

Ennalta määritetyt suorituskykylaskureita lisäksi voit lisätä omia mukautettuja suorituskyvyn laskureita seurannassa verkossa tai työntekijä roolit. Mukautetun suorituskyvyn laskureita voidaan seurata ja valvoa sovellusta kielikohtaiset toiminta ja voi luoda tai poistaa käynnistys tehtävän, web-roolin tai työntekijän rooli järjestelmänvalvojan oikeuksilla.

Azure diagnostiikka-agentti päivittää suorituskyvyn laskuri määritykset .wadcfg tiedostosta minuutin aloittamisen jälkeen.  Jos luot mukautetun suorituskyvyn laskureita OnStart menetelmä ja käynnistys tehtävien kestää kauemmin kuin minuutin suorittamaan, mukautettu suorituskyvyn laskureita ei ole luotu kun Azure diagnostiikka-agentti yrittää ladata ne.  Tässä skenaariossa näet Azure diagnostiikka oikein sieppaa kaikki paitsi mukautetun suorituskyvyn laskureita diagnostiikka tiedot.  Voit ratkaista ongelman suorituskyvyn laskureita luominen käynnistys tehtävän tai Siirrä osa käynnistys tehtävän toimi OnStart menetelmä suorituskyvyn laskureita luomisen jälkeen.

Seuraavien toimien luomaan yksinkertainen mukautetun suorituskyvyn laskuri nimeltä "\MyCustomCounterCategory\MyButton1Counter":

1. Avaa sovellus palvelun määritystiedosto (CSDEF).
2. Lisää Runtime elementti WebRole tai WorkerRole elementin oikeuksin suorittamiseen:

    ```
    <runtime executioncontext="elevated"/>
    ```
3. Tallenna tiedosto.
4. Avaa diagnostiikka-tiedosto (diagnostics.wadcfg SDK 2.4 ja alla tai diagnostics.wadcfgx SDK 2,5 ja yläpuolella) ja lisää seuraava teksti DiagnosticMonitorConfiguration 

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Tallenna tiedosto.
6. Luo mukautettu suorituskyvyn laskuri-luokan tehtäväsi, OnStart maksutavan ennen käynnistettäessä base. OnStart. Seuraava C#-esimerkki luo mukautettu luokka, jos sitä ei ole jo:

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. Päivitä laskureita sovelluksessa. Seuraavassa esimerkissä päivittää mukautetun suorituskyvyn laskuri Button1_Click tapahtumista:

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. Tallenna tiedosto.  

Mukautetun suorituskykytietoja laskuri kerätään Azure diagnostiikka tarkkailu.

## <a name="step-3-query-performance-counter-data"></a>Vaihe 3: Kyselyn suorituskykytietoja laskuri

Kun sovellus on otettu käyttöön ja diagnostiikka-näytön käynnissä alkaa kerääminen suorituskyvyn laskureita ja toteaa Azure tallennustilan tiedot. Työkaluilla, kuten palvelimen Explorer Visual Studiossa, [Azure-tallennustilan Explorer](http://azurestorageexplorer.codeplex.com/)tai Cerebrata mukaan [Azure diagnostiikka hallinnan](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) avulla voit tarkastella laskureita suorituskykytietoja WADPerformanceCountersTable taulukossa. Voit tehdä kyselyn myös ohjelmallisesti käyttämällä [C#](../storage/storage-dotnet-how-to-use-tables.d), [Java](../storage/storage-java-how-to-use-table-storage.md), [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [Ruby](../storage/storage-ruby-how-to-use-table-storage.md)tai [PHP](../storage/storage-php-how-to-use-table-storage.md)taulukon palvelu.

Seuraava C# Esimerkki Yksinkertainen kysely WADPerformanceCountersTable taulukossa ja diagnostiikka-tiedot tallennetaan CSV-tiedostoon. Kun suorituskyvyn laskureita on tallennettu CSV-tiedoston voit Visualisoi tietoja Microsoft Excel-tai muuta työkalua graphing ominaisuuksia. Muista lisätä viittauksen Microsoft.WindowsAzure.Storage.dll, joka on sisältyy .NET lokakuussa 2012 Azure SDK ja uudempi versio. Kokoonpano on asennettu ohjelma Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ %-kansio.

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

Kohteiden liittäminen C# objektien käyttäminen **TableEntity**johdettu mukautetun luokan. Seuraava koodi määrittää toimija-luokka, joka edustaa suorituskyvyn laskuri **WADPerformanceCountersTable** taulukossa.


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }


## <a name="next-steps"></a>Seuraavat vaiheet
[Tarkastella muita artikkeleita Azure diagnostiikka käyttöön] (.. / azure diagnostics.md)
