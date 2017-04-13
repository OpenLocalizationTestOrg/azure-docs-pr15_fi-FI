<properties
   pageTitle="Luo sisäinen kuormituksen, käyttämällä Azure-CLI perinteinen käyttöönoton mallin | Microsoft Azure"
   description="Sisäinen kuormituksen, käyttämällä Azure-CLI perinteinen käyttöönotto-mallin luominen"
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

# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Luomisesta sisäinen kuormituksen (perinteinen) Azure-CLI käyttäminen

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Lue, miten [Resurssienhallinta-mallin seuraavasti](load-balancer-get-started-ilb-arm-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>Luo sisäinen kuormituksen näennäiskoneiden määrittäminen

Sisäinen kuormituksen tasauspalvelun määrittää siihen ja palvelimissa, jotka lähetetään tietoliikenteen siihen luomiseen toimi seuraavasti:

1. Luo sisäinen ladata tasaamisen, jotka ovat saapuvan liikenteen on kuormitus tasataan palvelinten kuormituksen joukon päätepisteen esiintymä.

1. Lisää vastaavat näennäiskoneiden, jotka vastaanottaa saapuvan liikenteen päätepisteet.

1. Määritä palvelimet, joilla lähettäminen on kuormitus niiden liikenne lähettää virtual IP (VIP) osoitteeseen sisäinen ladata tasaamisen esiintymän tasataan liikenne.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Vaihe vaiheelta luominen käyttämällä CLI sisäinen kuormituksen

Tässä oppaassa esitetään, kuinka voit luoda yllä skenaarion perusteella sisäinen kuormituksen.

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../../articles/xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.

2. Perinteinen tila, siirry **azure config tila** -komennon suorittaminen alla kuvatulla tavalla.

        azure config mode asm

    Odotettu tulos:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Luo päätepiste ja lataa tasaustoiminto

Skenaario olettaa näennäiskoneiden "MJP1" ja "DB2" nimi "mytestcloud" pilvipalvelussa. Molemmat näennäiskoneiden on käytössä virtual verkon kutsutaan kanssa aliverkon "aliverkon-1" Oma "testvnet".

Tämän oppaan Luo sisäinen kuormituksen tasauspalvelun-määrittää siihen käyttämällä porttia 1433 yksityinen porttina ja 1433 paikallisen porttina.

Tämä on käytetty vaihtoehto, joihin SQL näennäiskoneiden on sisäinen kuormituksen avulla voit varmistaa tietokantapalvelimet eivät näy käyttämällä suoraan julkiseen IP-osoite takaisin lopussa.


### <a name="step-1"></a>Vaihe 1

Luo sisäinen kuormituksen, määrittää käyttämällä `azure network service internal-load-balancer add`.

     azure service internal-load-balancer add -r mytestcloud -n ilbset -t subnet-1 -a 192.168.2.7

Parametrit, joita käytetään:

**-r** - pilvi palvelunimi<BR>
**-n** - sisäinen kuormituksen tasauspalvelun nimi<BR>
**-t** - aliverkon nimi (sama aliverkon lisätään sisäisiä kuormituksen näennäiskoneiden mukaan)<BR>
**-** - (valinnainen) Lisää staattinen yksityinen IP-osoite<BR>

Tutustu `azure service internal-load-balancer --help` lisätietoja.

Sisäinen kuormituksen tasauspalvelun ominaisuudet-komennon avulla voit tarkistaa `azure service internal-load-balancer list` *cloud palvelunimi*.

Tässä on esimerkki tulosteen seuraavasti:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


## <a name="step-2"></a>Vaihe 2

Voit määrittää sisäinen kuormituksen tasauspalvelun määrittäminen, kun lisäät ensimmäisen päätepisteen. Voit liittää päätepisteen, virtuaalikoneen ja näytteenottimen portin sisäinen kuormituksen tasauspalvelun Määritä tässä vaiheessa.

    azure vm endpoint create db1 1433 -k 1433 tcp -t 1433 -r tcp -e 300 -f 600 -i ilbset

Parametrit, joita käytetään:

**-k** - virtual Paikallinen kone-portin<BR>
**-t** - näytteenottimen portti<BR>
**-r** - näytteenottimen protokolla<BR>
**-e** - näytteenottimen aikaväli sekunteina<BR>
**-f** - aikakatkaisun sekunnin kuluttua <BR>
**-i** – sisäinen kuormituksen tasauspalvelun nimi <BR>


## <a name="step-3"></a>Vaihe 3

Tarkista kuormituksen tasauspalvelun määritysten käyttäminen `azure vm show` *virtuaalikoneen nimi*

    azure vm show DB1

Tulos on:

        azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK


## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Luo remote työpöydän päätepiste virtual tietokoneelle

Voit luoda remote työpöydän päätepisteen välittäminen verkkoliikennettä Julkinen portti paikallista porttia tietyn virtuaalikoneen käyttämällä `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Poista virtuaalikoneen kuormituksen

Voit poistaa virtual machine määrittäminen poistamalla liittyvät päätepisteen sisäinen kuormituksen. Päätepisteen poistetaan, kun virtuaalikoneen ei kuulu enää määrittää kuormituksen.

 Käytä edellä olevaa esimerkkiä, voit poistaa virtuaalikoneen "MJP1" sisäinen kuormituksen "ilbset"-komennon avulla luotu päätepisteen `azure vm endpoint delete`.

    azure vm endpoint delete DB1 tcp-1433-1433


Tutustu `azure vm endpoint --help` lisätietoja.


## <a name="next-steps"></a>Seuraavat vaiheet

[Lähde-IP-affiniteetti käyttämällä kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)