<properties 
    pageTitle="Hadoop Streaming tehtävä" 
    description="Lue käyttämisestä Hadoop Streaming tehtävän Azure tietojen factory-toimimaan Hadoop Streaming ohjelmat-demand/your oman HDInsight-klusterin." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="shlo"/>

# <a name="hadoop-streaming-activity"></a>Hadoop Streaming tehtävä
> [AZURE.SELECTOR]
[Rakenne](data-factory-hive-activity.md)  
[Possu](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Tallennettu toimintosarja](data-factory-stored-proc-activity.md)
[Tietojen järvi Analytics U-SQL](data-factory-usql-activity.md)
[.NET mukautettu](data-factory-use-custom-activities.md)

Voit käyttää HDInsightStreamingActivity tehtävän Hadoop Streaming työn Azure Data Factory-myyntijakso-ongelma. Seuraavat JSON koodikatkelman näkyy syntaksi HDInsightStreamingActivity käyttäminen myyntijakso JSON-tiedosto. 

Data Factory [myyntijakso](data-factory-create-pipelines.md) HDInsight-Streaming-tehtävän suorittaa Hadoop Streaming ohjelmat tai [oman](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) [tarvittaessa](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windowsin tai Linux-pohjaiset HDInsight-klusterin. Tässä artikkelissa perustuu [tietojen muunnos tehtävät](data-factory-data-transformation-activities.md) -artikkelista, jossa näkyy yleiskatsaus muuntamiseen ja tuettujen muunnos-toimintoja.

## <a name="json-sample"></a>JSON-Esimerkki
Esimerkki ohjelmien (wc.exe ja cat.exe) ja tietojen (davinci.txt) HDInsight-klusterin täytetään automaattisesti. Oletusarvon mukaan nimi, jota käytetään HDInsight-klusterin säilö on itse klusterin nimen. Esimerkiksi jos klusterinimi on myhdicluster-blob-säilö liittyvä nimi on myhdicluster. 

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<nameofthecluster>/example/apps/wc.exe",
                            "<nameofthecluster>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService",
                        "getDebugInfo": "Failure"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

Huomaa seuraavat seikat:

1. Määritä **linkedServiceName** linkitetyn-palvelun, joka osoittaa, streaming mapreduce työ suoritetaan HDInsight yhteyttä klusterin nimi.
2. Määritä **HDInsightStreaming**tehtävän tyyppi.
3. Määritä **mapper** -ominaisuutta, mapper suoritettavan tiedoston nimi. Esimerkissä cat.exe on suoritettava mapper.
4. Määritä **reducer** -ominaisuutta, reducer suoritettavan tiedoston nimi. Esimerkissä wc.exe on suoritettava reducer.
5. **Syötteen** tyyppi-ominaisuuden määrittäminen mapper lähdetiedoston (mukaan lukien sijainti). Esimerkissä: "wasb://adfsample@ <account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ": adfsample on blob-säilö, esimerkiksi/tietojen/Gutenberg on kansio ja davinci.txt on blob.
6. **Tulosteen** tyyppi-ominaisuuden määrittäminen reducer kohdetiedosto (mukaan lukien sijainti). Hadoop Streaming työn tulos kirjoitetaan tämän ominaisuuden määritettyyn sijaintiin.
7. Määritä mapper ja reducer suoritettavat polut **filePaths** -osassa. Esimerkissä: "adfsample/example/apps/wc.exe" adfsample on blob-säilö, esimerkiksi/sovellukset on kansio ja wc.exe on suoritettavan tiedoston.
8. Määritä **fileLinkedService** -ominaisuutta, Azuren tallennustilaan linkitetty palvelu, joka edustaa Azuren tallennustilaan, joka sisältää tiedostot, filePaths-kohdassa.
9. Määritä **argumentit** -ominaisuutta, streaming työn argumentit.
10. **GetDebugInfo** -ominaisuus on olemassa valinnaisena elementtinä. Kun se on määritetty epäonnistumisen, lokit ladataan vain-virheen. Kun se on määritetty kaikissa, lokit ladataan aina riippumatta suorittamisen tila.

> [AZURE.NOTE] Esimerkki esitetyllä Määritä tulostus-tietojoukko Hadoop Streaming tehtävän **tulostaa** -ominaisuuden. Tämä tietojoukko on vain tyhjä, joita tarvitaan vaikuttavat aikatauluun myyntijakso tietojoukko. Sinun ei tarvitse määrittää minkä tahansa tehtävän **syötteiden** ominaisuudelle syötteen tietojoukko.  

    
## <a name="example"></a>Esimerkki
Tätä vaiheittaista putkijohto suoritetaan sanamäärän streaming kartta ja vähennä ohjelma Azure HDInsight-klusterin. 

### <a name="linked-services"></a>Linkitetyn palvelut

#### <a name="azure-storage-linked-service"></a>Azure linkitetty tallennuspalvelu
Luo ensin linkitetyn palvelun linkki, jota käytetään Azure tietojen factory Azure Hdinsightiin klusterin Azure-tallennustilan. Jos olet kopioi ja liitä seuraava koodi, älä unohda korvaa nimen ja avaimen Azure-tallennustilan tilin nimi ja tili-näppäintä. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
            }
        }
    }

#### <a name="azure-hdinsight-linked-service"></a>Azure Hdinsightiin linkitetty-palvelu
Seuraavaksi voit luoda linkitetyn palvelun linkki Azure HDInsight-klusterin Azure tietojen factory. Jos olet kopioi ja liitä seuraava koodi, korvaa HDInsight klusterinimi HDInsight-klusterin nimen ja muuta käyttäjän nimi ja salasana-arvot. 
    
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

### <a name="datasets"></a>Tietojoukkoja

#### <a name="output-dataset"></a>Tulostus-tietojoukko
Tässä esimerkissä myyntijakso ei tule mitään syötteiden. Voit määrittää tehtävän HDInsight-tietovirta tulostus-tietojoukko. Tämä tietojoukko on vain tyhjä, joita tarvitaan vaikuttavat aikatauluun myyntijakso tietojoukko. 

    {
        "name": "StreamingOutputDataset",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/streamingdata/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Myyntijakso

Tässä esimerkissä myyntijakso on vain yksi tehtävä, jonka tyyppi on: **HDInsightStreaming**. 

Esimerkki ohjelmien (wc.exe ja cat.exe) ja tietojen (davinci.txt) HDInsight-klusterin täytetään automaattisesti. Oletusarvon mukaan nimi, jota käytetään HDInsight-klusterin säilö on itse klusterin nimen. Esimerkiksi jos klusterinimi on myhdicluster-blob-säilö liittyvä nimi on myhdicluster.  

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<blobcontainer>/example/apps/wc.exe",
                            "<blobcontainer>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

## <a name="see-also"></a>Katso myös
- [Tehtävän rakenne](data-factory-hive-activity.md)
- [Possu toiminta](data-factory-pig-activity.md)
- [MapReduce toiminta](data-factory-map-reduce.md)
- [Käynnistä ohjattu ohjelmat](data-factory-spark.md)
- [Käynnistää R komentosarjat](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

