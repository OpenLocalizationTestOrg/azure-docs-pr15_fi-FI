<properties
    pageTitle="Luo ja lataa Oracle-Linux Näennäiskiintolevyn | Microsoft Azure"
    description="Opettele luomaan ja lataa Azure virtual kiintolevyn (Näennäiskiintolevyn), joka sisältää Oracle Linux-käyttöjärjestelmä."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Oracle Linux-virtuaalikoneen valmisteleminen Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Edellytykset ##

Tässä artikkelissa oletetaan, että olet jo asentanut Oracle Linux-käyttöjärjestelmän virtual kiintolevylle. Useita työkaluja .vhd-tiedostoja, kuten virtualization ratkaista esimerkiksi Hyper-V luomiseen. Katso ohjeet [Hyper-V rooli ja määritä Virtual Machine asentaminen](http://technet.microsoft.com/library/hh846766.aspx).


### <a name="oracle-linux-installation-notes"></a>Oracle Linux asennusohjeet

- Katso myös [Yleinen Linux asennusohjeet](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) Linux valmisteleminen Azure Lisää vihjeitä.

- Oracle on punainen on yhteensopiva ydin ja niiden UEK3 (Unbreakable yrityksen ydin) tuetaan Hyper-V ja Azure. Saat parhaan tuloksen muista päivittää uusimmat ydin valmisteltaessa Oracle-Linux Näennäiskiintolevyn.

- Oracle's UEK2 ei tueta Hyper-V ja Azure, kun se ei ole tarvittavia ohjaimia.

- Azure-vain **Kiinteä Näennäiskiintolevyn**ei tueta VHDX-muodossa.  Voit muuntaa levyn Näennäiskiintolevyn muotoon Hyper-V Manager tai Muunna näennäiskiintolevyn cmdlet-komennon avulla.

- Asennettaessa Linux-järjestelmä on suositeltavaa, että käytät vakio-osiosta LVM (usein useita asennusten oletusasetus) sijaan. Välttämiseksi LVM nimi on ristiriidassa kloonatun VMs, etenkin jos OS-levy on joskus liitettäväksi toiseen AM vianmääritystä varten. [LVM](virtual-machines-linux-configure-lvm.md) tai [RAID](virtual-machines-linux-configure-raid.md) voidaan tietojen levyille, jos ensisijainen.

- NUMA ei tueta suurempi AM koot ohjelmavirhe Linux ydin versioissa alla 2.6.37 vuoksi. Tämä ongelma vaikuttaa pääasiassa jaot avulla seuraavat punainen on 2.6.32 ydin. Manuaalinen asennus Azure Linux-agentti (waagent) automaattisesti estää NUMA Linux ydin GRUB määritystä. Lisätietoja tästä löytyvät alla kuvatulla tavalla.

- Vaihda-osion ei määritetä OS levyn. Vaihda tiedoston tilapäinen resurssin levyn luominen voi määrittää Linux-agentti.  Lisätietoja tästä löytyvät alla kuvatulla tavalla.

- Kaikki näennäiskiintolevyt on oltava koko on 1 Megatavu Kerrannaiset.

- Varmista, että `Addons` säilö on otettu käyttöön. Muokkaa tiedostoa `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux-6) tai `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), ja muuttaa viivan `enabled=0` , `enabled=1` **[ol6_addons]** ja **[ol7_addons]** tässä tiedostossa-kohdassa.


## <a name="oracle-linux-64"></a>Oracle Linux 6.4 + ##

Sinun on suoritettava tietyt määritysvaiheet virtuaalikoneen toimimaan Azure-käyttöjärjestelmässä.

1. Valitse keskimmäisessä ruudussa Hyper-V hallinnan, virtuaalikoneen.

2. Valitse **Yhdistä** Avaa virtuaalikoneen-ikkuna.

3. Poista NetworkManager suorittamalla seuraavan komennon:

        # sudo rpm -e --nodeps NetworkManager

    **Huomautus:** Jos paketti ei ole asennettu, tämä komento ei onnistu ja näyttöön tulee virhesanoma. Tämä on oikein.

4.  Luo tiedosto nimeltä **verkon** `/etc/sysconfig/` kansio, joka sisältää seuraavan tekstin:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Luo tiedosto nimeltä **ifcfg eth0** - `/etc/sysconfig/network-scripts/` kansio, joka sisältää seuraavan tekstin:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Muokkaa udev säännöt tapahtuvien Ethernet-liittymän staattinen säännöt luodaan. Sääntöjen saattaa aiheuttaa ongelmia, kun kloonaamalla virtual kone Microsoft Azure tai Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Varmista verkkopalvelu alkaa käynnistyksen yhteydessä suorittamalla seuraavan komennon:

        # chkconfig network on

8. Asenna python pyasn1 suorittamalla seuraavan komennon:

        # sudo yum install python-pyasn1

9.  Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa "/ boot/grub/menu.lst" tekstieditorissa ja varmista, että oletusarvoinen ydin sisältää seuraavat parametrit:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Näin varmistat myös konsolin viestit lähetetään ensimmäisen serial porttia, joka auttaa Azure tukevia virheenkorjaus ongelmat. Tämä poistaa käytöstä NUMA Oracle on punainen on yhteensopiva ydin olevan virheen vuoksi.

    Lisäksi edellä on suositeltavaa, jos haluat *poistaa* seuraavat parametrit:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.

    `crashkernel` Asetus on määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM joka voi olla pienempi AM koot ongelmia.


10. Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä.  Tämä on yleensä oletusarvo.

11. Asenna Azure Linux-agentti suorittamalla seuraavan komennon. Uusin versio on 2.0.15.

        # sudo yum install WALinuxAgent

    Huomaa, että WALinuxAgent-paketin asennuksen poistaa NetworkManager NetworkManager gnome pakettien jos ne ole jo poistettiin ohjeiden vaiheessa 2.

12. Vaihda tila ei luoda OS-levylle.

    Azure Linux-agentti automaattisesti määrittää swap-tallennustilasta on käytössä paikallinen resurssi levy, johon on liitetty AM Azure-valmistelun jälkeen. Huomaa, että paikallinen resurssi-levy on *väliaikainen* DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Asennettuasi Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa /etc/waagent.conf seuraavien parametrien oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Valitse Hyper-V Manager- **toiminto -> Sammuta alaspäin** . Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.


----------


## <a name="oracle-linux-70"></a>Oracle Linux 7.0 + ##

**Oracle Linux 7 muutokset**

Oracle Linux 7-virtuaalikoneen valmisteleminen Azure on hyvin samantyyppinen Oracle Linux 6, mutta liittämisasetuksia on otettava huomioon useita eroja:

 - Punainen on yhteensopiva ydin ja Oracle's UEK3 tuetaan Azure.  UEK3 ydin suositellaan.
 - NetworkManager paketin ristiriidassa enää Azure Linux-agentti. Tämä paketti asennetaan oletusarvoisesti ja suosittelemme, että se ei poisteta.
 - GRUB2 käytetään nyt oletusarvo-Käynnistyslataimen niin menettelyä muokattavaksi ydin parametrit on muuttunut (Katso alla).
 - XFS on nyt käyttöjärjestelmän. Ext4 tiedostojärjestelmän voidaan edelleen tarvittaessa.


**Määritysvaiheet**

1. Valitse virtuaalikoneen Hyper-V hallinta.

2. Valitse **Yhdistä** Avaa virtuaalikoneen console-ikkuna.

3.  Luo tiedosto nimeltä **verkon** `/etc/sysconfig/` kansio, joka sisältää seuraavan tekstin:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Luo tiedosto nimeltä **ifcfg eth0** - `/etc/sysconfig/network-scripts/` kansio, joka sisältää seuraavan tekstin:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

5.  Muokkaa udev säännöt tapahtuvien Ethernet-liittymän staattinen säännöt luodaan. Sääntöjen saattaa aiheuttaa ongelmia, kun kloonaamalla virtual kone Microsoft Azure tai Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Varmista verkkopalvelu alkaa käynnistyksen yhteydessä suorittamalla seuraavan komennon:

        # sudo chkconfig network on

7. Asenna python pyasn1 paketti suorittamalla seuraavan komennon:

        # sudo yum install python-pyasn1

8.  Suorita seuraava komento Poista nykyinen yum metatiedot ja asenna päivitykset:

        # sudo yum clean all
        # sudo yum -y update

9.  Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Toiminto avaa "/ jne/oletusarvon/grub" tekstieditorissa ja muokkaa `GRUB_CMDLINE_LINUX` parametri, esimerkiksi:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Näin varmistat myös konsolin viestit lähetetään ensimmäisen serial porttia, joka auttaa Azure tukevia virheenkorjaus ongelmat. Myös poistaa käytöstä uusi OEL 7 nimeämiskäytännöt NIC varten. Lisäksi edellä on suositeltavaa, jos haluat *poistaa* seuraavat parametrit:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.

    `crashkernel` Asetus on määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM joka voi olla pienempi AM koot ongelmia.


10. Kun olet lopettanut muokkaamisen "/ jne/oletusarvon/grub" kohti yläpuolella, suorita seuraava komento grub-määritys uudelleen:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

11. Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä.  Tämä on yleensä oletusarvo.

12. Asenna Azure Linux-agentti suorittamalla seuraavan komennon:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

13. Vaihda tila ei luoda OS-levylle.

    Azure Linux-agentti automaattisesti määrittää swap-tallennustilasta on käytössä paikallinen resurssi levy, johon on liitetty AM Azure-valmistelun jälkeen. Huomaa, että paikallinen resurssi-levy on *väliaikainen* DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Asennettuasi Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa /etc/waagent.conf seuraavien parametrien oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Valitse Hyper-V Manager- **toiminto -> Sammuta alaspäin** . Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.


## <a name="next-steps"></a>Seuraavat vaiheet
Voit nyt Oracle Linux .vhd avulla voit luoda uuden näennäiskoneiden Azure-tietokannassa. Jos kyseessä on, että lataamasi .vhd-tiedoston Azure ensimmäistä kertaa, katso vaiheet 2 ja 3 [luominen ja lataamisen virtual kiintolevyn, joka sisältää Linux-käyttöjärjestelmä](virtual-machines-linux-classic-create-upload-vhd.md).
