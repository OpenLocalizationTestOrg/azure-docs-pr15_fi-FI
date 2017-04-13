<properties
   pageTitle="Ottaa käyttöön usean NIC VMs mallin resurssien hallinnan avulla | Microsoft Azure"
   description="Lue, miten voit ottaa usean NIC VMs mallin resurssien hallinnan avulla"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="deploy-multi-nic-vms-using-a-template"></a>Ottaa käyttöön usean NIC VMs mallin avulla

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][perinteinen käyttöönottomalli](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Koska aika vaiheessa ei voi olla saman resurssiryhmän VMs yksittäisen NIC kanssa ja VMs, jossa on useita NIC, resurssiryhmä ja kaikki muut komponentit toiseen käyttöoikeusryhmän oikein uudelleen-palvelimiin. Seuraavia ohjeita käyttää nimetyt *IaaSStory* tärkeimmät resurssiryhmä ja *IaaSStory Taustajärjestelmä* uudelleen palvelimia resurssiryhmä.

## <a name="prerequisites"></a>Edellytykset

Ennen kuin voit ottaa käyttöön uudelleen palvelimissa, sinun täytyy käyttöönotto ja kaikki tarvittavat resurssit tämän skenaarion tärkeimmät resurssiryhmä. Ottaa käyttöön näiden resurssien, noudata seuraavia ohjeita.

1. Siirry [mallisivun](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Valitse mallisivun oikealta puolelta **ylätason resurssiryhmä** **käyttöönotto Azure**.
3. Tarvittaessa Muuta kaavarivillä parametriarvot ja noudata ohjeita ottamaan resurssiryhmän Azure esikatselu-portaalissa.

> [AZURE.IMPORTANT] Varmista, että tallennustilan tilin nimet ovat yksilöllisiä. Kaksoiskappaleiden tallennustilan tilin nimiä ei voi olla Azure-tietokannassa.

## <a name="understand-the-deployment-template"></a>Tietoja käyttöönottomalli

Ennen kuin otat mallin mukana näitä ohjeita, varmista, että ymmärrät kuvaus. Seuraavia ohjeita on hyvä yleiskatsaus poistettavaan mallia.

1. Siirry [mallisivun](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Valitse Avaa mallitiedoston **azuredeploy.json** .
3. Huomaa *osType* parametrin alla. Tämä parametri voidaan valita käytettävän tietokantapalvelimen mitä AM-kuva, sekä useita käyttöjärjestelmän liittyvät asetukset.

        "osType": {
          "type": "string",
          "defaultValue": "Windows",
          "allowedValues": [
            "Windows",
            "Ubuntu"
          ],
          "metadata": {
            "description": "Type of OS to use for VMs: Windows or Ubuntu."
          }
        },

4. Selaa kohtaan muuttujien luettelo ja tarkista **dbVMSetting** -muuttujan alla. Se saa jokin **dbVMSettings** muuttujan taulukon osiin. Jos olet tutustunut software development terminologiaa, voit tarkastella Hajautustaulukkoa tai dictionay **dbVMSettings** -muuttuja.

        "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"

5. Oletetaan, että haluat ottaa Windowsin virtuaalilaitteiksi SQL uudelleen. Valitse **osType** arvo olisi *Windowsin*ja **dbVMSetting** muuttujan sisältäisi elementin alla, joka edustaa **dbVMSettings** muuttujan ensimmäinen arvo.

          "Windows": {
            "vmSize": "Standard_DS3",
            "publisher": "MicrosoftSQLServer",
            "offer": "SQL2014SP1-WS2012R2",
            "sku": "Standard",
            "version": "latest",
            "vmName": "DB",
            "osdisk": "osdiskdb",
            "datadisk": "datadiskdb",
            "nicName": "NICDB",
            "ipAddress": "192.168.2.",
            "extensionDeployment": "",
            "avsetName": "ASDB",
            "remotePort": 3389,
            "dbPort": 1433
          },

6. Ilmoitus **vmSize** sisältää arvon *Standard_DS3*. Vain tiettyjen AM koot Salli useita NIC käyttöä varten. Voit tarkistaa, mitkä AM koot ovat usean NIC ohjesisältöä [usean NIC yleiskatsaus](virtual-networks-multiple-nics.md)käytössä.
7. Vieritä **resurssit** ja huomaat ensimmäinen elementti. Se kuvaa tallennustilan tilin. Tallennustilan tilin käytetään säilyttämään tietojen levyjen kunkin tietokannan AM. Tässä skenaariossa tietokantojen AM on säännöllisesti tallennustilaan tallennettuja OS-levyn ja kaksi Suoritettaessa (premium) tallennustilaan tallennettuja tietoja-levyä.

        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('prmStorageName')]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "Storage Account - Premium"
          },
          "properties": {
            "accountType": "[parameters('prmStorageType')]"
          }
        },

