<properties 
    pageTitle="Siirrä tiedot käyttämällä Data Factory Cassandra | Microsoft Azure" 
    description="Lisätietoja tietojen siirtäminen paikalliseen Cassandra tietokannasta käyttämällä Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Tietojen siirtäminen paikalliseen Cassandra tietokannasta käyttämällä Azure Data Factory
Tässä artikkelissa käsitellään käyttämisestä kopioi tehtävän Azure tietojen factory-tietojen kopioimiseen Cassandra paikallisen tietokannan kaikki tietovaraston käsittelytoiminto sarakkeen [Tuetut tietolähteet ja poistumia](data-factory-data-movement-activities.md#supported-data-stores) kohdassa-kohdasta. Tässä artikkelissa perustuu [tietojen siirtämistä tehtävät](data-factory-data-movement-activities.md) -artikkelista, jossa näkyy yleiskatsaus tietojen siirto kopioi tehtävän ja tuettuja kaupan yhdistelmät.

Tietoja factory tukee tällä hetkellä vain siirtäminen Cassandra-tietokannan tietojen lisätä [käsittelytoiminto tietojen stores](data-factory-data-movement-activities.md#supported-data-stores), mutta ei siirtäminen tiedot Cassandra tietokannan tietoja muiden tallennetuista tiedoista.

## <a name="prerequisites"></a>Edellytykset
Voivat muodostaa yhteys paikalliseen Cassandra tietokantaan Azure Data Factory-palveluun sinun on asennettava seuraavasti: 

- Data Management Gatewayn 2.0 tai uudempi samaan tietokoneeseen, joka isännöi tietokannan tai voit välttää resurssien tietokannassa pikaluistelukilpailuissa eri tietokoneeseen. Data Management Gateway on ohjelmisto, joka yhdistää käyttöön paikallisten tietolähteiden pilvipalveluihin turvallisemman ja hallitun tavalla. Lisätietoja Data Management Gateway on artikkelissa [paikallisen ja cloud tietojen siirtäminen](data-factory-move-data-between-onprem-and-cloud.md) artikkelissa.
  
    Kun asennat yhdyskäytävän, se asennetaan automaattisesti Cassandra tietokantayhteyden muodostamisessa käytettävä Microsoft Cassandra ODBC-ohjain. 

> [AZURE.NOTE] Artikkelissa [vianmääritys yhdyskäytävän ongelmien](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) vianmääritys yhteyden/yhdyskäytävän vihjeitä liittyvät ongelmat. 

## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Helpoin tapa luoda myyntijakso, kopioi tiedot Cassandra tietokannasta johonkin seuraavista tuetuista käsittelytoiminto tietoja tallennetuista tiedoista on käyttämällä ohjattua Kopioi tiedot. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavassa esimerkissä on esimerkki JSON määritykset, jonka avulla voit luoda putkijohto [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](data-factory-copy-activity-tutorial-using-powershell.md). Ne näyttävät tietojen kopioiminen Azure-Blob-säiliö Cassandra tietokannan. Tietoja voi kuitenkin kopioida johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.   


## <a name="sample-copy-data-from-cassandra-to-blob"></a>Esimerkki: Tietojen kopioiminen Cassandra Blob
Otosten kopioi tiedot Cassandra tietokannasta Azure-blob tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit. Tietoja voi kopioida suoraan mihin tahansa määrä esitetty [Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md#supported-data-stores) on artikkelissa käyttämällä Kopioi tehtävän Azure Data Factory. 

- Linkitetyn palvelu [OnPremisesCassandra](#onpremisescassandra-linked-service-properties)tyyppi.
- Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi.
- Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [CassandraTable](#cassandratable-properties).
- Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [CassandraSource](#cassandrasource-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

**Linkitetty Cassandra-palvelu**

Tässä esimerkissä käytetään **Cassandra** linkitetty palvelu. Katso [Cassandra linkitetty palvelun](#onpremisescassandra-linked-service-properties) osa linkitetyn palvelun tukemat ominaisuudet.  

    {
        "name": "CassandraLinkedService",
        "properties":
        {
            "type": "OnPremisesCassandra",
            "typeProperties":
            {
                "authenticationType": "Basic",
                "host": "mycassandraserver",
                "port": 9042,
                "username": "user",
                "password": "password",
                "gatewayName": "mygateway"
            }
        }
    }


**Azure linkitetty tallennuspalvelu**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
        "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }

**Cassandra näytettävä tietojoukko**

    {
        "name": "CassandraInput",
        "properties": {
            "linkedServiceName": "CassandraLinkedService",
            "type": "CassandraTable",
            "typeProperties": {
                "tableName": "mytable",
                "keySpace": "mykeyspace" 
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Data Factory-palvelun määrittäminen **ulkoisten** **Tosi** ilmoittaa, että dataset on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat.

**Azure-Blob tulosteen tietojoukko**

Tietoja kirjoitetaan uusia Blob-objektien tunnissa (taajuus: tunti, väli: 1). 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/fromcassandra"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Kopioi aktiviteettiin putkijohto**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **CassandraSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **BlobSink**. 

Katso [RelationalSource ominaisuudet](#cassandrasource-type-properties) RelationalSource tukemat ominaisuudet luettelo. 
    
    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "CassandraToAzureBlob",
                "description": "Copy from Cassandra to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "CassandraInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "CassandraSource",
                        "query": "select id, firstname, lastname from mykeyspace.mytable"
        
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
            ]   
        }
    }
## <a name="onpremisescassandra-linked-service-properties"></a>Linkitetty OnPremisesCassandra palveluominaisuudet

Seuraavassa taulukossa on kuvaus JSON elementtien tietyn linkitetty Cassandra-palveluun.

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- | 
| tyyppi | Type-ominaisuus on määritettävä: **OnPremisesCassandra** | Kyllä |
| Host (isäntä) | IP-osoitteet tai host Cassandra palvelinten nimet.<br/><br/>Määritä IP-osoitteet tai muodostaa yhteyden palvelimilta samanaikaisesti isäntänimet CSV-luettelon. | Kyllä | 
| Port (portti) | Cassandra palvelimen käyttämän kuunteleminen asiakasyhteydet TCP portin. | Ei, oletusarvo: 9042 |
| authenticationType | Tavallinen tai anonyymien | Kyllä |
| käyttäjänimi |Määritä käyttäjänimi käyttäjätilin. | Kyllä, jos authenticationType on määritetty Basic. |
| salasana | Määritä käyttäjätilin salasana.  | Kyllä, jos authenticationType on määritetty Basic. |
| gatewayName | Yhdyskäytävän, jota käytetään yhteyden muodostaminen paikalliseen Cassandra tietokannan nimi. | Kyllä |
| encryptedCredential | Tunnistetiedot salataan yhdyskäytävän. | Ei | 

## <a name="cassandratable-properties"></a>CassandraTable ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](data-factory-create-datasets.md) on artikkelissa. Osia, kuten rakenne, käytettävyys ja tietojoukko JSON käytännön muistuttavat tyypeissä tietojoukko (Azure SQL Azure-blob, Azuren taulukkojen jne.).

**TypeProperties** -osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. Tyypin **CassandraTable** tietojoukko typeProperties osiossa on seuraavat ominaisuudet

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- |
| keyspace | Keyspace tai Cassandra tietokannan rakenteen nimi. | Kyllä (Jos **CassandraSource** **kyselyä** ei ole määritetty). |
| taulukon nimi | Taulukon Cassandra tietokannan nimi. | Kyllä (Jos **CassandraSource** **kyselyä** ei ole määritetty). |


## <a name="cassandrasource-type-properties"></a>CassandraSource ominaisuudet
Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja taulukot ja käytäntö ovat käytettävissä kaikki tehtävälajit. 

Tehtävän typeProperties-osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle ne vaihtelevat sen mukaan, lähteiden ja poistumia.

Kun lähde on **CassandraSource**tyypin, seuraavat ominaisuudet ovat käytettävissä typeProperties-osassa:

| Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen |
| -------- | ----------- | -------------- | -------- |
| kyselyn | Mukautetun kyselyn avulla voit lukea tietoja. | SQL-92-kyselyn tai CQL kyselyn. Katso [CQL viittaus](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Kun käytät SQL-kysely, Määritä **keyspace name.table nimi** edustavan haluat suorittaa kyselyn taulukon. | Ei (Jos taulukon nimi ja valitse tietojoukko keyspace on määritetty).  |
| consistencyLevel | Yhdenmukaisuuden taso määrittää, kuinka monta replikoita on vastattava lukukuittauksen ennen tietojen palauttaminen-asiakassovellukseen. Cassandra tarkistaa replikoiden tietojen täyttämiseen lukukuittauksen annettuun määrään. | YKSI, KAKSI, KOLME, QUORUM, KAIKKI LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Katso lisätietoja [määrittäminen tietojen yhdenmukaisuuden](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) . | Ei. Oletusarvo on yksi. |  


### <a name="type-mapping-for-cassandra"></a>Kirjoita Cassandra yhdistäminen
Cassandra tyyppi | .NET-pohjaiset tyyppi
--------------- | ---------------
ASCII | Merkkijono 
BIGINT | Int64
BLOB | Byte]
TOTUUSARVO | Totuusarvo
DESIMAALILUKU | Desimaaliluku
KAKSINKERTAINEN | Kaksinkertainen
LIUKULUKU | Yksittäisen
INET | Merkkijono
KOKONAISLUKU | Int32
TEKSTI | Merkkijono
AIKALEIMA | Päivämäärä ja aika
TIMEUUID | GUID-tunnus
UUID | GUID-tunnus
VARCHAR | Merkkijono
VARINT | Desimaaliluku

> [AZURE.NOTE]  
> Sivustokokoelman tyypit (kartan, Määritä, luettelot, jne.) viittaavat [käsitteleminen Cassandra sivustokokoelman tyypit virtual taulukon](#work-with-collections-using-virtual-table) osan. 
> 
> Käyttäjän määrittämät tyypit eivät ole tuettuja.
> 
> Binaarisarakkeen ja merkkijonon sarakkeen pituudet pituus et voi olla suurempi kuin 4000. 

## <a name="work-with-collections-using-virtual-table"></a>Kokoelmien virtual taulukon käsitteleminen
Azure Data Factory käyttää valmiita ODBC-ohjain muodostaa yhteyden ja kopioi tietoja Cassandra-tietokannasta. Ohjaimen Normalisoi uudelleen sivustokokoelman tyypit, mukaan lukien kartan, määrittäminen ja luettelo-tiedot vastaavat virtual taulukoiksi. Tarkista etenkin jos taulukko sisältää sivustokokoelman kaikki sarakkeet-ohjain Luo virtual seuraavissa taulukoissa:

-   **Perustaulukko**, jossa on samat tiedot kuin oikean pöydän sivustokokoelman-sarakkeita lukuun ottamatta. Perustaulukon käyttää oikean pöydän, joka on sama nimi.
-   **Virtuaalinen taulukon** kunkin sivustokokoelman-sarake, jossa laajentaa sisäkkäisen tiedot. Virtuaalinen taulukot, jotka edustavat sivustokokoelmat nimetään käyttämällä oikean pöydän, erotin "_vt_" ja sarakkeen nimeä, nimeä.

Virtual taulukot viittaavat oikean pöydän, ottaminen käyttöön denormalisoidun tietojen ohjaimen tiedot. Katso esimerkki osassa. Voit käyttää Cassandra sivustokokoelmien sisällön kyselyt ja niihin liittyminen virtual taulukot.

Voit hyödyntää taulukoiden luettelon tarkasteleminen intuitiivisesti Cassandra-tietokantaa virtual [Ohjattu kopiointi](data-factory-data-movement-activities.md#data-factory-copy-wizard) ja esikatsella sisällä tietoja. Voit luoda kyselyn ohjatun kopio ja vahvista saat tuloksen.

### <a name="example"></a>Esimerkki
Esimerkiksi seuraava "ExampleTable" on Cassandra tietokantataulukon, joka sisältää kokonaisluku perusavainsarakkeeksi nimeltä "pk_int", arvo tekstisarakkeen, luettelosarakkeen, kartta-sarakkeen ja Määritä sarake (nimeltä "StringSet").

pk_int | Arvo | Luettelo | Kartta |   StringSet
------ | ----- | ---- | --- | --------
1 | "otoksen arvon 1" | ["1", "2", "3"]  | {"S1": "a", "S2": "b"} | {"A", "B", "C"}
3 | "otoksen arvon 3" | ["100", "101", "102", "105"] | {"S1": "t"} | {"A", "E"}

Ohjaimen luoda virtual useasta edustavan tämän yhden taulukon. Virtuaalinen taulukoiden ulkomaan avaimen sarakkeiden viitata oikean pöydän perusavaimen sarakkeet ja määritä virtual taulukkorivin vastaa oikean pöydän riviä. 

Ensimmäinen virtual taulukko on seuraavassa taulukossa näkyy perustaulukon nimeltä "ExampleTable". Perustaulukon on samat tiedot kuin alkuperäinen tietokantataulukko lukuun ottamatta kokoelmien, jotka ovat pois tästä taulukosta ja laajennettu virtual muissa taulukoissa.

pk_int | Arvo
------ | -----
1 | "otoksen arvon 1"
3 | "otoksen arvon 3"

Seuraavissa taulukoissa on virtual taulukot, jotka renormalize luettelon, kartta ja StringSet-sarakkeiden tiedot. Sarakkeiden nimet, jotka päättyvät "_index" tai "_key" osoittavat alkuperäisen luettelon tai kartan tietoja paikalle. Sarakkeiden nimet, jotka päättyvät "_value" sisältää laajennettu tietoja kokoelmasta.

#### <a name="table-exampletablevtlist"></a>"ExampleTable_vt_List"-taulukko:
pk_int | List_index | List_value
------ | ---------- | ----------
1 | 0 | 1
1 | 1 | 2
1 | 2 | 3
3 | 0 | 100
3 | 1 | 101
3 | 2 | 102
3 | 3 | 103

#### <a name="table-exampletablevtmap"></a>"ExampleTable_vt_Map"-taulukko:
pk_int | Map_key | Map_value
------ | ------- | ---------
1 | S1 | A
1 | S2 | b
3 | S1 | t

#### <a name="table-exampletablevtstringset"></a>"ExampleTable_vt_StringSet"-taulukko:
pk_int | StringSet_value
------ | ---------------
1 | A
1 | B
1 | C
3 | A
3 | E





[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.
