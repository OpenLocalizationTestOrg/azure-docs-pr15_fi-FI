<properties 
   pageTitle="Hallita reititys ja käyttää virtual laitteiden Resurssienhallinta-mallin avulla | Microsoft Azure"
   description="Lue, miten voit hallita reititys ja käyttää virtual laitteiden Azure Resurssienhallinta mallin avulla"
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
   ms.date="02/23/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-resource-manager-by-using-a-template"></a>Luo käyttäjän määrittämät tiet (UDR) Resurssienhallinta-mallin avulla

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään resurssien hallinnan käyttöönottomalli. 

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

## <a name="udr-resources-in-a-template-file"></a>UDR resurssien mallitiedosto

Voit tarkastella ja lataa [malli-malli](https://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR).

Aikatiedot näyttää edusta UDR määritelmän yllä skenaarion perustuvan **azuredeploy-vnet-nsg-udr.json** tiedoston.

    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/routeTables",
    "name": "[parameters('frontEndRouteTableName')]",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "UDR - FrontEnd"
    },
    "properties": {
      "routes": [
        {
          "name": "RouteToBackEnd",
          "properties": {
            "addressPrefix": "[parameters('backEndSubnetPrefix')]",
            "nextHopType": "VirtualAppliance",
            "nextHopIpAddress": "[parameters('vmaIpAddress')]"
          }
        }
      ]


Liitä UDR edusta-aliverkon, sinun on muuta aliverkon määritys malliin ja käyttää viittaus-tunnus UDR.

    "subnets": [
        "name": "[parameters('frontEndSubnetName')]",
        "properties": {
          "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
          },
          "routeTable": {
              "id": "[resourceId('Microsoft.Network/routeTables', parameters('frontEndRouteTableName'))]"
          }
        },
        ...]

Huomaa, sama on tehty takaisin loppuun NSG ja uudelleen aliverkon mallissa.

Haluat myös varmistaa, että **FW1** AM on otettu käyttöön, jota käytetään vastaanottamaan ja välittäminen pakettien NIC ominaisuuden välittäminen IP. Aikatiedot näyttää Verkkokortti määrittelyn FW1 yllä skenaarion perustuvan azuredeploy nsg-udr.json tiedoston.

    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "location": "[variables('location')]",
    "tags": {
      "displayName": "NetworkInterfaces - DMZ"
    },
    "name": "[concat(variables('fwVMSettings').nicName, copyindex(1))]",
    "dependsOn": [
      "[concat('Microsoft.Network/publicIPAddresses/', variables('fwVMSettings').pipName, copyindex(1))]",
      "[concat('Microsoft.Resources/deployments/', 'vnetTemplate')]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "enableIPForwarding": true,
            "privateIPAllocationMethod": "Static",
            "privateIPAddress": "[concat('192.168.0.',copyindex(4))]",
            "publicIPAddress": {
              "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('fwVMSettings').pipName, copyindex(1)))]"
            },
            "subnet": {
              "id": "[variables('dmzSubnetRef')]"
            }
          }
        }
      ]
    },
    "copy": {
      "name": "fwniccount",
      "count": "[parameters('fwCount')]"
    }

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Ottaa käyttöön ARM-mallin avulla napsauttamalla käyttöönotto

Käytettävissä otoksen malli, julkisen säilössä käyttää parametrin tiedosto, joka sisältää arvot, joita käytetään edellä kuvatun skenaarion luomiseen oletusarvo. Voit ottaa tämän mallin avulla käyttöönotto, [tätä](https://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR)linkkiä napsauttamalla, valitse **käyttöönotto Azure**, korvaa parametrien oletusarvot tarvittaessa ja portaalin ohjeiden avulla.

## <a name="deploy-the-arm-template-by-using-powershell"></a>ARM-mallin käyttöön PowerShell-toiminnolla

Käyttöönotto lataamasi PowerShell-toiminnolla ARM-malli, noudata seuraavia ohjeita.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Jos et ole aikaisemmin käyttänyt PowerShellin Azure-artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) ja noudata ohjeita päässä Azure Kirjaudu ja valitse tilauksen.

2. Suorita `New-AzureRmResourceGroup` cmdlet-komento, voit luoda resurssiryhmä.

        New-AzureRmResourceGroup -Name TestRG -Location westus

