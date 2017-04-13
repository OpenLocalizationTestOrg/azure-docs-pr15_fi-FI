<properties
   pageTitle="Hallitse Azure tietojen järvi Analytics Azure SDK käytön Node.js | Azure"
   description="Opi hallitsemaan tietoja järvi Analytics-tilit, tietolähteitä, töitä ja Azure SDK käytön Node.js käyttäjät"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Azure tietojen järvi Analytics Azure SDK käytön Node.js hallinta


[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure-SDK Node.js voi käyttää Azure tietojen järvi Analytics tilit, työt ja luetteloiden hallinta. Saat hallinta aiheen käyttää muita työkaluja valitsemalla välilehti Valitse yllä.

Oikealle, kun se tukee:

  *  **Node.js-versio: 0.10.0 tai uudempi versio**
  *  **Tilin REST API-versio: 2015-10-01-esikatselu**
  *  **Luettelon REST API-versio: 2015-10-01-esikatselu**
  *  **Työn REST API-versio: 2016-03-20-esikatselu**

## <a name="features"></a>Ominaisuudet

- Tilin hallinta: Luo, Hae, luettelon, päivittäminen ja poistaminen.
- Projektin hallinta: lähettämään Hae luettelon, peruuttaa.
- Luettelon hallinta: Hae, luettelon, Luo (tietoja), Päivitä (tietoja) ja poista (tietoja).

## <a name="how-to-install"></a>Asentaminen

```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Todentaa Azure Active Directoryn avulla

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-analytics-client"></a>Luo tietojen järvi Analytics-asiakas

```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Tietoja järvi Analytics-tilin luominen

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-jobs"></a>Projektien luettelo

```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-the-data-lake-analytics-catalog"></a>Saat luettelon käytettävissä tietokantojen järvi Analytics tietoluetteloon
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Katso myös

- [Microsoft Azure SDK Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK Node.js - järvi tietovaraston hallinta](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)
