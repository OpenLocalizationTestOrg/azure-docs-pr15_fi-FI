<properties
    pageTitle="MySQL-Linux AM määrittäminen | Microsoft Azure "
    description="Opi asentamaan MySQL-pino Linux virtual tietokoneeseen (Ubuntu tai RedHat perheen OS) Azure-tietokannassa"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="how-to-install-mysql-on-azure"></a>MySQL asentamisesta Azure


Tässä artikkelissa kerrotaan, asentaa ja määrittää MySQL Azure virtual-tietokoneessa, jossa Linux.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


##<a name="install-mysql-on-your-virtual-machine"></a>MySQL asentaminen virtuaalikoneen

> [AZURE.NOTE] Sinulla on jo käynnissä Linux, jotta voit suorittaa tässä opetusohjelmassa Microsoft Azure virtual koneen. [Azure Linux AM opetusohjelma](virtual-machines-linux-quick-create-cli.md) , jotta luominen ja määrittäminen Linux-AM kanssa, katso `mysqlnode` AM nimeksi ja `azureuser` käyttäjänä, ennen kuin jatkat.

Käytä tässä tapauksessa 3306 portin MySQL-porttina.  

Muodosta yhteys Linux AM luomasi painovärit, muste kautta. Jos käytät Azure Linux AM ensimmäistä kertaa, katso, miten voit käyttää painovärit, muste yhdistäminen Linux AM [tähän](virtual-machines-linux-mac-create-ssh-keys.md).

Käytämme säilöön paketin asentamiseksi MySQL5.6 esimerkkinä sisältö. MySQL5.6 asiassa Lisää Käyttömukavuuden suorituskyvyn kuin MySQL5.5.  Lisätietoja [tästä](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).


###<a name="how-to-install-mysql56-on-ubuntu"></a>Miten voit asentaa MySQL5.6 Ubuntu
Käytämme Linux AM kanssa Ubuntu azuren tähän.

- Vaihe 1: Asenna MySQL palvelimen 5.6 valitsin, joka `root` käyttäjä:

            #[azureuser@mysqlnode:~]sudo su -

    Asenna mysql-palvelin 5.6:

            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6

    Asennuksen aikana näet valintaikkunan ikkunan poping enintään Kysy, voit määrittää MySQL pääkansion salasana ja on määritetty salasana tähän.

    ![Kuva](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p1.png)


    Kirjoita salasana uudelleen.

    ![Kuva](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p2.png)

- Vaihe 2: Kirjautuminen MySQL-palvelin

    MySQL-palvelin asennuksen päätyttyä MySQL-palvelu käynnistyy automaattisesti. MySQL-palvelin, jossa voit kirjautua sisään `root` käyttäjä.
    Käytä Kirjaudu sisään ja input salasana-komennon alla.

             #[root@mysqlnode ~]# mysql -uroot -p

- Vaihe 3: Käynnissä MySQL-palvelun hallinnointi

    (a) Hae MySQL-palvelun tila

             #[root@mysqlnode ~]# service mysql status

    (b) MySQL-palvelun käynnistäminen

             #[root@mysqlnode ~]# service mysql start

    (c) MySQL-palvelun pysäyttäminen

             #[root@mysqlnode ~]# service mysql stop

    (d) MySQL-palvelun käynnistäminen uudelleen

             #[root@mysqlnode ~]# service mysql restart


###<a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>MySQL asentamisesta punainen on OS-tuoteperheeseen, esimerkiksi CentOS, Oracle Linux
Käytämme Linux AM CentOS tai Oracle Linux tähän.

- Vaihe 1: Lisää MySQL Yum säilö valitsin, `root` käyttäjä:

            #[azureuser@mysqlnode:~]sudo su -

    Lataa ja asenna MySQL-versiossa paketti:

            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm

- Vaihe 2: Muokkaa tiedostoon, jotta saat lataaminen MySQL5.6 MySQL-säilöön alapuolella.

            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo

    Päivitä tämän tiedoston jokaisen arvon alla:

        \# *Enable to use MySQL 5.6*

        [mysql56-community]
        name=MySQL 5.6 Community Server

        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/

        enabled=1

        gpgcheck=1

        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

- Vaihe 3: Asenna MySQL MySQL-säilöstä asentaa MySQL:

           #[root@mysqlnode ~]#yum install mysql-community-server

    MySQL JÄLLEENMYYNTIHINNAN paketin ja kaikki liittyvät paketit asennetaan.

- Vaihe 4: Käynnissä MySQL-palvelun hallinnointi

    (a) Tarkista palvelutila MySQL-palvelin:

           #[root@mysqlnode ~]#service mysqld status

    (b) Tarkista, onko MySQL portin oletuspalvelimeen on käynnissä:

           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306


    (c) Aloita MySQL-palvelin:

           #[root@mysqlnode ~]#service mysqld start

    (d) Pysäytä MySQL-palvelin:

           #[root@mysqlnode ~]#service mysqld stop

    (e) Määritä MySQL Aloita milloin järjestelmän käynnistys-määrittäminen:

           #[root@mysqlnode ~]#chkconfig mysqld on


###<a name="how-to-install-mysql-on-suse-linux"></a>MySQL asentamisesta SUSE Linux
Käytämme Linux AM kanssa OpenSUSE tähän.

- Vaihe 1: Lataa ja asenna MySQL-palvelin

    Siirry `root` käyttäjän – komennon alla:  

           #sudo su -

    Lataa ja asenna MySQL-paketti:

           #[root@mysqlnode ~]# zypper update

           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql

- Vaihe 2: Käynnissä MySQL-palvelun hallinnointi

    (a) MySQL-palvelin etenemisen tarkistaminen:

           #[root@mysqlnode ~]# rcmysql status

    (b) Tarkista onko oletusportti MySQL-palvelin:

           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306


    (c) Aloita MySQL-palvelin:

           #[root@mysqlnode ~]# rcmysql start

    (d) Pysäytä MySQL-palvelin:

           #[root@mysqlnode ~]# rcmysql stop

    (e) Määritä MySQL Aloita milloin järjestelmän käynnistys-määrittäminen:

           #[root@mysqlnode ~]# insserv mysql

###<a name="next-step"></a>Seuraava vaihe
Etsi lisää käyttö ja MySQL [tähän](https://www.mysql.com/)koskevat tiedot.
