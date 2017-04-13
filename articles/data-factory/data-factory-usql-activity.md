<properties 
    pageTitle="U-SQL-komentosarja suoritetaan Azure tietojen järvi Analytics Azure Data Factory" 
    description="Lue, miten voit käsitellä tietoja suorittamalla U-SQL-komentosarjat Azure tietojen järvi Analytics Laske-palvelusta." 
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
    ms.date="09/12/2016" 
    ms.author="spelluru"/>

# <a name="run-u-sql-script-on-azure-data-lake-analytics-from-azure-data-factory"></a>U-SQL-komentosarja suoritetaan Azure tietojen järvi Analytics Azure Data Factory
> [AZURE.SELECTOR]
[Rakenne](data-factory-hive-activity.md)  
[Possu](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Tallennettu toimintosarja](data-factory-stored-proc-activity.md)
[Tietojen järvi Analytics U-SQL](data-factory-usql-activity.md)
[.NET mukautettu](data-factory-use-custom-activities.md)
 
Myyntijakso-Azure tietojen factory käsittelee tietojen linkitetyn tallennustilan Services-palveluissa linkitetyn Laske-palvelujen avulla. Se sisältää toimintoja, joissa kunkin tehtävän suorittaa tiettyjä käsittely järjestyksessä. Tässä artikkelissa on **Tietoja järvi Analytics U SQL tehtävän** , joka suoritetaan **U-SQL** -komentosarja **Azure tietojen järvi Analytics** -linkitetty Laske-palvelusta. 

