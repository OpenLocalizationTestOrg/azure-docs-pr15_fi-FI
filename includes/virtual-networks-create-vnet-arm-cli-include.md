## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>VNet, käyttämällä Azure-CLI luominen

Azure-CLI avulla voit hallita Azure resursseja komentoriviltä, miltä tahansa tietokoneelta, joka on käynnissä Windows, Linux tai OS x. Voit luoda VNet Azure-CLI, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt Azure-CLI-kohdassa [Asenna ja Määritä Azure-CLI](../articles/xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.
2. Resurssienhallinta-tilassa, siirry **azure config tila** -komennon suorittaminen alla kuvatulla tavalla.

        azure config mode arm

    Näin Odotettu tulos yllä-komennon:

        info:    New mode is arm

3. Valitse tarvittaessa suorittaa **azure ryhmän luominen** Luo uusi resurssiryhmä alla kuvatulla tavalla. Huomaa komennon. Luettelon, kun tulos kerrotaan käytetyt parametrit. Lisätietoja resurssiryhmät käy [Azure resurssin hallinnassa: yleiskatsaus](../articles/virtual-network/resource-group-overview.md#resource-groups).

        azure group create -n TestRG -l centralus

    Näin Odotettu tulos yllä-komennon:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (tai--nimi)**. Uusi resurssiryhmä nimi. Tässä skenaariossa *TestRG*.
    - **-l (tai--sijainti)**. Azure alue, johon uusi resurssiryhmä luodaan. Tässä tapauksessa *centralus*.

4. Voit luoda VNet ja aliverkon, **azure verkon vnet luominen** -komennon suorittaminen alla kuvatulla tavalla. 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    Näin Odotettu tulos yllä-komennon:

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **-g (tai--resurssiryhmä)**. Resurssiryhmä, jossa VNet luodaan nimi. Tässä skenaariossa *TestRG*.
    - **-n (tai--nimi)**. Luoda VNet nimi. Tässä skenaariossa *TestVNet*
    - **-(tai--osoitteiden etuliitteiden)**. Luettelo CIDR lohkot käytetään VNet-osoitetilaa varten. Tässä skenaariossa *192.168.0.0/16*
    - **-l (tai--sijainti)**. Azure alue, jossa VNet luodaan. Tässä tapauksessa *centralus*.

5. Suorita **azure verkon vnet aliverkon luominen** -komento luo aliverkon alla kuvatulla tavalla. Huomaa komennon. Luettelon, kun tulos kerrotaan käytetyt parametrit.

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    Näin Odotettu tulos yllä-komennon:

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **-e (tai--vnet nimi**. Jos aliverkon luodaan VNet nimi. Tässä tapauksessa *TestVNet*.
    - **-n (tai--nimi)**. Uusi aliverkon nimi. Tässä tapauksessa *FrontEnd*.
    - **-(tai--osoitteen etuliite)**. Aliverkon CIDR estä. Neljä Microsoftin tapaus, *192.168.1.0/24*.

6. Toista vaihe 5 Edellä Luo muita aliverkosta, tarvittaessa. Tässä skenaariossa Suorita-komento alla *Taustajärjestelmä* aliverkon luomiseen.

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. Uusi vnet ominaisuuksien tarkasteleminen **azure verkon vnet Näytä** -komennon suorittaminen alla kuvatulla tavalla.

        azure network vnet show -g TestRG -n TestVNet

    Näin Odotettu tulos yllä-komennon:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
