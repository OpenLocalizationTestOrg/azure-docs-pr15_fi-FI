<properties 
   pageTitle="Pääset alkuun käyttämällä REST API tietojen järvi Analytics | Microsoft Azure" 
   description="Suorita tietojen järvi Analytics WebHDFS REST API avulla" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Azure tietojen järvi Analytics käyttämällä REST API käytön aloittaminen

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Opettele käyttämään tietoja järvi Analytics-tilit, työt ja luettelon WebHDFS REST API ja tietojen järvi Analytics REST API. 

## <a name="prerequisites"></a>Edellytykset

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

- **Azure Active Directory-sovelluksen luominen**. Azure AD-sovelluksen avulla voit todentaa Azure AD tietojen järvi Analytics-sovellukseen. Useilla eri tavoilla Azure AD-todentamismenetelmä on **käyttäjän todennusta** tai **palvelu todennusta**. Ohjeita ja lisätietoja tarkistamiseen on artikkelissa [Azure Active Directoryn avulla tietojen järvi Analytics todentamismenetelmä](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

- [cURL](http://curl.haxx.se/). Tässä artikkelissa käytetään kääntö miten REST API-puheluissa vastaan tietojen järvi Analytics-tili.

## <a name="authenticate-with-azure-active-directory"></a>Azure Active Directory todentamismenetelmä

Azure Active Directory-hakemistosta todennustapa kahdella eri tavalla.

### <a name="end-user-authentication-interactive"></a>Käyttäjän todennusta (vuorovaikutteinen)

Tällä menetelmällä sovelluksen käyttäjää kirjautumaan ja kaikki toiminnot suoritetaan käyttäjän kontekstissa. 

Vuorovaikutteinen todennusta varten seuraavasti:

1. Sovelluksen kautta uudelleenohjata käyttäjän seuraava URL-osoite:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<REDIRECT URI > on koodattu URL-käyttöä varten. Tällöin https://localhost, käytä `https%3A%2F%2Flocalhost`)

    Tässä opetusohjelmassa varten URL-Osoitteen paikkamerkki-arvojen korvaaminen ja liitä se WWW-selaimen osoiterivillä. Sinut ohjataan todennuksen Azure kirjautumisnimi. Kun olet onnistuneesti kirjautuvat, vastaus näkyy selaimen osoiteriville. Vastaus on seuraavanlainen:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Sieppaa vastauksen luvan koodin. Tässä opetusohjelmassa, voit kopioida luvan koodin selaimen osoiterivillä ja välittää sen JULKAISE-pyynnössä suojaustunnuksen päätepisteen alla kuvatulla tavalla:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] Tässä tapauksessa \<UUDELLEENOHJAUS URI > ei tarvitse koodattu.

3. Vastaus on JSON-objekti, joka sisältää access-tunnus (esimerkiksi `"access_token": "<ACCESS_TOKEN>"`) ja Päivitä-tunnus (esimerkiksi `"refresh_token": "<REFRESH_TOKEN>"`). Sovelluksen käyttää access-tunnuksen käytettäessä Azure tietojen järvi Tallenna ja Päivitä-tunnuksen hankkiminen toiseen käyttöoikeustietue, kun access-tunniste vanhentuu.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Kun access-tunnuksen vanhenee, voit pyytää uuden käyttöoikeustietue, Päivitä-tunnuksen käyttämällä alla kuvatulla tavalla:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Katso lisätietoja vuorovaikutteinen käyttöoikeuksista, [lupa koodin myöntää työnkulku](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Palvelun todennus (ei ole vuorovaikutteinen)

Tällä menetelmällä sovellus on oma tunnistetiedot toimintojen suorittaminen. Tämän ominaisuuden voit myöntää kirjaa pyyntö alla kuvatun kaltaisia: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Pyyntö tulos sisältää todennus-tunnuksen (merkitty `access-token` tuloksissa alla), voit myöhemmin siirtää REST API soitettaessa. Todennus-tunnuksen tallennetaan tekstitiedostoon; tarvitset tätä tämän artikkelin.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Tässä artikkelissa käyttää **ei ole vuorovaikutteinen** lähestymistapa. Lisätietoja ei ole vuorovaikutteinen (palvelu puhelut) on artikkelissa [palvelun palvelun puhelut-tunnistetiedoilla](https://msdn.microsoft.com/library/azure/dn645543.aspx).
## <a name="create-a-data-lake-analytics-account"></a>Tietoja järvi Analytics-tilin luominen

Sinun on luotava Azure-resurssiryhmä ja järvi tietovaraston tilin, ennen kuin voit luoda tietojen järvi Analytics-tilin.  Katso [Luo järvi tietovaraston tili](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Seuraava kääntö-komento näkyy tilin luominen:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Korvaa \< `REDACTED` \> kanssa todennus-tunnuksen \< `AzureSubscriptionID` \> kanssa Tilaustunnus- \< `AzureResourceGroupName` \> Azure resurssiryhmä aiemmin nimellä, ja \< `NewAzureDataLakeAnalyticsAccountName` \> tietojen järvi Analytics tilin uudella nimellä. **CreateDatalakeAnalyticsAccountRequest.json** -tiedosto, joka on tarkoitettu sisältyy tämän komennon pyynnön-paketti `-d` parametrin yläpuolella. Input.json-tiedoston sisällön näyttää seuraavankaltaiselta:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Luettelon tietojen järvi Analytics-tilit-tilaus

Kääntö seuraava komento näyttää, miten luettelon tilit-tilauksen:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Korvaa \< `REDACTED` \> kanssa todennus-tunnuksen \< `AzureSubscriptionID` \> käyttämällä tilauksen. Tulos on:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Tietoja tietojen järvi Analytics-tili

Seuraava kääntö-komento näkyy hankkiminen tilin tiedot:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Korvaa \< `REDACTED` \> kanssa todennus-tunnuksen \< `AzureSubscriptionID` \> kanssa Tilaustunnus- \< `AzureResourceGroupName` \> Azure resurssiryhmä aiemmin nimellä, ja \< `DataLakeAnalyticsAccountName` \> aiemmin tietojen järvi Analytics tilin nimi. Tulos on:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Luettelon tietojen järvi tallentaa tiedot järvi Analytics tilin

Seuraava kääntö komento näyttää, miten luettelon tietojen järvi Stores-tilin:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Korvaa \< `REDACTED` \> kanssa todennus-tunnuksen \< `AzureSubscriptionID` \> kanssa Tilaustunnus- \< `AzureResourceGroupName` \> Azure resurssiryhmä aiemmin nimellä, ja \< `DataLakeAnalyticsAccountName` \> aiemmin tietojen järvi Analytics tilin nimi. Tulos on:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>Lähettää U-SQL-työt

Seuraava kääntö-komento näkyy lähettäminen U-SQL-työ:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Korvaa \< `REDACTED` \> kanssa todennus-tunnuksen \< `DataLakeAnalyticsAccountName` \> aiemmin tietojen järvi Analytics tilin nimi. **SubmitADLAJob.json** -tiedosto, joka on tarkoitettu sisältyy tämän komennon pyynnön-paketti `-d` parametrin yläpuolella. Input.json-tiedoston sisällön näyttää seuraavankaltaiselta:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

Tulos on:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>Luettelon U-SQL-projektit

Kääntö seuraava komento näyttää, miten luettelon U-SQL-työt:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Korvaa \< `REDACTED` \> kanssa todennus-tunnuksen ja \< `DataLakeAnalyticsAccountName` \> aiemmin tietojen järvi Analytics tilin nimi. 


Tulos on:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Hae luettelokohteet

Seuraava kääntö-komento näkyy hankkiminen tietokantojen luettelosta:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

Tulos on:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Katso myös

- Monimutkaisen kyselyn on ohjeartikkelissa [Analysoi sivuston lokit Azure tietojen järvi Analytics avulla](data-lake-analytics-analyze-weblogs.md).
- Aloita U-SQL-sovellusten kehittäminen-kohdasta [kehittää U-SQL-komentosarjat käyttäminen tietojen järvi Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL-kohdassa [Azure tietojen järvi Analytics U-SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md).
- Katso hallintatehtäviä, [hallita Azure tietojen järvi Analytics käyttämällä Azure portal](data-lake-analytics-manage-use-portal.md).
- Saat yleiskatsauksen tietojen järvi Analytics-artikkelissa [Azure tietojen järvi Analytics yleiskatsaus](data-lake-analytics-overview.md).
- Saat saman opetusohjelman muiden työkaluilla valitsemalla välilehden valitsimet-sivulla.
- Diagnostiikan lokitiedot on artikkelissa [Azure tietojen järvi Analytics käytetään diagnostiikka lokit](data-lake-analytics-diagnostic-logs.md)
