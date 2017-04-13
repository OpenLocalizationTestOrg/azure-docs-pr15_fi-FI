<properties 
    pageTitle="Possu toiminta" 
    description="Lue käyttämisestä Possu tehtävän Azure tietojen factory-suorittamaan Possu komentosarjoja-demand/your oman HDInsight-klusterin." 
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
    ms.date="08/31/2016" 
    ms.author="shlo"/>

# <a name="pig-activity"></a>Possu toiminta
> [AZURE.SELECTOR]
[Rakenne](data-factory-hive-activity.md)  
[Possu](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Tallennettu toimintosarja](data-factory-stored-proc-activity.md)
[Tietojen järvi Analytics U-SQL](data-factory-usql-activity.md)
[.NET mukautettu](data-factory-use-custom-activities.md)

HDInsight Possu tehtävän Data Factory [myyntijakso](data-factory-create-pipelines.md) suorittaa Possu kyselyjen [oman](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) tai [tarvittaessa](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windowsin tai Linux-pohjaiset HDInsight-klusterin. Tässä artikkelissa perustuu [tietojen muunnos tehtävät](data-factory-data-transformation-activities.md) -artikkelista, jossa näkyy yleiskatsaus muuntamiseen ja tuettujen muunnos-toimintoja.

## <a name="syntax"></a>Syntaksi

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
                "inputs": [
                    {
                        "name": "input tables"
                    }
                ],
                "outputs": [
                    {
                        "name": "output tables"
                    }
                ],
                "linkedServiceName": "MyHDInsightLinkedService",
                "typeProperties": {
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## <a name="syntax-details"></a>Syntaksi tiedot

Ominaisuus | Kuvaus | Pakollinen
-------- | ----------- | --------
Nimi | Tehtävän nimi | Kyllä
kuvaus | Kuvausteksti tehtävän käyttötarkoitus | Ei
tyyppi | HDinsightPig | Kyllä
syötteiden | Yhden tai useamman syötteiden kulutettu Possu toiminnan mukaan | Ei
tulostus | Yhden tai useamman Possu tehtävän tuottamat tulostus | Kyllä
linkedServiceName | Viittaus HDInsight-klusterin rekisteröity Data Factory linkitetyn palvelu | Kyllä
komentosarja | Määritä Possu komentosarjan tekstiin | Ei
Komentosarjapolku | Voit tallentaa Possu komentosarja Azure-blob-säiliö ja tiedoston polku. 'Komentosarjan' tai 'scriptPath'-ominaisuuden avulla. Molemmat ei voi käyttää yhdessä. Isot ja pienet tiedostonimi. | Ei
määrittää | Viittaaminen sisällä Possu komentosarja kuin avain/arvo-pareina parametrien määrittäminen | Ei

## <a name="example"></a>Esimerkki

Seuraavassa Esimerkki Ottelu lokit analytics, johon haluat lisätä tunnistaa pelaajat toistaminen visualisointi n käynnistetty yrityksen aikamäärä.
 
Seuraava esimerkki Ottelu loki on pilkku (,) erotettu tiedosto. Se sisältää seuraavat kentät – Profiilitunnus, SessionStart, kesto, SrcIPAddress ja GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

**Possu komentosarjan** käsittelemään nämä tiedot:

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

Data Factory putkijohto Possu-komentosarjan suorittamiseen seuraavasti:

1. Luo linkitetty palvelu rekisteröi [oman HDInsight Laske klusterin](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) tai määritä [tarvittaessa HDInsight Laske klusterin](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Soita oletetaan, että linkitetty tämän palvelun **HDInsightLinkedService**.
2.  Luo [linkitetty palvelun](data-factory-azure-blob-connector.md) määrittäminen Azure-Blob-säiliö tietoja ylläpitävät yhteys. Soita oletetaan, että linkitetty tämän palvelun **StorageLinkedService**.
3.  Luo [tietojoukkoja](data-factory-create-datasets.md) osoittavat syötteen ja tiedot. Soita japanin syötteen tietojoukko **PigSampleIn** ja tulostus-tietojoukko **PigSampleOut**.
4.  Kopioi Possu kyselyn tiedoston Azure-Blob-objektien tallennustilaan määritetty vaiheessa #2. Jos Azuren tallennustilaan tietoja isännöivän on sama kuin, jossa tiedosto sijaitsee, luo erilliset Azuren tallennustilaan linkitetty palvelu. Lisätietoja linkitetyn palvelun määritys. Määritä polku possu komentosarjatiedosto ja **scriptLinkedService** **scriptPath **avulla. 
    
    > [AZURE.NOTE] Voit myös lisätä Possu komentosarjan sisäisen toiminnan määrityksessä **komentosarja** -ominaisuuden avulla. Kuitenkin Emme suosittele tämän menetelmän kuin komentosarja tarvitsee on sisällettävä kaikki erikoismerkit ja saattaa aiheuttaa ongelmia muistin. Paras käytäntö on vaiheen #4.
5. Luo putkijohto HDInsightPig-toimintoa. Tässä aktiviteetti käsittelee syöttötiedot suorittamalla Possu komentosarja HDInsight-klusterissa.

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. Ota käyttöön putkijohto. Lisätietoja on artikkelissa [luominen putkistot](data-factory-create-pipelines.md) artikkelissa. 
7. Seurata tietoja factory valvonnan ja hallinnan näkymien käyttäminen myyntijakso. Katso [Seuranta ja hallinta tietojen Factory putkistot](data-factory-monitor-manage-pipelines.md) artikkelin.

## <a name="specifying-parameters-for-a-pig-script"></a>Possu komentosarjan parametrien määrittäminen 

Seuraavassa on esimerkki: Ottelu lokit nautittuina päivittäin kyselyjä Azure-Blob-säiliö ja kansiossa osioitua mukaan päivämäärä ja kellonaika. Haluat parameterize Possu komentosarja ja siirtää syötteen kansion sijainnin dynaamisesti suorituksen aikana ja tuottaa myös osioitu päivämäärä ja aika.
 
Käytä Parametroitu Possu komentosarjaa, toimi seuraavasti:

- Määritä parametrit **määrittää**.

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- Possu-komentosarja viitata parametrien avulla "**$parameterName**", kuten seuraavassa esimerkissä:

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


## <a name="see-also"></a>Katso myös
- [Tehtävän rakenne](data-factory-hive-activity.md)
- [MapReduce toiminta](data-factory-map-reduce.md)
- [Hadoop Streaming tehtävä](data-factory-hadoop-streaming-activity.md)
- [Käynnistä ohjattu ohjelmat](data-factory-spark.md)
- [Käynnistää R komentosarjat](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


