<properties 
   pageTitle="Miten voit luoda NSGs ARM-tilassa Azure-CLI | Microsoft Azure"
   description="Lue, miten voit luoda ja ottaa käyttöön käyttämällä Azure-CLI KÄDESSÄ NSGs"
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

# <a name="how-to-create-nsgs-in-the-azure-cli"></a>Azure-CLI NSGs luominen

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään resurssien hallinnan käyttöönottomalli. Voit myös [luoda NSGs perinteinen käyttöönotto-mallissa](virtual-networks-create-nsg-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Esimerkki Azure CLI komentoja alla odottavat yksinkertainen ympäristössä, jossa on jo luotu edellä skenaarion mukaan. Jos haluat suorittaa komentoja, niin kuin ne näkyvät tässä asiakirjassa, luo ensin testiympäristössä ottamalla [tätä mallia](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), valitse **käyttöönotto Azure**, korvaa parametrien oletusarvot tarvittaessa ja noudata portaalissa.

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Edusta-aliverkon NSG luominen
Luodaksesi NSG nimeltä nimettyjä *NSG edusta* yllä skenaarion perusteella, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.

2. Resurssienhallinta-tilassa, siirry **azure config tila** -komennon suorittaminen alla kuvatulla tavalla.

        azure config mode arm

    Odotettu tulos:

        info:    New mode is arm

3. Voit luoda NSG **azure verkon nsg luominen** -komennon suorittaminen.

        azure network nsg create -g TestRG -l westus -n NSG-FrontEnd

    Odotettu tulos:

        info:    Executing command network nsg create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
        data:    Name                            : NSG-FrontEnd
        data:    Type                            : Microsoft.Network/networkSecurityGroups
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Security group rules:
        data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
        data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
        data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000   
        data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001   
        data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500   
        data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000   
        data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001   
        data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500   
        info:    network nsg create command OK

    Parametrit:
    - **-g (tai--resurssiryhmä)**. Resurssiryhmä, jossa NSG luodaan nimi. Tässä skenaariossa *TestRG*.
    - **-l (tai--sijainti)**. Azure alue, johon uusi NSG luodaan. Tässä tapauksessa *westus*.
    - **-n (tai--nimi)**. Uusi NSG nimi. Tässä skenaariossa *NSG edusta*.

4. Jos haluat luoda säännön, joka sallii käytön portti 3389 (RDP) Internetistä **azure verkon nsg säännön luominen** -komennon suorittaminen.

        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Odotettu tulos:

        info:    Executing command network nsg rule create
        warn:    Using default direction: Inbound
        info:    Looking up the network security rule "rdp-rule"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp
        -rule
        data:    Name                            : rdp-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : Internet
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 3389
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Parametrit:

    - **-(tai--nsg nimi)**. Nimi, johon sääntö luodaan NSG. Tässä skenaariossa *NSG edusta*.
    - **-n (tai--nimi)**. Uuden säännön nimi. Tässä skenaariossa *rdp-säännön*.
    - **-c (tai--access)**. Käyttöoikeustaso (estä tai salli).
    - **-p (tai--protocol)**. Protocol (Tcp, Udp- tai *) säännön.
    - **-r (tai--suunta)**. Yhteyden (saapuva tai lähtevä) suunta.
    - **-y (tai--prioriteetti)**. Säännön prioriteetti.
    - **-f (tai--lähde-osoite-etuliite)**. Tietolähteen osoitteen etuliite CIDR tai oletusarvo-ohjauskoodien käyttäminen.
    - **+ o (tai--lähde-Port (portti)-alue)**. Lähdeportti tai porttialue.
    - **-e (tai--kohde-osoite-etuliite)**. Kohde osoite etuliite CIDR tai oletusarvo-ohjauskoodien käyttäminen.
    - **-u (tai--kohde-Port (portti)-alue)**. Kohdeportti tai porttialue.    

5. Voit luoda säännön, joka sallii käytön porttiin 80 (HTTP) Internetistä **azure verkon nsg säännön luominen** -komennon suorittaminen.

        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Odotettu putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
        data:    Name                            : web-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : Internet
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 80
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Linkki NSG edusta-aliverkon **azure verkon vnet aliverkon set** -komennon suorittaminen

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -o NSG-FrontEnd

    Odotettu tulos:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ip
        Configurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ip
        Configurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Uudelleen aliverkon NSG luominen
Luodaksesi NSG nimeltä nimetty *NSG Taustajärjestelmä* yllä skenaarion perusteella, noudata seuraavia ohjeita.

3. Voit luoda NSG **azure verkon nsg luominen** -komennon suorittaminen.

        azure network nsg create -g TestRG -l westus -n NSG-BackEnd

    Odotettu tulos:

        info:    Executing command network nsg create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd
        data:    Name                            : NSG-BackEnd
        data:    Type                            : Microsoft.Network/networkSecurityGroups
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Security group rules:
        data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
        data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
        data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000   
        data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001   
        data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500   
        data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000   
        data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001   
        data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500   
        info:    network nsg create command OK

4. Voit luoda säännön, joka sallii käytön porttia 1433 (SQL) edusta-aliverkosta **azure verkon nsg säännön luominen** -komennon suorittaminen.

        azure network nsg rule create -g TestRG -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Odotettu tulos:

        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "sql-rule"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule
        data:    Name                            : sql-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 1433
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

5. Voit luoda säännön, joka estää käytön Internetin **azure verkon nsg säännön luominen** -komennon suorittaminen.

        azure network nsg rule create -g TestRG -a NSG-BackEnd -n web-rule -c Deny -p * -r Outbound -y 200 -f * -o * -e Internet -u *

    Odotettu putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd/securityRules/web-rule
        data:    Name                            : web-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : *
        data:    Source Port                     : *
        data:    Destination IP                  : Internet
        data:    Destination Port                : *
        data:    Protocol                        : *
        data:    Direction                       : Outbound
        data:    Access                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Linkki NSG uudelleen aliverkon **azure verkon vnet aliverkon set** -komennon suorittaminen

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -o NSG-BackEnd

    Odotettu tulos:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Setting subnet "BackEnd"
        info:    Looking up the subnet "BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/BackEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : BackEnd
        data:    Address prefix                  : 192.168.2.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICSQL1/ip
        Configurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICSQL2/ip
        Configurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
