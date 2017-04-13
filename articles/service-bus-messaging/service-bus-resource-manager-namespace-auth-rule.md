<properties
    pageTitle="Luo sääntö palvelun Bus luvan Azure Resurssienhallinta-mallin avulla | Microsoft Azure"
    description="Palvelun Bus luvan säännön nimitila ja jonon Azure Resurssienhallinta-mallin avulla"
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

# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Palvelun Bus luvan säännön nimitila ja jonon Azure Resurssienhallinta-mallin avulla

Tässä artikkelissa kerrotaan Azure Resurssienhallinta-malli, joka luo [käyttöoikeuksien sääntöä](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) palvelun Bus nimitilan ja jonon käyttämisestä. Opit, miten voit määrittää resursseja on otettu käyttöön ja määrittäminen parametrit, jotka ovat määritettyä kun käyttöönotto suoritetaan. Voit käyttää tätä mallia oman käyttöönotoissa tai mukauttaa sen omaa ehtojen mukaan.

Lisätietoja mallien luomisesta on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja][].

Valmis mallin Katso [palvelun Bus auth sääntömallin][] GitHub.

>[AZURE.NOTE] Seuraavat Azure Resurssienhallinta-mallit ovat ladattavissa ja käyttöönotto.
>
>-    [Luoda tapahtuman keskittimet nimitilan tapahtumaa-toiminnossa ja kuluttaja-ryhmän kanssa](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Luo palvelun Bus nimitilan jonossa](service-bus-resource-manager-namespace-queue.md)
>-    [Aihe ja tilauksen palvelun Bus nimitilan luominen](service-bus-resource-manager-namespace-topic.md)
>-    [Luo palvelun Bus nimitila](service-bus-resource-manager-namespace.md)
>
>Tarkista uusimmat mallit, käy [Azure pikaopas][] mallivalikoimasta ja Etsi "Palvelun Bus."

## <a name="what-will-you-deploy"></a>Mitä otetaan käyttöön?

Tämän mallin avulla otetaan käyttöön palvelun Bus luvan säännön nimitilan ja tekstiviesti kohteen (Kirjoita tässä tapauksessa jonossa).

Tämä malli käyttää [Jaettu Access allekirjoitus (SAS)](service-bus-sas-overview.md) todennusta varten. SAS mahdollistaa sovellusten todentaa palvelun Bus määritetty nimitila tai (jonossa tai aiheen), johon on liitetty oikeudet tekstiviesti kohteen access-näppäintä. Voit sitten avaimeen SAS-tunnuksen, asiakkaat voivat puolestaan käyttää todentaa palvelun Bus luomiseen.

Voit suorittaa käyttöönoton automaattisesti valitsemalla Seuraava painike:

[![Ottaa käyttöön Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

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

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName 

Nimitilan luvan säännön nimi.

```
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Palvelun Bus nimitilan jonossa nimi.

```
"serviceBusQueueName": {
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

Luo vakio palvelun Bus nimitilan tyypin **viestit**ja palvelun Bus luvan säännön nimitilan ja yritys.

```
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a>Komentojen suorittamiseen käyttöönotto

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShellin

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut ja käyttöön resurssien Azure Resurssienhallinta, opi hallitsemaan nämä resurssit tarkastelemalla seuraavissa artikkeleissa:

- [Hallitse palvelun Bus PowerShellin avulla](service-bus-powershell-how-to-provision.md)
- [Palvelun Bus resurssien palvelun Bus Resurssienhallinnassa](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)
- [Palvelun Bus todennus- ja](service-bus-authentication-and-authorization.md)

  [Authoring Azure Resurssienhallinta-mallit]: ../resource-group-authoring-templates.md
  [Azure pikaopas mallit]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Palvelun Bus auth sääntömallin]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
