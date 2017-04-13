<properties
   pageTitle="Usean NIC VMs käyttämällä Azure-CLI perinteinen käyttöönoton mallin käyttöön | Microsoft Azure"
   description="Lue, miten voit ottaa usean NIC VMs Azure-CLI käyttäminen perinteinen käyttöönottomalli"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-classic-using-the-azure-cli"></a>Ottaa käyttöön käyttämällä Azure-CLI usean NIC VMs (perinteinen)

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Voit luoda näennäiskoneiden (VMs) Azure ja liitä kunkin käyttäjän VMs useita verkkoliittymät (NIC). Useita NIC käyttöön tietoliikenteen eri erottaminen NIC yli. Esimerkiksi yksi NIC ehkä yhteydessä Internetiin, kun toiseen kommunikoi osallistuvan sisäiset resurssit ei ole yhteydessä Internetiin. Mahdollisuus verkkoliikenteestä erota yli useita NIC vaaditaan monta verkon virtual laitteita, kuten sovelluksen toimitus- ja WAN optimointimahdollisuudet.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Lue, miten [Resurssienhallinta-mallin seuraavasti](virtual-network-deploy-multinic-arm-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Ei voi tällä hetkellä olla VMs yksittäisen NIC kanssa ja VMs, jossa on useita NIC saman pilvipalvelussa. Tämän vuoksi haluat toteuttavien uudelleen palvelimissa eri pilvipalvelussa kuin ja muiden osien skenaariossa. Seuraavia ohjeita käyttäminen-nimiseen *IaaSStory* tärkeimmät resursseja ja *IaaSStory Taustajärjestelmä* uudelleen palvelimia pilvipalveluun.

## <a name="prerequisites"></a>Edellytykset

Ennen kuin voit ottaa käyttöön uudelleen palvelimissa, sinun täytyy ottaa käyttöön sisällön pilvipalvelussa sijaitsevaan ja kaikki tarvittavat resurssit tässä tapauksessa. Vähintään, sinun täytyy luoda virtuaalisen verkon taustaan aliverkon. Käy virtual verkon opetteleminen [luominen käyttämällä Azure-CLI virtual verkkoon](virtual-networks-create-vnet-classic-cli.md) .

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Edellinen loppuun VMs käyttöönotto

Taustajärjestelmä VMs määräytyvät alla luetellut resurssit luomista.

- **Tallennustilan tilin tiedot levyille**. Parantaa suorituskykyä tietoja levyjen tietokannan palvelimissa, jotka käyttävät tasainen tilan asemaa Suoritettaessa tekniikka, jossa pyydetään premium-tallennustilan tilin. Varmista, että käyttöönottoa premium tallennustilan tukemaan Azure sijainti.
- **NIC**. Kunkin AM on kaksi NIC-tietokannan käyttämiseen ja yksi hallintaa varten.
- **Saatavuus**. Tietokantapalvelin lisätään yksi käytettävyys määrittäminen, jotta vähintään yksi VMs on ylös ja suorittamalla ylläpidon aikana.

### <a name="step-1---start-your-script"></a>Vaihe 1 – komentosarja

Voit ladata käytetty koko bash komentosarja [tähän](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Noudata seuraavia ohjeita voit muuttaa toimimaan ympäristössäsi komentosarja.

1. Muuttaa arvoja muuttujien, otettu käyttöön edellä [edellytykset](#Prerequisites)aiemmin resurssiryhmän perusteella.

        location="useast2"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"

2. Muuttaa arvoja muuttujien, jota haluat käyttää Taustajärjestelmä käyttöönottoa varten arvojen perusteella.

        backendCSName="IaaSStory-Backend"
        prmStorageAccountName="iaasstoryprmstorage"
        image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskPrefix="db"
        dataDiskName="datadisk"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Vaihe 2: Luo tarvittavat resurssit oman VMs

1. Luo uusi pilvipalveluun kaikki Taustajärjestelmä VMs. Huomaa, käyttäminen `$backendCSName` muuttujan resurssin nimi- ja `$location` Azure alue.

        azure service create --serviceName $backendCSName \
            --location $location

2. Luo omasi VMs käytettävän OS ja tietojen levyjen premium tallennustilan-tili.

        azure storage account create $prmStorageAccountName \
            --location $location \
            --type PLRS

### <a name="step-3---create-vms-with-multiple-nics"></a>Vaihe 3 – Luo VMs useita NIC

1. Aloita silmukka, voit luoda useita VMs, perusteella `numberOfVMs` muuttujat.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Määritä kunkin AM nimi ja IP-osoite kunkin kaksi NIC.

            nic1Name=$vmNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x

            nic2Name=$vmNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x

4. Luo AM. Huomaa, käyttö `--nic-config` parametri, joka sisältää kaikki NIC sisältää nimen, aliverkon ja IP-osoite.

            azure vm create $backendCSName $image $username $password \
                --connect $backendCSName \
                --vm-name $vmNamePrefix$suffixNumber \
                --vm-size $vmSize \
                --availability-set $avSetName \
                --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
                --virtual-network-name $vnetName \
                --subnet-names $backendSubnetName \
                --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::

5. Luo kaksi tietojen levyä kunkin AM.

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
        done

### <a name="step-4---run-the-script"></a>Vaihe 4 - komentosarjan suorittaminen

Ladattu ja muuttaa käyttötarkoitukseen komentosarja, useita NIC tietokannan VMs luominen uudelleen suorittamalla.

1. Tallenna komentosarja ja pitäminen **Bash** päätteen. Alkuperäinen tulos näkyy alla kuvatulla tavalla.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. Muutaman minuutin kuluttua suorittamisen päättyy ja muiden tulos tulee näkyviin alla kuvatulla tavalla.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
