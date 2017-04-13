<properties
    pageTitle="Optimoi MySQL suorituskyvyn Linux VMs | Microsoft Azure"
    description="Opettele optimoimaan MySQL Azure virtual machine (AM) käynnissä Linux-käyttöjärjestelmässä."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="optimizing-mysql-performance-on-azure-linux-vms"></a>Valitse Azure Linux VMs MySQL optimoinnista

On monet eri tekijät, jotka vaikuttavat MySQL suorituskyvyn Azure, sekä virtual laitteiston valinta ja ohjelmistojen määritykset. Tässä artikkelissa keskitytään optimoiminen suorituskykyä kautta tallennustilan järjestelmän ja tietokannan määrityksiä.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="utilizing-raid-on-an-azure-virtual-machine"></a>Valitse Azure virtuaalikoneen käyttävien RAID
Tallennustila on avainasemassa, jotka vaikuttaa tietokannan suorituskykyä cloud-ympäristössä.  Verrattuna yhteen, RAID tarjota nopeuttamiseksi samanaikainen kautta.  Lisätietoja [Vakio RAID tasot](http://en.wikipedia.org/wiki/Standard_RAID_levels) n tarkemmin.   

Levyn i/o siirtonopeuden ja i/o vastausajan Azure voi merkittävästi parantaa RAID kautta. Kurssin Microsoftin testien Näytä levyn i/o siirtonopeuden voit kaksinkertaistettava ja i/o vastausajan voidaan pienentää puolella keskimäärin RAID levyjen määrä kaksinkertaistetaan (2, 4, 4-8 jne.). [Liite A](#AppendixA) lisätietoja.  

Lisäksi levyn i/o MySQL suorituskyky paranee, kun lisäät RAID taso.  [Lisäys B](#AppendixB) lisätietoja.  

Voit myös halutessasi ottaa huomioon lohkon koon. Yleinen kun suuremmaksi lohko, näyttöön tulee alemman yleiskustannus, erityisesti niiden suuri kirjoituksia. Kun lohkon koon on liian suuri, se voi lisätä suorituskykyä ja et voi hyödyntää RAID. Nykyinen oletuskoko on 512 kt, jotka on osoitettu on paras mahdollinen yleisimmät tuotannon ympäristössä. Katso lisätietoja [Lisäys C-näppäinyhdistelmää](#AppendixC) .   

Huomaa, että voit lisätä eri virtuaalikoneen tyypit, kuinka monta levyille ovat rajoitukset. Nämä raja-arvot ovat tarkat [virtuaalikoneen](http://msdn.microsoft.com/library/azure/dn197896.aspx)ja Azure Cloud palvelun koot. Sinun on 4 liitettyjä tietoja levyjen noudattamalla tämän artikkelin RAID-esimerkki, vaikka voi valita määrittämään RAID vähemmän levyjen kanssa.  

Tässä artikkelissa oletetaan, että olet jo luonut Linux virtual koneen ja MYSQL asentanut ja määrittänyt. Lisätietoja käytön aloittaminen Lue, miten voit asentaa MySQL Azure.  

###<a name="setting-up-raid-on-azure"></a>Valitse Azure RAID määrittäminen
Seuraavien ohjeiden mukaisesti näyttää, miten voit luoda RAID Azure-Azure perinteinen-portaalissa. Voit myös määrittää RAID Windows PowerShell-komentosarjojen avulla.
Tässä esimerkissä on määrittää RAID 0 4 levyjen kanssa.  

####<a name="step-1-add-a-data-disk-to-your-virtual-machine"></a>Vaihe 1: Lisää virtuaalikoneen tietojen DVD-levyllä  

Valitse Azure perinteinen portaalin näennäiskoneiden-sivulla, johon haluat lisätä tietoja DVD-levyllä virtuaalikoneen. Tässä esimerkissä virtuaalikoneen on mysqlnode1.  

![][1]

Valitse virtuaalikoneen-sivulla **raporttinäkymät-ikkunan**.  

![][2]


Tehtäväpalkin Valitse **Liitä**.

![][3]

Ja valitse sitten **Liitä levy on tyhjä**.  

![][4]

Tietoja levyjen **Host välimuisti-asetus** on asetettava **ei mitään**.  

Tämä lisää yksi tyhjä levy virtuaalikoneen kyselyjä. Toista tämä vaihe kolme kertaa niin, että sinulla on 4 tietojen levyjä RAID.  

Näet virtuaalikoneen lisätty asemaa katsomalla ydin Viestiloki. Jos haluat nähdä tämän Ubuntu, käytä seuraavaa komentoa:  

    sudo grep SCSI /var/log/dmesg

####<a name="step-2-create-raid-with-the-additional-disks"></a>Vaihe 2: Luo uusia levyjä RAID
Noudattamalla tämän artikkelin RAID-asetukset toimimalla seuraavasti:  

[Määritä ohjelmiston RAID Linux](virtual-machines-linux-configure-raid.md)

>[AZURE.NOTE] Jos käytössäsi on XFS tiedostojärjestelmässä, noudata seuraavia ohjeita, kun olet luonut RAID.

Jos haluat asentaa XFS Debian, Ubuntu tai Linux Minttutäyte, käytä seuraava komento:  

    apt-get -y install xfsprogs  

Asenna XFS-Fedora, CentOS tai RHEL, käytä seuraavaa komentoa:  

    yum -y install xfsprogs  xfsdump


####<a name="step-3-set-up-a-new-storage-path"></a>Vaihe 3: Määritä uusi tallennustilan polku
Kirjoita seuraava komento:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

####<a name="step-4-copy-the-original-data-to-the-new-storage-path"></a>Vaihe 4: Kopioi alkuperäiset tiedot uusi tallennustilan polku
Kirjoita seuraava komento:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

####<a name="step-5-modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Vaihe 5: Muokkaa käyttöoikeuksia, jotta MySQL käyttää (lukeminen ja kirjoittaminen) tietojen levy
Kirjoita seuraava komento:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


##<a name="adjust-the-disk-io-scheduling-algorithm"></a>Levyn i/o ajoituksen algoritmin säätäminen
Linux toteuttaa neljänlaisia i/o ajoituksen algoritmit:  

-   NOOP algoritmin (n-toiminto)
-   Määräajan algoritmin (määräajan)
-   Kokonaan tiedenäyttelyn queuing algoritmin (CFQ)
-   Budjetin kauden algoritmin (Anticipatory)  

Voit valita eri i/o-aikatauluttajien eri tilanteissa suorituskyvyn parantamiseksi. Kokonaan RAM-ympäristöä ei ole suorituskyvyn CFQ ja määräpäivä-algoritmeista ero. Suositellaan yleensä määritetään vakauteen määräajan MySQL-tietokanta-ympäristössä. Jos näkyvissä on useita peräkkäisiä i/o, CFQ voi pienentää i/o suorituskyvyn.   

Suoritettaessa ja muita laitteita NOOP tai määräajan avulla voit tehostaa kuin oletusarvoinen ajoituksen suorituskyvyn parantamiseksi.   

Ydin 2,5 oletusarvon i/o ajoituksen algoritmin on määräajan. Alusta ydin 2.6.18 CFQ tuli oletusarvon i/o ajoituksen algoritmin.  Voit määrittää tämän asetuksen ydin käynnistyksen yhteydessä tai muuttaa tätä asetusta, kun järjestelmä on käynnissä.  

Seuraavassa esimerkissä kerrotaan, miten voit tarkistaa ja määrittää oletusarvon ajoituksen NOOP algoritmin.  

Debian jakauman perheen käyttöön:

###<a name="step-1view-the-current-io-scheduler"></a>Vaihe 1 näkymä nykyinen i/o ajoitus
Kirjoita seuraava komento:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Näkyviin tulee tulos, joka viittaa nykyiseen päivitysyrityksen jälkeen.  

    noop [deadline] cfq


###<a name="step-2-change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Vaihe 2. Muuta ajoitus algoritmin i/o nykyinen laite (/ keskihajonta/sda)
Käytä seuraavia komentoja:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

>[AZURE.NOTE] Tämän määrittämistä varten /dev/sda yksin ei ole hyödyllisiä. Se on määritettävä kaikkien tietojen levyjen tietokannan sijainti.  

Näkyy seuraava tulos, joka ilmaisee, että grub.cfg on koottu onnistuneesti ja että oletusarvo-ajoitus on päivitetty NOOP.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Jakauman Redhat, perheen tarvitset vain seuraava komento:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

##<a name="configure-system-file-operations-settings"></a>Järjestelmän tiedoston toimintojen asetusten määrittäminen
Yksi paras käytäntö on tiedostojärjestelmässä atime kirjaaminen-toiminnon poistaminen käytöstä. Atime on tiedoston access viimeksi. Aina, kun tiedosto on käytettävissä, tiedostojärjestelmän kirjataan aikaleiman loki. Kuitenkin näitä tietoja käytetään vain harvoin. Voit poistaa sen, jos et tarvitse sitä, joka vähentää levyn access aikaa.  

Atime kirjaaminen käytöstä, sinun on Muokkaa tiedostoa järjestelmän kokoonpano tiedoston /etc/ fstab ja **noatime** -asetus.  

Muokkaa esimerkiksi vim /etc/fstab tiedostoa lisäämällä noatime alla kuvatulla tavalla.  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Ota uudelleen käyttöön, valitse tiedostojärjestelmän seuraavalla komennolla:  

    mount -o remount /RAID0

Testaa muokattu tulos. Huomaa, että muokkaat testitiedosto käyttöaika ei päivitetä.  

Esimerkki: ennen     

![][5]

Esimerkki: jälkeen

![][6]

##<a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Suurenna järjestelmän käsittelee n hyvin samanaikainen enimmäismäärä
MySQL on hyvin samanaikainen tietokanta. Samanaikainen kahvoja oletusmäärän on 1 024, Linux, joka ei ole aina riitä. **Järjestelmä tukemaan MySQL hyvin samanaikainen suurin samanaikainen kahvoja sisällyttämistä seuraavien ohjeiden mukaisesti**.

###<a name="step-1-modify-the-limitsconf-file"></a>Vaihe 1: Limits.conf-tiedoston muokkaaminen
Lisää seuraavat neljä riviä suurin sallittu samanaikainen kahvoja sisällyttämistä /etc/security/limits.conf-tiedoston. Huomaa, että 65536 enimmäismäärä, jotka tukevat järjestelmä.   

    * Pehmeä nofile 65536
    * Kova nofile 65536
    * Pehmeä nproc 65536
    * Kova nproc 65536

###<a name="step-2-update-the-system-for-the-new-limits"></a>Vaihe 2: Päivitä järjestelmä uudet raja-arvot
Suorita seuraavat komennot:  

    ulimit -SHn 65536
    ulimit -SHu 65536

###<a name="step-3-ensure-that-the-limits-are-updated-at-boot-time"></a>Vaihe 3: Varmistaa, että rajoituksia päivitetään käynnistyksen yhteydessä
Seuraavat käynnistyskomennot laittaa /etc/rc.local-tiedoston, niin se otetaan käyttöön jokaisen käynnistyksen yhteydessä.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

##<a name="mysql-database-optimization"></a>MySQL-tietokantaan optimointi
Voit käyttää samaa suorituskyvyn säätäminen strategia MySQL määrittämiseksi Azure kuin paikalliseen tietokoneeseen.  

Tärkeimmät i/o optimointi säännöt ovat:   

-   Suurentaa välimuistin kokoa.
-   Vähennä i/o vastausajan.  

Optimoi MySQL-palvelinasetukset, voit päivittää my.cnf-tiedoston, joka on määritystiedosto sekä palvelimessa oleva että Asiakastietokoneiden.  

Seuraavat määritettävät tiedot ovat tärkeimmät MySQL suorituskykyyn vaikuttavia tekijöitä:  

-   **innodb_buffer_pool_size**: puskurin-ryhmä sisältää puskuroitua tiedot ja indeksi. Tämä on yleensä määritetty muistin 70 %.
-   **innodb_log_file_size**: Tämä on Tee lokin koko. Tee lokit avulla voit varmistaa, että kirjoitustoiminnot ovat nopeasti, luotettava ja palautettavissa kaatumisen jälkeen. Tämä on määritetty 512 Mt, joiden avulla voit runsaasti tilaa kirjauduttaessa kirjoitustoimintoja.
-   **max_connections**: joskus sovellusten Älä sulje yhteydet oikein. Suurempi arvo avulla palvelimen enemmän aikaa Roskakorin idled yhteydet. Yhteyksien enimmäismäärä on 10 000, mutta suositeltava enimmäismäärä on 5000.
-   **Innodb_file_per_table**: tämän asetuksen käyttöön ottaminen InnoDB voi tallentaa taulukoita erillisinä tiedostoina. Ota käyttöön-vaihtoehto varmistat useita lisäasetusten hallinta toimintoja voi käyttää tehokkaasti. Suorituskyvyn näkökulmasta se nopeuttaa taulukon tilaa siirtämisen ja jäännökset hallinta suorituskyvyn parantamiseksi. Niin suositeltu tämä asetus on käytössä.</br>
    MySQL 5.6 oletusarvo on käytössä. Mitään toimia ei tarvita. Muut versiot, joka on aiempi kuin 5.6, oletusasetukset on ei käytössä. Tämä käytössä on pakollinen. Ja on voimassa se ennen tiedot on ladattu, koska vain luomasi taulukot muuttuvat.
-   **innodb_flush_log_at_trx_commit**: Oletusarvo on 1, ja arvo 0 laajuuden noin 2. Oletusarvo on erillinen MySQL DB parhaiten sopiva vaihtoehto. 2 asetus mahdollistaa useimmissa tietojen eheyden ja soveltuvat perustyyli MySQL-klusterin. 0 asetus mahdollistaa tietojen menetyksiltä, jotka voivat vaikuttaa luotettavuutta joissakin tapauksissa parempi suorituskyky ja soveltuvat toissijaisen MySQL-klusterin.
-   **Innodb_log_buffer_size**: lokin puskuri sallii tapahtumien eikä sinun tarvitse tyhjentää lokin levylle, ennen kuin tapahtumat Vahvista Suorita. Jos näkyvissä on suuri binaarinen objekti tai teksti-kentässä, välimuistin kulutettu nopeasti ja usein käytetyt levyn i/o käynnistyy. Puskurikoko kasvaa paremmin, jos Innodb_log_waits tilan muuttuja ei ole on 0.
-   **query_cache_size**: paras vaihtoehto on peräisin sen poistaminen käytöstä. Query_cache_size arvoksi 0 (tämä on nyt-MySQL 5.6 oletusasetus) ja muilla tavoilla voit nopeuttaa kyselyt.  

Lisätietoja [Liitteen D](#AppendixD) verrattaessa suorituskyvyn optimointia jälkeen.


##<a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Ota käyttöön MySQL hidas kyselyn lokissa pullonkaula suorituskyvyn analysoiminen
MySQL hidas kyselylokin auttaa sinua huomaamaan hidas kyselyt MySQL varten. Kun olet ottanut käyttöön MySQL hidas kyselyn lokiin, voit tunnistaa performance pullonkaula MySQL-työkaluja, kuten **mysqldumpslow** .  

Huomaa, että oletusarvoisesti tämä ei ole otettu käyttöön. Otetaan käyttöön hidas kyselylokin saattaa käyttää suorittimen resursseja. Tämän vuoksi on suositeltavaa, että tämä on otettu käyttöön tilapäisesti suorituskyvyn pullonkaulojen vianmääritystä varten.

###<a name="step-1-modify-mycnf-file-by-adding-the-following-lines-to-the-end"></a>Vaihe 1: My.cnf tiedoston muokkaaminen lisäämällä seuraavat rivit loppuun   

    long_query_time = 2
    slow_query_log = 1
    slow_query_log_file = /RAID0/mysql/mysql-slow.log

###<a name="step-2-restart-mysql-server"></a>Vaihe 2: Käynnistä mysql-palvelin
    service  mysql  restart

###<a name="step-3-check-whether-the-setting-is-taking-effect-using-the-show-command"></a>Vaihe 3: Tarkista, onko asetus kestää tehosteen, "Näytä"-komennon avulla

![][7]   

![][8]

Tässä esimerkissä näet hidas kysely-ominaisuus on otettu käyttöön. Voit määrittää suorituskyvyn pullonkaulojen ja suorituskyvyn, esimerkiksi lisäämällä indeksit sitten **mysqldumpslow** -työkalun avulla.





##<a name="appendix"></a>Lisäys

Seuraavassa on esimerkki suorituskyvyn testitiedot kohdennettujen testiympäristössä on valmistettu, ne antaa yleisiä suorituskyvyn tietojen trendi eri suorituskyvyn säätö tavoista, mutta saatava tulos vaihtelee eri ympäristön tai tuotteen versio-kohdassa.

<a name="AppendixA"></a>A: lisäys  
**Eri RAID määritetyt suorituskyvyn (IOPS)**


![][9]

**Testi-komennot:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

>AZURE. Huomautus: Tämän testin työmäärää käyttää 64 viestiketjuissa siirtyminen, haettavaa RAID yläraja.

<a name="AppendixB"></a>B: lisäys  
**MySQL suorituskyvyn (nopeus) verrattuna eri RAID tasot**   
(XFS tiedostojärjestelmässä)


![][10]  
![][11]

**Testi-komennot:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**MySQL suorituskyvyn (OLTP) verrattuna eri RAID tasot**  
![][12]

**Testi-komennot:**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

<a name="AppendixC"></a>C: lisäys   
**Eri lohkon koot levyn suorituskyvyn (IOPS) vertailu**  
(XFS tiedostojärjestelmässä)


![][13]

**Testi-komennot:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Huomautus käytettävä tämän testin tiedostokoko on 30 Gigatavua ja 1 gt vastaavasti, jossa RAID 0(4 disks) XFS kenttä järjestelmän.


<a name="AppendixD"></a>D: lisäys  
**MySQL suorituskyvyn (nopeus) vertailu ennen ja jälkeen optimointi**  
(XFS File System)


![][14]

**Testi-komennot:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**Oletusarvoiset ja optmization määritys-asetusta on seuraavanlainen:**

|Parametrit |Oletusarvo    |optmization
|-----------|-----------|-----------
|**innodb_buffer_pool_size**    |Ei mitään   |7G
|**innodb_log_file_size**   |5M |512M
|**max_connections**    |100    |5000
|**innodb_file_per_table**  |0  |1
|**innodb_flush_log_at_trx_commit** |1  |2
|**innodb_log_buffer_size** |8M |128M
|**query_cache_size**   |16M    |0


Enemmän yksityiskohtaisia optimointi parametreja, tutustu mysql virallinen ohjeita.  

[http://dev.MySQL.com/doc/refman/5.6/en/innodb-Configuration.HTML](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html)  

[http://dev.MySQL.com/doc/refman/5.6/en/innodb-parameters.HTML#sysvar_innodb_flush_method](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method)

**Testiympäristössä**  

|Laite   |Tiedot
|-----------|-------
|Suoritin    |AMD Opteron(tm) suoritin 4171 hän / 4 sydämiä
|Muisti |14G
|levy   |10G tai levy
|käyttöjärjestelmä |Ubuntu 14.04.1 l.



[1]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png
