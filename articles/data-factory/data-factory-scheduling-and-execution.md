<properties
    pageTitle="Ajoittamisen ja suorittamisen kanssa Data Factory | Microsoft Azure"
    description="Lisätietoja Azure Data Factory-sovelluksen malli ajoittaminen ja suorittamisen ominaisuuksia."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="spelluru"/>

# <a name="data-factory-scheduling-and-execution"></a>Tietoja Factory ajoittaminen ja suorittaminen
Tässä artikkelissa kerrotaan Azure Data Factory-sovelluksen malli ajoittaminen ja suorittamisen ominaisuuksia. 

## <a name="prerequisites"></a>Edellytykset
Tässä artikkelissa oletetaan ymmärtää perusteet Data Factory sovelluksen mallin käsitteistä, kuten tehtävän, putkistot, linkitetyn services ja tietojoukkoja. Azure Data Factory peruskäsitteet on seuraavissa artikkeleissa:

- [Johdatus Data Factory](data-factory-introduction.md)
- [Putkistot](data-factory-create-pipelines.md)
- [Tietojoukkoja](data-factory-create-datasets.md) 

## <a name="schedule-an-activity"></a>Tehtävän

JSON-tehtävän ajoitus-osassa voit määrittää toistuvan tehtävän aikataulu. Esimerkiksi voit ajoittaa tehtävän tunnissa seuraavasti:

    "scheduler": {
        "frequency": "Hour",
        "interval": 1
    },  

![Ajoitus-Esimerkki](./media/data-factory-scheduling-and-execution/scheduler-example.png)

Kuten kaavio, joka määrittää tehtävän ajoitusta Luo numerosarjan tumbling windows. Tumbling windows on kiinteä, ikkunat, jatkuvat aikavälejä sarjaa. Nämä looginen tumbling windows tehtävän kutsutaan *toiminta windows*.

