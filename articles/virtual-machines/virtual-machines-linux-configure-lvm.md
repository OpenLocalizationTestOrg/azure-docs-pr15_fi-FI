<properties 
    pageTitle="Määritä LVM käynnissä Linux virtual koneeseen | Microsoft Azure" 
    description="Opettele määrittämään LVM Linux Azure-tietokannassa." 
    services="virtual-machines-linux" 
    documentationCenter="na" 
    authors="szarkos"  
    manager="timlt" 
    editor="tysonn"
    tag="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="szark"/>


# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>LVM määrittämistä Linux-AM Azure-tietokannassa

Tämä asiakirja käsitellään määrittäminen Azure virtuaalikoneen loogisen aseman hallinta (LVM). Samalla, kun se on mahdollista LVM määrittämiseksi virtuaalikoneen liitettyä levyä, oletusarvoisesti cloud kuvia ei ole määritetty OS levyn LVM. Näin voit estää kaksoiskappaleiden aseman ryhmät Jos OS levyn koskaan liitetään toiseen AM on sama jakelu ja tyyppi, eli palautus tilanne aikana. Tämän vuoksi suositellaan vain, jos haluat käyttää LVM tietojen levyille.


## <a name="linear-vs-striped-logical-volumes"></a>Lineaarinen ja mustat looginen tietomääristä

LVM voidaan yhdistää yhden tallennustilan aseman fyysinen levyjen määrä. Oletusarvoisesti LVM yleensä Luo lineaarinen loogiset asemat, mikä tarkoittaa fyysinen tallennustilan yhdistetään yhteen. Tässä tapauksessa luku-/ kirjoitusoikeudet toimintojen yleensä vain lähetetään yhteen. Sen sijaan voit luoda myös mustat loogiset asemat, jossa lukee ja kirjoittaa jaetaan useille levyille (eli muistuttaa RAID0) äänenvoimakkuutta-ryhmän sisältämät. Ei-toivottuja nopeuttamiseksi haluat stripe looginen asemat niin, että lukee ja kirjoittaa käyttämiseen kaikki liitettyjä tietoja levyjen.

Tässä asiakirjassa kerrotaan eri tietojen levyjen yhdistäminen yhteen aseman ryhmän ja luo Raidallinen looginen asema. Seuraavia ohjeita hieman generalized useimmat jaot käyttöä varten. Useimmissa tapauksissa apuohjelmien ja hallintaan LVM Azure-työnkulut eivät ole olennaisesti erilaisia kuin muiden ympäristöjen. Tavalliseen tapaan myös tilanteeseesi Linux toimittajan asiakirjat ja parhaat käytännöt LVM tietyn-jakauman.


## <a name="attaching-data-disks"></a>Tietoja levyjen liittäminen
Yksi yleensä kannattaa aloittaminen vähintään kaksi tyhjä tietonäkymä-levyjen LVM käytettäessä. IO-tarpeiden perusteella, voit liittää levyjä, jotka on tallennettu Microsoftin vakio tallennus-ja enintään 500 IO/ps, levyn tai tutustu Premium tallennustilan kohden ja korkeintaan 5 000 IO/ps levyä kohden. Tässä artikkelissa ei siirry tiedot siitä, miten voit valmistella ja liitä tiedot levyjen Linux virtual koneeseen. On artikkelissa Microsoft Azure artikkelissa [Liitä DVD-levyllä](virtual-machines-linux-add-disk.md) saat yksityiskohtaiset ohjeet tyhjä tietonäkymä-levyn liittämisestä Linux virtual tietokoneen Azure.

## <a name="install-the-lvm-utilities"></a>Asenna LVM-apuohjelmat

- **Ubuntu**

        # sudo apt-get update
        # sudo apt-get install lvm2

- **RHEL, CentOS ja Oracle Linux**

        # sudo yum install lvm2

- **SLES 12 ja openSUSE**

        # sudo zypper install lvm2

- **SLES 11**

        # sudo zypper install lvm2

    Valitse SLES11 on myös muokata /etc/sysconfig/lvm ja määrittää `LVM_ACTIVATED_ON_DISCOVERED` "käyttöön":

        LVM_ACTIVATED_ON_DISCOVERED="enable" 


## <a name="configure-lvm"></a>Määritä LVM
Tässä oppaassa on olettaa, että olet liittänyt kolme tietojen levyä, joka viitataan `/dev/sdc`, `/dev/sdd` ja `/dev/sde`. Huomaa, että ne eivät ehkä aina oman AM saman polku nimet. Voit suorittaa '`sudo fdisk -l`' tai samanlaisia komennon luettelo käytettävissä levyjen.

1. Valmistele fyysinen asemat:

        # sudo pvcreate /dev/sd[cde]
          Physical volume "/dev/sdc" successfully created
          Physical volume "/dev/sdd" successfully created
          Physical volume "/dev/sde" successfully created


2.  Luo aseman ryhmä. Tässä esimerkissä on soitat-asema ryhmän "tietojen-vg01":

        # sudo vgcreate data-vg01 /dev/sd[cde]
          Volume group "data-vg01" successfully created


3. Luo looginen asema(t). Alla on komento luo yhden loogisen aseman nimeltä "tietojen-lv01" kattavat koko aseman-ryhmä, mutta Huomaa, että se on myös mahdollista voidaan luoda useita loogisen aseman-ryhmässä.

        # sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
          Logical volume "data-lv01" created.


4. Loogisen aseman muotoileminen

        # sudo mkfs -t ext4 /dev/data-vg01/data-lv01

  >[AZURE.NOTE] SLES11 ja "-t ext3" ext4 sijaan. SLES11 tukee vain lukuoikeudet ext4 filesystems.


## <a name="add-the-new-file-system-to-etcfstab"></a>Lisää uuden tiedostojärjestelmän /etc/fstab

**Varoitus:** Väärin /etc/fstab muokkaamisen saattaa johtaa järjestelmän käynnistämisen. Jos ole varma, katso lisätietoja siitä, miten voit muokata tiedostoa oikein jakauman ohjeissa. On myös suositeltavaa, että varmuuskopio /etc/fstab tiedostosta luodaan ennen muokkaamista.

1. Luo uusi tiedostojärjestelmän haluamasi liityntäkohtaan esimerkiksi:

        # sudo mkdir /data


2. Etsi loogisen aseman polku

        # lvdisplay
        --- Logical volume ---
        LV Path                /dev/data-vg01/data-lv01
        ....


3. Avaa tekstieditorissa /etc/fstab ja lisätä uuden tiedostojärjestelmän, kuten:

        /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2

    Tallenna ja sulje /etc/fstab.


4. Testaa, että/jne /-fstab tapahtuma on oikein:

        # sudo mount -a

    Jos tämä komento virhesanoman Tarkista syntaksi/jne/fstab-tiedostossa.

    Suorita seuraava `mount` -komento, jolla varmistetaan tiedostojärjestelmän on otettu käyttöön:

        # mount
        ......
        /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)


5. (Valinnainen) /Etc/fstab FailSafe käynnistyksen parametrit

    Monta jaot ovat joko `nobootwait` tai `nofail` käyttöön parametrit, joita voi lisätä/jne/fstab-tiedosto. Nämä parametrit Salli virheiden kun käyttöönottaminen tietyn tiedostojärjestelmässä ja jatka käynnistyy, vaikka se ei voi ottaa oikein RAID tiedostojärjestelmän Linux-järjestelmän. Tutustu oman jakauman ohjeissa nämä parametrit.

    Esimerkki (Ubuntu):

        /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