3. Suorita `New-AzureRmResourceGroupDeployment` cmdlet-komento, voit ottaa mallin käyttöön.

        New-AzureRmResourceGroupDeployment -Name DeployUDR -ResourceGroupName TestRG `
            -TemplateUri https://raw.githubusercontent.com/telmosampaio/azure-templates/master/IaaS-NSG-UDR/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/telmosampaio/azure-templates/master/IaaS-NSG-UDR/azuredeploy.parameters.json            

    Odotettu tulos:

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              : 
        Permissions       : 
                            Actions  NotActions
                            =======  ==========
                            *                  
                            
        Resources         : 
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            ASFW                Microsoft.Compute/availabilitySets       westus  
                            ASSQL               Microsoft.Compute/availabilitySets       westus  
                            ASWEB               Microsoft.Compute/availabilitySets       westus  
                            FW1                 Microsoft.Compute/virtualMachines        westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            WEB1                Microsoft.Compute/virtualMachines        westus  
                            WEB2                Microsoft.Compute/virtualMachines        westus  
                            NICFW1              Microsoft.Network/networkInterfaces      westus  
                            NICSQL1             Microsoft.Network/networkInterfaces      westus  
                            NICSQL2             Microsoft.Network/networkInterfaces      westus  
                            NICWEB1             Microsoft.Network/networkInterfaces      westus  
                            NICWEB2             Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            PIPFW1              Microsoft.Network/publicIPAddresses      westus  
                            PIPSQL1             Microsoft.Network/publicIPAddresses      westus  
                            PIPSQL2             Microsoft.Network/publicIPAddresses      westus  
                            PIPWEB1             Microsoft.Network/publicIPAddresses      westus  
                            PIPWEB2             Microsoft.Network/publicIPAddresses      westus  
                            UDR-BackEnd         Microsoft.Network/routeTables            westus  
                            UDR-FrontEnd        Microsoft.Network/routeTables            westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  
                            
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG

## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>ARM-mallin asentavat Azure-CLI

Otetaan käyttöön ARM-mallin avulla Azure-CLI, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.
2. Suorita `azure config mode` komento siirtyy Resurssienhallinta-tilaan, alla kuvatulla tavalla.

        azure config mode arm

    Näin Odotettu tulos yllä-komennon:

        info:    New mode is arm

3. Selaimessa **https://raw.githubusercontent.com/telmosampaio/azure-templates/master/IaaS-NSG-UDR/azuredeploy.parameters.json**Siirry, json-tiedoston sisällön, kopioi ja liitä uuteen tiedostoon tietokoneeseen. Tässä tapauksessa kopioit olla alhaisempien arvojen **c:\udr\azuredeploy.parameters.json**-tiedostoon.

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "fwCount": {
              "value": 1
            },
            "webCount": {
              "value": 2
            },
            "sqlCount": {
              "value": 2
            }
          }
        }

4. Suorita **azure ryhmän luominen** cmdlet-komento ottaa käyttöön uuden VNet mallin ja parametri-tiedostoja ladataan ja muokata yläpuolella. Luettelon, kun tulos kerrotaan käytetyt parametrit.

        azure group create -n TestRG -l westus --template-uri 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/IaaS-NSG-UDR/azuredeploy.json' -e 'c:\udr\azuredeploy.parameters.json'

    Odotettu tulos:

        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Updating resource group TestRG
        info:    Updated resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK

5. Voit tarkastella luodaan uusi resurssiryhmä resursseja **azure ryhmän Näytä** -komennon suorittaminen. 

        azure group show TestRG

    Odotettu tulos
        
        info:    Executing command group show
        info:    Listing resource groups
        info:    Listing resources for the group
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    Resources:
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/availabilitySets/ASFW
        data:      Name    : ASFW
        data:      Type    : availabilitySets
        data:      Location: westus
        data:      Tags    : displayName=AvailabilitySet - DMZ
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/availabilitySets/ASSQL
        data:      Name    : ASSQL
        data:      Type    : availabilitySets
        data:      Location: westus
        data:      Tags    : displayName=AvailabilitySet - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/availabilitySets/ASWEB
        data:      Name    : ASWEB
        data:      Type    : availabilitySets
        data:      Location: westus
        data:      Tags    : displayName=AvailabilitySet - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/FW1
        data:      Name    : FW1
        data:      Type    : virtualMachines
        data:      Location: westus
        data:      Tags    : displayName=VMs - DMZ
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/SQL1
        data:      Name    : SQL1
        data:      Type    : virtualMachines
        data:      Location: westus
        data:      Tags    : displayName=VMs - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/SQL2
        data:      Name    : SQL2
        data:      Type    : virtualMachines
        data:      Location: westus
        data:      Tags    : displayName=VMs - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/WEB1
        data:      Name    : WEB1
        data:      Type    : virtualMachines
        data:      Location: westus
        data:      Tags    : displayName=VMs - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/WEB2
        data:      Name    : WEB2
        data:      Type    : virtualMachines
        data:      Location: westus
        data:      Tags    : displayName=VMs - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1
        data:      Name    : NICFW1
        data:      Type    : networkInterfaces
        data:      Location: westus
        data:      Tags    : displayName=NetworkInterfaces - DMZ
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL1
        data:      Name    : NICSQL1
        data:      Type    : networkInterfaces
        data:      Location: westus
        data:      Tags    : displayName=NetworkInterfaces - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL2
        data:      Name    : NICSQL2
        data:      Type    : networkInterfaces
        data:      Location: westus
        data:      Tags    : displayName=NetworkInterfaces - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1
        data:      Name    : NICWEB1
        data:      Type    : networkInterfaces
        data:      Location: westus
        data:      Tags    : displayName=NetworkInterfaces - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2
        data:      Name    : NICWEB2
        data:      Type    : networkInterfaces
        data:      Location: westus
        data:      Tags    : displayName=NetworkInterfaces - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd
        data:      Name    : NSG-BackEnd
        data:      Type    : networkSecurityGroups
        data:      Location: westus
        data:      Tags    : displayName=NSG - Front End
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
        data:      Name    : NSG-FrontEnd
        data:      Type    : networkSecurityGroups
        data:      Location: westus
        data:      Tags    : displayName=NSG - Remote Access
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPFW1
        data:      Name    : PIPFW1
        data:      Type    : publicIPAddresses
        data:      Location: westus
        data:      Tags    : displayName=PublicIPAddresses - DMZ
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPSQL1
        data:      Name    : PIPSQL1
        data:      Type    : publicIPAddresses
        data:      Location: westus
        data:      Tags    : displayName=PublicIPAddresses - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPSQL2
        data:      Name    : PIPSQL2
        data:      Type    : publicIPAddresses
        data:      Location: westus
        data:      Tags    : displayName=PublicIPAddresses - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPWEB1
        data:      Name    : PIPWEB1
        data:      Type    : publicIPAddresses
        data:      Location: westus
        data:      Tags    : displayName=PublicIPAddresses - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPWEB2
        data:      Name    : PIPWEB2
        data:      Type    : publicIPAddresses
        data:      Location: westus
        data:      Tags    : displayName=PublicIPAddresses - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd
        data:      Name    : UDR-BackEnd
        data:      Type    : routeTables
        data:      Location: westus
        data:      Tags    : displayName=Route Table - Back End
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd
        data:      Name    : UDR-FrontEnd
        data:      Type    : routeTables
        data:      Location: westus
        data:      Tags    : displayName=UDR - FrontEnd
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:      Name    : TestVNet
        data:      Type    : virtualNetworks
        data:      Location: westus
        data:      Tags    : displayName=VNet
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Storage/storageAccounts/testvnetstorageprm
        data:      Name    : testvnetstorageprm
        data:      Type    : storageAccounts
        data:      Location: westus
        data:      Tags    : displayName=Storage Account - Premium
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Storage/storageAccounts/testvnetstoragestd
        data:      Name    : testvnetstoragestd
        data:      Type    : storageAccounts
        data:      Location: westus
        data:      Tags    : displayName=Storage Account - Simple
        data:    
        data:    Permissions:
        data:      Actions: *
        data:      NotActions: 
        data:    
        info:    group show command OK

>[AZURE.TIP] Jos kaikki resurssit ei ole näkyvissä, suorita `azure group deployment show` varmistamiseksi käyttöönoton valmistelu tila-komento on *Succeded*.