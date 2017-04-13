<properties
   pageTitle="Internet-perinteinen käyttöönoton mallissa Azure-CLI kuormituksen vastakkaisten luomisesta | Microsoft Azure"
   description="Opettele luomaan Internet-perinteinen käyttöönoton mallissa Azure-CLI kuormituksen vastakkaisten"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a>Luomisesta selainpohjainen vastakkaisten Azure-CLI kuormituksen (perinteinen)

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään perinteinen käyttöönottomalli. Voit myös [luominen selainpohjainen vastakkaisten kuormituksen Azure resurssien hallinnan avulla](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Selainpohjainen vastakkaisten käyttämällä CLI kuormituksen Step by step luominen

Tässä oppaassa kerrotaan, miten Luo Internet-kuormituksen, yllä skenaarion perusteella.

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../../articles/xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.

2. Perinteinen tila, siirry **azure config tila** -komennon suorittaminen alla kuvatulla tavalla.

        azure config mode asm

    Odotettu tulos:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Luo päätepiste ja lataa tasaustoiminto

Skenaario olettaa näennäiskoneiden "web1" ja "web2" on luotu.
Tämän oppaan Luo portti 80 julkisen porttina ja porttia 80 paikallisen porttina kuormituksen tasauspalvelun joukosta. Näytteenottimen portin myös on määritetty porttiin 80 ja nimeltä kuormituksen tasauspalvelun määrittäminen "lbset".


### <a name="step-1"></a>Vaihe 1

Ensimmäisen päätepisteen luominen ja määrittäminen käyttämällä kuormituksen `azure network vm endpoint create` virtuaalikoneen "web1" varten.

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset

Parametrit, joita käytetään:

**-k** - virtual Paikallinen kone-portin<br>
**+ o** - protokolla<BR>
**-t** - näytteenottimen portti<BR>
**-b** - kuormituksen tasauspalvelun nimi<BR>

## <a name="step-2"></a>Vaihe 2

Lisää toinen virtual machine "web2" kuormituksen tasauspalvelun määrittäminen.

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## <a name="step-3"></a>Vaihe 3

Tarkista kuormituksen tasauspalvelun määritysten käyttäminen `azure vm show` .

    azure vm show web1

Tulos on:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Luo remote työpöydän päätepiste virtual tietokoneelle

Voit luoda remote työpöydän päätepisteen välittäminen verkkoliikennettä Julkinen portti paikallista porttia tietyn virtuaalikoneen käyttämällä `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Poista virtuaalikoneen kuormituksen

Sinun tulee poistaa liittyvät virtuaalikoneen määrittää kuormituksen päätepiste. Päätepisteen poistetaan, kun virtuaalikoneen ei kuulu enää määrittää kuormituksen.

 Käytä edellä olevaa esimerkkiä, voit poistaa luotu virtuaalikoneen "web1" päätepisteen kuormituksen "lbset"-komennon avulla `azure vm endpoint delete`.

    azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE] Lisää asetuksia voit hallita päätepisteet-komennon avulla voit selata`azure vm endpoint --help`


## <a name="next-steps"></a>Seuraavat vaiheet

[Aloita sisäisten kuormituksen määrittäminen](load-balancer-get-started-ilb-arm-ps.md)

[Kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)

