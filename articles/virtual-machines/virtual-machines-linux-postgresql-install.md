<properties
    pageTitle="Määritä PostgreSQL-Linux AM | Microsoft Azure"
    description="Lue, miten voit asentaa ja määrittää PostgreSQL Linux virtual tietokoneeseen Azure-tietokannassa"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


# <a name="install-and-configure-postgresql-on-azure"></a>Asenna ja määritä PostgreSQL-Azure

PostgreSQL on samankaltainen kuin Oracle ja DB2 Lisäasetukset Avaa lähde-tietokanta. Se on valmiina enterprise-toimintoja, kuten koko HAPPAMAN yhteensopivuuden, luotettava tapahtumien käsittely ja usean version samanaikainen ohjausobjektin. Se tukee myös standardeja, kuten ANSI SQL: n ja SQL/MED (mukaan lukien ulkomaan tietojen kääreisiin Oracle, MySQL, MongoDB ja monista muista). On erittäin extensible yli 12 käsitteellisiä kielet, GINIÄ ja GiST indeksit paikkatietojen tietojen tuki tai useita NoSQL kaltaisten ominaisuuksien tuki JSON tai avain arvoihin perustuvan sovellusten kanssa.

Tässä artikkelissa kerrotaan, asentaa ja määrittää PostgreSQL Azure virtual-tietokoneessa, jossa Linux.


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="install-postgresql"></a>Asenna PostgreSQL

> [AZURE.NOTE] Sinulla on jo Azure virtual-tietokoneessa, jossa Linux, jotta voit suorittaa tässä opetusohjelmassa. Luo ja määritä Linux AM, ennen kuin jatkat on ohjeaiheessa [Azure Linux AM opetusohjelma](virtual-machines-linux-quick-create-cli.md).

Määritä portti 1999 tässä tapauksessa PostgreSQL-porttiin.  

Muodosta yhteys Linux AM luomasi painovärit, muste kautta. Jos käytät Azure-Linux AM ensimmäistä kertaa, katso, [miten voit käyttää SSH kanssa Linux Azure-](virtual-machines-linux-mac-create-ssh-keys.md) opit käyttämään painovärit, muste Linux AM.

1. Suorita seuraava komento siirtyy pääkansio (järjestelmänvalvojat):

        # sudo su -

2. Jotkin jaot riippuvuuksia, sinun on asennettava ennen asennusta PostgreSQL. Tarkista, että distro tämän luettelon ja suorita komento:

    - Punainen on kantaluku Linux:

            # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

    - Debian perus Linux:

            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  

    - SUSE Linux:

            # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

