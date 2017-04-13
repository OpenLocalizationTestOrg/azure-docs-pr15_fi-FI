<properties 
    pageTitle="Jakaminen ja skaalausta Azure DocumentDB | Microsoft Azure"      
    description="Lue lisätietoja siitä, miten osioinnin works Azure DocumentDB, määrittämisestä, jakaminen ja osion näppäimet ja valitse sovelluksen oikeassa osio-näppäintä."         
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/> 

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="arramac"/> 

# <a name="partitioning-and-scaling-in-azure-documentdb"></a>Jakaminen ja Azure DocumentDB skaalaus
[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) on suunniteltu helpottamaan saavuttamiseksi nopea ja ennakoitavissa suorituskyky ja skaalattavuus saumattomasti sovelluksen kanssa, kun se kasvaa. Tässä artikkelissa on yleiskatsaus siitä, miten osioinnin toimii DocumentDB, ja siitä, miten voit määrittää DocumentDB sivustokokoelmat skaalata sovelluksia tehokkaasti.

Luettuasi tämän artikkelin osaat seuraaviin kysymyksiin:   

- Miten osioinnin työn Azure DocumentDB?
- Miten jakaminen DocumentDB määritetään
- Mitä osion avaimet ja miten oma sovelluksen oikean osio-näppäintä Valitse?