> [AZURE.NOTE] 
> Luo Azure tietojen järvi Analytics-tili, ennen kuin luot putkijohto tietojen järvi Analytics U-SQL-aktiviteettiin. Lisätietoja Azure tietojen järvi Analytics on artikkelissa [Azure tietojen järvi Analytics käytön aloittaminen](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
>  
> Tarkista [luominen ensimmäisen myyntijakso-opetusohjelma](data-factory-build-your-first-pipeline.md) yksityiskohtaiset ohjeet tietojen factory, linkitetyn services, tietojoukkoja ja putkijohto luomiseen. Tietoja Factory editorin tai Visual Studio PowerShellin Azure JSON katkelmat avulla voit luoda Data Factory kohteita.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure tietojen järvi Analytics linkitetty palvelu
Voit luoda linkitetyn **Azure tietojen järvi Analytics** -palvelun linkki Azure tietojen järvi Analytics-Laske palvelun Azure tietojen factory. Putkijohto tietojen järvi Analytics U-SQL-toimintojen viittaa linkitetyn käyttöönsä. 

Seuraavassa esimerkissä on linkitetty Azure tietojen järvi Analytics-palvelun JSON-määritys. 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


Seuraavassa taulukossa on käytetty JSON määrityksessä ominaisuuksien kuvaukset. 

Ominaisuus | Kuvaus | Pakollinen
-------- | ----------- | --------
Tyyppi | Tyyppi-ominaisuuden arvoksi: **AzureDataLakeAnalytics**. | Kyllä
accountName | Azure tietojen järvi Analytics tilin nimi. | Kyllä
dataLakeAnalyticsUri | Azure tietojen järvi Analytics URI. |  Ei 
todennus | Luvan koodi haetaan automaattisesti, kun **Hyväksy** -painike tietojen Factory editorin ja Viimeistellään OAuth-kirjautuminen.  | Kyllä 
subscriptionId | Azure tilauksen tunnus | Ei (Jos ei määritetä, tietojen factory tilauksen käytetään). 
resourceGroupName | Azure resurssiryhmän nimi |  Ei (Jos ei määritetä, resurssiryhmä, tietojen factory käytetään).
Istunto | istunnon tunnus OAuth luvan istunnosta. Kunkin istunnon tunnus on yksilöllinen ja voi käyttää vain kerran. Istunnon tunnus on luotu tietojen Factory Kyselyeditorissa. | Kyllä

**Hyväksy** -painikkeen avulla luotu luvan koodin vanhenee kuluttua. Seuraavassa taulukossa saat varten käyttäjätilejä erityyppisiä vanheneminen-aikoja. Näkyviin voi tulla seuraava virhe yhteydessä todennus- **tunniste vanhentuu**: tunnistetietojen toiminto-virhe: invalid_grant - AADSTS70002: Virhe tarkistettaessa tunnistetiedot. AADSTS70008: Annettu access-myöntäminen vanhentunut tai kumottu. Jäljitys-tunnus: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelaatiotunnus: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 aikaleima: 2015 – 12-15 21:09:31Z

 
| Käyttäjätyyppi | Vanhentuu |
| :-------- | :----------- | 
| Azure Active Directory ei hallita käyttäjätilejä (@hotmail.com, @live.com, jne.) | 12 tuntia |
| Käyttäjien tilejä, joiden hallinnassa mukaan Azure Active Directory (AAD) | 14 päivän viimeinen sektoria Suorita. <br/><br/>90 päivää, OAuth-pohjaisen linkitetyn palvelun perusteella sektoria käytetään vähintään 14 päivän välein. |

Voit välttää ja ratkaista tämän virheen, käyttämällä **Hyväksy** reauthorize painikkeen milloin **tunniste vanhentuu** ja laukaisun linkitetyn palvelu. Voit myös luoda ohjelmallisesti käyttämällä seuraavassa osassa **istunto** ja **käyttöoikeuksien** ominaisuuksien arvot. 

  
### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>Voit luoda ohjelmallisesti istunto ja luvan arvot 

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

Näkyviin [AzureDataLakeStoreLinkedService luokan](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx) [AzureDataLakeAnalyticsLinkedService luokan](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)ja [AuthorizationSessionGetResponse luokan](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) käyttää koodissa Data Factory luokkien tietoja. Lisää viite: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll WindowsFormsWebAuthenticationDialog luokan. 
 
 
## <a name="data-lake-analytics-u-sql-activity"></a>Tietoja järvi Analytics U-SQL-tehtävä 

Seuraavat JSON koodikatkelman määrittää tietojen järvi Analytics U-SQL-aktiviteettiin putkijohto. Tehtävän määritelmä on viittaus aiemmin luomasi linkitetty Azure tietojen järvi Analytics-palveluun.   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


Seuraavassa taulukossa on kuvattu nimistä ja kuvauksista ominaisuuksia, jotka ovat tämän tehtävän. 

Ominaisuus | Kuvaus | Pakollinen
:-------- | :----------- | :--------
tyyppi | Type-ominaisuus on määritettävä **DataLakeAnalyticsU**SQL. | Kyllä
scriptPath | U-SQL-komentosarja sisältävän kansion polku. Tiedoston nimen kirjainkoko on merkitsevä. | Ei (Jos käytät komentosarjaa)
scriptLinkedService | Linkitetyn palvelu, joka linkittää tallennustilaa, joka sisältää tiedot factory komentosarja | Ei (Jos käytät komentosarjaa)
komentosarja | Määritä sisäisen komentosarjan scriptPath ja scriptLinkedService sijaan. Esimerkiksi: "komentosarjan": "Luo tietokanta testi". | Ei (Jos käytät scriptPath ja scriptLinkedService)
degreeOfParallelism | Käytettävä samanaikaisesti suoritetaan solmujen enimmäismäärä. | Ei
Priority (prioriteetti) | Määrittää, mitkä työt ulos kaikki, johon on lisätty pitäisi olla valittuna suorittaa ensimmäisen. Pienempi luku, suuremman prioriteetti. | Ei 
Parametrit | Parametrien U SQL-komentosarja | Ei 

Katso [SearchLogProcessing.txt komentosarjan määritelmä](#script-definition) komentosarja-määrityksen. 

## <a name="sample-input-and-output-datasets"></a>Esimerkki syötteen ja tulosteen tietojoukkoja

### <a name="input-dataset"></a>Kirjoita tietojoukko
Tässä esimerkissä syötteen tiedot sijaitsevat Azure tietojen järvi säilössä (SearchLog.tsv tiedoston datalake/input-kansioon). 

    {
        "name": "DataLakeTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

### <a name="output-dataset"></a>Tulostus-tietojoukko
Tässä esimerkissä U SQL-komentosarja tuottamat tiedot tallennetaan Azure tietojen järvi säilössä (datalake/tulostus-kansio). 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="sample-data-lake-store-linked-service"></a>Esimerkki järvi tietovaraston linkitetty palvelu
Seuraavassa on Azure järvi tietovaraston linkitetty i/tietojoukkoja käyttämä otosten määritys. 

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

Katso [Siirrä tiedot ja Azure tietojen järvi kaupasta](data-factory-azure-datalake-connector.md) artikkelissa JSON ominaisuuksien kuvaukset. 

## <a name="sample-u-sql-script"></a>Esimerkki U-SQL-komentosarja 

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv(nullEscape:"#NULL#");
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start <= DateTime.Parse("2012/02/19");
    
    OUTPUT @rs1   
        TO @out
          USING Outputters.Tsv(quoting:false, dateTimeFormat:null);

Arvot **@in** ja **@out** parametrien U SQL-komentosarja on lähettämän dynaamisesti SYÖTTÖ-parametrit-osan avulla. Myyntijakso-määrityksessä on "parametrit-kohdassa.

Voit määrittää muita ominaisuuksia, kuten degreeOfParallelism ja prioriteetin sekä myyntijakso-määrityksessä projekteille, jotka suoritetaan Azure tietojen järvi Analytics-palvelusta.

## <a name="dynamic-parameters"></a>Dynaaminen parametrit
Esimerkki myyntijakso-määritys ja Loitonna parametrit on määritetty koodattu arvoilla. 

    "parameters": {
        "in": "/datalake/input/SearchLog.tsv",
        "out": "/datalake/output/Result.tsv"
    }

Ei käytä dynaaminen parametrit sijaan. Esimerkki: 

    "parameters": {
        "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
        "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
    }

Tässä tapauksessa syötteen tiedostot ovat edelleen noudettu /datalake/input-kansiosta ja tiedostot muodostetaan /datalake/output-kansiossa. Tiedostojen nimet ovat dynaamisia sektoria alkamisajan perusteella.  