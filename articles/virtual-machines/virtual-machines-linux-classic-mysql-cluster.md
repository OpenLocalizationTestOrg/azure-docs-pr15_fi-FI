<properties
    pageTitle="Clusterize MySQL kanssa kuormituksen joukot | Microsoft Azure"
    description="Määritä kuormituksen, suuri käytettävyys, Linux MySQL-klusterin, joka on luotu käyttämällä Azure perinteinen käyttöönottomalli"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="bureado"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/14/2015"
    ms.author="jparrel"/>

# <a name="using-load-balanced-sets-to-clusterize-mysql-on-linux"></a>Clusterize MySQL Linux kuormituksen joukot avulla

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Tässä artikkelissa tarkoituksena on tutkia ja kuvaavat käytettävissä ottamaan Microsoft Azure-tutustuminen MySQL-palvelin askeleet kuin suuren käytettävyyden Linux-pohjaiset palvelut käytettävissä eri tavalla. Tämän menetelmän havainnollistava videon on käytettävissä [kanavan 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

Kerrottu jaettu mitään kahden solmun yhden pääobjektin MySQL suuren käytettävyyden ratkaista DRBD, Corosync ja Pacemaker perusteella. Vain yksi solmu toimii MySQL kerrallaan. Lukemiseen ja kirjoittamiseen DRBD resurssista on myös rajoitettu solmu vain yksi kerrallaan.

Ei ole tarpeen VIP-ratkaisun, kuten LVS, koska Käytämme Microsoft Azure kuormituksen saapuva joukot pyöreän pöydän toimintoja ja päätepisteen tunnistus, poisto sekä VIP kaksivaiheista palautus tarjoamista varten. VIP on määritetty Microsoft Azure mukaan, kun Microsoft on luonut pilvipalvelussa yleisesti reititettävä IPv4-osoite.

Valitse [AM jäävä](http://vmdepot.msopentech.com)AM käytettävissä olevat muihin mahdollista arkkitehtuureihin, mukaan lukien NBD klusterin, Percona ja Galera sekä useita middleware-ratkaisuja, kuten vähintään yksi MySQL. Kunhan näitä ratkaisuja toistaa unicast ja moni tai lähetys- ja ei riippuvaisia jaettua tallennustilan tai useita verkkoliittymät, käyttötavoista olisi helppoa käyttöön ottamiseksi Microsoft Azure.

Kurssin nämä klusteroinnin arkkitehtuureihin laajennettavissa muissa tuotteissa, kuten PostgreSQL ja OpenLDAP-samalla tavalla. Usean perustyylin OpenLDAP onnistuneesti testattu tämän kuormituksen toimintosarjan kanssa jaettujen mitään, ja voit katsella sitä kanavan 9-blogi.

## <a name="getting-ready"></a>Valmistautuminen

Tarvitset Microsoft Azure-tilin kanssa voivat luoda vähintään kaksi (2) VMs kelvollinen tilaus (XS käytettiin tässä esimerkissä), verkon ja aliverkon, affiniteetti-ryhmä ja saatavuus määritetty, voit luoda uuden näennäiskiintolevyjen samalla alueella kuin pilvipalvelussa ja liitä ne Linux VMs sekä.

### <a name="tested-environment"></a>Testattu ympäristössä

- Ubuntu 13.10
  - DRBD
  - MySQL-palvelin
  - Corosync ja Pacemaker

### <a name="affinity-group"></a>Affiniteetti-ryhmä

Affiniteetti-ryhmän ratkaisun luodaan kirjautumalla järjestelmään Azure perinteinen portaalin vieritys kohtaan asetukset ja luomalla uuden affiniteetti ryhmän. Varatut resurssit myöhemmin määritetään affiniteetti ryhmään.

### <a name="networks"></a>Verkot

Uuden verkon luodaan ja aliverkon luodaan verkon sisällä. Microsoft valitsi 10.10.10.0/24 verkossa, jossa on vain yksi /24 aliverkon.

### <a name="virtual-machines"></a>Näennäiskoneiden

Ensimmäinen Ubuntu 13.10 AM on luotu käyttäen Endorsed Ubuntu-valikoiman kuva ja kutsua `hadb01`. Prosessin nimeltä hadb luodaan uusi pilvipalvelussa. Emme anna sille tällä tavalla voit havainnollistaa jaettu, kuormituksen laatu, joka palvelun esiintyä on lisää resursseja. Luomisen `hadb01` on uneventful ja valmiit-portaalissa. Päätepiste SSH luodaan automaattisesti ja tutustu luodun verkon on valittuna. On myös valita, voit luoda uuden käytettävyys, VMs määrittäminen.

Kun ensimmäisen AM luodaan (tarkalleen ottaen, kun pilvipalvelussa luodaan) jatkamista Luo toinen AM `hadb02`. Toinen AM, myös Käytämme Ubuntu 13.10 AM portaalissa valikoimasta, mutta olemme valitsemalla Käytä aiemmin pilvipalvelussa `hadb.cloudapp.net`, uuden sijaan. Verkko- ja käytettävyyden määrittäminen pitäisi olla automaattisesti valittuna us. Päätepisteen SSH luodaan, liian.

Kun sekä VMs luonut olemme huomioi SSH portti `hadb01` (TCP-22) ja `hadb02` (automaattisesti määritetty Azure mukaan)

### <a name="attached-storage"></a>Liitetty tallennustila

Syy uuden levyn liittäminen sekä VMs ja luoda uusia 5 gt levyjä prosessin. Levyjen sovelluksessa Näennäiskiintolevyn säilö Microsoftin tärkeimmät käyttöjärjestelmän levyjen käyttäminen. Kun levyjä luodaan ja liitetty ei ole tarpeen, että Linux uudelleen, kun ydin näkevät uuden laitteen (yleensä `/dev/sdc`, voit tarkistaa `dmesg` tulostukseen)

Valitse kunkin AM jatkamista, Luo uusi osio käyttäen `cfdisk` (perus-Linux-osio) ja kirjoita uusi osiotaulukko. **Älä luo tämän osion tiedostojärjestelmässä** .

## <a name="setting-up-the-cluster"></a>Klusterin määrittäminen

Molemmat Ubuntu VMs tarvitsemme APT avulla voit asentaa Corosync, Pacemaker ja DRBD. Käyttämällä `apt-get`:

    sudo apt-get install corosync pacemaker drbd8-utils.

**Älä asenna MySQL tällä hetkellä** . Debian ja Ubuntu asennuksen komentosarjoja alustaa MySQL-datakansiossa `/var/lib/mysql`, mutta koska hakemiston korvattu DRBD tiedostojärjestelmässä, annettava toiminto myöhemmin.

Tässä vaiheessa on kannattaa myös tarkistaa (käyttämällä `/sbin/ifconfig`), sekä VMs käytät 10.10.10.0/24 aliverkon osoitteet ja että ne ping toisiinsa nimen mukaan. Halutessasi voit myös käyttää `ssh-keygen` ja `ssh-copy-id` varmistaaksesi, että molemmat VMs ottaa heihin yhteyttä SSH ilman salasanan.

### <a name="setting-up-drbd"></a>DRBD määrittäminen

Luodaan DRBD resurssia, joka käyttää pohjana olevan `/dev/sdc1` osio, jolloin lopputulos `/dev/drbd1` resurssi voi olla muotoiltu käyttämällä ext3 ja käyttää ensisijainen ja toissijainen solmujen. Voit tehdä tämän Avaa `/etc/drbd.d/r0.res` ja kopioi seuraavat resurssien määritykset. Tee molemmat VMs näin:

    resource r0 {
      on `hadb01` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.4:7789;
        meta-disk internal;
      }
      on `hadb02` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.5:7789;
        meta-disk internal;
      }
    }

