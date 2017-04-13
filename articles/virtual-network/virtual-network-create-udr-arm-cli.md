<properties 
   pageTitle="Hallita reititys ja virtual laitteiden-resurssien hallinnan käyttäminen Azure-CLI | Microsoft Azure"
   description="Lue, miten voit hallita reititys ja käyttää virtual laitteiden käyttämällä Azure-CLI"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-the-azure-cli"></a>Luo käyttäjän määrittämä tiet (UDR) Azure CLI

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään resurssien hallinnan käyttöönottomalli. Voit myös [luoda UDRs perinteinen käyttöönotto-mallissa](virtual-network-create-udr-classic-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Esimerkki Azure CLI komentoja alla odottavat yksinkertainen ympäristössä, jossa on jo luotu edellä skenaarion mukaan. Jos haluat suorittaa komentoja, niin kuin ne näkyvät tässä asiakirjassa-ensin Muodosta testiympäristössä ottamalla [tätä mallia](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), valitse **käyttöönotto Azure**, korvaa parametrien oletusarvot tarvittaessa ja portaalin ohjeiden avulla.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Luo edusta-aliverkon UDR
Voit luoda reititystaulukon ja reitin tarvittavat edusta-aliverkon yllä skenaarion perusteella, noudattamalla seuraavia ohjeita.

3. Suorita **`azure network route-table create`** edusta-aliverkon reititystaulukon luominen-komennolla.

        azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest

    Tulos:

        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK

    Parametrit:
    - **-g (tai--resurssiryhmä)**. Resurssiryhmä, jossa UDR luodaan nimi. Tässä tapauksessa *TestRG*.
    - **-l (tai--sijainti)**. Azure alue, johon uusi UDR luodaan. Tässä tapauksessa *westus*.
    - **-n (tai--nimi)**. Uusi UDR nimi. Tässä tapauksessa *UDR FrontEnd*.

4. Suorita **`azure network route-table route create`** luomalla reitin luotu edellä tarkoitettu uudelleen aliverkon (192.168.2.0/24) ja **FW1** AM (192.168.0.4) kaikki liikenne lähettää reitti-taulukko-komennon.

        azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4

    Tulos:

        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK

    Parametrit:
    - **-r (tai--reitti-taulukko-nimi)**. Jos reitin lisätään reititystaulukon nimi. Tässä tapauksessa *UDR FrontEnd*.
    - **-(tai--osoitteen etuliite)**. Osoitteen etuliite aliverkon, jos paketit on tarkoitettu. Tässä tapauksessa *192.168.2.0/24*.
    - **-y (tai--seuraava-siirtojen-tyypin)**. Objektin verkkoliikenteen tyypin lähetetään. Mahdolliset arvot ovat *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*tai *ei mitään*.
    - **-p (tai--seuraava-kohde-ip-osoite**). Seuraavan pisteen IP-osoite. Tässä tapauksessa *192.168.0.4*.

5. Suorita **`azure network vnet subnet set`** liittää luotu edellä **edusta** -aliverkon reitti-taulukko-komennon.

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd

    Tulos:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

    Parametrit:
    - **-e (tai--vnet nimi)**. Jos aliverkon sijaitsee VNet nimi. Tässä tapauksessa *TestVNet*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Luo uudelleen aliverkon UDR
Voit luoda reititystaulukon ja reitin perusteella yllä skenaarion uudelleen aliverkon tarvittavat, noudattamalla seuraavia ohjeita.

1. Suorita **`azure network route-table create`** Luo uudelleen aliverkon reitin taulukko-komennon.

        azure network route-table create -g TestRG -n UDR-BackEnd -l westus

4. Suorita **`azure network route-table route create`** luomalla reitin luotu edellä tarkoitettu edusta-aliverkon (192.168.1.0/24) **FW1** AM (192.168.0.4) ja kaikki liikenne lähettää reitti-taulukko-komennon.

        azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4

5. Suorita **`azure network vnet subnet set`** liittää luotu edellä **Taustajärjestelmä** aliverkon reitti-taulukko-komennon.

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd

## <a name="enable-ip-forwarding-on-fw1"></a>IP-välityksen FW1 ottaminen käyttöön
Lähettäminen edelleen käyttää **FW1**NIC IP käyttöön noudattamalla seuraavia ohjeita.

1. Suorita **`azure network nic show`** komento, niin huomaat, **Ota käyttöön IP-osoitteen välityksen**arvo. Sen pitäisi olla määritetty *Epätosi*.

        azure network nic show -g TestRG -n NICFW1

    Tulos:
        
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK

2. Suorita **`azure network nic set`** IP-edelleenlähetyksen-komennolla.

        azure network nic set -g TestRG -n NICFW1 -f true

    Tulos:

        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK

    Parametrit:

    - **-f (tai – ota käyttöön-ip-edelleenlähetys)**. *Tosi* tai *Epätosi*.
