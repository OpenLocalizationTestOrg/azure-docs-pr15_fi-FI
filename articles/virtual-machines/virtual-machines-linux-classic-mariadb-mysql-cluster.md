<properties
    pageTitle="Käynnissä MariaDB (MySQL)-klusterin Azure"
    description="Luo MariaDB + Galera MySQL klusterin-Azuren näennäiskoneiden"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="sabbour"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="04/15/2015"
    ms.author="v-ahsab"/>

# <a name="mariadb-mysql-cluster---azure-tutorial"></a>MariaDB (MySQL) klusterin - Azure opetusohjelma

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

> [AZURE.NOTE]  MariaDB yrityksen klusterin on nyt saatavilla Azure Marketplacesta.  Uusi tarjoaa otetaan automaattisesti käyttöön MariaDB Galera-klusterin ja ARM. Sinun on käytettävä https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/-uusi versio 

Olemme luot usean pääkohteen [Galera](http://galeracluster.com/products/) klusterin [MariaDBs](https://mariadb.org/en/about/), tehokkaat, skaalattava ja luotettavat drop-in korvaa MySQL toimivat hyvin käytettävissä Azuren näennäiskoneiden-ympäristössä.

## <a name="architecture-overview"></a>Arkkitehtuuri yleiskatsaus

Tässä ohjeaiheessa tekee seuraavat toimet:

1. 3-solmu-klusterin luominen
2. Erota tietojen levyjen OS levyltä
3. Tiedot-levyjen luominen RAID-0/Raidallinen asetusta niin, että IOPS
4. 3 solmujen kuormituksen tasauspalvelun Azure ladata avulla
5. Voit pienentää toistuvien työ, Luo AM-kuva, joka sisältää MariaDB + Galera ja muiden klusterin VMs luoda sen avulla.

![Arkkitehtuuri](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Setup.png)

> [AZURE.NOTE]  Tässä ohjeaiheessa käyttää [Azure CLI](../xplat-cli-install.md) työkaluja, joten varmista, että voit ladata ne ja yhdistä ne Azure tilaus ohjeiden mukaisesti. Jos tarvitset komennot ovat käytettävissä Azure-CLI viittaus, tutustu tämä linkki [Azure CLI komento-opas](../virtual-machines-command-line-tools.md). Myös tarvittavan [SSH-avain todennustavaksi] ja **.pem tiedostosijainti**muistiin.


## <a name="creating-the-template"></a>Mallin luominen

### <a name="infrastructure"></a>Infrastruktuuri

1. Resurssien säilyttämiseen yhdessä affiniteetti-ryhmän luominen

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"

2. Luo virtuaalisia verkko

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet

3. Tallennustilan tilin isännöimiseen Microsoftin levyjen luominen. Huomaa, että sinun ei kannata voidaan sijoittaa yli 40 raskaasti käyttää levyjen saman tallennustilan tilin pallolla 20 000 IOPS tilin enimmäiskoko välttämiseksi. Tässä tapauksessa harjoitteluistonto äärimmäisenä käytöstä tämän numeron niin olemme tallentaa kaikki saman tilin Yksinkertaisuuden

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster

3. Etsi CentOS 7 virtuaalikoneen kuvan nimi

        azure vm image list | findstr CentOS
Tämä siirtää suunnilleen `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`. Älä käytä nimessä seuraavassa vaiheessa.

4. **/Path/to/key.pem** tilalle tallennuspaikka luotu .pem SSH avaimen polku AM-mallin luominen

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser

5. 4-x 500 gt tietoja levyjen liittäminen AM käytettäviksi RAID-määritys

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd

6. SSH malliin AM, jotka olet luonut osoitteessa **mariadbhatemplate.cloudapp.net:22** ja muodostaa yhteyden yksityinen avain.

### <a name="software"></a>Ohjelmiston

1. Hanki pääkansio

        sudo su

2. Asenna RAID tuki:

     - Asenna mdadm

                yum install mdadm

     - Luo EXT4-tiedostojärjestelmän RAID0/raita-asetukset

                mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
                mdadm --detail --scan >> /etc/mdadm.conf
                mkfs -t ext4 /dev/md0

     - Ota kohdassa kansion luominen

                mkdir /mnt/data

     - Noutaa juuri luomasi RAID-laite Tunnuksena

                blkid | grep /dev/md0

     - Muokkaa /etc/fstab

                vi /etc/fstab

     - Lisää laite silloin, jos haluat ottaa käyttöön automaattisen mouting uudelleenkäynnistyksen yhteydessä UUID tilalle ennen **blkid** -komento saadun arvon

                UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2

     - Ota käyttöön uusi osio

                mount /mnt/data

3. Asenna MariaDB:

     - Luo MariaDB.repo-tiedosto:

                vi /etc/yum.repos.d/MariaDB.repo

     - Täytä siihen kanssa sisällön alapuolella

                [mariadb]
                name = MariaDB
                baseurl = http://yum.mariadb.org/10.0/centos7-amd64
                gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
                gpgcheck=1

     - Poista aiemmin postfix ja mariadb-kirjastoa, jotta vältät ristiriidat

            yum remove postfix mariadb-libs-*

     - Asenna MariaDB Galera

            yum install MariaDB-Galera-server MariaDB-client galera

4. Siirrä MySQL-datakansiossa RAID lohko-laite

     - Kopioi Nykyinen MySQL-kansio uuteen paikkaan ja poistaa vanhan hakemiston

            cp -avr /var/lib/mysql /mnt/data  
            rm -rf /var/lib/mysql

     - Vastaavasti uuden kansion käyttöoikeuksien määrittäminen

            chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/  

     - Luo symlink, valitsemalla vanhan hakemiston RAID-osioon uuteen sijaintiin

            ln -s /mnt/data/mysql /var/lib/mysql

5. Koska [SELinuxin häiritse klusterin toimintoja](http://galeracluster.com/documentation-webpages/configuration.html#selinux), ei tarvitse poistaa käytöstä nykyisessä istunnossa (kunnes yhteensopiva näkyy). Muokkaa `/etc/selinux/config` käytöstä myöhemmin käynnistyy:

            setenforce 0

       then editing `/etc/selinux/config` to set `SELINUX=permissive`

6. Vahvista MySQL suoritetaan

    - Käynnistä MySQL

            service mysql start

    - Suojatun MySQL-asennuksen, määrittää ylimmällä tasolla, poista anonyymit käyttäjät remote pääkansion kirjautuminen käytöstä ja että testitietokanta poistaminen

            mysql_secure_installation

    - Käyttäjän klusterin toiminnot ja vaihtoehtoisesti sovellukset-tietokannan luominen

            mysql -u root -p
            GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
            exit

   - Lopeta MySQL

            service mysql stop

7. Luo määritys paikkamerkki

    - Muokkaa Luo klusterin asetukset paikkamerkki MySQL-määrityksiä. Korvaa **`<Vairables>`** tai kommentointi nyt. Tapahtuu, kun AM luodaan mallin pohjalta.

            vi /etc/my.cnf.d/server.cnf

    - Muokkaa **[galera]** -osassa ja poista se

    - **[Mariadb]** -osan muokkaaminen

            wsrep_provider=/usr/lib64/galera/libgalera_smm.so
            binlog_format=ROW
            wsrep_sst_method=rsync
            bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
            default_storage_engine=InnoDB
            innodb_autoinc_lock_mode=2

            wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
            #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
            #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
            #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
            #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server

8. Avaa tarvittavat portit palomuurin (joko FirewallD CentOS 7: ssä)

    - MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
    - GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
    - GALERA IST:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
    - RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
    - Lataa palomuurin:`firewall-cmd --reload`

9.  Optimoi järjestelmän suorituskykyä. Tässä artikkelissa on lisätietoja [suorituskyvyn säätäminen strategia](virtual-machines-linux-classic-optimize-mysql.md) viitata

    - Muokkaa MySQL-määritystiedoston uudelleen

            vi /etc/my.cnf.d/server.cnf

    - Muokkaa **[mariadb]** -kohta ja liittää alapuolella

    > [AZURE.NOTE] On suositeltavaa **innodb\_puskurin\_pool_size** on 70 % oman AM muistin. Se on määritetty tähän 2.45 gt osoitteessa Normaali Azure AM 3,5 Gigatavua RAM-Muistia kanssa.

            innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70% of physical memory.
            innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
            max_connections = 5000 # A larger value will give the server more time to recycle idled connections
            innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
            innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
            innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
            query_cache_size = 0

10. Lopeta MySQL MySQL-palvelun poistaminen käynnistettäessä välttämiseksi messing klusterin määrittäminen, kun lisäät uuden solmun käynnissä ja deprovision koneen.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision

11. Sieppaa AM portaalin kautta. (Tällä hetkellä [ongelman #1268 Azure-CLI-] Työkalut kuvataan Azure CLI työkalujen kuvien varaa liitettyjä tietoja levyjen kertoma.)

    - Portaalin kautta tietokoneen sammuttamisen
    - Valitse sieppaus ja määritä kuvan nimi **mariadb galera -** kuva ja ongelman kuvaus ja tarkista "On suoritetaan waagent".
    ![Sieppaa virtuaalikoneen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture.png)
    ![siepata virtuaalikoneen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture2.PNG)

## <a name="creating-the-cluster"></a>Klusterin luominen

Luo 3 VMs mallin olet juuri luonut ja sitten määritettäessä ja käynnistettäessä klusterin ulos.

1. Luo ensimmäinen CentOS 7 AM luomasi **mariadb galera-kuva** -kuvasta, antamalla VPN nimi **mariadbvnet** ja aliverkon **mariadb**-koneen koon **Normaali**pilvipalvelussa kulkeva nimi on **mariadbha** (tai jostakin nimi, jota haluat käyttää mariadbha.cloudapp.net kautta) nimi, tämä asetus koneen **mariadb1** ja käyttäjänimi on **azureuser**ja SSH käytön ja kulkeva SSH varmenteen .pem tiedosto- ja korvaamisvaihtoehtoja **/path/to/key.pem** polulla kohtaa, johon voit tallennettu luotu .pem SSH-näppäintä.

    > [AZURE.NOTE] Alla olevia komentoja jaetaan selkeyttä Monirivinen päälle, mutta jokainen kuin yksi rivi on syötettävä.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser

2. Voit luoda 2 Lisää näennäiskoneiden _yhdistämällä_ ne parhaillaan luotavien **mariadbha** pilvipalvelussa muuttaminen **AM nimi** sekä **SSH portti** ei ole ristiriidassa muiden VMs saman pilvipalvelussa portin.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
ja MariaDB3

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser

3. Sinun on kaikkien 3 VMs sisäinen IP-osoitteen hankkiminen seuraavaan vaiheeseen:

    ![Käytön IP-osoite](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/IP.png)

4. SSH 3 VMs kyselyjä ja ja muokkaa kunkin kokoonpanotiedosto

        sudo vi /etc/my.cnf.d/server.cnf

    uncommenting **`wsrep_cluster_name`** ja **`wsrep_cluster_address`** poistamalla **#** alussa ja kelpoisuuden ne ovat varmasti oikein.
    Lisäksi korvaa **`<ServerIP>`** - **`wsrep_node_address`** ja **`<NodeName>`** - **`wsrep_node_name`** kanssa AM IP-osoite ja nimeä tarpeen mukaan ja kommentointi ne rivit.

5. Klusterin käynnistäminen MariaDB1 ja anna järjestelmän käynnistyksen yhteydessä

        sudo service mysql bootstrap
        chkconfig mysql on

6. Käynnistä MySQL MariaDB2 ja MariaDB3 ja anna järjestelmän käynnistyksen yhteydessä

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balancing-the-cluster"></a>Kuormituksen klusterin
Liitetty VMs luotaessa lisätä ne yhdeksi laiduntamismahdollisuuksien asettaminen kutsutaan **clusteravset** varmistaa ne sijoitetaan eri vika ja päivittää toimialueet ja että Azure koskaan onko kaikissa tietokoneissa ylläpidon samalla kertaa. Tässä määrityksessä vastaa aiotaan lisätä mukaan kyseisen Azure palvelussa palvelutasosopimusta (SLA).

Nyt voit Azure kuormituksen saldo pyynnöt Microsoftin 3 solmujen välillä.

Suorita komentojen tietokoneeseen käyttämällä Azure-CLI.
Komennon parametrit-rakenne on seuraava:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

Lopuksi CLI määrittää kuormituksen näytteenottimen välin 15 sekunnin (joka voi olla hieman liian pitkä), koska se muuttuu **päätepisteet** -portaalissa jokin VMs

![Muokkaa päätepiste](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint.PNG)

Valitse Valitse Reconfigure The Load-Balanced määrittäminen ja siirry seuraavaan

![Määritä Lataa tasapainoinen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint2.PNG)

Valitse Muokkaa tutkia, jolloin viisi sekuntia ja tallentaminen

![Muuta näytteenottimen aikaväli](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validating-the-cluster"></a>Klusterin vahvistaminen

Kova työnteko on valmis. Klusterin pitäisi olla nyt käytettävissä osoitteessa `mariadbha.cloudapp.net:3306` joka osumien kuormituksen ja reitittää välillä 3 VMs sujuvasti ja tehokkaasti.

Tuttuja MySQL-asiakasohjelman avulla voit yhdistää tai liitä olevista VMs Tarkista tämän klusterin toimii.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Valitse Luo uusi tietokanta ja lisää tietoja

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

Alla olevassa taulukossa aiheuttaa

    +----+--------+
  	| id | value  |
    +----+--------+
  	|  1 | Value1 |
  	|  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa 3 luomasi solmu MariaDB + Galera erittäin käytettävissä klusterin-Azuren näennäiskoneiden CentOS 7. VMs ovat kuormituksen tasauspalvelun Azure ladata kanssa.

Haluat ehkä tutustumme [klusterin MySQL Linux toinen tapa](virtual-machines-linux-classic-mysql-cluster.md) ja [optimointi](virtual-machines-linux-classic-optimize-mysql.md)ja testaa MySQL suorituskyvyn Azure Linux VMs.

<!--Anchors-->
[Architecture overview]: #architecture-overview
[Creating the template]: #creating-the-template
[Creating the cluster]: #creating-the-cluster
[Load balancing the cluster]: #load-balancing-the-cluster
[Validating the cluster]: #validating-the-cluster
[Next steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Galera]: http://galeracluster.com/products/
[MariaDBs]: https://mariadb.org/en/about/
[Luo SSH-avain todennusta varten]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[Azure-CLI ongelman #1268]:https://github.com/Azure/azure-xplat-cli/issues/1268
