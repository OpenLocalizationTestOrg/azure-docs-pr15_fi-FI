## <a name="how-to-create-a-vnet-using-powershell"></a>Voit luoda VNet, PowerShellin avulla
Luodaksesi VNet PowerShell-toiminnolla, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt PowerShellin Azure-artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../articles/powershell-install-configure.md) ja noudata ohjeita päässä Azure Kirjaudu ja valitse tilauksen.
    
2. Valitse tarvittaessa luoda uusi resurssiryhmä alla kuvatulla tavalla. Luo nimetty *TestRG*resurssiryhmä skenaariossa. Lisätietoja resurssiryhmät käy [Azure resurssin hallinnassa: yleiskatsaus](../articles/resource-group-overview.md).

        New-AzureRmResourceGroup -Name TestRG -Location centralus

    Odotettu tulos:
    
        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG   

3. Luo uusi VNet, nimeltä *TestVNet*alla kuvatulla tavalla.

        New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
            -AddressPrefix 192.168.0.0/16 -Location centralus   
        
    Odotettu tulos:

        Name                : TestVNet
        ResourceGroupName   : TestRG
        Location            : centralus
        Id                  : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState       : Succeeded
        Tags                    : 
        AddressSpace            : {
                                   "AddressPrefixes": [
                                     "192.168.0.0/16"
                                   ]
                                 }
        DhcpOptions             : {}
        Subnets                 : []
        VirtualNetworkPeerings  : []

4. Tallenna VPN-objektin muuttujaan, alla kuvatulla tavalla.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    
    >[AZURE.TIP] Voit yhdistää suorittamalla vaiheet 3 ja 4 **$vnet = uusi AzureRmVirtualNetwork - ResourceGroupName TestRG-nimen TestVNet - AddressPrefix 192.168.0.0/16-sijaintiin centralus**.

5. Lisää aliverkon VNet uuden muuttuja, alla kuvatulla tavalla.

        Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
            -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
        
    Odotettu tulos:

        Name                : TestVNet
        ResourceGroupName   : TestRG
        Location            : centralus
        Id                  : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState   : Succeeded
        Tags                :
        AddressSpace        : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions         : {}
        Subnets             : [
                                  {
                                    "Name": "FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24"
                                  }
                                ]
        VirtualNetworkPeerings  : []

6. Toista vaihe 5 Edellä kunkin aliverkon, jolle haluat luoda. Alla oleva komento luo *Taustajärjestelmä* aliverkon Microsoftin tapauksessa.

        Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
            -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24

7. Vaikka voit luoda aliverkosta, ne tällä hetkellä vain ovat paikallista muuttujaa avulla voit luoda vaiheessa 4 VNet noudetaan. Tallenna muutokset Azure-Suorita **Määrittäminen AzureRmVirtualNetwork** cmdlet-komennon alla kuvatulla tavalla.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet 
        
    Odotettu tulos:

        Name                : TestVNet
        ResourceGroupName   : TestRG
        Location            : centralus
        Id                  : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState   : Succeeded
        Tags                :
        AddressSpace        : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions         : {
                                  "DnsServers": []
                                }
        Subnets             : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  },
                                  {
                                    "Name": "BackEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                    "AddressPrefix": "192.168.2.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  }
                                ]
        VirtualNetworkPeerings : []
