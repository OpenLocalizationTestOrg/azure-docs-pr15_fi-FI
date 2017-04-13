<properties 
   pageTitle="Hallitse NSGs Azure-CLI resurssien hallinnan avulla | Microsoft Azure"
   description="Opi hallitsemaan aiemmin NSGs Azure-CLI resurssien hallinnan avulla"
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

# <a name="manage-nsgs-using-the-azure-cli"></a>Azure-CLI käyttämällä NSGs hallinta

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönoton mallia.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="retrieve-information"></a>Tietojen hakeminen

Voit tarkastella aiemmin luotuja NSGs, säännöt noutaminen aiemmin NSG ja selvitä, mitä resursseja NSG on liitetty.

### <a name="view-existing-nsgs"></a>Tarkastele aiemmin NSGs

Jos haluat tarkastella tietyn resurssiryhmän NSGs luettelo, suorita `azure network nsg list` -komennon alla kuvatulla tavalla. 

    azure network nsg list --resource-group RG-NSG

Odotettu tulos:

    info:    Executing command network nsg list
    + Getting the network security groups
    data:    Name          Location
    data:    ------------  --------
    data:    NSG-BackEnd   westus
    data:    NSG-FrontEnd  westus
    info:    network nsg list command OK
         
### <a name="list-all-rules-for-an-nsg"></a>NSG on lueteltu kaikki säännöt

Voit tarkastella NSG **NSG edusta**-niminen sääntöjä, suorita `azure network nsg show` -komennon alla kuvatulla tavalla. 

    azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd

Odotettu tulos:
    
    info:    Executing command network nsg show
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Name                            : NSG-FrontEnd
    data:    Type                            : Microsoft.Network/networkSecurityGroups
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    Tags                            : displayName=NSG - Front End
    data:    Security group rules:
    data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
    data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
    data:    rdp-rule                       Internet           *            *               3389              Tcp       Inbound    Allow   100
    data:    web-rule                       Internet           *            *               80                Tcp       Inbound    Allow   101
    data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000
    data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001
    data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500
    data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000
    data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001
    data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500
    info:    network nsg show command OK

>[AZURE.NOTE] Voit myös käyttää `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` **NSG edusta** NSG säännöt-luettelo.

### <a name="view-nsg-associations"></a>Näytä NSG yhteydet

Mitä **NSG edusta** -NSG on resursseja yhdistää katselemista Suorita `azure network nsg show` -komennon alla kuvatulla tavalla. Huomaa, että ainoa ero on **--json** -parametrin.

    azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json

Etsi **networkInterfaces** ja **aliverkosta** ominaisuudet alla kuvatulla tavalla:

    "networkInterfaces": [],
    ...
    "subnets": [
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
        }
    ],
    ...

Yllä olevassa esimerkissä NSG ei ole liitetty verkon käyttöliittymää (NIC) ja se liittyy **edusta**-niminen aliverkon.

## <a name="manage-rules"></a>Sääntöjen hallinta

Voit lisätä sääntöjä aiemmin NSG, muokata aiemmin luotuja sääntöjä ja Poista säännöt.

### <a name="add-a-rule"></a>Lisää sääntö

Lisää sääntö salliminen **Saapuvan** liikenteen porttiin **443** -konetta **NSG edusta** NSG, suorita `azure network nsg rule create` -komennon alla kuvatulla tavalla.

    azure network nsg rule create --resource-group RG-NSG \
        --nsg-name NSG-FrontEnd \
        --name allow-https \
        --description "Allow access to port 443 for HTTPS" \
        --protocol Tcp \
        --source-address-prefix * \
        --source-port-range * \
        --destination-address-prefix * \
        --destination-port-range 443 \
        --access Allow \
        --priority 102 \
        --direction Inbound     

Odotettu tulos:

    info:    Executing command network nsg rule create
    + Looking up the network security rule "allow-https"
    + Creating a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : *
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule create command OK

### <a name="change-a-rule"></a>Säännön muuttaminen

Jos haluat muuttaa luotu edellä sallimaan **Internet** vain saapuvan liikenteen sääntö, suorita `azure network nsg rule set` -komennon alla kuvatulla tavalla.

    azure network nsg rule set --resource-group RG-NSG \
        --nsg-name NSG-FrontEnd \
        --name allow-https \
        --source-address-prefix Internet

Odotettu tulos:

    info:    Executing command network nsg rule set
    + Looking up the network security group "NSG-FrontEnd"
    + Setting a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : Internet
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule set command OK

### <a name="delete-a-rule"></a>Säännön poistaminen

Jos haluat poistaa säännön, joka on luotu edellä, suorita `azure network nsg rule delete` -komennon alla kuvatulla tavalla.

    azure network nsg rule delete --resource-group RG-NSG \
        --nsg-name NSG-FrontEnd \
        --name allow-https \
        --quiet

>[AZURE.NOTE] **--Hiljainen** parametri, joka kertoo sinun ei tarvitse Vahvista poisto.

