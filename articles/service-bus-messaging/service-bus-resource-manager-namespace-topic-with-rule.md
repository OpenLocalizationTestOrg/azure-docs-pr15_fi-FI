<properties
    pageTitle="Luo palvelun Bus nimitilan aihe-tilaukseen ja sääntö Azure Resurssienhallinta-mallin avulla | Microsoft Azure"
    description="Luo palvelun Bus nimitilan ohjeaiheen, tilaus ja säännön Azure Resurssienhallinta-mallin avulla"
    services="service-bus"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Luo palvelun Bus nimitilan ohjeaiheen, tilaus ja säännön Azure Resurssienhallinta-mallin avulla

Tämän artikkelin avulla voit käyttää Azure Resurssienhallinta-malli, joka luo palvelun Bus nimitilan ohjeaiheen, tilaus ja säännön (suodatin). Kerrotaan, miten voit määrittää resursseja on otettu käyttöön ja määrittäminen parametrit, jotka ovat määritettyä kun käyttöönotto suoritetaan. Käytä tätä mallia oman ominaisuuksissa tai mukauttaa sen omaa ehtojen mukaan

Lisätietoja mallien luomisesta on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta mallit][].

Saat lisätietoja harjoittelu ja Azure resurssien nimeämiskäytännöt kuvioita on artikkelissa [Azure resurssien nimeäminen nimeämiskäytännön][].

Valmis mallin Katso [palvelun Bus nimitilan, jonka aiheessa, tilauksen, ja sääntö][] -mallin.

>[AZURE.NOTE] Seuraavat Azure Resurssienhallinta-mallit ovat ladattavissa ja käyttöönotto.
>
>-    [Luo palvelun Bus nimitilan jonossa ja käyttöoikeuksien sääntöä](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Luo palvelun Bus nimitilan jonossa](service-bus-resource-manager-namespace-queue.md)
>-    [Luo palvelun Bus nimitila](service-bus-resource-manager-namespace.md)
>-    [Aihe ja tilauksen palvelun Bus nimitilan luominen](service-bus-resource-manager-namespace-topic.md)
>
>Voit tarkistaa uusimman mallit-käy [Azure][] -pikaopas mallivalikoimasta ja Etsi palvelun Bus.

## <a name="what-will-you-deploy"></a>Mitä otetaan käyttöön?

Tämän mallin avulla voit ottaa käyttöön palvelun Bus nimitilan, jonka aiheessa, tilaus ja säännön (suodatin).

[Palvelun Bus aiheet ja tilaukset](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) on yksi-moneen-lomakkeen tiedonvälityksen, *Julkaise ja tilaa* kuvio. Kun käytät aiheet ja tilaukset-jaetun sovelluksen osia ei keskenään suoraan, sen sijaan ne vaihtaa viestejä artikkelista, johon välittämään kautta. Aiheen tilannut muistuttaa virtual jono, joka vastaanottaa viestejä, jotka on lähetetty aiheen kopioita. Suodatin-tilauksen avulla voit määrittää, mitkä viestit lähetetään aiheen pitäisi näkyä tiettyä aihetta-tilauksen piiriin kuuluvien.

## <a name="what-are-rules-filters"></a>Mitkä ovat säännöt (suodattimet)?

Skenaarioita viestejä, joilla on tiettyjä ominaisuuksia, on käsiteltävä eri tavoilla. Voit ottaa käyttöön, voit määrittää tilaukset, jos haluat löytää viestit, jotka on haluamasi ominaisuudet ja suorita sitten tietyt muutokset nämä ominaisuudet. Kun palvelun Bus tilaukset näkyvissä aiheeseen lähetetyt viestit, voit kopioida alijoukkoa viestit vain virtual tilauksen jonossa. Tämä tehdään tilauksen suodattimien avulla. Lisätietoja-rules(filters) on artikkelissa [palvelun Bus olevien, aiheet, ja tilaukset][].

Voit suorittaa käyttöönoton automaattisesti valitsemalla Seuraava painike:

[![Ottaa käyttöön Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrit

Azure resurssien hallinnan avulla kannattaa määrittää arvot, jotka haluat määrittää, kun malli otetaan käyttöön parametrit. Malli sisältää osan nimeltä `Parameters` , joka sisältää kaikki parametriarvot. Olisi määrittää kyseiset arvot, vaihtelevat otat projektin pohjalta tai otat käyttöön ympäristöön parametri. Määritä parametrien arvot, jotka pysyvät aina sama. Kunkin parametriarvo käytetään mallin määrittäminen resurssit, jotka on otettu käyttöön.

Mallin määrittää seuraavat parametrit:

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
### <a name="servicebusrulename"></a>serviceBusRuleName

Luotu-palvelun Bus nimitilan rule(filter) nimi.

```
   "serviceBusRuleName": {
   "type": "string",
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

Luo vakio palvelun Bus nimitilan tyypin **viestit**, aihe ja tilauksen ja säännöt.

```
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
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
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Komentojen suorittamiseen käyttöönotto

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShellin

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut ja käyttöön resurssien Azure Resurssienhallinta, opi hallitsemaan nämä resurssit tarkastelemalla seuraavissa artikkeleissa:

- [Azure-palvelu Bus käyttämällä Azure automaatio hallinta](service-bus-automation-manage.md)
- [Hallitse palvelun Bus PowerShellin avulla](service-bus-powershell-how-to-provision.md)
- [Palvelun Bus resurssien palvelun Bus Resurssienhallinnassa](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Authoring Azure Resurssienhallinta-mallit]: ../resource-group-authoring-templates.md
  [Azure pikaopas mallit]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Azure resurssien nimeämiskäytännöt]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
  [Palvelun Bus nimitilan, jonka aiheessa, tilaus ja sääntö]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
  [Palvelun Bus olevien, aiheet ja tilaukset]:service-bus-queues-topics-subscriptions.md
  
