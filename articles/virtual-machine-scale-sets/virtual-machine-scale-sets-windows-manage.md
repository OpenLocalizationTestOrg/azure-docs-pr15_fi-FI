<properties
    pageTitle="Hallitse VMs virtuaalikoneen asteikko joukon | Microsoft Azure"
    description="Voit hallita näennäiskoneiden virtuaalikoneen mittakaava määrittäminen Azure PowerShellin avulla."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>Hallitse näennäiskoneiden virtuaalikoneen asteikko-määrittäminen

Tässä artikkelissa tehtävien avulla voit hallita näennäiskoneiden virtuaalikoneen asteikko-määrittäminen.

Useimmat tehtävät, joihin sisältyy hallinta virtual kone asteikko määrittäminen edellyttää, että tiedät tunniste, jota haluat hallita koneen. Voit [Azure resurssin Explorer](https://resources.azure.com) etsitään asteikko virtual machine tunniste. Voit myös resurssi Resurssienhallinnan avulla tarkistaa tehtävät, jotka olet tilan.

Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja PowerShellin Azure uusimman version asentamisesta, valitsemalla tilauksen ja tiliisi kirjautuminen.

## <a name="display-information-about-a-scale-set"></a>Näyttää tietoja asteikko

Saat joukon mittakaava, jossa käytetään myös nimitystä näkymä esiintymän yleisiä tietoja. Tai saat tarkempia tietoja, kuten Skaalaa määrittäminen resurssien tietoja.

Tarjottu arvojen korvaaminen nimi tai resurssiryhmä ja määritä ja suorita-komento:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Se palauttaa seuraavankaltaiselta:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded
    
Korvaa resurssien ryhmittely ja mittakaava määrittäminen nimi lainausmerkeissä olevaa arvot. Korvaa *#* , jonka haluat tietoja ja suorittaa virtuaalikoneen esiintymä-tunnuksella:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
        
Se palauttaa suunnilleen tässä esimerkissä:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded
        
## <a name="start-a-virtual-machine-in-a-scale-set"></a>Käynnistettäessä asteikko määrittäminen

Korvaa resurssien ryhmittely ja mittakaava määrittäminen nimi lainausmerkeissä olevaa arvot. Korvaa *#* , jonka haluat Käynnistä ja suorita sitten virtuaalikoneen tunnuksella:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Resurssin Resurssienhallinnassa näkyvissä esiintymän tila on **käynnissä**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

Voit aloittaa kaikki näennäiskoneiden järjestäminen esiintymän tunnus - parametri ei mittakaava.
    
## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Lopeta virtual kone asteikko määrittäminen

Korvaa resurssien ryhmittely ja mittakaava määrittäminen nimi lainausmerkeissä olevaa arvot. Korvaa *#* , jonka haluat lopettaa ja suorita sitten virtuaalikoneen tunnuksella:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Resurssin Resurssienhallinnassa näemme, että esiintymän tila on **Varaus poistettu**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]
    
Voit pysäyttää virtual machine ja poista se ei ole varaus, käytä - StayProvisioned-parametria. Voit lopettaa kaikki joukon näennäiskoneiden käyttämällä esiintymän tunnus - parametria ei.
    
## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Käynnistä virtual machine asteikko määrittäminen

Korvaa tarjottu arvot nimi ja resurssiryhmä-asteikko-määrittäminen. Korvaa *#* , jonka haluat Käynnistä ja suorita sitten virtuaalikoneen tunnuksella:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
    
Voit käynnistää kaikki näennäiskoneiden olevan käytössä ei ole esiintymän tunnus - parametrin.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>Poista virtual machine asteikko joukosta

Korvaa tarjottu arvot nimi ja resurssiryhmä-asteikko-määrittäminen. Korvaa *#* , jonka haluat poistaa, ja suorita sitten virtuaalikoneen tunnuksella:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Voit poistaa kaikki kerralla virtuaalikoneen-asteikon määrittäminen käyttämällä esiintymän tunnus - parametria ei.

## <a name="change-the-capacity-of-a-scale-set"></a>Muuta asteikko-joukon kapasiteetti

Voit lisätä tai poistaa näennäiskoneiden muuttamalla joukon kapasiteetti. Pyydä asteikko joukko, jonka haluat muuttaa, kapasiteetin määrittäminen mitä haluat määrittää ja päivittää määrityksessä uutta kapasiteettia asteikon. Nämä komennot korvaa resurssiryhmän ja skaalaus-joukon nimi lainausmerkeissä olevaa arvot.

  $vmss = get-AzureRmVmss - ResourceGroupName "resurssin ryhmänimi" - VMScaleSetName "asteikko joukon nimi" $vmss.sku.capacity = 5 päivitys AzureRmVmss - ResourceGroupName "resurssiryhmän nimi"-nimen "skaalata joukon nimi" - VirtualMachineScaleSet $vmss 

Jos poistat näennäiskoneiden asteikko joukosta, näennäiskoneiden on suurin tunnukset poistetaan ensin.
