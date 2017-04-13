<properties 
    pageTitle="Azure koneen Learning päätepisteet määrittämisestä Stream Analytics | Microsoft Azure" 
    description="Tietokoneen kielen käyttäjän määrittämät funktiot, Stream Analytics"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"
/>

# <a name="machine-learning-integration-in-stream-analytics"></a>Koneen Stream Analytics integration oppiminen

Virta Analytics tukee käyttäjän määrittämät funktiot, jotka Azure koneen Learning päätepisteet vastaanottaja. REST API tuki tämä ominaisuus on yksityiskohtainen [Stream Analytics REST API-kirjasto](https://msdn.microsoft.com/library/azure/dn835031.aspx). Tässä artikkelissa lisätietoa onnistuneen soveltaminen Stream Analytics tätä ominaisuutta ei tarvittu. Opetusohjelman on myös kirjattu ja voivat käyttää [tätä](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Yleistä: Azure koneen Learning sanasto

Microsoft Azure koneen Learning tarjoaa voit tehdä muodosta, Testaa ja ota käyttöön ennakoivan analytics ratkaisujen tietojen yhteiskäytön, vedä ja pudota-työkalu. Tämä työkalu kutsutaan *Azure koneen Learning Studio*. Studio käytetään tietokoneen oppimisresurssit käsitellä helposti muodosta, Testaa ja suunnittelemasi käytöstä. Nämä resurssit ja määritelmät ovat jäljempänä.

- **Työtilan**: *työtilan* on säilö, joka sisältää kaikki muut tietokoneen oppimisresurssit yhdessä säilössä, hallinta ja ohjausobjektit.
- **Kokeile**: *kokeissa* luotuja tietoja tutkijoiden kouluttaminen koneen learning mallin ja hyödyntää tietojoukkoja.
- **Päätepisteen**: *päätepisteet* ovat Azure Konepohjaisten Oppimistekniikoiden objekti, jota käytetään kestää ominaisuuksien syötteenä, käytä määritettyä koneen learning mallin ja palauttaa tulos.
- **Näkyvissä pistemäärä verkkopalvelu**: *näkyvissä pistemäärä verkkopalvelu* on päätepisteet kokoelma, kuten edellä mainittiin.

Kukin päätepiste on ohjelmointirajapinnan eräkäsittely ja painikkeen suorittamisen. Virta Analytics käyttää synkronisen suorittamisen. Tietyn palvelun nimi on [Pyynnön ja vastauksen palvelun](../machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs) AzureML Studiossa.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Konepohjaisten Oppimistekniikoiden Stream Analytics työt tarvittavat resurssit

Virta Analytics tarkoitetaan työn käsittely, pyynnön ja vastauksen päätepisteen, [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md#get-an-azure-machine-learning-authorization-key)ja swagger määritelmä ovat kaikki tarvittavat onnistuneen suorittamisen. Virta Analytics on muita päätepiste, joka muodostaa swagger päätepisteen URL-osoite, hakee käyttöliittymä ja palauttaa oletusarvon UDF määritelmän käyttäjälle.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Määritä Stream analyysin ja Konepohjaisten Oppimistekniikoiden UDF REST API kautta

REST API avulla voit määrittää työtäsi Soita Azure Machine-kielen funktiot. Vaiheet ovat seuraavat:

1. Luo Stream Analytics-työ
2. Määritä syöte
3. Määritä tulos
4. Käyttäjän määrittämien funktioiden (UDF) luominen
5. Kirjoita, joka UDF kutsuu Stream Analytics-muunnos
6. Aloita työ

## <a name="creating-a-udf-with-basic-properties"></a>Luomalla UDF perusominaisuudet

Esimerkiksi seuraava näyte koodi luo nimeltä *newudf* , joka sitoo Azure koneen Learning päätepisteen skalaarinen UDF. Huomaa, että *päätepisteen* (URI-palvelu) löytyy API Ohjesivun valitsemasi palvelun ja *apiKey* löytyy palveluiden pääsivulta.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Esimerkki pyynnön leipäteksti:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Puhelun RetrieveDefaultDefinition päätepisteen default UDF

Kun rakenne UDF on luotu UDF valmis määritys tarvita. RetreiveDefaultDefinition päätepisteen avulla saat skalaarinen-funktion, joka on sidottu Azure koneen Learning päätepisteen oletusarvo-määritys. Alla paketti edellyttää, että saat skalaarinen-funktion, joka on sidottu Azure koneen Learning päätepisteen oletusarvon UDF-määritys. Se ei määritä todellinen päätepisteen, koska se on jo annettu HYLLYTETTY pyynnön aikana. Virta Analytics puhelujen päätepisteen säädetty pyynnön, jos se on annettu erikseen. Muussa tapauksessa se käyttää alun perin Viitattu yksi. Tässä yhden merkkijonon parametrin (lauseen) ja palauttaa yhden tulosteen tyypin UDF on merkkijono, joka ilmaisee, että lause "markkinatunnelma" otsikko.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Esimerkki pyynnön leipäteksti:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Esimerkki tulosteen tämän etsimällä jotain haluaisit alapuolella.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-the-response"></a>Korjaustiedoston UDF vastauksen mukana 

Nyt UDF täytyy olla asennettu edellisen vastausta alla kuvatulla tavalla.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Pyyntö leipäteksti (RetrieveDefaultDefinition tulosteen):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Toteuta Soita UDF Stream Analytics-muunnos

Nyt kyselyä (jonka nimi tässä scoreTweet) UDF syötteen jokaisen tapahtuman ja kirjoita vastaus kyseisen tapahtuman tulos.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