Aloita koodi, lataa projektin osoitteesta [DocumentDB suorituskyvyn testaaminen ohjaimen malli](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

## <a name="partitioning-in-documentdb"></a>DocumentDB jakaminen

DocumentDB voit tallentaa ja kyselyn rakenteeseen pienempi JSON-asiakirjojen millisekunnin tilauksen vastauksen kertaa tahansa tasolla. DocumentDB on säilöjen kutsutaan **sivustokokoelmat**tietojen tallentamista varten. Kokoelmien looginen resursseja ja voi olla fyysinen osioiden tai palvelimiin. Tallennustilan koko ja kokoelman valmistellun siirtonopeuden perusteella DocumentDB määräytyy osioiden määrää. DocumentDB jokaista osiota on kiinteä liittyy Suoritettaessa palautettua tallennustilaa ja suuren replikoida. Osion hallinta hallitsee täysin Azure DocumentDB ja sinun ei tarvitse monimutkaisia koodin kirjoittaminen ja osioiden hallinta. DocumentDB sivustokokoelmat ovat **käytännössä rajoittamaton** tallennustilan ja siirtonopeuden. 

Jakaminen on täysin läpinäkyvä-sovellukseen. DocumentDB tukee nopea lukuja ja kirjoituksia, SQL- ja LINQ kyselyt, JavaScript-pohjainen tapahtumien logiikan, yhdenmukaisuuden tasot ja tarkasti rajattuja käyttöoikeuksia REST API puhelut yhden sivustokokoelman resurssin kautta. Palvelun käsittelee jakelutavoista tietojen osiot ja reititys kyselyn pyynnöt oikealle-osioon. 

Miten tämä toimii? Kun luot DocumentDB kokoelma, huomaat, että on **osion avaimen ominaisuuden** määrittäminen arvo, joka voidaan määrittää. Tämä on JSON-ominaisuuden (tai polku) tiedostoissasi, jotka voidaan käyttää DocumentDB useita palvelimia tai osiot tietojen jakamiseen. DocumentDB tulee hash-osion avainarvon ja hajautettujen tulos avulla voit määrittää osion, jossa JSON-asiakirja tallennetaan. Kaikki tiedostot, joissa on sama osioavain tallennetaan samaan osioon. 

Esimerkkinä sovellus, joka tallentaa tiedot työntekijöiden ja niiden yksiköiden DocumentDB. Valitse seuraavaksi `"department"` , jos haluat skaalata tiedot pois osasto-osion avaimen ominaisuutena. DocumentDB Jokaisella tiedostolla on oltava pakollinen `"id"` -ominaisuus, joka on oltava yksilöllinen jokaiselle asiakirjalle samaa osion avaimen arvoa, kuten `"Marketing`". Jokaisen kokoelman tallennetun tiedoston on oltava yksilöllinen yhdistelmä osio-näppäintä ja tunnus, kuten `{ "Department": "Marketing", "id": "0001" }`, `{ "Department": "Marketing", "id": "0002" }`, ja `{ "Department": "Sales", "id": "0001" }`. Toisin sanoen koostetun ominaisuus, (osion key-tunnuksen) on kokoelma perusavain.

### <a name="partition-keys"></a>Osion näppäimet
Osion avaimen valinta on tärkeä päätös, sinun täytyy tehdä suunnitteluvaiheessa. Sinun on valittava JSON-ominaisuuden nimi, jossa on monien arvot ja todennäköisesti on tasaisesti access kuviot. Osion avain on määritetty JSON-polku, kuten `/department` edustaa ominaisuuden osasto. 

Seuraavassa taulukossa on esimerkkejä osion avaimen määritykset ja kunkin JSON-arvoja.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Osion polku</strong></p></td>
            <td valign="top"><p><strong>Kuvaus</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Vastaa missä doc tiedoston doc.department JSON-arvoa.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ ominaisuudet/nimi</p></td>
            <td valign="top"><p>Vastaa doc.properties.name missä asiakirjan asiakirjan (sisäkkäisiä-ominaisuus) JSON-arvoa.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Vastaa doc.id JSON-arvon (tunnuksen ja osion avaimen ovat samassa ominaisuutta).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ "osaston nimi"</p></td>
            <td valign="top"><p>Vastaa asiakirjan ["osaston nimi"] missä doc tiedoston JSON-arvoa.</p></td>
        </tr>
    </tbody>
</table>

> [AZURE.NOTE] Osion avaimen polun syntaksi on samankaltainen kuin polun määritystä indeksoinnin käytännön verkkopolut avaimen erotuksella, että polku vastaa arvoa sijaan-ominaisuutta, eli ei ole yleismerkki lopussa. Jos esimerkiksi määrität/osasto /? Kun indeksoida osasto-kohdassa arvot, Määritä /department mutta osion avaimen määritys. Osion avaimen polun implisiittisesti indeksoitu ja ei voi poistaa indeksoinnista käyttämällä indeksoinnin käytäntö korvaa.

Voit tarkastella, kuinka osion avaimen valinta vaikuttaa sovelluksen suorituskykyä.

### <a name="partitioning-and-provisioned-throughput"></a>Osioinnin ja valmisteltu
DocumentDB on suunniteltu ennakoitavissa suorituskykyä. Kun luot kokoelma, varata siirtonopeuden ** [pyynnön yksiköt](documentdb-request-units.md) (RU) sekunnissa**kannalta. Sivupyynnön määritetään pyynnön yksikön kulu, suhteutettu järjestelmäresurssit, kuten suorittimen ja IO toiminnon Kulutettu määrä. Luku 1 kt asiakirjasta, jossa istunnon yhdenmukaisuuden siinä käytetään 1 pyynnön yksikkö. Luku on 1 RU tallennetut kohteet numero tai käytössä samaan samanaikaiset pyynnöt riippumatta. Suurempien asiakirjojen edellyttävät suurempi pyynnön yksiköiden koon mukaan. Jos tiedät kokoa ja kohteiden määrän lukee tarvitset tukea sovelluksen, voit valmistella siirtonopeuden edellyttämistä sovelluksen käyttäjän lukea tarvitsee tarkka summa. 

Kun DocumentDB tallentaa asiakirjat, se jakaa ne tasaisesti osion avaimen arvon perusteella osioiden välillä. Siirtonopeuden myös jaetaan tasaisesti kesken käytettävissä olevista osioiden eli kohti osion siirtonopeuden = (yhteensä nopeus sivustokokoelman kohden) / (osioiden lukumäärä). 

>[AZURE.NOTE] Sinun on valittava osio-näppäintä, jonka avulla voit jakaa tasaisesti pyynnöt useista eri osion avainarvot saavuttamiseksi koko kokoelman siirtonopeuden.

## <a name="single-partition-and-partitioned-collections"></a>Osio ja osioitua sivustokokoelmat
DocumentDB tukee sekä yhden osion ja osioitua kokoelmien luomista. 

- **Partitioned sivustokokoelmat** voit kattavat useita osioita ja hyvin suuria määriä säilytys- ja siirtonopeuden tuki. Sinun on määritettävä kokoelman osio-näppäintä.
- **Yhden osion sivustokokoelmat** on pienempi hintatietoja ja mahdollisuuden pitää kyselyn ja tapahtumien suorittamista kaikki sivustokokoelman tietojen yli. Heillä on osio skaalattavuus ja tallennustilaa rajoissa. Ei tarvitse määrittää nämä sivustokokoelmat osion avain. 

![Osioitu sivustokokoelmat-DocumentDB][2] 

Tilanteessa, et tarvitse tallennustilan tai siirtonopeuden suuria tietomääriä osio sivustokokoelmat sopivat hyvin ratkaistavaan. Huomaa, että yhden osion sivustokokoelmien tallennustilan rajoitusten yksittäisen osion ja skaalattavuus eli enintään 10 Gigatavua lisätallennustilaa ja korkeintaan 10 000 pyynnön yksiköt sekunnissa. 

Osioitu sivustokokoelmat tukee hyvin suuria määriä säilytys- ja siirtonopeuden. Oletus-tarjouksia määritetään kuitenkin enintään 250 Gigatavua lisätallennustilaa tallentamiseen ja skaalata enintään 250,000 pyynnön yksiköt sekunnissa. Jos tarvitset ylemmälle tallennustilan tai siirtonopeuden kohti sivustokokoelman, ota yhteyttä [Azure-tuki](documentdb-increase-limits.md) on nämä parantavat tilissäsi.

Seuraavassa taulukossa on lueteltu yhden osion käyttämisestä ja osioitua sivustokokoelmat erot:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Yksittäisen osion sivustokokoelman</strong></p></td>
            <td valign="top"><p><strong>Osioitu sivustokokoelman</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Osion avain</p></td>
            <td valign="top"><p>Ei mitään</p></td>
            <td valign="top"><p>Pakollinen</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Asiakirjan perusavain</p></td>
            <td valign="top"><p>"tunnus"</p></td>
            <td valign="top"><p>koostetun avaimen &lt;osion avaimen&gt; ja "tunnus"</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Tallennustilan vähimmäiskoko</p></td>
            <td valign="top"><p>0 GT</p></td>
            <td valign="top"><p>0 GT</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Tallennustilan enimmäiskoko</p></td>
            <td valign="top"><p>10 GIGATAVUA</p></td>
            <td valign="top"><p>Rajoittamaton (oletusarvoisesti 250 gt)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Pienin nopeus</p></td>
            <td valign="top"><p>400 pyynnön yksiköt sekunnissa</p></td>
            <td valign="top"><p>10 000 pyynnön yksiköt sekunnissa</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Suurin nopeus</p></td>
            <td valign="top"><p>10 000 pyynnön yksiköt sekunnissa</p></td>
            <td valign="top"><p>Rajoittamaton (oletusarvoisesti sekunnissa 250,000 pyynnön yksiköt)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>API-versiot</p></td>
            <td valign="top"><p>Kaikki</p></td>
            <td valign="top"><p>API 2015 – 12-16 ja uudempi</p></td>
        </tr>
    </tbody>
</table>

## <a name="working-with-the-sdks"></a>Käyttö SDK: T

Azure DocumentDB lisätty tuki automaattinen jakaminen [REST](https://msdn.microsoft.com/library/azure/dn781481.aspx)-ohjelmointirajapinnalla versio 2015 – 12-16. Jotta he voivat luoda osioitua sivustokokoelmat, sinun on ladattava SDK-versioiden 1.6.0 tai uudempaa versiota jossakin tuetut SDK-ympäristöt (.NET, Node.js, Java, Python). 

### <a name="creating-partitioned-collections"></a>Osioitu kokoelmien luominen

Seuraavassa esimerkissä näkyy .NET-koodikatkelman laitteen telemetriatietojen tietoja 20 000 pyynnön yksiköiden sekunnissa siirtonopeuden sivustokokoelman luomiseen. SDK asettaa OfferThroughput (joka vuorostaan määrittää `x-ms-offer-throughput` pyynnön otsikossa REST API). Seuraavassa on määritetty `/deviceId` osion avaimeksi. Osion avaimen valinta on tallennettu sivustokokoelman metatietoja, kuten nimi ja indeksoinnin käytännön muiden kanssa.

Tässä esimerkissä on kerätty `deviceId` Microsoft tietää, että (a) koska käsittelyalueella on suuri määrä laitteita, koska kirjoituksia voi olla tasavälein osioiden välillä ja sallimisen että skaalata tietokannan ingest valtaviin tietomäärien ja (b) useat pyynnöt kuten haetaan laitteen uusin lukeminen on rajattu yhteen deviceId ja voi hakea osio.

    DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
    await client.CreateDatabaseAsync(new Database { Id = "db" });

    // Collection for device telemetry. Here the JSON property deviceId will be used as the partition key to 
    // spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
    // sorting against any number or string property.
    DocumentCollection myCollection = new DocumentCollection();
    myCollection.Id = "coll";
    myCollection.PartitionKey.Paths.Add("/deviceId");

    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri("db"),
        myCollection,
        new RequestOptions { OfferThroughput = 20000 });
        

> [AZURE.NOTE] Jotta he voivat luoda osioitua sivustokokoelmat > 10 000 pyynnön yksiköiden sekunnissa siirtonopeuden arvo on määritettävä. Siirtonopeuden on kerrannaisten 100, tämä on oltava 10,100 tai uudempi versio.

Tämä menetelmä tekee kutsu DocumentDB REST-Ohjelmointirajapinnalla ja palvelun käytön valmistelu on pyydetty siirtonopeuden perusteella osioiden määrä. Voit muuttaa kokoelma siirtonopeuden suorituskyvyn tarpeiden kehityttävä. Saat lisätietoja [Suorituskykyä](documentdb-performance-levels.md) .

### <a name="reading-and-writing-documents"></a>Asiakirjojen lukemiseen ja kirjoittamiseen

Nyt-tietojen lisääminen japanin DocumentDB. Ohessa on otoksen luokan sisältävä lukeminen laite ja CreateDocumentAsync kutsu Lisää uusi laite lukeminen kokoelmaan.

    public class DeviceReading
    {
        [JsonProperty("id")]
        public string Id;

        [JsonProperty("deviceId")]
        public string DeviceId;

        [JsonConverter(typeof(IsoDateTimeConverter))]
        [JsonProperty("readingTime")]
        public DateTime ReadingTime;

        [JsonProperty("metricType")]
        public string MetricType;

        [JsonProperty("unit")]
        public string Unit;

        [JsonProperty("metricValue")]
        public double MetricValue;
      }

    // Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new DeviceReading
        {
            Id = "XMS-001-FE24C",
            DeviceId = "XMS-0001",
            MetricType = "Temperature",
            MetricValue = 105.00,
            Unit = "Fahrenheit",
            ReadingTime = DateTime.UtcNow
        });


