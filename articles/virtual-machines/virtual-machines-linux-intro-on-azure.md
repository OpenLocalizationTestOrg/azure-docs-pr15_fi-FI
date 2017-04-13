<properties
    pageTitle="Johdanto Linux Azure | Microsoft Azure"
    description="Opi käyttämään Linux näennäiskoneiden Azure."
    services="virtual-machines-linux"
    documentationCenter="python"
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

#<a name="introduction-to-linux-on-azure"></a>Linux Azure-esittely

Tämä artikkeli sisältää joitakin ominaisuuksia käyttämällä Linux näennäiskoneiden Azure pilveen yleiskatsaus. Käyttöönotto Linux virtual koneen on onnistuu helposti ja nopeasti käyttämällä kuvan valikoimasta.


## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Todentaminen: Käyttäjänimet, salasanat ja SSH näppäimet

Luodessasi Linux virtual machine-perinteinen Azure-portaalissa, sinua pyydetään antamaan käyttäjänimi, salasana tai julkinen SSH-avain. Käyttäjänimi käyttöönoton Linux virtual tietokoneen Azure valinta peritään seuraava rajoite: järjestelmätilillä nimet (UID < 100) on jo määritetty virtuaalikoneen eivät ole sallittuja, 'pääkansio' esimerkiksi.


 - Katso [Virtual koneeseen, jossa käytetään Linux luominen](virtual-machines-linux-quick-create-cli.md)
 - Katso, [miten voit käyttää SSH Linux Azure-kanssa](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="obtaining-superuser-privileges-using-sudo"></a>Hankkiminen pääkäyttäjän oikeudet käyttäminen`sudo`

Sellaisten tili on käyttäjätili, jolla on määritetty Azure virtuaalikoneen esiintymän käyttöönoton aikana. Tällä tilillä on määritetty voivat nostaa oikeuksilla pääkansio (pääkäyttäjän tili) käyttämään Azure Linux-agentti `sudo` apuohjelma. Kun tämä käyttäjätunnuksella kirjautunut, osaat suorittaa komennon syntaksin päämyöntäjien komennot

    # sudo <COMMAND>

Voit halutessasi hankkia pääkansio-liittymän avulla **sudo s**.

- Katso [käyttäminen pääkansion jakamisoikeudet Linux näennäiskoneiden Azure-tietokannassa](virtual-machines-linux-use-root-privileges.md)


## <a name="firewall-configuration"></a>Palomuurin

Azure on saapuva paketti-suodatin, joka rajoittaa connectivity porttien määritetty Azure perinteinen-portaalissa. Vain sallitut porttia on oletusarvoisesti SSH. Voit voi avata muita portit-Linux virtuaalikoneen määrittämällä päätepisteet Azure perinteinen portaalissa:

 - Lisätietoja: [Voit määrittää päätepisteet Virtual Machine](virtual-machines-windows-classic-setup-endpoints.md)

Azure-valikoiman Linux-kuvat eivät salli *iptables* palomuurin oletusarvoisesti. Halutessasi palomuurin on määritetty antamaan muita suodatus.


## <a name="hostname-changes"></a>Hostname muutokset

Kun otat käyttöön alun perin esiintymän Linux-kuva, tarvitaan antamaan virtuaalikoneen isäntänimi. Kun virtuaalikoneen on käynnissä, tämä hostname on julkaistu ympäristö DNS-palvelimet niin, että useita toisiinsa näennäiskoneiden suorittaa IP-osoite haut käyttämällä isäntänimet.

Jos hostname muutokset ovat toivottuja, kun virtual machine on otettu käyttöön, käytä komento

    # sudo hostname <newname>

Azure Linux-agentti sisältää automaattinen haku muutoksen nimi ja määritä asianmukaisesti, muutoksen ja julkaista muutoksen ympäristössä DNS-palvelimet virtuaalikoneen toimintoja.

 - [Azure Linux-agentti käyttöopas](virtual-machines-linux-agent-user-guide.md)

### <a name="cloud-init"></a>Cloud alusta
**Ubuntu** ja **CoreOS** käyttämiseen Azure, joka sisältää lisäominaisuuksia automaattiseen käynnistykseen virtual machine-pilvi-alusta.

 - [Voit lisätä mukautettuja tietoja](virtual-machines-windows-classic-inject-custom-data.md)
 - [Mukautettujen tietojen ja Microsoft Azure-pilvi-alusta](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Luo Azure Vaihda osioita käyttämällä Cloud alusta](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [Azure CoreOS käyttämisestä](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="virtual-machine-image-capture"></a>Virtuaalikoneen kuvan sieppaus

Azure avulla voit siepata aiemmin virtuaalikoneen tilan kuvaksi, joita voidaan käyttää myöhemmin ottaa käyttöön lisää virtuaalikoneen esiintymät. Azure Linux-agentti voidaan käyttää palauttaminen osa, joka on tehty valmistelun aikana mukauttaminen. Saattaa noudattamalla seuraavia ohjeita voit siepata virtual koneen kuvana:

1. Suorita **waagent-deprovision** Kumoa valmistelu mukauttaminen. Tai **waagent-deprovision + käyttäjän** voit myös poistaa valmistelu ja kaikki siihen liittyvät tiedot aikana määritetyn käyttäjätilin.

2. Lopeta alas ja power virtuaalikoneen käytöstä.

3. Valitse Azure perinteinen portaalissa *siepata* tai Powershell tai CLI-työkalujen avulla voit siepata virtuaalikoneen kuvana.

 - Lisätietoja: [sieppaaminen Linux-Virtual Machine, jos haluat käyttää mallina](virtual-machines-linux-classic-capture-image.md)


## <a name="attaching-disks"></a>Liittäminen levyjen

Kunkin virtuaalikoneen on liitetty *resurssin levyn* väliaikaisen, paikallinen. Koska resurssin levyn tiedot eivät välttämättä ole kestävät koko käynnistyy, sitä käytetään usein sovellusten ja-prosesseja käyntiin virtuaalikoneen lyhytaikainen ja **Väliaikaiset** tietojen tallennuksessa. Sitä käytetään myös tallentaa sivun tai vaihda käyttöjärjestelmän tiedostot.

Linux resurssi-levy on yleensä hallitsee Azure Linux-agentti ja käyttöön automaattisesti **/mnt/resource** (tai **/mnt** Ubuntu kuvista).


>[AZURE.NOTE] Huomaa, että resurssin levy on **tilapäinen** DVD-levyllä, ja ehkä voi poistaa ennallaan, kun AM käynnistetään.

Linux tiedot-levyn nimi saattaa olla ydin kuin `/dev/sdc`, ja käyttäjät tarvitsevat osio, muotoileminen ja ottaa käyttöön kyseiselle resurssille. Tämä koskee vaiheittaiset opetusohjelman: [liittäminen tietojen DVD-levyllä Virtual koneeseen](virtual-machines-linux-classic-attach-disk.md).

 - **Katso myös:** [Määritä ohjelmiston RAID Linux](virtual-machines-linux-configure-raid.md)  &  [Määrittäminen LVM-Linux-AM Azure-tietokannassa](virtual-machines-linux-configure-lvm.md)

