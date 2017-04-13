<properties 
    pageTitle="Tietojen siirtäminen paikalliseen HDFS | Azure Data Factory" 
    description="Voit siirtää tietoja käyttämällä Azure Data Factory paikallisen HDFS tietoja." 
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
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Tietojen siirtäminen paikalliseen HDFS Azure Data Factory avulla
Tässä artikkelissa käsitellään käyttämisestä kopioi tehtävän Azure tietojen factory-voit siirtää tietoja paikallisen HDFS toiseen tietosäilö. Tässä artikkelissa perustuu [tietojen siirtämistä toimintoja](data-factory-data-movement-activities.md) artikkeliin, jossa näkyy yleiskatsaus tietojen siirto kopioi tehtävän ja tuettuja kaupan yhdistelmät.

Tietoja factory tukee tällä hetkellä vain siirtäminen tiedot paikallisen HDFS ja muiden tietojen stores, mutta ei tietoja siirtäminen paikalliseen HDFS muiden tietojen stores.


## <a name="enabling-connectivity"></a>Yhteyden ottaminen käyttöön
Tietoja Factory-palvelu tukee yhteyden muodostaminen paikalliseen HDFS käyttämällä Data Management Gateway. Lisätietoja Data Management Gateway ja vaiheittaiset ohjeet yhdyskäytävän määrittäminen [paikallista sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) artikkelista. Yhteyden muodostaminen HDFS, vaikka sitä isännöidään Azure-IaaS AM yhdyskäytävän avulla. 

Vaikka voit asentaa yhdyskäytävän samaan paikalliseen tietokoneeseen tai Azure-AM nimellä HDFS, on suositeltavaa, että asennat yhdyskäytävä erillinen tietokoneen/Azure IaaS AM. Yhdyskäytävä on erillinen koneeseen vähentää resurssiristiriita ja parantaa suorituskykyä. Kun asennat yhdyskäytävän eri tietokoneeseen, tietokoneen pitäisi käyttää tietokoneessa, jossa HDFS. 


## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Helpoin tapa luoda myyntijakso, kopioi tiedot paikallisen HDFS on käyttämällä ohjattua Kopioi tiedot. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavissa esimerkeissä otoksen JSON määritykset, jonka avulla voit luoda putkijohto [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](data-factory-copy-activity-tutorial-using-powershell.md). Ne näyttävät tietojen kopioiminen Azure-Blob-objektien tallennustilaan paikallisen HDFS. Tietoja voi kuitenkin kopioida johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Esimerkki: Tietojen kopioiminen paikallisen HDFS Azure-Blob

Tässä esimerkissä esitetään, kuinka haluat kopioida tiedot paikallisen HDFS Azure-Blob-objektien tallennustilaan. Tietoja voi kuitenkin kopioidun **suoraan** johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.  
 
Malli on tietoja factory-kohteet:

