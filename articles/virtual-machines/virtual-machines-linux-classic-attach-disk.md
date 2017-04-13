<properties
    pageTitle="Liitä DVD-levyllä Linux AM | Microsoft Azure"
    description="Lue, miten voit liittää tiedot DVD-levyllä Azure virtual-tietokoneessa, jossa Linux ja alustaa, joten se on valmis käytettäväksi."
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
    ms.date="08/23/2016"
    ms.author="iainfou"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Tietoja DVD-levyllä liittämisestä Linux-Virtual Machine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Katso [tietoja DVD-levyllä Resurssienhallinta käyttöönoton mallin](virtual-machines-linux-add-disk.md)liittäminen.

Voit liittää tyhjä levyille ja levyjä, jotka sisältävät Azure VMs tiedot. Molemmat levyjä ovat .vhd-tiedostoihin, jotka sijaitsevat Azure-tallennustilan tilin. Kuin minkä tahansa levyn lisääminen Linux-koneen kanssa sen jälkeen, kun liität levyn haluat alusta ja muotoile se, jotta se on valmis käytettäväksi. Tämän artikkelin tiedot liittäminen tyhjä levyille ja levyjä, jossa on jo tietoja oman VMs sekä sitten alusta ja muotoilla uuden levyn.

> [AZURE.NOTE]Se on paras käytäntö on yhden tai useamman erillisen levyjen avulla voit tallentaa virtual tietokoneen tietoja. Kun luot Azure virtuaalikoneen, siinä on käyttöjärjestelmä-levyn ja tilapäinen DVD-levyllä. **Älä käytä tilapäinen levyn pysyvä tietojen tallentamiseen.** Nimensä se antaa vain väliaikaisten. Siinä on redundancy tai varmuuskopiointi ei, koska se ei sijaitsevat Azure-tallennustilan.
> Tilapäinen levy on yleensä hallitsee Azure Linux-agentti ja käyttöön automaattisesti **/mnt/resource** (tai **/mnt** Ubuntu kuvista). Toisaalta, tiedot-levyn nimi saattaa olla Linux ydin suunnilleen `/dev/sdc`, eikä sinun tarvitse osio, muotoileminen ja ottaa käyttöön tämän resurssin. Saat [Azure Linux agentti käyttöoppaassa] [ Agent] lisätietoja.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Alusta-Linux uudet tiedot DVD-levyllä

1. SSH, että AM. Lisätietoja edelleen on artikkelissa [Kirjautuminen virtual koneeseen, jossa käytetään Linux][Logon].

