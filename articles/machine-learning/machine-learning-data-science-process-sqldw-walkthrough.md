<properties
    pageTitle="Ryhmän tietojen tiede prosessin käytössä: SQL-tietovarasto avulla | Microsoft Azure"
    description="Prosessin kehittyneen analyysin ja tekniikka-toiminto"  
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/24/2016"
    ms.author="bradsev;hangzh;weig"/>


# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>Ryhmän tietojen tiede prosessin käytössä: SQL-tietovarasto käyttäminen

Tässä opetusohjelmassa on seuraamalla ja käyttöönottoon konepohjaisten oppimistekniikoiden malliin käyttämällä SQL-tietovarasto (SQL-DW) kautta, yleisesti saatavilla tietojoukko-- [Kokousesitelmän taksin Trips](http://www.andresmh.com/nyctaxitrips/) tietojoukko. Binaarinen luokitus-mallin rakennettava ennustaa riippumatta siitä, onko Vihje maksetaan matkan ja mallien multiclass luokitukseen ja regression myös käsitellään, ennustetaan jakauman Vihje summat, joka on maksettu.

Menettelyn seuraa [Ryhmän tietojen tiede prosessi (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) -työnkulun. Emme esittämään tietoja tiede-ympäristön määrittäminen lataa tiedot SQL DW ja käyttämisestä siitä, miten voit tutkia tietoja ja lähdekoodiksi SQL DW tai IPython muistikirjan ominaisuudet-malliin. Olemme sitten näyttää, miten voivat laatia ja kehittää malli, joka sisältää Azure koneen Learning.


## <a name="dataset"></a>Kokousesitelmän taksin Trips-tietojoukko

Kokousesitelmän taksin työmatkan tiedot koostuu noin 20 gt: n pakattu CSV-tiedostoja (noin 48 gt puretaan), jokaisen matkan tallenteen 173 miljoonaa yksittäisiä trips ja kuljetusmaksut maksettu. Työmatkan kunkin tietueen sisältää nouto ja latauskirjaston sijainnit ja ajat, anonymized hakkeroida (ohjaimen) käyttöoikeuden numero ja medallion (taksin on yksilöllinen tunnus)-numero. Tietoja kattaa kaikki trips vuoden 2013 ja annetaan seuraavat kaksi tietojoukkoja jokaiselle kuukaudelle:

1. **Trip_data.csv** -tiedosto sisältää työmatkan tiedot, kuten matkustajien, nouto ja dropoff pistettä, työmatkan keston ja työmatkan pituus määrän. Tässä on muutama Esimerkki tietue:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. **Trip_fare.csv** -tiedosto sisältää tietoja maksettu kunkin työmatkan, kuten maksutyyppi, maksun summa, Lisämaksu ja verojen, vihjeet ja tietullit, maksun ja yhteensä maksetun summan. Tässä on muutama Esimerkki tietue:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

**Yksilöivä** liittyä työmatkan\_tiedot ja työmatkan\_maksun koostuu seuraavat kolme kenttää:

- medallion,
- yhteyden Bluetooth\_käyttöoikeuden ja
- nouto\_päivämäärä ja aika.

## <a name="mltasks"></a>Osoite kolmenlaisia tekstinsyöttö tehtävät

Olemme muodostetaan kolme tekstinsyöttö ongelmien perusteella *Vihje\_summa* esitä kolmenlaisia mallinnus tehtävät:

1. **Binaarinen luokitus**: ennustaa onko Vihje maksettuun matkan, eli *Vihje\_summa* , joka on suurempi kuin 0 euroa on positiivinen esimerkissä, mutta *Vihje\_summa* $ 0 on negatiivinen esimerkki.

2. **Multiclass luokitus**: ennustaa Vihje matkan maksettu solualue. Jaetaan *Vihje\_summa* viisi palkkien tai luokat:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regressiosuoran tehtävän**: ennustaa Vihje matkan maksettu määrä.  


## <a name="setup"></a>Saat kehittyneen analyysin Azure tietojen tiede-ympäristön määrittäminen

Voit määrittää Azure tietojen tiede-ympäristön toimimalla seuraavasti.

**Oman Azure-blob-tallennustilan tilin luominen**

- Kun valmistelu oman Azure-blob-säiliö, mihin Azure-blob-tallennustilan geo-sijaintiin tai mahdollisimman, **Etelä keskitetyn US**, joka on lähellä Kokousesitelmän taksin tietojen tallennussijainti. Tiedot kopioidaan käyttämällä AzCopy julkisen blob storage säilöstä säilöön tallennustilan-tilisi. Mitä lähempänä Azure-blob-säiliö on Etelä keskitetyn meille, sitä nopeammin (vaihe 4) tehtävä suoritettu.
- Voit luoda oman Azure-tallennustilan tilin seuraa artikkelin [tietoja Azure-tallennustilan tilit](../storage/storage-create-storage-account.md)-palvelussa. Muista tehdä muistiinpanoja seuraavat tallennustilan tilin tunnistetiedot arvot niitä tarvittaessa myöhemmin-tämän vaiheittaisen kuvauksen suorittamiseksi.

  - **Tallennustilan tilin nimi**
  - **Tallennustilan tilin avain**
  - **Säilön nimi** (joka haluat tiedot tallennetaan Azure-blob-säiliö)

**Valmistele Azure SQL-DW esiintymä.**
Noudata [Luo SQL-tietovarasto](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) valmistelu SQL-tietovarasto esiintymän documentation. Varmista, että muotoilun laittaa seuraava SQL-tietovarasto tunnistetiedot, joilla käytetään myöhemmissä vaiheissa.

  - **Palvelimen nimi**: <server Name>. database.windows.net
  - **SQLDW (tietokanta) nimi**
  - **Käyttäjänimi**
  - **Salasana**

**Asenna Visual Studio 2015 ja SQL Server Data Tools.** Katso ohjeet [Asenna Visual Studio 2015 ja/tai SSDT (SQL Server Data Tools) SQL-tietovarasto varten](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Muodosta yhteys Azure SQL-DW Visual Studiossa.** Katso ohjeet vaiheet 1 ja 2 [Azure SQL-tietovarasto Visual Studiossa Yhdistä](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

>[AZURE.NOTE] Suorittaa SQL-kysely tietokanta on luotu SQL-tietovarasto (sijaan vaiheen 3 Yhdistä, aiheen kysely) voit **luoda perusmuodon-näppäintä**.

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Luo Azure tilauksen Azure koneen Learning-työtila.** Katso ohjeet [Luo Azure koneen Learning työtila](machine-learning-create-workspace.md).

## <a name="getdata"></a>Lataa tiedot SQL-tietovarasto

Avaa Windows PowerShell-komentoa konsoli. Suorita seuraavat PowerShell komentoja, jotka lataavat Esimerkki SQL komentosarjan tiedostot, jotka on jakaa kanssasi Github paikallisessa kansiossa, jonka määrität parametria *- DestDir*. Voit muuttaa parametrin arvo *-DestDir* paikallisen-kansioon. Jos *- DestDir* ei ole, se luodaan PowerShell-komentosarjaa.

>[AZURE.NOTE] Voit joutua muuttamaan **Suorita järjestelmänvalvojana** , suoritettaessa seuraavaa PowerShell-komentosarjaa, jos *DestDir* -kansio on järjestelmänvalvojan oikeudet, jos haluat luoda tai kirjoittaa siihen.

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Onnistuneen suorittamisen jälkeen työskentelyä nykyisen hakemiston muuttuu *- DestDir*. Sinun pitäisi nähdä näytössä, kuten alla:

![][19]

Oman *-DestDir*, valitse Suorita järjestelmänvalvojan oikeuksin seuraavaa PowerShell-komentosarjaa:

    ./SQLDW_Data_Import.ps1

PowerShell-komentosarjojen suorittamisen ensimmäistä kertaa, sinua pyydetään kirjaimilla Azure SQL-DW ja Azure-blob-tallennustilan tilin tiedot. Kun tämä PowerShell-komentosarja on valmis käynnissä ensimmäistä kertaa, tunnistetiedot voit syöte kirjoitettu määritystiedoston SQLDW.conf esitä toimimasta hakemistossa. Tämä PowerShell-komentosarjatiedosto tuleva Suorita on asetus, jos haluat lukea kaikki tarvittavat parametrit määritysten tästä tiedostosta. Jos haluat muuttaa joitakin parametreja, voit valita syöttämistä yhteydessä kehote näytössä parametrit poistamalla tämän kokoonpanotiedosto ja kirjoittaminen parametrien arvot kehottaessa tai muuttaa parametriarvot muokkaamalla SQLDW.conf tiedosto *- DestDir* hakemistossa.

>[AZURE.NOTE] Siten vältät rakenteen Nimiristiriitojen verrattuna kaavaan, jotka ovat jo Azure-SQL DW luettaessa parametrit suoraan SQLDW.conf-tiedostosta, 3-numeroinen satunnaisluvun lisätään rakenteen nimeä SQLDW.conf tiedostosta jokaisen rakenteen oletusnimi. PowerShell-komentosarjaa voi pyytää mallin nimi: nimeä voi määrittää käyttäjän harkintasi.

**PowerShell-komentosarjaa** tämän tiedoston suorittaa seuraavia tehtäviä:

- **Lataa ja asentaa AzCopy**, jos AzCopy ei ole vielä asennettu

        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
            Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }

- **Kopioi tiedot yksityinen blob storage tiliisi** julkisen Blob-objektien kanssa AzCopy

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"


- **Lataa tiedot käyttämällä Polybase (suorittamalla LoadDataToSQLDW.sql) Azure SQL-DW** yksityinen blob storage tiliisi seuraavista komennoista.

    - Mallin luominen

            EXEC (''CREATE SCHEMA {schemaname};'');

    - Tietokannan kohdistettu tunnistetietojen luominen

            CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
            WITH IDENTITY = ''asbkey'' ,
            Secret = ''{StorageAccountKey}''

    - Tallennustilan Azure-blob ulkoisen tietolähteen luominen

            CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

            CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

    - Luo csv-tiedoston ulkoisessa tiedostomuodossa. Tiedot ovat pakkaamattomat ja kentät on erotettu putkien merkillä.

            CREATE EXTERNAL FILE FORMAT {csv_file_format}
            WITH
            (   
                FORMAT_TYPE = DELIMITEDTEXT,
                FORMAT_OPTIONS  
                (
                    FIELD_TERMINATOR ='','',
                    USE_TYPE_DEFAULT = TRUE
                )
            )
            ;

    - Luo ulkoinen maksun ja työmatkan taulukoiden Kokousesitelmän taksin tietojoukko Azure-blob-säiliö.

            CREATE EXTERNAL TABLE {external_nyctaxi_fare}
            (
                medallion varchar(50) not null,
                hack_license varchar(50) not null,
                vendor_id char(3),
                pickup_datetime datetime not null,
                payment_type char(3),
                fare_amount float,
                surcharge float,
                mta_tax float,
                tip_amount float,
                tolls_amount float,
                total_amount float
            )
            with (
                LOCATION    = ''/nyctaxifare/'',
                DATA_SOURCE = {nyctaxi_fare_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12     
            )  


            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                medallion varchar(50) not null,
                hack_license varchar(50)  not null,
                vendor_id char(3),
                rate_code char(3),
                store_and_fwd_flag char(3),
                pickup_datetime datetime  not null,
                dropoff_datetime datetime,
                passenger_count int,
                trip_time_in_secs bigint,
                trip_distance float,
                pickup_longitude varchar(30),
                pickup_latitude varchar(30),
                dropoff_longitude varchar(30),
                dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Lataa tiedot SQL-tietovarasto ulkoiset taulukot Azure Blob-objektien tallennustilaan

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Luo tiedot-taulukossa (NYCTaxi_Sample) ja lisätä siihen tietoja valitsemalla työmatkan ja maksun taulukot SQL-kyselyjä. (Muutama tätä vaiheittaista on käytettävä tämän mallitaulukon.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

Tallennustilan asiakkaiden maantieteellisen sijainnin vaikuttaa lataamista.

>[AZURE.NOTE] Yksityinen blob storage tilisi maantieteellisen sijainnin mukaan tietojen kopioiminen yksityisen-tilin julkisen Blob-objektien voi kestää noin 15 minuuttia, tai jopa myöhemmäksi, ja tietojen lataaminen tallennustilan tililtä Azure SQL-DW saattaa kestää 20 minuutin ajan tai pidempi.  

Sinun on päättää mitä tehdä, jos sinulla on kaksoiskappaleiden lähde- ja tiedostoja.

>[AZURE.NOTE] Jos .csv-tiedostot kopioidaan yksityinen blob storage tiliisi julkisen blob-säiliö jo yksityinen blob storage tilisi, AzCopy kysyy, haluatko korvata. Jos et halua korvata, kirjoita **n** pyydettäessä. Jos haluat korvata **kaikki** ne, syötteen **** pyydettäessä. Voit myös syötteen **y** korvaa .csv-tiedostoissa yksitellen.

![Piirrä #21][21]

Voit käyttää omilla tiedoillasi. Jos tiedoissa on paikallinen tietokoneen todellisia-sovelluksessa, voit silti käyttää AzCopy paikallinen tietojen lataaminen yksityinen Azure-blob-säiliö. Haluat muuttaa **Lähdesijainnin,** `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, PowerShell-komentosarjatiedosto paikalliseen kansioon, joka sisältää tietoja AzCopy-komennolla.


>[AZURE.TIP] Jos tiedot ovat jo yksityinen Azure-Blob-objektien tallennustilaan todellisia-sovelluksessa, voit ohittaa PowerShell-komentosarjaa AzCopy vaiheessa ja lataa tiedot suoraan Azure SQL-DW. Tämä edellyttää muita muokkauksia komentosarjan käyttötarkoitukseesi tietojen muoto.


Powershell-komentosarjaa myös kytketään Azure SQL-DW tietojen tarkasteluun Esimerkki datatiedostojen SQLDW_Explorations.sql, SQLDW_Explorations.ipynb ja SQLDW_Explorations_Scripts.py niin, että nämä kolme tiedostot ovat valmiita yritetään välittömästi PowerShell-komentosarjaa päätyttyä.

Onnistuneen suorittamisen jälkeen tulee näkyviin näytön alla, kuten:

![][20]

## <a name="dbexplore"></a>Tietojen tarkasteluun ja Azure SQL-tietovarasto-ominaisuus-tekniikka

Tässä osassa on suorittaa tietojen tarkasteluun ja ominaisuus luonti suorittamalla SQL-kyselyjä Azure SQL-DW käyttämällä suoraan **Visual Studio Datatyökalut**vastaan. Kaikki tässä osassa käytetyn SQL-kyselyjä löytyvät nimeltä *SQLDW_Explorations.sql*komentosarjan. Tämä tiedosto on jo ladattu paikalliseen kansioon, PowerShell-komentosarjaa. Voit myös hakea ne [Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql)kohteesta. Mutta Github tiedosto ei ole kytketty oikein Azure SQL-DW tiedot.

Muodostaa yhteyttä Azure SQL-DW Visual Studio käyttäminen SQL DW käyttäjätunnus ja salasana ja Avaa **SQL Object Explorerissa** Vahvista tietokanta ja taulukot on tuotu. Hae *SQLDW_Explorations.sql* -tiedosto.

>[AZURE.NOTE] Voit avata Parallel Data Warehouse (PDW) kyselyeditori-komennolla **Uusi kysely** oman PDW valittuna **SQL Object Explorerissa**. Vakio SQL-kyselyeditori ei tue PDW.

Seuraavien tietojen tyypin tarkasteluun ja ominaisuus luonti tehtävät suoritetaan sisältö:

- Tutki tietoja jaot erilaisten Windowsin muutaman kenttien.
- Tutki pituutta ja leveyttä kenttien tietojen laadun.
- Luo binaarinen ja multiclass luokittelu otsikot **Vihje\_summa**.
- Luo ominaisuudet ja Laske/Vertaile työmatkan etäisyydet.
- Liitä taulukot ja Pura satunnaisia malli, jota käytetään mallien luomiseen.

### <a name="data-import-verification"></a>Tietojen tuominen tarkistaminen

Nämä kyselyt on lyhyt tarkastaminen rivien määrä ja täytetty Polybase's rinnakkain joukkotuontia käyttäen aiemmissa taulukoiden sarakkeiden tuominen

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Tulos:** Hanki 173,179,759 rivien ja sarakkeiden 14.

### <a name="exploration-trip-distribution-by-medallion"></a>Siihen tarkemmin: Matkan jakelu medallion mukaan

Tässä esimerkissä kyselyn tunnistaa medallions (taksin numerot), joka suorittaa yli 100 trips määritetyn ajan kuluessa. Kyselyn hyötyä osioitua taulukon Accessista, koska se on tietämysperusta osion toimintasuunnitelma **nouto\_datetime**. Kyselyt koko tietojoukko myös hyödyntää osioitua taulukon käyttää ja/tai indeksoida tarkistus.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Tulos:** Kyselyn tuloksessa taulukon 13,369 medallions (taksit) ja suorittaa 2013: n työmatkan määrä rivejä. Viimeinen sarake sisältää trips valmis määrä.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Siihen tarkemmin: Medallion ja hack_license työmatkan jakauman.

Tässä esimerkissä tunnistaa medallions (taksin numerot) ja hack_license numerot (ohjaimet) suoritettujen yli 100 trips määritetyn ajan kuluessa.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Tulos:** Kyselyn tuloksessa taulukon 13,369 rivejä 13,369 auton/ohjaimen tunnukset, joka on valmis, Lisää 100 trips 2013: n määrittäminen. Viimeinen sarake sisältää trips valmis määrä.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Tietojen laadun arviointi: Tarkista tietueet, joiden virheellinen pituutta ja/tai leveyttä

Tässä esimerkissä tutkii, jos mitään pituutta ja/tai leveys-kentistä sisältää virheellisen arvon (Radiaani astetta pitäisi olla-90 – 90), tai jos käytössäsi on (0; 0) koordinaatit.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Tulos:** Kysely palauttaa 837,467 trips, joka on virheellinen pituutta ja/tai leveyttä kenttiä.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Siihen tarkemmin: Ei Kallistettu trips jakelu ja Kallistettu

Tämän esimerkin löytää trips, joka on Kallistettu ja numero, joka ei ole Kallistettu tietyn ajan (tai jos sen päällä koko vuosi, kun se on määritetty tähän koko tietojoukko) määrä. Jako kuvastaa käytettävän myöhemmin binaarinen luokitus mallinnus binaarinen otsikko-jakauman.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Tulos:** Kyselyn tuloksessa vuoden 2013: 90,447,622 seuraavat Vihje altistetuissa Kallistettu ja ei-Kallistettu 82,264,709.

### <a name="exploration-tip-classrange-distribution"></a>Siihen tarkemmin: Vihje luokan tai alue-jakauman.

Tässä esimerkissä laskee Vihje alueiden jakautumisen tiettynä aikajaksona (tai koko tietojoukko, jos sen päällä koko vuosi). Tämä on otsikko luokat, jota käytetään myöhemmin multiclass luokitus mallinnus jakautumisen.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Tulos:**

|tip_class  | tip_freq |
| --------- | -------|
|1  | 82230915 |
|2  | 6198803 |
|3  | 1932223 |
|0  | 82264625 |
|4  | 85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Siihen tarkemmin: Laske ja työmatkan etäisyys vertaileminen

Tässä esimerkissä muuntaa nouto ja latauskirjaston pituutta ja leveyttä SQL geography osoittaa, laskee työmatkan etäisyys käyttäen SQL geography eron ja palauttaa satunnaisia vertailun tulokset. Esimerkki rajoittaa kelvollinen koordinaatit käyttämällä vain distributor aiemmin laadun arviointi kyselyn tulokset.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>Ominaisuuden tekniikka SQL-funktioiden käyttäminen

Joskus SQL-funktioissa voi olla tehokas vaihtoehdon ominaisuus tekniikka. Tätä vaiheittaista määritimme SQL-funktion avulla lasketaan nouto- ja dropoff sijainnit suoraan etäisyys. Voit suorittaa SQL-komentosarjat **Visual Studio Datatyökalut**.

Seuraavassa on SQL-komentosarja, joka määrittää etäisyyden-funktiota.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

Tässä on esimerkki Soita tämän funktion luomiseen SQL-kyselyn ominaisuudet:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Tulos:** Tämä kysely luo (2,803,538 rivejä) on taulukon nouto ja dropoff leveyttä ja pituuspiirien Mailia vastaavan suoraan etäisyydet. Seuraavassa on ensin 3 riviä tulokset:

||pickup_latitude | pickup_longitude    | dropoff_latitude |dropoff_longitude | DirectDistance |
|---| --------- | -------|-------| --------- | -------|
|1  | 40.731804 | -74.001083 | 40.736622 | -73.988953 | .7169601222 |
|2  | 40.715794 | -74,010635 | 40.725338 | -74.00399 | .7448343721 |
|3  | 40.761456 | -73.999886 | 40.766544 | -73.988228 | 0.7037227967 |



### <a name="prepare-data-for-model-building"></a>Tietojen valmisteleminen mallin luominen

Seuraava kysely liitokset **nyctaxi\_työmatkan** ja **nyctaxi\_maksun** taulukot, Luo binaarinen luokitus otsikko **Kallistettu**, usean luokan luokitus selitteen **Vihje\_luokan**, ja otoksen poimii koko liitettyjen tietojoukko. Esimerkkejä tehdään hakemalla alijoukkoa trips noudon ajan perusteella.  Tämä kysely voi kopioida ja liitetty suoraan [Azure koneen Learning Studio](https://studio.azureml.net) [Tietojen] [ import-data] erilaisille Suorasaantitiedot Azure SQL-tietokanta-esiintymästä moduuli. Kyselyn ulkopuolelle tietueet, joiden virheellinen (0; 0) koordinaatit.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Kun olet valmis, siirry Azure koneen Learning, hän voi joko:  

1. Tallenna lopullinen SQL-kyselyn poimimiseen ja mallitiedot ja kopioiminen ja liittäminen kyselyn suoraan [Tietojen] [ import-data] Azure koneen Learning moduulissa tai
2. Pysyvä aiot käyttää mallin luomisesta SQL DW uuteen taulukkoon ja uuden taulukon [Tietojen] näytteeksi muodostamien ja tietojen[ import-data] Azure koneen Learning moduuli. Aiemmassa vaiheessa PowerShell-komentosarjaa on tehnyt tämän puolestasi. Voit lukea suoraan tämän taulukon tietojen tuominen-moduulissa.


## <a name="ipnb"></a>Tietojen tarkasteluun ja IPython muistikirjan tekniikka ominaisuus

Tässä osassa on suorittaa tietojen tarkasteluun ja ominaisuus luonti käyttämällä sekä Python ja SQL-kyselyjä vastaan SQL-DW aiemmin luotu. Esimerkki IPython muistikirjan **SQLDW_Explorations.ipynb** ja Python komentosarjatiedosto **SQLDW_Explorations_Scripts.py** on ladattu paikalliseen kansioon. Ne ovat saatavilla myös [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Nämä tiedostot ovat samat Python komentosarjoissa. Python-komentosarjatiedosto sinulle on annettu siltä varalta, että sinun ei tarvitse IPython muistikirjan-palvelimeen. Nämä kaksi Python mallitiedostojen on suunniteltu **Python 2.7**-kohdassa.

Tarvittavat Azure SQL-DW otoksessa IPython muistikirja ja Python-komentosarjatiedosto ladata tietoja paikallisessa tietokoneessa on liitetty PowerShell-komentosarjaa aiemmin. He ovat suoritettavan ilman muutoksista.

Jos olet jo määrittänyt AzureML-työtila, voit ladata otosten IPython muistikirjan AzureML IPython muistikirjan-palveluun ja käynnistä se käynnissä suoraan. Näin ladataan AzureML IPython muistikirjan palvelu:

1. Kirjaudu sisään AzureML lisääminen työtilaan, valitsemalla sen yläreunassa "Studio" ja "muistikirjat-web-sivun vasemmassa reunassa.

    ![Piirrä #22][22]

2. "Uusi"-web-sivun vasemmassa alakulmassa ja valitse "Python 2". Sitten muistikirjan nimi ja valitse Luo uusi tyhjä IPython muistikirja valintamerkki.

    ![Piirrä #23][23]

3. Valitse vasemmasta yläkulmasta uusi IPython muistikirjan "Jupyter"-merkki.

    ![Piirrä #24][24]

4. Vedä ja pudota otosten IPython muistikirjan AzureML IPython muistikirjan palvelun **puun** -sivulle ja valitse **Lataa**. Esimerkki IPython muistikirjan ladataan sitten AzureML IPython muistikirjan-palveluun.

    ![Piirrä #25][25]

Jotta voit suorittaa otosten IPython muistikirjan tai Python komentosarja tiedosto-pakettien tarvitaan seuraavia Python. Jos käytössäsi on AzureML IPython muistikirjan service-pakkauksissa on asennettu valmiiksi.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

Suositellut järjestys, kun ratkaisujen Lisäasetukset analytical AzureML käyttöön suuri tiedoilla on seuraava:

- Lue tietojen pieni otoksen muistin tiedot kehykseen.
- Suorita kaikki visualisoinnit ja explorations näytteeksi tietojen avulla.
- Voit kokeilla ominaisuus tekniikka näytteeksi tietojen avulla.
- Suurempi tietojen tarkasteluun, tietojen käsittelemistä ja ominaisuus tekniikka Käytä Python lähettääksesi suoraan SQL-DW vastaan SQL-kyselyjä.
- Päätä sopivan Azure koneen Learning mallin luominen otoksen suuruus.

Kun se on julkaistu on muutama tietojen tarkasteluun, tietojen visualisointi ja tekniikka esimerkkejä ominaisuus. Lisää tietoja explorations löytyvät otosten IPython muistikirjan ja Python komentosarjan mallitiedostossa.

### <a name="initialize-database-credentials"></a>Tietokannan tunnistetietoja alusta

Alusta tietokannan yhteysasetukset-seuraavat muuttujat:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Tietokantayhteyden luominen

Seuraavassa on yhteysmerkkijonon, joka luo yhteys tietokantaan.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Raportin määrä rivejä ja sarakkeita taulukossa < nyctaxi_trip >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Rivien kokonaismäärästä = 173179759  
- Sarakkeiden määrä = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Raportin määrä rivejä ja sarakkeita taulukossa < nyctaxi_fare >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Rivien kokonaismäärästä = 173179759  
- Sarakkeiden määrä = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Luku-kohdassa pieni tietojen otoksen SQL tietojen varasto-tietokantaan

    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Aika lukemaan mallitaulukon on 14.096495 sekuntia.  
Rivien ja sarakkeiden määrä noudettu = (1 000, 21).

### <a name="descriptive-statistics"></a>Tunnusluvut

Nyt olet valmis, voit tarkastella näytteeksi tietoja. Olemme aloittaminen jotkin tunnusluvut katsoo **työmatkan\_etäisyys** (tai muita kenttiä, joita halua määrittää).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Visualisoinnin: Piirron Esimerkki

Seuraavaksi on Tarkista ruutuun piirron työmatkan etäisyydelle voi esittää quantiles.

    df1.boxplot(column='trip_distance',return_type='dict')

![Piirrä #1][1]

### <a name="visualization-distribution-plot-example"></a>Visualisoinnin: Jakauman piirto-Esimerkki

Joudutaan, joka esittää jakauman ja näytteeksi työmatkan etäisyydet Histogrammi.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Piirrä #2][2]

### <a name="visualization-bar-and-line-plots"></a>Visualisoinnin: Palkki- ja viiva piirretään

Tässä esimerkissä palkin työmatkan etäisyys viisi palkkien kyselyjä ja esittää binning tulokset.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Emme voi piirtää yllä bin jakauman, palkki- tai ja Piirrä viiva:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Piirrä #3][3]

ja

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Piirrä #4][4]

### <a name="visualization-scatterplot-examples"></a>Visualisoinnin: Scatterplot esimerkkejä

Emme esittämään piste piirron välillä **työmatkan\_ajan\_-\_ja osat** ja **työmatkan\_etäisyys** onko kaikki korrelaatio

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Piirrä #6][6]

Olemme samalla tavalla tarkistaa välisen suhteen **korko\_koodin** ja **työmatkan\_etäisyys**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Piirrä #8][8]


### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Tietojen tarkasteluun näytteeksi tietojen SQL-kyselyjen käyttäminen IPython muistikirjaan

Tässä osassa tutustumme tietojen jaot näytteeksi tiedoista, joka on samanlainen edellä luomaasi uuteen taulukkoon. Huomaa, että samalla explorations voidaan suorittaa alkuperäisen taulukoista.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Siihen tarkemmin: Raportin määrä rivejä ja sarakkeita näytteeksi taulukossa

    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Siihen tarkemmin: Tripped Kallistettu/ei-jakauman.

    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Siihen tarkemmin: Vihje luokan jakelu

    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Siihen tarkemmin: Piirtää Vihje jakauman luokan mukaan

    tip_class_dist['tip_freq'].plot(kind='bar')

![Piirrä #26][26]


#### <a name="exploration-daily-distribution-of-trips"></a>Siihen tarkemmin: Trips päivittäinen jakautumisen

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Siihen tarkemmin: Matkan jakauman medallion kohden

    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Siihen tarkemmin: Matkan jakelu medallion ja hakkeroida oleva mukaan

    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Siihen tarkemmin: Matkan ajan jakelu

    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Siihen tarkemmin: Matkan etäisyys jakelu

    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Siihen tarkemmin: Maksun tyyppi jako

    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Tarkista lopullinen lomakkeen featurized taulukon

    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Luoda malleja Azure koneen oppiminen

On nyt Jatka mallin luominen ja mallin [Azure koneen Learning](https://studio.azureml.net)käyttöönotto. Tiedot on valmis käytettäväksi kaikista tunnistaa aiempaa versiota, eli tekstinsyöttö ongelmat:

1. **Binaarinen luokitus**: ennustaa onko Vihje maksettuun matkan.

2. **Multiclass luokitus**: ennustaa solualue, Vihje maksettu aiemmin määritettyä luokkien mukaan.

3. **Regressiosuoran tehtävän**: ennustaa Vihje matkan maksettu määrä.  



Aloita mallinnus Harjoitus, kirjaudu sisään **Azure koneen Learning** lisääminen työtilaan. Jos et ole luonut Koulujen työtilan kone-kohdassa [Luo Azure ML työtila](machine-learning-create-workspace.md).

1. Aloita Azure koneen Learning kohdasta [Azure koneen Learning Studio ominaisuudet?](machine-learning-what-is-ml-studio.md)

2. Kirjaudu sisään ja [Azure koneen Koulujen Studio](https://studio.azureml.net).

3. Studio-kotisivulla on runsaasti tiedot, videoita, opetusohjelmat linkit moduulit viittaus ja muita resursseja. Lisätietoja Azure koneen Learning kuulla [Azure koneen Oppimiskeskus ohjeissa](https://azure.microsoft.com/documentation/services/machine-learning/).

Tyypillinen koulutus kokeen kuuluu seuraavat toimet:

1. Luo **+ Uusi** koe.
2. Tietojen noutaminen Azure ml.
3. Esikäsittely, muunna ja käsitellä tietoja tarpeen mukaan.
4. Luo ominaisuuksia tarpeen mukaan.
5. Tietojen jakaminen koulutus ja kelpoisuuden/testaaminen tietojoukkoja (tai jos käytössäsi on erillinen tietojoukkoja kunkin).
6. Valitse vähintään yksi tietokoneen learning algoritmit mukaan learning ongelman ratkaisemiseen. Esimerkiksi binaarinen luokittelu, multiclass luokittelu, regression.
7. Kouluttaminen vähintään yksi mallien avulla koulutus-tietojoukko.
8. Pisteet vahvistus-tietojoukko käyttämällä koulutetun mallit.
9. Arvioi laskemiseen learning ongelma asianmukainen mittaukset mallit.
10. Tietoa paranna mallit ja parhaat malli otetaan käyttöön.

Tässä on jo tarkasteltavia ja muutetaan SQL-tietovarasto tiedot, ja Azure millilitroina ingest otoksen suuruus päättäneet. Näin voit luoda yhden tai useamman tekstinsyöttö mallien kuvatulla tavalla:

1. Tietojen noutaminen Azure ml [Tietojen] [ import-data] moduuli, käytettävissä olevat **tiedot tarkistavan ja tulostus** -osassa. Lisätietoja on artikkelissa [Tietojen tuominen] [ import-data] moduulin viittaus-sivu.

    ![Azure ML tuo tiedot][17]

2. Valitse **tietolähteen** **Ominaisuudet** -ruudussa **Azure SQL-tietokantaan** .

3. Kirjoita tietokannan nimi DNS **Tietokantapalvelimen nimi** -kenttään. Muoto:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Kirjoita **tietokannan nimi** vastaavassa kentässä.

5. Kirjoita *SQL-käyttäjänimi* **palvelimen käyttäjänimi**ja *salasana* - **palvelimen käyttäjätilin salasana**.

6. Valitse **Hyväksy kaikki palvelinvarmennetta** -vaihtoehto.

7. Liitä kysely, joka poimii tarvittavat Tietokantakentät (mikä tahansa laskettuja kenttiä, kuten otsikot mukaan lukien) ja alaspäin esimerkit **tietokantakyselyn** Muokkaa tekstialueessa tiedot haluamasi otoksen suuruus.

Alla olevassa kuvassa on esimerkki binaarinen luokitus-kokeen tietojen lukeminen suoraan tietovarasto SQL-tietokanta (korvaa nyctaxi_fare ja taulukon nimien nyctaxi_trip rakenteen nimi ja että ongelmatilanteita käytit Taulukkonimet muista). Vastaavat kokeissa on rakennettava multiclass luokittelu ja regression ongelmatilanteita varten.

![Azure ML junassa][10]

> [AZURE.IMPORTANT] Tietojen mallinnus poiminnan ja näytteiden kyselyn esimerkkejä säädetty edellisten kohtien, **kolme mallinnus harjoitusten kaikki otsikot ovat kyselyssä**. Kussakin mallinnus harjoitusten (pakollinen) tärkeä vaihe on **jättäminen pois** tarpeettomat tarrojen muita ongelmia ja kaikki muut **kohde vuodot**. Esimerkiksi käytettäessä binaarinen luokitus käyttää otsikko **Kallistettu** ja jättää kenttiä **Vihje\_luokan**, **Vihje\_summa**, ja **yhteensä\_summa**. Nämä ovat kohde vuodot jälkeen ne ilmentävät Vihje maksettu.
>
> Minkä tahansa tarpeettomien sarakkeiden jättäminen pois tai kohteen vuodot, voivat käyttää [Valitse sarakkeet-tietojoukko] [ select-columns] moduuli tai [Muokkaa metatietojen][edit-metadata]. Katso lisätietoja, [Valitse sarakkeet-tietojoukko] [ select-columns] ja [Muokkaa metatiedot] [ edit-metadata] viittaavat sivut.

## <a name="mldeploy"></a>Ottaa käyttöön Azure koneen Learning mallit

Kun malli on valmis, voit asentaa sen helposti suoraan kokeen WWW-palvelulle. Saat lisätietoja Azure ML verkkopalvelut käyttöönotto [käyttöönotto Azure koneen Learning web-palveluun](machine-learning-publish-a-machine-learning-web-service.md).

Ottaa käyttöön uusi web-palvelu on:

1. Luo tulosmalli kokeen.
2. WWW-palvelun käyttöön.

Luo tulosmalli kokeen **Valmis** -koulutusta koe, valitsemalla alemman toimintoriviltä **Luominen näkyvissä PISTEMÄÄRÄ KOKEILLA** .

![Azure näkyvissä pistemäärä][18]

Azure koneen Learning yrittää luoda tulosmalli koe, koulutus kokeen osat perusteella. Erityisesti se:

1. Tallenna koulutetun malli ja poista mallin koulutus moduulit.
2. Määritä looginen **syötteen portin** edustavan odotettu syöttötiedot rakenne.
3. Määritä looginen **output portti** edustavan odotettu web tulostus-malli.

Luotaessa tulosmalli kokeilla sitä ja tee Säädä tarvittaessa. Tavallinen muutos on korvata syötteen tietojoukko ja/tai kyselyn ulkopuolelle kentät, jotka kuin ne eivät ole käytettävissä, kun palvelua kutsutaan. Kannattaa myös hyvä syötteen tietojoukko ja/tai kyselyn koon pienentäminen muutamalla tietueisiin, vain vähän osoittamaan syötteen rakenne. Tulostus-portin on yhteinen jättää pois kaikki kentät ja sisällyttää vain tulosteen, [Valitse sarakkeet-tietojoukko] avulla **Tulos otsikot** ja **Tulos todennäköisyys liittyy** [ select-columns] moduuli.

Näkyvissä pistemäärä kokeen otoksen annetaan alla olevassa kuvassa. Kun valmis ottamaan, napsauta alemman toimintoriviltä **JULKAISTA WEB-palvelu** -painiketta.

![Azure ML julkaiseminen][11]


## <a name="summary"></a>Yhteenveto
Voit recap, mikä on vielä tehnyt ongelmatilanteita Tässä opetusohjelmassa, olet luonut Azure tietojen tiede ympäristössä, suuri julkisen tietojoukko käsitellyt ottaen ryhmän tietojen tiede vaiheet, aivan kuin hankintaa malliin koulutusta ja Azure koneen Learning WWW-palvelun käyttöönoton.

### <a name="license-information"></a>Käyttöoikeustiedot

Esimerkki tätä vaiheittaista ja sen mukana komentosarjojen ja IPython notebook(s) on jaettu Microsoft MIT-käyttöoikeudet-kohdassa. Tarkista LICENSE.txt-tiedoston sample code GitHub lisätietoja hakemistossa.

## <a name="references"></a>Viittaukset

• [Andrés Monroy Kokousesitelmän taksin Trips lataussivulle](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing Kokousesitelmän 's taksin työmatkan tietojen Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Kokousesitelmän taksin ja Umpiauto eli Myyntipalkkio oheis- ja tilastot](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
