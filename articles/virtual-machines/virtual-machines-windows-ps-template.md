<properties
    pageTitle="Voit luoda AM Resurssienhallinta-mallin | Microsoft Azure"
    description="Resurssienhallinta-mallien ja PowerShellin avulla voit helposti luoda uuden Windows virtual koneen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-with-a-resource-manager-template"></a>Voit luoda Windows-virtual machine Resurssienhallinta-mallin avulla

Tässä artikkelissa esitellään Azure Resurssienhallinta-malli ja näytetään, miten voit ottaa raporttinäkymän käyttöön PowerShellin avulla. Malli otetaan käyttöön käytössä Windows Server uusi virtual verkossa, jossa yhden aliverkon yhteen virtual tietokoneeseen.

Olisi kestää noin 20 minuutin ajan voit tehdä tämän artikkelin ohjeita.

> [AZURE.IMPORTANT] Oman AM käytettävyys-joukon osana halutessasi lisätä sen joukkoa AM luodessasi. Tällä hetkellä ei ole mahdollisuutta lisätä AM Määritä, kun se on luotu saatavuus.

## <a name="step-1-create-the-template-file"></a>Vaihe 1: Luo mallitiedosto

Voit luoda oman mallin tiedot [Yhtä aikaa muiden kanssa Azure Resurssienhallinta](../resource-group-authoring-templates.md)mallien avulla. Voit myös ottaa malleilla, jotka on luotu puolestasi [Azure Quickstarts](https://azure.microsoft.com/documentation/templates/)malleista.

1. Avaa tuttuja tekstieditorissa ja lisää tarvittavat rakenneosan ja tarvittavat contentVersion elementti:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
        }
2. [Parametrit](../resource-group-authoring-templates.md#parameters) eivät ole aina pakollisia, mutta niissä voi kirjoittaa arvoja, kun malli otetaan käyttöön. Lisää parametrit-elementin ja sen alielementit contentVersion elementin jälkeen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

3. [Muuttujia](../resource-group-authoring-templates.md#variables) voi käyttää mallissa, voit määrittää arvot, jotka saattavat muuttua usein tai arvoja, jotka on luotava parametriarvojen yhdistelmän. Lisää muuttujat osan jälkeen parametrit-osassa:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }
        
4. [Resurssit](../resource-group-authoring-templates.md#resources) , kuten virtuaalikoneen, virtual verkon ja tallennustilaa tili on määritetty seuraava mallissa. Lisää resursseja-osassa muuttujat-osan jälkeen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvn1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
          
    >[AZURE.NOTE] Tässä artikkelissa Luo virtual kone Windows Server-käyttöjärjestelmän versio. Lisätietoja valitsemalla muut kuvat-kohdassa [Siirry ja valitse Azure virtuaalikoneen kuvat ja Windows PowerShellin Azure-CLI](virtual-machines-linux-cli-ps-findimage.md).  
            
2. Tallentaa mallitiedoston *VirtualMachineTemplate.json*.

## <a name="step-2-create-the-parameters-file"></a>Vaihe 2: Luo parametrit-tiedosto

Jos haluat määrittää arvot, jotka on määritetty malli resurssin parametreille, Luo parametrit-tiedosto, joka sisältää arvot, joita käytetään, kun malli otetaan käyttöön.

1. Kopioi tekstieditorissa, JSON sisältö uuteen tiedostoon, jota kutsutaan *Parameters.json*:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Katso lisätietoja [käyttäjänimi ja salasana](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).

2. Tallenna tiedosto parametrit.

## <a name="step-3-install-azure-powershell"></a>Vaihe 3: Azure PowerShellin asentaminen

Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja PowerShellin Azure uusimman version asentamisesta, valitsemalla tilauksen ja tiliisi kirjautuminen.

## <a name="step-4-create-a-resource-group"></a>Vaihe 4: Resurssiryhmän luominen

Kaikki resurssit on otettava käyttöön [resurssiryhmä](../azure-resource-manager/resource-group-overview.md).

1. Saat luettelon käytettävissä olevat sijainnit-kohtaa, johon voidaan luoda resurssit.

        Get-AzureRmLocation | sort DisplayName | Select DisplayName

2. Korvaa **$locName** arvon sijainti esimerkiksi **Yhdysvaltojen Keski**-luettelosta. Luo muuttuja.

        $locName = "location name"
        
3. **$RgName** arvon tilalle uusi resurssiryhmä nimi. Luo muuttujan ja resurssiryhmän.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Näyttöön tulee suunnilleen tässä esimerkissä:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1

## <a name="step-5-create-the-resources-with-the-template-and-parameters"></a>Vaihe 5: Luo resursseja mallia ja parametrit

Korvaa **$templateFile** arvosta polku ja mallitiedoston nimi. Korvaa **$parameterFile** arvon parametrit-tiedoston nimi ja polku. Luo muuttujat ja ottaa sitten mallin käyttöön. 

        $templateFile = "template file"
        $parameterFile = "parameter file"
        New-AzureRmResourceGroupDeployment -ResourceGroupName $rgName -TemplateFile $templateFile -TemplateParameterFile $parameterFile

    You will see something like this:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString

        Outputs           :

>[AZURE.NOTE] Voit myös ottaa mallit ja Azure-tallennustilan tilin parametrit. Lisätietoja on artikkelissa [Azure PowerShellin Azure-tallennustilan kanssa](../storage/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Seuraavat vaiheet

- Jos käyttöönoton ongelmat, seuraava vaihe on tarkasteltavan [vianmääritys resurssin ryhmän ominaisuuksissa Azure-portaalissa](../resource-manager-troubleshoot-deployments-portal.md)
- Opi hallitsemaan virtuaalikoneen, jonka loit tarkastelemalla [hallinta näennäiskoneiden Azure Resurssienhallinta ja PowerShellin avulla](virtual-machines-windows-ps-manage.md).
