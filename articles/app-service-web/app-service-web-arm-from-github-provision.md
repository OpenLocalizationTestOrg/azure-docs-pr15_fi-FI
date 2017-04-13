<properties 
    pageTitle="Web-sovelluksen, joka on linkitetty GitHub säilön käyttöönotto" 
    description="Azure Resurssienhallinta-mallin avulla voit ottaa käyttöön, joka sisältää GitHub säilöstä project web App-sovelluksesta." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/27/2016" 
    ms.author="cephalin"/>

# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Linkitetty GitHub säilöön web-sovelluksen käyttöönotto

Tässä ohjeaiheessa kerrotaan luominen Azure Resurssienhallinta-malli, joka ottaa käyttöön, joka on linkitetty GitHub säilöön, project web App-sovelluksesta. Opit, miten voit määrittää resursseja on otettu käyttöön ja määrittäminen parametrit, jotka ovat määritettyä kun käyttöönotto suoritetaan. Voit käyttää tätä mallia oman käyttöönotoissa tai mukauttaa sen omaa ehtojen mukaan.

Lisätietoja mallien luomisesta on artikkelissa [Yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md).

Katso valmis malli [Web App linkitetyn GitHub malliin](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-deploy"></a>Mitä otetaan käyttöön

Tämän mallin avulla otetaan käyttöön, joka sisältää GitHub projektin koodin verkkosovellukseen.

Voit suorittaa käyttöönoton automaattisesti valitsemalla Seuraava painike:

[![Ottaa käyttöön Azure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrit

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL

GitHub säilöön, joka sisältää projektin ottamaan URL-osoite. Tämä parametri on oletusarvo, mutta tämä arvo on tarkoitettu vain noudattamalla voit kirjoittaa URL-Osoitteen säilöön. Voit käyttää tätä arvoa, kun testaus mallin, mutta haluat määrittää URL-Osoitteen oman säilöön, kun käsittelet malli.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>haara

Sovelluksen käyttöönotossa käytettävä säilö haaraa. Oletusarvo on perusmuodon, mutta voit antaa minkään haaran säilöön, jonka haluat ottaa nimi.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## <a name="resources-to-deploy"></a>Resurssien käyttöönotto

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Web App-sovelluksessa

Luo web-sovellusta, joka on linkitetty GitHub projektiin. 

Määritä **nimi** -parametrin kautta web-sovelluksen nimi ja sijainti web-sovelluksen **siteLocation** -parametrin kautta. **DependsOn** -osaan mallin määritellään web-sovelluksen riippuvaiset isännöinnin suunnitelma-palvelusta. Koska se on riippuvainen isännöintipalvelu suunnitelma, web app ei ole luotu, ennen kuin isännöintipalvelu palvelupaketti on päättynyt luomisen. **DependsOn** -osa käytetään vain käyttöönoton tilauksen määrittämiseen. Jos et Merkitse sellaisena kuin se on riippuvainen isännöintipalvelu suunnitelman online-Azure resurssin Mananger yrittää luoda molempien resurssien samanaikaisesti ja saattaa tulla virhesanoma, jos web-sovellus on luotu ennen isännöintipalvelu suunnitelma.

Web-sovelluksessa on myös lapsen resurssin, joka on määritetty **resursseja** on kuvattu. Lapsen tämän resurssin määrittää tietolähteen ohjausobjektin Project web Appin käyttöön. Tämän mallin tietolähteen-ohjausobjekti on linkitetty tietyn GitHub säilöön. GitHub säilö on määritetty koodilla **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** voit ehkä koodata säilöön URL-osoite, kun haluat luoda mallin, joka ottaa toistuvasti vaativat parametrien vähimmäismäärää yhden projektin.
Sen sijaan, että Kova-koodaus säilöön URL-osoite, voit lisätä parametrin säilöön URL-osoite ja käytettävän **RepoUrl** -ominaisuuden arvon.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Komentojen suorittamiseen käyttöönotto

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShellin

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 
