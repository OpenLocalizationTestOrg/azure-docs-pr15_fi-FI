<properties 
   pageTitle="Hallita reititys ja käyttää virtual laitteiden resurssien hallinta PowerShellin avulla | Microsoft Azure"
   description="Lue, miten voit hallita reititys ja käyttää virtual laitteiden resurssien hallinta PowerShellin avulla"
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

#<a name="create-user-defined-routes-udr-in-resource-manager-by-using-powershell"></a>Luo käyttäjän määrittämät tiet (UDR) resurssien hallinta PowerShellin avulla

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään resurssien hallinnan käyttöönottomalli. Voit myös [luoda UDRs perinteinen käyttöönotto-mallissa](virtual-network-create-udr-classic-ps.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Esimerkki PowerShell-komennot alla toiminta yksinkertainen ympäristössä, jossa on jo luotu perusteella yllä skenaario. Jos haluat suorittaa komentoja, niin kuin ne näkyvät tässä asiakirjassa-ensin Muodosta testiympäristössä ottamalla [tätä mallia](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), valitse **käyttöönotto Azure**, korvaa parametrien oletusarvot tarvittaessa ja portaalin ohjeiden avulla.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Luo edusta-aliverkon UDR
Voit luoda reititystaulukon ja reitin tarvittavat edusta-aliverkon yllä skenaarion perusteella, noudattamalla seuraavia ohjeita.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Luo reitti lähettämiseen kaikki liikenne reitittämistä reititetään **FW1** virtual laitteeseen (192.168.0.4) uudelleen aliverkon (192.168.2.0/24).

        $route = New-AzureRmRouteConfig -Name RouteToBackEnd `
            -AddressPrefix 192.168.2.0/24 -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

4. Luo reititystaulukon **UDR FrontEnd** **westus** alueen, jossa on luotu edellä reitin.

        $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
            -Name UDR-FrontEnd -Route $route

5. Luo muuttuja, joka sisältää aliverkon missä VNet. Tässä tilanteessa VNet nimi on **TestVNet**.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet

6. Liitä edellä **edusta** -aliverkon luotu reititystaulukon.
        
        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            -AddressPrefix 192.168.1.0/24 -RouteTable $routeTable

>[AZURE.WARNING] Yllä olevien komentojen tulos näyttää VPN kokoonpano-objekti, joka on käytettävissä vain tietokoneeseen, jossa PowerShell sisällön. Sinun täytyy suorittaa Tallenna asetukset Azure **Määrittäminen AzureVirtualNetwork** cmdlet-komennon.

7. Tallenna uusi aliverkon kokoonpanon Azure-tietokannassa.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Odotettu tulos:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : westus
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState : Succeeded
        Tags              : 
                            Name         Value
                            ===========  =====
                            displayName  VNet 
                            
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                                ...,
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [
                                  {
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConfigurations/ipconfig1"
                                  },
                                  {
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                                  }
                                ],
                                "NetworkSecurityGroup": {
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                },
                                "RouteTable": {
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                                },
                                "ProvisioningState": "Succeeded"
                              },
                                ...
                            ]   

## <a name="create-the-udr-for-the-back-end-subnet"></a>Luo uudelleen aliverkon UDR
Voit luoda reititystaulukon ja reitin perusteella yllä skenaarion uudelleen aliverkon tarvittavat, noudattamalla seuraavia ohjeita.

1. Luo reitti lähettämiseen kaikki liikenne reitittämistä edusta-aliverkon (192.168.1.0/24) reititetään **FW1** virtual laitteeseen (192.168.0.4).

        $route = New-AzureRmRouteConfig -Name RouteToFrontEnd `
            -AddressPrefix 192.168.1.0/24 -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

4. Luo reititystaulukon **uswest** alueen, jossa on luotu edellä reitin **UDR Taustajärjestelmä** .

        $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
            -Name UDR-BackEnd -Route $route

5. Liitä edellä **Taustajärjestelmä** aliverkon luotu reititystaulukon.

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd `
            -AddressPrefix 192.168.2.0/24 -RouteTable $routeTable

7. Tallenna uusi aliverkon kokoonpanon Azure-tietokannassa.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Odotettu tulos:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : westus
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState : Succeeded
        Tags              : 
                            Name         Value
                            ===========  =====
                            displayName  VNet 
                            
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              ...,
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [
                                  {
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL2/ipConfigurations/ipconfig1"
                                  },
                                  {
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL1/ipConfigurations/ipconfig1"
                                  }
                                ],
                                "NetworkSecurityGroup": {
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BacEnd"
                                },
                                "RouteTable": {
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd"
                                },
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="enable-ip-forwarding-on-fw1"></a>IP-välityksen FW1 ottaminen käyttöön
Lähettäminen edelleen käyttää **FW1**NIC IP käyttöön noudattamalla seuraavia ohjeita.

1. Luo muuttuja, joka sisältää FW1 käyttämä NIC asetukset. Tässä skenaariossa Verkkokortti nimeltä **NICFW1**.

        $nicfw1 = Get-AzureRmNetworkInterface -ResourceGroupName TestRG -Name NICFW1

2. IP-edelleenlähetyksen ja Tallenna NIC-asetukset.

        $nicfw1.EnableIPForwarding = 1
        Set-AzureRmNetworkInterface -NetworkInterface $nicfw1

    Odotettu tulos:

        Name                 : NICFW1
        ResourceGroupName    : TestRG
        Location             : westus
        Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1
        Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState    : Succeeded
        Tags                 : 
                               Name         Value                  
                               ===========  =======================
                               displayName  NetworkInterfaces - DMZ
                               
        VirtualMachine       : {
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/FW1"
                               }
        IpConfigurations     : [
                                 {
                                   "Name": "ipconfig1",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1/ipConfigurations/ipconfig1",
                                   "PrivateIpAddress": "192.168.0.4",
                                   "PrivateIpAllocationMethod": "Static",
                                   "Subnet": {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/DMZ"
                                   },
                                   "PublicIpAddress": {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPFW1"
                                   },
                                   "LoadBalancerBackendAddressPools": [],
                                   "LoadBalancerInboundNatRules": [],
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
        DnsSettings          : {
                                 "DnsServers": [],
                                 "AppliedDnsServers": [],
                                 "InternalDnsNameLabel": null,
                                 "InternalFqdn": null
                               }
        EnableIPForwarding   : True
        NetworkSecurityGroup : null
        Primary              : True

