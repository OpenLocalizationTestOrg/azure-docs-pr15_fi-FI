<properties
    pageTitle="Luo ja lataa Linux Näennäiskiintolevyn Azure-tietokannassa"
    description="Opettele luomaan ja lataa Azure virtual kiintolevyn (Näennäiskiintolevyn), joka sisältää Linux-käyttöjärjestelmä."
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
    ms.date="09/23/2016"
    ms.author="szark"/>

# <a name="information-for-non-endorsed-distributions"></a>Tietoja ei ole vahvistanut jakelu #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


**Tärkeää**: Azure-ympäristö SLA koskee näennäiskoneiden Linux OS vain, kun jokin [vahvistettava jaot](virtual-machines-linux-endorsed-distros.md) käytetään. Linux jaot, joka on tarkoitettu Azure kuvavalikoima on vahvistettu jaot tarvittavat määrityksissä.

- [Valitse Azure - Linux vahvistettava jakelu](virtual-machines-linux-endorsed-distros.md)
- [Microsoft Azure Linux kuvat tuki](https://support.microsoft.com/kb/2941892)

Azure-käyttöjärjestelmässä jaot on täytettävä tietyt edellytykset, jotta voit suorittaa oikein ympäristössä.  Tässä artikkelissa ei ole mukaan tavoin täydellinen jokaisen jakauman on eri; ja jos se on aivan mahdollista, vaikka kaikki alla ehdot täyttyvät edelleen tarvitset merkittävästi säätää tehosteasetuksilla Linux järjestelmän varmistaa, että se toimii oikein ympäristössä.

On tästä syystä, on suositeltavaa, että käynnistät mainitun Microsoftin [Linux Azure vahvistettava jaot,](virtual-machines-linux-endorsed-distros.md) kun se on mahdollista. Seuraavissa artikkeleissa opastaa sinua valmistelemisesta eri vahvistettu Linux jaot, joita tuetaan Azure:

- **[CentOS perustuva jakelu](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Punainen on Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**

Muiden tässä artikkelissa keskitytään yleisiä ohjeita käytössä Azure Linux-jakauman.


## <a name="general-linux-installation-notes"></a>Yleiset Linux asennusohjeet ##

- Azure-vain **Kiinteä Näennäiskiintolevyn**ei tueta VHDX-muodossa.  Voit muuntaa levyn Näennäiskiintolevyn muotoon Hyper-V Manager tai Muunna näennäiskiintolevyn cmdlet-komennon avulla. Jos käytössäsi on VirtualBox siis valitsemalla **kiinteän kokoinen** eikä varataan dynaamisesti luotaessa levyn oletusarvo.

- Kun asennat Linux-järjestelmän on *suositeltavaa* , että käytät vakio-osiosta LVM (usein useita asennusten oletusasetus) sijaan. Välttämiseksi LVM nimi on ristiriidassa kloonatun VMs, etenkin jos OS-levy on joskus liitettäväksi toiseen samanlaiset AM vianmääritystä varten. [LVM](virtual-machines-linux-configure-lvm.md) tai [RAID](virtual-machines-linux-configure-raid.md) voidaan käyttää tietojen levyille.

- Ydin tuki käyttöönottaminen UDF tiedostojärjestelmää tarvitaan. Ensimmäisen käynnistyksen Azure-valmistelu määritysten siirretään Linux AM, UDF muotoiltujen media, joka on liitetty Vierailijan kautta. Azure Linux-agentti voivat lukea sen määritykset ja valmistella AM UDF-tiedostojärjestelmän käyttöön.

- Linux ydin versiot 2.6.37 alla eivät tue NUMA Hyper-V, joiden koko on suurempi AM. Tämän ongelman ensisijaisesti vaikutukset vanhempia jaot avulla seuraavat punainen on 2.6.32 ydin ja on korjattu RHEL 6.6 (ydin-2.6.32-504). Järjestelmiin mukautetun ytimet vanhempia 2.6.37 tai RHEL perustuva ytimet vanhempi kuin 2.6.32-504 käynnistys-parametri on määritettävä `numa=off` komentorivin grub.conf ydin käyttöön. Katso lisätietoja punainen on [Knowledge Base-436883](https://access.redhat.com/solutions/436883).

- Vaihda-osion ei määritetä OS levyn. Vaihda tiedoston tilapäinen resurssin levyn luominen voi määrittää Linux-agentti.  Lisätietoja tästä löytyvät alla kuvatulla tavalla.

- Kaikki näennäiskiintolevyt on oltava koko on 1 Megatavu Kerrannaiset.


### <a name="installing-linux-without-hyper-v"></a>Asentaminen Linux ilman Hyper-V ###

Joissakin tapauksissa Linux asennusohjelmia ei ehkä sisällä ohjaimet, Hyper-V alkuperäinen ramdisk (initrd tai initramfs) paitsi, jos se havaitsee, että se on käynnissä Hyper-V-ympäristössä.  Kun eri virtualisointi-järjestelmän (eli Virtualbox, KVM, jne.) avulla voit laatia Linux-kuva, voit joutua muodostamaan initrd varmistaa, että vähintään `hv_vmbus` ja `hv_storvsc` ydin moduulit ovat käytettävissä alkuperäinen ramdisk.  Tämä on tunnettu ongelma vähintään järjestelmiin ylöspäin punainen on jakauman perusteella.

Järjestelmä, joiden initrd tai initramfs kuvan voivat vaihdella jakauman mukaan. Oman jakauman käyttöohjeista tai tuki ERISNIMI kuvatulla tavalla.  Seuraavassa on esimerkki siitä, miten uudelleen käyttämällä initrd `mkinitrd` apuohjelma:

Varmuuskopioi ensin initrd kuvaan:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Muodostaa seuraavaksi initrd kanssa `hv_vmbus` ja `hv_storvsc` ydin moduulit:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Näennäiskiintolevyjen koon muuttaminen ###

Azure Näennäiskiintolevyn kuvat on oltava virtual koko on tasattu 1 Megatavu.  Yleensä näennäiskiintolevyjen Hyper-V-sovelluksella luodut jo tasataan oikein.  Jos Näennäiskiintolevyn ei ole tasattu oikein seuraavanlainen virhesanoma saattaa tulla kun yrität luoda oman Näennäiskiintolevyn *Kuva* :

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Voit korjata ongelman, voit sovittaa AM Hyper-V hallinta-konsolin tai [Koon Näennäiskiintolevyn](http://technet.microsoft.com/library/hh848535.aspx) Powershell cmdlet-komennon avulla.  Jos käytössäsi ei ole Windows-ympäristössä sitten on suositeltavaa qemu img avulla voit muuntaa (tarvittaessa) ja koon muuttaminen Näennäiskiintolevyn.

> [AZURE.NOTE] On tunnettu ohjelmavirhe qemu img versioissa > = 2.2.1, jonka tuloksena on virheellisesti muotoiltu Näennäiskiintolevyn. Ongelma on korjattu QEMU 2.6. On suositeltavaa käyttää qemu-img 2.2.0 tai alemman tai päivittää 2.6 tai uudempi versio. Viite: https://bugs.launchpad.net/qemu/+bug/1490611.


 1. Koon muuttaminen suoraan työkaluilla, kuten Näennäiskiintolevyn `qemu-img` tai `vbox-manage` voi aiheuttaa käynnistämisen Näennäiskiintolevyn.  Niin kannattaa ensimmäisen Muunna Näennäiskiintolevyn levyn kuva.  Jos AM-kuva on jo luotu levyn näköistiedoston (kuten KVM joitakin Hypervisors oletusarvo) voivat ohittaa tämän vaiheen:

        # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. Laske levy-kuvaa, jos haluat varmistaa, että virtual kokoa on tasattu 1 Megatavu tarvittavat kokoa.  Seuraava bash liittymän komentosarja voi auttaa tämän.  Komentosarja käyttää "`qemu-img info`" levyn näköistiedoston virtual koon määrittäminen ja laskee seuraavan 1 Megatavu kooksi:

        rawdisk="MyLinuxVM.raw"
        vhddisk="MyLinuxVM.vhd"

        MB=$((1024*1024))
        size=$(qemu-img info -f raw --output json "$rawdisk" | \
               gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        rounded_size=$((($size/$MB + 1)*$MB))
        echo "Rounded Size = $rounded_size"

 3. Muuta kokoa joukkona yllä komentosarjan $rounded_size raaka levy:

        # qemu-img resize MyLinuxVM.raw $rounded_size

 4. Muunna levyn takaisin kiinteä Näennäiskiintolevyn:

        # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## <a name="linux-kernel-requirements"></a>Linux ydin vaatimukset ##

Linux Integration Services (LIS) ohjaimet Hyper-V ja Azure suorittaneet suoraan ylöspäin Linux ydin. Monta jaot, jotka sisältävät viimeisimmät Linux Ydinversio (eli 3.x) ohjaimia käytettävissä vielä ole tai muussa antaa backported versioiden ohjaimia niiden ytimet.  Nämä ohjaimet päivitetään-korjaukset ja ominaisuuksista, seuraavat ytimessä jatkuvasti, niin kun mahdollista kannattaa suorittaa [vahvistettava jakauman](virtual-machines-linux-endorsed-distros.md) , joka sisältää nämä korjaukset ja päivitykset.

Jos käytössäsi on variantin punainen on Enterprise Linux versioiden **6.0 6.3**, täytyy asentaa LIS uusimmat ohjaimet Hyper-V varten. Ohjaimet löytyy [tähän sijaintiin](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). RHEL **6.4 +** (ja johdannaiset) LIS ohjaimet ovat jo mukana ydin ja siten ei ole muita asennuksen paketteja tarvitaan voidaan suorittaa järjestelmät Azure.

Jos mukautetun ydin on tehtävä, on suositeltavaa käyttää ydin uudemman version (eli **3.8 +**). Näiden jaot tai toimittajien kuka ylläpitää omia ydin, jotkin työmäärään on pakollinen säännöllisesti backport LIS ohjaimet-mukautetun ydin ylöspäin ydin.  Vaikka käytössäsi on jo suhteellisen uusi Ydinversio, on erittäin suositeltavaa seurantaan ylöspäin korjauksia LIS ohjaimet ja backport niitä tarvittaessa. LIS ohjaimen lähde-tiedostojen sijainti on käytettävissä Linux ydin lähde puun [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) -tiedosto:

    F:  arch/x86/include/asm/mshyperv.h
    F:  arch/x86/include/uapi/asm/hyperv.h
    F:  arch/x86/kernel/cpu/mshyperv.c
    F:  drivers/hid/hid-hyperv.c
    F:  drivers/hv/
    F:  drivers/input/serio/hyperv-keyboard.c
    F:  drivers/net/hyperv/
    F:  drivers/scsi/storvsc_drv.c
    F:  drivers/video/fbdev/hyperv_fb.c
    F:  include/linux/hyperv.h
    F:  tools/hv/

Erittäin pienin seuraavat korjaukset puuttuminen tiedetään aiheuttaa ongelmia Azure- ja siten nämä täytyy sisältyä ydin. Tämä luettelo ei mukaan tavoin monipuolisen tai jaot valmis:

- [ata_piix: myöhemmäksi oletusarvoisesti levyjen Hyper-V-ohjain](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [storvsc: Palauta PATH-siirrossa pakettien tili](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [storvsc: välttää WRITE_SAME käyttö](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [storvsc: käytöstä KIRJOITTAA SAMAA RAID ja virtual host verkkosovittimen ohjaimet](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [storvsc: NULL-osoitin dereference korjaaminen](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [storvsc: soi puskurin virheitä saattaa johtaa i/o kiinnittäminen](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
- [scsi_sysfs: suojautua __scsi_remove_device Sulje suorittaminen](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)


## <a name="the-azure-linux-agent"></a>Azure Linux-agentti ##

[Azure Linux agentti](virtual-machines-linux-agent-user-guide.md) (waagent) vaaditaan valmistelu oikein Linux virtual kone Azure-tietokannassa. Voit hankkia uusimman version, tiedoston ongelmia tai Lähetä [Linux agentti GitHub repo](https://github.com/Azure/WALinuxAgent)pyyntöjä salaus puretaan.

- Linux-agentti on julkaistu Apache 2.0-käyttöoikeus. Monta jaot jo sisältävät JÄLLEENMYYNTIHINNAN tai deb pakettien agentti ja siten joissakin tapauksissa tämä voidaan asentaa ja päivittää little työmäärään.

- Azure Linux-agentti edellyttää Python v2.6 +.

- Agentti edellyttää myös python pyasn1-moduuli. Useimmat jaot antaa tämä asennettuina voi olla erilliset pakettina.

- Joissakin tapauksissa Azure Linux-agentti ei ehkä NetworkManager-yhteensopiva. JÄLLEENMYYNTIHINNAN/Deb paketit myöntämä jaot monia määrittää NetworkManager ristiriitoja waagent paketin ja siten poistaa NetworkManager Linux agent-paketin asennuksen yhteydessä.


## <a name="general-linux-system-requirements"></a>Yleinen Linux-järjestelmävaatimukset ##

- Muokkaa ydin käynnistyksen rivin GRUB tai GRUB2 seuraavat parametrit. Näin varmistat myös konsolin viestit lähetetään ensimmäinen sarja portti, joka auttaa Azure tukevia virheenkorjaus ongelmat:

        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300

    Näin varmistat myös konsolin viestit lähetetään ensimmäisen serial porttia, joka auttaa Azure tukevia virheenkorjaus ongelmat.

    Lisäksi edellä on suositeltavaa, jos haluat *poistaa* seuraavat parametrit löytämääsi:

        rhgb quiet crashkernel=auto

    Graafinen ja hiljainen käynnistys eivät ole hyödyllisiä cloud-ympäristössä, jossa haluamme sarja portti lähettää lokit.

    `crashkernel` Asetus on määritetty halutessasi vasemmalle, mutta Huomaa, että tämä parametri vähentää käytettävissä oleva muisti: vähintään 128 Mt AM joka voi olla pienempi AM koot ongelmia.

- Asentaminen Azure Linux-agentti

    Azure Linux-agentti vaaditaan valmistelussa Valitse Azure Linux-kuva.  Monta jaot määrittää agentti (paketin nimi on tavallisesti 'WALinuxAgent' tai 'walinuxagent') JÄLLEENMYYNTIHINNAN tai Deb-paketti.  Agentti voidaan asentaa myös manuaalisesti noudattamalla [Linux agentti opas](virtual-machines-linux-agent-user-guide.md).

- Varmista, että SSH palvelin on asennettu ja määritetty alkamaan käynnistyksen yhteydessä.  Tämä on yleensä oletusarvo.

- Älä luo swap tilaa OS-levyllä

    Azure Linux-agentti automaattisesti määrittää swap-tallennustilasta on käytössä paikallinen resurssi levy, johon on liitetty AM Azure-valmistelun jälkeen. Huomaa, että paikallinen resurssi-levy on *väliaikainen* DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan. Asennettuasi Azure Linux-agentti (Katso edellisessä vaiheessa), Muokkaa /etc/waagent.conf seuraavien parametrien oikein:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- Suorittaa seuraavat komennot deprovision virtuaalikoneen viimeisessä vaiheessa:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

    >[AZURE.NOTE] Virtualbox saattaa näet seuraavan virheen käynnistyttyä ' waagent-pakottaa - deprovision': `[Errno 5] Input/output error`. Virheilmoitus ei ole kriittinen ja sen voi ohittaa.

- Sinun on virtuaalikoneen Sulje ja lataa Näennäiskiintolevyn Azure.