3. Lataa PostgreSQL pääkansion kyselyjä ja unzip paketin:

        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/

        # tar jxvf  postgresql-9.3.5.tar.bz2

    Edellä on esimerkki. Löydät yksityiskohtaisempia Lataa osoite [FP, / pub/lähde/indeksi](https://ftp.postgresql.org/pub/source/).

4. Aloita Luo Suorita nämä komennot:

        # cd postgresql-9.3.5

        # ./configure --prefix=/opt/postgresql-9.3.5

5. Jos haluat luoda kaikki tiedot, jotka voit on muodostettu, mukaan lukien dokumentaatio (HTML- ja mies sivujen) sekä muita moduulit (contrib), suorita seuraava komento:

        # gmake install-world

    Näyttöön tulee seuraava vahvistusviestissä:

        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>Määritä PostgreSQL

1. (Valinnainen) Luoda symbolinen linkki Lyhennä sisällä versionumero PostgreSQL viittaus:

        # ln -s /opt/pgsql9.3.5 /opt/pgsql

2. Tietokannan hakemiston luominen:

        # mkdir -p /opt/pgsql_data

3. Pääkansio käyttäjän luominen ja muokkaaminen kyseisen käyttäjän profiiliin. Siirry sitten käyttöoikeuden uudelle käyttäjälle (kutsutaan *postgres* tässä esimerkissä):

        # useradd postgres

        # chown -R postgres.postgres /opt/pgsql_data

        # su - postgres

   > [AZURE.NOTE] Tietoturvasyistä PostgreSQL käyttää pääkansio käyttäjän alusta, Käynnistä-painiketta tai sulje tietokanta.


4. Muokkaa *bash_profile* tiedostoa kirjoittamalla alla olevia komentoja. Nämä rivit lisätään *bash_profile* tiedoston loppuun:

        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF

5. Suorita *bash_profile* tiedostoa:

        $ source .bash_profile

6. Vahvista asennuksen avulla seuraava komento:

        $ which psql

    Jos asennuksen onnistuu, näet seuraavan vastauksen:

        /opt/pgsql/bin/psql

7. Voit myös tarkistaa PostgreSQL-versio:

        $ psql -V

8. Alusta tietokanta:

        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W

    Näyttöön tulee seuraava tulos:

![Kuva](./media/virtual-machines-linux-postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>PostgreSQL määrittäminen

<!--    [postgres@ test ~]$ exit -->

Suorita seuraavat komennot:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Muokkaa kahden muuttujan /etc/init.d/postgresql-tiedostossa. Etuliite on määritetty PostgreSQL asennuspolku: **/opt/pgsql**. PGDATA on määritetty tietojen tallennustilan polun PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Kuva](./media/virtual-machines-linux-postgresql-install/no2.png)

Muuta tiedoston, jotta suoritettavan:

    # chmod +x /etc/init.d/postgresql

Aloita PostgreSQL:

    # /etc/init.d/postgresql start

Tarkista, onko PostgreSQL päätepisteen käyttöön:

    # netstat -tunlp|grep 1999

Näyttöön tulee seuraava tulos:

![Kuva](./media/virtual-machines-linux-postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Yhteyden muodostaminen Postgres-tietokantaan

Siirry postgres käyttäjän uudelleen:

    # su - postgres

Luo Postgres tietokanta:

    $ createdb events

Juuri luomasi tapahtumat-tietokantayhteyden muodostamisessa:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Luo ja Postgres taulukon poistaminen

Nyt kun olet muodostanut yhteyden tietokantaan, voit luoda taulukoita ei.

Luoda uuden Esimerkki Postgres taulukon esimerkiksi käyttämällä seuraava komento:

    CREATE TABLE potluck (name VARCHAR(20), food VARCHAR(30),   confirmed CHAR(1), signup_date DATE);

Voit nyt määrittäminen Nelisarakkeinen taulukko, jossa seuraavat sarakkeiden nimet ja rajoitukset:

1. VARCHAR-komento on kohdassa 20 merkin pituinen rajoita "nimi"-sarake.
2. "Ruoka"-sarake sisältää ruoka kohde, joka tuo mukanaan kaikille käyttäjille. VARCHAR rajoittaa teksti on alle 30 merkkiä.
3. "Vahvistettu" sarakkeen tietueet, onko kyseinen henkilö on RSVP'd Nyyttikestit. "K" ja "O" ovat kelvollisia arvoja.
4. "Päivämäärä" ‑sarakkeessa näkyy ne rekisteröinnin tapahtuman. Postgres edellyttää, että päivämäärät kirjoitetaan VVVV-KK-PP

Sinun pitäisi näkyä seuraavat, jos taulukko on luotu:

![Kuva](./media/virtual-machines-linux-postgresql-install/no4.png)

Voit myös tarkistaa Taulukkorakenteen käyttämällä seuraava komento:

![Kuva](./media/virtual-machines-linux-postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Tietojen lisääminen taulukkoon

Lisää ensin rivin tiedot:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Tämä tulos pitäisi näkyä seuraavat:

![Kuva](./media/virtual-machines-linux-postgresql-install/no6.png)

Voit lisätä muutama osallistujia myös taulukossa. Seuraavassa on joitakin asetuksia, tai voit luoda omia:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Näytä taulukot

Käytä seuraavaa komentoa näyttämään taulukkoon:

    select * from potluck;

Tulos on seuraava:

![Kuva](./media/virtual-machines-linux-postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Taulukon tietojen poistaminen

Seuraavalla komennolla voit poistaa taulukon tietoja:

    delete from potluck where name=’John’;

Tämä toiminto poistaa kaikki "John"-rivin tiedot. Tulos on seuraava:

![Kuva](./media/virtual-machines-linux-postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Taulukon tietojen päivittäminen

Seuraavalla komennolla taulukon tietojen päivittäminen. Tämä yhden Sandy on vahvistanut, että hän on osallistumisesta, jotta muutamme hänen RSVP "N", "K":

    UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


##<a name="get-more-information-about-postgresql"></a>Lisätietoja PostgreSQL
Nyt kun olet suorittanut Azure-Linux AM PostgreSQL asennuksen, voit käyttää käyttäminen Azure. Lisätietoja PostgreSQL-käy [PostgreSQL-sivustossa](http://www.postgresql.org/).
