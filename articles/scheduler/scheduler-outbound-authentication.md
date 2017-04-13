<properties
 pageTitle="Ajoituksen lähtevien yhteyksien todennusta"
 description="Ajoituksen lähtevien yhteyksien todennusta"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/15/2016"
 ms.author="deli"/>

# <a name="scheduler-outbound-authentication"></a>Ajoituksen lähtevien yhteyksien todennusta

Ajoituksen työt täytyy mahdollisesti services todennusta edellyttävien vastaanottaja. Tällä tavalla kutsutaan palvelun selviää Jos ajoitus-työ voi käyttää sen resursseja. Jotkin näistä palveluista kattavat muiden Azure services, Salesforce.com, Facebook ja suojattujen mukautettujen sivustojen.

## <a name="adding-and-removing-authentication"></a>Lisääminen ja käyttöoikeuksien poistaminen

Lisääminen ajoituksen työn todennus on yksinkertainen – Lisää JSON alielementti `authentication` , `request` luotaessa tai päivitettäessä työn elementti. Tietoja välitetään HYLLYTETTY, KORJAUSTIEDOSTON tai POST-pyynnössä – ajoituspalvelu osana `authentication` objektin – palautetaan koskaan vastaukset. Vastaukset salainen tiedot on määritetty null tai voi olla julkisen tunnus, joka edustaa todennetut kohteen.

Jos haluat poistaa käyttöoikeuden, SIJOITTAA tai KORJAUSTIEDOSTON työn erikseen määrittäminen `authentication` objektin Null. Et näe takaisin vastauksena todennus ominaisuuksia.

Tällä hetkellä vain todentamistyyppiä ovat `ClientCertificate` (käyttämiseen SSL/TLS-asiakasohjelman varmenteet)-malli `Basic` malli (for perustodentamista) ja `ActiveDirectoryOAuth` malli (todennustavaksi Active Directory-OAuth.)

## <a name="request-body-for-clientcertificate-authentication"></a>Pyydä ClientCertificate todennustavaksi tekstissä

Kun lisäät todennus `ClientCertificate` malli, määrittää seuraavat muita elementtejä pyynnön tekstissä.  

|Osa|Kuvaus|
|:---|:---|
|_todennus (pääelementti)_|Käyttämällä asiakkaan SSL-varmenteen todennus-objekti.|
|_tyyppi_|Pakollinen. Todennus tyyppi. SSL-asiakasohjelman varmenteet-arvon on oltava `ClientCertificate`.|
|_PFX_|Pakollinen. Base64-koodattu PFX-tiedoston sisällön.|
|_salasana_|Pakollinen. Salasana PFX-tiedostoa.|


## <a name="response-body-for-clientcertificate-authentication"></a>Vastauksen leipätekstin ClientCertificate todennusta varten

Kun todennusta tiedot lähetetään pyyntö, vastaus on seuraavat todennus liittyvät osat.

|Osa |Kuvaus |
|:--|:--|
|_todennus (pääelementti)_ |Ohjatun asiakkaan SSL-varmenteen todentaminen objekti.|
|_tyyppi_ |Todennus tyyppi. SSL-asiakasohjelman varmenteet-arvo on `ClientCertificate`.|
|_certificateThumbprint_ |Varmenteen allekirjoitus.|
|_certificateSubjectName_ |Varmenteen DN nimi.|
|_certificateExpiration_ |Varmenteen voimassaolon.|

## <a name="sample-rest-request-for-clientcertificate-authentication"></a>Esimerkki REST-pyynnöstä ClientCertificate todennusta varten

