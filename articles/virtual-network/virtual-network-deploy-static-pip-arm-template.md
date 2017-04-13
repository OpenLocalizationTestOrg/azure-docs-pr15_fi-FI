<properties
   pageTitle="Käyttöönotto AM kanssa staattinen julkiseen IP mallin resurssien hallinnan avulla | Microsoft Azure"
   description="Opi ottamaan VMs kanssa staattinen julkiseen IP mallin resurssien hallinnan avulla"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-a-template"></a>Ottaa käyttöön AM kanssa staattinen julkiseen IP mallin avulla

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönoton mallia.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-resources-in-a-template-file"></a>Julkiseen IP resurssien mallitiedosto

Voit tarkastella ja lataa [malli-malli](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

Aikatiedot näyttää määritelmän julkiseen IP-resurssin yllä skenaarion perusteella.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[variables('webVMSetting').pipName]",
        "location": "[variables('location')]",
        "properties": {
          "publicIPAllocationMethod": "Static"
        },
        "tags": {
          "displayName": "PublicIPAddress - Web"
        }
      },

Huomaa **publicIPAllocationMethod** -ominaisuutta, joka on määritetty *staattinen*. Tämä ominaisuus voi olla *dynaaminen* (oletusarvo) tai *staattinen*. Miten se staattinen takuita, julkinen IP-osoite on määritetty ei koskaan muuttaa.

Aikatiedot näyttää verkkoliittymän liitoksen julkiseen IP-osoite.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[variables('webVMSetting').nicName]",
        "location": "[variables('location')]",
        "tags": {
          "displayName": "NetworkInterface - Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
          "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
          "ipConfigurations": [
            {
              "name": "ipconfig1",
              "properties": {
                "privateIPAllocationMethod": "Static",
                "privateIPAddress": "[variables('webVMSetting').ipAddress]",
                "publicIPAddress": {
                  "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
                },
                "subnet": {
                  "id": "[variables('frontEndSubnetRef')]"
                }
              }
            }
          ]
        }
      },

Huomaa **publicIPAddress** ominaisuus nimeltä **variables('webVMSetting').pipName**resurssin **tunnus** osoittamalla. Tämä on yllä julkiseen IP-resurssin nimi.

Lopuksi yllä verkkoliittymän näkyy luotavan AM **networkProfile** -ominaisuudessa.

      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Asentavat mallia valitsemalla käyttöönotto

Käytettävissä otoksen malli, julkisen säilössä käyttää parametrin tiedosto, joka sisältää arvot, joita käytetään edellä kuvatun skenaarion luomiseen oletusarvo. Voit ottaa mallin käyttöön napsauttamalla avulla, valitse **käyttöönotto Azure** [AM kanssa staattinen PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) mallin Readme.md-tiedostossa. Korvaa parametrien oletusarvot halutessasi ja kirjoita arvot tyhjä parametrit.  Luo virtual machine staattinen julkiseen IP-osoite portaalin ohjeiden mukaan.

## <a name="deploy-the-template-by-using-powershell"></a>Ottaa mallin käyttöön PowerShell-toiminnolla

Ottaa käyttöön PowerShell-toiminnolla lataamasi malli, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt PowerShellin Azure, katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) ja noudata ohjeita vaiheissa 1 – 3.

2. PowerShell-konsolin Luo uusi resurssiryhmä tarvittaessa **Uusi AzureRmResourceGroup** cmdlet-komennon suorittaminen Jos sinulla on jo luotu resurssiryhmä, siirry vaiheeseen 3.

        New-AzureRmResourceGroup -Name PIPTEST -Location westus

    Odotettu tulos:

        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. PowerShell-konsolissa ottaa mallin käyttöön **Uusi AzureRmResourceGroupDeployment** cmdlet-komennon suorittaminen

        New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
            -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json

    Odotettu tulos:

        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : <Deployment date> <Deployment time>
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0

        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         

        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Mallin asentavat Azure-CLI

Otetaan käyttöön mallin avulla Azure-CLI, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt Azure CLI, noudata [Asenna ja Määritä Azure-CLI](../xplat-cli-install.md) -artikkelin ohjeiden mukaisesti ja valitse sitten vaiheet ja yhdistää CLI tilauksen [Yhdistä Azure tilaukseen Azure komentorivivalitsimet Interface (Azure CLI)-](../xplat-cli-connect.md) artikkelin "Käytä azure kirjautuminen tarkistamiseen vuorovaikutteisesti"-osassa.
2. Resurssienhallinta-tilassa, siirry **azure config tila** -komennon suorittaminen alla kuvatulla tavalla.

        azure config mode arm

    Näin Odotettu tulos yllä-komennon:

        info:    New mode is arm

3. [Parametrin tiedoston](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json)avaaminen, valitse sen sisältöä ja tallenna se tietokoneeseen tiedoston. Tässä esimerkissä parametrit tallennetaan *parameters.json*-tiedostoon. Muuta kaavarivillä parametriarvot tarvittaessa-tiedostossa, mutta vähintään, on suositeltavaa, muuttaa adminPassword-parametrin yksilöllinen, monimutkaisia salasana.

4. Suorita **azure ryhmän käyttöönoton luominen** cmdlet-komento otetaan käyttöön uuden VNet avulla mallin ja parametri tiedostoja ladataan ja muokata yläpuolella. Korvaa-komennon alla, <path> polulla tallensit tiedoston. 

        azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json

    Odotettu tulos (luettelon käytetään parametriarvot):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/<Subscription ID>/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
