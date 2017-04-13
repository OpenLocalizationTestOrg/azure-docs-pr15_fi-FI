<properties
    pageTitle="Luoda tapahtuman keskittimet nimitilan tapahtumaa-toiminnossa ja ota käyttöön arkisto Azure Resurssienhallinta-mallin avulla | Microsoft Azure"
    description="Luoda tapahtuman keskittimet nimitilan tapahtumaa-toiminnossa ja ota käyttöön arkisto Azure Resurssienhallinta-mallin avulla"
    services="event-hubs"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="09/14/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-enable-archive-using-an-azure-resource-manager-template"></a>Luoda tapahtuman keskittimet nimitilan tapahtumaa-toiminnossa ja ota käyttöön arkisto Azure Resurssienhallinta-mallin avulla

Tässä artikkelissa kerrotaan, miten voit käyttää Azure Resurssienhallinta mallia, joka luo tapahtuman keskittimet nimitilan, jossa tapahtuma-toiminnosta ja mahdollistaa arkisto-tapahtumaa-toiminnossa. Kerrotaan, miten voit määrittää resursseja on otettu käyttöön ja määrittäminen parametrit, jotka ovat määritettyä kun käyttöönotto suoritetaan. Käytä tätä mallia oman ominaisuuksissa tai mukauttaa sen omaa ehtojen mukaan

Lisätietoja mallien luomisesta on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta mallit][].

Käytäntöjä Saat lisätietoja ja Azure resurssien nimeämiskäytännöt kuvioita on artikkelissa [Azure resurssien nimeäminen nimeämiskäytännön][].

Valmis malli on [tapahtumaa-toiminnossa ja ota arkisto-malli][] -GitHub.

>[AZURE.NOTE]
>Voit tarkistaa uusimman mallit-käy [Azure pikaopas][] mallivalikoimasta ja Etsi tapahtuman keskittimet.

## <a name="what-you-deploy"></a>Mitä otetaan käyttöön?

Tämän mallin käyttöön tapahtuman keskittimet-nimitilan, jonka tapahtumaa-toiminnossa ja ota käyttöön arkisto.

[Tapahtuman keskittimet](../event-hubs/event-hubs-what-is-event-hubs.md) on käsittelemiseen käytettävä tapahtuma- ja telemetriatietojen tunkeutumisen lainata Azure valtaviin tasolla pieni viive ja suuri luotettavuutta tapahtuma. Tapahtuman keskittimet arkisto avulla voit pitää automaattisesti streaming tiedot Azure-Blob-säiliö valittua tapahtuman oman keskittimiä määritetyllä aikavälillä tai koon aikavälin valitsemaasi sähköpostikansioon.

Voit suorittaa käyttöönoton automaattisesti valitsemalla Seuraava painike:

[![Ottaa käyttöön Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrit

Azure resurssien hallinnan avulla voit määrittää parametrien arvot, jotka haluat määrittää, kun malli otetaan käyttöön. Malli sisältää osan nimeltä `Parameters` , joka sisältää kaikki parametriarvot. Olisi määrittää kyseiset arvot, vaihtelevat otat projektin pohjalta tai otat käyttöön ympäristöön parametri. Määritä parametrien arvot, jotka pysyvät aina sama. Kunkin parametrin arvon käytetään mallia voit määrittää resurssit, jotka on otettu käyttöön.

Mallin määrittää seuraavat parametrit.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Voit luoda tapahtuman keskittimet nimitilan nimi.

```
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName

Tapahtuma-toiminnossa nimi luoda tapahtuman keskittimet nimitilan.

```
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the Event Hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays

Haluat säilyttää viestin tapahtumaa-toiminnossa päivien määrän. 

```
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in Event Hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount

Osioiden tapahtumaa-toiminnossa haluamasi määrä.

```
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="archiveenabled"></a>archiveEnabled

Ota käyttöön arkisto-tapahtumaa-toiminnossa.

```
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your Event Hub"
    }
 }
```
### <a name="archiveencodingformat"></a>archiveEncodingFormat

Koodauksen muotoilua, voit määrittää onnistu tapahtumatietoja.

```
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### <a name="archivetime"></a>archiveTime

Aikaväli, josta arkiston alkaa arkistointi Azure-blob-säiliö tiedot.

```
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### <a name="archivesize"></a>archiveSize

Koon aikaväli, josta arkisto alkaa arkistointi Azure-blob-säiliö tiedot.

```
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId

Arkisto edellyttää tallennustilan tilin resurssi-tunnus, jotta arkisto haluamasi Azure-tallennustilan.

```
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName

Tapahtuman tiedot haluamaasi blob-säilö arkistoida.

```
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Container that you want the blobs archived in"
    }
}
```


### <a name="apiversion"></a>apiVersion

Mallin API-versio.

```
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>Resurssien käyttöönotto

Luo tyypin **EventHubs**nimitilan, jossa tapahtuma-toiminnosta ja mahdollistaa arkisto.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }
                  
               }
               
            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>Komentojen suorittamiseen käyttöönotto

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShellin

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

[Authoring Azure Resurssienhallinta-mallit]: ../resource-group-authoring-templates.md
[Azure pikaopas mallit]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
[Azure resurssien nimeämiskäytännöt]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
[Tapahtuma-toiminnosta ja ota arkisto-malli]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive
