<properties
    pageTitle="Määrittäminen Oracle tietojen suojaa VMs | Microsoft Azure"
    description="Askel opetusohjelma määrittäminen ja täytäntöön Oracle tietojen suojaa Azuren näennäiskoneiden suuri käytettävyys ja tietojen palauttamista varten."
    services="virtual-machines-windows"
    authors="rickstercdn"
    manager="timlt"
    documentationCenter=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/06/2016"
    ms.author="rclaus" />

#<a name="configuring-oracle-data-guard-for-azure"></a>Azure määrittäminen Oracle tietojen suojaa


Tässä opetusohjelmassa kerrotaan, miten voit määrittää ja toteuttaa Oracle tietojen suojaa Azuren näennäiskoneiden ympäristössä hyvän käytettävyyden ja palauttaminen. Opetusohjelman ohjeessa on yksisuuntainen RAC Oracle-tietokantojen replikointi.

Oracle tietojen suojaa tukee tietojen suojaaminen ja palauttaminen Oracle-tietokantaan. On yksinkertainen, tehokas, drop-in-ratkaisun tietojen palauttaminen, tietojen suojaaminen ja suuren käytettävyyden koko Oracle-tietokantaan.

Tässä opetusohjelmassa oletetaan, että sinulla on jo teoreettinen ja käytännön tiedon Oracle-tietokantaan suuren käytettävyyden ja palauttaminen käsitteitä käyttöön. Lisätietoja on artikkelissa [Oracle-web-sivuston](http://www.oracle.com/technetwork/database/features/availability/index.html) ja myös [Oracle tietojen suojaa käsitteitä ja järjestelmänvalvojan oppaan](https://docs.oracle.com/cd/E11882_01/server.112/e41134/toc.htm).

Lisäksi opetusohjelman oletetaan, että seuraavat edellytykset on jo käytössä:

- Olet jo tarkistanut ohjeaiheen [Oracle virtuaalikoneen kuvat - muita huomioon otettavia seikkoja](virtual-machines-windows-classic-oracle-considerations.md) suuren käytettävyyden ja tietojen palauttaminen huomioon otettavia seikkoja-kohdassa. Azure tukee erillinen Oracle-tietokantaan esiintymät, mutta ei Oracle reaali sovelluksen varausyksiköt (Oracle RAC) tällä hetkellä.


- Olet luonut kaksi näennäiskoneiden (VMs) Azure käyttämällä samaa platform annettu Oracle Enterprise Edition-kuva. Varmista, näennäiskoneiden on [sama pilvipalvelussa](virtual-machines-windows-load-balance.md) ja samassa Virtual verkossa, jotta he voivat käyttää toistensa päälle pysyvä yksityinen IP-osoite. Lisäksi kannattaa sijoittaa VMs saman [saatavuus](virtual-machines-windows-manage-availability.md) sallimaan Azure sijoittaa ne erillisessä vika toimialueille ja päivittää toimialueet. Oracle tietojen suojaa on käytettävissä vain Oracle-tietokantaan Enterprise Edition. Jokainen tietokone on oltava vähintään 2 Gigatavua muistia ja 5 gt levytilaa. Katso uusimmat tiedot annettu AM koot ympäristössä [Virtuaalikoneen koot Azure](virtual-machines-windows-sizes.md). Jos tarvitset lisää levyaseman oman VMs, voit liittää muita levyjä. Lisätietoja on artikkelissa [liittäminen tietojen DVD-levyllä Virtual koneeseen](virtual-machines-windows-classic-attach-disk.md).



- Olet määrittänyt virtuaalikoneen nimet "KONE1" kuin ensisijaisen AM ja "KONE2" valmiustilassa AM Azure perinteinen portaalissa osoitteessa.

- Olet määrittänyt **ORACLE_HOME** ympäristömuuttuja osoittavan samaan pääkansion oracle-asennuspolku perus-ja valmius näennäiskoneiden, kuten `C:\OracleDatabase\product\11.2.0\dbhome_1\database`.

- Kirjaudut Windows server **Järjestelmänvalvojat** -ryhmän jäsen tai **ORA_DBA** -ryhmän jäsen.

Tässä opetusohjelmassa menettelet seuraavasti:

Toteuta fyysinen valmiustilassa tietokantaympäristössä

1. Ensisijainen tietokannan luominen

2. Ensisijaisen tietokannan valmisteleminen valmiustilassa tietokannan luominen

    1. Pakotetun kirjaaminen

    2. Salasana-tiedoston luominen

    3. Määritä valmiustilassa Tee lokia

    4. Arkistoinnin ottaminen käyttöön

    5. Ensisijaisen tietokannan alustus parametrien määrittäminen

Fyysinen valmius-tietokannan luominen

1. Valmiustilassa tietokannan alustus parametri-tiedosto

2. Määritä listener ja tnsnames tukemaan tietokannan perus- ja valmiustilassa tietokoneissa

    1. Määritä listener.ora sekä palvelimissa pitoon molemmat tietokannat tapahtumille

    2. Pidä tapahtumat perus- ja valmiustilassa tietokantoja, määrittää tnsnames.ora perus- ja valmius-näennäiskoneiden. 

    3. Käynnistä kuuntelua ja tarkista tnsping käyttöön sekä näennäiskoneiden sekä palveluihin.

3. Käynnistä valmiustilassa esiintymän nomount-tilaan

4. Käytä RMAN Kloonaa tietokannan ja valmius-tietokannan luominen

5. Fyysinen valmius-tietokannan käyttöönotto hallitun palautus-tilassa

6. Tarkista fyysinen valmiustilassa tietokanta

> [AZURE.IMPORTANT] Tässä opetusohjelmassa on määrittäminen ja verrataan laitteiston ja ohjelmiston-asetukset ovat seuraavat:
>
>|                      | **Ensisijainen tietokanta**                      | **Valmiustilassa tietokanta**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Oracle-versio**   | Oracle11g Enterprise-versiossa (11.2.0.4.0) | Oracle11g Enterprise-versiossa (11.2.0.4.0) |
>| **Tietokoneen nimi**     | KONE1                                  | KONE2                                  |
>| **Käyttöjärjestelmä** | Windows 2008 R2                           | Windows 2008 R2                           |
>| **Oracle SUOJAUSTUNNUS**       | TESTI                                      | TESTAA\_STBY                                |
>| **Muisti**           | Min 2 gt                                  | Min 2 gt                                  |
>| **Levytila**       | Min 5 gt                                  | Min 5 gt                                  |

Myöhemmissä versioissa Oracle-tietokantaan ja Oracle tietojen suojaa voi olla joitakin muita muutoksia, jotka haluat ottaa käyttöön. Uusin versio-kohtaisia, tiedot on [Tietoja suojaa](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) ja [Oracle-tietokantaan](http://www.oracle.com/us/corporate/features/database-12c/index.html) ohjeissa Oracle-sivustossa.

##<a name="implement-the-physical-standby-database-environment"></a>Toteuta fyysinen valmiustilassa tietokantaympäristössä
### <a name="1-create-a-primary-database"></a>1. ensisijainen tietokannan luominen

- Luo ensisijaisen tietokannan "TESTATA" ensisijainen virtuaalikoneen. Lisätietoja on artikkelissa luominen ja määrittäminen Oracle-tietokantaan.
- Näkyviin tietokannan nimen, muodosta yhteys tietokantaan SYS käyttäjänä, jolla SYSDBA rooli SQL * Plus komentokehote ja suorita seuraava komento:

        SQL> select name from v$database;

        The result will display like the following:

        NAME
        ---------
        TEST
- Seuraavaksi kyselyn dba_data_files järjestelmän näkymästä tietokantatiedostoja nimet:

        SQL> select file_name from dba_data_files;
        FILE_NAME
        -------------------------------------------------------------------------------
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF

### <a name="2-prepare-the-primary-database-for-standby-database-creation"></a>2. ensisijaisen tietokannan valmisteleminen valmiustilassa tietokannan luominen

Ennen kuin luot valmiustilassa tietokantaan, on suositeltavaa varmistaa ensisijainen tietokanta on määritetty oikein. Seuraavassa on luettelo, sinun on suoritettava vaiheet:

1. Pakotetun kirjaaminen
2. Salasana-tiedoston luominen
3. Määritä valmiustilassa Tee lokia
4. Arkistoinnin ottaminen käyttöön
5. Ensisijaisen tietokannan alustus parametrien määrittäminen

#### <a name="enable-forced-logging"></a>Pakotetun kirjaaminen

Toteuttamaan valmius-tietokannan annettava pakotettu kirjaaminen käyttöön ensisijainen tietokannan. Tämä asetus varmistaa, että vaikka 'nologging'-toiminto on valmis, voimassa kirjaaminen ohittaa ja tee uudelleen-lokit kirjautuneena kaikki toiminnot. Tämän vuoksi on Varmista, että kaikki ensisijainen tietokannassa on kirjautunut ja valmius-replikoinnin sisältää ensisijaisen tietokannan kaikki toiminnot. Voimassa kirjaaminen suorittamalla alter database-lause:

    SQL> ALTER DATABASE FORCE LOGGING;

    Database altered.

#### <a name="create-a-password-file"></a>Salasana-tiedoston luominen

Jos toimitus ja käyttää arkistoidut lokit ensisijaisen palvelimen valmius-palvelimeen, sys salasana on oltava sama perus- ja valmius-palvelimiin. Minkä vuoksi luo ensisijaisen tietokannan Salasanatiedosto ja kopioi se valmius-palvelimeen.

>[AZURE.IMPORTANT] Kun käytät Oracle-tietokantaan 12c, on uusi käyttäjä, **SYSDG**, jonka avulla voit hallita Oracle tietojen suojaa. Lisätietoja on artikkelissa [Oracle-tietokantaan 12 c Release muutokset](http://docs.oracle.com/database/121/UNXAR/release_changes.htm#UNXAR404).

Varmista lisäksi, että ORACLE\_KONE1 ALOITUS-ympäristö on jo määritetty. Jos näin ei ole, määritä se ympäristömuuttuja ympäristömuuttujat-valintaikkunassa. Jos haluat käyttää tässä valintaikkunassa, käynnistää **järjestelmän** -apuohjelma kaksoisnapsauttamalla; **Ohjauspaneelin**Järjestelmä-kuvaketta Valitse **Lisäasetukset** -välilehti ja valitse **Ympäristömuuttujat**. Määritä ympäristömuuttujat **Järjestelmämuuttujat**-kohdasta **Uusi** -painiketta. Kun olet määrittänyt ympäristömuuttujat olemassa olevan Windows-komentokehote Sulje ja avaa uuden.

Suorita seuraava siirtyäksesi Oracle lause\_aloitus-hakemistosta, kuten C:\\OracleDatabase\\tuotteen\\11.2.0\\dbhome\_1\\tietokannan.

    cd %ORACLE_HOME%\database

Luo sitten salasana salasana tiedoston luominen apuohjelmalla, [ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm)tiedoston. KONE1 sama komento Windows kehotteessa Suorita seuraava komento **SYS**salasana salasana-arvo:

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

Tämä komento luo salasanan tiedoston nimennyt sinut PWDTEST.ora ORACLE-\_HOME\\tietokannan hakemisto. Kopioiminen % ORACLE tämän tiedoston\_ALOITUS %\\tietokannan KONE2 hakemistossa manuaalisesti.

#### <a name="configure-a-standby-redo-log"></a>Määritä valmiustilassa Tee lokia

Tämän jälkeen sinun täytyy määrittää valmiustilassa uudelleen lokia niin, että ensisijainen saavat Tee oikein, kun se muuttuu valmius. Tee valmiustilassa lokit luodaan automaattisesti valmius myös luodaan ne valmiiksi tähän avulla. On tärkeää määrittää valmius uudelleen lokit (SRL) yhtä suuri kuin online uudelleen lokitiedot. Nykyinen valmiustilassa Tee lokitiedostojen kokoa on oltava täsmälleen samat nykyisen ensisijaisen tietokannan online Tee lokitiedostojen kokoa.

Suorita seuraava lause SQL-\*PLUS KONE1 komentorivi-ikkuna. V$ lokitiedoston on järjestelmänäkymä, joka sisältää tietoja Tee lokitiedostot.

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file.
    SQL> select bytes from v$log;
    BYTES
    ----------
    52428800
    52428800
    52428800


Huomaa, että 52428800 50 megatavua.

Valitse SQL-\*Plus-ikkunassa Suorita seuraavat lauseet voit lisätä uuden valmiustilassa Tee loki tiedostoryhmän valmiustilassa tietokannan ja määritä numero, joka määrittää ryhmän puolesta ryhmä-lause. Ryhmän luvuilla voi tehdä valmiustilassa uudelleensuorituslokiryhmien helpompaa hallinta:

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

Seuraavaksi suorittamalla seuraavat järjestelmänäkymä luettelon tietoja Tee lokitiedostot. Tämä toiminto tarkistaa myös valmiustilassa uudelleensuorituslokiryhmien on luotu:

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### <a name="enable-archiving"></a>Arkistoinnin ottaminen käyttöön

Ota sitten arkistointi suorittamalla seuraavat lauseet ensisijaisen tietokannan ARCHIVELOG-tilassa ja ota käyttöön automaattinen arkistointi. Voit ottaa arkisto log tilan käyttöönottaminen tietokannan ja suoritettaessa archivelog-komento.

Kirjaudu sisään ensimmäisen kerran, sysdba. Suorita Windows-komentokehotteeseen:

    sqlplus /nolog

    connect / as sysdba

Valitse Sammuta SQL-tietokantaan\*Plus komentokehote:

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

Suorita käynnistys käyttöönotto-komento, joka ottaa tietokannan. Näin varmistat, että Oracle liittää esiintymän määritettyä tietokantaa.

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

Suorita sitten:

    SQL> alter database archivelog;
    Database altered.

Suorita Alter database-lause ja haluat, että tietokanta käytettäväksi Normaali Avaa-lauseen:

    SQL> alter database open;

    Database altered.

#### <a name="set-primary-database-initialization-parameters"></a>Ensisijaisen tietokannan alustus parametrien määrittäminen

Määrittämiseen tietojen suojaa haluat luoda ja valmiustilassa parametrit määrittämistä säännöllisesti pfile (alustus parametrin tekstitiedosto) ensimmäisen. Kun pfile on valmis, sinun on muunnettava parametri-palvelintiedostoa (SPFILE).

Voit hallita tietojen suojaa-ympäristö käyttämällä parametrit alusta. SIITÄ tiedosto. Kun seuraa Tässä opetusohjelmassa, sinun täytyy päivittää ensisijainen tietokannan alusta. SIITÄ siten, että se mahtuu molempia rooleja: ensisijainen tai valmiustilassa.

    SQL> create pfile from spfile;
    File created.

Seuraavaksi haluat muokata pfile valmiustilassa parametrit. Avaa INITTEST. SIITÄ-tiedoston sijainti % ORACLE\_ALOITUS %\\tietokannan. Liitä seuraavaksi seuraavista väittämistä INITTEST.ora-tiedosto. Oman alusta nimeämiskäytäntö. SIITÄ tiedosto on alusta\<YourDatabaseName\>. SIITÄ.

    db_name='TEST'
    db_unique_name='TEST'
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------


Lause edelliset sisältää kolme tärkeät asennusohjelman kohdetta:
-   **LOG_ARCHIVE_CONFIG...:** Voit määrittää tämän lauseella yksilöivä tietokanta tunnukset.
-   **LOG_ARCHIVE_DEST_1...:** Voit määrittää tämän lauseella paikallisen arkiston kansio. On suositeltavaa, Luo uusi kansio tietokantasi tunnistenimiin arkistoinnin tarpeisiin ja määritä paikallisen arkistoon käyttämällä tietosuojatiedoissa erikseen kuin käyttämällä Oracle on oletusarvoinen kansioon % ORACLE_HOME%\database\archive.
-   **LOG_ARCHIVE_DEST_2... LGWR ASYNKRONINEN...:** määrität asynkroninen log writer prosessi (LGWR) tapahtuman Tee tietojen kerääminen ja lähettää sen valmiustilassa kohteisiin. Tässä DB_UNIQUE_NAME määrittää tietokannan kohde valmiustilassa palvelimessa yksilöllinen nimi.

Kun uusi parametri-tiedosto on valmis, sinun täytyy SPFile-tiedoston luominen.

Sammuta ensin tietokanta:

    SQL> shutdown immediate;

    Database closed.

    Database dismounted.

    ORACLE instance shut down.

Suorita seuraavaksi käynnistys nomount komento seuraavasti:

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

Nyt voit luoda spfile:

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';

    File created.

Valitse Sammuta tietokanta:

    SQL> shutdown immediate;

    ORA-01507: database not mounted

Valitse käynnistää käynnistyskomennon avulla:

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

##<a name="create-a-physical-standby-database"></a>Fyysinen valmius-tietokannan luominen
Tässä osassa keskitytään vaiheet, jotka on suoritettava KONE2 fyysinen valmiustilassa tietokannan valmisteleminen.

Ensin sinun täytyy etätyöpöydän KONE2 Azure perinteinen portaalin kautta.

Luo sitten valmiustilassa Server (KONE2) tarvittavat kansiot valmiustilassa tietokannalle, kuten C:\\\<YourLocalFolder\>\\testi. Kun seuraa Tässä opetusohjelmassa, varmista, että kansiorakenne vastaa KONE1 pitämään kaikki tarvittavat tiedostot, kuten ohjaustiedoston, datafiles, redologfiles, udump, bdump ja cdump tiedostoja kansiorakenne. Määritä lisäksi ORACLE\_ALOITUS- ja ORACLE\_KONE2 muuttujat perus ympäristössä. Jos näin ei ole, määrittää niitä ympäristömuuttuja ympäristömuuttujat-valintaikkunassa. Jos haluat käyttää tässä valintaikkunassa, käynnistää **järjestelmän** -apuohjelma kaksoisnapsauttamalla; **Ohjauspaneelin**Järjestelmä-kuvaketta Valitse **Lisäasetukset** -välilehti ja valitse **Ympäristömuuttujat**. Määritä ympäristömuuttujat **Järjestelmämuuttujat**-kohdasta **Uusi** -painiketta. Kun olet määrittänyt ympäristömuuttujat, sinun on olemassa olevan Windows-komentokehote Sulje ja avaa uuden muutokset.

Seuraavaksi toimimalla seuraavasti:

1. Valmiustilassa tietokannan alustus parametri-tiedosto

2. Määritä listener ja tnsnames tukemaan tietokannan perus- ja valmiustilassa tietokoneissa

    1. Määritä listener.ora sekä palvelimissa pitoon molemmat tietokannat tapahtumille

    2. Määritä tnsnames.ora perus- ja valmius-näennäiskoneiden pitoon perus- ja valmiustilassa tietokantojen tapahtumille

    3. Käynnistä kuuntelua ja tarkista tnsping käyttöön sekä näennäiskoneiden sekä palveluihin.

3. Käynnistä valmiustilassa esiintymän nomount-tilaan

4. Käytä RMAN Kloonaa tietokannan ja valmius-tietokannan luominen

5. Fyysinen valmius-tietokannan käyttöönotto hallitun palautus-tilassa

6. Tarkista fyysinen valmiustilassa tietokanta

### <a name="1-prepare-an-initialization-parameter-file-for-standby-database"></a>1. alustus parametri-tiedoston valmiustilassa tietokannan valmisteleminen

Tässä osassa kerrotaan, miten valmiustilassa tietokannan alustus parametri-tiedosto. Voit tehdä tämän kopioi INITTEST. SIITÄ tiedoston koneen 1 KONE2 manuaalisesti. Sinun pitäisi nähdä INITTEST. SIITÄ tiedoston prosenttia ORACLE\_ALOITUS %\\molempien tietokoneiden tietokannan kansioon. Muokkaa sitten INITTEST.ora tiedoston KONE2 se määritetään valmiustilassa roolin alla kuvatulla tavalla:

    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'


    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30


Lause edelliset sisältää kaksi tärkeää asennuksen kohdetta:

-   **\*. LOG_ARCHIVE_DEST_1:** on luotava c:\OracleDatabase\TEST_STBY\archives-kansion manuaalisesti KONE2.
-   **\*. LOG_ARCHIVE_DEST_2:** tämä on valinnainen vaihe. Tämä määrittää, se saattaa kun ensisijainen tietokone on ylläpitoa ja valmiustilassa koneen tulee ensisijaisen tietokannan tarpeen mukaan.

Valitse sinun täytyy Aloita valmiustilassa esiintymää. Anna valmiustilassa tietokantapalvelimen Oracle luominen luomalla Windows-palvelu Windows komentokehotteeseen seuraava komento:

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

**Oradim** -komento luo erillisen Oracle, mutta ei välttämättä käynnisty. Löydät sen c:\\OracleDatabase\\tuotteen\\11.2.0\\dbhome\_1\\BIN directory.

##<a name="configure-the-listener-and-tnsnames-to-support-the-database-on-primary-and-standby-machines"></a>Määritä listener ja tnsnames tukemaan tietokannan perus- ja valmiustilassa tietokoneissa
Ennen kuin luot valmiustilassa tietokannan, sinun täytyy Varmista, että kokoonpanoa perus- ja valmius-tietokantojen puhua toisiinsa. Voit tehdä tämän haluat listener ja TNSNames määrittäminen manuaalisesti tai verkon määritysten apuohjelman NETCA avulla. Tämä on pakollinen tehtävä, kun käytät palautus Manager-apuohjelma (RMAN).

### <a name="configure-listenerora-on-both-servers-to-hold-entries-for-both-databases"></a>Määritä listener.ora sekä palvelimissa pitoon molemmat tietokannat tapahtumille

Etätyöpöytä KONE1 ja Muokkaa alla määritettyyn listener.ora tiedoston. Kun muokkaat tiedostoa listener.ora, varmista aina, että avaavat ja sulkeva Sulje järjestystä samassa sarakkeessa. Löydät listener.ora tiedoston seuraavaan kansioon: c:\\OracleDatabase\\tuotteen\\11.2.0\\dbhome\_1\\verkon\\JÄRJESTELMÄNVALVOJAN\\.

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

Seuraavaksi remote työpöytä KONE2 ja Muokkaa listener.ora tiedostoon seuraavasti: # listener.ora verkon kokoonpanotiedosto: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )


### <a name="configure-tnsnamesora-on-the-primary-and-standby-virtual-machines-to-hold-entries-for-both-primary-and-standby-databases"></a>Määritä tnsnames.ora perus- ja valmius-näennäiskoneiden pitoon perus- ja valmiustilassa tietokantojen tapahtumille

Etätyöpöytä KONE1 ja Muokkaa alla määritettyä tnsnames.ora tiedosto. Löydät tnsnames.ora-tiedoston seuraavaan kansioon: c:\\OracleDatabase\\tuotteen\\11.2.0\\dbhome\_1\\verkon\\JÄRJESTELMÄNVALVOJAN\\.

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

Etätyöpöytä KONE2 ja Muokkaa tnsnames.ora tiedostoon seuraavasti:

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )


### <a name="start-the-listener-and-check-tnsping-on-both-virtual-machines-to-both-services"></a>Käynnistä kuuntelua ja tarkista tnsping käyttöön sekä näennäiskoneiden sekä palveluihin.

Avaa uusi Windows-komentokehote, perus- ja valmiustilassa näennäiskoneiden ja suorita seuraavat lausekkeet:

    C:\Users\DBAdmin>tnsping test

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)


    C:\Users\DBAdmin>tnsping test_stby

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)