Oletetaan, että Lue asiakirja on osio-näppäintä ja ID-tunnus, päivittää ja poistaa sen sitten viimeisessä vaiheessa osio-näppäintä ja tunnus. Huomaa, lukee sisällä PartitionKey arvon (vastaa `x-ms-documentdb-partitionkey` pyynnön otsikossa REST API).

    // Read document. Needs the partition key and the ID to be specified
    Document result = await client.ReadDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

    DeviceReading reading = (DeviceReading)(dynamic)result;

    // Update the document. Partition key is not required, again extracted from the document
    reading.MetricValue = 104;
    reading.ReadingTime = DateTime.UtcNow;

    await client.ReplaceDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      reading);

    // Delete document. Needs partition key
    await client.DeleteDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });



### <a name="querying-partitioned-collections"></a>Kyselyt osioitua sivustokokoelmat

Kun testaat osioitua sivustokokoelmat tietojen, DocumentDB reitittää automaattisesti kyselyn osiot vastaava osio avainarvot määritetty suodattimen (Jos ei mitään). Esimerkiksi tämä kysely reititetään osion avaimen "XMS 0001" sisältävän osion.

    // Query using partition key
    IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"))
        .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");

Seuraava kysely ei ole suodattimen osion key (DeviceId), ja se on fanned, johon se on kohdistaa osion indeksi kaikki osiot. Huomaa, että sinun on määritettävä EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` -REST API) on SDK suorittaa kyselyn osioiden välillä.

    // Query across partition keys
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true })
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);

### <a name="parallel-query-execution"></a>Rinnakkaisia kyselyn suorittaminen

DocumentDB SDK: T 1.9.0 ja tuen rinnakkainen kyselyn suorittamisen asetukset, jotka avulla voit tehdä pienen viive kyselyitä, jotka perustuvat osioitua sivustokokoelmat myös silloin, kun he tarvitsevat kosketa on runsaasti osioita yläpuolella. Esimerkiksi seuraava kysely on määritetty suorittamaan rinnakkain osioiden välillä.

    // Cross-partition Order By Queries
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
        .OrderBy(m => m.MetricValue);

Voit hallita rinnakkain kyselyn suoritus säätäminen seuraavat parametrit:

- Määrittämällä `MaxDegreeOfParallelism`, voit hallita rinnakkaisuus eli samanaikaisesti verkkoyhteyksien eikä kokoelman osioita enimmäismäärä aste. Jos tämä arvo 1, rinnakkaisuus aste hallitsee SDK. Jos `MaxDegreeOfParallelism` ei ole määritetty tai aseta-arvoksi 0, joka on oletusarvo, eikä kokoelman osioiden yksittäisen verkkoyhteyttä ole.
- Määrittämällä `MaxBufferedItemCount`, vaihdetaan kyselyn viive ja asiakkaan puoli muistin käyttö käytöstä. Jos tämä parametri jätetään pois tai määritä 1, ellei rinnakkain kyselyn suorituksen aikana kohteiden määrän hallitsee SDK.

Valita kokoelman samaan tilaan, rinnakkain kysely palauttaa tulokset samassa järjestyksessä kuin peräkkäiset suorittamisen. Rajat-osion kysely, joka sisältää lajittelu (ORDER BY ja/tai TOP) suoritettaessa DocumentDB SDK ongelmat rinnakkain kyselyn osioiden välillä ja yhdistää asiakaspuolen yleisesti järjestetyn tulokset osittain lajitellut tulokset.

### <a name="executing-stored-procedures"></a>Tallennettujen toimintosarjojen suorittaminen

Voit myös suorittaa käytettäviä jakamattomia tapahtumia vastaan asiakirjoja, joilla on sama Laitetunnus, esimerkiksi jos olet ylläpito koosteet tai yksittäisen tiedoston laitteen uusin tila. 

    await client.ExecuteStoredProcedureAsync<DeviceReading>(
        UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
        new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
        "XMS-001-FE24C");

Seuraavan osion Odotamme, kuinka voit siirtää ositetun sivustokokoelmat yhden osion peräisin.

<a name="migrating-from-single-partition"></a>
### <a name="migrating-from-single-partition-to-partitioned-collections"></a>Siirtyminen yhden osion osioitua sivustokokoelmat
Kun sovellus kokoelma single-osio on suurempi siirtonopeuden (> 10 000 RU/s) tai suurempi tietosäilö (> 10 Gigatavun), tietojen siirtäminen osioitua sivustokokoelman yhden osion sivustokokoelman [DocumentDB tietojen siirtotyökalun](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) avulla. 

Voit siirtää ositetun sivustokokoelman kokoelma single-osio

1. Vie tiedot JSON yhden osion kokoelmasta. Saat lisätietoja [vieminen JSON-tiedostoon](documentdb-import-data.md#export-to-json-file) .
2. Tuo tiedot osion avaimen määritys ja yli 10 000 pyynnön yksikköä kohden toisen siirtonopeuden luotu osioitua kokoelmaan, kuten alla olevassa esimerkissä. Saat lisätietoja [DocumentDB tuo](documentdb-import-data.md#DocumentDBSeqTarget) .

![Tietojen siirtämisestä Partitioned DocumentDB-kokoelmaan][3]  

>[AZURE.TIP] Harkitse tuo nopeammin lisääntyvien numero, rinnakkain pyynnöt 100 tai sitä uudemmassa versiossa hyödyntää suurempi jo osioitua loppu. 

Nyt kun olemme prosenttiosuuden perusteet, katsotaan joitakin tärkeitä tyyliseikat käsittelyyn liittyvistä DocumentDB näppäimet osio.

## <a name="designing-for-partitioning"></a>Jakaminen suunnitteleminen
Osion avaimen valinta on tärkeä päätös, sinun täytyy tehdä suunnitteluvaiheessa. Tässä osassa käsitellään joitakin kompromissien yhdistävää valitsemalla kokoelmaa osion avain.

### <a name="partition-key-as-the-transaction-boundary"></a>Osion key tapahtuman reunaa
Osion avaimen valittua olisi saldo tarvitse tapahtumat vastaan vaatimus kohteiden jakaa useita osio-näppäimiä varmistamiseksi skaalattava ratkaisun käyttöönotto. Yksi hyvin voimakas voit määrittää saman osion avaimen kaikkia asiakirjoja varten, mutta tämä saattaa rajoittaa ratkaisu skaalattavuus. Osoitteessa muita hyvin voimakas voit määrittää yksilölliset osion avaimen jokaiselle tiedostolle, joka on erittäin skaalattava, mutta estää käyttämällä cross asiakirjan tapahtumat tallennettujen toimintosarjojen ja käynnistimien kautta. Ihanteellinen osioavain on yksi, jonka avulla voit käyttää tehokkaasti kyselyitä ja, jossa on riittävä kardinaliteetti, jotta ratkaisu on skaalattava. 

### <a name="avoiding-storage-and-performance-bottlenecks"></a>Tallennustila ja suorituskyvyn pullonkaulojen välttäminen 
On myös tärkeää Valitse ominaisuus, joka sallii kirjoituksia jaetaan eri arvojen määrä. Saman osion avaimen pyynnöt eivät saa ylittää osio siirtonopeuden ja rajoitettu. Joten on tärkeää Valitse osio-näppäintä, joka ei aiheuta **"kohdepisteet-** sovelluksessa. Tallennustilan koko asiakirjoihin, joissa on sama osioavain voit myös ei ole suurempi kuin 10 Gigatavua tallennustilaan. 

### <a name="examples-of-good-partition-keys"></a>Esimerkkejä hyvä osion avaimet
Seuraavassa on muutamia esimerkkejä siitä, miten voi valita sovelluksen osio-näppäintä:

* Jos olet käyttöönoton käyttäjän profiiliin taustassa, Käyttäjätunnus on hyvä osion avaimen.
* Jos tallennettavien tietojen IoT esimerkiksi laitteen tila-laitteen tunnus on hyvä osion avaimen.
* Jos käytät DocumentDB kirjauduttaessa aikasarjalle tiedot, isäntänimi tai prosessi-tunnus on hyvä osion avaimen.
* Jos sinulla on useita vuokraajan arkkitehtuuri, vuokraajan tunnus on hyvä osion avaimen.

Huomautus joissakin tapauksissa käyttää (kuten edellä kuvatun IoT ja käyttäjän profiilit) osion avain voi olla sama kuin ID-tunnus (asiakirja-näppäintä). Toisissa like sarjojen aikatietoja sinun on ehkä osio-näppäintä, joka eroaa tunnus.

### <a name="partitioning-and-loggingtime-series-data"></a>Tietojen osioinnin ja kirjaaminen-/ aikasarjalle
Yksi DocumentDB yleisimmät Käytä tapauksista on kirjaaminen ja telemetriatietojen. On tärkeää valita hyvä osion näppäintä jälkeen voit joutua muuttamaan luku-/ kirjoitusoikeudet suuria tietomääriä. Valinta määräytyvät oman luku ja kirjoita korvaukset ja suorita odotat kyselyistä. Seuraavassa on vihjeitä siitä, miten voit valita hyvä osio-näppäintä.

- Jos Käytä tapaus pieni korkokannan kirjoittaa acculumating pitkän ajanjakson aikana ja kysely valitsemalla alueiden, aikaleimoja tarvetta ja muut suodattimet käyttämällä rollup aikaleiman, kuten päivämäärä osion näppäintä on hyvä vaihtoehto. Tämän avulla voit kyselyn päälle kaikki tiedot, päivämäärä-osio. 
- Jos havainnollistamiseen kirjoittaminen näkyvä, joka on yleensä tavallisimmista, sinun on käytettävä osio-näppäintä, joka ei perustu aikaleima niin, että DocumentDB voit jakaa kirjoituksia tasaisesti osioiden määrää. Tähän isäntänimi, tunnus, Tehtävätunnus tai toisen-ominaisuutta, jolla suuri kardinaliteetti on hyvä. 
- Kolmas vaihtoehto on hybrid jotakin kohtaa, johon sinulla on useita sivustokokoelmat, yhteen kunkin päivä-, kuukausi ja osio-näppäin ei hajautetun ominaisuuden, kuten isäntänimi. Tämä on hyötyä, voit määrittää suorituskyvyn eri tasot perusteella aika-ikkuna, kuten kuluvan kuukauden sivustokokoelman on valmisteltu suurempi siirtonopeuden kanssa, koska se on lukee ja kirjoittaa, olisi edellisen kuukauden Pienennä siirtonopeuden, koska ne toimivat vain lukuja.

### <a name="partitioning-and-multi-tenancy"></a>Jakaminen ja usean vuokraajan
Jos toteuttavat useita vuokraajan-sovellus, joka käyttää DocumentDB, sinun on kaksi tärkeintä kuviot soveltamisesta kanssa DocumentDB – vuokraajan kohden yksi osio-näppäintä painettuna ja yksi sivustokokoelman kohden. Seuraavassa on ja kunkin haitat:

* Yksi osio-näppäin kohden: tämän mallin alihallinnat, jotka ovat collocated yhden kokoelmassa. Mutta kyselyjen ja yhtä vuokraajan olevissa tiedostoissa Lisää voidaan suorittaa vastaan osio. Voit myös toteuttaa tapahtumien logiikan yli palvelutili kaikki tiedostot. Koska useita alihallinnat, jotka jakavat kokoelma, voit tallentaa säilytys- ja siirtonopeuden kustannukset yhteysvarannon resurssien sisällä yhden sivustokokoelman omistajien, sen sijaan, että kunkin vuokraajan valmistelu ylimääräisiä hyväksyttyjen. Haitta on, että sinulla ei ole suorituskyvyn eristystaso kohden. Suorituskyky ja siirtonopeuden kasvaa koskevat koko sivustokokoelman ja kohdennettujen kasvaa alihallinnat.
* Yhden sivustokokoelman kohden: kunkin vuokraajan on oma. Tämän mallin voit varata suorituskyvyn kohden. DocumentDB's uuden perusteella kulutus hinnoittelu mallin Tämä malli on edullisempaa usean vuokraajan sovellusten Palveltavien kohteiden pieni määrä.

Voit käyttää myös yhdessä/tapaa, joka collocates pieni omistajien ja siirtää suurempi alihallinnat omia sivustokokoelman.

## <a name="next-steps"></a>Seuraavat vaiheet
Tässä artikkelissa on on artikkelissa Azure DocumentDB miten osioinnin toimii, miten voit luoda osioitua sivustokokoelmat ja siitä, miten voit valita hyvä osion näppäintä sovelluksen. 

-   Suorita asteikko ja suorituskyvyn testaaminen oikeilla DocumentDB. Saat otoksen [suorituskyvyn ja mittakaava testaaminen Azure DocumentDB kanssa](documentdb-performance-testing.md) .
-   Aloita [SDK: T](documentdb-sdk-dotnet.md) tai [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) -koodaus
-   Lisätietoja [valmistellun nopeus-DocumentDB](documentdb-performance-levels.md)
-   Jos haluat mukauttaa miten sovelluksesi suorittaa jakaminen, voidaan liittää oman asiakkaan osioinnin käyttöönotto. Katso [asiakkaan jakaminen tuki](documentdb-sharding.md).

[1]: ./media/documentdb-partition-data/partitioning.png
[2]: ./media/documentdb-partition-data/single-and-partitioned.png
[3]: ./media/documentdb-partition-data/documentdb-migration-partitioned-collection.png  

 