Tämän jälkeen alusta resurssin käyttämällä `drbdadm` -sekä VMs:

    sudo drbdadm -c /etc/drbd.conf role r0
    sudo drbdadm up r0

Ja lopuksi ensisijainen (`hadb01`) pakottaa DRBD resurssin omistajuuden (perus):

    sudo drbdadm primary --force r0

Jos tarkastelet/prosessi/drbd sisällön (`sudo cat /proc/drbd`) sekä VMs näytössä pitäisi näkyä `Primary/Secondary` - `hadb01` ja `Secondary/Primary` - `hadb02`ja yhtenäinen ratkaisuun tässä vaiheessa. 5 gt levyn synkronoidaan 10.10.10.0/24 verkossa maksutta asiakkaille.

Kun levyn synkronoidaan voit luoda tiedostojärjestelmän `hadb01`. Testausta varten on käytetty ext2, mutta Luo ext3 tiedostojärjestelmän seuraavia ohjeita:

    mkfs.ext3 /dev/drbd1

### <a name="mounting-the-drbd-resource"></a>DRBD resurssin käyttöönottaminen

Valitse `hadb01` olemme on nyt valmis käyttöön DRBD resursseja. Debian ja johdannaiset `/var/lib/mysql` MySQL's datakansiossa nimellä. Koska emme ole vielä asentanut MySQL-olemme luodaan hakemiston ja ottaa käyttöön DRBD resurssi. On `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="setting-up-mysql"></a>MySQL määrittäminen