```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a>Esimerkki muiden vastauksen ClientCertificate todennusta varten

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a>Pyyntö Perustodentamista tekstissä

Kun lisäät todennus `Basic` malli, määrittää seuraavat muita elementtejä pyynnön tekstissä.

|Osa|Kuvaus|
|:--|:--|
|_todennus (pääelementti)_ |Todennus-objektin käyttämällä perustodentamista.|
|_tyyppi_ |Pakollinen. Todennus tyyppi. Tavallinen todennustavaksi arvon on oltava `Basic`.|
|_käyttäjänimi_ |Pakollinen. Käyttäjänimi tarkistamiseen.|
|_salasana_ |Pakollinen. Salasanan todennusta.|

## <a name="response-body-for-basic-authentication"></a>Vastauksen leipätekstin Perustodentamista varten

Kun todennusta tiedot lähetetään pyyntö, vastaus sisältää seuraavat todennusta liittyvät osat.

|Osa|Kuvaus|
|:--|:--|
|_todennus (pääelementti)_ |Todennus-objektin käyttämällä perustodentamista.|
|_tyyppi_ |Todennus tyyppi. Perustodentamista-arvo on `Basic`.|
|_käyttäjänimi_ |Todennettu käyttäjänimi.|

## <a name="sample-rest-request-for-basic-authentication"></a>Perustodentamista Esimerkki REST-pyynnöstä

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a>Esimerkki REST-vastauksen Perustodentamista

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a>Pyydä leipätekstin ActiveDirectoryOAuth todennusta varten

Kun lisäät todennus `ActiveDirectoryOAuth` malli, määrittää seuraavat muita elementtejä pyynnön tekstissä.

|Osa |Kuvaus |
|:--|:--|
|_todennus (pääelementti)_ |Todennus-objektin ActiveDirectoryOAuth todennusta.|
|_tyyppi_ |Pakollinen. Todennus tyyppi. ActiveDirectoryOAuth todennustavaksi arvon on oltava `ActiveDirectoryOAuth`.|
|_vuokraajan_ |Pakollinen. Azure AD-vuokraajan vuokraajan tunnus.|
|_käyttäjäryhmälle_ |Pakollinen. Tämä on määritetty https://management.core.windows.net/.|
|_clientId_ |Pakollinen. Anna asiakkaan tunnus Azure AD-sovelluksen.|
|_salainen_ |Pakollinen. Asiakas, joka pyytää tunnuksen salainen.|

### <a name="determining-your-tenant-identifier"></a>Sen selvittäminen vuokraajan tunnus

Voit etsiä vuokraajan tunnus Azure AD-vuokraajan suorittamalla `Get-AzureAccount` Azure powershellissä.

## <a name="response-body-for-activedirectoryoauth-authentication"></a>Vastauksen leipätekstin ActiveDirectoryOAuth todennusta varten

Kun todennusta tiedot lähetetään pyyntö, vastaus on seuraavat todennus liittyvät osat.

|Osa |Kuvaus |
|:--|:--|
|_todennus (pääelementti)_ |Todennus-objektin ActiveDirectoryOAuth todennusta.|
|_tyyppi_ |Todennus tyyppi. ActiveDirectoryOAuth todennustavaksi arvo on `ActiveDirectoryOAuth`.|
|_vuokraajan_ |Azure AD-vuokraajan vuokraajan tunnus. |
|_käyttäjäryhmälle_ |Tämä on määritetty https://management.core.windows.net/.|
|_clientId_ |Azure AD-sovelluksen asiakkaan tunnus.|

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a>Esimerkki REST-pyynnöstä ActiveDirectoryOAuth todennustavaksi

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a>Esimerkki REST-vastauksen ActiveDirectoryOAuth todennustavaksi

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a>Katso myös


 [Ajoituksen ominaisuudet](scheduler-intro.md)

 [Azure ajoituksen käsitteitä ja termejä kohteen hierarkia](scheduler-concepts-terms.md)

 [Aloita käyttö ajoituksen Azure-portaalissa](scheduler-get-started-portal.md)

 [Suunnitelmien ja Azure ajoituksen Laskutus](scheduler-plans-billing.md)

 [Azure ajoituksen REST API-viittaus](https://msdn.microsoft.com/library/mt629143)

 [Azure ajoituksen PowerShell cmdlet-viittaus](scheduler-powershell-reference.md)

 [Azure ajoituksen suuren käytettävyyden ja luotettavuutta](scheduler-high-availability-reliability.md)

 [Azure ajoituksen rajoitukset, oletusarvot ja virhekoodit](scheduler-limits-defaults-errors.md)