##<a name="start-up-the-standby-instance-in-nomount-state"></a>Käynnistä valmiustilassa esiintymän nomount-tilaan
Jos haluat ympäristön määrittäminen tukemaan valmiustilassa tietokannan valmiustilassa virtuaalikoneen (KONE2).

Ensin kopioi Salasanatiedosto ensisijainen tietokoneesta (KONE1) valmiustilassa machine (KONE2) manuaalisesti. Tämä on tarpeen, kuten **sys** Salasanassa on oltava samat molemmissa tietokoneissa.

Avaa Windows-komentokehote KONE2 ja asennuksen ympäristömuuttujat osoittamaan valmius-tietokantaan seuraavasti:

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

Seuraavaksi valmius-tietokannan käyttöönotto nomount-tilaan ja luo sitten spfile.

Tietokannan käynnistäminen:

    SQL>shutdown immediate;

    SQL>startup nomount
    ORACLE instance started.

    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes


##<a name="use-rman-to-clone-the-database-and-to-create-a-standby-database"></a>Käytä RMAN Kloonaa tietokannan ja valmius-tietokannan luominen
Voit ottaa varmuuskopion fyysinen valmiustilassa tietokannan luomiseen ensisijaisen tietokannan palautus Manager-apuohjelma (RMAN).

Etätyöpöytä valmiustilassa virtuaalikoneen (KONE2) ja suorita RMAN-apuohjelman määrittämällä koko yhteyden merkkijono sekä kohteen (ensisijainen tietokanta, KONE1) ja AUXILLARY (valmiustilassa tietokanta, KONE2) esiintymät.

>[AZURE.IMPORTANT] Älä käytä käyttöjärjestelmän todennus, se ei ole tietokantaa valmiustilassa palvelimen tietokoneen vielä.

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY

    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

##<a name="start-the-physical-standby-database-in-managed-recovery-mode"></a>Fyysinen valmius-tietokannan käyttöönotto hallitun palautus-tilassa
Tässä opetusohjelmassa kerrotaan, miten voit luoda fyysinen valmiustilassa tietokannan. Lisätietoja looginen valmiustilassa tietokannan luomiseen on Oracle ohjeissa.

Avaa määrittäminen SQL\*Plus komento kehote ja ota käyttöön tietojen suojaa valmiustilassa virtuaalikoneen tai palvelimelle (KONE2) seuraavasti:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

Kun avaat valmiustilassa tietokannan **käyttöön** tilassa Arkistointiloki toimitus jatkuu ja hallittujen palauttaminen jatkuu log valmius-tietokannan käyttämisen. Tällä varmistetaan, että valmiustilassa tietokannan pysyy ajan tasalla ensisijaisen tietokannan kanssa. Huomaa, että valmiustilassa tietokantaan ei voi olla käytettävissä tänä aikana raportointia varten.