Kun olet valmis, asenna MySQL `hadb01`:

    sudo apt-get install mysql-server

Saat `hadb02`, sinulla on kaksi vaihtoehtoa. Voit asentaa mysql-palvelin, joka luodaan /var/lib/mysql ja täyttäminen uuden data-kansion ja siirry sitten Poista. On `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

Toinen vaihtoehto on automaattisesti `hadb02` ja asenna sitten mysql-palvelin (asennuksen komentosarjojen huomaat aiempi asennus ja ei kosketa)

On `hadb01`:

    sudo drbdadm secondary –force r0

On `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Jos et aio automaattisesti DRBD nyt, ensimmäinen vaihtoehto on helpompi vaikka arguably vähemmän klassinen. Kun olet määrittänyt, voit aloittaa työskentelyn MySQL-tietokantaan. Valitse `hadb02` (tai sen mukaan, kumpi jokin palvelimet on aktiivinen, DRBD mukaan):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

**Varoitus**: viimeisen tietosuojatiedoissa tehokkaasti poistaa tämän taulukon pääkansion käyttäjän todennusta. Tämä korvataan tuotannon-luokan MYÖNTÄÄ lauseet ja sisältyy epäsuotuisista tarkoituksiin.

Voit myös on otettava käyttöön Verkko MySQL, jos haluat tehdä kyselyjen ulkopuolella VMs, joiden on tarkoitus tässä oppaassa. Avaa molemmat VMs `/etc/mysql/my.cnf` ja Etsi `bind-address`, muuttamalla sen 127.0.0.1 0.0.0.0. Kun olet tallentanut tiedoston, ongelma `sudo service mysql restart` nykyisen ensisijainen käyttöön.

### <a name="creating-the-mysql-load-balanced-set"></a>MySQL-kuormituksen luominen saapuva määrittäminen

Syy Palaa portaaliin ja siirry `hadb01` AM ja sitten päätepisteet. Luo uusi päätepiste, valitse MySQL (TCP-3306) avattavan luettelon ja jakoviivojen *Luo uusi kuormituksen saapuva set* -ruutu. Olemme soittavat Microsoftin kuormitus tasataan päätepisteen `lb-mysql`. Olemme jättää useimmat yksin lukuun ottamatta aika, joka pienennetään 5 (sekunteina, pienin)

Päätepisteen luomisen jälkeen on Siirry `hadb02`, päätepisteet, ja luo uusi päätepiste mutta olemme valitsee `lb-mysql`, valitse sitten avattavasta valikosta MySQL. Voit käyttää myös Azure-CLI tämän vaiheen.

Tällä hetkellä on kaikki annettava klusterin manuaalinen toiminnan.

### <a name="testing-the-load-balanced-set"></a>Lataa testaaminen saapuva määrittäminen

Testit voidaan suorittaa ulkopuolelta tietokoneesta, käyttämällä mikä tahansa MySQL-asiakas sekä sovellusten (phpMyAdmin esimerkiksi käynnissä Azure-sivuston) tässä tapauksessa on käytetty MySQL's komento viivatyökalu toiseen Linux-ruutu:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Kaatuvat manuaalisesti päälle

Voit simuloida failovers nyt MySQL suljetaan, miten DRBD on ensisijainen ja MySQL käynnistetään uudelleen.

Valitse hadb01:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Napsauta hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

Kun olet automaattisesti manuaalisesti toista remote kyselyn ja sen pitäisi toimia täysin.

## <a name="setting-up-corosync"></a>Corosync määrittäminen

Corosync on tarvittavat Pacemaker toimimaan pohjana klusterin infrastruktuuri. Uudelleenaktivoinnin yhteydessä v1 ja v2 käyttäjien (ja muita menetelmiä, kuten Ultramonkey) Corosync on jaon CRM-toimintoja Pacemaker pysyy Hearbeat muistuttamaan enemmän toimintoja.

Saat Corosync tärkeimmät rajoituksen Azure on Corosync haluaa moni päälle lähetyksen unicast communications päälle, mutta Microsoft Azure verkko tukee vain unicast.

Lähettäjä-Corosync on työskentelyä yksittäislähetystilassa ja vain reaali rajoitus on, koska kaikki solmut ovat ei ole yhteydessä keskenään *automagically*, sinun on määritettävä solmut määritysten tiedostojen, mukaan lukien niiden IP-osoitteet. Microsoft käyttää Corosync Esimerkki tiedostoja varten Unicast ja vain muuta sitoa osoite, solmu luettelot ja kirjaaminen directory (Ubuntu käyttää `/var/log/corosync` samalla, kun esimerkin tiedostojen käyttäminen `/var/log/cluster`) ja ottamalla käyttöön quorum työkalut.

**Huomautus `transport: udpu` direktiivin alla ja manuaalisesti määritetty IP-osoitteita solmut**.

Valitse `/etc/corosync/corosync.conf` molempien solmujen:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

Olemme Kopioi tämä kokoonpanotiedosto sekä VMs ja Käynnistä Corosync molemmissa solmuissa:

    sudo service start corosync

Pian palvelun aloittamisen jälkeen klusterin vahvistettava nykyisen Soita ja quorum olisi muodostettu. Olemme voit tarkistaa tämän toiminnon tarkastelemalla lokit tai:

    sudo corosync-quorumtool –l

Tulos muistuttaa alla olevassa kuvassa pitäisi seurata:

![corosync quorumtool - l otoksen tulostus](media/virtual-machines-linux-classic-mysql-cluster/image001.png)

## <a name="setting-up-pacemaker"></a>Pacemaker määrittäminen

Pacemaker käyttää klusterin seurata resurssien, kun primaareista siirtyy alaspäin määrittäminen ja siirry secondaries resursseja. Resurssien voidaan määrittää joukon käytettävissä olevat komentosarjat tai LSB (alusta kaltaisessa) komentosarjoja muiden vaihtoehdosta toiseen.

Haluamme Pacemaker "omista" liitoskohtaa ja MySQL-palvelun DRBD resurssille. Jos Pacemaker voi ottaa käyttöön ja poistaminen käytöstä DRBD, ota se käyttöön / umount se ja Aloita/Lopeta MySQL oikeassa järjestyksessä, kun jokin virheelliset tapahtuu on ensisijainen, tutustu asennus on valmis.

Kun asennat Pacemaker kokoonpanosi pitäisi olla yksinkertainen tarpeeksi, kuten julkaiseminen:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

Tarkista suorittamalla `sudo crm configure show`. Luo nyt-tiedosto (Yammer- `/tmp/cluster.conf`) kanssa on seuraavissa resursseissa:

    primitive drbd_mysql ocf:linbit:drbd \
          params drbd_resource="r0" \
          op monitor interval="29s" role="Master" \
          op monitor interval="31s" role="Slave"

    ms ms_drbd_mysql drbd_mysql \
          meta master-max="1" master-node-max="1" \
            clone-max="2" clone-node-max="1" \
            notify="true"

    primitive fs_mysql ocf:heartbeat:Filesystem \
          params device="/dev/drbd/by-res/r0" \
          directory="/var/lib/mysql" fstype="ext3"

    primitive mysqld lsb:mysql

    group mysql fs_mysql mysqld

    colocation mysql_on_drbd \
           inf: mysql ms_drbd_mysql:Master

    order mysql_after_drbd \
           inf: ms_drbd_mysql:promote mysql:start

    property stonith-enabled=false

    property no-quorum-policy=ignore

Ja lataa se nyt (tarvitset vain yhden solmun toiminto) määritykset:

    sudo crm configure
      load update /tmp/cluster.conf
      commit
      exit

Varmista, että Pacemaker alkaa molemmissa solmuissa käynnistyksen yhteydessä:

    sudo update-rc.d pacemaker defaults

