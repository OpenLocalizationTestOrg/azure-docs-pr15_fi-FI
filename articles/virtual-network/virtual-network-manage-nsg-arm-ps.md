<properties 
   pageTitle="Hallitse NSGs resurssien hallinta PowerShellin avulla | Microsoft Azure"
   description="Opi hallitsemaan exising NSGs resurssien hallinta PowerShellin avulla"
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
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-powershell"></a>Hallitse NSGs PowerShellin avulla

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönoton mallia.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Tietojen hakeminen

Voit tarkastella aiemmin luotuja NSGs, säännöt noutaminen aiemmin NSG ja selvitä, mitä resursseja NSG on liitetty.

### <a name="view-existing-nsgs"></a>Tarkastele aiemmin NSGs
Jos haluat tarkastella kaikkien aiemmin NSGs Tilauksen, suorita `Get-AzureRmNetworkSecurityGroup` cmdlet-komennon alla kuvatulla tavalla.

    Get-AzureRmNetworkSecurityGroup

Odotettu tulos:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
                            
    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Jos haluat tarkastella tietyn resurssiryhmän NSGs luettelo, suorita `Get-AzureRmNetworkSecurityGroup` cmdlet-komennon alla kuvatulla tavalla. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG

Odotettu tulos:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
         
### <a name="list-all-rules-for-an-nsg"></a>NSG on lueteltu kaikki säännöt

Voit tarkastella NSG **NSG edusta**-niminen sääntöjä, suorita `Get-AzureRmNetworkSecurityGroup` cmdlet-komennon alla kuvatulla tavalla. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules

Odotettu tulos:
    
    Name                     : rdp-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound
    
    Name                     : web-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

>[AZURE.NOTE] Voit myös käyttää `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` **NSG edusta** NSG oletusarvon säännöt-luettelo.

### <a name="view-nsgs-associations"></a>Näytä NSGs yhteydet

Mitä **NSG edusta** -NSG on resursseja yhdistää katselemista Suorita `Get-AzureRmNetworkSecurityGroup` cmdlet-komennon alla kuvatulla tavalla.

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd

Etsi **NetworkInterfaces** ja **aliverkosta** ominaisuudet alla kuvatulla tavalla:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

Yllä olevassa esimerkissä NSG ei ole liitetty verkon käyttöliittymää (NIC) ja se liittyy **edusta**-niminen aliverkon.

## <a name="manage-rules"></a>Sääntöjen hallinta

Voit lisätä sääntöjä aiemmin NSG, muokata aiemmin luotuja sääntöjä ja Poista säännöt.

### <a name="add-a-rule"></a>Lisää sääntö

Lisää sääntö salliminen **Saapuvan** liikenteen porttiin **443** -konetta **NSG edusta** NSG, noudata seuraavia ohjeita.

1. Suorita `Get-AzureRmNetworkSecurityGroup` cmdlet-komento, voit hakea aiemmin NSG ja tallentaa sen muuttujaa alla kuvatulla tavalla.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Suorita `Add-AzureRmNetworkSecurityRuleConfig` cmdlet-komento, alla kuvatulla tavalla.

        Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange * `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Jos haluat tallentaa NSG tehdyt muutokset, suorita `Set-AzureRmNetworkSecurityGroup` cmdlet-komennon alla kuvatulla tavalla.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Odotettu tulos näkyy vain suojaussäännöt:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Säännön muuttaminen

Jos haluat muuttaa luotu edellä sallimaan **Internet** vain saapuvan liikenteen sääntö, noudata seuraavia ohjeita.

1. Suorita `Get-AzureRmNetworkSecurityGroup` cmdlet-komento, voit hakea aiemmin NSG ja tallentaa sen muuttujaa alla kuvatulla tavalla.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Suorita `Set-AzureRmNetworkSecurityRuleConfig` cmdlet-komento, alla kuvatulla tavalla.

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange Internet `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Jos haluat tallentaa NSG tehdyt muutokset, suorita `Set-AzureRmNetworkSecurityGroup` cmdlet-komennon alla kuvatulla tavalla.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Odotettu tulos näkyy vain suojaussäännöt:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Säännön poistaminen

1. Suorita `Get-AzureRmNetworkSecurityGroup` cmdlet-komento, voit hakea aiemmin NSG ja tallentaa sen muuttujaa alla kuvatulla tavalla.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Suorita `Remove-AzureRmNetworkSecurityRuleConfig` cmdlet-komento, alla kuvatulla tavalla.

        Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule

3. Jos haluat tallentaa NSG tehdyt muutokset, suorita `Set-AzureRmNetworkSecurityGroup` cmdlet-komennon alla kuvatulla tavalla.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Odotettu tulos näkyy vain suojaussäännöt **https-sääntö** ei enää näy ilmoitus:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Hallitse yhteydet

Voit liittää NSG aliverkosta, ja NIC. Voit myös dissociate NSG minkä tahansa resurssin, se on liitetty.

### <a name="associate-an-nsg-to-a-nic"></a>Liitä NSG NIC: ssä voit

Liitä **NSG edusta** NSG **TestNICWeb1** NIC: ssä, noudata seuraavia ohjeita.

