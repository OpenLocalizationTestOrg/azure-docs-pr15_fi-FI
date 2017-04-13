<properties
   pageTitle="Luo VNet Peering Powershell cmdlet-komentojen käyttäminen | Microsoft Azure"
   description="Opettele luomaan virtual verkon Azure portaalin resurssien hallinnan avulla."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai; annahar"/>

# <a name="create-vnet-peering-using-powershell-cmdlets"></a>Luo VNet Peering käyttämällä Powershell cmdlet-komennot

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Voit luoda VNet, peering PowerShell-toiminnolla, noudata seuraavia ohjeita:

1. Jos et ole aikaisemmin käyttänyt PowerShellin Azure-artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) ja noudata ohjeita päässä Azure Kirjaudu ja valitse tilauksen.

> [AZURE.NOTE] PowerShell cmdlet-komennon hallintaan VNet peering toimitetaan [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Lue VPN-objektit:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. Muodostaa VNet peering sinun on luotava kaksi linkkiä, yksi jokaista suuntaa. Seuraava vaihe luoda VNet peering linkin VNet1 VNet2 ensimmäisen kerran:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

    Tulos näyttää:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Tämä vaihe luo VNet peering linkin VNet2 VNet1:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id

    Tulos näyttää:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Kun VNet peering linkki on luotu, näet alla Linkkitilan:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Tulos näyttää:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Liittyy muutama määritettäviä ominaisuuksia VNet peering:

  	|Vaihtoehto|Kuvaus|Oletusarvo|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Onko verkko-osoitteissa Peer VNet Virtual_network tunnisteen sisällyttää tila|Kyllä|
  	|AllowForwardedTraffic|Onko liikenne ei peräisin peered VNet hyväksytään tai poistetaan|Ei|
  	|AllowGatewayTransit|Vertaisjärjestelmä VNet VNet yhdyskäytävän käyttämään avulla|Ei|
  	|UseRemoteGateways|Käytä omaa peer VNet yhdyskäytävää. Vertaisjärjestelmä VNet on määritetty yhdyskäytävän ja AllowGatewayTransit valittuna. Et voi käyttää tämä vaihtoehto, jos sinulla on määritetty yhdyskäytävän|Ei|

    Kunkin linkkiä VNet peering on yllä ominaisuuksia. Voit esimerkiksi määrittää AllowVirtualNetworkAccess tosi VNet peering linkin VNet1 VNet2 ja määritä sen arvoksi False VNet peering linkin toiseen suuntaan.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    Voit suorittaa Hae AzureRmVirtualNetworkPeering kaksinkertainen Kuittaa ominaisuuden arvon muutoksen jälkeen. Tulosteesta näet AllowForwardedTraffic muuttuu Aseta arvoksi True yllä cmdlet-komennot suorittamisen jälkeen.

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Kun peering on muodostettu tässä skenaariossa voit pitäisi aloittaa virtual minkä tahansa tietokoneesta yhteydet virtual minkä tahansa tietokoneeseen, sekä VNets. Oletusarvon mukaan AllowVirtualNetworkAccess on TOSI ja VNet peering käytön ERISNIMI käyttöoikeusluettelot sallimaan välisen VNets valmistelu. Voit edelleen käyttää verkon ryhmän (NSG) suojaussäännöt estää tietyn aliverkosta tai näennäiskoneiden hieno syyt hallitsemaan access kaksi virtual verkkojen välillä väliset yhteydet.  Lisätietoja NSG sääntöjen luomisesta tutustu tämän [artikkelin](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Voit luoda VNet peering käyttämällä PowerShell-tilauksissa, noudata seuraavia ohjeita:

1. Kirjaudu Azure luottamuksellisten käyttäjän A on tili tilauksen A ja suorita seuraava cmdlet-komento:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    Tämä ei ole tarpeen, peering voi muodostaa, vaikka käyttäjien Nosta yksitellen peering pyynnöt niiden vastaaviin VNets, kunhan pyynnöt vastaa. Muut VNet sellaisten käyttäjä lisääminen paikalliseen VNet käyttäjäksi Tee asennus on helpompaa.

2. Kirjaudu Azure sellaisten käyttäjien-B tilillä tilauksen b ja suorita seuraavat cmdlet-komento:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. Valitse käyttäjän A on kirjautuminen istunnon suorittaa cmdlet-komennon alla:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. Käyttäjä-B kirjautuminen istunnossa suorittaa cmdlet-komennon alla:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Kun peering on muodostettu, virtual-konetta VNet3 pitäisi VNet5 virtual minkä tahansa tietokoneen kanssa kommunikoimiseen.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Tässä skenaariossa voit suorittaa PowerShell cmdlet-komentoja muodostaa VNet peering seuraavia ohjeita.  Sinun täytyy AllowForwardedTraffic-ominaisuuden arvoksi True ja linkittää VNET1 HubVNet, joka sallii saapuvan liikenteen-peering VNet osoitetilaa ulkopuolella.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id

2. Kun peering on muodostettu, voit tämän [artikkelin](virtual-network-create-udr-arm-ps.md) ja määrittää omia reititys (UDR), voit ohjata VNet1 liikenne kautta voit käyttää sen ominaisuuksia virtual laitteen. Kun määrität reitin seuraava osoite, voit määrittää sen peer VNet HubVNet virtual laitteen IP-osoitteeseen. Alla on esimerkki:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Luo VNet, peering perinteinen virtual verkon ja Azure Resurssienhallinta virtual verkossa powershellissä, noudata seuraavia ohjeita:

1. Lue VPN-objektin **VNET1**, Azure Resurssienhallinta virtual verkon seuraavasti:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. Tässä skenaariossa peering VNet muodostaa vain yksi linkki tarvita, erityisesti linkki- **VNET1** **VNET2**. Tämä vaihe edellyttää perinteinen VNet resurssin tunnus. Resurssin ryhmän tunnuksen muoto näyttää seuraavanlaiselta:

        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}

    Muista SubscriptionID, ResourceGroupName ja VirtualNetworkName tilalle haluamasi nimet.

    Tämä onnistuu seuraavasti:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Kerran VNet, peering linkki luodaan, näet Linkkitilan alla tulosteen esitetyllä tavalla:

        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>Poista VNet Peering

1.  Jos haluat poistaa VNet peering, sinun on suoritettava seuraavat cmdlet-komennon:

        Remove-AzureRmVirtualNetworkPeering  

        Remove both links, using the following commands:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Kun haluat poistaa linkin VNET peering, peer Linkkitilan siirtyvät yhteys katkaistu. Tässä tilassa ei voi luoda linkin uudelleen peer Linkkitilan muuttuu Mahdollisuuden löytäjä. On suositeltavaa poistaa sekä linkit, ennen kuin voit luoda uudelleen VNet peering.
