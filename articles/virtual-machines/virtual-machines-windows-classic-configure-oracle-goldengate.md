<properties
    pageTitle="Määrittäminen Oracle GoldenGate VMs | Microsoft Azure"
    description="Askel opetusohjelma määrittäminen ja täytäntöön Oracle GoldenGate Azure Windows Server VMs suuri käytettävyys ja tietojen palauttamista varten."
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


#<a name="configuring-oracle-goldengate-for-azure"></a>Azure Oracle GoldenGate määrittäminen


Tässä opetusohjelmassa kerrotaan, miten määrittäminen Oracle GoldenGate Azuren näennäiskoneiden ympäristössä hyvän käytettävyyden ja palauttaminen. Opetusohjelman ohjeessa on [kaksisuuntaisen replikoinnin](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_about_gg.htm) RAC Oracle-tietokantojen ja edellyttää, että molemmat sivustot ovat käytössä.

Oracle GoldenGate tukee tietojen jakelu ja tietojen integrointi. Sen avulla voit määrittää tietojen jakelu ja tietojen synkronointi-ratkaisun Oracle Oracle replikoinnin määrittämisen ja tarjoaa joustavia suuri käytettävyys-ratkaisun. Oracle GoldenGate täydentää Oracle tietojen suojaa sen toistoa ominaisuuksia, jotka käyttöön koko yrityksen tiedot jakelu ja nolla-käyttökatkot päivitykset ja siirrot. Lisätietoja on artikkelissa [Käyttämällä Oracle GoldenGate kanssa Oracle tietojen suojaa](http://docs.oracle.com/cd/E11882_01/server.112/e17157/unplanned.htm).

Oracle GoldenGate sisältää seuraavat pääosaa: Poimi tiedot palopumppu Replicat, jäljet tai purkaa tiedostot, tarkistuspisteet, hallinta ja kerääminen. Kahden sivuston välillä on kaksisuuntaisen replikoinnin, joudut luominen ja kaikkien osien Aloita sekä sivustoissa. Lisätietoja Oracle GoldenGate arkkitehtuuri oppaassa [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html).

Tässä opetusohjelmassa oletetaan, että sinulla on jo teoreettinen ja käytännön tiedon Oracle-tietokantaan suuren käytettävyyden ja palauttaminen käsitteitä sekä [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html). Lisätietoja on artikkelissa [Oracle-web-sivuston](http://www.oracle.com/technetwork/database/features/availability/index.html).

Lisäksi opetusohjelman oletetaan, että seuraavat edellytykset on jo käytössä:

- Olet jo tarkistanut ohjeaiheen [Oracle virtuaalikoneen kuvat - muita huomioon otettavia seikkoja](virtual-machines-windows-classic-oracle-considerations.md) suuren käytettävyyden ja tietojen palauttaminen huomioon otettavia seikkoja-kohdassa. Huomaa, että Azure tukee erillinen Oracle-tietokantaan esiintymät, mutta ei Oracle reaali sovelluksen varausyksiköt (Oracle RAC) tällä hetkellä.

- Olet ladannut Oracle GoldenGate ohjelmiston [Oracle-lataukset](http://www.oracle.com/us/downloads/index.html) -sivustosta. Olet valinnut fuusioenergian tuotteen Pack Oracle – Middleware tietojen integrointi. Tämän jälkeen olet valinnut Oracle GoldenGate Oracle v11.2.1 Media Pack-Microsoft Windowsin x64 (64-bittinen) for Oracle 11 g-tietokannasta. Lataa Oracle GoldenGate V11.2.1.0.3 Oracle seuraavaksi 11g 64-bittinen Windows 2008: n (64-bittinen).

- Olet luonut kaksi näennäiskoneiden (VMs) Oracle Enterprise Edition käyttäminen Windows Server Azure-tietokannassa. Varmista, näennäiskoneiden on [sama pilvipalvelussa](virtual-machines-linux-load-balance.md) ja samassa [VPN](https://azure.microsoft.com/documentation/services/virtual-network/) , jotta he pääsevät toistensa päälle pysyvä yksityinen IP-osoite.

- Olet määrittänyt virtuaalikoneen nimet sivuston a "MachineGG1" ja "MachineGG2" sivuston b Azure perinteinen portaalissa osoitteessa.

- Olet luonut testi tietokantojen "TestGG1" sivuston A ja "TestGG2" sivuston b.

- Kirjaudut Windows server Järjestelmänvalvojat-ryhmän jäsen tai **ORA_DBA** -ryhmän jäsen.

Tässä opetusohjelmassa menettelet seuraavasti:

1. Tietokannan sivustosi A ja B sivuston asetukset  

    1. Suorittaa aloitustiedot lataaminen

2. Sivuston A ja B sivuston valmisteleminen tietokannan replikoiminen

3. Luo tarvittavat objektit DDL replikoinnin tuki

4. GoldenGate hallinnan määrittämistä A ja B-sivustot

5. Luo Pura ryhmittely- ja tietojen pumppu prosessit sivustosi A ja B sivuston

    1. Luo sivusto ote ja tietojen pumppu

    2. Sivuston b GoldenGate tarkistuspiste taulukon luominen

    3. Lisää REPLICAT B-sivustossa

    4. Pura ja tietojen pumppu prosessien luominen sivuston b

    5. Luoda GoldenGate tarkistuspiste taulukon sivusto

    6. Lisää REPLICAT A-sivustossa

    7. Lisää sivuston A ja B sivuston trandata

    8. Pura ja tietojen pumppu käynnistäminen sivusto

    9. Käynnistä ote ja tietojen pumppu sivuston b

    10. Aloita REPLICAT sivusto

    11. Aloita REPLICAT sivuston B

6. Tarkista kaksisuuntaisen replikoinnin prosessi

>[AZURE.IMPORTANT] Tässä opetusohjelmassa on määritetty ja verrataan ohjelmiston asetukset ovat seuraavat:
>
>|                        | **Sivuston tietokannan**              | **Sivuston B-tietokanta**              |
>|------------------------|----------------------------------|----------------------------------|
>| **Oracle-versio**     | Oracle11g Release 2 – (11.2.0.1) | Oracle11g Release 2 – (11.2.0.1) |
>| **Tietokoneen nimi**       | MachineGG1                       | MachineGG2                       |
>| **Käyttöjärjestelmä**   | Windows 2008 R2                  | Windows 2008 R2                  |
>| **Oracle SUOJAUSTUNNUS**         | TESTGG1                          | TESTGG2                          |
>| **Replikoinnin rakenne** | JUHANI                            | JUHANI                            |

Myöhemmissä versioissa Oracle-tietokantaan ja Oracle GoldenGate voi olla joitakin muita muutoksia, jotka haluat ottaa käyttöön. Uusin versio tiettyjä tietoja ohjeissa [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) ja [Oracle-tietokantaan](http://www.oracle.com/us/corporate/features/database-12c/index.html) Oracle-sivustossa. Esimerkiksi, Vapauta 11.2.0.4 lähdetietokannan tai uudempaa versiota DDL sieppaus suoritetaan asynkronisesti logmining-palvelin ja edellyttää, että ei erityistä käynnistimien, taulukoita tai muihin tietokannan objekteihin on asennettu. Oracle GoldenGate päivityksiä voi käyttää ilman lopettaminen käyttäjän sovellukset. DDL käynnistin ja tukevat objektit tarvitaan, kun Pura on integroitu tilassa Oracle-11g lähteen tietokannan, joka on aiempi kuin 11.2.0.4 kanssa. Yksityiskohtaisia ohjeita on artikkelissa [asentaminen ja määrittäminen Oracle GoldenGate Oracle-tietokantaan](http://docs.oracle.com/goldengate/1212/gg-winux/GIORA.pdf).

##<a name="1-setup-database-on-site-a-and-site-b"></a>1. tietokannan sivustosi A ja B sivuston asetukset
Tässä osassa kerrotaan, miten voit suorittaa tietokannan edellytykset sivusto ja sivuston B. Kaikki tämän osan vaiheet on suoritettava sekä sivustojen: sivusto- ja sivuston B.

Ensimmäinen, remote työpöydän sivuston A ja B sivuston Azure perinteinen portaalin kautta. Avaa Windowsin komentorivi-ikkuna ja Luo Oracle GoldenGate asennustiedostot kotiverkon kansion:

    mkdir C:\OracleGG

Valitse unzip ja asenna Oracle GoldenGate ohjelmiston tähän kansioon. Kun tässä vaiheessa voit aloittaa GoldenGate ohjelmiston komento kääntäjän (GGSCI) suorittamalla seuraava komento:

    C:\OracleGG\.\ggsci

Voit suorittaa useita komentoja, joka määrittää [GGSCI](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_gettingstarted.htm) , hallinta ja näytön Oracle GoldenGate.

Seuraavaksi suorittamalla seuraavan komennon kaikki alikansiot luominen asennuspaketin:

    GGSCI (Hostname) 1> CREATE SUBDIRS

Suorita Lopeta GGSCI komentokehotteeseen seuraava komento:

    GGSCI (Hostname) 1> EXIT

Valitse sinun täytyy tietokannan käyttäjän, jota käytetään Oracle GoldenGate johtajan, ote ja replikoinnin prosessien luominen. Huomaa, että voit luoda yksittäisten käyttäjien kunkin prosessin tai määrittäminen vain yksi yleisiä käyttäjä. Tässä opetusohjelmassa nyt luoda yksittäisen käyttäjän, jota kutsutaan nimellä ggate. Microsoft myöntää kyseisen käyttäjän tarvittavat oikeudet. Huomaa, että on suoritettava seuraavat toimenpiteet sivusto ja sivuston B.

Avaa määrittäminen SQL\*Plus komentoikkunassa sivustosi A ja B sivuston ja tietokannan järjestelmänvalvojan oikeudet käyttää **SYSDBA**, kuten:

    Enter username: / as sysdba

Ja:

    SQL> create tablespace ggs_data   datafile 'c:\OracleDatabase\oradata\<DBNAME>\<DBNAME>ggs_data01.dbf' size 200m;
    SQL> create user ggate identified by ggate default tablespace ggs_data  temporary tablespace temp;
          grant connect, resource to ggate;
          grant select any dictionary, select any table to ggate;
          grant create table to ggate;
          grant flashback any table to ggate;
          grant execute on dbms_flashback to ggate;
          grant execute on utl_file to ggate;
          grant create any table to ggate;
          grant insert any table to ggate;
          grant update any table to ggate;
          grant delete any table to ggate;
          grant drop any table to ggate;

Etsi seuraavaksi alusta\<DatabaseSID\>. SIITÄ tiedoston prosenttia ORACLE\_ALOITUS %\\tietokannan kansion sivustossa A ja B sivuston ja liitä seuraavat tietokannan parametrit INITTEST.ora:

    UNDO\_MANAGEMENT=AUTO
    UNDO\_RETENTION=86400

Katso täysi luettelo kaikki Oracle GoldenGate GGSCI komennot [Reference for Oracle GoldenGate for Windows](http://docs.oracle.com/goldengate/1212/gg-winux/GWURF/ggsci_commands.htm).

### <a name="perform-initial-data-load"></a>Suorittaa aloitustiedot lataaminen

Voit suorittaa aloitustiedot kuormituksen tietokannan seuraamalla useilla eri tavoilla. Esimerkiksi voi käyttää [Oracle GoldenGate suoraan kuormituksen](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_initsync.htm) tai säännöllisesti vieminen ja tuominen apuohjelmia taulukkotietojen tuominen sivusto sivusto b.

Tässä opetusohjelmassa esitellään osoittamaan Oracle GoldenGate replikoinnin prosessin taulukon luominen käyttämällä seuraavia komentoja sivuston A ja B sivuston.

Avaa ensin SQL\*Plus komento ikkunan ja suorita seuraava komento varastotaulukon luominen sivuston A ja B sivuston tietokantoja:

    create table scott.inventory
    (prod_id number,
    prod_category varchar2(20),
    qty_in_stock number,
    last_dml timestamp default systimestamp);

Lisää seuraavaksi sivuston A ja B sivuston tietokantojen juuri luomasi taulukon rajoituksen:

    alter table scott.inventory add constraint pk_inventory primary key (prod_id) ;

Myönnä sitten uuden käyttäjän ggate sivusto ja sivuston B: varastotaulukon kaikki jakamisoikeudet

    grant all on scott.inventory to ggate;

Seuraavaksi luominen ja ottaminen käyttöön tietokantaa käynnistimen, INVENTORY_CDR_TRG, varmista, että uuden taulukon kaikki tapahtumat tallennetaan, jos käyttäjä ei ole ggate juuri luomasi taulukon. Tämä toiminto sivusto ja sivuston B.

    CREATE OR REPLACE TRIGGER INVENTORY_CDR_TRG
    BEFORE UPDATE
    ON SCOTT.INVENTORY
    REFERENCING NEW AS New OLD AS Old
    FOR EACH ROW
    BEGIN
    IF SYS_CONTEXT ('USERENV', 'SESSION_USER') != 'GGATE'
    THEN
    :NEW.LAST_DML := SYSTIMESTAMP;
    END IF;
    END;
    /


##<a name="2-prepare-site-a-and-site-b-for-database-replication"></a>2. valmisteleminen sivuston A ja B sivuston tietokannan replikoiminen
Tässä osassa kerrotaan, kuinka sivuston A ja B sivuston valmisteleminen tietokannan replikoiminen. Kaikki tämän osan vaiheet on suoritettava sekä sivustojen: sivusto- ja sivuston B.

Ensimmäinen, remote työpöydän sivuston A ja B sivuston Azure perinteinen portaalin kautta. Vaihda tietokannan archivelog tilaan käyttäen SQL * Plus komentorivi-ikkuna:

    sql>shutdown immediate
    sql>startup mount
    sql>alter database archivelog;
    sql>alter database open;


Ota sitten mahdollisimman vähän [täydentävät kirjaaminen](http://docs.oracle.com/cd/E11882_01/server.112/e22490/logminer.htm) seuraavasti:

    SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

Seuraavaksi tukemaan DDL (data definition language) replikoinnin tietokannan valmisteleminen:

    SQL> alter system set recyclebin=off scope=spfile;

Valitse, sulkeminen ja Käynnistä tietokannan:

    sql>shutdown immediate
    sql>startup


##<a name="3-create-all-necessary-objects-to-support-ddl-replication"></a>3. kaikki tarvittavat objektit tukemaan DDL replikoinnin luominen
Tässä osassa on luettelo komentosarjoja, jotka sinun on käytettävä kaikki tarvittavat objektien tukemaan DDL replikoinnin luomiseen. Sinun on suoritettava tässä kohdassa sivuston A ja b sivuston komentosarjat

Avaa Windowsin komentorivi-ikkuna ja siirry Oracle GoldenGate kansioon, kuten C:\\OracleGG. Käynnistä SQL\*Plus komentokehote ja tietokannan järjestelmänvalvojan oikeudet, kuten **SYSDBA** käyttäminen sivusto ja sivuston B.

Suorita seuraavat komentosarjat:

    SQL> @marker_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @ddl_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @role_setup.sql
    Enter GoldenGate schema name: ggate
    SQL> grant ggs_ggsuser_role to ggate;
     Grant succeeded.
     SQL> @ddl_enable
     Trigger altered.
     SQL> @ddl_pin ggate


Oracle GoldenGate työkalu edellyttää taulukon tason kirjautuminen DDL (data definition language)-tuen. Minkä vuoksi, ota käyttöön täydentävät kirjaaminen taulukon tasolla Lisää TRANDATA-komennon avulla. Avaa Oracle GoldenGate kääntäjän-komentoikkunassa tietokantaan, kirjaudu sisään ja sitten Lisää TRANDATA-komennolla:

    GGSCI 5> DBLOGIN USERID ggate, PASSWORD ggate

    GGSCI(Hostname) 6> add trandata scott.inventory

##<a name="4-configure-goldengate-manager-on-site-a-and-site-b"></a>4. määrittämistä GoldenGate hallinnan sivuston A ja B
Oracle GoldenGate hallinnan suorittaa toimintoja, kuten käynnistäminen muiden GoldenGate prosesseja, kirjausketju lokitiedoston hallinta ja raportointi.

Sinun on määritettävä Oracle GoldenGate hallinnan-prosessi sivusto ja sivuston B. Voit tehdä tämän suorittamalla seuraavat vaiheet A sivuston ja sivuston B.

Avaa Windows komento ikkunan ja aloittaa Oracle GoldenGate komento kääntäjän:

    cd C:\OracleGG\
    c:\OracleGG>ggsci
    Oracle GoldenGate Command Interpreter for Oracle
    Version 11.2.1.0.3 14400833 OGGCORE_11.2.1.0.3_PLATFORMS_120823.1258
    Windows x64 (optimized), Oracle 11g on Aug 23 2012 16:50:36
    Copyright (C) 1995, 2012, Oracle and/or its affiliates. All rights reserved.


Kirjaa GGSCI istunnon tietokantaan, niin, että voit suorittaa komennot, jotka vaikuttavat tietokanta:

    GGSCI (HostName) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Näytä tila ja viive johtajan, ote ja Replicat prosessien järjestelmään (tarvittaessa):

    GGSCI (HostName) 2> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Avaa Muokkaa parametrit-komennon avulla parametri-tiedosto ja liitä sitten seuraavat tiedot:

    GGSCI (HostName) 3> edit params mgr
    PORT 7809
    USERID ggate, PASSWORD ggate
    PURGEOLDEXTRACTS  C:\OracleGG\dirdat\ex, USECHECKPOINTS

Näytä tila ja viive johtajan, ote ja Replicat prosessien järjestelmään (tarvittaessa):

    GGSCI (HostName) 46> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Kirjaa GGSCI istunnon tietokantaan, niin, että voit suorittaa komennot, jotka vaikuttavat tietokanta:

    GGSCI (HostName) 47> dblogin USERID ggate, PASSWORD ggate

    Successfully logged into database.

Aloita hallinta:

    GGSCI (HostName) 48> start manager
    Manager started.

##<a name="5-create-extract-group-and-data-pump-processes-on-site-a-and-site-b"></a>5. luominen Pura ryhmittely- ja tietojen pumppu prosessit sivustosi A ja B sivuston

###<a name="create-extract-and-data-pump-processes-on-site-a"></a>Luo sivusto ote ja tietojen pumppu

Sinun täytyy ote ja tietojen pumppu prosessien luominen sivusto ja sivuston B. etätyöpöydän kautta sivustossa A ja B sivuston Azure perinteinen portaalin kautta. Avaa GGSCI kääntäjän komentoikkunassa ylöspäin. Suorita seuraavat komennot sivuston A:

    GGSCI (MachineGG1) 14> add extract ext1 tranlog begin now
    EXTRACT added.
    GGSCI (MachineGG1) 4> add exttrail C:\OracleGG\dirdat\lt, extract ext1
    EXTTRAIL added.
    GGSCI (MachineGG1) 16> add extract dpump1 exttrailsource C:\OracleGG\dirdat\aa
    EXTRACT added.
    GGSCI (MachineGG1) 17> add rmttrail C:\OracleGG\dirdat\ab extract dpump1
    RMTTRAIL added.

Avaa Muokkaa parametrit-komennon avulla parametri-tiedosto ja liitä sitten seuraavat tiedot: GGSCI (MachineGG1) 18 > Muokkaa parametrit ext1 PURA ext1 KÄYTTÄJÄTUNNUS ggate, salasana ggate EXTTRAIL C:\OracleGG\dirdat\aa TRANLOGOPTIONS EXCLUDEUSER ggate taulukon scott.inventory GETBEFORECOLS (edelleen päivitys KEYINCLUDING (prod_category, qty_in_stock, last_dml), edelleen poistaa KEYINCLUDING (prod_category, qty_in_stock, last_dml));

Avaa Muokkaa parametrit-komennon avulla parametri-tiedosto ja liitä sitten seuraavat tiedot:

    GGSCI (MachineGG1) 15> edit params dpump1
    EXTRACT dpump1
     USERID ggate, PASSWORD ggate
     RMTHOST ActiveGG2orcldb, MGRPORT 7809, TCPBUFSIZE 100000
     RMTTRAIL C:\OracleGG\dirdat\ab
     PASSTHRU
     TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-b"></a>Sivuston b GoldenGate tarkistuspiste taulukon luominen

Seuraavaksi sinun on lisättävä tarkistuspiste taulukon sivuston b. Tällöin sinun on kääntäjän GoldenGate komentorivi-ikkuna avautuu ja suorittaa:

    C:\OracleGG\ggsci
    GGSCI (MachineGG2) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Ja lisää sitten tietokantaan, missä ggate omistaja tarkistuspiste-taulukko:

    GGSCI (MachineGG2) 2> ADD CHECKPOINTTABLE ggate.checkpointtable
    Successfully created checkpoint table ggate.checkpointtable.

Lisää valintaruutu kohdassa taulukon nimi palvelimessa kohde, joka on sivuston B Tässä vaiheessa GLOBALS-tiedosto. Sivuston B: GLOBALS-tiedoston muokkaaminen

    GGSCI (MachineGG2) 1> EDIT PARAMS ./GLOBALS

Lisää sitten CHECKPOINTTABLE-parametrin GLOBALS aiemmin luotu tiedosto:

    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Lopullinen vaiheessa Tallenna ja sulje GLOBALS parametri-tiedosto.


###<a name="add-replicat-on-site-b"></a>Lisää REPLICAT B-sivustossa
Tässä osassa kerrotaan, miten voit lisätä REPLICAT prosessin "REP2" sivuston b.

Lisää REPLICAT-komennon avulla voit luoda Replicat ryhmän sivuston B:

    GGSCI (MachineGG2) 37> add replicat rep2 exttrail C:\OracleGG\dirdatab, checkpointtable ggate.checkpointtable

Avaa Muokkaa parametrit-komennon avulla parametri-tiedosto ja liitä sitten seuraavat tiedot:

    GGSCI (MachineGG2) 10> edit params rep2
    REPLICAT rep2
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append,megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

###<a name="create-extract-and-data-pump-processes-on-site-b"></a>Pura ja tietojen pumppu prosessien luominen sivuston b

Tässä osassa kuvataan, miten voit luoda uuden Pura prosessin "EXT2" ja uusi tietojen pumpun prosessi "DPUMP2" sivuston B:

    GGSCI (MachineGG2) 3> add extract ext2 tranlog begin now
     EXTRACT added.
    GGSCI (MachineGG2) 4> add exttrail C:\OracleGG\dirdat\ac extract ext2
     EXTTRAIL added.
    GGSCI (MachineGG2) 5> add extract dpump2 exttrailsource C:\OracleGG\dirdat\ac
     EXTRACT added.
    GGSCI (MachineGG2) 6> add rmttrail C:\OracleGG\dirdat\ad extract dpump2
     RMTTRAIL added.

Avaa Muokkaa parametrit-komennon avulla parametri-tiedosto ja liitä sitten seuraavat tiedot:

    GGSCI (MachineGG2) 31> edit params ext2
    EXTRACT ext2
    USERID ggate, PASSWORD ggate
    EXTTRAIL C:\OracleGG\dirdat\ac
    TRANLOGOPTIONS EXCLUDEUSER ggate
    TABLE scott.inventory,
    GETBEFORECOLS (
    ON UPDATE KEYINCLUDING (prod_category,qty_in_stock, last_dml),
    ON DELETE KEYINCLUDING (prod_category,qty_in_stock, last_dml));

Avaa Muokkaa parametrit-komennon avulla parametri-tiedosto ja liitä sitten seuraavat tiedot:

    GGSCI (MachineGG2) 32> edit params dpump2
    EXTRACT dpump2
    USERID ggate, PASSWORD ggate
    RMTHOST MachineGG1, MGRPORT 7809, TCPBUFSIZE 100000
    RMTTRAIL C:\OracleGG\dirdat\ad
    PASSTHRU
    TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-a"></a>Luoda GoldenGate tarkistuspiste taulukon sivusto

Avaa Oracle GoldenGate kääntäjän komentoikkunassa ja tarkistuspiste taulukon luominen:

    GGSCI (MachineGG1) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

    GGSCI (MachineGG1) 2> ADD CHECKPOINTTABLE ggate.checkpointtable

    Successfully created checkpoint table ggate.checkpointtable.

Sinun on myös lisättävä valintaruutu kohdassa taulukon nimeä, sivuston A. GLOBALS-tiedostoon

Avaa Oracle GoldenGate kääntäjän komentoikkunassa ja sivuston A: GLOBALS-tiedoston muokkaaminen

    GGSCI (MachineGG1) 1> EDIT PARAMS ./GLOBALS
    Add the CHECKPOINTTABLE parameter to the existing GLOBALS file as follows:
    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Tallenna ja sulje GLOBALS parametri-tiedosto.

###<a name="add-replicat-on-site-a"></a>Lisää REPLICAT A-sivustossa

Tässä osassa kerrotaan, miten voit lisätä REPLICAT prosessin "REP1" a-sivustossa

Seuraava komento luo Replicat-ryhmän rep1, jossa nimi kirjausketju ja liittyvän checkpointtable.

    GGSCI (MachineGG1) 21> add replicat rep1 exttrail C:\OracleGG\dirdat\ad,checkpointtable ggate.checkpointtable
     REPLICAT added.

Avaa Muokkaa parametrit-komennon avulla parametri-tiedosto ja liitä sitten seuraavat tiedot:

    GGSCI (MachineGG1) 10> edit params rep1
    REPLICAT rep1
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append, megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

### <a name="add-trandata-on-site-a-and-site-b"></a>Lisää sivuston A ja B sivuston trandata

Ota käyttöön täydentävät kirjaaminen taulukon tasolla Lisää TRANDATA-komennon avulla. Avaa Oracle GoldenGate kääntäjän-komentoikkunassa tietokantaan, kirjaudu sisään ja suorita sitten Lisää TRANDATA-komento.

Etätyöpöytä MachineGG1, voit avata Oracle GoldenGate komento kääntäjän ja suorita:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Etätyöpöytä MachineGG2, voit avata Oracle GoldenGate komento kääntäjän ja suorita:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.

Näytä tilatietoja taulukon tason täydentävät kirjaaminen:

    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="add-trandata-on-site-a-and-site-b"></a>Lisää sivuston A ja B sivuston trandata

Ota käyttöön täydentävät kirjaaminen taulukon tasolla Lisää TRANDATA-komennon avulla. Avaa Oracle GoldenGate kääntäjän-komentoikkunassa tietokantaan, kirjaudu sisään ja suorita sitten Lisää TRANDATA-komento.

Etätyöpöytä MachineGG1, voit avata Oracle GoldenGate komento kääntäjän ja suorita:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Etätyöpöytä MachineGG2, voit avata Oracle GoldenGate komento kääntäjän ja suorita:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.
    Display information about the state of table-level supplemental logging:
    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="start-extract-and-data-pump-processes-on-site-a"></a>Pura ja tietojen pumppu käynnistäminen sivusto

Pura prosessin ext1 käynnistäminen sivuston A:

    GGSCI (MachineGG1) 31> start extract ext1
    Sending START request to MANAGER …
    EXTRACT EXT1 starting

Sivuston A: tiedot pumpun prosessin dpump1 käynnistäminen

    GGSCI (MachineGG1) 23> start extract dpump1
    Sending START request to MANAGER …
    EXTRACT DPUMP1 starting
Näyttää tietoja Pura ryhmän ext1: GGSCI (MachineGG1) 32 > tiedot Pura ext1 PURA EXT1 viimeisen aloittaminen 2013-11-25 08:03 tila käynnissä tarkistuspiste viive 00:00:00 (päivitetty 00:00:02 sitten) lokin luku tarkistuspiste Oracle uudelleen lokit 2013-11-25 08:03:18 Seqno 6, RBA 3230720 SCN 0.1074371 (1074371)

Näytä tila ja viive johtajan, ote ja Replicat prosessien järjestelmään (tarvittaessa):

    GGSCI (MachineGG1) 16> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP1      00:00:00      00:46:33
    EXTRACT     RUNNING     EXT1        00:00:00      00:00:04

###<a name="start-extract-and-data-pump-processes-on-site-b"></a>Käynnistä ote ja tietojen pumppu sivuston b

Pura prosessin ext2 käynnistäminen sivuston B:

    GGSCI (MachineGG2) 22> start extract ext2
    Sending START request to MANAGER …
    EXTRACT EXT2 starting

Sivuston B: tiedot pumpun prosessin dpump2 käynnistäminen

    GGSCI (MachineGG2) 23> start extract dpump2
    Sending START request to MANAGER …
    EXTRACT DPUMP2 starting

Näytä tila ja viive johtajan, ote ja Replicat prosessien järjestelmään (tarvittaessa):

    GGSCI (ActiveGG2orcldb) 6> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP2      00:00:00      136:13:33
    EXTRACT     RUNNING     EXT2        00:00:00      00:00:04

### <a name="start-replicat-process-on-site-a"></a>Aloita REPLICAT sivusto

Tässä osassa kuvataan Aloita REPLICAT "REP1" sivustossa A.

Aloita sivuston A:-Replicat

    GGSCI (MachineGG1) 38> start replicat rep1
    Sending START request to MANAGER …
    REPLICAT REP1 starting

Näytä Replicat ryhmän tila:

    GGSCI (MachineGG1) 39> status replicat rep1
     REPLICAT REP1: RUNNING

###<a name="start-replicat-process-on-site-b"></a>Aloita REPLICAT sivuston B

Tässä osassa kuvataan Aloita REPLICAT "REP2" sivuston b.

Aloita sivuston B:-Replicat

    GGSCI (MachineGG2) 26> start replicat rep2
    Sending START request to MANAGER …
    REPLICAT REP2 starting

Näytä Replicat ryhmän tila:

    GGSCI (MachineGG2) 27> status replicat rep2
    REPLICAT REP2: RUNNING

##<a name="6-verify-the-bi-directional-replication-process"></a>6. Tarkista kaksisuuntaisen replikoinnin prosessi

Tarkista Oracle GoldenGate määritykset, Lisää rivi osoitteessa sivuston A. etätyöpöydän sivuston A. Avaa määrittäminen SQL-tietokantaan * Plus komentorivi-ikkuna ja suorita: SQL > v$ tietokannasta, valitse nimi

    NAME
    ———
    TESTGG

    SQL> insert into inventory values  (100,’TV’,100,sysdate);

    1 row created.

    SQL> commit;

    Commit complete.

Valitse Tarkista, jos kyseisen rivin replikoida sivuston b. Voit tehdä tämän sivuston B. Avaa etätyöpöydän määrittäminen SQL Plus-ikkuna ja suorittaa:

    SQL> select name from v$database;

    NAME
    ———
    TESTGG

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13

Uuden tietueen lisääminen sivuston B:

    SQL> insert into inventory  values  (101,’DVD’,10,sysdate);
    1 row created.

    SQL> commit;

    Commit complete.

Etätyöpöytä sivusto ja tarkista, onko replikointi on tapahtunut:

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13
    101 DVD 10 22-MAR-13

##<a name="additional-resources"></a>Lisäresursseja
[Azure Oracle virtuaalikoneen kuvat](virtual-machines-linux-classic-oracle-images.md)