Valmiustilassa tietokannan avaamisen **Vain luku** -tilassa-arkiston Kirjaudu toimitus jatkuu. Mutta hallitun palauttaminen pysäyttää. Tämä aiheuttaa valmiustilassa tietokannan tulee yhä ajan tasalla, kunnes hallitun palauttaminen sen käyttöä jatketaan. Voit käyttää valmiustilassa tietokannan raportointia varten tänä aikana, mutta tietoja ei ilmaise uusimmat muutokset.

On suositeltavaa säilyttää valmiustilassa tietokannan **käyttöön** tilassa ja säilyttää tiedot valmiustilassa tietokannan ajan tasalla Jos on ilmennyt virhe ensisijaisen tietokannan. Kuitenkin voit pitää valmiustilassa tietokannan **Vain luku** -tilassa sovelluksen vaatimukset mukaan raportointia varten. Seuraavat vaiheet kuvaavat ottamisesta käyttöön tietojen suojaa vain luku-tilassa käyttämällä SQL\*Plus:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;


##<a name="verify-the-physical-standby-database"></a>Tarkista fyysinen valmiustilassa tietokanta
Tässä osassa esitellään, miten tarkistamaan suuren käytettävyyden järjestelmänvalvojana.

Avaa määrittäminen SQL\*Plus komentorivi-ikkuna ja tarkista, arkistoida uudelleen log valmius virtuaalikoneen (KONE2):

    SQL> show parameters db_unique_name;

    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY

    SQL> SELECT NAME FROM V$DATABASE

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

