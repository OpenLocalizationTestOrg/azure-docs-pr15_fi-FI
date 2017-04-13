<properties
    pageTitle="Lisätä levyn Linux AM | Microsoft Azure"
    description="Opi lisäämään Linux AM pysyvä DVD-levyllä"
    keywords="Linux virtuaalikoneen resurssin levyn lisääminen"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.topic="article"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="add-a-disk-to-a-linux-vm"></a>Lisää Linux AM DVD-levyllä

Tässä artikkelissa kerrotaan liittämisestä pysyvä DVD-levyllä oman AM niin, että voit säilyttää tiedot – vaikka oman AM on valmistella ylläpito tai koon vuoksi uudelleen. Jos haluat lisätä DVD-levyllä, sinun on [Azure-CLI](../xplat-cli-install.md) määritetty Resurssienhallinta-tilassa (`azure config mode arm`).  

## <a name="quick-commands"></a>Pikakomennot

Korvaa arvot väliltä-komennon seuraavissa esimerkeissä &lt; ja &gt; oman ympäristön arvoilla.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## <a name="attach-a-disk"></a>Liitä DVD-levyllä

Uuden levyn liittäminen on nopeaa. Kirjoita `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` luominen ja liitä uuden gt DVD-levyllä, että AM. Jos et havaitse tallennustilan tilin erikseen, minkä tahansa levyä, voit luoda sijoitetaan saman tallennustilan tilin OS levytilaa sijainti.  Se pitäisi näyttää seuraavanlaiselta:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Tulos

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Ota käyttöön uuden levyn Linux-AM yhdistäminen

> [AZURE.NOTE] Tässä ohjeaiheessa muodostaa yhteyden AM, käyttäjänimet ja salasanat. Yhteydenpito oman AM julkisista ja yksityisistä avaimen pareina avulla, katso, [miten voit käyttää SSH kanssa Linux Azure.](virtual-machines-linux-mac-create-ssh-keys.md) Voit muokata **SSH** yhteys luodaan VMs `azure vm quick-create` -komennon avulla `azure vm reset-access` komennon palauttaminen **SSH** access kokonaan, Lisää tai poista käyttäjiä tai lisää julkisen avaimen tiedostot suojaamiseen access.

Sinun on SSH kyselyjä Azure-AM osioon, muotoileminen ja ottaa käyttöön uuden levyn, jotta Linux AM käyttää sitä. Jos et ole tottunut yhdistämällä **ssh**-komennolla lomakkeen `ssh <username>@<FQDNofAzureVM> -p <the ssh port>`, ja näyttää seuraavalta:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Tulos

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ops@myuniquevmname:~$
```

Nyt kun olet muodostanut yhteyden oman AM, voit ryhtyä liitettävä DVD-levyllä.  Etsi ensin levyn avulla `dmesg | grep SCSI` (tapa löytää uuden levyn voivat vaihdella). Tässä tapauksessa se näyttää suunnilleen:

```bash
dmesg | grep SCSI
```

Tulos

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

ja tässä ohjeaiheessa `sdc` levy on haluamme haluamasi vaihtoehto. Nyt osion sisältävän levyn `sudo fdisk /dev/sdc` --oletetaan, että tapaus-levy on `sdc`, ja julkaista ensisijainen levy 1-osioon ja hyväksy muiden oletusarvot.

```bash
sudo fdisk /dev/sdc
```

Tulos

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Luo osio kirjoittamalla `p` tulee näkyviin:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

Ja kirjoittaa tiedostojärjestelmän osio käyttämällä **mkfs** -komentoa, tiedostojärjestelmän tyyppi-ja laitteen nimi. Tämän artikkelin esimerkissä on käytössä `ext4` ja `/dev/sdc1` edellä:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Tulos

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Nyt liittämään tiedoston järjestelmän käyttämällä kansio luodaan `mkdir`:

```bash
sudo mkdir /datadrive
```

Ja liität kansion käyttäminen `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