1. Suorita `Get-AzureRmNetworkSecurityGroup` cmdlet-komento, voit hakea aiemmin NSG ja tallentaa sen muuttujaa alla kuvatulla tavalla.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Suorita `Get-AzureRmNetworkInterface` cmdlet-komento, voit hakea aiemmin NIC ja tallentaa sen muuttujaa alla kuvatulla tavalla.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. **NIC** muuttujan **NetworkSecurityGroup** -ominaisuuden arvoksi **NSG** muuttuja-arvo alla kuvatulla tavalla.

        $nic.NetworkSecurityGroup = $nsg

4. Jos haluat tallentaa Verkkokortti tehdyt muutokset, suorita `Set-AzureRmNetworkInterface` cmdlet-komennon alla kuvatulla tavalla.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Odotettu tulos, jossa **NetworkSecurityGroup** -ominaisuuden:

        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Dissociate NSG-NIC: ssä

Voit dissociate **NSG edusta** - **TestNICWeb1** NIC NSG, noudata seuraavia ohjeita.

1. Suorita `Get-AzureRmNetworkSecurityGroup` cmdlet-komento, voit hakea aiemmin NSG ja tallentaa sen muuttujaa alla kuvatulla tavalla.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Suorita `Get-AzureRmNetworkInterface` cmdlet-komento, voit hakea aiemmin NIC ja tallentaa sen muuttujaa alla kuvatulla tavalla.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. **NIC** muuttujan **NetworkSecurityGroup** -ominaisuuden arvoksi **$null**alla kuvatulla tavalla.

        $nic.NetworkSecurityGroup = $null

4. Jos haluat tallentaa Verkkokortti tehdyt muutokset, suorita `Set-AzureRmNetworkInterface` cmdlet-komennon alla kuvatulla tavalla.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Odotettu tulos, jossa **NetworkSecurityGroup** -ominaisuuden:

        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Dissociate aliverkosta NSG

Voit dissociate **NSG edusta** NSG **edusta** -aliverkosta, noudata seuraavia ohjeita.

1. Suorita `Get-AzureRmVirtualNetwork` cmdlet-komento, voit hakea aiemmin VNet ja tallentaa sen muuttujaa alla kuvatulla tavalla.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Suorita `Get-AzureRmVirtualNetworkSubnetConfig` cmdlet-komento, voit hakea **edusta** -osoitteiden ja tallentaa sen muuttujaa alla kuvatulla tavalla.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

3. **Aliverkon** muuttujan **NetworkSecurityGroup** -ominaisuuden arvoksi **$null**alla kuvatulla tavalla.

        $subnet.NetworkSecurityGroup = $null

4. Jos haluat tallentaa aliverkon tehdyt muutokset, suorita `Set-AzureRmVirtualNetwork` cmdlet-komennon alla kuvatulla tavalla.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Odotettu tulos näkyy vain **edusta** -aliverkon ominaisuudet. Huomaa **NetworkSecurityGroup**ominaisuus ei ole:

            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Liitä NSG aliverkon avulla

Jos haluat liittää **NSG edusta** NSG **FronEnd** aliverkon uudelleen, noudata seuraavia ohjeita.

1. Suorita `Get-AzureRmVirtualNetwork` cmdlet-komento, voit hakea aiemmin VNet ja tallentaa sen muuttujaa alla kuvatulla tavalla.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Suorita `Get-AzureRmVirtualNetworkSubnetConfig` cmdlet-komento, voit hakea **edusta** -osoitteiden ja tallentaa sen muuttujaa alla kuvatulla tavalla.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

1. Suorita `Get-AzureRmNetworkSecurityGroup` cmdlet-komento, voit hakea aiemmin NSG ja tallentaa sen muuttujaa alla kuvatulla tavalla.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

3. **Aliverkon** muuttujan **NetworkSecurityGroup** -ominaisuuden arvoksi **$null**alla kuvatulla tavalla.

        $subnet.NetworkSecurityGroup = $nsg

4. Jos haluat tallentaa aliverkon tehdyt muutokset, suorita `Set-AzureRmVirtualNetwork` cmdlet-komennon alla kuvatulla tavalla.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Odotettu tulos, jossa vain **edusta** -aliverkon **NetworkSecurityGroup** -ominaisuuden:

        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Poista NSG

Voit poistaa NSG vain, jos se ei liity mihin tahansa resurssiin. Jos haluat poistaa NSG, noudata seuraavia ohjeita.

1. Voit tarkistaa NSG liittyvät resurssit, suorita `azure network nsg show` [näkymän NSGs suhteet](#View-NSGs-associations)esitetyllä tavalla.
2. Jos kaikki NIC NSG liitetään, suorita `azure network nic set` esitetyllä tavalla [Dissociate NSG NIC-](#Dissociate-an-NSG-from-a-NIC) varten kunkin NIC-sivustosta. 
3. Jos NSG liitetään minkä tahansa aliverkon, suorita `azure network vnet subnet set` [Dissociate aliverkosta NSG](#Dissociate-an-NSG-from-a-subnet) kunkin aliverkon esitetyllä tavalla.
4. Voit poistaa NSG suorittamalla `Remove-AzureRmNetworkSecurityGroup` cmdlet-komennon alla kuvatulla tavalla.

        Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force

    >[AZURE.NOTE] **-Voimassa** parametri, joka kertoo sinun ei tarvitse Vahvista poisto.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Ota loki käyttöön](virtual-network-nsg-manage-log.md) NSGs.