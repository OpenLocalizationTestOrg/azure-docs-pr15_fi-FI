<properties 
   pageTitle="Hallita reititys ja käyttää virtual laitteiden käyttämällä Azure-CLI perinteinen käyttöönoton mallin | Microsoft Azure"
   description="Lue, miten voit hallita reitityksestä VNets Azure-CLI käyttäminen perinteinen käyttöönottomalli"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Hallita reititys ja käyttää virtual laitteiden (perinteinen) Azure-CLI käyttäminen

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään perinteinen käyttöönottomalli. Voit myös [ohjausobjektin reititys ja käyttää virtual laitteiden resurssien hallinnan käyttöönottomalli](virtual-network-create-udr-arm-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Esimerkki Azure CLI komentoja alla odottavat yksinkertainen ympäristössä, jossa on jo luotu edellä skenaarion mukaan. Jos haluat suorittaa komentoja, niin kuin ne näkyvät tässä asiakirjassa, Luo näkyvät [luominen (perinteinen) käyttämällä Azure-CLI VNet](virtual-networks-create-vnet-classic-cli.md)ympäristössä.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Luo edusta-aliverkon UDR
Voit luoda reititystaulukon ja reitin tarvittavat edusta-aliverkon yllä skenaarion perusteella, noudattamalla seuraavia ohjeita.

1. Suorita **`azure config mode`** Siirry perinteinen tila.

        azure config mode asm

    Tulos:

        info:    New mode is asm

3. Suorita **`azure network route-table create`** edusta-aliverkon reititystaulukon luominen-komennolla.

        azure network route-table create -n UDR-FrontEnd -l uswest

    Tulos:

        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK

    Parametrit:
    - **-l (tai--sijainti)**. Azure alue, johon uusi NSG luodaan. Tässä tapauksessa *westus*.
    - **-n (tai--nimi)**. Uusi NSG nimi. Tässä skenaariossa *NSG edusta*.

4. Suorita **`azure network route-table route set`** luomalla reitin luotu edellä tarkoitettu uudelleen aliverkon (192.168.2.0/24) ja **FW1** AM (192.168.0.4) kaikki liikenne lähettää reitti-taulukko-komennon.

        azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4

    Tulos:

        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK

    Parametrit:
    - **-r (tai--reitti-taulukko-nimi)**. Jos reitin lisätään reititystaulukon nimi. Tässä tapauksessa *UDR FrontEnd*.
    - **-(tai--osoitteen etuliite)**. Osoitteen etuliite aliverkon, jos paketit on tarkoitettu. Tässä tapauksessa *192.168.2.0/24*.
    - **-t (tai--seuraava-siirtojen-tyypin)**. Objektin verkkoliikenteen tyypin lähetetään. Mahdolliset arvot ovat *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*tai *ei mitään*.
    - **-p (tai--seuraava-kohde-ip-osoite**). Seuraavan pisteen IP-osoite. Tässä tapauksessa *192.168.0.4*.

5. Suorita **`azure network vnet subnet route-table add`** liittää luotu edellä **edusta** -aliverkon reitti-taulukko-komennon.

        azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd

    Tulos:

        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK 

    Parametrit:
    - **-t (tai--vnet nimi)**. Jos aliverkon sijaitsee VNet nimi. Tässä tapauksessa *TestVNet*.
    - **- n (tai--aliverkon nimi**. Reititys-taulukkoon lisätään myös aliverkon nimi. Tässä tapauksessa *FrontEnd*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Luo uudelleen aliverkon UDR
Voit luoda reititystaulukon ja reitin perusteella yllä skenaarion uudelleen aliverkon tarvittavat, noudattamalla seuraavia ohjeita.

3. Suorita **`azure network route-table create`** Luo uudelleen aliverkon reitin taulukko-komennon.

        azure network route-table create -n UDR-BackEnd -l uswest

4. Suorita **`azure network route-table route set`** luomalla reitin luotu edellä tarkoitettu edusta-aliverkon (192.168.1.0/24) **FW1** AM (192.168.0.4) ja kaikki liikenne lähettää reitti-taulukko-komennon.

        azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4

5. Suorita **`azure network vnet subnet route-table add`** liittää luotu edellä **Taustajärjestelmä** aliverkon reitti-taulukko-komennon.

        azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd

