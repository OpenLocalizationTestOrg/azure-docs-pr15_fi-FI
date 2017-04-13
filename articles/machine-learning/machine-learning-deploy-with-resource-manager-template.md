<properties
    pageTitle="Ottaa käyttöön Konepohjaisten Oppimistekniikoiden työtilan Azure Resurssienhallinta-mallin avulla | Microsoft Azure"
    description="Työtilan ottamisesta Azure koneen Learning Azure Resurssienhallinta-mallin avulla"
    services="machine-learning"
    documentationCenter=""
    authors="ahgyger"
    manager="haining"
    editor="garye"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="ahgyger"/>
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Ottaa käyttöön Konepohjaisten Oppimistekniikoiden työtilan Azure resurssien hallinnan avulla

## <a name="introduction"></a>Johdanto
Käyttämällä Azure-Resurssienhallinta, käyttöönottomallin tallentaa aikaa antamalla skaalattava tapa ottaa kelpoisuustarkistuksen yhdistetty osia ja yritä uudelleen järjestelmä. Määrittää Azure koneen Learning työtilat, esimerkiksi haluat määrittää ensin Azure-tallennustilan tilin ja ottaa sitten työtilan. Kuvitellaan tekisit työtilat satoja varten. Azure Resurssienhallinta-mallin avulla voit ottaa käyttöön Azure koneen Learning-työtilan ja kaikki sen riippuvuudet on helpompi löytämiseen. Tässä artikkelissa kerrotaan vaiheittaiset tätä prosessia. Artikkelissa [Azure Resurssienhallinta yleiskatsaus](../azure-resource-manager/resource-group-overview.md)hyvä yleiskatsaus Azure resurssin hallintaan.

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Vaiheittaiset ohjeet: tietokoneen Learning työtilan luominen
Olemme Luo Azure resurssiryhmä ja ottaa käyttöön uuden Azure-tallennustilan tilin ja uuden Azure koneen Learning työtilan Resurssienhallinta mallin avulla. Kun asennus on valmis, emme tulostuu tärkeitä tietoja työtilat, jotka on luotu (perusavaimen, workspaceID ja työtilan URL-osoite).

### <a name="create-an-azure-resource-manager-template"></a>Azure Resurssienhallinta-mallin luominen
Koneen Learning työtilan edellyttää Azure-tallennustilan tilin linkitetyn tietojoukko tallentamiseen.
Seuraavaan malliin käyttää resurssiryhmä luomiseen tallennustilan tilin nimi sekä työtilan nimi.  Se myös käyttää tallennustilan tilin nimi ominaisuutena työtilan luotaessa.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Tämän mallin tallentaminen c:\temp\ mlworkspace.json-tiedostoa.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Resurssiryhmä-malliin perustuvat käyttöönotto
* Avaa PowerShell
* Asenna moduulit Azure Resurssienhallinta ja Azure hallinta  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Nämä vaiheet Lataa ja asenna tarvittavat Viimeistele loput vaiheet moduulit. Tämä on vain on tehtävä vain kerran ympäristössä, jossa suoritetaan PowerShell-komennoilla.   

* Azure todentaminen  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
Tämä vaihe on toistettava istunnossa. Kun todennettu, tilauksen tiedot näytetään.

![Azure-tili][1]

Nyt kun Microsoft on Azure, voit luoda resurssiryhmän.

* Resurssiryhmän luominen

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Varmista, että resurssiryhmän on valmisteltu oikein. **ProvisioningState** pitäisi olla "onnistui."
Resurssin ryhmän nimeä käytetään mallin tallennustilan tilin nimi. Tallennustilan tilin nimi on 3 ja 24 merkkiä välillä ja käytä numerot ja pienet kirjaimet.

![Resurssiryhmä][2]

* Uuden tietokoneen Learning työtilan käyttämällä resurssien ryhmä-käyttöönoton, ota käyttöön.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Kun asennus on valmis, on helppoa, voit ottaa käyttöön työtilan ominaisuuksien access. Voit esimerkiksi käyttää ensisijainen Key-tunnuksen.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Voit hakea luodun työtilan tunnusten on Käynnistä AzureRmResourceAction-komennolla. Voit esimerkiksi näyttää kaikkia työtiloja ensisijainen ja toissijainen-tunnukset.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Kun työtila on valmisteltu, voit automatisoida monia Azure koneen Learning Studio tehtäviä [Azure koneen Learning PowerShell-moduuli](http://aka.ms/amlps).

## <a name="next-steps"></a>Seuraavat vaiheet 
* Lisätietoja [Authoring Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md). 
* Katsoa [Azure pikaopas malleja säilöön](https://github.com/Azure/azure-quickstart-templates)-palvelussa. 
* Tässä videossa tietoja [Azure Resurssienhallinta](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 
 
<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
