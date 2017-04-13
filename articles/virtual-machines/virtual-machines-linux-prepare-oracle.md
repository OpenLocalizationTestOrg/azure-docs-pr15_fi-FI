<properties
pageTitle="Oracle-Linux virtuaalikoneen valmisteleminen Azure | Microsoft Azure"
description="Vaihe vaiheelta määrityksen suorittamalla Linux: Microsoft Azure Oracle virtual konetta."
services="virtual-machines-linux"
authors="bbenz"
manager="timlt"
documentationCenter="virtual-machines"
tags="azure-service-management,azure-resource-manager"
/>

<tags
ms.service="virtual-machines-linux"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-linux"
ms.workload="infrastructure-services"
ms.date="06/22/2015"
ms.author="bbenz" />

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Oracle Linux-virtuaalikoneen valmisteleminen Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


-   [Oracle Linux 6.4 +-virtuaalikoneen valmisteleminen Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

-   [Oracle Linux 7.0 +-virtuaalikoneen valmisteleminen Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

## <a name="prerequisites"></a>Edellytykset
Tässä artikkelissa oletetaan, että olet jo asentanut Oracle Linux-käyttöjärjestelmän virtual kiintolevylle. Useita työkaluja .vhd-tiedostoja, kuten virtualization ratkaista esimerkiksi Hyper-V luomiseen. Katso ohjeet [asentaa Hyper-V ja luo virtual machine](http://technet.microsoft.com/library/hh846766.aspx).

**Oracle Linux asennusohjeet**

- Oracle on punainen on yhteensopiva ydin ja sen UEK3 (Unbreakable yrityksen ydin) tuetaan Hyper-V ja Azure. Saat parhaan tuloksen muista päivittää uusimmat ydin valmisteltaessa Oracle-Linux Näennäiskiintolevyn.

- Oracle's UEK2 ei tueta Hyper-V ja Azure, kun se ei ole tarvittavia ohjaimia.

- Azure VHDX uudempaan tiedostomuotoon ei tueta. Voit muuntaa levyn Näennäiskiintolevyn muotoon Hyper-V Manager tai Muunna näennäiskiintolevyn cmdlet-komennon avulla.

- Kun asennat Linux-järjestelmään, on suositeltavaa, että käytät vakio-osiosta LVM (usein useita asennusten oletusasetus) sijaan. Välttämiseksi LVM nimi on ristiriidassa kloonatun VMs, etenkin jos OS-levy on joskus liitettäväksi toiseen AM vianmääritystä varten. LVM tai [RAID](virtual-machines-linux-configure-raid.md) voidaan tietojen levyille, jos ensisijainen.

- NUMA ei tueta suurempi AM koot ohjelmavirhe Linux ydin versioissa alla 2.6.37 vuoksi. Tämä ongelma vaikuttaa pääasiassa jaot, jotka käyttävät ylöspäin punainen on 2.6.32 ydin. Manuaalinen asennus Azure Linux-agentti (waagent) automaattisesti estää NUMA Linux ydin GRUB määritystä. Lisätietoja tästä löytyvät alla kuvatulla tavalla.

- Vaihda-osion ei määritetä OS levyn. Vaihda tiedoston tilapäinen resurssin levyn luominen voi määrittää Linux-agentti. Lisätietoja tästä löytyvät alla kuvatulla tavalla.

- Kaikki näennäiskiintolevyt on oltava koko on 1 Megatavu Kerrannaiset.

- Varmista, että `Addons` säilö on otettu käyttöön. Valitse Muokkaa tiedostoa `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux-6) tai `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), ja muuttaa viivan `enabled=0` , `enabled=1` **[ol6_addons]** ja **[ol7_addons]** tässä tiedostossa-kohdassa.


## <a name="oracle-linux-64"></a>Oracle Linux 6.4 +
Sinun on suoritettava tietyt määritysvaiheet virtuaalikoneen toimimaan Azure-käyttöjärjestelmässä.

1. Valitse keskimmäisessä ruudussa Hyper-V hallinnan, virtuaalikoneen.

2. Valitse **Yhdistä** Avaa virtuaalikoneen-ikkuna.

3. Poista NetworkManager suorittamalla seuraavan komennon:

        # sudo rpm -e --nodeps NetworkManager

    >[AZURE.NOTE] Jos paketti ei ole asennettu, tämä komento ei onnistu ja näyttöön tulee virhesanoma. Tämä on oikein.

4. Luo tiedosto nimeltä **verkon** /etc/sysconfig/hakemistossa, joka sisältää seuraavan tekstin:

    `NETWORKING=yes`  
    `HOSTNAME=localhost.localdomain`

5.  Luo-/etc/sysconfig/network-scripts **ifcfg eth0** -tiedosto tai kansio, joka sisältää seuraavan tekstin:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
            TYPE=Ethernet
            USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

6.  Siirrä udev säännöt tapahtuvien luodaan staattinen säännöt Ethernet-liittymän (tai poista). Sääntöjen aiheuttaa ongelmia, kun olet kloonaamalla virtual kone Azure tai Hyper-v

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2\>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2\>/dev/null

7.  Varmista, että verkkopalvelu alkaa käynnistyksen yhteydessä suorittamalla seuraavan komennon:

        # chkconfig network on

8.  Asenna python pyasn1 suorittamalla seuraavan komennon:

        # sudo yum install python-pyasn1

9.  Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa "/ boot/grub/menu.lst" tekstieditorissa ja varmista, että oletusarvoinen ydin sisältää seuraavat parametrit:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Tämä myös varmistaa, että kaikki konsolin viestit lähetetään ensimmäisen serial porttiin, joka auttaa Azure tukevia virheenkorjaus ongelmat. Tämä poistaa käytöstä NUMA Oracle on punainen on yhteensopiva ydin olevan virheen vuoksi.

    Lisäksi edellä, suosittelemme, voit *poistaa* seuraavat parametrit:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.

    `crashkernel` Asetus on määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM. Tämä voi olla pienempi AM koot ongelmia.

10.  Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä. Tämä on yleensä oletusarvo.

11.  Asenna Azure Linux-agentti suorittamalla seuraavan komennon:

        # <a name="sudo-yum-install-walinuxagent"></a>sudo yum asennuksen WALinuxAgent

    Huomaa, että WALinuxAgent-paketin asennuksen poistaa NetworkManager NetworkManager gnome pakettien jos ne ole jo poistettiin ohjeiden vaiheessa 2.

12.  Vaihda tila ei luoda OS-levylle.

    Azure Linux-agentti automaattisesti määrittää swap tilaa, joka on liitetty AM Azure-valmistelun jälkeen paikallinen resurssi-levyn avulla. Huomaa, että paikallinen resurssi-levy on *väliaikainen* DVD-levyllä, ja se voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Kun olet asentanut Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa /etc/waagent.conf seuraavien parametrien oikein:

        ResourceDisk.Format=y

        ResourceDisk.Filesystem=ext4

        ResourceDisk.MountPoint=/mnt/resource

        ResourceDisk.EnableSwap=y

        ResourceDisk.SwapSizeMB=2048 ## Huomautus: Tämä asettaminen riippumatta siitä, mitä tarvitset sitä voi.

13.  Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-pakottaa - deprovision
        # <a name="export-histsize0"></a>Vie HISTSIZE = 0
        # <a name="logout"></a>Kirjaudu ulos

14.  Valitse **Action -\> Sammuta** Hyper-V hallinnassa. Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.

## <a name="oracle-linux-70"></a>Oracle Linux 7.0 +
**Oracle Linux 7 muutokset**

Oracle Linux 7-virtuaalikoneen valmisteleminen Azure muistuttaa Oracle Linux 6-prosessi. Välillä on kuitenkin otettava huomioon useita eroja:

-   Punainen on yhteensopiva ydin ja Oracle's UEK3 tuetaan Azure. On suositeltavaa UEK3 ydin.

-   NetworkManager paketin ristiriidassa enää Azure Linux-agentti. Tämä paketti asennetaan oletusarvoisesti ja suosittelemme, että se ei poisteta.

-   GRUB2 käytetään nyt oletusarvo-Käynnistyslataimen niin menettelyä muokattavaksi ydin parametrit on muuttunut (Katso alla).

-   XFS on nyt käyttöjärjestelmän. Ext4 tiedostojärjestelmän voidaan edelleen tarvittaessa.

**Määritysvaiheet**

1.  Valitse virtuaalikoneen Hyper-V hallinta.

2.  Valitse **Yhdistä** Avaa virtuaalikoneen console-ikkuna.

3.  Luo tiedosto nimeltä **verkon** /etc/sysconfig/hakemistossa, joka sisältää seuraavan tekstin:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Luo-/etc/sysconfig/network-scripts **ifcfg eth0** -tiedosto tai kansio, joka sisältää seuraavan tekstin:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

5.  Siirrä udev säännöt tapahtuvien luodaan staattinen säännöt Ethernet-liittymän (tai poista). Sääntöjen aiheuttaa ongelmia, kun olet kloonaamalla virtual kone Microsoft Azure tai Hyper-V.

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2>/dev/null

6.  Varmista, että verkkopalvelu alkaa käynnistyksen yhteydessä suorittamalla seuraavan komennon:

        # sudo chkconfig network on

7.  Asenna python pyasn1 paketti suorittamalla seuraavan komennon:

        # sudo yum install python-pyasn1

8.  Suorita seuraava komento Poista nykyinen yum metatiedot ja asenna päivitykset:

        # sudo yum clean all
        # sudo yum -y update

9.  Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa "/ jne/oletusarvon/grub" tekstieditorissa ja Muokkaa GRUB\_CMDLINE\_LINUX-parametria, esimerkiksi:

        GRUB\_CMDLINE\_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"

    Näin varmistat myös konsolin viestit lähetetään ensimmäisen serial porttia, joka auttaa Azure tukevia virheenkorjaus ongelmat. Lisäksi edellä, suosittelemme, voit *poistaa* seuraavat parametrit:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.

    `crashkernel` Asetus on määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM. Tämä voi olla pienempi AM koot ongelmia.

10.  Kun olet lopettanut muokkaamisen "/ jne/oletusarvon/grub", suorita seuraava komento grub-määritys uudelleen:

        # <a name="sudo-grub2-mkconfig--o-bootgrub2grubcfg"></a>sudo grub2 mkconfig - o /boot/grub2/grub.cfg

11.  Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä. Tämä on yleensä oletusarvo.

12.  Asenna Azure Linux-agentti suorittamalla seuraavan komennon:

        # <a name="sudo-yum-install-walinuxagent"></a>sudo yum asennuksen WALinuxAgent

13.  Vaihda tila ei luoda OS-levylle.

    Azure Linux-agentti automaattisesti määrittää swap tilaa, joka on liitetty AM Azure-valmistelun jälkeen paikallinen resurssi-levyn avulla. Huomaa, että paikallinen resurssi-levy on *väliaikainen* DVD-levyllä, ja se voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Kun olet asentanut Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa /etc/waagent.conf seuraavien parametrien oikein:

        ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Huomautus: Tämä asettaminen riippumatta siitä, mitä tarvitset sitä voi.

14.  Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-pakottaa - deprovision
        # <a name="export-histsize0"></a>Vie HISTSIZE = 0
        # <a name="logout"></a>Kirjaudu ulos

15.  Valitse **Action -\> Sammuta** Hyper-V hallinnassa. Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.
