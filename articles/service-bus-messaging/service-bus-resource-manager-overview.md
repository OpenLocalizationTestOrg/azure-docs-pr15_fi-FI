<properties
    pageTitle="Luo palvelun Bus resurssien Azure Resurssienhallinta mallien avulla | Microsoft Azure"
    description="Palvelun Bus resurssien luonnin automatisoida Azure Resurssienhallinta mallien avulla"
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
    ms.author="sethm"/>

# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Luo palvelun Bus resurssien Azure Resurssienhallinta mallien käyttäminen

Tässä artikkelissa käsitellään luoda ja ottaa käyttöön palvelun Bus ja tapahtuman keskittimet resurssien Azure Resurssienhallinta mallit, PowerShell ja palvelun Bus resurssin toimittaja.

Azure Resurssienhallinta-mallien avulla voit ratkaista käyttöönottaminen ja Määritä parametrit ja muuttujat, joiden avulla voit kirjoittaa arvoja eri ympäristöissä resurssien määrittäminen. Mallin koostuu JSON ja lausekkeita, joiden avulla voit muodostaa käyttöönoton arvot. Lisätietoja Azure Resurssienhallinta mallit ja mallin muodon keskustelun kirjoittamiseen on [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md). 

>[AZURE.NOTE] Tämän artikkelin esimerkeissä, miten voit käyttää Azure Resurssienhallinta palvelun Bus nimitilan ja tekstiviesti kohteen (jonon). Mallin muita esimerkkejä Siirry [Azure pikaopas mallivalikoimasta][] ja Etsi "Palvelun Bus."

## <a name="service-bus-and-event-hubs-resource-manager-templates"></a>Palvelun Bus ja tapahtuman keskittimet Resurssienhallinta-mallit

Nämä palvelun Bus ja tapahtuman keskittimet Azure Resurssienhallinta mallit ovat ladattavissa ja käyttöönotto. Valitse lisätietoja yksitellen, sekä linkit GitHub oleviin malleihin on seuraavissa linkeissä: 

