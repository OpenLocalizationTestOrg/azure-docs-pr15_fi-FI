<properties
    pageTitle="Valmistele Debian Linux Näennäiskiintolevyn | Microsoft Azure"
    description="Opi luomaan Azure Debian 7 ja 8 näennäiskiintolevytiedostoja käyttöönottoa varten."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>



# <a name="prepare-a-debian-vhd-for-azure"></a>Azure Debian Näennäiskiintolevyn valmisteleminen

## <a name="prerequisites"></a>Edellytykset
Tässä osassa oletetaan, että olet jo asentanut Debian Linux-käyttöjärjestelmän .iso-tiedostosta ladataan [Debian sivuston](https://www.debian.org/distrib/) virtual kiintolevylle. Useita työkaluja, voit luoda .vhd tiedostoja; Hyper-V on vain yksi esimerkki. Katso ohjeet käyttämällä Hyper-V [Hyper-V rooli ja määritä Virtual Machine asentaminen](https://technet.microsoft.com/library/hh846766.aspx).


## <a name="installation-notes"></a>Asennusohjeet

- Katso myös [Yleinen Linux asennusohjeet](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) Linux valmisteleminen Azure Lisää vihjeitä.
- Azure VHDX uudempaan tiedostomuotoon ei tueta. Voit muuntaa levyn Näennäiskiintolevyn muotoon Hyper-V Manager tai **Muunna näennäiskiintolevyn** cmdlet-komennon avulla.
- Asennettaessa Linux-järjestelmä on suositeltavaa, että käytät vakio-osiosta LVM (usein useita asennusten oletusasetus) sijaan. Välttämiseksi LVM nimi on ristiriidassa kloonatun VMs, etenkin jos OS-levy on joskus liitettäväksi toiseen AM vianmääritystä varten. [LVM](virtual-machines-linux-configure-lvm.md) tai [RAID](virtual-machines-linux-configure-raid.md) voidaan tietojen levyille, jos ensisijainen.
- Vaihda-osion ei määritetä OS levyn. Azure Linux-agentti voi määrittää väliaikaisen resurssin levyn swap-tiedoston luominen. Lisätietoja tästä löytyvät alla kuvatulla tavalla.
- Kaikki näennäiskiintolevyt on oltava koko on 1 Megatavu Kerrannaiset.


## <a name="use-azure-manage-to-create-debian-vhds"></a>Luo Debian näennäiskiintolevyjen Azure hallinta avulla

Käytettävissä luonnissa Debian näennäiskiintolevyjen Azure-, kuten on työkaluja [azure-hallita](https://github.com/credativ/azure-manage) komentosarjojen [credativ](http://www.credativ.com/). Tämä on suositeltu lähestymistapa ja kuvan luominen alusta alkaen. Esimerkiksi Luo Suorita lataamaan seuraavat komennot Debian 8 Näennäiskiintolevyn azure-hallinta (ja riippuvuuksien) ja suorita azure_build_image komentosarja:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Valmistele manuaalisesti Debian Näennäiskiintolevyn

1. Valitse virtuaalikoneen Hyper-V hallinta.

2. Valitse **Yhdistä** Avaa virtuaalikoneen console-ikkuna.

3. Kommentin **deb CD** rivi, `/etc/apt/source.list` Jos määrität AM vastaan ISO-tiedosto.

4. Muokkaa `/etc/default/grub` tiedoston ja Muokkaa seuraavasti, jotka haluat lisätä muita ydin parametrit Azure **GRUB_CMDLINE_LINUX** -parametria.

        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"

5. Muodosta uudelleen grub ja suorita:

        # sudo update-grub

6. Lisää Debian's Azure säilöjen tietoihin /etc/apt/sources.list Debian 7 ja 8:

    **Debian 7.x "Wheezy"**

        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main


    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


7. Asenna Azure Linux-agentti:

        # sudo apt-get update
        # sudo apt-get install waagent

8. Debian 7 se on suoritettava 3.16 perustuva ydin wheezy backports säilöstä. Luo ensin tiedosto nimeltä /etc/apt/preferences.d/linux.pref seuraavat sisältöön:

        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500

    Valitse Suorita "sudo-piharakennus get Asenna linux-kuva-amd64" Asenna uusi ydin.

8. Deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu ja suorita:

        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

9. Valitse **toiminto** alaspäin-Sammuta > Hyper-V hallinnassa. Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.


## <a name="next-steps"></a>Seuraavat vaiheet

Voit nyt Debian virtual kiintolevyn avulla voit luoda uuden näennäiskoneiden Azure-tietokannassa. Jos kyseessä on, että lataamasi .vhd-tiedoston Azure ensimmäistä kertaa, katso vaiheet 2 ja 3 [luominen ja lataamisen virtual kiintolevyn, joka sisältää Linux-käyttöjärjestelmä](virtual-machines-linux-classic-create-upload-vhd.md).
