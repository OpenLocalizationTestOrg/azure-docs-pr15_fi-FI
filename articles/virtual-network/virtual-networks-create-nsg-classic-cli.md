<properties
   pageTitle="NSGs luominen käyttämällä Azure-CLI perinteistä | Microsoft Azure"
   description="Lue, miten voit luoda ja ottaa NSGs käyttämällä Azure-CLI perinteinen tila"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
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

# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>Azure-CLI NSGs (perinteinen) luominen

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään perinteinen käyttöönottomalli. Voit myös [luoda NSGs resurssien hallinnan käyttöönotto-mallissa](virtual-networks-create-nsg-arm-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Esimerkki Azure CLI komentoja alla odotettua yksinkertainen ympäristössä, jossa on jo luotu edellä skenaarion mukaan. Jos haluat suorittaa komentoja, niin kuin ne näkyvät tässä asiakirjassa, luoda testiympäristössä luomalla [VNet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Edusta-aliverkon NSG luominen
Luodaksesi NSG nimeltä nimettyä **NSG edusta** yllä skenaarion perusteella, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.

2. Suorita **`azure config mode`** komennolla voit siirtyä perinteinen tila alla kuvatulla tavalla.

        azure config mode asm

    Odotettu tulos:

        info:    New mode is asm

3. Suorita **`azure network nsg create`** luominen NSG-komennolla.

        azure network nsg create -l uswest -n NSG-FrontEnd

    Odotettu tulos:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parametrit:

    - **-l (tai--sijainti)**. Azure alue, johon uusi NSG luodaan. Tässä tapauksessa *westus*.
    - **-n (tai--nimi)**. Uusi NSG nimi. Tässä skenaariossa *NSG edusta*.

4. Suorita **`azure network nsg rule create`** luoda säännön, joka sallii käytön portti 3389 (RDP) Internetistä-komennolla.

        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Odotettu tulos:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Parametrit:

    - **-(tai--nsg nimi)**. Nimi, johon sääntö luodaan NSG. Tässä skenaariossa *NSG edusta*.
    - **-n (tai--nimi)**. Uuden säännön nimi. Tässä skenaariossa *rdp-säännön*.
    - **-c (tai--toiminto)**. Käyttöoikeustaso (estä tai salli).
    - **-p (tai--protocol)**. Protocol (Tcp, Udp- tai *) säännön.
    - **-r (tai--tyyppi)**. Yhteyden (saapuva tai lähtevä) suunta.
    - **-y (tai--prioriteetti)**. Säännön prioriteetti.
    - **-f (tai--lähde-osoite-etuliite)**. Tietolähteen osoitteen etuliite CIDR tai oletusarvo-ohjauskoodien käyttäminen.
    - **+ o (tai--lähde-Port (portti)-alue)**. Lähdeportti tai porttialue.
    - **-e (tai--kohde-osoite-etuliite)**. Kohde osoite etuliite CIDR tai oletusarvo-ohjauskoodien käyttäminen.
    - **-u (tai--kohde-Port (portti)-alue)**. Kohdeportti tai porttialue.

5. Suorita **`azure network nsg rule create`** luoda säännön, joka sallii käytön porttiin 80 (HTTP) Internetistä-komennolla.

        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Odotettu putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Suorita **`azure network nsg subnet add`** NSG linkittäminen edusta-aliverkon-komennolla.

        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd

    Odotettu tulos:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Uudelleen aliverkon NSG luominen
Luodaksesi NSG nimeltä nimettyjä *NSG Taustajärjestelmä* yllä skenaarion perusteella, noudata seuraavia ohjeita.

3. Suorita **`azure network nsg create`** luominen NSG-komennolla.

        azure network nsg create -l uswest -n NSG-BackEnd

    Odotettu tulos:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parametrit:

    - **-l (tai--sijainti)**. Azure alue, johon uusi NSG luodaan. Tässä tapauksessa *westus*.
    - **-n (tai--nimi)**. Uusi NSG nimi. Tässä skenaariossa *NSG edusta*.

4. Suorita **`azure network nsg rule create`** luoda säännön, joka sallii käytön porttia 1433 (SQL) edusta-aliverkosta-komennolla.

        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Odotettu tulos:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK


5. Suorita **`azure network nsg rule create`** luoda säännön, joka estää käytön Internet-komennolla.

        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80

    Odotettu putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Suorita **`azure network nsg subnet add`** NSG linkittäminen uudelleen aliverkon-komennolla.

        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd

    Odotettu tulos:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK
