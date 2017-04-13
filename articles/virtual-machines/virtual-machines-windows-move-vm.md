<properties
    pageTitle="Siirrä Windows AM | Microsoft Azure"
    description="Siirtää Windows AM toiseen Azure tilaus tai resurssin ryhmään Resurssienhallinta käyttöönotto-mallissa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Windows-AM siirtäminen toiseen Azure-tilauksen tai resurssin ryhmään 

Tässä artikkelissa käydään läpi siirtäminen Windowsin AM resurssiryhmät tai tilausten välillä. Siirtyminen välillä tilaukset voidaan kätevä, jos alun perin luotu AM Omat tilauksen ja haluat siirtää yrityksen tilaukseen, jos haluat jatkaa työskentelyä.

> [AZURE.NOTE] Siirrä osana luodaan uusi resurssitunnukset. Kun AM on siirretty, sinun on päivitettävä työkalut ja komentosarjojen käyttää uusi resurssi tunnukset. 


[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]


## <a name="use-powershell-to-move-a-vm"></a>Siirrä AM PowerShellin avulla

Jos haluat siirtää toiseen resurssiryhmä virtual machine, varmista, että siirrät myös kaikki riippuvaiset resurssit. Käyttää Siirrä AzureRMResource cmdlet-komento, sinun on resurssin nimen ja resurssin lajin. Saat molempiin Etsi AzureRMResource cmdlet-komento.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"
    

Jos haluat siirtää AM, annettava siirtää useita resursseja. Syy voidaan luoda vain erillistä muuttujaa kullekin resurssille ja niistä luettelo. Tässä esimerkissä sisältää useimmat basic resursseista AM, mutta voit lisätä Lisää tarpeen mukaan.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"
    
    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"
    
    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Siirry eri tilauksen resurssit ovat **- DestinationSubscriptionId** parametrin. 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Sinua pyydetään vahvistamaan, että haluat siirtää määritettyjä resursseja. Kirjoita **Y** vahvistamaan, että haluat siirtää resurssit.

  
## <a name="next-steps"></a>Seuraavat vaiheet

Voit siirtää useita erityyppisiä resursseja resurssiryhmät ja tilaukset. Lisätietoja on artikkelissa [resurssien uusi resurssiryhmä tai-Tilaustani siirtäminen](../resource-group-move-resources.md).    