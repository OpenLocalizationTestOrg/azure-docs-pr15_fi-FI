<properties
    pageTitle="Aihe ja Azure Resurssienhallinta-malli-tilaukseesi palvelun Bus nimitilan luominen | Microsoft Azure"
    description="Aihe ja Azure Resurssienhallinta malli-tilaukseesi palvelun Bus nimitilan luominen"
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
    ms.date="10/14/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a>Aihe ja Azure Resurssienhallinta-malli-tilaukseesi palvelun Bus nimitilan luominen

Tämän artikkelin avulla voit käyttää Azure Resurssienhallinta-malli, joka luo palvelun Bus nimitilan aihe ja tilauksen. Opit, miten voit määrittää resursseja on otettu käyttöön ja määrittäminen parametrit, jotka ovat määritettyä kun käyttöönotto suoritetaan. Käytä tätä mallia oman ominaisuuksissa tai mukauttaa sen omaa ehtojen mukaan

Lisätietoja mallien luomisesta on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja][].

Valmis mallin Katso [palvelun Bus nimitilan, jonka aiheessa ja tilauksen][] -mallin.

>[AZURE.NOTE] Seuraavat Azure Resurssienhallinta-mallit ovat ladattavissa ja käyttöönotto.
>
>-    [Luo palvelun Bus nimitilan jonossa ja käyttöoikeuksien sääntöä](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Luo palvelun Bus nimitilan jonossa](service-bus-resource-manager-namespace-queue.md)
>-    [Luo palvelun Bus nimitila](service-bus-resource-manager-namespace.md)
>-    [Luoda tapahtuman keskittimet nimitilan tapahtumaa-toiminnossa ja kuluttaja-ryhmän kanssa](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Tarkista uusimmat mallit, käy [Azure pikaopas][] mallivalikoimasta ja Etsi "Palvelun Bus."

## <a name="what-will-you-deploy"></a>Mitä otetaan käyttöön?

Tämän mallin avulla otetaan käyttöön palvelun Bus nimitilan, jonka aiheessa ja tilauksen.

[Palvelun Bus aiheet ja tilaukset](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) on yksi-moneen-lomakkeen tiedonvälityksen, *Julkaise ja tilaa* kuvio.

Voit suorittaa käyttöönoton automaattisesti valitsemalla Seuraava painike:

[![Ottaa käyttöön Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrit

Azure resurssien hallinnan avulla voit määrittää parametrien arvot, jotka haluat määrittää, kun malli otetaan käyttöön. Malli sisältää osan nimeltä `Parameters` , joka sisältää kaikki parametriarvot. Olisi määrittää kyseiset arvot, vaihtelevat otat projektin pohjalta tai otat käyttöön ympäristöön parametri. Määritä parametrien arvot, jotka aina säilyy ennallaan. Kunkin parametriarvo käytetään mallin määrittäminen resurssit, jotka on otettu käyttöön.

Mallin määrittää seuraavat parametrit.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Palvelun Bus nimitilan luominen nimi.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName

Luotu-palvelun Bus nimitilan aiheen nimi.

```
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName

Luotu-palvelun Bus nimitilan tilauksen nimi.

```
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Mallin palvelun Bus API-versio.

```
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a>Resurssien käyttöönotto

Luo vakio palvelun Bus nimitilan tyypin **viestit**, aihe ja tilauksen.

```
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Komentojen suorittamiseen käyttöönotto

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShellin

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut ja käyttöön resurssien Azure Resurssienhallinta, opi hallitsemaan nämä resurssit tarkastelemalla seuraavissa artikkeleissa:

- [Hallitse palvelun Bus PowerShellin avulla](service-bus-powershell-how-to-provision.md)
- [Palvelun Bus resurssien palvelun Bus Resurssienhallinnassa](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Authoring Azure Resurssienhallinta-mallit]: ../resource-group-authoring-templates.md
  [Azure pikaopas mallit]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Palvelun Bus nimitilan, jonka aiheessa ja tilauksen]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