Parhaillaan tehtävä-ikkunan voit käyttää liittyvän tehtävän ikkunan Järjestelmämuuttujat [WindowStart](data-factory-functions-variables.md#data-factory-system-variables) ja [WindowEnd](data-factory-functions-variables.md#data-factory-system-variables) tehtävän JSON aikaväli. Voit käyttää muuttujia eri tarkoituksiin toiminnossa JSON. Voit käyttää niitä esimerkiksi tietojen valinta syöttö- ja aikatietoja sarjan edustava tietojoukkoja.

**Ajoitus** -ominaisuus tukee saman alaominaisuudet **käytettävyys** ominaisuutena tietojoukossa. Katso lisätietoja [tietojoukko käytettävyys](data-factory-create-datasets.md#Availability) . Esimerkkejä: osoitteessa tietyn ajan siirtymän ajoituksen tai määrittäminen tilassa voit tasata käsittely alussa tai lopussa välin toiminta-ikkuna.

Voit määrittää tehtävän **ajoitus** ominaisuuksia, mutta tämä ominaisuus on **Valinnainen**. Jos määrität ominaisuus, sen on oltava samat tulosteen tietojoukko määritelmän määritettyyn cadence. Tulosteen tietojoukko ei tällä hetkellä, mitä ohjaa aikataulussa, joten sinun on luotava tulostus-tietojoukko, vaikka tehtävän tuota tuloksen. Jos tehtävän sähköpostit tulevat kaikki syötetyt, voit ohittaa syötteen tietojoukon luominen.

## <a name="time-series-datasets-and-data-slices"></a>Aika sarjan tietojoukkoja ja tietojen sektorit

Sarjan aikatietoja on arvopisteiden jatkuvan sarjan, joka koostuu yleensä peräkkäiset mittaukset aikavälin päälle. Sarjan aikatietoja yleisiä esimerkkejä ovat tunnistimen tiedot ja sovelluksen telemetriatietojen.

Data Factory, jossa voit käsitellä sarja-arvojen oletusmäärä tavalla aktiviteettiin suoritetaan kerran. Yleensä on toistuva cadence, jolla syöttötiedot saapuu, ja siirtää tiedot on esitettävä. Tämä cadence on mallintaa määrittämällä **käytettävyys** tietojoukossa seuraavasti:

    "availability": {
      "frequency": "Hour",
      "interval": 1
    },

Yksittäisen kulutettu ja tehtävän suorittaminen tuottamat tiedot kutsutaan tietojen sektoria. Seuraavassa kaaviossa on esitetty Esimerkki tehtävään, jonka yhden syötteen tietojoukko ja yksi tulosteen tietojoukko. Nämä tietojoukkoja on **käytettävyyden** asettaminen tunnin välein korkojakso.

![Käytettävyys ajoitus](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Edellä olevassa kaaviossa näkyy tunnin välein tietojen sektorit syöttö- ja -tietojoukko varten. Kaaviossa näkyy kolme syötteen sektoria, jotka ovat valmiita käsittelyä varten. 10-11 AM tehtävä on käynnissä, tuottavat 10-11 AM tulosteen sektoria.

Voit käyttää tietojoukossa JSON ja muuttujien [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) ja [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables)yhdistämällä nykyinen sektori liittyvät aikaväli.

Tällä hetkellä Data Factory edellyttää aikatauluun määritetyn tehtävän täsmälleen vastaa tulostus-tietojoukko **käytettävyys** -parametrissa aikatauluun. Tämän vuoksi **WindowStart**, **WindowEnd**, **SliceStart**ja **SliceEnd** aina Yhdistä saman ajanjakson ja yksi tulos sektoria.

Lisätietoja eri ominaisuuksia käytettävyys-osassa on artikkelissa [luominen tietojoukkoja](data-factory-create-datasets.md).

## <a name="move-data-from-sql-database-to-blob-storage"></a>Tietojen siirtäminen SQL-tietokantaan Blob-objektien tallennustilaan

Oletetaan, että Sijoita asioita yhteen ja käytössä luomalla myyntijakso, kopioi tiedot Azure SQL-tietokannan Azure-Blob-säiliö tunnissa.

**IME: Azure SQL-tietokanta-tietojoukko**

    {
        "name": "AzureSqlInput",
        "properties": {
            "published": false,
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


**Korkojakso** on määritetty **Tunti** ja **väli** on määritetty **1** käytettävyys-osassa.

**Tulos: Azure Blob storage tietojoukko**

    {
        "name": "AzureBlobOutput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
                "format": {
                    "type": "TextFormat"
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
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
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


**Korkojakso** on määritetty **Tunti** ja **väli** on määritetty **1** käytettävyys-osassa.



**Tehtävä: Kopioi tehtävä**

    {
        "name": "SamplePipeline",
        "properties": {
            "description": "copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 100000,
                            "writeBatchTimeout": "00:05:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureSQLInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            ],
            "start": "2015-01-01T08:00:00Z",
            "end": "2015-01-01T11:00:00Z"
        }
    }


Otosten näyttää tehtävän aikataulun ja tietojoukko käytettävyys osien Määritä tunnin välein korkojakso. Otosten näyttää, miten voit käyttää **WindowStart** ja **WindowEnd** tehtävään liittyvien tietojen valitseminen Suorita ja kopioi se Blob-objektien kanssa tarvittavat **kansiopolku**. **Kansiopolku** on Parametroitu on omiin kansioihinsa tunnissa.

Kun kolme välillä 8 – 11 AM sektorit suorittaa tietojen Azure SQL-tietokantaan on seuraavanlainen:

![Esimerkkisyöte](./media/data-factory-scheduling-and-execution/sample-input-data.png)

Kun putkijohto käyttää, Azure-blob lisätään seuraavasti:

-   Tiedoston mypath/2015/1/1/8/tiedot. &lt;Guid&gt;.txt tiedoilla

            10002345,334,2,2015-01-01 08:24:00.3130000
            10002345,347,15,2015-01-01 08:24:00.6570000
            10991568,2,7,2015-01-01 08:56:34.5300000

    > [AZURE.NOTE] &lt;GUID-tunnus&gt; on korvattu todellinen GUID-tunnus. Esimerkki tiedostonimi: Data.bcde1348 7620-4f93-bb89-0eed3455890b.txt
-   Tiedoston mypath/2015/1/1/9/tiedot. &lt;Guid&gt;.txt tiedot:

            10002345,334,1,2015-01-01 09:13:00.3900000
            24379245,569,23,2015-01-01 09:25:00.3130000
            16777799,21,115,2015-01-01 09:47:34.3130000
-   Tiedoston mypath/2015/1/1/10/tiedot. &lt;Guid&gt;.txt, joissa ei ole tietoja.


## <a name="active-period-for-pipeline"></a>Myyntijakso aktiivista kautta

Määritetty **Käynnistä** - ja **Päättymisaika** -ominaisuuksilla putkijohto aktiivista kautta käsite [luominen putkistot](data-factory-create-pipelines.md) käyttöön.

Voit määrittää myyntijakso aktiivisen kauden alkamispäivä menneisyydessä. Tietojen Factory laskee kaikki tiedot sektorit aiemman (takaisin täytöt) ja alkaa käsitellä niitä.

## <a name="parallel-processing-of-data-slices"></a>Rinnakkainen tietojen sektorien käsittely
Voit määrittää Edellinen täytetty tietojen sektoreiden suoritetaan rinnakkain määrittämällä **samanaikainen** ominaisuuden tehtävän JSON käytäntö-osassa. Saat lisätietoja tämän ominaisuuden [luominen putkistot](data-factory-create-pipelines.md).

## <a name="rerun-a-failed-data-slice"></a>Suorita on epäonnistunut tietojen sektori 
Voit valvoa suorittamisen sektorien Monimuotoisen ja visual tavalla. Katso lisätietoja [Seuranta ja hallinta käyttämällä Azure portaalin lavat putkistot](data-factory-monitor-manage-pipelines.md) tai [Seuranta ja hallinta-sovelluksessa](data-factory-monitor-manage-app.md) .

Harkitse seuraavassa esimerkissä, jossa näkyy kaksi toimintoja. Activity1 tuottaa aika sarjan tietojoukon, jonka sektoreiden tulosteen, joka tuottaa lopputulos aika sarjan tietojoukko Activity2 kulutettu syötteeksi nimellä.

![Epäonnistuneiden sektori](./media/data-factory-scheduling-and-execution/failed-slice.png)

Kaavio osoittaa, että ulos kolme viimeisimmät sektoreiden tuottavat 9 – 10 AM sektoria varten Dataset2 virhe. Tietoja Factory seuraa automaattisesti ajan sarjan tietojoukko riippuvuuden. Tämän vuoksi se ei käynnisty 9 – 10 AM edeltävät sektoria suorittaa tehtävän.

Factory valvonnan ja hallinnan Datatyökalut avulla voit siirtyä helposti Etsi pääkansion syy ongelmaan ja korjaa se epäonnistui sektoria vianmäärityslokeihin. Kun ongelma on korjattu, voit helposti aloittaa tehtävän suorittaminen epäonnistui sektoria tuottamaan. Lisätietoja uudelleen ja ymmärtää tilan siirrot tietojen sektoreiden varten on artikkelissa [Seuranta ja hallinta käyttämällä Azure portaalin lavat putkistot](data-factory-monitor-manage-pipelines.md) tai [Seuranta ja hallinta-sovelluksessa](data-factory-monitor-manage-app.md).

Kun **Dataset2**varten suorittamalla 9 – 10 AM sektoria, Data Factory aloittaa 9 – 10 AM riippuvaiset sektoria Suorita lopullinen tietojoukko.

![Suorita epäonnistui sektori](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="run-activities-in-a-sequence"></a>Suorita toiminnot peräkkäin
Kahden toiminnan (suorittaminen yhdestä aktiviteetin toiselle) voivat ketjuttaa määrittämällä yhdestä aktiviteetin tulostus-tietojoukko kuin muiden toimintojen syötteen tietojoukko. Toimintojen voi olla samaa putkijohtoa tai eri putkistot. Toinen tehtävä suorittaa vain kun ensimmäinen on suoritettu onnistuneesti.

Esimerkiksi seuraava tilanne:

1.  Myyntijakso P1 on tehtävä A1, joka edellyttää ulkoisen syötteen tietojoukko D1 ja tuottaa tulosteen tietojoukko D2.
2.  Myyntijakso P2 on tehtävä A2, joka edellyttää tietojoukko D2 toimia ja tuottaa tulosteen tietojoukko D3.

Tässä skenaariossa A1 ja A2 toiminnot ovat eri putkistot. Tehtävän A1 suoritetaan, kun ulkoiset tiedot ovat käytettävissä ja ajoituksen korkojakso on saavutettu. Tehtävän A2 suoritetaan, kun ajoitetun sektorit D2-palvelupakettiisi ja ajoituksen korkojakso on saavutettu. Jos virheen tietojoukko D2 sektorit A2 ei voi käyttää sitä varten, kunnes se on käytettävissä.

Verkkokaavio-näkymän näyttäisi samanlaiselta kuin seuraavassa kaaviossa:

![Kaksi yhdistettyä putkijohtoa ketjutus toimintoja](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Kuten edellä mainittiin, toimintojen voi olla samaa putkijohtoa. Kaavio-näkymä, jossa molemmat samaa putkijohtoa toimintojen näyttäisi samanlaiselta kuin seuraavassa kaaviossa:

![Samaa putkijohtoa ketjutus toimintoja](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

### <a name="copy-sequentially"></a>Kopioi peräkkäin
On mahdollista suorittaa useita kopiointi peräkkäin peräkkäisiä/vyöhykkeen tavalla. Kahden kopioi toiminnan voi esimerkiksi ovat putkijohto ja syöttötiedot tulostus-tietojoukkoja (CopyActivity1 ja CopyActivity2):   

CopyActivity1

IME: tietojoukko. Tulos: Dataset2.

CopyActivity2

IME: Dataset2.  Tulos: Dataset3.

CopyActivity2 suoritetaan vain, jos CopyActivity1 on suoritettu onnistuneesti ja Dataset2 on käytettävissä.

Tässä on esimerkki myyntijakso JSON:

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob1ToBlob2",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset3"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob2ToBlob3",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2016-08-25T01:00:00Z",
            "end": "2016-08-25T01:00:00Z",
            "isPaused": false
        }
    }

Huomaa, että esimerkissä tulostus-tietojoukko kopioi ensimmäisen tehtävän (Dataset2) on määritetty syötteeksi toisen toiminnon. Tämän vuoksi toinen tehtävä suorittaa vain, kun tulostus-tietojoukko: ensimmäinen tehtävä on valmis.  

Esimerkissä CopyActivity2 voi olla eri toimia, kuten Dataset3, mutta Dataset2 määrittää CopyActivity2, syötteeksi niin, että tehtävä ei toimi, ennen kuin CopyActivity1 on valmis. Esimerkki:

CopyActivity1

IME: Dataset1. Tulos: Dataset2.

CopyActivity2

Syötteiden: Dataset3, Dataset2. Tulos: Dataset4.

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlobToBlob",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset3"
                        },
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset4"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob3ToBlob4",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2017-04-25T01:00:00Z",
            "end": "2017-04-25T01:00:00Z",
            "isPaused": false
        }
    }


Huomaa, että esimerkissä kaksi syötteen tietojoukkoja on määritetty toisen kopioi tehtävälle. Kun useita syötteiden on määritetty, vain ensimmäinen syötteen tietojoukko käytetään tietojen kopioiminen, mutta muiden tietojoukkoja käytetään riippuvuuksien. CopyActivity2 alkaa vasta, kun seuraavat ehdot täyttyvät:

- CopyActivity1 on suoritettu ja Dataset2 on käytettävissä. Tämä tietojoukko ei ole käytössä, kun kopioit tietoja Dataset4. Se vain toimii ajoituksen riippuvuuden CopyActivity2.   
- Dataset3 on käytettävissä. Tämä tietojoukko edustaa tietoja, jotka on kopioitu kohteeseen.  



## <a name="model-datasets-with-different-frequencies"></a>Mallin tietojoukkoja maksutiheydet kanssa

Mallit altistetuissa syöttö- ja tietojoukkoja ja tehtävän ajoitus-ikkuna on sama. Joissakin tilanteissa edellyttävät voi tuottaa on erilainen kuin vähintään yksi syötteiden tiheydet usein. Tietoja Factory tukee mallinnus tilanteista.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Esimerkki 1: Tuottaa päivittäisen tulosteen raportin syötteen tiedot, jotka ovat käytettävissä tunnissa

Harkitse tilanne, jossa on syötät anturit käytettävissä olevat mittaustiedot tunnissa Azure-Blob-objektien tallennustilaan. Haluat tuottaa päivittäinen kooste raportin tilastotietoja, kuten keskiarvo, suurin ja pienin päivän [Data Factory rakenne toimintojen](data-factory-hive-activity.md)kanssa.

Näin, miten voit muovata artikkelista Data Factory kanssa:

**Kirjoita tietojoukko**

Tunnin välein syötteen tiedostot jätetään pois kansion tiettynä päivänä. Käytettävyys syöte on määritetty **Tunti** (taajuus: tunti, väli: 1).

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Tulostus-tietojoukko**

Yksi kohdetiedosto luodaan päivittäin päivän kansioon. Käytettävyys tulosteen asetetaan **päivä** (taajuus: päivä- ja väli: 1).


    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Toiminta: rakenne putkijohto toimintaa**

Rakenne-komentosarja saa ilmoituksen, joka käyttää seuraavia koodikatkelman mukaisesti **WindowStart** muuttujaa parametrit *DateTime* tarvittavat tiedot. Rakenne-komentosarja käyttää muuttuja lataa tiedot oikeaan kansioon päivän ja suorita koosteen tulosteen luomiseen.

        {  
            "name":"SamplePipeline",
            "properties":{  
            "start":"2015-01-01T08:00:00",
            "end":"2015-01-01T11:00:00",
            "description":"hive activity",
            "activities": [
                {
                    "name": "SampleHiveActivity",
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adftutorial\\hivequery.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                            "Month": "$$Text.Format('{0:%M}',WindowStart)",
                            "Day": "$$Text.Format('{0:%d}',WindowStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },          
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 2,
                        "timeout": "01:00:00"
                    }
                 }
             ]
           }
        }

Seuraavassa kaaviossa on esitetty skenaarion tietojen riippuvuuden näkökulmasta.

![Tietojen riippuvuuden](./media/data-factory-scheduling-and-execution/data-dependency.png)

Saat päivittäin tulostus-sektori määräytyy 24 tunnin välein sektorit-syötteen tietojoukko. Tietoja Factory laskee riippuvuussuhteita automaattisesti käyttäminen syöttötiedot sektoreiden, jotka kuuluvat tulostus-sektorin esitettävä kuin saman ajanjakson mukaan. Jos jokin 24 syötteen sektorit ei ole käytettävissä, Data Factory odottaa syötteen sektori on valmis ennen kuin aloitat päivittäinen tehtävän suorittaminen.


### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Esimerkki 2: Määritä riippuvuuden lausekkeet ja Data Factory-Funktiot

Seuraavassa toiseen skenaario. Oletetaan, että sinulla on rakenne-toiminto, joka käsittelee kahden syötteen tietojoukkoja. Jonkin niistä on uusia tietoja päivittäin, mutta jonkin niistä saa uusia tietoja viikoittain. Oletetaan, että haluat tehdä liitoksen yli kaksi syötteiden ja tuottaa tulos päivittäin.

Mitä tietoja Factory yksinkertainen lähestymistavan automaattisesti luvut oikealle, arvosarjan sektoreiden käsittelemään kohdistamalla tulostukseen tietojen sektoria aika kauden ei toimi

Määritä, että jokaisen tehtävän suorittaminen-Data-Factory käytettävä viime viikolla tietojen sektoria viikoittain syötteen tietojoukko. Azure Data Factory-funktioiden käyttäminen seuraavat koodikatkelman esitetyllä tavalla toteuttamisesta ongelman.

**Input1: Azure-blob**

Ensimmäisellä syöte on Azure-blob on päivitetty päivittäin.

    {
      "name": "AzureBlobInputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Input2: Azure-blob**

Input2 on Azure-blob on päivitetty viikoittain.

    {
      "name": "AzureBlobInputWeekly",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 7
        }
      }
    }

**Tulos: Azure-blob**

Yksi kohdetiedosto luodaan päivittäin kansion päivän. Käytettävyys tulosteen määritetään **päivä** (taajuus: päivä, väli: 1).

    {
      "name": "AzureBlobOutputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Toiminta: rakenne putkijohto toimintaa**

Rakenne-toimintojen tulee kaksi syötteiden ja tuottaa tulostus-sektori päivittäin. Voit määrittää joka päivä tulosteen sektorin määräytyvät edellisellä viikolla syötteen sektoria viikoittain toimia seuraavasti.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-01-01T08:00:00",
        "end":"2015-01-01T11:00:00",
        "description":"hive activity",
        "activities": [
          {
            "name": "SampleHiveActivity",
            "inputs": [
              {
                "name": "AzureBlobInputDaily"
              },
              {
                "name": "AzureBlobInputWeekly",
                "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
                "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutputDaily"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
              "scriptPath": "adftutorial\\hivequery.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                "Month": "$$Text.Format('{0:%M}',WindowStart)",
                "Day": "$$Text.Format('{0:%d}',WindowStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            },          
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 2,  
              "timeout": "01:00:00"
            }
           }
         ]
       }
    }


## <a name="data-factory-functions-and-system-variables"></a>Tietoja Factory Funktiot ja Järjestelmämuuttujat   

Katso [Data Factory-funktioita ja järjestelmämuuttujia](data-factory-functions-variables.md) Funktiot ja Järjestelmämuuttujat, joka tukee Data Factory luettelo.

## <a name="data-dependency-deep-dive"></a>Tietojen riippuvuuden perinpohjaisesti käsittelevään artikkeliin

Data Factory käyttää luomiseen tietojoukko sektoria tehtävän-asennuksen käyttämää seuraavaan *riippuvuuden mallin* tietojoukkoja kulutettu ja aktiviteetin tuottamat välisten yhteyksien määrittäminen.

Syötteen tietojoukkoja tulosteen tietojoukko sektoria luomiseen tarvittavien ajanjakson kutsutaan *riippuvuuden ajan*.

Tehtävän suorittaminen luo tietojoukko sektoria vasta, kun syötteen tietojoukkoja riippuvuuden ajan kuluessa sektorit tiedot ovat käytettävissä. Toisin sanoen kaikki joka riippuvuuden kauden syötteen sektorit on oltava **valmiina** tilassa tehtävän suorittaminen tuottaa tulostus-tietojoukko sektoria.

Luomiseen tietojoukko sektoria [**Käynnistä-painiketta**, **Lopeta**] funktio on yhdistettävä tietojoukko sektoria riippuvuuden sen ajan. Tämä funktio on käytettävä kaava, joka muuntaa alkuun tai loppuun riippuvuudet kauden alussa ja lopussa tietojoukko sektoria. Lisää varsinaisesti:

    DatasetSlice = [start, end]
    DependecyPeriod = [f(start, end), g(start, end)]

**F** ja **g** yhdistäminen Funktiot, jotka laskevat alkamis- ja lisää kunkin tehtävän riippuvuuden kauden lopussa.

Näytteiden tarkastelu riippuvuuden kauden on sama kuin tietojen sektoria, joka on valmistettu ajaksi. Tällaisissa tapauksissa Data Factory laskee automaattisesti syötteen sektorit, jotka kuuluvat riippuvuuden ajanjakson.  

Jos tulos on valmistettu päivittäin ja kenttään annettavat tiedot eivät käytettävissä tunnissa kooste-Esimerkki tietojen sektoria kauden on 24 tuntia. Tietoja Factory löytää asiaa tunnin välein syötteen sektoreiden ajanjaksolle ja tekee tulosteen sektoria riippuvaiset syötteen sektoria.

Voit myös lisätä omia yhdistäminen riippuvuuden ajan mukaisesti otosten, missä jokin syötteiden viikoittain ja tulostus-sektori on valmistettu päivittäin.

## <a name="data-dependency-and-validation"></a>Tietojen riippuvuuden ja kelpoisuuden tarkistaminen

Tietojoukko voi olla vahvistuskäytännön, joka määrittää, miten sektoria suorittamisen luomien tietojen vahvistetaan ennen kuin se on valmiina käytettäväksi määritetty. Katso lisätietoja [luominen tietojoukkoja](data-factory-create-datasets.md) .

Tässä tapauksessa jälkeen sektoria suorittamisen tulosteen sektori on tilaksi **odottaa** alitila **kelpoisuuden**kanssa. Kun sektorit on vahvistettu, sektoria tilaksi **Valmis**.

Jos tiedot on sektori on valmistettu, mutta ei läpäissyt vahvistus, tehtävän suoritetaan edeltävät sektoreiden, jotka riippuvat tämän sektorin, ei käsitellä.

[Näyttö ja hallita putkistot](data-factory-monitor-manage-pipelines.md) käsitellään Data Factory sektoreiden tietoja eri tiloista.

## <a name="external-data"></a>Ulkoiset tiedot

Ulkoinen (seuraavat JSON koodikatkelman esitetyllä), mikä se ei ole luotu Data Factory voi merkitä tietojoukko. Siinä tapauksessa tietojoukko käytännön voit on joukko kuvaava vahvistus parametrit ja yritä uudelleen dataset käytännön. Saat kaikki ominaisuudet kuvauksen [luominen putkistot](data-factory-create-pipelines.md) .

Samalla tietojoukkoja, jotka ovat tuottamat tiedot Factory-tietojen sektorit ulkoisiin tietoihin on oltava valmiina ennen riippuvaiset sektoreiden voidaan käsitellä.

    {
        "name": "AzureSqlInput",
        "properties":
        {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties":
            {
                "tableName": "MyTable"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1     
            },
            "external": true,
            "policy":
            {
                "externalData":
                {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }  
        }
    }


## <a name="onetime-pipeline"></a>Etsii kerran myyntijakso
Voit luoda ja ajoittaa putkijohto suorittaa säännöllisin väliajoin (esimerkiksi: kerran tunnissa tai päivittäin) sisällä myyntijakso määrityksessä Määritä alkamis- ja päättymisajat. [Ajoittaminen toimintoja](#scheduling-and-execution) lisätietoja. Voit myös luoda putkijohto, joka suoritetaan vain kerran. Voit tehdä **pipelineMode** -ominaisuus asetetaan myyntijakso määritelmän **etsii kerran** JSON seuraava näyte esitetyllä tavalla. Tämän ominaisuuden oletusarvo on **ajoitettu**.

    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ]
                    "name": "CopyActivity-0"
                }
            ]
            "pipelineMode": "OneTime"
        }
    }

Ota seuraavat seikat huomioon:

- ** **Käynnistä** - ja** päättymisajat putkijohto ei ole määritetty.
- **Käytettävyys** syöttö- ja tietojoukkoja on määritetty (**korkojakso** ja **väli**), vaikka Data Factory ei käytä arvot.  
- Kaavionäkymä ei näy erikseen putkistot. Tämä on suunniteltu ominaisuus.
- Erikseen putkistot ei voi päivittää. Voit Kloonaa erikseen putkijohto, nimetä sen uudelleen, Päivitä ominaisuudet ja haluat luoda uuden käytännön käyttöön.