- [Luo palvelun Bus nimitila](service-bus-resource-manager-namespace.md)
- [Luo palvelun Bus nimitilan jonossa](service-bus-resource-manager-namespace-queue.md)
- [Aihe ja tilauksen palvelun Bus nimitilan luominen](service-bus-resource-manager-namespace-topic.md)
- [Luo palvelun Bus nimitilan jonossa ja käyttöoikeuksien sääntöä](service-bus-resource-manager-namespace-auth-rule.md)
- [Luoda tapahtuman keskittimet nimitilan tapahtumaa-toiminnossa ja kuluttaja-ryhmän kanssa](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## <a name="deploy-with-powershell"></a>Ottaa käyttöön PowerShellin avulla

Seuraavassa kerrotaan, miten Azure Resurssienhallinta-malli, joka luo **Vakio** taso-palvelun Bus nimitilan ja jonon kyseisen nimitilan PowerShellin avulla. Tässä esimerkissä perustuu [Luo palvelun Bus nimitilan, jonka jonossa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) -malliin. Lähes työnkulku on seuraavanlainen:

1. PowerShellin asentaminen.
2. Luo malli ja (valinnainen) parametri-tiedosto.
2. PowerShellin Kirjaudu sisään Azure-tili.
3. Luo uusi resurssiryhmä, jos sellaista ei ole.
4. Testaa käyttöönotto.
5. Määritä halutessasi käyttöönotto-tilassa.
6. Ota malli käyttöön.

Saat lisätietoja Azure Resurssienhallinta mallien käyttämisessä, [käyttöönotto resurssien Azure Resurssienhallinta malleja][].

### <a name="install-powershell"></a>PowerShellin asentaminen

Asenna PowerShellin Azure [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md)ohjeita noudattamalla.

### <a name="create-a-template"></a>Mallin luominen

Kloonaa tai kopioi [201-servicebus-luominen-jonossa](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) mallin GitHub:

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Parametrit-tiedoston luominen (valinnainen)

Voit käyttää valinnaisten parametrien tiedostoa kopioi [201-servicebus-luominen-jonossa](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) -tiedosto. Korvaa arvo `serviceBusNamespaceName` haluat luoda käyttöönottoon ja korvaa arvo palvelun Bus nimitilan nimellä `serviceBusQueueName` luotavan jonossa nimellä. 

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Lisätietoja on aiheessa [parametri tiedosto](../resource-group-template-deploy.md#parameter-file) .

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Kirjaudu sisään Azure ja Määritä Azure tilaus

Suorita seuraava komento PowerShell-kehotteessa:

```
Login-AzureRmAccount
```

Sinua kehotetaan kirjautumaan Azure-tili. Kun olet kirjautunut, suorita seuraava komento voit tarkastella käytettävissä tilaukset.

```
Get-AzureRMSubscription
```

Tämä komento palauttaa luettelon käytettävissä Azure tilaukset. Valitse tilauksen nykyisessä istunnossa suorittamalla seuraavan komennon. Korvaa `<YourSubscriptionId>` GUID Azure tilaukseen, jota haluat käyttää.

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Määritä resurssiryhmä

Jos sinulla ei ole olemassa resurssiryhmä, Luo uusi resurssiryhmä **Uusi AzureRmResourceGroup** -komennolla. Anna nimi resurssiryhmä ja sijainnin, jota haluat käyttää. Esimerkki:

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Jos onnistuu, näyttöön tulee uusi resurssiryhmä yhteenveto.

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Käyttöönoton testaaminen

Vahvista käyttöönoton suorittamalla `Test-AzureRmResourceGroupDeployment` cmdlet-komento. Kun käyttöönoton testaaminen määritettävä parametrit samalla tavalla kuin yksittäiselle suoritettaessa käyttöönotto.

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Luo käyttöönotto

Jos haluat luoda uuden käyttöönoton, suorita `New-AzureRmResourceGroupDeployment` komento ja anna sitten tarvittavat parametrit pyydettäessä. Parametrit ovat käyttöönoton, resurssi-ryhmässä ja polku tai URL-Osoitteen nimen mallitiedoston nimi. Jos **tila** -parametria ei ole määritetty, käytetään **vaiheittainen** oletusarvon. Lisätietoja on artikkelissa [vaiheittainen ja valmis ominaisuuksissa](../resource-group-template-deploy.md#incremental-and-complete-deployments).

Seuraava komento kehottaa kolme parametrien PowerShell-ikkunassa:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Jos haluat määrittää parametrit tiedoston sen sijaan, käytä seuraavaa komentoa.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Voit myös tekstiin parametrit, kun suoritat käyttöönoton cmdlet-komento. Komento on seuraavanlainen:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

[Valmis](../resource-group-template-deploy.md#incremental-and-complete-deployments) käyttöönoton suorittamiseen **valmiina** **tila** -parametrin arvoksi:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### <a name="verify-the-deployment"></a>Tarkista käyttöönotto

Jos resurssit ovat käyttöön onnistuneesti, PowerShell-ikkunassa näkyvät käyttöönoton yhteenveto:

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt nähnyt basic työnkulun ja komentojen käyttöönotto Azure Resurssienhallinta-malli. Lisätietoja daxista on seuraavissa linkeissä:

- [Azure resurssin hallinnassa: yleiskatsaus][]
- [Resurssien Azure Resurssienhallinta malleja käyttöönotto][]
- [Mallien luominen](../resource-group-authoring-templates.md)


[Azure resurssin hallinnassa: yleiskatsaus]: ../resource-group-overview.md
[Resurssien Azure Resurssienhallinta malleja käyttöönotto]: ../resource-group-template-deploy.md
[Azure pikaopas mallivalikoimasta]: https://azure.microsoft.com/documentation/templates/?term=service+bus