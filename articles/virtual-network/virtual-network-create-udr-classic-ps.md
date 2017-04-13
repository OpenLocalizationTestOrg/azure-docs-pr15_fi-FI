<properties 
   pageTitle="Hallita reititys ja käyttää virtual laitteiden PowerShellin perinteinen käyttöönoton mallin | Microsoft Azure"
   description="Lue, miten voit hallita reitityksestä VNets perinteinen käyttöönoton mallin PowerShellin avulla"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Hallita reititys ja käyttää virtual laitteiden (perinteinen) PowerShellin avulla

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään perinteinen käyttöönottomalli.

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Otosten PowerShellin Azure-komennot alla toiminta yksinkertainen ympäristössä, jossa on jo luotu edellä skenaarion perusteella. Jos haluat suorittaa komentoja, niin kuin ne näkyvät tässä asiakirjassa, Luo näkyvät [luominen (perinteinen) PowerShellin avulla VNet](virtual-networks-create-vnet-classic-netcfg-ps.md)ympäristössä.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Luo edusta-aliverkon UDR
Voit luoda reititystaulukon ja reitin tarvittavat edusta-aliverkon yllä skenaarion perusteella, noudattamalla seuraavia ohjeita.

3. Suorita **`New-AzureRouteTable`** cmdlet-komento, edusta-aliverkon reititystaulukon luomiseen.

        New-AzureRouteTable -Name UDR-FrontEnd `
            -Location uswest `
            -Label "Route table for front end subnet"

    Tulos:

        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet

4. Suorita **`Set-AzureRoute`** cmdlet-komento, Luo reitti reititys-taulukossa luotu edellä kaikki liikenteen reitittämistä uudelleen aliverkon (192.168.2.0/24) ja **FW1** AM (192.168.0.4).
    
        Get-AzureRouteTable UDR-FrontEnd `
            |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

    Tulos:

        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  

5. Suorita **`Set-AzureSubnetRouteTable`** cmdlet-komento, Liitä reitti-taulukossa luotu edellä **edusta** -aliverkon.

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Luo uudelleen aliverkon UDR
Voit luoda reititystaulukon ja reitin perusteella yllä skenaarion uudelleen aliverkon tarvittavat, noudattamalla seuraavia ohjeita.

3. Suorita **`New-AzureRouteTable`** cmdlet-komento, luo uudelleen aliverkon reitin taulukko.

        New-AzureRouteTable -Name UDR-BackEnd `
            -Location uswest `
            -Label "Route table for back end subnet"

4. Suorita **`Set-AzureRoute`** cmdlet-komento, Luo reitti reititys-taulukossa luotu edellä kaikki liikenteen reitittämistä edusta-aliverkon (192.168.1.0/24) ja **FW1** AM (192.168.0.4).

        Get-AzureRouteTable UDR-BackEnd `
            |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

5. Suorita **`Set-AzureSubnetRouteTable`** cmdlet-komento, Liitä reitti-taulukossa luotu edellä **Taustajärjestelmä** aliverkon.

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName BackEnd `
            -RouteTableName UDR-BackEnd

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>IP-välityksen FW1 AM ottaminen käyttöön
IP FW1 AM edelleenlähetyksen käyttöön noudattamalla seuraavia ohjeita.

1. Suorita **`Get-AzureIPForwarding`** cmdlet Tarkista IP-välityksen tila

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Get-AzureIPForwarding

    Tulos:

        Disabled

2. Suorita **`Set-AzureIPForwarding`** voi *FW1* AM edelleenlähetyksen IP-komento.

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Set-AzureIPForwarding -Enable
