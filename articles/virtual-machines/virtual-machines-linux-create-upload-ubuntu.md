<properties
    pageTitle="Luo ja lataa Ubuntu Linux-Näennäiskiintolevyn Azure-tietokannassa"
    description="Opettele luomaan ja lataa Azure virtual kiintolevyn (Näennäiskiintolevyn), joka sisältää Ubuntu Linux-käyttöjärjestelmä."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Azure Ubuntu-virtuaalikoneen valmisteleminen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Virallinen Ubuntu cloud kuvat
Ubuntu julkaisee nyt virallinen Azure näennäiskiintolevyjen [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/)latausta varten. Jos tarvitset erityistä Ubuntu-kuvan luominen Azure, mieluummin kuin manuaalinen alla olevien ohjeiden kannattaa aloittaa näitä tunnetut toimi näennäiskiintolevyjen ja mukauttaa sen tarpeisiisi sopivaksi. Uusimman kuva-versioiden tukikäytännöistä aina seuraavista sijainneista:

 - Ubuntu 12.04/tarkka: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/precise/release/ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)


## <a name="prerequisites"></a>Edellytykset

Tässä artikkelissa oletetaan, että olet jo asentanut Ubuntu Linux-käyttöjärjestelmän virtual kiintolevylle. Useita työkaluja .vhd-tiedostoja, kuten virtualization ratkaista esimerkiksi Hyper-V luomiseen. Katso ohjeet [Hyper-V rooli ja määritä Virtual Machine asentaminen](http://technet.microsoft.com/library/hh846766.aspx).

**Ubuntu asennusohjeet**

- Katso myös [Yleinen Linux asennusohjeet](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) Linux valmisteleminen Azure Lisää vihjeitä.
- Azure-vain **Kiinteä Näennäiskiintolevyn**ei tueta VHDX-muodossa.  Voit muuntaa levyn Näennäiskiintolevyn muotoon Hyper-V Manager tai Muunna näennäiskiintolevyn cmdlet-komennon avulla.
- Asennettaessa Linux-järjestelmä on suositeltavaa, että käytät vakio-osiosta LVM (usein useita asennusten oletusasetus) sijaan. Välttämiseksi LVM nimi on ristiriidassa kloonatun VMs, etenkin jos OS-levy on joskus liitettäväksi toiseen AM vianmääritystä varten. [LVM](virtual-machines-linux-configure-lvm.md) tai [RAID](virtual-machines-linux-configure-raid.md) voidaan tietojen levyille, jos ensisijainen.
- Vaihda-osion ei määritetä OS levyn. Vaihda tiedoston tilapäinen resurssin levyn luominen voi määrittää Linux-agentti.  Lisätietoja tästä löytyvät alla kuvatulla tavalla.
- Kaikki näennäiskiintolevyt on oltava koko on 1 Megatavu Kerrannaiset.


## <a name="manual-steps"></a>Manuaaliset vaiheet

> [AZURE.NOTE] Ennen kuin luot mukautetun Ubuntu-kuva, Azure, harkitse [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) kuvia sijaan.


1. Valitse keskimmäisessä ruudussa Hyper-V hallinnan, virtuaalikoneen.

2. Valitse **Yhdistä** Avaa virtuaalikoneen-ikkuna.

3.  Korvaa nykyinen säilöjen tietoihin Ubuntu's Azure repos käyttää kuvassa. Etenee hieman Ubuntu-version mukaan.

    Muokata /etc/apt/sources.list, ennen kuin kannattaa tehdä varmuuskopio:

        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

4. Ubuntu Azure kuvat ovat nyt seurannassa ydin *laitteiston mahdollistamisen* (HWE). Päivitä käyttöjärjestelmän uusimman ydin suorittamalla seuraavat komennot:

    Ubuntu 12.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    Ubuntu 14.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot


5. Muokkaa Grub, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistyksen rivi. Toiminto avaa "/ jne/oletusarvon/grub" tekstieditorissa, Etsi muuttujaan, jota kutsutaan `GRUB_CMDLINE_LINUX_DEFAULT` (tai lisää se tarvittaessa) ja muokkaa sitä sisältämään seuraavat parametrit:

        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Tallenna ja sulje tiedosto ja suorita sitten "`sudo update-grub`". Näin varmistat konsolin viestit lähetetään ensimmäinen sarja portti, joka auttaa Azure teknisen tuen kanssa virheenkorjaus ongelmat.

6.  Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä.  Tämä on yleensä oletusarvo.

7.  Asenna Azure Linux-agentti:

        # sudo apt-get update
        # sudo apt-get install walinuxagent

    Huomaa, että asentaminen `walinuxagent` package poistetaan `NetworkManager` ja `NetworkManager-gnome` paketteja, jos ne on asennettu.

8.  Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Valitse Hyper-V Manager- **toiminto -> Sammuta alaspäin** . Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.


## <a name="next-steps"></a>Seuraavat vaiheet
Voit nyt Ubuntu Linux virtual kiintolevyn avulla voit luoda uuden näennäiskoneiden Azure-tietokannassa. Jos kyseessä on, että lataamasi .vhd-tiedoston Azure ensimmäistä kertaa, katso vaiheet 2 ja 3 [luominen ja lataamisen virtual kiintolevyn, joka sisältää Linux-käyttöjärjestelmä](virtual-machines-linux-classic-create-upload-vhd.md).

## <a name="references"></a>Viittaukset ##

Ubuntu laitteiston mahdollistamisen (HWE) ydin:

- [http://blog.utlemming.org/2015/01/Ubuntu-1404-Azure-Images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
- [http://blog.utlemming.org/2015/02/1204-Azure-cloud-Images-Now-Using-hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)
