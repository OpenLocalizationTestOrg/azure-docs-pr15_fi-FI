<properties 
   pageTitle="Voit määrittää perinteistä ausing staattinen yksityinen IP CLI | Microsoft Azure"
   description="Tietoja staattinen yksityinen IP-osoitteet (tapahtuneen) ja niiden hallinnassa perinteinen tila CLI käyttäminen"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-azure-cli"></a>Staattinen yksityisen IP-osoitteen määrittämisestä (perinteinen)-Azure CLI

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään perinteinen käyttöönottomalli. Voit myös [hallita staattinen yksityinen IP-osoite-resurssien hallinnan käyttöönottomalli](virtual-networks-static-private-ip-arm-cli.md).

Esimerkki Azure CLI komentoja alla odottavat yksinkertainen ympäristössä, jossa on jo luotu. Jos haluat suorittaa komentoja, niin kuin ne näkyvät tässä asiakirjassa, luo ensin kuvattu [luominen vnet](virtual-networks-create-vnet-classic-cli.md)testiympäristössä.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Määrittäminen staattinen yksityinen IP-osoite AM luotaessa
Jos haluat luoda uuden AM, nimeltä *DNS01* nimeltä *TestService* yllä skenaarion perusteella uuden pilvipalvelussa, toimi seuraavasti:

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.
1. Voit luoda pilvipalvelussa **azure-palvelu luoda** -komennon suorittaminen.

        azure service create TestService --location uscentral

    Odotettu tulos:

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
    
2. Voit luoda AM **azure luominen AM** -komennon suorittaminen. Huomaa staattinen yksityinen IP-osoite-arvo. Luettelon, kun tulos kerrotaan käytetyt parametrit.

        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd

    Odotettu tulos:

        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK

    - **-l (tai--sijainti)**. Azure alue, jossa AM luodaan. Tässä tapauksessa *centralus*.
    - **-n (tai--AM nimi)**. Luoda AM nimi.
    - **w (tai--virtual verkko-nimi)**. VNet, jossa AM luodaan nimi. 
    - **-S (tai – staattinen ip)**. Staattinen yksityinen IP-osoite AM.
    - **TestService**. Pilvipalvelussa, jossa AM luodaan nimi.
    - **bd507d3a70934695bc2128e3e5a255ba__RightImage Windows-2012R2-x64-v14.2**. Kuva AM luomiseen käytetyt.
    - **adminuser**. Windows-AM paikallisen järjestelmänvalvojan.
    - **AdminP@ssw0rd**. Windows-AM paikallisen järjestelmänvalvojan salasanan.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Voit noutaa staattisen yksityinen IP-osoitetiedot AM varten
Staattinen yksityinen IP-osoitetiedot, joka on luotu käyttämällä yllä komentosarja AM katselemista varten suorittamalla seuraavan komennon Azure CLI ja noudata arvon *Verkon StaticIP*varten:

    azure vm static-ip show DNS01

Odotettu tulos:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Staattinen yksityinen IP-osoite poistamisesta AM
Voit poistaa staattinen yksityinen IP-osoite lisätään edellä komentosarjan AM varten suorittamalla seuraavan komennon Azure CLI:
    
    azure vm static-ip remove DNS01

Odotettu tulos:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Staattinen yksityinen IP lisääminen aiemmin luotuun AM
Voit lisätä staattisen yksityinen IP-osoite, luotu edellä runt komentosarja AM seuraavan komennon:

    azure vm static-ip set DNS01 192.168.1.101

Odotettu tulos:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [varattu julkiseen IP](virtual-networks-reserved-public-ip.md) -osoitteet.
- Lisätietoja [esiintymän tason julkiseen IP (ILPIP)](virtual-networks-instance-level-public-ip.md) -osoitteista.
- Tietojen kysyminen [varattu IP REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).