2. Seuraavaksi sinun täytyy etsiä tietoja levylle alustaa laitteen tunnus. Voit tehdä kahdella tavalla:

    a) Grep SCSI-laitteiden lokit, kuten seuraava komento:

            $sudo grep SCSI /var/log/messages

    Viimeisimmät Ubuntu jaot, voit joutua käyttämään `sudo grep SCSI /var/log/syslog` koska sisään `/var/log/messages` eivät ehkä ole käytössä oletusarvoisesti.

    Voit etsiä viimeinen tietojen levy, joka on lisätty viestit, jotka näkyvät tunnus.

    ![Levyn viestien noutaminen](./media/virtual-machines-linux-classic-attach-disk/scsidisklog.png)

    TAI

    b) Käytä `lsscsi` laitteen tunnus Lue-komennon. `lsscsi`Voit asentaa joko `yum install lsscsi` (punainen on perustuu jaot) tai `apt-get install lsscsi` (Debian perustuu jaot). Löydät sen _lun_ tai **loogisen yksikön numeron**näyttöä levy. Esimerkiksi-levyjä, voit liittää _lun_ voi helposti nähdä `azure vm disk list <virtual-machine>` nimellä:

            ~$ azure vm disk list TestVM
            info:    Executing command vm disk list
            + Fetching disk images
            + Getting virtual machines
            + Getting VM disks
            data:    Lun  Size(GB)  Blob-Name                         OS
            data:    ---  --------  --------------------------------  -----
            data:         30        TestVM-2645b8030676c8f8.vhd  Linux
            data:    0    100       TestVM-76f7ee1ef0f6dddc.vhd
            info:    vm disk list command OK

    Vertaa tämän tulosteen tietojasi `lsscsi` , sama Esimerkki virtuaalikoneen:

            ops@TestVM:~$ lsscsi
            [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
            [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
            [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
            [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc

    Viimeinen numero monikon kullakin rivillä on _lun_. Katso `man lsscsi` lisätietoja.

3. Kehottaessa Kirjoita Luo laitteen seuraava komento:

        $sudo fdisk /dev/sdc


4. Kirjoita Luo uusi osio **n** pyydettäessä.


    ![Laitteen luominen](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)

5. Kirjoita pyydettäessä **p** , jotta osion ensisijainen osio. Kirjoita **1** , jotta se ensimmäinen osio ja kirjoita tyyppi Hyväksy lieriö oletusarvo. Jotkin järjestelmiin se voi näyttää oletusarvot ensimmäisen ja viimeisen sektorit lieriö sijaan. Voit hyväksyä oletusasetuksia.


    ![Luo osio](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartdetails.png)



6. Kirjoita levyllä, joka on parhaillaan osioitu lisätietoja **p-näppäinyhdistelmää** .


    ![Levyn tiedot](./media/virtual-machines-linux-classic-attach-disk/fdiskpartitiondetails.png)



7. Kirjoita **w** kirjoittaa levyn asetuksia.


    ![Kirjoita levyn muutokset](./media/virtual-machines-linux-classic-attach-disk/fdiskwritedisk.png)

8. Nyt voit luoda uuden osion tiedostojärjestelmän. Lisää osion numeroa laitteen tunnus (Seuraavassa esimerkissä `/dev/sdc1`). Seuraava esimerkki luo ext4-osion /dev/sdc1:

        # sudo mkfs -t ext4 /dev/sdc1

    ![Luo tiedostojärjestelmässä](./media/virtual-machines-linux-classic-attach-disk/mkfsext4.png)

    >[AZURE.NOTE] SuSE Linux Enterprisen 11 järjestelmät tukevat vain luku-tilassa vain ext4 tiedoston Systemsin. Nämä Systemsin suositellaan uuden tiedostojärjestelmän muotoillaan ext3 ext4 sijaan.


9. Tee hakemiston käyttöönottoon uuden tiedostojärjestelmän seuraavasti:

        # sudo mkdir /datadrive


10. Lopuksi voit liittää aseman, seuraavasti:

        # sudo mount /dev/sdc1 /datadrive

    Tietoja levy on nyt valmis käyttämään **/datadrive**.
    
    ![Hakemiston luominen ja ottaa käyttöön levy](./media/virtual-machines-linux-classic-attach-disk/mkdirandmount.png)


11. Lisää uusi asema /etc/fstab:

    Jotta asemaa liitettyinä automaattisesti uudelleenkäynnistyksen jälkeen se on lisättävä/jne/fstab-tiedosto. Lisäksi on erittäin suositeltavaa, että UUID (yleisesti yksilöllinen) käytetään /etc/fstab viittaamaan vain laitteen nimi (eli /dev/sdc1) sijaan. Väärä levy on liitetty tietyssä paikassa, jos käyttöjärjestelmän havaitsee levyvirhe aikana käynnistys- ja jäljellä olevia tietoja-levyjä, valitse on varattu näiden laitteiden tunnuksia käyttämällä UUID vältetään. Jos haluat löytää uuden aseman Tunnuksena, voit käyttää **blkid** -apuohjelma:

        # sudo -i blkid

    Tulos näyttää seuraavankaltaiselta:

        /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
        /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
        /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"


    >[AZURE.NOTE] Väärin **/etc/fstab** muokkaamisen saattaa johtaa järjestelmän käynnistämisen. Jos ole varma, voit tarkistaa jakelua ohjeista siitä, miten voit muokata tätä tiedostoa oikein. On myös suositeltavaa, että varmuuskopio /etc/fstab tiedostosta luodaan ennen muokkaamista.

    Avaa seuraavaksi **/etc/fstab** tiedostoa tekstieditorissa:

        # sudo vi /etc/fstab

    Tässä esimerkissä Käytämme uusi **/dev/sdc1** laitteen, joka on luotu edelliset vaiheet ja liitoskohtaa **/datadrive**UUID-arvo. Lisää seuraava rivi **/etc/fstab** tiedoston loppuun:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2

    Tai järjestelmiin SuSE Linux perusteella voit joutua muotoilulla hieman eri tavalla:

        /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    
    >[AZURE.NOTE] `nofail` Vaihtoehto, joka kertoo, AM käynnistyy, vaikka tiedostojärjestelmän on vahingoittunut tai levy ei ole käynnistyksen yhteydessä. Tämän asetuksen, ilman t toiminta kuvatulla tavalla [Et voi SSH, Linux AM FSTAB virheiden vuoksi](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    Voit testata nyt tiedostojärjestelmän liitetty poistaminen käytöstä ja remounting tiedostojärjestelmän eli oikein esimerkin käyttöön pisteen `/datadrive` luotu edellä kuvatut toimet:

        # sudo umount /datadrive
        # sudo mount /datadrive

    Jos `mount` komento tuottaa virheen, tarkista/jne/fstab tiedosto syntaksin. Jos tiedot asemat tai osiot on luotu, kirjoita ne/jne/fstab sekä erikseen.

    Varmista aseman voi kirjoittaa tämän-komennolla:

        # sudo chmod go+w /datadrive

>[AZURE.NOTE] Poistaminen myöhemmin ilman muokkaus fstab tietojen DVD-levyllä saattaa aiheuttaa AM käynnistyksen epäonnistuu. Jos kyseessä on yleisiä esiintymä, useimmat jaot toimitettava joko `nofail` ja/tai `nobootwait` fstab asetukset, jotka antavat käynnistyy, vaikka levyn epäonnistuu liittää järjestelmän käynnistys. Lisätietoja näiden parametrien oman jakauman käyttöohjeista.

### <a name="trimunmap-support-for-linux-in-azure"></a>RAJAAMINEN ja UNMAP Linux Azure-tuki
Jotkin Linux ytimet tue RAJAAMINEN/UNMAP toimintoja käyttämättömät lohkot levyn Hylkää. Näistä toiminnoista on hyötyä erityisesti vakio tallennustilan ilmoittaa Azure Poistetut sivut, jotka eivät ole enää voimassa, ja voit hylätä. Hylkää sivujen voit tallentaa kustannukset, jos suurten tiedostojen luominen ja poista ne.

On Linux AM tuki käyttöön RAJAAMINEN kahdella tavalla. Jakoa saat suositellaan tavalliseen tapaan:

- Käytä `discard` -asetus käyttöön `/etc/fstab`, esimerkiksi:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- Vaihtoehtoisesti voit suorittaa `fstrim` komennon manuaalisesti komentoriviltä tai lisätä oman crontab säännöllisesti suoritettaviksi:

    **Ubuntu**

        # sudo apt-get install util-linux
        # sudo fstrim /datadrive

    **RHEL/CentOS**

        # sudo yum install util-linux
        # sudo fstrim /datadrive

## <a name="troubleshooting"></a>Vianmääritys
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]


## <a name="next-steps"></a>Seuraavat vaiheet
Voit lukea lisää Linux AM käyttämisestä on seuraavissa artikkeleissa:

- [Käynnissä Linux virtual tietokoneeseen kirjautuminen][Logon]

- [Miten irrottaa Linux virtual tietokoneesta DVD-levyllä](virtual-machines-linux-classic-detach-disk.md)

- [Azure-CLI käyttäminen perinteinen käyttöönotto-malli](../virtual-machines-command-line-tools.md)

<!--Link references-->
[Agent]: virtual-machines-linux-agent-user-guide.md
[Logon]: virtual-machines-linux-mac-create-ssh-keys.md
