<properties
    pageTitle="Luoda tapahtuman keskittimet nimitilan tapahtumaa-toiminnossa ja kuluttajien ryhmän Azure Resurssienhallinta-mallin avulla | Microsoft Azure"
    description="Tapahtuman keskittimet nimitilan tapahtumaa-toiminnossa ja Azure Resurssienhallinta mallilla kuluttaja-ryhmän luominen"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/31/2016"
    ms.author="sethm;shvija"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Luoda tapahtuman keskittimet nimitilan tapahtumaa-toiminnossa ja kuluttajien ryhmän Azure Resurssienhallinta-mallin avulla

Tässä artikkelissa kerrotaan, miten voit käyttää Azure Resurssienhallinta mallia, joka luo tapahtuman keskittimet nimitilan tapahtumaa-toiminnossa ja kuluttaja-ryhmä. Opit, miten voit määrittää resursseja on otettu käyttöön ja määrittäminen parametrit, jotka ovat määritettyä kun käyttöönotto suoritetaan. Käytä tätä mallia oman ominaisuuksissa tai mukauttaa sen omaa ehtojen mukaan

Lisätietoja mallien luomisesta on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja][].

Valmis malli on [tapahtumaa-toiminnossa ja kuluttaja ryhmän malli][] -GitHub.

>[AZURE.NOTE]
>Voit tarkistaa uusimman mallit-käy [Azure pikaopas][] mallivalikoimasta ja Etsi tapahtuman keskittimet.

## <a name="what-will-you-deploy"></a>Mitä otetaan käyttöön?

Tämän mallin avulla otetaan käyttöön tapahtuman keskittimet-nimitilan, jonka tapahtumaa-toiminnossa ja kuluttaja-ryhmä.

[Tapahtuman keskittimet](../event-hubs/event-hubs-what-is-event-hubs.md) on käsittelemiseen käytettävä tapahtuma- ja telemetriatietojen tunkeutumisen lainata Azure valtaviin tasolla pieni viive ja suuri luotettavuutta tapahtuma.

Voit suorittaa käyttöönoton automaattisesti valitsemalla Seuraava painike:

[![Ottaa käyttöön Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrit

Azure resurssien hallinnan avulla voit määrittää parametrien arvot, jotka haluat määrittää, kun malli otetaan käyttöön. Malli sisältää osan nimeltä `Parameters` , joka sisältää kaikki parametriarvot. Olisi määrittää kyseiset arvot, vaihtelevat otat projektin pohjalta tai otat käyttöön ympäristöön parametri. Määritä parametrien arvot, jotka aina säilyy ennallaan. Kunkin parametriarvo käytetään mallin määrittäminen resurssit, jotka on otettu käyttöön.

Mallin määrittää seuraavat parametrit.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Voit luoda tapahtuman keskittimet nimitilan nimi.

```
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

Tapahtuma-toiminnossa nimi luoda tapahtuman keskittimet nimitilan.

```
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

Luo tapahtumaa-toiminnossa kuluttaja-ryhmän nimi.

```
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

Mallin API-versio.

```
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Resurssien käyttöönotto

Luo nimitilan tyypin **EventHubs**tapahtumaa-toiminnossa ja kuluttaja-ryhmä.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
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
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Komentojen suorittamiseen käyttöönotto

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShellin

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[Authoring Azure Resurssienhallinta-mallit]: ../resource-group-authoring-templates.md
[Azure pikaopas mallit]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Tapahtuman keskittimeen ja kuluttajien ryhmän malli]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
