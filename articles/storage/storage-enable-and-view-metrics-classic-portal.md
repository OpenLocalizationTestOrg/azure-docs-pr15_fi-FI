<properties 
    pageTitle="Tallennustilan arvot Azure-portaalissa ottaminen käyttöön | Microsoft Azure" 
    description="Tallennustilan arvot Blob, jonossa taulukon tai tiedosto-palveluiden ottaminen käyttöön" 
    services="storage" 
    documentationCenter="" 
    authors="robinsh" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/03/2016" 
    ms.author="robinsh"/>

# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a>Tallennustilan arvot ottaminen käyttöön ja arvot tietojen tarkasteleminen

[AZURE.INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Yleiskatsaus

Oletusarvon mukaan tallennustilan arvot ei ole käytössä liittyviä palveluja. Voit ottaa käyttöön joko [Azure perinteinen Portal](https://manage.windowsazure.com)-Windows PowerShellin seuranta tai ohjelmallisesti tallennustilan Ohjelmointirajapinnan kautta.

Kun otat tallennustilan arvot, sinun on valittava tietojen säilytysaika: tänä määrittää, kuinka kauan tallennustilan palvelu säilyttää arvot ja olet välilyönti tarvitse tallentaa ne ovat. Yleensä käytettävä lyhyempi säilytysaika minuutit arvot kuin tunnin välein arvot vuoksi minuutit arvot vaaditaan merkittäviä ylimääräistä tilaa. Valitse säilytysaika siten, että sinulla on riittävästi aikaa, voit analysoida tietoja ja ladata kaikki arvot säilytettävä offline-tilassa analyyseja tai raportointia varten. Muista, että sinua myös laskuteta arvot tietojen lataamisen tallennustilan-tililtä.

## <a name="how-to-enable-storage-metrics-using-the-azure-classic-portal"></a>Tallennustilan arvot Azure perinteinen portaalissa ottaminen käyttöön

[Perinteinen Azure-portaalissa](https://manage.windowsazure.com)voit käyttää Määritä sivun tallennustilan tilin ohjausobjektiin tallennustilan arvot. Seuranta, voit määrittää tasolle ja säilytysaika kunkin BLOB-objektit, taulukoita ja olevien päivien. Kaikissa tapauksissa taso on jokin seuraavista:

- Ei käytössä, Ei ole arvot kerätään.

- Vähäinen – Tallennustilan arvot kerää arvot, kuten tunkeutumisen/juniin, käytettävyyttä, viive ja success prosenttiosuudet, jotka yhdistetään Blob, taulukon ja jonon tukipalvelujen basic joukko.

- Yksityiskohtainen – Tallennustilan arvot kerää joukon arvot, joka sisältää kullekin tallennustilan API-toiminnon lisäksi palvelun tason arvot saman mittaukset. Yksityiskohtainen arvot ottaa käyttöön sovelluksen toimintojen aikana ilmenevät ongelmat tarkempi analyysi.

Huomaa, että Azure perinteinen portaalin ei tällä hetkellä, joiden avulla voit määrittää minuutit arvot tallennustilan; tilisi Sinun on otettava minuutit arvot PowerShellin avulla tai ohjelmallisesti.


## <a name="how-to-enable-storage-metrics-using-powershell"></a>Tallennustilan arvot PowerShellin ottaminen käyttöön

Paikallisessa tietokoneessa PowerShellin avulla voit määrittää tallennustilan arvot-tallennustilan tilin käyttämällä Azure PowerShell-cmdlet-komennon Get-AzureStorageServiceMetricsProperty hakea nykyisen asetuksia ja cmdlet-komento määrittäminen AzureStorageServiceMetricsProperty nykyisten asetusten muuttamiseen.

Cmdlet-komennot, jotka ohjaavat tallennustilan arvot parametreilla:

- MetricsType mahdolliset arvot ovat tunnit ja minuutit.

- ServiceType mahdolliset arvot ovat Blob, jonossa ja taulukon.

- MetricsLevel mahdolliset arvot ovat ei mitään (vastaa Azure perinteinen portaalissa Off)-palvelun (vastaa mahdollisimman vähän Azure perinteinen portaalissa) ja ServiceAndApi (vastaa yksityiskohtainen Azure perinteinen portaalissa).

Esimerkiksi seuraava komento siirtyy minuutit mittaukset blob-palvelusta on tallennustilan oletustilin määrittäminen kauden viisi päivää säilytys kanssa:

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

Seuraava komento hakee nykyisen tunnin välein arvot taso ja säilytyskäytäntöjä päivien tallennustilan oletustilin blob-palvelun:

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

Lisätietoja määrittäminen toimimaan Azure-tilauksesi ja niiden oletusarvoisen tallennustilan tili, jota haluat käyttää, Azure PowerShell cmdlet-komennot on: [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Tallennustilan arvot sallimisesta ohjelmallisesti

Seuraavat C#-koodikatkelman näytetään, miten arvot ja tallennustilaa asiakas-kirjaston käytön .NET Blob-palvelun kirjaaminen otetaan käyttöön:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Enable Storage Analytics logging and set retention policy to 10 days. 
    ServiceProperties properties = new ServiceProperties();
    properties.Logging.LoggingOperations = LoggingOperations.All;
    properties.Logging.RetentionDays = 10;
    properties.Logging.Version = "1.0";

    // Configure service properties for metrics. Both metrics and logging must be set at the same time.
    properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.HourMetrics.RetentionDays = 10;
    properties.HourMetrics.Version = "1.0";

    properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.MinuteMetrics.RetentionDays = 10;
    properties.MinuteMetrics.Version = "1.0";

    // Set the default service version to be used for anonymous requests.
    properties.DefaultServiceVersion = "2015-04-05";

    // Set the service properties.
    blobClient.SetServiceProperties(properties);
    
## <a name="viewing-storage-metrics"></a>Tallennustilan arvot tarkasteleminen

Kun olet määrittänyt tallennustilan arvot seurannassa tallennustilan tilin, se tallentaa määritetty tunnetun taulukoiden tallennustilaan tilisi joukko. Tallennustilan tilin Azure perinteinen portaalissa näyttö-sivun avulla voit tarkastella tunnin välein arvot, kun ne ovat käytettävissä kaaviossa. Tällä sivulla perinteinen Azure-portaalissa voit:

- Valitse mitkä arvot piirtää kaavion (käytettävissä olevat arvot valinta riippuu siitä, määrititkö yksityiskohtainen tai mahdollisimman vähän seuranta-palvelun määrittäminen-sivulla).


- Valitse aika-alue näyttää kaavion arvot.


- Valitse määritetty piirtää suoria tai suhteellisia asteikon avulla.


- Määritä sähköposti-ilmoituksia, joka ilmoittaa, kun tietyn metrijärjestelmä saavuttaa tietyn arvon.


Jos haluat ladata pitkään tallennustilan mittaukset tai analysoida paikallisesti, sinun on käyttää työkalua tai kirjoittaa lisäkoodin lukemaan taulukot. Sinun on ladattava analyysi minuutit mittaukset. Taulukot eivät näy, jos kaikkien taulukoiden luettelon tallennustilan tilisi, mutta voit käyttää niitä suoraan nimen mukaan. Useita kolmannen osapuolen tallennustilan selaaminen työkaluja voidaan ottaa huomioon nämä taulukot ja, joiden avulla voit tarkastella niitä suoraan (seuraavassa blogimerkinnässä [Microsoft Azure-tallennustilan hallinnasta](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) käytettävissä Työkalut luettelo).

### <a name="hourly-metrics"></a>Tunnin välein arvot
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minuutit arvot
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Kapasiteetti
- $MetricsCapacityBlob

Seuraavassa taulukossa on [Tallennustilan Analytics arvot Taulukkorakenteen](https://msdn.microsoft.com/library/azure/hh343264.aspx)löytyvät rakenteet tarkat tiedot. Esimerkki rivejä alapuolelle Näytä vain osaa käytettävissä olevat sarakkeet, mutta kuvaavat tallennustilan arvot tallentaa nämä arvot tapa tärkeitä ominaisuuksia:

| PartitionKey  |       RowKey       |                    Aikaleima | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Käytettävyys | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 |      käyttäjän; Kaikki      | 2014-05-22T11:01:16.7650250Z | 7             | 7                     | 4003         | 46801       | 100          | 104.4286          | 6.857143             | 100            |
| 20140522T1100 | käyttäjän; QueryEntities | 2014-05-22T11:01:16.7640250Z | 5             | 5                     | 2694         | 45951       | 100          | 143.8             | 7,8                  | 100            |
| 20140522T1100 |  käyttäjän; QueryEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 538          | 633         | 100          | 3                 | 3                    | 100            |
| 20140522T1100 | käyttäjän; UpdateEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 771          | 217         | 100          | 9                 | 6                    | 100               |

Tämän esimerkkitietoja minuutit arvot osio-näppäintä käyttää kellonaika minuutit tarkkuudella. Rivin tunnusta tunnistaa tietotyyppi, joka on tallennettu rivillä ja tämä koostuu kaksi kappaletta, käyttöoikeustyyppi ja pyynnön tyyppiä:

- Access-tyyppi on käyttäjän tai järjestelmä-kohtaa, johon käyttäjä viittaa tallennuspalvelu kaikki käyttäjäpyynnöt ja järjestelmän viittaa tallennustilan Analytics tekemät pyynnöt.

- Pyynnön tyyppi on-kaikki joka tapauksessa on yhteenveto rivi- tai se tunnistaa tietyn API, kuten QueryEntity tai UpdateEntity.


Esimerkkitiedot yllä näyttää kaikki tietueet yhden minuutin ajan (alkaa kello 11:00), jotta QueryEntities pyyntöjen määrä sekä QueryEntity määrän pyytää plus määrän UpdateEntity pyytää lisätä enintään seitsemän, joka on yhteensä näytetään käyttäjän: All-rivi. Vastaavasti voi saada keskimääräinen lopusta loppuun odotusaika 104.4286 käyttäjä: kaikki rivillä laskemalla ((143.8 * 5) + 3 + 9) / 7.

Ota huomioon Azure perinteinen portaalissa näytön sivulla ilmoitusten määrittäminen niin, että tallennustilan arvot automaattisesti voi ilmoittaa sinulle tärkeistä muutoksista tallennustilan palvelujen toimintaa. Jos Lataa arvot tiedoista erotinmerkkejä muodossa tallennustilan explorer-työkalun avulla, voit analysoida tietoja Microsoft Excelissä. Seuraavassa blogimerkinnässä [Microsoft Azure-tallennustilan hallinnasta](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) tallennustilaa Explorerin Työkalut luettelo.



## <a name="accessing-metrics-data-programmatically"></a>Arvot tietojen käyttäminen ohjelmallisesti

Seuraavassa luettelossa on otoksen C# koodi, joka käyttää kesto minuutteina minuutit mittaukset ja näyttää tulokset console-ikkuna. Tiedostossa käytetään Azure-tallennustilan kirjaston versio 4, joka sisältää CloudAnalyticsClient-luokka, joka helpottaa käyttäminen tallennustilan arvot taulukot.

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    
    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0 
    select entity;
    
    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }
    
    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
    
    }




## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Mitä ovat voit maksamaan kun otat tallennustilan arvot?

Voit luoda taulukon kohteet arvot pyynnöt peritään koskevat kaikkia Azuren tallennustilaan toimintoja normaalin korvauksen kirjoittaminen

Lue ja poista pyynnöt asiakkaan tietoihin arvot ovat myös laskutettavan on vakio.. Jos olet määrittänyt tietojen säilytyskäytännön, ovat ei peri kun Azuren tallennustilaan poistaa vanhoja arvot tietoja. Jos poistat analytics-tiedot, tilisi veloitetaan Poista toimille.

Arvot taulukot käyttämä kapasiteetti on myös laskutettavan: seuraavat avulla voit arvioida kapasiteetti käytettävät arvot tietojen tallentaminen:

- Jos tunnin välein palvelu käyttää jokaisella palvelulla jokaisen Ohjelmointirajapinnan, sitten noin 148 kt tiedot tallennetaan tunnissa arvot tapahtuman taulukoissa, jos olet ottanut sekä service API tason yhteenveto.

- Jos tunnin välein palvelu käyttää jokaisella palvelulla jokaisen Ohjelmointirajapinnan, sitten noin 12KB tiedot tallennetaan tunnissa arvot tapahtuman taulukoissa, jos olet ottanut vain palvelutason yhteenveto.

- Kapasiteetin taulukon BLOB-objektit on lisätty päivittäin (Jos käyttäjä on valinnut lokitiedot) kaksi riviä: Tämä tarkoittaa sitä, että päivittäin tämän taulukon kokoa suurentaa enintään 300 tavua.

## <a name="next-steps"></a>Seuraavat vaiheet:
[Tallennustilan Analytics kirjaaminen ja käyttää tietoja ottaminen käyttöön](https://msdn.microsoft.com/library/dn782840.aspx)
 