Muutaman sekunnin kuluttua ja käyttämällä `sudo crm_mon –L`, varmista, että jokin oman solmujen on tullut klusterin perusmuotoa ja kaikki resurssit on käynnissä. Voit käyttää asennustapa ja ps, varmista, että resurssit ovat käynnissä.

Näyttökuva seuraavassa `crm_mon` yhden solmun pysäytetty (Lopeta käyttämällä CTRL + C) kanssa

![pysäytetty crm_mon solmu](media/virtual-machines-linux-classic-mysql-cluster/image002.png)

Ja tässä näyttökuvassa näkyy molemmat solmut yksi pää- ja yhden toissijaisen kanssa:

![crm_mon toiminnallisia perustyyli/toissijaisen](media/virtual-machines-linux-classic-mysql-cluster/image003.png)

## <a name="testing"></a>Testaaminen

Olemme ryhtyä automaattinen automaattisesti simuloinnissa. Näin voit kahdella tavalla: Pehmeä ja Kova. Pehmeä tapaa, jolla on käytössä klusterin Sammuta funktio: ``crm_standby -U `uname -n` -v on``. Käytä perustyylissä, toissijaisen tulevat. Määritä tämä takaisin käytöstä (crm_mon selviää yksi solmu on valmiustilassa muussa)

Kiintolevyillä tavalla suljetaan ensisijainen AM (hadb01) portaalin kautta tai muuttaminen/RL AM (eli pysäyttää, sulkeminen) valitse sitten Microsoft kanssaan Corosync ja Pacemaker mukaan Signalointi perusmuodon siirtyä alaspäin. Emme voi testata (käytännöllinen ylläpito windows), mutta on voimassa skenaarion kiinnittämällä AM vain.

## <a name="stonith"></a>STONITH

Olisi mahdollista antaa AM-Sammuta sijasta STONITH komentosarjan, joka ohjaa fyysinen laite Azure-CLI kautta. Voit käyttää `/usr/lib/stonith/plugins/external/ssh` kuin perus- ja ota käyttöön STONITH klusterin määrityksessä. Azure CLI on asennettava yleisesti ja julkaise asetukset/profiilin ladattava klusterin käyttäjälle.

Esimerkki käytettävissä [GitHub](https://github.com/bureado/aztonith)resurssin koodi. Sinun on muutettava klusterin kokoonpano lisäämällä seuraavat `sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

**Huomautus:** komentosarja ei tehdä ylös/alas-tarkistukset. Alkuperäinen SSH resurssi oli 15 ping tarkistukset mutta Azure-AM Palautumisaika voi olla yksi muuttuja.

## <a name="limitations"></a>Rajoitukset

Seuraavat rajoitukset:

- Linbit DRBD resurssin komentosarja, joka hallitsee DRBD Pacemaker käyttää resurssiksi `drbdadm down` kun suljetaan solmu, vaikka solmu vain suorittaminen valmiustilassa. Tämä ei ole paras mahdollinen, koska toissijaisen ei voi Synkronoi DRBD resurssin samalla, kun perusmuodon saa kirjoituksia. Jos perusmuodon eivät graciously, toissijaisen voit menee vanhempia tiedostojärjestelmän-tilaan. Kahdella mahdolliset, ratkaiseminen näin:
  - Pakotetut `drbdadm up r0` kaikki klusterin solmuissa paikallisen (ei clusterized) watchdog kautta tai
  - Muokkaaminen linbit DRBD komentosarjan varmistamalla, että `down` ei kutsutaan, joka `/usr/lib/ocf/resource.d/linbit/drbd`.
- Kuormituksen on oltava vähintään viisi sekuntia niin sovellusten olisi klusterin aware ja aiempaa varmatoimisempia aikakatkaisun; voi vastata muut arkkitehtuureihin avulla, kuten sovelluskohtaisesta olevien, kyselyn middlewares jne.
- MySQL säätäminen on tarvittavat kirjoittaminen tapahtuu sane tahdissa ja tallentaa on tyhjennetty levylle niin usein kuin mahdollista Pienennä muistin tappio
- Kirjoita suorituskyky on riippuvainen AM interconnect virtual valitsin, järjestelmä käyttää DRBD replikoida laite on
