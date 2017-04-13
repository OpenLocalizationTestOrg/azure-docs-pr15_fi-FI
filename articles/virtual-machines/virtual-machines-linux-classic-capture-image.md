<properties
    pageTitle="Kuvan tallentaminen Linux AM | Microsoft Azure"
    description="Opettele Linux-pohjaiset Azure virtual koneen (AM) perinteinen käyttöönotto-mallin avulla luotu kuva."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="iainfou"/>


# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Voit siepata perinteinen Linux virtual machine kuvana

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Lue, miten [Resurssienhallinta-mallin seuraavasti](virtual-machines-linux-capture-image.md).

Tämän artikkelin avulla voit siepata perinteinen Azure virtual koneeseen, jossa käytetään Linux kuvana muiden näennäiskoneiden luomiseen. Tämä kuva on OS levyn ja yhdistetty virtuaalikoneen tietojen levyjä. Se ei ole verkon määritys-, joten sinun on määritettävä, kun luot muiden näennäiskoneiden kuvan.

Azure tallentaa lisättyjen kuvia sekä **kuvia**, valitse kuva. Katso lisätietoja kuvien [Tietoja virtuaalikoneen kuvia Azure-tietokannassa] [].

## <a name="before-you-begin"></a>Ennen aloittamista

Näissä vaiheissa oletetaan, että olet jo luonut Azure virtual-koneen perinteinen käyttöönotto-mallin avulla ja määrittänyt käyttöjärjestelmä, mukaan lukien kaikki tiedot levyjen liittäminen. Jos haluat luoda AM, lue, [miten voit luoda Linux-Virtual Machine] [].


## <a name="capture-the-virtual-machine"></a>Sieppaa virtuaalikoneen

1. [Yhteyden muodostaminen virtuaalikoneen](virtual-machines-linux-mac-create-ssh-keys.md) valittua SSH-asiakas.

2. Kirjoita seuraava komento SSH-ikkunaan. Tuloste `waagent` voivat vaihdella hiukan tämän apuohjelman version mukaan:

    `sudo waagent -deprovision+user`

    Tämä komento yrittää puhdistaa järjestelmä ja varmista sopii reprovisioning. Tämä toiminto tekee seuraavat toimet:

    - Poistaa SSH host avaimet (Jos Provisioning.RegenerateSshHostKeyPair on y, jos määritystiedostossa)
    - Tyhjentää /etc/resolv.conf nameserver määrittäminen
    - Poistaa `root` käyttäjän salasanan/jne/varjostus (Jos Provisioning.DeleteRootPassword on "a" määritystiedostossa)
    - Poistaa välimuistiin DHCP-asiakasohjelman varauksia
    - Palauttaa localhost.localdomain isäntänimi
    - Poistaa viimeksi valmistellun käyttäjän tili (saatu /var/lib/waagent) **ja siihen liittyvät tiedot**.

    >[AZURE.NOTE] Valmistelun poistaminen poistaa tiedostot ja tiedot "generalize" kuva. Suorita tämä komento vain virtual tietokoneeseen, jotka haluat siepata uutena mallina kuva. Ei takaa kuva ole valittu kaikki luottamuksellisia tietoja tai sopii uudelleenjakamista kolmansille osapuolille.


3. Kirjoita **y** Jatka. Voit lisätä `-force` parametri välttää vahvistus tämän vaiheen.

4. Kirjoita **Lopeta** Sulje SSH asiakas.

    >[AZURE.NOTE] Jäljellä oleva vaiheissa oletetaan, että sinulla on jo [asennettu Azure-CLI](../xplat-cli-install.md) asiakastietokoneen. Seuraavat vaiheet voidaan tehdä myös [Azure perinteinen portal] [].

5. Asiakastietokoneesta Avaa Azure CLI ja kirjaudu sisään Azure-tilaukseen. Lisätietoja Lue [Azure tilauksen Azure-CLI Yhdistä](../xplat-cli-connect.md).

6. Varmista, että hallinta-tilassa:

    `azure config mode asm`

7. Sulje virtuaalikoneen, joka on jo käyttömahdollisuus purettu, edelliset vaiheet ja:

    `azure vm shutdown <your-virtual-machine-name>`

    >[AZURE.NOTE] Voit tarkistaa kaikki tilauksen-ohjelmalla luotu näennäiskoneiden`azure vm list`

8. Kun virtuaalikoneen pysäytetään, kuvan-komennolla:

    `azure vm capture -t <your-virtual-machine-name> <new-image-name>`

    Kirjoita _Uusi nimi kuin kuvan_tilalla haluamasi kuvan nimi. Tämä komento luo generalized OS-kuva. `-t` Vaihtoehto poistaa alkuperäisen virtuaalikoneen.

9.  Uusi kuva on nyt saatavilla kuvia, jotka voidaan määrittää minkä tahansa uusien näennäiskoneiden luettelo. Voit tarkastella sitä-komennolla:

    `azure vm image list`

    [Azure perinteinen portal] []se näkyy **kuvien** -luettelossa.

    ![Kuva sieppaus onnistui](./media/virtual-machines-linux-classic-capture-image/VMCapturedImageAvailable.png)


## <a name="next-steps"></a>Seuraavat vaiheet
Kuva on valmis käytettäväksi näennäiskoneiden luomiseen. Voit käyttää Azure CLI-komento `azure vm create` ja kuvan nimi, jonka loit. Katso lisätietoja komennon [käyttämällä Azure CLI perinteinen käyttöönoton mallia](../virtual-machines-command-line-tools.md) . Voit myös luoda mukautettuja virtual machine **-Valikoima** -menetelmällä ja valitsemalla kuva, jonka loit [Azure perinteinen portal] [] avulla. Katso, [miten voit luoda mukautetun Virtual koneen] [] lisätietoja.

**Katso myös:** [Azure Linux-agentti käyttöopas](virtual-machines-linux-agent-user-guide.md)

[Azure perinteinen portal]: http://manage.windowsazure.com
[Lisätietoja Azure virtuaalikoneen kuvat]: virtual-machines-linux-classic-about-images.md
[Voit luoda mukautetun Virtual Machine]: virtual-machines-linux-classic-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]: virtual-machines-windows-classic-attach-disk.md
[Linux-Virtual Machine luominen]: virtual-machines-linux-classic-create-custom.md
