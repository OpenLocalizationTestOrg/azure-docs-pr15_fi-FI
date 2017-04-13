## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>ARM-mallin asentavat Azure-CLI

Käyttöönotto lataamasi käyttämällä Azure CLI ARM-malli, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../articles/xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.
2. Suorita **`azure config mode`** komento siirtyy Resurssienhallinta-tilaan, alla kuvatulla tavalla.

        azure config mode arm

    Näin Odotettu tulos yllä-komennon:

        info:    New mode is arm

3. Valitse tarvittaessa Suorita **`azure group create`** Luo uusi resurssiryhmä alla kuvatulla tavalla. Huomaa komennon. Luettelon, kun tulos kerrotaan käytetyt parametrit. Lisätietoja resurssiryhmät käy [Azure resurssin hallinnassa: yleiskatsaus](../articles/resource-group-overview.md).

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

4. Suorita **`azure group deployment create`** cmdlet-komento, ottaa käyttöön uuden VNet käyttämällä mallia ja parametri tiedostoja ladataan ja muokata yläpuolella. Luettelon, kun tulos kerrotaan käytetyt parametrit.

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    Näin Odotettu tulos yllä-komennon:

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **-g (tai--resurssiryhmä)**. Resurssiryhmä uusi VNet nimi luodaan.
    - **-f (tai--mallia-tiedosto)**. ARM-mallitiedoston polku.
    - **-e (tai--parametrit-tiedosto)**. ARM-parametrit-tiedoston polku.

5. Suorita **`azure network vnet show`** komento uusi vnet ominaisuuksien tarkasteleminen alla kuvatulla tavalla.

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
