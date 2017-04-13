<properties 
    pageTitle="Logiikan sovelluksen Azure Resurssienhallinta mallien käyttäminen Azure App palvelun luominen | Microsoft Azure" 
    description="Azure Resurssienhallinta-mallin avulla voit tyhjä logiikan-sovelluksen käyttöönotto työnkulut määrittämistä varten." 
    services="logic-apps" 
    documentationCenter="" 
    authors="MSFTMan" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/25/2016" 
    ms.author="deonhe"/>

# <a name="create-a-logic-app-using-a-template"></a>Mallin avulla logiikan sovelluksen luominen

Azure Resurssienhallinta-mallin avulla voit luoda tyhjän logiikan-sovellusta, joka voidaan määrittää työnkulkuja. Voit määrittää resursseja on otettu käyttöön ja määrittäminen parametrit, jotka on määritetty, kun käyttöönotto suoritetaan. Voit käyttää tätä mallia oman käyttöönotoissa tai mukauttaa sen omaa ehtojen mukaan.

Saat lisätietoja logiikan sovelluksen ominaisuuksien [Logiikan App työnkulun hallinnan API](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Esimerkkejä itse määritelmä on [Tekijä logiikan sovelluksen määritykset](app-service-logic-author-definitions.md). 

Lisätietoja mallien luomisesta on artikkelissa [Yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md).

Artikkelissa [logiikan malli](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json)valmis malli.

## <a name="what-you-will-deploy"></a>Mitä otetaan käyttöön

Tämän mallin avulla logiikan sovelluksen käyttöönotto.

Voit suorittaa käyttöönoton automaattisesti valitsemalla Seuraava painike:  

[![Ottaa käyttöön Azure](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrit

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }
    
## <a name="resources-to-deploy"></a>Resurssien käyttöönotto

### <a name="logic-app"></a>Logiikan app

Luo logiikan-sovellus.

Mallien käyttää parametrin arvon logiikan sovelluksen nimi. Se asettaa logiikan sovelluksen sijainnin resurssiryhmän samaan sijaintiin. 

Tämä tietyn määritelmä suoritetaan kerran tunnissa ja testaa **testUri** -parametrissa määritetty sijainti. 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Komentojen suorittamiseen käyttöönotto

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShellin

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 
