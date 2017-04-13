<properties
    pageTitle="Luo palvelun Bus nimitilan Resurssienhallinta mallin avulla | Microsoft Azure"
    description="Luo palvelun Bus nimitilan Azure Resurssienhallinta-mallin avulla"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Luo palvelun Bus nimitilan Azure Resurssienhallinta-mallin avulla

Tässä artikkelissa käsitellään Azure Resurssienhallinta-mallin, joka luo palvelun Bus nimitilan tyypin **viestit** ja standardin/Basic-tuote. Artikkelin määrittää myös parametrit, jotka on määritetty käyttöönoton suorittamista varten. Voit käyttää tätä mallia oman käyttöönotoissa tai mukauttaa sen omaa ehtojen mukaan.

Lisätietoja mallien luomisesta on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja][].

Valmis mallin Katso [palvelun Bus nimitilan mallin][] GitHub.

>[AZURE.NOTE] Seuraavat Azure Resurssienhallinta-mallit ovat ladattavissa ja käyttöönotto. 
>
>-    [Luoda tapahtuman keskittimet nimitilan tapahtumaa-toiminnossa ja kuluttaja-ryhmän kanssa](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Luo palvelun Bus nimitilan jonossa](service-bus-resource-manager-namespace-queue.md)
>-    [Aihe ja tilauksen palvelun Bus nimitilan luominen](service-bus-resource-manager-namespace-topic.md)
>-    [Luo palvelun Bus nimitilan jonossa ja käyttöoikeuksien sääntöä](service-bus-resource-manager-namespace-auth-rule.md)
>
>Voit tarkistaa uusimman mallit-käy [Azure][] -pikaopas mallivalikoimasta ja Etsi palvelun Bus.

## <a name="what-will-you-deploy"></a>Mitä otetaan käyttöön?

Tämän mallin avulla otetaan käyttöön palvelun Bus nimitilan [Basic, vakio- tai maksullisten](https://azure.microsoft.com/pricing/details/service-bus/) tuote.

Voit suorittaa käyttöönoton automaattisesti valitsemalla Seuraava painike:

[![Ottaa käyttöön Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrit

Azure resurssien hallinnan avulla voit määrittää parametrien arvot, jotka haluat määrittää, kun malli otetaan käyttöön. Malli sisältää osan nimeltä `Parameters` , joka sisältää kaikki parametriarvot. Olisi määrittää kyseiset arvot, vaihtelevat otat projektin pohjalta tai otat käyttöön ympäristöön parametri. Määritä parametrien arvot, jotka aina säilyy ennallaan. Kunkin parametriarvo käytetään mallin määrittäminen resurssit, jotka on otettu käyttöön.

Tämä malli määrittää seuraavat parametrit.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Palvelun Bus nimitilan luominen nimi.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a>serviceBusSKU

Luo palvelun Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) nimi.

```
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

Mallin määrittää arvot, jotka ovat sallittuja parametrin (Basic, Standard tai Premium) ja määrittää oletusarvon (vakio), jos arvoa ei ole määritetty.

Saat lisätietoja palvelun Bus hinnat [palvelun Bus hinnat ja laskutus][].

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Mallin palvelun Bus API-versio.

```
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Resurssien käyttöönotto

### <a name="service-bus-namespace"></a>Palvelun Bus nimitila

Luo vakio palvelun Bus nimitilan tyypin **viestit**.

```
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Komentojen suorittamiseen käyttöönotto

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShellin

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut ja käyttöön resurssien Azure Resurssienhallinta, opi hallitsemaan nämä resurssit lukemalla seuraavissa artikkeleissa:

- [Hallitse palvelun Bus PowerShellin avulla](service-bus-powershell-how-to-provision.md)
- [Palvelun Bus resurssien palvelun Bus Resurssienhallinnassa](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Authoring Azure Resurssienhallinta-mallit]: ../resource-group-authoring-templates.md
  [Palvelun Bus nimitilan malli]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
  [Azure pikaopas mallit]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Palvelun Bus hinnat ja laskutus]: https://azure.microsoft.com/documentation/articles/service-bus-pricing-billing/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
