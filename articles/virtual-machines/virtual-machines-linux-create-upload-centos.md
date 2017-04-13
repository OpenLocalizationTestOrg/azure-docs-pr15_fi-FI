<properties
    pageTitle="Luo ja lataa CentOS-pohjainen Linux Näennäiskiintolevyn Azure-tietokannassa"
    description="Opettele luomaan ja lataa Azure virtual kiintolevyn (Näennäiskiintolevyn), joka sisältää CentOS perustuva Linux-käyttöjärjestelmä."
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
    ms.date="05/09/2016"
    ms.author="szarkos"/>

# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Azure CentOS perustuva virtual machine valmisteleminen

- [Azure CentOS 6.x virtual koneen valmisteleminen](#centos-6x)
- [Azure CentOS 7.0 + virtual koneen valmisteleminen](#centos-70)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Edellytykset ##

Tässä artikkelissa oletetaan, että olet jo asentanut CentOS (tai samanlaisia tehty) Linux käyttöjärjestelmän virtual kiintolevylle. Useita työkaluja .vhd-tiedostoja, kuten virtualization ratkaista esimerkiksi Hyper-V luomiseen. Katso ohjeet [Hyper-V rooli ja määritä Virtual Machine asentaminen](http://technet.microsoft.com/library/hh846766.aspx).


**CentOS asennusohjeet**

- Katso myös [Yleinen Linux asennusohjeet](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) Linux valmisteleminen Azure Lisää vihjeitä.

- Azure-vain **Kiinteä Näennäiskiintolevyn**ei tueta VHDX-muodossa.  Voit muuntaa levyn Näennäiskiintolevyn muotoon Hyper-V Manager tai Muunna näennäiskiintolevyn cmdlet-komennon avulla.

- Asennettaessa Linux-järjestelmä on suositeltavaa, että käytät vakio-osiosta LVM (usein useita asennusten oletusasetus) sijaan. Välttämiseksi LVM nimi on ristiriidassa kloonatun VMs, etenkin jos OS-levy on joskus liitettäväksi toiseen AM vianmääritystä varten.  [LVM](virtual-machines-linux-configure-lvm.md) tai [RAID](virtual-machines-linux-configure-raid.md) voidaan tietojen levyille, jos ensisijainen.

- NUMA ei tueta suurempi AM koot ohjelmavirhe Linux ydin versioissa alla 2.6.37 vuoksi. Tämä ongelma vaikuttaa pääasiassa jaot avulla seuraavat punainen on 2.6.32 ydin. Manuaalinen asennus Azure Linux-agentti (waagent) automaattisesti estää NUMA Linux ydin GRUB määritystä. Lisätietoja tästä löytyvät alla kuvatulla tavalla.

- Vaihda-osion ei määritetä OS levyn. Vaihda tiedoston tilapäinen resurssin levyn luominen voi määrittää Linux-agentti.  Lisätietoja tästä löytyvät alla kuvatulla tavalla.

- Kaikki näennäiskiintolevyt on oltava koko on 1 Megatavu Kerrannaiset.


## <a name="centos-6x"></a>CentOS 6.x ##

1. Valitse virtuaalikoneen Hyper-V hallinta.

2. Valitse **Yhdistä** Avaa virtuaalikoneen console-ikkuna.

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

        # sudo chkconfig network on


8. **Vain centOS 6.3**: Asenna ohjaimet Linux Integration Services (LIS).

    **Tärkeää: Vaihe on vain kelvollisia CentOS 6.3 ja aiempi versio.**  CentOS 6.4 + Linux Integration Services ovat *jo käytettävissä standard ydin*.

    - Noudata asennusohjeita [LIS lataussivulle](https://www.microsoft.com/en-us/download/details.aspx?id=46842) ja asenna JÄLLEENMYYNTIHINNAN kuvan päälle.  


9. Asenna python pyasn1 paketti suorittamalla seuraavan komennon:

        # sudo yum install python-pyasn1

10. Jos haluat käyttää OpenLogic vastaa, joita isännöidään Azure palvelinkeskusten sisällä, korvaa /etc/yum.repos.d/CentOS-Base.repo tiedoston seuraavien säilöjen tietoihin.  Tämä lisää myös **[openlogic]** säilö, joka sisältää paketteja, jotka Azure Linux-agentti:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    **Huomautus:** Tämän oppaan loput olettaa, että käytössäsi on oltava vähintään [openlogic]-repo, jota käytetään asentaminen alla Azure Linux-agentti.


11. Lisää seuraava rivi /etc/yum.conf:

        http_caching=packages

    Ja lisää seuraava rivi **CentOS 6.3 vain** :

        exclude=kernel*

12. Poistaa käytöstä yum moduulin "fastestmirror" tiedostoa muokkaavat "/ etc/yum/pluginconf.d/fastestmirror.conf", ja valitse `[main]`, kirjoita seuraava:

        set enabled=0

13. Suorita seuraava komento Poista nykyinen yum metatiedot:

        # yum clean all

14. **Valitse CentOS 6.3 vain**Päivitä ydin käyttämällä seuraava komento:

        # sudo yum --disableexcludes=all install kernel

15. Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa "/ boot/grub/menu.lst" tekstieditorissa ja varmista, että oletusarvoinen ydin sisältää seuraavat parametrit:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Näin varmistat myös konsolin viestit lähetetään ensimmäisen serial porttia, joka auttaa Azure tukevia virheenkorjaus ongelmat. Tämä poistaa käytöstä NUMA vuoksi ohjelmavirhe CentOS 6 käyttämä ydin-versiossa.

    Lisäksi edellä on suositeltavaa, jos haluat *poistaa* seuraavat parametrit:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.

    `crashkernel` Asetus on määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM joka voi olla pienempi AM koot ongelmia.


16. Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä.  Tämä on yleensä oletusarvo.

17. Asenna Azure Linux-agentti suorittamalla seuraavan komennon:

        # sudo yum install WALinuxAgent

    Huomaa, että WALinuxAgent-paketin asennuksen poistaa NetworkManager NetworkManager gnome pakettien jos ne ole jo poistettiin ohjeiden vaiheessa 2.

18. Vaihda tila ei luoda OS-levylle.

    Azure Linux-agentti automaattisesti määrittää swap-tallennustilasta on käytössä paikallinen resurssi levy, johon on liitetty AM Azure-valmistelun jälkeen. Huomaa, että paikallinen resurssi-levy on *väliaikainen* DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Asennettuasi Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa /etc/waagent.conf seuraavien parametrien oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

19. Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

20. Valitse Hyper-V Manager- **toiminto -> Sammuta alaspäin** . Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.


----------


## <a name="centos-70"></a>CentOS 7.0 + ##

**Muutosten CentOS 7 (ja samalla johdannaiset)**

CentOS 7 virtual koneen valmisteleminen Azure on hyvin samantyyppinen CentOS 6, mutta liittämisasetuksia on otettava huomioon useita eroja:

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

6.  Muokkaa udev säännöt tapahtuvien Ethernet-liittymän staattinen säännöt luodaan. Sääntöjen saattaa aiheuttaa ongelmia, kun kloonaamalla virtual kone Microsoft Azure tai Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Varmista verkkopalvelu alkaa käynnistyksen yhteydessä suorittamalla seuraavan komennon:

        # sudo chkconfig network on

7. Asenna python pyasn1 paketti suorittamalla seuraavan komennon:

        # sudo yum install python-pyasn1

8. Jos haluat käyttää OpenLogic vastaa, joita isännöidään Azure palvelinkeskusten sisällä, korvaa /etc/yum.repos.d/CentOS-Base.repo tiedoston seuraavien säilöjen tietoihin.  Tämä lisää myös **[openlogic]** säilö, joka sisältää paketteja, jotka Azure Linux-agentti:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7



    **Huomautus:** Tämän oppaan loput olettaa, että käytössäsi on oltava vähintään [openlogic]-repo, jota käytetään asentaminen alla Azure Linux-agentti.

9.  Suorita seuraava komento Poista nykyinen yum metatiedot ja asenna päivitykset:

        # sudo yum clean all
        # sudo yum -y update

10. Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa "/ jne/oletusarvon/grub" tekstieditorissa ja muokkaa `GRUB_CMDLINE_LINUX` parametri, esimerkiksi:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Näin varmistat myös konsolin viestit lähetetään ensimmäisen serial porttia, joka auttaa Azure tukevia virheenkorjaus ongelmat. Myös poistaa käytöstä uusi CentOS 7 nimeämiskäytännöt NIC varten. Lisäksi edellä on suositeltavaa, jos haluat *poistaa* seuraavat parametrit:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.

    `crashkernel` Asetus on määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM joka voi olla pienempi AM koot ongelmia.

11. Kun olet lopettanut muokkaamisen "/ jne/oletusarvon/grub" kohti yläpuolella, suorita seuraava komento grub-määritys uudelleen:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

12. Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä.  Tämä on yleensä oletusarvo.

13. **Vain, jos muodostetaan kuvan VMWare, VirtualBox tai KVM:** Lisää Hyper-V moduulit initramfs:

    Muokkaa `/etc/dracut.conf`, Lisää sisältö:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Muodosta uudelleen initramfs:

        # dracut –f -v

14. Asenna Azure Linux-agentti suorittamalla seuraavan komennon:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

15. Vaihda tila ei luoda OS-levylle.

    Azure Linux-agentti automaattisesti määrittää swap-tallennustilasta on käytössä paikallinen resurssi levy, johon on liitetty AM Azure-valmistelun jälkeen. Huomaa, että paikallinen resurssi-levy on *väliaikainen* DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Asennettuasi Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa /etc/waagent.conf seuraavien parametrien oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Valitse Hyper-V Manager- **toiminto -> Sammuta alaspäin** . Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.

## <a name="next-steps"></a>Seuraavat vaiheet
Voit nyt CentOS Linux virtual kiintolevyn avulla voit luoda uuden näennäiskoneiden Azure-tietokannassa. Jos kyseessä on, että lataamasi .vhd-tiedoston Azure ensimmäistä kertaa, katso vaiheet 2 ja 3 [luominen ja lataamisen virtual kiintolevyn, joka sisältää Linux-käyttöjärjestelmä](virtual-machines-linux-classic-create-upload-vhd.md).