8. Siirry seuraavaan resurssiin alla kuvatulla tavalla. Tämän resurssin edustaa käytettävä tietokanta access-tietokantojen AM NIC. Huomaa tämän resurssin **kopio** -funktion käyttöä. Mallin avulla voit ottaa käyttöön niin monta VMs kuin haluat, **dbCount** -parametrin perusteella. Tämän vuoksi sinun täytyy luoda tietokantoja, yhteen kunkin AM NIC yhtä paljon.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB DA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

9. Siirry seuraavaan resurssiin alla kuvatulla tavalla. Tämän resurssin edustaa tietokantojen AM hallintaan käytettävät NIC. Uudelleen sinun on jokin näistä NIC jokaiselle tietokannalle AM. Huomaa **networkSecurityGroup** -osan linkittäminen NSG, joka sallii RDP/SSH NIC vain, jos haluat käyttää.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB RA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
                  },
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

10. Siirry seuraavaan resurssiin alla kuvatulla tavalla. Tämän resurssin edustaa määrittäminen jaettava kaikki tietokannan VMs saatavuus. Näin takaa, että käytettävissä on aina yksi AM suorittaminen aikana ylläpito määrittäminen.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/availabilitySets",
          "name": "[variables('dbVMSetting').avsetName]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "AvailabilitySet - DB"
          }
        },

11. Vieritä alaspäin seuraavaan resurssiin. Tämän resurssin edustaa tietokannan VMs-komento ensimmäisen muutaman rivin alla. Huomaa, käytä **Kopioi** -funktion uudelleen varmistaa, että useita VMs luodaan **dbCount** -parametrin perusteella. Huomaa myös **dependsOn** -sivustokokoelman. Se näyttää kaksi NIC tarpeen luodaan ennen AM on otettu käyttöön, sekä käytettävyyden määrittäminen ja tallennustilaa-tili.

          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
          "location": "[variables('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
            "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
            "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
          ],
          "tags": {
            "displayName": "VMs - DB"
          },
          "copy": {
            "name": "dbvmcount",
            "count": "[parameters('dbCount')]"
          },

12. Vierittää alaspäin AM resurssin **networkProfile** -elementin alla kuvatulla tavalla. Huomaa, että käytettävissä on kaksi NIC parhaillaan kunkin AM viittaus. Kun luot useita NIC AM, sinun on määritettävä jokin NIC **primary** -ominaisuus *Tosi*ja *Epätosi*Aseta.

        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
              "properties": { "primary": true }
            },
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
              "properties": { "primary": false }
            }
          ]
        }
      }

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Ottaa käyttöön ARM-mallin avulla napsauttamalla käyttöönotto

> [AZURE.IMPORTANT] Varmista, että [vaatimat](#Pre-requisites) toimet ennen seuraavia ohjeita.

Käytettävissä otoksen malli, julkisen säilössä käyttää parametrin tiedosto, joka sisältää arvot, joita käytetään edellä kuvatun skenaarion luomiseen oletusarvo. Ottamaan tämän mallin avulla käyttöönotto, [tätä](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)linkkiä napsauttamalla oikealla puolella **Taustajärjestelmä resurssien ryhmitteleminen (ohjeissa)** Valitse **Ota käyttöön Azure**korvaa parametrien oletusarvot tarvittaessa ja portaalin ohjeiden avulla.

Seuraavassa kuvassa on uusi resurssiryhmä sisällön käyttöönoton jälkeen.

![Uudelleen resurssiryhmä](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>Ottaa mallin käyttöön PowerShell-toiminnolla

Ottaa käyttöön PowerShell-toiminnolla lataamasi malli, noudata seuraavia ohjeita.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Suorita **`New-AzureRmResourceGroup`** cmdlet-komento, voit luoda resurssiryhmä-mallin avulla.

        New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'

    Odotettu tulos:

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                 Type                                 Location
                            ===================  ===================================  ========
                            ASDB                 Microsoft.Compute/availabilitySets   westus  
                            DB1                  Microsoft.Compute/virtualMachines    westus  
                            DB2                  Microsoft.Compute/virtualMachines    westus  
                            NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                            wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Mallin asentavat Azure-CLI

Otetaan käyttöön mallin avulla Azure-CLI, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.
2. Suorita **`azure config mode`** komento siirtyy Resurssienhallinta-tilaan, alla kuvatulla tavalla.

        azure config mode arm

    Näin Odotettu tulos yllä-komennon:

        info:    New mode is arm

3. [Parametrin tiedoston](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json)avaaminen, valitse sen sisältö ja tallenna se tietokoneeseen tiedoston. Tässä esimerkissä on tallennettu parametrit-tiedoston *parameters.json*.

4. Suorita **`azure group deployment create`** cmdlet-komento, ottaa käyttöön uuden VNet käyttämällä mallia ja parametri tiedostoja ladataan ja muokata yläpuolella. Luettelon, kun tulos kerrotaan käytetyt parametrit.

        azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json

    Odotettu tulos:

        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
