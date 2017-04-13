<properties
    pageTitle="Luo ja lataa SUSE Linux Näennäiskiintolevyn Azure-tietokannassa"
    description="Opettele luomaan ja lataa Azure virtual kiintolevyn (Näennäiskiintolevyn), joka sisältää SUSE Linux-käyttöjärjestelmä."
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

# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Azure SLES tai openSUSE virtual machine valmisteleminen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Edellytykset ##

Tässä artikkelissa oletetaan, että olet jo asentanut SUSE tai Linux-käyttöjärjestelmän openSUSE virtual kiintolevylle. Useita työkaluja .vhd-tiedostoja, kuten virtualization ratkaista esimerkiksi Hyper-V luomiseen. Katso ohjeet [Hyper-V rooli ja määritä Virtual Machine asentaminen](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE asennusohjeet

- Katso myös [Yleinen Linux asennusohjeet](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) Linux valmisteleminen Azure Lisää vihjeitä.

- Azure-vain **Kiinteä Näennäiskiintolevyn**ei tueta VHDX-muodossa.  Voit muuntaa levyn Näennäiskiintolevyn muotoon Hyper-V Manager tai Muunna näennäiskiintolevyn cmdlet-komennon avulla.

- Asennettaessa Linux-järjestelmä on suositeltavaa, että käytät vakio-osiosta LVM (usein useita asennusten oletusasetus) sijaan. Välttämiseksi LVM nimi on ristiriidassa kloonatun VMs, etenkin jos OS-levy on joskus liitettäväksi toiseen AM vianmääritystä varten. [LVM](virtual-machines-linux-configure-lvm.md) tai [RAID](virtual-machines-linux-configure-raid.md) voidaan tietojen levyille, jos ensisijainen.

- Vaihda-osion ei määritetä OS levyn. Vaihda tiedoston tilapäinen resurssin levyn luominen voi määrittää Linux-agentti.  Lisätietoja tästä löytyvät alla kuvatulla tavalla.

- Kaikki näennäiskiintolevyt on oltava koko on 1 Megatavu Kerrannaiset.


## <a name="use-suse-studio"></a>SUSE Studiolla
[SUSE Studio](http://www.susestudio.com) voit helposti luoda ja hallita SLES ja openSUSE kuvien Azure ja Hyper-V. Tämä on suositellut lähestymistapa mukauttamiseen SLES ja openSUSE omia kuvia.

Vaihtoehtona rakentaminen oman Näennäiskiintolevyn SUSE julkaisee myös BYOS (Tuo Your oman tilausta) kuvia SLES osoitteessa [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007)varten.


## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>Valmistele SUSE Linux Enterprise Server 11 SP4 ##

1. Valitse keskimmäisessä ruudussa Hyper-V hallinnan, virtuaalikoneen.

2. Valitse **Yhdistä** Avaa virtuaalikoneen-ikkuna.

3. Rekisteröi SUSE Linux Enterprise-järjestelmään, jotta se päivitykset ladataan ja asennetaan paketit.

4. Päivitä järjestelmä uusimmat korjaukset:

        # sudo zypper update

5. Asenna Azure Linux-agentti SLES säilöstä.

        # sudo zypper install WALinuxAgent

6. Jos waagent määritetään "on" Kuittaa sisään chkconfig ja jos ei salli sen määrittää:
               
        # sudo chkconfig waagent on

7. Tarkista, jos waagent palvelu on käynnissä, ja jos ei, käynnistä se: 

        # sudo service waagent start
                
8. Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa "/ boot/grub/menu.lst" tekstieditorissa ja varmista, että oletusarvoinen ydin sisältää seuraavat parametrit:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Näin varmistat, konsolin viestit lähetetään ensimmäinen sarja portti, joka auttaa Azure tukevia virheenkorjaus ongelmat.

9. Varmista, että /boot/grub/menu.lst /etc/fstab sekä viitetietojen ja sen UUID (mukaan-uuid) sen sijaan, että levyn tunnus (mukaan-id) levy. 

    Hae levyn UUID
    
        # ls /dev/disk/by-uuid/

    Jos /dev/disk/by-id / on käytössä, päivittää sekä /boot/grub/menu.lst että/jne/fstab ERISNIMI uuid mukaan arvolla

    Ennen kuin muutos
    
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1

    Muutoksen jälkeen
    
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

10. Muokkaa udev säännöt tapahtuvien Ethernet-liittymän staattinen säännöt luodaan. Sääntöjen saattaa aiheuttaa ongelmia, kun kloonaamalla virtual kone Microsoft Azure tai Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

11. Se on suositellut tiedoston muokkaaminen "/ jne/sysconfig/verkon/dhcp" ja muuta `DHCLIENT_SET_HOSTNAME` parametri seuraavasti:

        DHCLIENT_SET_HOSTNAME="no"

12. "/ Jne/sudoers" kommentoi pois tai poistaa seuraavat rivit, jos ne ovat olemassa:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

13. Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä.  Tämä on yleensä oletusarvo.

14. Vaihda tila ei luoda OS-levylle.

    Azure Linux-agentti automaattisesti määrittää swap-tallennustilasta on käytössä paikallinen resurssi levy, johon on liitetty AM Azure-valmistelun jälkeen. Huomaa, että paikallinen resurssi-levy on *väliaikainen* DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Asennettuasi Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa /etc/waagent.conf seuraavien parametrien oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15. Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Valitse Hyper-V Manager- **toiminto -> Sammuta alaspäin** . Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.


----------

## <a name="prepare-opensuse-131"></a>Valmistele openSUSE 13.1 + ##

1. Valitse keskimmäisessä ruudussa Hyper-V hallinnan, virtuaalikoneen.

2. Valitse **Yhdistä** Avaa virtuaalikoneen-ikkuna.

3. Valitse shell-komennon suorittaminen '`zypper lr`". Jos tämä komento palauttaa tulosteen seuraavanlainen ja valitse säilöjen tietoihin on määritetty oikein--ei muutoksia ei tarvita (Huomaa, että versioita voivat vaihdella):

        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes

    Jos komento palauttaa "Ei säilöjen tietoihin määritetty..." Lisää nämä repos Käytä seuraavia komentoja:

        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates

    Voit tarkistaa sitten säilöjen tietoihin on lisätty suorittamalla komennon "`zypper lr`' uudelleen. Siltä varalta, että jokin sopiva päivitys säilöjen tietoihin ei ole käytössä, ota se seuraava komento:

        # sudo zypper mr -e [NUMBER OF REPOSITORY]


4. Päivitä ydin käytettävissä versioon:

        # sudo zypper up kernel-default

    Tai Päivitä järjestelmän uusimman korjaukset seuraavasti:

        # sudo zypper update

5.  Asenna Azure Linux-agentti.

        # sudo zypper install WALinuxAgent

6.  Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa "/ boot/grub/menu.lst" tekstieditorissa ja varmista, että oletusarvoinen ydin sisältää seuraavat parametrit:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Näin varmistat, konsolin viestit lähetetään ensimmäinen sarja portti, joka auttaa Azure tukevia virheenkorjaus ongelmat. Poista lisäksi seuraavat parametrit ydin käynnistys-riviltä, jos ne ovat olemassa:

        libata.atapi_enabled=0 reserve=0x1f0,0x8

7.  Se on suositellut tiedoston muokkaaminen "/ jne/sysconfig/verkon/dhcp" ja muuta `DHCLIENT_SET_HOSTNAME` parametri seuraavasti:

        DHCLIENT_SET_HOSTNAME="no"

8.  **Tärkeä:** "/ Jne/sudoers" kommentoi pois tai poistaa seuraavat rivit, jos ne ovat olemassa:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä.  Tämä on yleensä oletusarvo.

10. Vaihda tila ei luoda OS-levylle.

    Azure Linux-agentti automaattisesti määrittää swap-tallennustilasta on käytössä paikallinen resurssi levy, johon on liitetty AM Azure-valmistelun jälkeen. Huomaa, että paikallinen resurssi-levy on *väliaikainen* DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Asennettuasi Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa /etc/waagent.conf seuraavien parametrien oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. Varmista Azure Linux-agentti suoritetaan käynnistyksen yhteydessä:

        # sudo systemctl enable waagent.service

13. Valitse Hyper-V Manager- **toiminto -> Sammuta alaspäin** . Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.

## <a name="next-steps"></a>Seuraavat vaiheet
Voit nyt SUSE Linux virtual kiintolevyn avulla voit luoda uuden näennäiskoneiden Azure-tietokannassa. Jos kyseessä on, että lataamasi .vhd-tiedoston Azure ensimmäistä kertaa, katso vaiheet 2 ja 3 [luominen ja lataamisen virtual kiintolevyn, joka sisältää Linux-käyttöjärjestelmä](virtual-machines-linux-classic-create-upload-vhd.md).