Tietoja levy on nyt valmis käyttämään `/datadrive`.

```bash
ls
```

Tulos

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

Jotta asemaa liitettyinä automaattisesti uudelleenkäynnistyksen jälkeen se on lisättävä/jne/fstab-tiedosto. Lisäksi on erittäin suositeltavaa, että UUID (yleisesti yksilöllinen) käytetään/jne/fstab viittaamaan laitteen nimen sijaan (esimerkiksi `/dev/sdc1`). Jos käyttöjärjestelmän havaitsee levyvirhe käynnistyksen aikana, käyttämällä UUID vältetään väärä levy on liitetty tiettyyn paikkaan. Jäljellä olevat tiedot levyjen sitten määritetään näiden saman laitteen tunnukset. Jos haluat löytää uuden aseman Tunnuksena, **blkid** -apuohjelman avulla:

```bash
sudo -i blkid
```

Tulos näyttää seuraavankaltaiselta:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

>[AZURE.NOTE] Väärin **/etc/fstab** muokkaamisen saattaa johtaa järjestelmän käynnistämisen. Jos ole varma, voit tarkistaa jakelua ohjeista siitä, miten voit muokata tätä tiedostoa oikein. On myös suositeltavaa, että varmuuskopio /etc/fstab tiedostosta luodaan ennen muokkaamista.

Avaa seuraavaksi **/etc/fstab** tiedostoa tekstieditorissa:

```bash
sudo vi /etc/fstab
```

Tässä esimerkissä Käytämme uusi **/dev/sdc1** laitteen, joka on luotu edelliset vaiheet ja liitoskohtaa **/datadrive**UUID-arvo. Lisää seuraava rivi **/etc/fstab** tiedoston loppuun:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

>[AZURE.NOTE] Poistaminen myöhemmin ilman muokkaus fstab tietojen DVD-levyllä saattaa aiheuttaa AM käynnistyksen epäonnistuu. Useimmat jaot toimitettava joko `nofail` ja/tai `nobootwait` fstab asetukset. Näistä asetuksista järjestelmä käynnistyy, vaikka levyn epäonnistuu liittämään käynnistyksen yhteydessä. Lisätietoja näiden parametrien oman jakauman käyttöohjeista.

>[AZURE.NOTE] **Nofail** -vaihtoehto varmistaa, AM käynnistyy, vaikka tiedostojärjestelmän on vahingoittunut tai levy ei ole käynnistyksen yhteydessä. Ilman tämän asetuksen viestejä toiminta kuvatulla tavalla [Et voi SSH Linux AM FSTAB virheiden vuoksi](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)


### <a name="trimunmap-support-for-linux-in-azure"></a>RAJAAMINEN ja UNMAP Linux Azure-tuki
Jotkin Linux ytimet tue RAJAAMINEN/UNMAP toimintoja käyttämättömät lohkot levyn Hylkää. Tämä on hyötyä erityisesti vakio tallennustilan ilmoittaa Azure Poistetut sivut, jotka eivät ole enää voimassa, ja voit hylätä. Näin voit tallentaa kustannukset, jos suurten tiedostojen luominen ja poista ne.

On kaksi tapaa käyttöön RAJAAMINEN tuki Linux AM. Jakoa saat suositellaan tavalliseen tapaan:

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

- Muista, että uuden levyn ei ole käytettävissä AM, jos se käynnistyy, ellei tietoja kirjoittamaan [fstab](http://en.wikipedia.org/wiki/Fstab) -tiedosto.
- Jotta Linux AM on määritetty oikein, tarkista [Optimoi Linux tietokoneen suorituskyvyn](virtual-machines-linux-optimization.md) suositukset.
- Laajenna tallennustilan kapasiteetti lisäämällä muita levyille ja [Määritä RAID](virtual-machines-linux-configure-raid.md) suorituskykyä.
