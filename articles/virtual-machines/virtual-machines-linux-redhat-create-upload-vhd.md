<properties
    pageTitle="Luo ja lataa punainen on Enterprise Linux Näennäiskiintolevyn Azure käytettäviksi"
    description="Opettele luomaan ja lataa Azure virtual kiintolevyn (Näennäiskiintolevyn), joka sisältää punainen on Linux-käyttöjärjestelmä."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/17/2016"
    ms.author="mingzhan"/>


# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Punainen on perustuva virtual machine valmisteleminen Azure

Tässä artikkelissa kerrotaan, punainen on Enterprise Linux (RHEL) virtual koneen valmistelemisesta käytettäviksi Azure. RHEL-versiota, joka on kuvattu tämän artikkelin ovat 6,7, 7.1 ja 7.2. Hypervisors valmistelun, joka on kuvattu tämän artikkelin on Hyper-V-ydin pohjainen virtuaalikoneen (KVM) ja VMware. Lisätietoja punainen on Pilvikäytön ohjelma osallistuminen kelpoisuusvaatimukset on artikkelissa [Punainen on Pilvikäytön verkkosivusto](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) - ja [Azure-käynnissä RHEL](https://access.redhat.com/articles/1989673).

[Valmistele RHEL 6,7 virtual koneen Hyper-V hallinnasta](#rhel67hyperv)

[Valmistele RHEL 7.1 ja 7.2 virtual koneen Hyper-V hallinnasta](#rhel7xhyperv)

[Valmistele RHEL 6,7 virtual kone-KVM](#rhel67kvm)

[Valmistele RHEL 7.1 ja 7.2 virtual kone-KVM](#rhel7xkvm)

[Valmistele RHEL 6,7 virtual kone-VMware](#rhel67vmware)

[Valmistele RHEL 7.1 ja 7.2 virtual kone-VMware](#rhel7xvmware)

[Valmistele RHEL 7.1 ja 7.2 virtual koneen kickstart-tiedostosta](#rhel7xkickstart)


## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Punainen on perustuva virtual machine valmisteleminen Hyper-V hallinnasta
### <a name="prerequisites"></a>Edellytykset
Tässä osassa oletetaan, että olet jo asentanut RHEL kuva (ISO-tiedostosta, punainen on sivuston saatu) virtual kiintolevylle (Näennäiskiintolevyn). Lisätietoja siitä, miten voit asentaa käyttöjärjestelmän kuvan Hyper-V hallinnan avulla on artikkelissa [Asenna Hyper-V rooli ja määritä Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).

**RHEL asennusohjeet**

- Katso myös [Yleinen Linux asennusohjeet](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) Linux valmisteleminen Azure Lisää vihjeitä.

- Azure VHDX uudempaan tiedostomuotoon ei tueta. Voit muuntaa levyn Näennäiskiintolevyn muotoon Hyper-V Manager tai **Muunna näennäiskiintolevyn** PowerShell-cmdlet-komennon avulla.

- Näennäiskiintolevyjen on luotava kiinteäksi""--dynaaminen näennäiskiintolevyjen ei tueta.

- Kun asennat Linux-järjestelmään, on suositeltavaa, että käytät vakio-osiosta LVM (usein useita asennusten oletusasetus) sijaan. Välttämiseksi LVM nimi on ristiriidassa kloonatun VMs, etenkin jos OS-levy on joskus liitettäväksi toiseen AM vianmääritystä varten. LVM tai [RAID](virtual-machines-linux-configure-raid.md) voidaan tietojen levyille, jos ensisijainen.

- Vaihda-osion ei määritetä OS levyn. Voit määrittää väliaikaisen resurssin levyn swap-tiedoston luominen Linux-agentti. Lisätietoja tästä on käytettävissä seuraavissa vaiheissa.

- Kaikki näennäiskiintolevyt on oltava koko on 1 Megatavu Kerrannaiset.

- Kun käytät **qemu img** levyn kuvien muuntaminen Näennäiskiintolevyn muotoon, Huomaa, että qemu img versioissa 2.2.1 on tunnettu ohjelmavirhe tai uudempi versio. Tämä ohjelmavirhe hakutulosten virheellisesti muotoiltu Näennäiskiintolevyn. Ongelman tarkoituksena on vahvistettava tulevista qemu img-versiossa. Nyt, suosittelemme, että käytät qemu-img 2.2.0 versio tai sitä aiemmissa versioissa.

### <a id="rhel67hyperv"> </a>Valmisteleminen RHEL 6,7 virtual koneen Hyper-V hallinnasta###


1.  Valitse virtuaalikoneen Hyper-V hallinta.

2.  Valitse **Yhdistä** Avaa virtuaalikoneen console-ikkuna.

3.  Poista NetworkManager suorittamalla seuraavan komennon:

        # sudo rpm -e --nodeps NetworkManager

    Huomaa, että jos paketti ei ole asennettu, tämä komento ei onnistu ja näyttöön tulee virhesanoma. Tämä on oikein.

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

6.  Siirrä udev säännöt tapahtuvien luodaan staattinen säännöt Ethernet-liittymän (tai poista). Sääntöjen aiheuttaa ongelmia, kun Kloonaa virtual kone Microsoft Azure tai Hyper-v

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Varmista, että verkkopalvelu alkaa käynnistyksen yhteydessä suorittamalla seuraavan komennon:

        # sudo chkconfig network on

8.  Rekisteröi punainen on tilauksen käyttöön RHEL säilöstä pakettien asentamista suorittamalla seuraavan komennon:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9.  WALinuxAgent paketin `WALinuxAgent-<version>` on ollut miten on punainen lisäominaisuudet säilöön. Ota käyttöön lisäominaisuudet säilö suorittamalla seuraavan komennon:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa `/boot/grub/menu.lst` tekstieditorissa ja varmista, että oletusarvoinen ydin sisältää seuraavat parametrit:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Tämä myös varmistaa, että kaikki konsolin viestit lähetetään ensimmäisen serial porttiin, joka auttaa Azure tukevia virheenkorjaus ongelmat. Tämä poistaa käytöstä NUMA ohjelmavirhe ydin-versiossa, jota käytetään RHEL 6 vuoksi.

    Lisäksi edellä toimintoa on suositeltavaa poistaa seuraavat parametrit:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.

    Crashkernel-vaihtoehto voi olla määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM. Tämä voi olla pienempi AM koot ongelmia.

11. Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä. Tämä on yleensä oletusarvo. Muokkaa /etc/ssh/sshd_config sisällytettävien seuraava rivi:

        ClientAliveInterval 180

12. Asenna Azure Linux-agentti suorittamalla seuraavan komennon:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

    Huomaa, että WALinuxAgent-paketin asennuksen poistaa NetworkManager NetworkManager gnome pakettien jos ne ole jo poistettiin ohjeiden vaiheessa 2.

13. Vaihda tila ei luoda OS-levylle.
Azure Linux-agentti automaattisesti määrittää Vaihda tila, joka on liitetty AM jälkeen AM on valmisteltu Azure-paikallinen resurssi-levyn avulla. Huomaa, että paikallinen resurssi-levy on väliaikainen DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Kun olet asentanut Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa /etc/waagent.conf seuraavien parametrien oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Unregister tilaus (tarvittaessa) suorittamalla seuraava komento:

        # sudo subscription-manager unregister

15. Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Valitse **toiminto > Sulje** Hyper-V hallinnassa. Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.
 

### <a id="rhel7xhyperv"> </a>Valmisteleminen RHEL 7.1 ja 7.2 virtual koneen Hyper-V hallinnasta###

1.  Valitse virtuaalikoneen Hyper-V hallinta.

2.  Valitse **Yhdistä** Avaa virtuaalikoneen console-ikkuna.

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

5.  Varmista, että verkkopalvelu alkaa käynnistyksen yhteydessä suorittamalla seuraavan komennon:

        # sudo chkconfig network on

6.  Rekisteröi on punainen tilauksen käyttöön RHEL säilöstä pakettien asentamista suorittamalla seuraavan komennon:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa `/etc/default/grub` tekstieditorissa ja muokkaa **GRUB_CMDLINE_LINUX** -parametrin. Esimerkki:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Tämä myös varmistaa, että kaikki konsolin viestit lähetetään ensimmäisen serial porttiin, joka auttaa Azure tukevia virheenkorjaus ongelmat. Lisäksi edellä toimintoa on suositeltavaa poistaa seuraavat parametrit:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.
    Crashkernel-vaihtoehto voi olla määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM. Tämä voi olla pienempi AM koot ongelmia.

8.  Kun olet valmis muokkaaminen `/etc/default/grub`, suorittamalla seuraavan komennon grub-määritys uudelleen:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9.  Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä. Tämä on yleensä oletusarvo. Muokkaa `/etc/ssh/sshd_config` sisällytettävien seuraava rivi:

        ClientAliveInterval 180

10. WALinuxAgent paketin `WALinuxAgent-<version>` on ollut miten punainen on lisäominaisuudet säilöön. Ota käyttöön lisäominaisuudet säilö suorittamalla seuraavan komennon:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Asenna Azure Linux-agentti suorittamalla seuraavan komennon:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

12. Vaihda tila ei luoda OS-levylle. Azure Linux-agentti automaattisesti määrittää Vaihda tila, joka on liitetty AM jälkeen AM on valmisteltu Azure-paikallinen resurssi-levyn avulla. Huomaa, että paikallinen resurssi-levy on väliaikainen DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Kun olet asentanut Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa seuraavien parametrien `/etc/waagent.conf` oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Jos haluat unregister tilaus, suorita seuraava komento:

        # sudo subscription-manager unregister

14. Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Valitse **toiminto > Sulje** Hyper-V hallinnassa. Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.
 


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Valmistele punainen on perustuva virtual machine-KVM

### <a id="rhel67kvm"> </a>Valmisteleminen RHEL 6,7 virtual kone-KVM###


1.  Lataa RHEL 6,7 KVM kuvan punainen on sivustosta.

2.  Pääkansio salasanan määrittäminen.

    Luo salattu salasana ja kopioi komennon:

        # openssl passwd -1 changeme

    Pääkansio salasanan kanssa guestfish määrittäminen:

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Muuta pääkansion käyttäjältä ja toinen kenttä-. " salattu salasana.

3.  Luoda virtual machine KVM qcow2 kuvasta, Aseta levy **qcow2**ja asettaminen **virtio**VPN-liittymän laitteen mallista. Valitse Käynnistä virtuaalikoneen ja kirjaudu ylimmällä tasolla.

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

6.  Siirtää udev sääntöjen välttämiseksi luodaan staattinen säännöt Ethernet-liittymän (tai poista). Sääntöjen aiheuttaa ongelmia, kun Kloonaa virtual kone Microsoft Azure tai Hyper-v

        # mkdir -m 0700 /var/lib/waagent
        # mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Varmista, että verkkopalvelu alkaa käynnistyksen yhteydessä suorittamalla seuraavan komennon:

        # chkconfig network on

8.  Rekisteröi punainen on tilauksen käyttöön RHEL säilöstä pakettien asentamista suorittamalla seuraavan komennon:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa `/boot/grub/menu.lst` tekstieditorissa ja varmista, että oletusarvoinen ydin sisältää seuraavat parametrit:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Tämä myös varmistaa, että kaikki konsolin viestit lähetetään ensimmäisen serial porttiin, joka auttaa Azure tukevia virheenkorjaus ongelmat. Tämä poistaa käytöstä NUMA ohjelmavirhe ydin-versiossa, jota käytetään RHEL 6 vuoksi.

    Lisäksi edellä toimintoa on suositeltavaa poistaa seuraavat parametrit:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.
    Crashkernel-asetus on määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM. Tämä voi olla pienempi AM koot ongelmia.

10. Lisää Hyper-V moduuleista initramfs:  

    Muokkaa `/etc/dracut.conf` ja sisällön: add_drivers += "hv_vmbus hv_netvsc hv_storvsc"

    Muodosta uudelleen initramfs: # dracut – f - v

11. Poista cloud alusta:

        # yum remove cloud-init

12. Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä:

        # chkconfig sshd on

    Muokkaa /etc/ssh/sshd_config sisällytettävien seuraavista riveistä:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Käynnistä sshd:

        # service sshd restart

13. WALinuxAgent paketin `WALinuxAgent-<version>` on ollut miten on punainen lisäominaisuudet säilöön. Ota käyttöön lisäominaisuudet säilö suorittamalla seuraavan komennon:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Asenna Azure Linux-agentti suorittamalla seuraavan komennon:

        # yum install WALinuxAgent
        # chkconfig waagent on

15. Azure Linux-agentti automaattisesti määrittää Vaihda tila, joka on liitetty AM jälkeen AM on valmisteltu Azure-paikallinen resurssi-levyn avulla. Huomaa, että paikallinen resurssi-levy on väliaikainen DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Kun olet asentanut Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa **/etc/waagent.conf** seuraavien parametrien oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Unregister tilaus (tarvittaessa) suorittamalla seuraava komento:

        # subscription-manager unregister

17. Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Sulje-KVM AM.

19. Qcow2 kuvan muuntaminen Näennäiskiintolevyn muoto.
    Kuvan muuntaminen ensin raaka muoto:

         # qemu-img convert -f qcow2 –O raw rhel-6.7.qcow2 rhel-6.7.raw
    Varmista, että raaka kuvan koko on kohdistettu 1 Megatavu. Pyöristää muussa tapauksessa voit tasata ja 1 Megatavu kokoa: # Mt = $((1024*1024)) # koon = $(qemu img tiedot -f raaka--tulosteen json "rhel-6.7.raw" | \ gawk ' vastaa ($0, / "virtual koon": ([0-9] +), /, val) {tulostaminen val[1]}') # rounded_size = $((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-6.7.raw $rounded_size

    Muunna levyn kiinteä kokoisen Näennäiskiintolevyn:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xkvm"> </a>Valmisteleminen RHEL 7.1 ja 7.2 virtual kone-KVM###


1.  Lataa KVM kuvan RHEL 7.1 (tai 7.2) on punainen-sivustosta. Käytämme RHEL 7.1 kuin seuraavassa esimerkissä.

2.  Pääkansio salasanan määrittäminen.

    Luo salattu salasana ja kopioi komennon:

        # openssl passwd -1 changeme

    Määritä guestfish pääkansion salasanassa.

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Muuta pääkansion käyttäjältä ja toinen kenttä-. " salattu salasana.

3.  Luoda virtual machine KVM qcow2 kuvasta, Aseta levy **qcow2**ja asettaminen **virtio**VPN-liittymän laitteen mallista. Valitse Käynnistä virtuaalikoneen ja kirjaudu ylimmällä tasolla.

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

6.  Varmista, että verkkopalvelu alkaa käynnistyksen yhteydessä suorittamalla seuraavan komennon:

        # chkconfig network on

7.  Rekisteröi on punainen tilauksen käyttöön asennuksen pakettien RHEL säilöstä suorittamalla seuraavan komennon:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8.  Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa `/etc/default/grub` tekstieditorissa ja muokkaa **GRUB_CMDLINE_LINUX** -parametrin. Esimerkki:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Tämä myös varmistaa, että kaikki konsolin viestit lähetetään ensimmäisen serial porttiin, joka auttaa Azure tukevia virheenkorjaus ongelmat. Lisäksi edellä toimintoa on suositeltavaa poistaa seuraavat parametrit:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.
    Crashkernel-vaihtoehto voi olla määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM. Tämä voi olla pienempi AM koot ongelmia.

9.  Kun olet valmis muokkaaminen `/etc/default/grub`, suorittamalla seuraavan komennon grub-määritys uudelleen:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Lisää Hyper-V moduulit initramfs:

    Muokkaa `/etc/dracut.conf` ja sisällön lisääminen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Muodosta uudelleen initramfs:

        # dracut –f -v

11. Poista cloud alusta:

        # yum remove cloud-init

12. Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä:

        # systemctl enable sshd

    Muokkaa /etc/ssh/sshd_config sisällytettävien seuraavista riveistä:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Käynnistä sshd:

        systemctl restart sshd

13. WALinuxAgent paketin `WALinuxAgent-<version>` on ollut miten on punainen lisäominaisuudet säilöön. Ota käyttöön lisäominaisuudet säilö suorittamalla seuraavan komennon:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Asenna Azure Linux-agentti suorittamalla seuraavan komennon:

        # yum install WALinuxAgent

    Ota käyttöön waagent-palvelu:

        # systemctl enable waagent.service

15. Vaihda tila ei luoda OS-levylle. Azure Linux-agentti automaattisesti määrittää Vaihda tila, joka on liitetty AM jälkeen AM on valmisteltu Azure-paikallinen resurssi-levyn avulla. Huomaa, että paikallinen resurssi-levy on väliaikainen DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Kun olet asentanut Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa seuraavien parametrien `/etc/waagent.conf` oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Unregister tilaus (tarvittaessa) suorittamalla seuraava komento:

        # subscription-manager unregister

17. Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Sulje-KVM virtuaalikoneen.

19. Qcow2 kuvan muuntaminen Näennäiskiintolevyn muoto.

    Kuvan muuntaminen ensin raaka muoto:

         # qemu-img convert -f qcow2 –O raw rhel-7.1.qcow2 rhel-7.1.raw

    Varmista, että raaka kuvan koko on kohdistettu 1 Megatavu. Muussa tapauksessa pyöristää haluat tasata ja 1 Megatavu koon seuraavasti:

         # MB=$((1024*1024))
         # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                  gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
         # rounded_size=$((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-7.1.raw $rounded_size

    Muunna levyn kiinteä kokoisen Näennäiskiintolevyn:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Valmistele punainen on perustuva virtual machine-VMware
### <a name="prerequisites"></a>Edellytykset
Tässä osassa oletetaan, että olet jo asentanut RHEL virtual kone-VMware. Lisätietoja siitä, miten voit asentaa käyttöjärjestelmän VMware oppaassa [VMware vieras käyttöjärjestelmän version asennus](http://partnerweb.vmware.com/GOSIG/home.html).

- Kun asennat Linux-käyttöjärjestelmä, suosittelemme, että käytät vakio-osiosta LVM (usein useita asennusten oletusasetus) sijaan. Välttämiseksi LVM nimi on ristiriidassa kloonatun VMs, etenkin jos OS-levy on joskus liitettäväksi toiseen AM vianmääritystä varten. LVM tai RAID voidaan tietojen levyille, jos ensisijainen.

- Vaihda-osion ei määritetä OS levyn. Voit määrittää väliaikaisen resurssin levyn swap-tiedoston luominen Linux-agentti. Löydät lisätietoja tästä seuraavien vaiheiden kuvissa.

- Kun luot virtual kovalevy, valitse **kaupan virtual levy yhtenä tiedostona**.



### <a id="rhel67vmware"> </a>Valmisteleminen RHEL 6,7 virtual kone-VMware###

1.  Poista NetworkManager suorittamalla seuraavan komennon:

         # sudo rpm -e --nodeps NetworkManager

    Huomaa, että jos paketti ei ole asennettu, tämä komento ei onnistu ja näyttöön tulee virhesanoma. Tämä on oikein.

2.  Luo tiedosto nimeltä **verkon** /etc/sysconfig/hakemistossa, joka sisältää seuraavan tekstin:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3.  Luo-/etc/sysconfig/network-scripts **ifcfg eth0** -tiedosto tai kansio, joka sisältää seuraavan tekstin:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4.  Siirtää udev sääntöjen välttämiseksi luodaan staattinen säännöt Ethernet-liittymän (tai poista). Sääntöjen aiheuttaa ongelmia, kun Kloonaa virtual kone Microsoft Azure tai Hyper-v

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

5.  Varmista, että verkkopalvelu alkaa käynnistyksen yhteydessä suorittamalla seuraavan komennon:

        # sudo chkconfig network on

6.  Rekisteröi punainen on tilauksen käyttöön RHEL säilöstä pakettien asentamista suorittamalla seuraavan komennon:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  WALinuxAgent paketin `WALinuxAgent-<version>` on ollut miten on punainen lisäominaisuudet säilöön. Ota käyttöön lisäominaisuudet säilö suorittamalla seuraavan komennon:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8.  Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa "/ boot/grub/menu.lst" tekstieditorissa ja varmista, että oletusarvoinen ydin sisältää seuraavat parametrit:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Tämä myös varmistaa, että kaikki konsolin viestit lähetetään ensimmäisen serial porttiin, joka auttaa Azure tukevia virheenkorjaus ongelmat. Tämä poistaa käytöstä NUMA ohjelmavirhe ydin-versiossa, jota käytetään RHEL 6 vuoksi.
    Lisäksi edellä toimintoa on suositeltavaa poistaa seuraavat parametrit:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.
    Crashkernel-vaihtoehto voi olla määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM. Tämä voi olla pienempi AM koot ongelmia.

9.  Lisää Hyper-V moduuleista initramfs:

        Edit `/etc/dracut.conf` and add content:

            add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

        Rebuild initramfs:

            # dracut –f -v

10. Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä. Tämä on yleensä oletusarvo. Muokkaa `/etc/ssh/sshd_config` sisällytettävien seuraava rivi:

        ClientAliveInterval 180

11. Asenna Azure Linux-agentti suorittamalla seuraavan komennon:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

12. Älä luo Vaihda tilaa OS levyn:

    Azure Linux-agentti automaattisesti määrittää Vaihda tila, joka on liitetty AM jälkeen AM on valmisteltu Azure-paikallinen resurssi-levyn avulla. Huomaa, että paikallinen resurssi-levy on väliaikainen DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Kun olet asentanut Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa seuraavien parametrien `/etc/waagent.conf` oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Unregister tilaus (tarvittaessa) suorittamalla seuraava komento:

        # sudo subscription-manager unregister

14. Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Sulje AM ja VMDK-tiedoston muuntaminen .vhd-tiedosto.

    Kuvan muuntaminen ensin raaka muoto:

        # qemu-img convert -f vmdk –O raw rhel-6.7.vmdk rhel-6.7.raw

    Varmista, että raaka kuvan koko on kohdistettu 1 Megatavu. Muussa tapauksessa pyöristää haluat tasata ja 1 Megatavu koon seuraavasti:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.7.raw" | \
                gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.7.raw $rounded_size

    Muunna levyn kiinteä kokoisen Näennäiskiintolevyn:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xvmware"> </a>Valmisteleminen RHEL 7.1 ja 7.2 virtual kone-VMware###

1.  Luo tiedosto nimeltä **verkon** /etc/sysconfig/hakemistossa, joka sisältää seuraavan tekstin:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2.  Luo-/etc/sysconfig/network-scripts **ifcfg eth0** -tiedosto tai kansio, joka sisältää seuraavan tekstin:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

3.  Varmista, että verkkopalvelu alkaa käynnistyksen yhteydessä suorittamalla seuraavan komennon:

        # sudo chkconfig network on

4.  Rekisteröi punainen on tilauksen käyttöön RHEL säilöstä pakettien asentamista suorittamalla seuraavan komennon:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5.  Muokkaa grub-kokoonpanoa, jotka haluat lisätä muita ydin parametrit Azure ydin käynnistys-riville. Voit tehdä tämän Avaa `/etc/default/grub` tekstieditorissa ja muokkaa **GRUB_CMDLINE_LINUX** -parametrin. Esimerkki:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Tämä myös varmistaa, että kaikki konsolin viestit lähetetään ensimmäisen serial porttiin, joka auttaa Azure tukevia virheenkorjaus ongelmat. Lisäksi edellä toimintoa on suositeltavaa poistaa seuraavat parametrit:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.
    Crashkernel-vaihtoehto voi olla määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM. Tämä voi olla pienempi AM koot ongelmia.

6.  Kun olet valmis muokkaaminen `/etc/default/grub`, suorittamalla seuraavan komennon grub-määritys uudelleen:

         # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7.  Lisää Hyper-V moduuleista initramfs:

    Muokkaa `/etc/dracut.conf`, Lisää sisältö:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Muodosta uudelleen initramfs:

        # dracut –f -v

8.  Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä. Tämä on yleensä oletusarvo. Muokkaa `/etc/ssh/sshd_config` sisällytettävien seuraava rivi:

        ClientAliveInterval 180

9.  WALinuxAgent paketin `WALinuxAgent-<version>` on ollut miten on punainen lisäominaisuudet säilöön. Ota käyttöön lisäominaisuudet säilö suorittamalla seuraavan komennon:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Asenna Azure Linux-agentti suorittamalla seuraavan komennon:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

11. Vaihda tila ei luoda OS-levylle. Azure Linux-agentti automaattisesti määrittää Vaihda tila, joka on liitetty AM jälkeen AM on valmisteltu Azure-paikallinen resurssi-levyn avulla. Huomaa, että paikallinen resurssi-levy on väliaikainen DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Kun olet asentanut Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa seuraavien parametrien `/etc/waagent.conf` oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Jos haluat unregister tilaus, suorita seuraava komento:

        # sudo subscription-manager unregister

13. Suorita seuraavat komennot deprovision virtuaalikoneen ja valmisteleminen lähettäväksi Azure-valmistelu:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Sulje AM ja VMDK-tiedoston muuntaminen Näennäiskiintolevyn muoto.

    Kuvan muuntaminen ensin raaka muoto:

        # qemu-img convert -f vmdk –O raw rhel-7.1.vmdk rhel-7.1.raw

    Varmista, että raaka kuvan koko on kohdistettu 1 Megatavu. Muussa tapauksessa pyöristää haluat tasata ja 1 Megatavu koon seuraavasti:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                 gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.1.raw $rounded_size

    Muunna levyn kiinteä kokoisen Näennäiskiintolevyn:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Valmistele punainen on perustuva virtual machine ISO-automaattisesti kickstart-tiedoston avulla


### <a id="rhel7xkickstart"> </a>Valmisteleminen RHEL 7.1 ja 7.2 virtual koneen kickstart-tiedostosta###


1.  Luo kickstart-tiedosto, jonka alla ja Tallenna tiedosto. Lisätietoja kickstart asennuksesta on artikkelissa [Kickstart oppaaseen](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).



        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
        auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff



        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=yes
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2.  Aseta kickstart-tiedoston sijainti, joita voit käyttää asennus-järjestelmästä.

3.  Hyper-V-hallintaan ja luo uusi AM. **Yhdistä Virtual kiintolevyn** sivulla Valitse **Liitä virtual kiintolevyn myöhemmin**ja suorita ohjattu virtuaalikoneen.

4.  Avaa AM-asetukset:

    a.  Liittää uuden virtual kiintolevyn AM. Varmista, että valitset **Näennäiskiintolevyn muoto** ja **Kiinteän kokoinen**.

    b.  Liittää asennuksen ISO DVD-asemaan.

    c-näppäinyhdistelmää.  Määritä BIOSISTA CD-levyltä.

5.  Aloita AM. Kun asennusopas tulee näkyviin, painamalla **välilehti** käynnistys-asetusten määrittäminen.

6.  ENTER `inst.ks=<the location of the kickstart file>` lopussa käynnistysasetuksia ja paina **Enter**-näppäintä.

7.  Odota, viimeistele asennus. Kun se on valmis, AM suljetaan automaattisesti. Linux Näennäiskiintolevyn on nyt valmis ladattava Azure.

## <a name="known-issues"></a>Tunnetut ongelmat
Aiheuttavat tunnetusti ongelmia käyttäessäsi RHEL 7.1 Hyper-V ja Azure.

### <a name="disk-io-freeze"></a>Levyn i/o kiinnittäminen

Tämä ongelma voi ilmetä usein käytetyt tallennustilan levyn i/o sisältävistä RHEL 7.1 Hyper-V ja Azure aikana.   

Ongelman kuvaus korko:

Tämä ongelma on katkonainen. Kuitenkin se tapahtuu Lisää usein aikana säännöllisempää levyn i/o-toimintojen Hyper-V ja Azure.   


[AZURE.NOTE] Tämä tunnettu ongelma on jo punainen on osoitettu. Voit asentaa liittyvät korjaukset suorittamalla seuraavan komennon:

    # sudo yum update

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Hyper-V-ohjain ei voitu sisällyttää alkuperäisen RAM-levy-, Hyper V hypervisor käytettäessä

Joissakin tapauksissa Linux asennusohjelmia ehkä Sisällytä ohjaimet, Hyper-V alkuperäinen RAM-levy (initrd tai initramfs) paitsi, jos se havaitsee, että se on käynnissä Hyper-V-ympäristössä.

Kun käytät eri virtualisointi-järjestelmän (eli Virtualbox, Xen, jne.) valmisteleminen Linux-kuva, voit joutua muodostamaan initrd varmistaa, että vähintään hv_vmbus ja hv_storvsc ydin moduulit ovat käytettävissä alkuperäinen RAM-Muistia levyllä. Tämä on tunnettu ongelma vähintään järjestelmiin ylöspäin punainen on jakauman perusteella.

Voit ratkaista ongelman, sinun on lisätä Hyper-V moduulit initramfs ja muodostaa siihen:

Muokkaa `/etc/dracut.conf` ja sisällön lisääminen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

Muodosta uudelleen initramfs:

        # dracut –f -v

Lisätietoja on artikkelissa tietoja [initramfs muodostetaan uudelleen](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Seuraavat vaiheet
Voit nyt punainen on Enterprise Linux virtual kiintolevyn avulla voit luoda uuden näennäiskoneiden Azure-tietokannassa. Jos kyseessä on, että lataamasi .vhd-tiedoston Azure ensimmäistä kertaa, katso vaiheet 2 ja 3 [luominen ja lataamisen virtual kiintolevyn, joka sisältää Linux-käyttöjärjestelmä](virtual-machines-linux-classic-create-upload-vhd.md).

Katso tarkempia tietoja, jotka ovat sertifioituja punainen on Enterprise Linux hypervisors [Punainen on-sivustossa](https://access.redhat.com/certified-hypervisors).
