<properties
    pageTitle="Streaming Azure diagnostiikka tiedot käyttämällä tapahtuman keskittimet kuuma polku | Microsoft Azure"
    description="Havainnollistaa määrittäminen Azure diagnostiikka tapahtuman keskittimet-loppuun, mukaan lukien Yleisiä tilanteita, joissa on ohjeissa."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/14/2016"
    ms.author="sethm" />

# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Kuuma polku Azure diagnostiikka tietojen Streaming tapahtuman keskittimet avulla

Azure diagnostiikka tarjoaa joustavia tapoja arvot ja lokien kerääminen cloud services-näennäiskoneiden (VMs) ja siirtää tulokset Azure-tallennustilan. Maaliskuussa 2016 (SDK 2.9) aikavälin alkaen voit allas diagnostiikka kokonaan mukautetun tietolähteisiin ja siirtää kuuma polku tietojen sekunteina [Azure tapahtuman keskittimet](https://azure.microsoft.com/services/event-hubs/)avulla.

Tuetut tietotyypit ovat:

- Tapahtuman jäljitys for Windows (tapahtumien seuranta) tapahtumat
- Suorituskyvyn laskureita
- Windowsin tapahtumalokien
- Sovelluksen lokit
- Azure diagnostiikka infrastruktuurin lokit

Tässä artikkelissa kerrotaan, miten määrittää Azure diagnostiikka tapahtuman keskittimet lopusta loppuun. Seuraavat Yleisiä tilanteita, joissa kerrotaan myös ohjeet:

- Lokit ja tietoja, joista saat sinked tapahtuman porttiin mukauttaminen
- Voit muuttaa kunkin ympäristössä käyttömahdollisuudet
- Tapahtuman keskittimet stream tietojen tarkasteleminen
- Yhteyden vianmääritys  

## <a name="prerequisites"></a>Edellytykset

Tapahtuman keskittimet Azure diagnostiikka nopeuttaminen tuetaan pilvipalveluihin, VMs virtuaalikoneen asteikko joukot tai palvelun kangasta alkaen Visual Studio Azure SDK 2.9 ja vastaavan Azure-työkalut.

- Azure diagnostiikka tunniste 1.6 ([Azure SDK .NET 2.9 tai uudempi](https://azure.microsoft.com/downloads/) kohteena Tämä oletusarvon mukaan)
- [Visual Studio 2013 tai uudempi versio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Olemassa olevan käyttömahdollisuudet Azure-kansio-sovelluksessa käyttämällä *.wadcfgx* tiedoston ja jollakin seuraavista tavoista:
    - Visual Studio: [Azure pilvipalveluihin ja näennäiskoneiden diagnostiikan asetusten määrittäminen](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
    - Windows PowerShellin: [Ota diagnostiikan Azure Cloud Services-palveluissa PowerShellin avulla](../cloud-services/cloud-services-diagnostics-powershell.md)
- Tapahtuman keskittimet nimitilan valmisteltu kohden on artikkelissa [tapahtuman keskittimet käytön aloittaminen](./event-hubs-csharp-ephcs-getstarted.md)

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Azure diagnostiikka yhdistäminen tapahtuman keskittimet käsittelytoiminto

Azure diagnostiikka täyttyvät aina lokit ja arvot, oletusarvoisesti Azuren tallennustilaan-tiliin. Sovellus saattaa lisäksi allas tapahtuman porttiin lisäämällä uuden **poistumia** osan **WadCfg** elementin *.wadcfgx* tiedoston **PublicConfig** -kohtaan. Visual Studion *.wadcfgx* -tiedosto on tallennettu seuraava polku: **Cloud palvelun projektin** > **roolit** >  **(RoleName)** > **diagnostics.wadcfgx** tiedosto.

```
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```

Tässä esimerkissä tapahtuman pääsivuston URL-osoite on määritetty täydellinen nimitila tapahtumaa-toiminnossa: tapahtuman keskittimet nimitilan + "/" + tapahtumaa-toiminnossa nimi.  

Tapahtuman pääsivuston URL-osoite näkyy [Azure portal](http://go.microsoft.com/fwlink/?LinkID=213885) tapahtuman keskittimet koontinäytössä.  

Mikä tahansa kelvollinen merkkijono voidaan määrittää **allas** nimi, kunhan sama arvo käytetään johdonmukaisesti koko määritystiedoston.

> [AZURE.NOTE]  Voi olla myös muita poistumia, kuten *applicationInsights* määritetty sisältö. Azure Diagnostiikan avulla määritetään, jos kunkin käsittelytoiminto on määritetty myös **PrivateConfig** -osassa vähintään yksi poistumia.  

Tapahtuman keskittimet käsittelytoiminto myös on määritetty ja *.wadcfgx* määritystiedoston **PrivateConfig** -osan.

```
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="" key="" endpoint="" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
</PrivateConfig>
```

`SharedAccessKeyName` Arvon vastattava jaettu Access allekirjoitus (SAS)-näppäintä ja käytännön, joka on määritetty **Tapahtuman keskittimet** nimitilan. Siirry [Azure portal](https://manage.windowsazure.com)tapahtuman keskittimet Raporttinäkymät-ikkunan, **määritys** -välilehti ja määritä nimetyt käytännön (esimerkiksi "SendRule"), joka *lähettää* käyttöoikeudet. **StorageAccount** on määritetty myös **PrivateConfig**. Ei ole tarpeen, jos haluat muuttaa tähän arvot, jos ne toimivat. Tässä esimerkissä jäädä arvot tyhjä, joka on merkki, että edeltävät resurssi asetettu arvoja. Esimerkiksi *ServiceConfiguration.Cloud.cscfg* ympäristön kokoonpanotiedosto asettaa ympäristön mukaiset nimet ja avaimet.  

> [AZURE.WARNING] Vain teksti- *.wadcfgx* tiedoston tallennetaan tapahtuman keskittimet SAS-näppäintä. Usein avaimeen on kuitattu sisään lähdekoodin hallinta tai on käytettävissä muodosta palvelimellesi annetaan, joten on suojata tarpeen mukaan. On suositeltavaa, että käytät SAS-avain *Lähetä vain* käyttöoikeuksien avulla niin, että joku voit kirjoittaa tapahtumaa-toiminnossa, mutta ei sen kuunteleminen tai hallita sitä.

## <a name="configure-azure-diagnostics-logs-and-metrics-to-sink-with-event-hubs"></a>Azure diagnostiikka-lokit ja mittarit allas kanssa tapahtuman keskittimet määrittäminen

Kuten edellä kerrottiin, kaikki oletusarvoiset ja mukautetut diagnostiikka tietojen, arvot ja lokit, on automaattisesti sinked Azuren tallennustilaan määritetyn välein. Tapahtuman keskittimet ja kaikki muut käsittelytoiminto voit määrittää minkä tahansa pääkansion tai lehti solmu hierarkiassa olevan sinked kanssa tapahtumaa-toiminnossa. Tämä sisältää tapahtumien seuranta tapahtumia, suorituskyvyn laskureita, Windowsin tapahtumalokien ja sovelluksen lokit.   

On tärkeä ottaa huomioon arvopisteiden todella voidaan siirtää tapahtuman porttiin. Yleensä kehittäjät siirtää pieni viive kuuman polku tiedot, joita on käytetty ja tulkita nopeasti. Esimerkkejä ovat järjestelmät, valvominen ilmoitusten tai Automaattinen skaalaus-sääntöjä. Kehittäjä voi myös määrittää vaihtoehtoiset analysis-kaupan tai Etsi kaupan – esimerkiksi Azure Stream Analytics, Elasticsearch, mukautetut valvonta järjestelmän tai tuttuja valvonta järjestelmän muilta.

Seuraavassa on esimerkki jotkin määritykset.

```
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
</PerformanceCounters>
```

Seuraavassa esimerkissä käsittelytoiminto otetaan käyttöön ylemmän tason **PerformanceCounters** solmu hierarkiassa, mikä tarkoittaa sitä, kaikki lapsen **PerformanceCounters** lähetetään tapahtuma-porttiin.  

```
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```

Edellisen esimerkin käsittelytoiminto käytetään vain kolme laskureita: **Pyyntöjä jonossa**, **Pyyntöjä hylätty**ja **% Suoritinaika**.  

Seuraavassa esimerkissä esitetään, miten kehittäjä voi rajoittaa lähetetyt tiedot kriittinen arvot, joita käytetään tämän palvelun kunto määrää.  

```
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```

Tässä esimerkissä käsittelytoiminto otetaan käyttöön lokit ja suodatetaan vain tason Jäljitä virhe.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Ottaa käyttöön ja Päivitä Cloud Services-sovellusten ja diagnostiikka config

Visual Studio on helpoin polun ottaa käyttöön sovelluksen ja tapahtuman keskittimet käsittelytoiminto määritys. Voit tarkastella ja muokata tiedostoa, *.wadcfgx* -tiedoston avaaminen Visual Studio, muokata ja tallentaa sen. Polku on **Cloud palvelun projektin** > **roolit** > **(RoleName)** > **diagnostics.wadcfgx**.  

Tässä vaiheessa käyttöönotto- ja käyttöönoton Päivitä toiminnot Visual Studiossa, Visual Studio Team Systemin, ja kaikki komennot tai komentosarjoja, jotka perustuvat MSBuild ja käyttöä **/t: Julkaise** kohde sisällyttäminen *.wadcfgx* pakkaus prosessi. Lisäksi ominaisuuksissa ja päivitykset käyttöön tiedoston Azure käyttämällä omaa VMs Azure diagnostiikka agentti tunniste.

Kun asennat sovelluksen ja Azure diagnostiikka määritykset, näet välittömästi tehtävän tapahtumaa-toiminnossa raporttinäkymät-ikkunassa. Tämä ilmaisee, että olet valmis siirtymään kuuman polku-tietojen tarkasteleminen listener asiakas- tai analysis-työkalun valittua.  

Seuraavassa kuvassa tapahtuman keskittimet Raporttinäkymät-ikkunan näyttää diagnostiikka tietojen kunnossa lähettäminen tapahtuma-keskittimeen alkavat viime 11 PM. Milloin on sovellus on otettu käyttöön päivitetyn *.wadcfgx* -tiedosto ja käsittelytoiminto on määritetty oikein.

![][0]  

> [AZURE.NOTE] Kun teet päivitykset Azure diagnostiikka määritystiedosto (.wadcfgx), on suositeltavaa push päivitykset koko sovelluksen sekä määritykset Visual Studio julkaiseminen tai Windows PowerShell-komentosarjaa.  

## <a name="view-hot-path-data"></a>Oikotie polku tietojen tarkasteleminen

Kuten edellä aiemmin on käyttää usein kuunteleminen ja tapahtuman keskittimet tietojen käsittelemistä.

Yksi yksinkertainen tapa on kuunnella tapahtumaa-toiminnossa ja tulostaa tulosteen virta pieni testi console-sovelluksen luominen. Voit sijoittaa seuraava koodi, jossa selitetään tarkemmin [tapahtuman keskittimet käytön aloittaminen](./event-hubs-csharp-ephcs-getstarted.md)), console-sovelluksessa.  

Huomaa, että console-sovellus on sisältävät [tapahtuman suoritin Host Nuget paketti](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Muista kulmasulkeiden **Main** -funktiossa arvojen korvaaminen resurssien arvoilla.   

```
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event Hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sink"></a>Tapahtuman keskittimet käsittelytoiminto vianmääritys

- Tapahtuma-toiminnossa ei näy vastaanotettaville tai lähetettäville tapahtuman toiminnon odotetulla tavalla.

    Tarkista, että tapahtumaa-toiminnossa on valmisteltu. Kaikki *.wadcfgx* **PrivateConfig** -osassa yhteyden tiedot on oltava samat arvot resurssin tarkastelu-portaalissa. Varmista, että olet SAS käytännön määritetyt ("SendRule" esimerkissä) portaalissa ja *lähettää* käyttöoikeus myönnetään.  

- Tapahtuma-toiminnossa näyttää päivityksen jälkeen ei enää vastaanotettaville tai lähetettäville tapahtuman toiminnon.

    Varmista ensin, että tapahtumaa-toiminnossa- ja määrityspalveluja tiedot ovat oikein aiemmin kuvatulla. Joskus **PrivateConfig** palautetaan käyttöönotto-päivitys. Suositeltu korjaustyökalu on *.wadcfgx* projektin kaikkien muutosten ja push valmis sovelluksen päivitys. Jos tämä ei ole mahdollista, varmista, että diagnostiikka päivitys Vie valmis **PrivateConfig** , joka sisältää SAS-näppäintä.  

- Yritin ehdotuksiin ja tapahtumaa-toiminnossa ei vieläkään toimi.

    Onko muistikirjassa Azuren tallennustilaan taulukko, joka sisältää lokit ja virheiden Azure diagnostiikan asetuksia itse: **WADDiagnosticInfrastructureLogsTable**. Yksi vaihtoehto on käyttää työkalua, kuten [Azure-tallennustilan Explorer](http://www.storageexplorer.com) Yhdistä tallennustilan tähän tiliin, Näytä tämän taulukon ja lisää kyselyyn aikaleima viimeisen 24 tunnin aikana. Vie .csv-tiedosto ja avaa se jokin sovellus, kuten Microsoft Excel-työkalulla. Excel on helppo etsiä soitat kortin merkkijonoja, kuten **EventHubs**, näet, mitä virhe raportoidaan.  

## <a name="next-steps"></a>Seuraavat vaiheet

• [Lisätietoja tapahtuman keskittimet](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Lisäys: Viimeistele Azure diagnostiikka määritys (.wadcfgx) Esimerkki

```
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount />
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" key="" endpoint="" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

Tässä esimerkissä komplementtivirhefunktion *ServiceConfiguration.Cloud.cscfg* näyttää seuraavalta.

```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png
