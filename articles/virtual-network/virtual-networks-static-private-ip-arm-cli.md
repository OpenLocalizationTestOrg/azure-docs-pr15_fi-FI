<properties 
   pageTitle="Staattinen yksityinen IP määrittämisestä ARM-tilassa CLI | Microsoft Azure"
   description="Tietoja staattinen IP-osoitteet (tapahtuneen) ja kuinka voit hallita niiden ARM-tilassa CLI"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-azure-cli"></a>Staattinen yksityisen IP-osoitteen määrittämisestä Azure CLI

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään resurssien hallinnan käyttöönottomalli. Voit myös [hallita staattinen yksityinen IP-osoite perinteinen käyttöönotto-mallissa](virtual-networks-static-private-ip-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Esimerkki Azure CLI komentoja alla odottavat yksinkertainen ympäristössä, jossa on jo luotu. Jos haluat suorittaa komentoja, niin kuin ne näkyvät tässä asiakirjassa, luo ensin kuvattu [luominen vnet](virtual-networks-create-vnet-arm-cli.md)testiympäristössä.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Määrittäminen staattinen yksityinen IP-osoite AM luotaessa
Luo AM, nimeltä *DNS01* VNet, nimeltä *TestVNet* staattinen yksityinen IP *192.168.1.101*, jossa *edusta* -aliverkon, noudata seuraavia ohjeita:

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.

2. Resurssienhallinta-tilassa, siirry **azure config tila** -komennon suorittaminen alla kuvatulla tavalla.

        azure config mode arm

    Odotettu tulos:

        info:    New mode is arm

3. Suorita **azure verkon julkiseen ip - Luo** luodaksesi julkiseen IP AM. Luettelon, kun tulos kerrotaan käytetyt parametrit.

        azure network public-ip create -g TestRG -n TestPIP -l centralus
    
    Odotettu tulos:

        info:    Executing command network public-ip create
        + Looking up the public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up the public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK

    - **-g (tai--resurssiryhmä)**. Julkiseen IP luodaan resurssiryhmän nimi.
    - **-n (tai--nimi)**. Julkiseen IP nimi.
    - **-l (tai--sijainti)**. Azure alue, jossa julkinen IP luodaan. Tässä tapauksessa *centralus*.

3. Luo NIC staattinen yksityinen IP **luominen azure verkon nic** -komennon suorittaminen. Luettelon, kun tulos kerrotaan käytetyt parametrit.

        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd

    Odotettu tulos:

        + Looking up the network interface "TestNIC"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up the network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

    - **-(tai--yksityinen-ip-osoite)**. Staattinen NIC-sivustosta. yksityinen IP-osoite
    - **m (tai--aliverkon vnet-nimi)**. Jos Verkkokortti luodaan VNet nimi.
    - **-k (tai--aliverkon nimi)**. Jos Verkkokortti luodaan aliverkon nimi.

4. Voit luoda julkisen IP ja NIC: ssä luotu edellä AM **azure AM luominen** -komennon suorittaminen. Luettelon, kun tulos kerrotaan käytetyt parametrit.

        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd

    Odotettu tulos:

        info:    Executing command vm create
        + Looking up the VM "DNS01"
        info:    Using the VM Size "Standard_A1"
        warn:    The image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account vnetstorage
        + Looking up the NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "TestNIC"
        info:    Found public ip parameters, trying to setup PublicIP profile
        + Looking up the public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up the NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK

    - **-y (tai--os tyyppi)**. AM-käyttöjärjestelmän tyypin *Windows* - tai *Linux*.
    - **-f (tai--nic-nimi)**. Käyttää NIC AM nimi.
    - **-i (tai--julkiseen ip-nimi)**. Julkiseen IP AM käyttää nimi.
    - **-F (tai--vnet nimi)**. VNet, jossa AM luodaan nimi.
    - **-j (tai--vnet-aliverkon-nimi)**. Missä AM luodaan aliverkon nimi.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Voit noutaa staattisen yksityinen IP-osoitetiedot AM varten

Staattinen yksityinen IP-osoitetiedot, joka on luotu käyttämällä yllä komentosarja AM katselemista varten suorittamalla seuraavan komennon Azure CLI ja noudata arvot *yksityinen IP varata-menetelmä* ja *yksityinen IP-osoite*:

    azure vm show -g TestRG -n DNS01

Odotettu tulos:

    info:    Executing command vm show
    + Looking up the VM "DNS01"
    + Looking up the NIC "TestNIC"
    + Looking up the public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Staattinen yksityinen IP-osoite poistamisesta AM
Staattinen yksityinen IP-osoite ei voi poistaa NIC-Azure CLI resurssien hallinta. Luo uusi NIC, joka käyttää dynaaminen IP, poistaa edellisen NIC AM ja lisää sitten uusi NIC AM. Jos haluat muuttaa jota käytetään AM int eh varten NIC yllä olevien komentojen, noudata seuraavia ohjeita.
    
1. Voit luoda uuden NIC, käyttämällä dynaamisia IP-kohdistus **luominen azure verkon nic** -komennon suorittaminen. Huomaa, miten sinun ei tarvitse määrittää IP-osoitteen tällä hetkellä.

        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd

    Odotettu tulos:

        info:    Executing command network nic create
        + Looking up the network interface "TestNIC2"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up the network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

2. Voit muuttaa AM käyttämä NIC **azure AM set** -komennon suorittaminen

        azure vm set -g TestRG -n DNS01 -N TestNIC2

    Odotettu tulos:

        info:    Executing command vm set
        + Looking up the VM "DNS01"
        + Looking up the NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK

3. Voit poistaa vanhan NIC-sivustosta. **azure verkon nic Poista** -komennon suorittaminen, jos

        azure network nic delete -g TestRG -n TestNIC --quiet

    Odotettu tulos:

        info:    Executing command network nic delete
        + Looking up the network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Staattinen yksityisen IP-osoitteen lisääminen aiemmin luotuun AM
Voit lisätä edellä komentosarja-sovelluksella luodut AM käyttämä NIC staattinen yksityinen IP-osoite, suorita seuraava komento:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Odotettu tulos:

    info:    Executing command network nic set
    + Looking up the network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up the network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [varattu julkiseen IP](virtual-networks-reserved-public-ip.md) -osoitteet.
- Lisätietoja [esiintymän tason julkiseen IP (ILPIP)](virtual-networks-instance-level-public-ip.md) -osoitteista.
- Tietojen kysyminen [varattu IP REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).