1.  Linkitetyn palvelu [OnPremisesHdfs](#hdfs-linked-service-properties)tyyppi.
2.  Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi.
3.  Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [tiedostossa](#hdfs-dataset-type-properties).
4.  Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [FileSystemSource](#hdfs-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Otosten kopioi tietoja paikallisen HDFS Azure-blob tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit. 

Määritä data management gateway on ensimmäiseksi. [Paikallisen sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) artikkelissa annettuja ohjeita. 

**HDFS linkitetty palvelu** Tässä esimerkissä käytetään Windows-todennusta. Todennus, voit käyttää erilaista Katso [HDFS linkitetty palvelun](#hdfs-linked-service-properties) osa. 

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
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

**HDFS syöte tietojoukko** Tämä tietojoukko viittaa HDFS kansioon DataTransfer/UnitTest /. Putkijohto kopioi kaikki tiedostot tähän kansioon kohteeseen. 

Asetus "ulkoinen": "true" ilmoittaa Data Factory-palvelun dataset on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat.
    
    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Azure-Blob tulosteen tietojoukko**

Tietoja kirjoitetaan uusia Blob-objektien tunnissa (taajuus: tunti, väli: 1). Kansiopolun blob arvioidaan dynaamisesti sektoria, käsitellään alkamispäivää perusteella. Kansiopolku käytetään vuoden, kuukauden, päivän ja alkamisajan tuntia osat.

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Kopioi aktiviteettiin putkijohto**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään nämä syöttö- ja tietojoukkoja ja on suunniteltu toimimaan tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **FileSystemSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **BlobSink**. SQL-kyselyn, **kysely** -ominaisuuden valitsee edellisen tunnin kopioitavat tiedot.
    
    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>HDFS linkitetyn palvelun ominaisuudet

Seuraavassa taulukossa on kuvaus JSON elementtien tietyn HDFS linkitetty palvelu.

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- | 
| tyyppi | Type-ominaisuus on määritettävä: **ään** | Kyllä | 
| URL-osoite | URL-osoite HDFS | Kyllä |
| encryptedCredential | Access-tunnistetiedon [Uusi AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) tulos. | Ei |
| Käyttäjänimi | Käyttäjänimi for Windows-todennusta. | Kyllä (for Windows-todennus)
| salasana | Windows-todennuksen salasana. | Kyllä (for Windows-todennus)
| authenticationType | Windows- tai julkinen. | Kyllä |
| gatewayName | Joka Data Factory-palvelun avulla HDFS yhdistäminen yhdyskäytävän nimeä. | Kyllä |   

Saat lisätietoja tunnistetietojen määrittäminen paikallista HDFS [määrittää käyttöoikeudet ja suojaus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

### <a name="using-anonymous-authentication"></a>Käyttämällä Anonyymi todentaminen

    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Windows-todennusta
    
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }
 


## <a name="hdfs-dataset-type-properties"></a>HDFS tietojoukko ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](data-factory-create-datasets.md) on artikkelissa. Osia, kuten rakenne, käytettävyys ja tietojoukko JSON käytännön muistuttavat tyypeissä tietojoukko (Azure SQL Azure-blob, Azuren taulukkojen jne.).

**TypeProperties** -osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. Tyypin **tiedostossa** (joka sisältää HDFS tietojoukko) tietojoukko typeProperties osiossa on seuraavat ominaisuudet

Ominaisuus | Kuvaus | Pakollinen
-------- | ----------- | --------
Kansiopolku | Kansion polku. Esimerkki:`myfolder`<br/><br/>Käytä ohjausmerkkiä "\" erikoismerkkien merkkijonon. Esimerkki: Määritä folder\subfolder-kansion\\\\alikansio ja määritä d:\samplefolder, d:\\\\mallikansio.<br/><br/>Voit yhdistää tämän ominaisuuden **partitionBy** on kansion polut sektoria perusteella ja Aloita/Lopeta päivämäärä-aikoja. | Kyllä
Tiedostonimi | Määritä tiedoston nimi **kansiopolku** , jos haluat tietyn tiedoston kansioon viitata taulukon. Jos et määritä tätä ominaisuutta, jos arvo osoittaa taulukon kaikki tiedostot-kansiossa.<br/><br/>Kun tulostus-tietojoukko tiedostonimi ei ole määritetty, luodun tiedoston nimi on oltava seuraavat tässä muodossa: <br/><br/>Tietoja. <Guid>.txt (esimerkiksi:: Data.0a405f8a 93ff-4c6f-b3be-f69616f1df7a.txt | Ei
partitionedBy | partitionedBy avulla voidaan määrittää dynamic kansiopolku sarjan aikatietoja tiedostonimi. Esimerkki: kansiopolku Parametroitu tunnissa tietoja varten. | Ei
fileFilter | Määritä suodatus, jota käytetään osajoukkoa kansiopolku tiedostojen kaikki tiedostot sijaan. <br/><br/>Sallittu seuraavia arvoja: `*` (useita merkkejä) ja `?` (yksittäinen merkki).<br/><br/>Esimerkki 1:`"fileFilter": "*.log"`<br/>Esimerkki 2:`"fileFilter": 2014-1-?.txt"`<br/><br/>**Huomautus**: fileFilter sovelletaan syötteen tiedostossa-tietojoukko | Ei
| Muotoile | Muotoile seuraavanlaisia tuetaan: **TekstinMuoto**, **AvroFormat**, **JsonFormat**, **OrcFormat**ja **ParquetFormat**. Muotoile **tyyppi** -ominaisuuden arvoksi jokin näistä arvoista. Lue lisätietoja [Määrittäminen TekstinMuoto](#specifying-textformat), [Määrittämällä AvroFormat](#specifying-avroformat), [Määrittämällä JsonFormat](#specifying-jsonformat), [Määrittämällä OrcFormat](#specifying-orcformat)ja [Määrittämällä ParquetFormat](#specifying-parquetformat) osat. Jos haluat kopioida tiedostot muodossa-on välillä tiedostopohjaisia stores (binaarinen kopio), voit ohittaa sekä syöttö- ja tietojoukko määritelmät muoto-kohdassa. | Ei 
| pakkaus | Määritä tyyppi ja tietojen pakkaus taso. Tuetut tiedostotyypit ovat: **GZip**, **Deflate**ja **BZip2** ja tuettujen tasot ovat: **Optimal** ja **nopein**. Tällä hetkellä pakkausasetuksia ei tue tietojen **AvroFormat** tai **OrcFormat**. Jos haluat lisätietoja, katso [pakkaamisen tuki](#compression-support) -osa.  | Ei |



> [AZURE.NOTE] tiedoston nimen ja fileFilter ei voi käyttää samanaikaisesti.


### <a name="using-partionedby-property"></a>PartionedBy-ominaisuuden avulla

Mainittu edellisessä osassa voit määrittää dynaaminen kansiopolku sarjan aikatietoja partitionedBy ja tiedostonimi. Voit tehdä Data Factory-makrot ja järjestelmämuuttujan SliceStart, SliceEnd, jotka ilmaisevat looginen ajan, on annettu data-sektori. 

Lisätietoja aika sarjan tietojoukkoja ajoitus ja sektorit, lue [Luominen tietojoukkoja](data-factory-create-datasets.md), [ajoittaminen ja suorittamisen](data-factory-scheduling-and-execution.md)ja [Luominen putkistot](data-factory-create-pipelines.md) artikkelit. 

#### <a name="sample-1"></a>Esimerkki 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

Tässä esimerkissä {sektoria} tilalla on määritetty Data Factory järjestelmämuuttujan SliceStart (YYYYMMDDHH)-muodossa arvo. SliceStart viittaa sektoria aloitusaika. Kansiopolku on erilainen jokaiselle sektoria. Esimerkki: wikidatagateway/wikisampledataout/2014100103 tai wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Esimerkki 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

Tässä esimerkissä vuoden, kuukauden, päivän ja ajan SliceStart puretaan tuominen eri muuttujat, joita käytetään kansiopolku ja tiedostonimi-ominaisuudet.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>HDFS kopioi tehtävän ominaisuudet

Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja taulukot ja käytännöt ovat käytettävissä kaikki tehtävälajit. 

Tehtävän typeProperties-osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle ne vaihtelevat sen mukaan, lähteiden ja poistumia.

Kopioi tehtävälle lähteen tyyppi **FileSystemSource** on seuraavat ominaisuudet ovat käytettävissä typeProperties-osassa:

**FileSystemSource** tukee seuraavia ominaisuuksia:

| Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen |
| -------- | ----------- | -------------- | -------- |
| Rekursiiviset | Ilmaisee, onko tiedot luetaan rekursiivisesti sub-kansiot tai vain määritettyyn kansioon. | TOSI, EPÄTOSI (oletus)| Ei |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.

