<properties 
    pageTitle="Valmistele Redis.txt välimuistin | Microsoft Azure" 
    description="Azure Resurssienhallinta mallin avulla voit ottaa käyttöön Azure Redis välimuistin." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="sdanie"/>

# <a name="create-a-redis-cache-using-a-template"></a>Luoda Redis välimuistin mallin avulla

Tässä aiheessa kerrotaan luominen Azure Resurssienhallinta-malli, joka ottaa käyttöön Azure Redis välimuistin. Välimuistin voidaan diagnostiikan tiedot pysyvät tallennustilan-tilillä. Opit myös siitä, miten voit määrittää resursseja on otettu käyttöön ja määrittäminen parametrit, jotka ovat määritettyä kun käyttöönotto suoritetaan. Voit käyttää tätä mallia oman käyttöönotoissa tai mukauttaa sen omaa ehtojen mukaan.

Tällä hetkellä kaikki tallentaa samalla alueella tilauksen jaetaan diagnostiikka-asetusten muuttaminen. Muut alueen tallentaa yhden alueen välimuistin päivitys vaikuttaa.

Lisätietoja mallien luomisesta on artikkelissa [Yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md).

Valmis mallin Katso [Redis välimuistin mallin](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

>[AZURE.NOTE] Uusi [Premium taso](cache-premium-tier-intro.md) Resurssienhallinta mallit ovat käytettävissä. 
>
>-    [Luoda Premium Redis välimuistin asentaminen](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
>-    [Luo Premium Redis välimuistin tiedot pysyvyys](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
>-    [VNet ja valinnainen klusterointi Premium Redis välimuistin luominen](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
>
>Tarkista uusimmat mallit, näet [Azure pikaopas mallit](https://azure.microsoft.com/documentation/templates/) ja etsiä `Redis Cache`.

## <a name="what-you-will-deploy"></a>Mitä otetaan käyttöön

Tämä malli otetaan käyttöön Azure Redis välimuistin, joka käyttää käytössä olevan tallennustilan tilin diagnostiikkatiedot.

Voit suorittaa käyttöönoton automaattisesti valitsemalla Seuraava painike:

[![Ottaa käyttöön Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrit

Azure resurssien hallinnan avulla voit määrittää parametrien arvot, jotka haluat määrittää, kun malli otetaan käyttöön. Malli sisältää osan nimeltä parametrit, joka sisältää kaikki parametriarvot.
Olisi määrittää kyseiset arvot, vaihtelevat otat projektin pohjalta tai otat käyttöön ympäristöön parametri. Määritä parametrien arvot, jotka pysyvät aina sama. Kunkin parametriarvo käytetään mallin määrittäminen resurssit, jotka on otettu käyttöön. 


[AZURE.INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation

Redis välimuistin sijainti. Parhaan suorituskyvyn Käytä samaa sijaintia kuin sovellus, jota käytetään välimuistin.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName

Olemassa olevan tallennustilan tili, jota käytetään diagnostiikan asetuksia nimi. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort

Totuusarvo, joka ilmaisee, voivatko käyttää-SSL portit kautta.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus

Arvo, joka ilmaisee, onko diagnostiikka käytössä. Käytä käytössä tai ei käytössä.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }
    
## <a name="resources-to-deploy"></a>Resurssien käyttöönotto

### <a name="redis-cache"></a>Redis välimuisti

Luo Azure Redis.txt välimuistissa.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Komentojen suorittamiseen käyttöönotto

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)] 

### <a name="powershell"></a>PowerShellin

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache -redisCacheLocation "West US"

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


