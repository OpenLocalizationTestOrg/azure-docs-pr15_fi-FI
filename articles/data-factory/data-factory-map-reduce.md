<properties 
    pageTitle="Azure Data Factory MapReduce-ohjelman käynnistäminen" 
    description="Lue, miten voit käsitellä tietoja suorittamalla MapReduce ohjelmat Azure HDInsight-klusterissa Azure tietojen factory." 
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="invoke-mapreduce-programs-from-data-factory"></a>Käynnistää MapReduce ohjelmien tietojen Factory
> [AZURE.SELECTOR]
[Rakenne](data-factory-hive-activity.md)  
[Possu](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Tallennettu toimintosarja](data-factory-stored-proc-activity.md)
[Tietojen järvi Analytics U-SQL](data-factory-usql-activity.md)
[.NET mukautettu](data-factory-use-custom-activities.md)

HDInsight MapReduce tehtävän Data Factory [myyntijakso](data-factory-create-pipelines.md) suorittaa MapReduce ohjelmat tai [oman](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) [tarvittaessa](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windowsin tai Linux-pohjaiset HDInsight-klusterin. Tässä artikkelissa perustuu [tietojen muunnos tehtävät](data-factory-data-transformation-activities.md) -artikkelista, jossa näkyy yleiskatsaus muuntamiseen ja tuettujen muunnos-toimintoja.

## <a name="introduction"></a>Johdanto 
Myyntijakso-Azure tietojen factory käsittelee tietojen linkitetyn tallennustilan Services-palveluissa linkitetyn Laske-palvelujen avulla. Se sisältää toimintoja, joissa kunkin tehtävän suorittaa tiettyjä käsittely järjestyksessä. Tässä artikkelissa kuvataan HDInsight MapReduce tehtävää käytettäessä.
 
Saat lisätietoja suorittaminen Possu/rakenne komentosarjoja Windows tai Linux-pohjaiset HDInsight-klusterissa putkijohto käyttämällä HDInsight Possu ja rakenteen [Possu](data-factory-pig-activity.md) ja [rakenne](data-factory-hive-activity.md) . 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON HDInsight MapReduce tehtävälle 

JSON määrityksessä HDInsight tehtävän: 
 
1. Määritä **tehtävän** **tyyppi** **Hdinsightista**.
3. Määritä **LuokanNimi** ominaisuuden luokan nimi.
4. Määritä tiedostonimi **jarFilePath** ominaisuuden mukaan lukien PURKKI tiedoston polku.
5. Määritä linkitetyn palvelu, joka viittaa Azure-Blob-objektien tallennustilaan, joka sisältää **jarLinkedService** ominaisuuden PURKKI-tiedoston.   
6. Määritä argumentteja MapReduce ohjelman **argumentit** -osassa. Suorituksen muutamia ylimääräisiä argumentit näkyviin (esimerkiksi: mapreduce.job.tags) poikkeavat MapReduce. Erottaa että argumentit MapReduce argumentteja, kannattaa käyttää-asetus ja arvo argumentteina seuraavan esimerkin mukaisesti (- s--syöttö-,--tulosteen jne vaihtoehtoja, perässä niiden arvojen mukaan).

        {
            "name": "MahoutMapReduceSamplePipeline",
            "properties": {
                "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
                "activities": [
                    {
                        "type": "HDInsightMapReduce",
                        "typeProperties": {
                            "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                            "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                            "jarLinkedService": "StorageLinkedService",
                            "arguments": [
                                "-s",
                                "SIMILARITY_LOGLIKELIHOOD",
                                "--input",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                                "--output",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                                "--maxSimilaritiesPerItem",
                                "500",
                                "--tempDir",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                            ]
                        },
                        "inputs": [
                            {
                                "name": "MahoutInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "MahoutOutput"
                            }
                        ],
                        "policy": {
                            "timeout": "01:00:00",
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MahoutActivity",
                        "description": "Custom Map Reduce to generate Mahout result",
                        "linkedServiceName": "HDInsightLinkedService"
                    }
                ],
                "start": "2014-01-03T00:00:00Z",
                "end": "2014-01-04T00:00:00Z",
                "isPaused": false,
                "hubName": "mrfactory_hub",
                "pipelineMode": "Scheduled"
            }
        }
    
    

Voit suorittaa minkä tahansa MapReduce purkki tiedoston HDInsight-klusterin HDInsight MapReduce tehtävän. Putkijohto seuraavat JSON otoksen määrityksessä HDInsight-toiminto on määritetty suorittamaan Mahout JAR tiedoston.

## <a name="sample-on-github"></a>Valitse GitHub malli
Voit ladata käytettäessä HDInsight MapReduce aktiviteetin otoksen: [Tietomallia Factory-GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Sanamäärä-ohjelmaa
Myyntijakso tässä esimerkissä suoritetaan sanamäärän kartta ja vähennä ohjelma Azure HDInsight-klusterin.   

### <a name="linked-services"></a>Linkitetyn palvelut
Luo ensin linkitetyn palvelun linkki, jota käytetään Azure tietojen factory Azure Hdinsightiin klusterin Azure-tallennustilan. Jos olet kopioi ja liitä seuraava koodi, älä unohda nimen ja avaimen Azure-tallennustilan **tilin nimi** ja **tiliavain tiliavain** tilalle. 

#### <a name="azure-storage-linked-service"></a>Azure linkitetty tallennuspalvelu

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
Seuraavaksi voit luoda linkitetyn palvelun linkki Azure HDInsight-klusterin Azure tietojen factory. Jos olet kopioi ja liitä seuraava koodi, korvaa **HDInsight klusterinimi** HDInsight-klusterin nimen ja muuta käyttäjän nimi ja salasana-arvot.   

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
Tässä esimerkissä myyntijakso ei tule mitään syötteiden. Voit määrittää tulostus-tietojoukko HDInsight MapReduce tehtävälle. Tämä tietojoukko on vain tyhjä, joita tarvitaan vaikuttavat aikatauluun myyntijakso tietojoukko.  

    {
        "name": "MROutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "WordCountOutput1.txt",
                "folderPath": "example/data/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Myyntijakso
Tässä esimerkissä myyntijakso on vain yksi tehtävä, jonka tyyppi on: HDInsightMapReduce. Joitakin tärkeitä JSON ominaisuudet ovat seuraavat: 

Ominaisuus | Huomautuksia
:-------- | :-----
tyyppi | Tyyppi on määritettävä **HDInsightMapReduce**. 
LuokanNimi | Luokan nimi on: **wordcount**
jarFilePath | Sisältävän luokan purkki tiedoston polku. Jos olet kopioi ja liitä seuraava koodi, älä unohda klusterin nimen muuttaminen. 
jarLinkedService | Azure-tallennustilan linkitetty palvelu, joka sisältää purkki tiedoston. Linkitetyn palvelua viittaa tallennustilaa, joka on liitetty HDInsight-klusterin. 
argumentit | Wordcount ohjelma on kaksi pakollista argumenttia, arvon ja tulos. Syötteen tiedosto on davinci.txt-tiedosto.
Toistumisvälin / | Näiden ominaisuuksien arvot vastaa tulostus-tietojoukko. 
linkedServiceName | viittaa oli aiemmin luomasi linkitetty HDInsight-palvelun.   

    {
        "name": "MRSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run the Word Count Program",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "wordcount",
                        "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "/example/data/gutenberg/davinci.txt",
                            "/example/data/WordCountOutput1"
                        ]
                    },
                    "outputs": [
                        {
                            "name": "MROutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "MRActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-03T00:00:00Z",
            "end": "2014-01-04T00:00:00Z"
        }
    }

## <a name="run-spark-programs"></a>Suorita ohjattu ohjelmat
Voit suorittaa ohjattu ohjelmia HDInsight Ohjattu klusterin MapReduce tehtävän. [Käynnistä ohjattu ohjelmien Azure Data Factory](data-factory-spark.md) lisätietoja.  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com
 
## <a name="see-also"></a>Katso myös
- [Tehtävän rakenne](data-factory-hive-activity.md)
- [Possu toiminta](data-factory-pig-activity.md)
- [Hadoop Streaming tehtävä](data-factory-hadoop-streaming-activity.md)
- [Käynnistä ohjattu ohjelmat](data-factory-spark.md)
- [Käynnistää R komentosarjat](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