Avaa määrittäminen SQL * Plus komentorivi-ikkuna ja vaihda publish logfiles ensisijainen tietokoneeseen (KONE1):

    SQL> alter system switch logfile;
    System altered.

    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

Tarkista arkistoidut Tee loki valmius virtuaalikoneen (KONE2):

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES

    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

Tarkista kaikki välin valmius virtuaalikoneen (KONE2):

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

Toisen tarkistustapa voi olla automaattisesti valmius-tietokantaan ja testaa jos se on mahdollista tuntisesta ensisijainen tietokantaan. Aktivoi valmiustilassa tietokannan olevan muu kuin ensisijaisen tietokannan, käytä seuraavia komentoja:

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

Jos et ole ottanut flashback alkuperäisen ensisijainen tietokannan, että alkuperäiseen ensisijainen tietokannan poistaminen ja luo valmiustilassa tietokantana on suositeltavaa.

On suositeltavaa, että otat ensisijainen flashback tietokanta ja valmius-tietokannat. Automaattisesti tapahtuu, kun ensisijaisen tietokannan voit flashed takaisin ennen vikasietotilaa aika ja muunnetaan nopeasti valmiustilassa tietokannan.

##<a name="additional-resources"></a>Lisäresursseja
[Azure Oracle virtuaalikoneen kuvat](virtual-machines-windows-classic-oracle-images.md)
