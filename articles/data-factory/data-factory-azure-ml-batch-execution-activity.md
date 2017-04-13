<properties 
    pageTitle="Koneen Learning toimet | Microsoft Azure" 
    description="Kerrotaan, miten voit luoda luominen ennakoivan putkistot Azure Data Factory ja Azure koneen oppiminen" 
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
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="create-predictive-pipelines-using-azure-machine-learning-activities"></a>Luo ennakoivan putkistot Azure koneen Learning tehtävien avulla   
> [AZURE.SELECTOR]
[Rakenne](data-factory-hive-activity.md)  
[Possu](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Tallennettu toimintosarja](data-factory-stored-proc-activity.md)
[Tietojen järvi Analytics U-SQL](data-factory-usql-activity.md)
[.NET mukautettu](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Johdanto

[Azure koneen Learning](https://azure.microsoft.com/documentation/services/machine-learning/) mahdollistaa muodosta, Testaa ja ota käyttöön ennakoivan analytics-ratkaisuja. Ylätason näkökulmasta se tehdään kolme vaihetta: 

1. **Luo koulutus kokeen**. Voit tehdä tämän vaiheen Azure ML Studiossa. ML studio on yhteiskäytön visual kehitysympäristö, jonka avulla kouluttaminen ja testaa koulutus tiedoista ennakoivan analytics-malli.
2. **Muunna ennakoivan kokeen**. Kun malli on poistettu koulutus olemassa olevien tietojen kanssa ja haluat sen avulla uusia tietoja pisteet, valmisteleminen ja nopeuttaa oman koe, näkyvissä pistemäärä.
3. **Ottaa käyttöön sen verkkopalvelun**. Voit julkaista tulosmalli kokeen Azure web-palveluun. Voit lähettää tietoja kautta tämän web-palvelu lopuksi mallin ja vastaanottaa tuloksen ennusteiden tullutta mallin.  

Azure Data Factory avulla voit helposti luoda putkistot, jotka käyttävät julkaistun [Azure koneen Learning] [ azure-machine-learning] WWW-palvelun ennakoivan analysoinnissa. Lisätietoja [Azure Data Factory johdanto](data-factory-introduction.md) ja [luoda ensimmäisen myyntijakso](data-factory-build-your-first-pipeline.md) pääset nopeasti alkuun Azure Data Factory-palvelussa. 

**Erän suorittamisen toimintojen** käyttäminen Azure Data Factory-myyntijakso, voit käynnistää Azure ML WWW-palvelun tekemään ennusteiden tietojen osalta. Lisätietoja on artikkelissa [käynnistettäessä Azure-ML web-palvelu erä suorittamisen tehtävän](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) osan.

Ajan myötä näkyvissä pistemäärä kokeissa Azure millilitroina ennakoivan malleja on retrained käyttämällä uusi syötteen tietojoukkoja. Voit suuntaamista Data Factory putkijohto Azure ML mallia toimimalla seuraavasti: 

1. Julkaise verkkopalvelun koulutus kokeen (ei ennakoivan kokeen). Voit tehdä tämän vaiheen Azure ML Studiossa samoin kuin näyttää ennakoivan kokeen edellisen skenaariossa WWW-palvelulle.
2. Azure ML erä suorittamisen toimintojen avulla voit käynnistää koulutus kokeen web-palvelu. Lähinnä voit käynnistää sekä koulutus-verkkopalvelun tulosmalli verkkopalvelun Azure ML eräkäsittely tehtävän. 
  
Sen jälkeen, kun olet käsitellyt uudelleenkoulutusta, haluat päivittää tulosmalli WWW-palvelun (ennakoivan kokeen web-palveluna) juuri koulutetun mallia. Tee näin: 

1. Lisätä muun kuin oletusarvoisen lopuksi tulosmalli web-palveluun. WWW-palvelun päätepisteelle oletusarvon ei voi päivittää, joten sinun täytyy luoda muun kuin oletusarvoisen loppupisteen Azure-portaalissa. [Luo päätepisteet](../machine-learning/machine-learning-create-endpoint.md) on artikkelissa sekä käsitteellisiä tietoja kirjaamisesta vaiheet.
2. Päivitä aiemmin luotu linkitetty Azure ML palvelut näkyvissä pistemäärä käyttämään muun kuin oletusarvoisen päätepisteen. Voit aloittaa uuden päätepisteen käyttämään verkkopalveluun, joka on päivitetty.
3. **Azure ML Päivitä resurssin toimintojen** avulla voit päivittää WWW-palvelun juuri koulutetun mallia.  

Katso [päivittäminen Azure ML mallien päivityksen resurssi-toiminnon käyttäminen](#updating-azure-ml-models-using-the-update-resource-activity) osassa. 

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Käynnistettäessä verkkopalvelun erä suorittamisen toimintojen käyttäminen

Tietojen siirto ja käsittelyn orchestrate Azure Data Factory avulla ja tee sitten eräkäsittely Azure koneen Learning avulla. Seuraavassa on ylimmän tason vaiheet:

1. Luo linkitetty Azure koneen Learning-palvelu. Tarvitset seuraavat:
    1. Eräkäsittely API **pyytää URI** . Löydät pyytää URI napsauttamalla services verkkosivun **ERÄKÄSITTELY** -linkkiä.
    1. Julkaistun Azure koneen Learning verkkopalvelun **Ohjelmointirajapinnan avain** . Löydät Ohjelmointirajapinnan avain valitsemalla verkkopalveluun, joka on julkaistu. 
 2. Käytä **AzureMLBatchExecution** -tehtävän.

    ![Koneen Learning Raporttinäkymät-ikkunan](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

    ![Erän URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)


### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Skenaario: Kokeilua käyttäen Web palvelun syötteiden ja tulostaa, jotka viittaavat tietojen Azure-Blob-objektien tallennustilaan
Tässä skenaariossa Azure koneen Learning Web-palvelu on tietoja käyttämällä Azure-blob-säiliö tiedoston ennusteiden ja tallentaa tekstinsyöttö tulokset Blob-objektien tallennustilaan. Seuraavat JSON määrittää Data Factory-putkijohto ja AzureMLBatchExecution tehtävän. Tehtävä on tietojoukko **DecisionTreeInputBlob** syötteenä ja **DecisionTreeResultBlob** kuin tulos. **DecisionTreeInputBlob** välitetään WWW-palvelun syötteeksi **webServiceInput** JSON-ominaisuuden avulla. **DecisionTreeResultBlob** välitetään kuin tulos WWW-palvelun **webServiceOutputs** JSON-ominaisuuden avulla.  

> [AZURE.IMPORTANT] 
> Jos WWW-palvelun kestää useita syötteiden, sijaan **webServiceInput** **webServiceInputs** -ominaisuuden avulla. On kohdassa [Web-palvelu edellyttää useita syötteiden](#web-service-requires-multiple-inputs) Esimerkki webServiceInputs-ominaisuuden avulla.
>  
> Tietojoukkoja, joihin viitataan **webServiceInput**/**webServiceInputs** ja **webServiceOutputs** ominaisuudet ( **typeProperties**) täytyy sisältyä myös tehtävän **syötteiden** ja **tulostaa**.
> 
> Azure ML-kokeessa-web-palvelu syötteen ja portit ja yleiset parametrit on oletusnimet ("input1", "input2"), jotka voit mukauttaa. Käytät webServiceInputs, webServiceOutputs ja globalParameters asetukset nimet on oltava täsmälleen samat kokeet nimet. Voit tarkastella otoksen pyynnön paketti erä suorittamisen Ohjesivun Azure ML päätepisteelle vahvistamiseksi odotettu yhdistämismääritys. 


    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "DecisionTreeInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "DecisionTreeResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties":
            {
                "webServiceInput": "DecisionTreeInputBlob",
                "webServiceOutputs": {
                    "output1": "DecisionTreeResultBlob"
                }                
            },
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

> [AZURE.NOTE] Vain syötteiden ja tulostaa AzureMLBatchExecution tehtävän voi välittää parametreiksi Web-palveluun. Yllä JSON-koodikatkelman DecisionTreeInputBlob on AzureMLBatchExecution-toimintoon, joka on välitetty syötteeksi Web-palveluun webServiceInput parametrin arvon.   

### <a name="example"></a>Esimerkki

Tässä esimerkissä käytetään Azuren tallennustilaan sisältää syöttö- ja -tietoja. 

On suositeltavaa kerrotaan [muodostaa yhteyttä ensimmäisen putkijohto ja Data Factory] [ adf-build-1st-pipeline] opetusohjelman ennen tämän esimerkin loppuun. Tietoja Factory-editorin avulla voit luoda Data Factory palvelutiedot (linkitetyn services, tietojoukkoja myyntijakso) tässä esimerkissä.   
 

1. Luo **linkitetty service** - **Azure-tallennustilan**. Jos syöttö- ja tiedostot ovat eri tallennustilan tilin, sinun on kaksi linkitetyn palvelut. Seuraavassa on esimerkki JSON:

        {
          "name": "StorageLinkedService",
          "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
            }
          }
        }

2. Luo **syötteen** Azure Data Factory **tietojoukko**. Toisin kuin joitakin muita tietoja Factory tietojoukkoja nämä tietojoukkoja on sisällettävä **kansiopolku** ja **Tiedostonimi** . Voit käyttää jakaminen aiheuttaa prosessin tai tuottaa yksilöllinen syötteen ja tiedostoja kunkin eräkäsittely (kukin tietojen sektori). Joudut ehkä sisältää joitakin ylöspäin tehtävän muuntaa syötteen CSV-tiedostomuoto ja sijoittaminen kunkin sektorin tallennustilan-tili. Siinä tapauksessa seuraavan esimerkin mukainen **ulkoisista** ja **externalData** asetuksia ei sisällyttää ja että DecisionTreeInputBlob olisi tulostus-tietojoukko eri osat.

        {
          "name": "DecisionTreeInputBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/input",
              "fileName": "in.csv",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Day",
              "interval": 1
            },
            "policy": {
              "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
              }
            }
          }
        }
    
    Syötteen csv-tiedoston on oltava sarakkeen otsikkorivi. Jos käytät **Kopioi aktiviteetin** luominen tai Siirrä csv-blob-säiliö kyselyjä, sinun kannattaa asettaa käsittelytoiminto ominaisuuden **blobWriterAddHeader** **Tosi**. Esimerkki:
    
         sink: 
         {
             "type": "BlobSink",     
             "blobWriterAddHeader": true 
         }
     
    Jos otsikkorivillä ei ole csv-tiedoston, näkyviin voi tulla seuraava virhe: **tehtävän virhe: Virhe luettaessa merkkijono. Odottamaton tunnus: StartObject. Polku ", rivi 1, sijoita 1**.
3. Luo **tulosteen** Azure Data Factory **tietojoukko**. Tässä esimerkissä jakaminen luoda yksilölliset tulosteen polku kunkin sektorin suorittamista varten. Tehtävän korvaa ilman jakaminen, tiedosto.

        {
          "name": "DecisionTreeResultBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/scored/{folderpart}/",
              "fileName": "{filepart}result.csv",
              "partitionedBy": [
                {
                  "name": "folderpart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "yyyyMMdd"
                  }
                },
                {
                  "name": "filepart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HHmmss"
                  }
                }
              ],
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Day",
              "interval": 15
            }
          }
        }

4. Luo **linkitetty palvelun** tyyppi: **AzureMLLinkedService**, jotka tarjoavat Ohjelmointirajapinnan avain ja mallin erä suorittamisen URL-osoite.
        
        {
          "name": "MyAzureMLLinkedService",
          "properties": {
            "type": "AzureML",
            "typeProperties": {
              "mlEndpoint": "https://[batch execution endpoint]/jobs",
              "apiKey": "[apikey]"
            }
          }
        }
5. Luo lopuksi sisältävä **AzureMLBatchExecution** tehtävän putkijohto. Suorituksen myyntijakso tekee seuraavat toimet:
    1. Hakee syötteen tietojoukkoja input-tiedoston sijainti.
    2. Käynnistää Azure koneen Learning eräkäsittely Ohjelmointirajapinta
    3. Kopioi tulosteen tietojoukko alan Blob-objektien erä suorittamisen tulos. 

    > [AZURE.NOTE] AzureMLBatchExecution tehtävän voi olla nolla tai syötteiden ja vähintään yhden tulostaa.

        {
          "name": "PredictivePipeline",
          "properties": {
            "description": "use AzureML model",
            "activities": [
              {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                  {
                    "name": "DecisionTreeInputBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "DecisionTreeResultBlob"
                  }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                  "concurrency": 3,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 1,
                  "timeout": "02:00:00"
                }
              }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
          }
        }

    **Käynnistä** ja **Lopeta** päivämääriä ja aikoja on oltava [ISO-muodossa](http://en.wikipedia.org/wiki/ISO_8601). Esimerkki: 2014-10-14T16:32:41Z. **Päättymisaika** on valinnainen. Jos **end** -ominaisuuden arvoa ei määritetä, sen lasketaan muodossa "**Käynnistä + 48 tuntia.**" Suorita putkijohto jatkuvasti, Määritä **9999-09-09** **end** -ominaisuuden arvon. Katso lisätietoja JSON ominaisuudet [JSON komentosarjan viittaus](https://msdn.microsoft.com/library/dn835050.aspx) .

    > [AZURE.NOTE] Syöte AzureMLBatchExecution, joka määrittää tehtävän on valinnainen. 

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Skenaario: Kokeilua eri tallennukset tietoihin viitata Reader/Writer moduulit avulla

Toisen Azure ML kokeissa luotaessa käytetty vaihtoehto on käyttää ja moduulit. Lukija-moduulin käytetään ladata tietoja kokeessa ja writer-moduuli on tallentamaan tietoja oman kokeissa. Lisätietoja ja moduulit on ohjeaiheissa [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) ja [kirjoittajan](https://msdn.microsoft.com/library/azure/dn905984.aspx) MSDN-kirjastossa.     

Kun käytät ja moduulit, on hyvä käyttämään Web-palvelun parametri kullekin ominaisuudelle reader/writer nämä moduulit. Näiden web-parametrien avulla voit määrittää arvot suorituksen aikana. Voit esimerkiksi luoda kokeen lukija-moduulissa, joka käyttää Azure SQL-tietokantaan: XXX.database.windows.net. Web-palvelu on otettu käyttöön, kun haluat ottaa käyttöön WWW-palvelun kuluttajille toiseen Azure SQL Serverin kutsutaan YYY.database.windows.net määrittäminen. Voit käyttää Web-palvelu-parametrin arvoksi on määritetty sallimaan.

> [AZURE.NOTE] Web-palvelu syötteen ja tulosteen poikkeavat WWW-palveluparametrit. Ensimmäinen skenaario on nähdä, miten syötteen ja tulosteen voidaan määrittää Azure ML WWW-palvelun. Tässä skenaariossa välittää verkkopalvelun parametrit, jotka vastaavat reader/writer moduulit ominaisuudet. 

Katsotaan tilanne WWW-palvelun parametrien käytöstä. Sinulla on otettu käyttöön, joka käyttää lukija-moduulin voit lukea tietoja yhdestä Azure koneen Learning tukemat tietolähteet Azure koneen Learning web-palvelu (esimerkiksi: Azure SQL-tietokanta). Kun Eräkäsittely suoritetaan, tulokset kirjoitetaan Writer-moduulin (Azure SQL-tietokanta).  Ei ole WWW-palvelun syötteiden ja tulostaa määritetään kokeet. Tässä tapauksessa on suositeltavaa, että määrität ja moduulit asiaa web palveluparametrit. Määritysten avulla lukija/writer moduulit on määritettävä AzureMLBatchExecution tehtävän käytettäessä. Määrität WWW-palvelun parametrien tehtävän JSON **globalParameters** -osassa seuraavasti. 


    "typeProperties": {
        "globalParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }

Voit käyttää myös [Tietojen Factory Funktiot](https://msdn.microsoft.com/library/dn835056.aspx) -kulkeva arvot WWW-palvelun parametrien seuraavan esimerkin mukaisesti:

    "typeProperties": {
        "globalParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] WWW-palvelun parametrien kirjainkoko on merkitsevä, joten varmista, että määrität tehtävän nimet JSON samat WWW-palvelun tarjoamista. 

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Lukija-moduulin avulla voit lukea tietoja useiden tiedostojen Azure-Blob
Big datasta putkijohdot toimintoja, kuten Possu kanssa ja rakenteen voi tuottaa yhden tai useamman tulosteen tiedostot on ei. Kun määrität ulkoisen rakennetaulukko, ulkoisen rakennetaulukko tiedot tallennettu esimerkiksi Azure Blob-objektien tallennustilaan seuraavat nimi 000000_0 kanssa. Voit lukea useita tiedostoja kokeessa lukija-moduulin avulla ja käyttää niitä ennusteiden. 

Käytettäessä lukija-moduulin Azure koneen Learning kokeessa, voit määrittää Azure-Blob syötteeksi. Azure-blob-säiliö tiedostot voivat olla tulostus-tiedostot (Esimerkki: 000000_0), joka on valmistettu Possu ja rakenteen komentosarja HDInsight-käyttöjärjestelmässä. Lukija-moduulin avulla voit lukea tiedostoja (jossa ei ole laajennuksia) määrittämällä **polku säilöön, hakemisto-Blob-objektien**. **Säilön polku** kohdeosoite säilö ja **Hakemisto-Blob-objektien** kohdeosoite-kansio, joka sisältää tiedostot seuraavassa kuvassa esitetyllä tavalla. Tähti eli \*) **määrittää, että kaikki säilö/kansiossa olevat tiedostot (eli tietojen/aggregateddata/vuosi = 2014/kuukausi-6 /\*)** luetaan kokeen osana.

![Azure Blob-ominaisuudet](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)


### <a name="example"></a>Esimerkki 
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Web-palvelu parametreilla aktiviteettiin AzureMLBatchExecution putkijohto

    {
      "name": "MLWithSqlReaderSqlWriter",
      "properties": {
        "description": "Azure ML model with sql azure reader/writer",
        "activities": [
          {
            "name": "MLSqlReaderSqlWriterActivity",
            "type": "AzureMLBatchExecution",
            "description": "test",
            "inputs": [
              {
                "name": "MLSqlInput"
              }
            ],
            "outputs": [
              {
                "name": "MLSqlOutput"
              }
            ],
            "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
            "typeProperties":
            {
                "webServiceInput": "MLSqlInput",
                "webServiceOutputs": {
                    "output1": "MLSqlOutput"
                }
                "globalParameters": {
                    "Database server name": "<myserver>.database.windows.net",
                    "Database name": "<database>",
                    "Server user account name": "<user name>",
                    "Server user account password": "<password>"
                }              
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            },
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }
 
JSON edellä olevassa esimerkissä:

- Käyttöön Azure koneen Learning Web-palvelu käyttää lukuohjelmaa ja kirjoittajan moduuli luku ja kirjoita tiedot / Azure SQL-tietokantaan. Web-palvelu sisältää seuraavat neljä parametria: Database-palvelimen nimen, tietokannan nimi, palvelimen käyttäjänimi ja palvelimen käyttäjän salasana.  
- **Käynnistä** ja **Lopeta** päivämääriä ja aikoja on oltava [ISO-muodossa](http://en.wikipedia.org/wiki/ISO_8601). Esimerkki: 2014-10-14T16:32:41Z. **Päättymisaika** on valinnainen. Jos **end** -ominaisuuden arvoa ei määritetä, sen lasketaan muodossa "**Käynnistä + 48 tuntia.**" Suorita putkijohto jatkuvasti, Määritä **9999-09-09** **end** -ominaisuuden arvon. Katso lisätietoja JSON ominaisuudet [JSON komentosarjan viittaus](https://msdn.microsoft.com/library/dn835050.aspx) .

### <a name="other-scenarios"></a>Muissa tilanteissa

#### <a name="web-service-requires-multiple-inputs"></a>Web-palvelu edellyttää useita syötteiden
Jos WWW-palvelun kestää useita syötteiden, sijaan **webServiceInput** **webServiceInputs** -ominaisuuden avulla. Tietojoukkoja, joihin viitataan **webServiceInputs** on myös sisällytettävä tehtävä- **syötteitä**.
 
Azure ML-kokeessa-web-palvelu syötteen ja portit ja yleiset parametrit on oletusnimet ("input1", "input2"), jotka voit mukauttaa. Käytät webServiceInputs, webServiceOutputs ja globalParameters asetukset nimet on oltava täsmälleen samat kokeet nimet. Voit tarkastella otoksen pyynnön paketti erä suorittamisen Ohjesivun Azure ML päätepisteelle vahvistamiseksi odotettu yhdistämismääritys.


    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [{
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [{
                    "name": "inputDataset1"
                }, {
                    "name": "inputDataset2"
                }],
                "outputs": [{
                    "name": "outputDataset"
                }],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": "inputDataset1",
                        "input2": "inputDataset2"
                    },
                    "webServiceOutputs": {
                        "output1": "outputDataset"
                    }
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }

#### <a name="web-service-does-not-require-an-input"></a>Web-palvelu ei edellytä syöte

Azure ML erä suorittamisen-verkkopalvelut voidaan suorittaa työnkulkuja, esimerkiksi R tai Python komentosarjojen, jotka eivät vaadi minkä tahansa syötteiden. Voit myös kokeilla on ehkä määritetty Reader moduuli, jolla ei näytä mitään GlobalParameters kanssa. Tässä tapauksessa AzureMLBatchExecution tehtävän määritetty seuraavasti:

    {
        "name": "scoring service",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "myBlob"
            }
        ],
        "typeProperties": {
            "webServiceOutputs": {
                "output1": "myBlob"
            }              
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },
   

#### <a name="web-service-does-not-require-an-inputoutput"></a>Web-palvelu ei edellytä syöte/tulos
Azure ML erä suorittamisen web-palvelu ei ehkä ole määritetty verkkopalvelun tuloksen. Tässä esimerkissä ei ole verkkopalvelun syötteen tai tulosteen eikä mitään GlobalParameters määritetään. Määritetty tehtävä itse tulos on edelleen, mutta se ei ole annettu webServiceOutput.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "placeholderOutputDataset"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Web-palvelu käyttää lukijat ja kirjoittajat ja tehtävän toimii vain, kun muita toimintoja on onnistui

Azure ML web palvelun ja moduulit on ehkä määritetty suorittamaan kanssa tai ilman mitään GlobalParameters. Kuitenkin haluat upottaa palvelun puhelut putkijohto, joka käyttää tietojoukko riippuvuudet käynnistää palvelun vain, kun joitakin ylöspäin käsittely on valmis. Voit myös käynnistää jonkin muun toiminnon jälkeen eräkäsittely tämän menetelmän avulla. Siinä tapauksessa voit ilmaista käyttäminen ilman nimeäminen halutut arvosarjat verkkopalvelun syötteiden tai tulostaa tehtävän syötteiden ja tulostaa, riippuvuudet.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "inputs": [
            {
                "name": "upstreamData1"
            },
            {
                "name": "upstreamData2"
            }
        ],
        "outputs": [
            {
                "name": "downstreamData"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

**Takeaways** ovat seuraavat:

-   Jos kokeen päätepisteen on käytössä webServiceInput: se edustaa Blob-objektien tietojoukkoa ja sisältyy tehtävän syötteiden ja webServiceInput-ominaisuus. Muussa tapauksessa webServiceInput-ominaisuuden jätetään pois. 
-   Jos kokeen päätepisteen käyttää webServiceOutput(s): ne ilmaistaan blob tietojoukkoja ja tehtävä-tulostus ja webServiceOutputs-ominaisuudessa. Tehtävän tulostaa ja webServiceOutputs yhdistetään kunkin tulosteen kokeessa nimen mukaan. Muussa tapauksessa webServiceOutputs-ominaisuuden jätetään pois.
-   Jos kokeen päätepiste paljastaa globalParameter(s), käyttäjät saavat tehtävän globalParameters ominaisuutta avaimen-arvo-pareina. Muussa tapauksessa globalParameters-ominaisuuden jätetään pois. Näppäimet kirjainkoko on merkitsevä. [Azure Data Factory-funktioita](data-factory-scheduling-and-execution.md#data-factory-functions-reference) voidaan määrittää arvot. 
- Tehtävän syötteiden ja tulostaa ominaisuuksiin, voidaan sisällyttää muita tietojoukkoja ilman viitattuun tehtävän typeProperties. Nämä tietojoukkoja ohjaavat suorittaminen sektoria riippuvuudet, mutta ohitetaan muussa AzureMLBatchExecution toiminnan mukaan. 


## <a name="updating-models-using-update-resource-activity"></a>Mallien käyttäminen päivityksen resurssin tehtävän päivittäminen
Ajan myötä näkyvissä pistemäärä kokeissa Azure millilitroina ennakoivan malleja on retrained käyttämällä uusi syötteen tietojoukkoja. Kun olet käsitellyt uudelleenkoulutusta, haluat päivittää tulosmalli WWW-palvelun retrained ML mallia. Voit ottaa käyttöön uudelleenkoulutuksen ja päivityksessä Azure ML mallien WWW-palveluiden kautta tyypillinen vaiheet ovat: 

1. Luo kokeen [Azure ML](https://studio.azureml.net)Studiossa. 
2. Kun olet tyytyväinen mallin, Azure-ML Studiolla voit julkaista web-palvelut sekä **koulutusta kokeilla** ja näkyvissä pistemäärä /**ennakoivan kokeen**.

Seuraavassa taulukossa on kuvattu tässä esimerkissä käytetään verkkopalvelut.  Katso lisätietoja [suuntaamista koneen Learning mallit ohjelmallisesti](../machine-learning/machine-learning-retrain-models-programmatically.md) .

| WWW-palvelun tyyppi | kuvaus 
| :------------------ | :---------- 
| **Koulutus web-palvelu** | Vastaanottaa koulutustietoja ja tuottaa koulutetun mallit. Tulos uudelleenkoulutusta on Azure-Blob-säiliö .ilearner tiedosto.  **Oletusarvoinen päätepiste** luodaan automaattisesti, kun julkaiset koulutus kokeen WWW-palvelulle. Voit luoda lisää päätepisteet mutta esimerkissä käytetään vain oletusarvoinen päätepiste |
| **Näkyvissä WWW-palvelun pistemäärä** | Vastaanottaa Nimetön tietojen esimerkkejä ja tekee ennusteiden. Ennakoiva tekstinsyöttö tulos voi olla eri muodoissa, kuten .csv-tiedoston tai rivien Azure SQL-tietokantaan, kokeilla asetuksista riippuen. Oletusarvoinen päätepiste luodaan automaattisesti, kun julkaiset ennakoivan kokeen WWW-palvelulle. Luo toisen **muun kuin oletusarvoisen ja päivitettäviä päätepisteen** avulla [Azure portal](https://manage.windowsazure.com). Voit luoda lisää päätepisteet, mutta tässä esimerkissä käytetään vain yhden muun kuin oletusarvoisen päivitettäviä päätepiste. [Luo päätepisteet](../machine-learning/machine-learning-create-endpoint.md) on artikkelissa ohjeita.       
 
Seuraava kuva esittää koulutus ja näkyvissä päätepisteet pistemäärä Azure millilitroina välisen suhteen. 

![Verkkopalvelut](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)


Voit käynnistää **koulutus verkkopalvelun** **Azure ML erä suorittamisen toimintojen**avulla. Käynnistettäessä koulutus web-palvelu on sama kuin käynnistettäessä Azure ML-verkkopalvelun, (näkyvissä pistemäärä verkkopalvelun) tulosmalli tiedoille. Edellä olevissa osioissa kansilehden, voit käynnistää Azure ML WWW-palvelun from Azure Data Factory-myyntijakso yksityiskohtaiset tiedot. 
  
Voit käynnistää **näkyvissä WWW-palvelun pistemäärä** Päivitä WWW-palvelun juuri koulutetun mallin **Azure ML Päivitä resurssin tehtävän** avulla. Kuten edellä olevassa taulukossa mainitaan, Luo ja käyttää muun kuin oletusarvoisen päivitettäviä päätepistettä. Päivitä lisäksi olemassa olevan linkitetyn palvelut-tiedot-factory käyttämään muun kuin oletusarvoisen päätepisteen niin, että ne Käytä aina retrained malli. 

Seuraavassa on lisätietoja. Se on esimerkki uudelleenkoulutusta ja päivityksessä Azure Data Factory-myyntijakso Azure ML malleja. 
 
### <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Skenaario: uudelleenkoulutusta ja Azure ML mallin päivitys
Tässä osassa on esimerkki putkijohto, joka käyttää **Azure ML eräkäsittely tehtävän** suuntaamista malli. Putkijohto käyttää **Azure ML Päivitä resurssin tehtävän** myös päivittää mallin tulosmalli WWW-palvelun käyttöön. Osan sisältää myös JSON katkelmat linkitetyn services, tietojoukkoja ja myyntijakso esimerkissä. 

Tässä on esimerkki putkijohto kaavionäkymään. Kuten näet, Azure ML erä suorittamisen tehtävän koulutus-syöte tulee ja tuottaa koulutus-tulokset (iLearner-tiedosto). Azure ML päivityksen resurssin tehtävään tulee koulutus-tulostus ja päivittää mallin käyttöön tulosmalli WWW-palvelupäätepiste. Päivitä resurssien tehtävä ei tuota tuloksen. PlaceholderBlob on juuri, joita tarvitaan Azure Data Factory-palvelu suorittaa putkijohto tyhjä tulos tietojoukko. 

![myyntijakso-kaavio](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


#### <a name="azure-blob-storage-linked-service"></a>Azure-Blob-säiliö linkitetty palvelu:
Azure-tallennustilan sisältää seuraavat tiedot:

- koulutus tiedot. Azure ML koulutus-verkkopalvelun syöttötiedot.  
- iLearner-tiedosto. Azure ML koulutus verkkopalvelusta tulos. Tiedosto on myös syötteen päivityksen resurssi-toimintoon.  
   
Tässä on esimerkki JSON määritys on linkitetty palvelun: 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
            }
        }
    }


#### <a name="training-input-dataset"></a>Koulutus syötteen tietojoukko:
Seuraava tietojoukon edustaa Azure ML koulutus WWW-palvelun syötteen koulutus-tietoja. Azure ML eräkäsittely tehtävän otetaan syötteeksi tämä tietojoukko. 

    {
        "name": "trainingData",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "labeledexamples",
                "fileName": "labeledexamples.arff",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "policy": {          
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

#### <a name="training-output-dataset"></a>Koulutus tulosteen tietojoukko:
Seuraava tietojoukon edustaa iLearner kohdetiedosto Azure ML koulutus web-palveluun. Azure ML erä suorittamisen tehtävän tuottaa tämä tietojoukko. Tämä tietojoukko on myös Azure ML päivityksen resurssin toimintoon syöte.

    {
        "name": "trainedModelBlob",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "trainingoutput",
                "fileName": "model.ilearner",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            }
        }
    }

#### <a name="linked-service-for-azure-ml-training-endpoint"></a>Linkitetyn palvelun Azure ML koulutus päätepisteen 
Seuraavat JSON koodikatkelman määrittää Azure koneen Learning linkitetty-palvelu, joka osoittaa koulutus-web-palvelun oletusarvoinen päätepiste. 

    {   
        "name": "trainingEndpoint",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
                "apiKey": "myKey"
            }
        }
    }

**Azure ML Studio**saat arvot **mlEndpoint** ja **apiKey**seuraavasti:

1. Valitse vasemmalla olevasta valikosta **VERKKOPALVELUIHIN** .
2. Valitse **koulutus verkkopalvelun** verkkopalvelut luettelossa. 
3. Valitse Kopioi **Ohjelmointirajapinnan avain** tekstiruudun vieressä. Liitä Leikepöydän avaimen tietojen Factory JSON-editorin.
4. Napsauta **Azure ML studio** **ERÄKÄSITTELY** -linkkiä.
5. **Pyydä URI** kopioi **pyytää** -osan ja liitä tiedot Factory JSON-editorin.   


#### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Azure ML päivitettäviä tulosmalli päätepisteen linkitetyn palvelu:
Seuraavat JSON koodikatkelman määrittää Azure koneen Learning linkitetty-palvelu, joka osoittaa tulosmalli WWW-palvelun päätepisteelle muun kuin oletusarvoisen päivitettäviä.  

    {
        "name": "updatableScoringEndpoint2",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
                "apiKey": "endpoint2Key",
                "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
            }
        }
    }


Ennen luominen ja käyttöönotto Azure-ML linkitetty palvelu, luo toinen (muun kuin oletusarvoisen ja päivitettäviä) päätepiste tulosmalli WWW-palvelun [Luominen päätepisteet](../machine-learning/machine-learning-create-endpoint.md) artikkelin ohjeiden mukaisesti.

Kun olet luonut muun kuin oletusarvoisen päivitettäviä päätepisteen, toimi seuraavasti:

- Valitse **ERÄKÄSITTELY** URI-arvon hakeminen **mlEndpoint** JSON-ominaisuutta.
- Valitse **Päivityksen RESURSSIN** linkki URI-arvon hakeminen **updateResourceEndpoint** JSON-ominaisuus. Ohjelmointirajapinnan avain on päätepiste-sivulla itse (oikeassa alakulmassa). 

![päivitettäviä päätepiste](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

 
#### <a name="placeholder-output-dataset"></a>Paikkamerkin tulosteen tietojoukko:
Azure ML päivityksen resurssin tehtävän Luo tuloksen. Azure Data Factory vaatii kuitenkin tulostus-tietojoukko levyasemassa putkijohto aikataulu. Tämän vuoksi Käytämme nuken/paikkamerkin tietojoukko tässä esimerkissä.  

    {
        "name": "placeholderBlob",
        "properties": {
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "any",
                "format": {
                    "type": "TextFormat"
                }
            }
        }
    }


#### <a name="pipeline"></a>Myyntijakso
Putkijohto on kaksi toiminnot: **AzureMLBatchExecution** ja **AzureMLUpdateResource**. Azure ML eräkäsittely tehtävän otetaan syötteeksi koulutus-tiedot ja tuottaa tulos iLearner-muodossa. Tehtävän käynnistää koulutus web-palvelu (koulutus kokeen web-palveluna) syötteen koulutus tiedot ja niistä ilearner tiedoston verkkopalvelu. PlaceholderBlob on juuri, joita tarvitaan Azure Data Factory-palvelu suorittaa putkijohto tyhjä tulos tietojoukko. 

![myyntijakso-kaavio](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


    {
        "name": "pipeline",
        "properties": {
            "activities": [
                {
                    "name": "retraining",
                    "type": "AzureMLBatchExecution",
                    "inputs": [
                        {
                            "name": "trainingData"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "typeProperties": {
                        "webServiceInput": "trainingData",
                        "webServiceOutputs": {
                            "output1": "trainedModelBlob"
                        }              
                     },
                    "linkedServiceName": "trainingEndpoint",
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "02:00:00"
                    }
                },
                {
                    "type": "AzureMLUpdateResource",
                    "typeProperties": {
                        "trainedModelName": "Training Exp for ADF ML [trained model]",
                        "trainedModelDatasetName" :  "trainedModelBlob"
                    },
                    "inputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "placeholderBlob"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "name": "AzureML Update Resource",
                    "linkedServiceName": "updatableScoringEndpoint2"
                }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }


### <a name="reader-and-writer-modules"></a>Lukija ja kirjoittajan moduulit

Ohjatun Web-palvelun parametrien käytetty vaihtoehto on Azure SQL-lukijat ja kirjoittajat. Lukija-moduulin käytetään ladata tietoja kokeessa tietojen hallinta-palveluiden Azure koneen Learning Studio ulkopuolella. Kirjoittaja-moduuli on tallentamaan tietoja oman kokeissa tietojen hallintapalvelut Azure koneen Learning Studio ulkopuolella.  

Lisätietoja SQL Azure-Blob-objektien ja Azure reader/writer on ohjeaiheissa [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) ja [kirjoittajan](https://msdn.microsoft.com/library/azure/dn905984.aspx) MSDN-kirjastossa. Azure-Blob-lukuohjelman ja Azure-Blob writer käytetään Esimerkki edellisessä osassa. Tässä osassa käsitellään Azure SQL-lukuohjelman ja Azure SQL-writer avulla.


## <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

**Q:** Minulla on useita tiedostoja, jotka on luotu big datasta-putkistot. Kaikki tiedostoja AzureMLBatchExecution-toiminnon avulla?

**A:** Kyllä. Lisätietoja tiedot **lukija-moduulin voi lukea tietoja useiden tiedostojen Azure-Blob** -osiossa. 

## <a name="azure-ml-batch-scoring-activity"></a>Azure ML erä näkyvissä pistemäärä tehtävä
Jos käytät **AzureMLBatchScoring** tehtävän voi integroida Azure koneen Learning, on suositeltavaa, että käytät uusinta **AzureMLBatchExecution** tehtävän. 

AzureMLBatchExecution tehtävä on otettu käyttöön Azure SDK ja PowerShellin Azure elokuussa 2015-version.

Voit halutessasi jatkaa AzureMLBatchScoring tehtävän Jatka lukemista – tässä osassa.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure ML erä näkyvissä pistemäärä tehtävän käyttämällä i/Azuren tallennustilaan 

    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchScoring",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "ScoringInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "ScoringResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

### <a name="web-service-parameters"></a>WWW-palveluparametrit
Voit määrittää WWW-palvelun parametrien arvot, lisäämällä **typeProperties** osan myyntijakso JSON, kuten seuraavassa esimerkissä **AzureMLBatchScoringActivty** kohtaan: 

    "typeProperties": {
        "webServiceParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }


Voit käyttää myös [Tietojen Factory Funktiot](https://msdn.microsoft.com/library/dn835056.aspx) -kulkeva arvot WWW-palvelun parametrien seuraavan esimerkin mukaisesti:

    "typeProperties": {
        "webServiceParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] WWW-palvelun parametrien kirjainkoko on merkitsevä, joten varmista, että määrität tehtävän nimet JSON samat WWW-palvelun tarjoamista. 

## <a name="see-also"></a>Katso myös

- [Azure blogimerkinnässä: Azure Data Factory ja Azure koneen Learning käytön aloittaminen](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)





[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/


 