Odotettu tulos:

    info:    Executing command network nsg rule delete
    + Looking up the network security group "NSG-FrontEnd"
    + Deleting network security rule "allow-https"
    info:    network nsg rule delete command OK

## <a name="manage-associations"></a>Hallitse yhteydet

Voit liittää NSG aliverkosta, ja NIC. Voit myös dissociate NSG minkä tahansa resurssin, se on liitetty.

### <a name="associate-an-nsg-to-a-nic"></a>Liitä NSG NIC: ssä voit

Voit liittää **NSG edusta** NSG **TestNICWeb1** NIC, suorittamalla `azure network nic set` -komennon alla kuvatulla tavalla.

    azure network nic set --resource-group RG-NSG \
        --name TestNICWeb1 \
        --network-security-group-name NSG-FrontEnd

Odotettu tulos:

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Looking up the network security group "NSG-FrontEnd"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-nic"></a>Dissociate NSG-NIC: ssä

Dissociate **NSG edusta** - **TestNICWeb1** NIC NSG suorittamalla `azure network nic set` -komennon alla kuvatulla tavalla.

    azure network nic set --resource-group RG-NSG --name TestNICWeb1 --network-security-group-id ""

>[AZURE.NOTE] Ilmoitus "" (tyhjä) **Verkko-suojaus-ryhmä-id** -parametrin arvon. Tämä johtuu siitä, miten voit poistaa NSG liitosta. Et voi tehdä saman **Verkko-suojaus-ryhmä-name** -parametria.

Odotettu tulos:

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-subnet"></a>Dissociate aliverkosta NSG

Dissociate **NSG edusta** NSG **edusta** -aliverkosta, suorita `azure network vnet subnet set` -komennon alla kuvatulla tavalla.

    azure network vnet subnet set --resource-group RG-NSG \
        --vnet-name TestVNet \
        --name FrontEnd \
        --network-security-group-id ""

Odotettu tulos:

    info:    Executing command network vnet subnet set
    + Looking up the subnet "FrontEnd"
    + Setting subnet "FrontEnd"
    + Looking up the subnet "FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:    Type                            : Microsoft.Network/virtualNetworks/subnets
    data:    ProvisioningState               : Succeeded
    data:    Name                            : FrontEnd
    data:    Address prefix                  : 192.168.1.0/24
    data:    IP configurations:
    data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
    data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
    data:
    info:    network vnet subnet set command OK

### <a name="associate-an-nsg-to-a-subnet"></a>Liitä NSG aliverkon avulla

Jos haluat liittää **NSG edusta** NSG **FronEnd** aliverkon uudelleen, suorita `azure network vnet subnet set` -komennon alla kuvatulla tavalla.

    azure network vnet subnet set --resource-group RG-NSG \
        --vnet-name TestVNet \
        --name FrontEnd \
        --network-security-group-name NSG-FronEnd

>[AZURE.NOTE] Edellä komento toimii vain sillä **NSG edusta** -NSG virtual verkon **TestVNet**samaan resurssiryhmään. Jos NSG on eri resurssiryhmä, sinun on käytettävä **--Verkko-suojaus-ryhmä-tunnus** parametri, ja antaa koko tunnus NSG. Voit hakea tunnuksen suorittamalla **azure verkon nsg Näytä--resurssiryhmä RG-NSG--nimi NSG edusta--json** ja esittää **id** -ominaisuutta. 

Odotettu tulos:

        info:    Executing command network vnet subnet set
        + Looking up the subnet "FrontEnd"
        + Looking up the network security group "NSG-FrontEnd"
        + Setting subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
        data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
        data:
        info:    network vnet subnet set command OK

## <a name="delete-an-nsg"></a>Poista NSG

Voit poistaa NSG vain, jos se ei liity mihin tahansa resurssiin. Jos haluat poistaa NSG, noudata seuraavia ohjeita.

1. Voit tarkistaa NSG liittyvät resurssit, suorita `azure network nsg show` [näkymän NSGs suhteet](#View-NSGs-associations)esitetyllä tavalla.
2. Jos kaikki NIC NSG liitetään, suorita `azure network nic set` esitetyllä tavalla [Dissociate NSG NIC-](#Dissociate-an-NSG-from-a-NIC) varten kunkin NIC-sivustosta. 
3. Jos NSG liitetään minkä tahansa aliverkon, suorita `azure network vnet subnet set` [Dissociate aliverkosta NSG](#Dissociate-an-NSG-from-a-subnet) kunkin aliverkon esitetyllä tavalla.
4. Voit poistaa NSG suorittamalla `azure network nsg delete` -komennon alla kuvatulla tavalla.

        azure network nsg delete --resource-group RG-NSG --name NSG-FrontEnd --quiet

    Odotettu tulos:

        info:    Executing command network nsg delete
        + Looking up the network security group "NSG-FrontEnd"
        + Deleting network security group "NSG-FrontEnd"
        info:    network nsg delete command OK

## <a name="next-steps"></a>Seuraavat vaiheet

- [Ota loki käyttöön](virtual-network-nsg-manage-log.md) NSGs.