<properties 
    pageTitle="Tehtävän rakenne" 
    description="Lue käyttämisestä rakenne tehtävän Azure tietojen factory-toimimaan rakenteen kyselyt-demand/your oman HDInsight-klusterin." 
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
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="hive-activity"></a>Tehtävän rakenne
> [AZURE.SELECTOR]
[Rakenne](data-factory-hive-activity.md)  
[Possu](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Tallennettu toimintosarja](data-factory-stored-proc-activity.md)
[Tietojen järvi Analytics U-SQL](data-factory-usql-activity.md)
[.NET mukautettu](data-factory-use-custom-activities.md)

Data Factory [myyntijakso](data-factory-create-pipelines.md) HDInsight-rakenne-tehtävän suorittaa rakenteen kyselyjen [oman](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) tai [tarvittaessa](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windowsin tai Linux-pohjaiset HDInsight-klusterin. Tässä artikkelissa perustuu [tietojen muunnos tehtävät](data-factory-data-transformation-activities.md) -artikkelista, jossa näkyy yleiskatsaus muuntamiseen ja tuettujen muunnos-toimintoja.

## <a name="syntax"></a>Syntaksi

    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
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
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }
    
## <a name="syntax-details"></a>Syntaksi tiedot

Ominaisuus | Kuvaus | Pakollinen
-------- | ----------- | --------
Nimi | Tehtävän nimi | Kyllä
kuvaus | Kuvausteksti tehtävän käyttötarkoitus | Ei
tyyppi | HDinsightHive | Kyllä
syötteiden | Syötteiden kulutettu rakenteen toiminnan mukaan | Ei
tulostus | Rakenne-toimintojen tuottamat tulostus | Kyllä 
linkedServiceName | Viittaus HDInsight-klusterin rekisteröity Data Factory linkitetyn palvelu | Kyllä 
komentosarja | Määritä sidottua komentosarjan rakenne | Ei
Komentosarjapolku | Tallenna rakenne-komentosarjan Azure-blob-säiliö ja anna tiedoston polku. 'Komentosarjan' tai 'scriptPath'-ominaisuuden avulla. Molemmat ei voi käyttää yhdessä. Isot ja pienet tiedostonimi. | Ei 
määrittää | Viittaaminen sisällä rakenne-komentosarja käyttäminen 'hiveconf' kuin avain/arvo-pareina parametrien määrittäminen  | Ei

## <a name="example"></a>Esimerkki

Seuraavassa on esimerkki Ottelu lokit analytics, johon haluat määrittää toistaminen visualisointi n käynnistetty yrityksen käyttäjien aikaa. 

Seuraavat loki on esimerkki Ottelu lokia, joka on pilkku (`,`) erotettu ja sisältää seuraavat kentät – Profiilitunnus, SessionStart, kesto, SrcIPAddress ja GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

**Rakenne komentosarjan** käsittelemään nämä tiedot:

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID       string, 
        SessionStart    string, 
        Duration        int, 
        SrcIPAddress    string, 
        GameType        string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
    
    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (   
        ProfileID   string, 
        Duration    int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
    
    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

Data Factory putkijohto rakenne-komentosarjan suorittamiseen, sinun on suoritettava seuraavat

1. Luo linkitetty palvelu rekisteröi [oman HDInsight Laske klusterin](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) tai määritä [tarvittaessa HDInsight Laske klusterin](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Soita oletetaan, että linkitetyn palvelua "HDInsightLinkedService".
2. Luo [linkitetty palvelun](data-factory-azure-blob-connector.md) määrittäminen Azure-Blob-säiliö tietoja ylläpitävät yhteys. Soita oletetaan, että linkitetyn "StorageLinkedService-palvelu
3. Luo [tietojoukkoja](data-factory-create-datasets.md) osoittavat syötteen ja tiedot. Soita japanin syötteen tietojoukko "HiveSampleIn" ja "HiveSampleOut" tulostus-tietojoukko
4. Kopioi Azure-Blob-säiliö tiedostona rakenne-kyselyn määritetty vaiheessa #2. Jos tietoja ylläpitävät tallennustila on eri isännöinnin tämän kyselytiedoston, luo erilliset Azuren tallennustilaan linkitetty palvelu ja viittaa siihen tehtävän. **ScriptPath **avulla voit määrittää kyselyn rakennetiedostoon ja Määritä Azure tallennustilaa, joka sisältää komentosarjatiedosto **scriptLinkedService** polun. 

    > [AZURE.NOTE] Voit myös lisätä rakenne komentosarjan sisäisen toiminnan määrityksessä **komentosarja** -ominaisuuden avulla. Emme suosittele tämän menetelmän kuin kaikkien komentosarja on sisällettävä tarpeet JSON asiakirjan sisällä erikoismerkkejä ja saattaa aiheuttaa ongelmia muistin. Paras käytäntö on vaiheen #4.
5.  Luo putkijohto HDInsightHive-toimintoa. Tehtävän prosessit/muunnoksia tiedot.

        {
          "name": "HiveActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                  {
                    "name": "HiveSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "HiveSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
              }
            ]
          }
        }

6.  Ota käyttöön putkijohto. Lisätietoja on artikkelissa [luominen putkistot](data-factory-create-pipelines.md) artikkelissa. 
7.  Seurata tietoja factory valvonnan ja hallinnan näkymien käyttäminen myyntijakso. Katso [Seuranta ja hallinta tietojen Factory putkistot](data-factory-monitor-manage-pipelines.md) artikkelin. 


## <a name="specifying-parameters-for-a-hive-script"></a>Parametrien määrittämisen rakenne-komentosarja  
Tässä esimerkissä Ottelu lokit päivittäin kyselyjä Azure-Blob-säiliö nautittuina ja tallennetaan kansioon, osioitu päivämäärä ja aika. Haluat parameterize rakenne-komentosarjan ja siirtää syötteen kansion sijainnin dynaamisesti suorituksen aikana ja tuottaa myös osioitu päivämäärä ja aika.

Voit käyttää Parametroitu rakenne-komentosarjan, toimimalla seuraavasti

- Määritä parametrit **määrittää**.

        {
            "name": "HiveActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                        {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                        {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                }
            ]
          }
        }

- Rakenne-komentosarjan viitata parametria **${hiveconf:parameterName}**. 

        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID   string, 
            SessionStart    string, 
            Duration    int, 
            SrcIPAddress    string, 
            GameType    string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
        
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID   string, 
            Duration    int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
        
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID


## <a name="see-also"></a>Katso myös
- [Possu toiminta](data-factory-pig-activity.md)
- [MapReduce toiminta](data-factory-map-reduce.md)
- [Hadoop Streaming tehtävä](data-factory-hadoop-streaming-activity.md)
- [Käynnistä ohjattu ohjelmat](data-factory-spark.md)
- [Käynnistää R komentosarjat](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)









