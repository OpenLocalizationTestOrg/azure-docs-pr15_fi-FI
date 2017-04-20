<properties
    pageTitle="Ryhmän tiedot tieteen prosessi-toiminto: SQL Serverin avulla | Microsoftin Azure"
    description="Analytics kokeneet prosessin ja teknologian toimi-"  
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
    ms.date="09/19/2016"
    ms.author="fashah;bradsev"/>


# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Ryhmän tiedot tieteen prosessi-toiminto: SQL Serverin avulla

Tämän opetusohjelman, voit vaihekuvauksen muodostaminen ja ottaminen käyttöön SQL Server ja yleisesti saatavilla dataset-- [Kokousesitelmän taksin Trips](http://www.andresmh.com/nyctaxitrips/) -tietojoukon machine learning-malli. Menettelyn vakiotietojen tieteen Työnkulku seuraa: ingest ja tutkia tietoja, insinööri helpottaa oppimista sitten rakentaa ja ottaa käyttöön mallin ominaisuuksia.


## <a name="dataset"></a>Kokousesitelmän taksin Trips DataSet-ryhmän kuvaus

Kokousesitelmän taksi matkan tiedot on pakattu CSV-tiedostot (noin 48GB pakkaamaton) noin 20 Gigatavua, 173 miljoonaa yksittäistä trips-ja kuljetusmaksut maksaa jokaisen matkan. Matkan jokainen tietue sisältää nouto- ja pudotus sijainti ja aika, anonymized (kuljettajan) hakkeroida lisenssinumero ja medallion (taksi 's unique id)-numeron. Kattaa kaikki trips vuonna 2013 ja toimitetaan kunkin kuukauden seuraavan kahden DataSet-ryhmien:

1. 'trip_data' CSV on matkan tietoja, kuten määrä matkustajia, nouto ja dropoff pisteet, matkan kesto ja matkan pituuden. Seuraavassa on joitakin:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. 'trip_fare' CSV on matkan hinta maksetaan kunkin kalastusmatkan maksutyyppi, maksun summa, Lisämaksu ja verot, vinkkejä ja tietullit, ja maksettu kokonaissumma. Seuraavassa on joitakin:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Yksilöivä avain liittyä matkan\_tiedot ja matkan\_kuljetusmaksua koostuu kentistä: medallion hakkeroida\_todistuksen ja nouto\_päivämäärä ja aika.

## <a name="mltasks"></a>Esimerkkejä tehtävistä, tekstinsyöttö

Voimme antaa kolme ennakoivat ongelmia perusteella *kärki\_summan*, nimittäin:

1. Binary luokitus: ennustaa onko vihjeen maksettiin matkan, eli *Vihje\_summa* , joka on suurempi kuin 0 euroa positiivinen esimerkki, mutta *Vihje\_summa* negatiivinen esimerkki on 0 euroa.

2. Multiclass luokitus: ennustaa Vihje maksaa matkan välillä. Emme Jaa *Vihje\_summan* viisi varastopaikkoja tai luokkia:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. Regressio-tehtävä: Vihje maksanut matkan määrä ennustaa.  


## <a name="setup"></a>Asetus Up Azure tiedot tieteen ympäristön kehittynyt analytics

Kuten näet [Suunnitella ympäristösi](machine-learning-data-science-plan-your-environment.md) opas, on useita vaihtoehtoja toimimaan Azure dataset Kokousesitelmän taksin Trips:

- Azure-tietojen käsitteleminen BLOB: t sitten mallin Azure Machine Learning
- Lataa tiedot SQL Server-tietokantaan ja Azure Machine Learning-malli

Tässä opetusohjelmassa on esitetty rinnakkain joukkotuonnin SQL Serverin tietojen etsintä, ominaisuus tietojen suunnittelu ja näytteenotto alaspäin käyttämällä SQL Server Management Studiossa sekä IPython Muistion avulla. [Mallikomentosarjoja](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) ja [IPython muistikirjat](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) ovat yhteiset GitHub. Näyte IPython muistikirja Azure BLOB-tietojen käsitteleminen on myös samassa paikassa.

Tietoja tieteen Azure-ympäristön määrittäminen:

1. [Varastointi-tilin luominen](../storage/storage-create-storage-account.md)

2. [Azure-Machine Learning-työtilan luominen](machine-learning-create-workspace.md)

3. [Säännös tietojen tiede-Virtual-Machine](machine-learning-data-science-setup-sql-server-virtual-machine.md), jotka toimivat myös SQL Serverin IPython muistikirja-palvelin.

    > [AZURE.NOTE] Mallikomentosarjoja ja IPython muistikirjat ladataan tietoja tieteen virtuaalikoneen asennuksen aikana. VM: N asennuksen jälkeiset komentosarja on suoritettu, kun näytteet ovat oman VM: N tiedostokirjasto:  
    > - Malli komentosarjat:`C:\Users\<user_name>\Documents\Data Science Scripts`  
    > - Näyte muistikirjat IPython:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
    > Jos `<user_name>` on että VM: N Windows-käyttäjätunnuksen. Olemme viittaavat **Mallikomentosarjoja** ja **Näytteen IPython muistikirjat**näyte-kansiot.


DataSet-ryhmän koko, tietolähteen sijainti ja valittu kohde Azure-ympäristön mukaan, tämä tilanne muistuttaa [Skenaario \#5: suuresta tietojoukosta paikallisia tiedostoja, valitse kohde Azure VM: ssä SQL Server](../machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Hae tietoja julkisen tahon

Saat [Taksin Trips Kokousesitelmän](http://www.andresmh.com/nyctaxitrips/) dataset julkiseen paikkaan tahansa [Siirtää tiedot, ja Azure Blob-etäsäilöpalvelun](machine-learning-data-science-move-azure-blob.md) kuvattuja menetelmiä saa käyttää kopioi tiedot uuden virtuaalikoneen.

Jos haluat kopioida tiedot käyttämällä AzCopy:

1. Kirjaudu sisään oman virtual machine (VM)

2. Luo uusi hakemisto on VM: N tiedot levyn (Huomautus: Älä käytä tilapäinen levy joka VM levynä tietojen mukana).

3. Komentorivi-ikkunassa suoritetaan seuraavat Azcopy komentorivi-että data-kansio (2) luodaan korvaamalla < path_to_data_folder >:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

    Kun on valmis AzCopy, yhteensä 24 zip CSV-tiedostoja (12 matkan\_tiedot ja matkaa 12\_kuljetusmaksua) pitäisi olla data-kansio.

4. Pura ladatut tiedostot. Huomaa pakkaamattomat tiedostot kansioon. Tämän kansion viitataan nimellä < polku\_ja\_tiedot\_tiedostoja\>.

## <a name="dbload"></a>Irtotavarana tuonnin tiedot SQL Server-tietokantaan

Lastaus/siirtää suuria määriä tietoja SQL-tietokannan ja seuraavien kyselyyn suorituskykyä voidaan parantaa _osioitu taulukoita_ja näkymiä. Tässä kohdassa olemme ohjeiden mukaan Luo uusi tietokanta ja lataa tiedot osioitu taulukoita samanaikaisesti [Rinnakkain irtotavarana tietojen tuominen käyttämällä SQL osiotaulukoita](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) .

1. Kirjautuneena oman VM: n, Käynnistä **SQL Server Management Studio**.

2. Yhdistä käyttäen Windows-todennusta.

    ![SSMS muodostaa][12]

3. Jos sinulla ei ole vielä muutettu SQL Server-todennustilaa ja SQL login uusi käyttäjä on luotu, Avaa script-tiedosto nimeltä **muuttaa\_auth.sql** **Komentosarjat** -kansioon. Muuta oletus käyttäjätunnus ja salasana. Valitse **! Suorita** työkalurivin Suorita komentosarja.

    ![Suorita komentosarja][13]

4. Tarkista tai muuta SQL Server tietokanta ja lokitiedostot oletuskansioista sen varmistamiseksi, että uusiin tietokantoihin tallennetaan tietoja levylle. SQL Server-VM kuva, joka on optimoitu kuormien datawarehousing on ennalta määritettyjä data- ja lokitiedostojen levyjen kanssa. Jos käyttämäsi VM ei sisältänyt tietoja levyn ja VM: N asennuksen aikana lisätty uusi virtuaalinen kiintolevy, Vaihda oletuskansioista seuraavasti:

    - Napsauta SQL Serverin nimeä vasemmassa paneelissa ja valitse **Ominaisuudet**.

        ![SQL-palvelimen ominaisuudet][14]

    - **Tietokannan asetukset** -luettelosta **Valitse sivu** vasemmalla.

    - Tarkista ja/tai muuttaa **tietokannan oletussijainnit** **Tiedot levyn** valinta tiloihin. Tämä on jossa Jos sijainti oletusasetukset ovat uusia tietokantoja.

        ![SQL-tietokannan oletusarvot][15]  

5. Luo uusi tietokanta ja joukko FileGroup-ryhmien säilyttämiseen osioitu taulukoita, Avaa komentosarjan **Luo\_db\_default.sql**. Komentosarja luo uuden tietokannan, joka on nimeltään **TaxiNYC** ja 12 FileGroup-ryhmien tiedot oletussijaintiin. Tiedostoryhmän kunkin kuukauden matkan mahtuu\_tiedot ja matkan\_tiedot kuljetusmaksua. Muokata tietokannan nimi sekä tarvittaessa. Valitse **! Suorittaa** suorittaa komentosarjan.

6. Luo seuraavaksi kaksi osiotaulukoita, yksi matkan\_tiedot ja toisen matkan\_kuljetusmaksua. Avaa komentosarjan **Luo\_ositetun\_table.sql**, joka on:

    - Luo partition-funktio jakaa tiedot kuukausittain.
    - Luo osio järjestelmän jokaisen kuukauden tiedot yhdistetään toiseen tiedostoryhmään.
    - Osamalli yhdistetty kaksi osioitu taulukoita luodaan: **nyctaxi\_matkan** mahtuu matkan\_tietoja ja **nyctaxi\_kuljetusmaksua** mahtuu matkan\_tiedot kuljetusmaksua.

    Valitse **! Suorita** ja Suorita komentosarja luo osioitu taulukoita.

7. **Näyte Scripts** -kansio on kaksi näytettä PowerShell-komentosarjojen rinnakkainen irtotavarana tuonnin tiedot SQL Server-taulukot on osoitettava toimittamalla.

    - **bcp\_rinnakkainen\_generic.ps1** on yleiskomentosarjan joukkona rinnakkaisia tuo tiedot taulukkoon. Muokkaa tämän komentosarjan input- ja muuttujien määrittäminen komentosarjan kommenttirivit esitetyllä tavalla.
    - **bcp\_rinnakkainen\_nyctaxi.ps1** on etukäteen määritetty versio, tulevan yleiskomentosarjan ja sitä voidaan käyttää, molempien taulukoiden Kokousesitelmän taksin Trips-tietojen lataaminen.  

8. Napsauta hiiren kakkospainikkeella **bcp\_rinnakkainen\_nyctaxi.ps1** komentosarjan nimi ja valitse **Muokkaa** avataksesi PowerShell. Valmiiksi määritetyt muuttujat tarkastella ja muokata valitun tietokannan nimeä, syöttötiedot kansio, lokin kohdekansio ja polut näyte muodossa tiedostot **nyctaxi_trip.xml** ja **nyctaxi\_fare.xml** (mikäli **Näytteen Scripts** -kansio).

    ![Bulk-tietojen tuominen][16]

    Voit myös valita todennustilaa, oletusarvo on Windows-todennusta. Napsauta vihreää nuolta työkalurivin Suorita. Komentosarjan käynnistää 24 tuo joukkotoiminnot rinnakkais-12 osioitu kutakin taulua varten. Tietojen tuonnin edistyminen voi valvoa avaamalla edellä määritellyt tiedot SQL Server oletuskansioon.

9. PowerShell-komentosarja ilmoittaa alkamis- ja päättymisajat. Kaikki joukkosähköposti tuonti on valmis, kun lopetusaika on raportoitu. Tarkista loki kohdekansio, varmista, että joukkotuonnit onnistuivat, eli log kohdekansio ei ole virheitä.

10. Tietokannan voi nyt talteenoton, ominaisuuden vesirakennustöihin ja muihin toimintoihin haluamallasi tavalla. Koska taulukot on osioitu sen mukaan, **nouto\_datetime** kentässä, kyselyt, jotka sisältävät **nouto\_datetime** osio järjestelmän hyötyvät **WHERE** -lauseen ehdot.

11. **SQL Server Management Studio**, tutustu annettu mallikomentosarjaa **näyte\_queries.sql**. Näyte kyselyiden suorittaminen, korosta kyselyrivit ja valitse sitten **! Suorita** -työkalurivin.

12. Kokousesitelmän taksin Trips-tiedot ladataan kahdessa erillisessä taulukossa. Jos haluat parantaa liitos toimintoja, on erittäin suositeltavaa tehdä taulukoita. Olevaa mallikomentosarjaa **Luo\_osioitu\_index.sql** osioitu indeksejä avaimen yhdistelmä join **medallion hakkeroida\_käyttöoikeus ja nouto\_päivämäärä ja aika**.

## <a name="dbexplore"></a>Tiedot tutkimus- ja SQL Server-palvelimen toiminto Engineering

Tässä kohdassa olemme suorittaa tietojen hyödyntämiseen ja sukupolven ominaisuus SQL-kyselyt suorittamalla suoraan **SQL Server Management Studion** avulla aiemmin luodun SQL Server-tietokantaan. Mallikomentosarja, jonka nimi on **näyte\_queries.sql** on tarkoitettu **Näyte Scripts** -kansio. Muokkaa komentosarjaa siten muuttaa tietokannan nimeä, jos se on muu kuin oletus: **TaxiNYC**.

Tässä se on:

- Muodosta yhteys **SQL Server Management Studion** avulla joko Windows-todennus tai SQL-todennusta ja SQL-käyttäjätunnus ja salasana.
- Tutustu tietojen jakautuminen eri Windowsin muutaman kentän.
- Tutkia itäistä pituutta ja pohjoista leveyttä kenttien tietojen laatua.
- Luo luokittelu binaari- ja multiclass perustuvia tekstiselitteitä **kärki\_summan**.
- Luo ominaisuudet ja Laske tai verrata matkan matka.
- Liitä taulukot ja poimia satunnaisesti näyte, jota käytetään sellaisten mallien muodostamista varten.

Kun olet valmis jatkamaan, Azure Machine Learning, hän voi joko:  

1. Tallenna lopullinen ja näyte ja kopioi-Liitä kysely [Tuontitiedot] suoraan SQL-kysely[ import-data] Azure Machine Learning-moduuli tai
2. Säilyisivät otokseen ja Selvitetyt aiot käyttää mallin luomista uuden tietokannan taulukon ja [Tuontitiedot] uuteen taulukkoon käyttää[ import-data] Azure Machine Learning-moduulissa.

Tässä kohdassa voimme tallentaa lopullinen näyte ja kysely. Toinen tapa on osoittanut [tietojen hyödyntämiseen ja ominaisuus Engineering IPython muistikirjan](#ipnb) osan.

Rivien ja sarakkeiden avulla rinnakkainen joukkotuonnin aiemmin täytetty taulukoiden määrä nopeasti varmistamista varten

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Etsintä: Medallion mukaan matkan jakelu

Tässä esimerkissä määritetään yli 100 trips medallion (taksin numerot) tietyn ajanjakson kuluessa. Kyselyn hyötyä osioitu taulukon Accessista, koska on tietämysperusta toimintasuunnitelman osion **nouto\_päivämäärä ja aika**. Koko tietojoukon kysellään myös tehdä osioitu taulukon käyttö ja/tai indeksi skannaus.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Etsintä: Medallion ja hack_license matkan jakelu

    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Tietojen laadun arviointi: Tarkista tietueet, joiden virheellinen itäistä pituutta ja pohjoista leveyttä

Tässä esimerkissä tutkii Jos itäistä pituutta ja pohjoista leveyttä kenttiä sisältää virheellisen arvon (Radiaani astetta on oltava väliltä-90-90), tai (0, 0) koordinaatit.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Talteenoton: Tipped vs. ei-Kallistettu Trips-jakelu

Tässä esimerkissä etsii trips, joka oli Kallistettu Kallistettu ei tiettynä aikana kausi (tai jos koko vuoden kattavan koko tietojoukon) määrä. Jako kuvastaa binary otsikko, jota käytetään myöhemmin binary luokitus mallinnus jakelu.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Talteenoton: Vihje luokka/alueen jako

Tämä esimerkki laskee Vihje alueiden jakautuminen tietyn ajanjakson (tai jos koko vuoden kattavan koko dataset). Tämä on otsikko luokat, joita käytetään myöhemmin mallinnus multiclass luokittelua varten jakautuminen.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Talteenoton: Laskea ja verrata matkan etäisyyden

Tässä esimerkissä muuntaa nouto- ja pudotus itäistä pituutta ja pohjoista leveyttä SQL maantieteeseen osoittaa matkan etäisyyden käyttämällä SQL Maantiede eron laskee ja palauttaa vertailun tulokset satunnainen näyte. Esimerkki rajoittaa tulokset kysely laadun arviointi kattaa aiemmin vain käyttämällä kelvollista koordinaatteihin.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>SQL-kyselyiden ominaisuus Engineering

Otsikko luonnin ja Maantiede muuntaminen talteenoton kyselyt voidaan myös luoda otsikoiden ominaisuuksien poistamalla laskettu osa. Lisäominaisuus engineering SQL esimerkkejä annetaan [tietoja ja ominaisuus Engineering IPython muistikirjan](#ipnb) osan. On tehokkaampaa tehdä sukupolven kyselyjä koko dataset tai suuren määrän käyttämällä SQL-kyselyitä suoritetaan SQL Server-tietokannan esiintymä, joka ominaisuus. Kyselyt saatetaan suorittaa **SQL Server Management Studio**, IPython muistikirjan tai minkä tahansa työkalu/kehitysympäristö jossa tietokantaa voi käyttää paikallisesti tai etäyhteyden välityksellä.

#### <a name="preparing-data-for-model-building"></a>Mallin luominen tietojen valmistelemisesta

Seuraavat kyselyn liitokset **nyctaxi\_matkan** ja **nyctaxi\_kuljetusmaksua** taulukot, Luo binaarinen luokitus otsikko **Kallistettu**, usean luokan luokituksen otsikko **Vihje\_luokan**, ja 1 % satunnainen näyte poimii koko liitetty dataset. Tämä kysely voidaan kopioida sitten liittää suoraan [Azure Machine Learning Studio](https://studio.azureml.net) - [Tietojen tuominen] -[ import-data] erilaisille tiedot suoraan SQL Server-tietokannan esiintymästä Azure-moduuli. Kysely sulkee pois tietueiden virheellisesti (0, 0) koordinaatit.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Tietojen hyödyntämiseen ja ominaisuus IPython muistikirjan suunnittelu

Tässä kohdassa olemme suorittaa tietojen hyödyntämiseen ja toiminnon luonti sekä Python SQL kyselyitä, jotka perustuvat aiemmin luodun SQL Server-tietokannan avulla. Näyte IPython muistikirjan nimeltä **machine-Learning-data-science-process-sql-story.ipynb** on tarkoitettu **Näyte IPython muistikirjat** -kansioon. Tämä muistikirja on myös saatavilla [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

Suositeltava suoritusjärjestys ISO tietoja käsiteltäessä on seuraava:

- Lue tietoja pieni näyte muistin tiedot-kehykseen.
- Tehdä joitakin visualisoinnit ja explorations otokseen kuuluvien tietojen avulla.
- Voit kokeilla ominaisuus engineering otokseen kuuluvien tietojen avulla.
- Käyttää enemmän tietoja talteenoton tietojenkäsittelyä ja suunnittelu ominaisuus, Python antaa suoraan vastaan Azure VM: ssä SQL Server-tietokannan SQL-kyselyitä.
- Päättää Azure Machine Learning mallin luominen käytettävän näytteen koko.

Kun haluat jatkaa Azure Machine Learning, hän voi joko:  

1. Tallenna lopullinen ja näyte ja kopioi-Liitä kysely [Tuontitiedot] suoraan SQL-kysely[ import-data] Azure Machine Learning-moduulissa. Tämä menetelmä on osoittanut [Azure Machine Learning rakennuksen mallit](#mlmodel) -osassa.    
2. Säilyisivät otokseen ja Selvitetyt aiot käyttää mallin luomista uuden tietokannan taulukon ja sitten käyttää [Tuontitiedot] uuteen taulukkoon[ import-data] moduuli.

Seuraavassa on joitakin tietoja talteenoton tietojen visualisointi ja esimerkkejä suunnittelu ominaisuus. Lisää esimerkkejä Katso näyte SQL-IPython muistikirjan **Näytteen IPython muistikirjat** -kansioon.

#### <a name="initialize-database-credentials"></a>Alustaa tietokannan valtuustiedot

Alustaa tietokannan yhteysasetukset-seuraavat muuttujat:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Tietokantayhteyden luominen

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Rivien ja sarakkeiden taulukon nyctaxi_trip numero

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Rivien kokonaismäärä = 173179759  
- Sarakkeiden kokonaismäärä = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Luku, pieni näyte SQL Server-tietokannasta

    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Aikaa lukea mallitaulukon on 6.492000 sekuntia  
Haettujen rivien ja sarakkeiden määrä = (84952, 21)

#### <a name="descriptive-statistics"></a>Tunnusluvut

Ovat nyt valmiita otokseen tietoja. Emme Aloita kuvaavia tilastoja katsomalla **matkan\_matkan** (tai jokin muu) avainkenttien:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Visualisointi: Ruutuun koealan Esimerkki

Seuraavaksi tarkastelemme ruutuun koealan, visualisoida quantiles matkan etäisyyden

    df1.boxplot(column='trip_distance',return_type='dict')

![Piirrä #1][1]

#### <a name="visualization-distribution-plot-example"></a>Visualisointi: Jakelu koealan Esimerkki

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Piirrä #2][2]

#### <a name="visualization-bar-and-line-plots"></a>Visualisointi: Palkki ja viiva koealojen

Tässä esimerkissä viiden varastopaikkoja huomioon matkan etäisyyden varastopaikan ja visualisoida binning tulokset.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Olemme piirtää edellä varastopaikan jako-palkin tai viivan piirto kuin alla

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Piirrä #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Piirrä #4][4]

#### <a name="visualization-scatterplot-example"></a>Visualisointi: Scatterplot-Esimerkki

Esittelemme XY koealan välillä **matkan\_aika\_-\_s** ja **matkan\_matkan** onko mitään korrelaatiota

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Piirrä #6][6]

Samoin tarkistamme suhdetta **nopeus\_koodi** ja **matkan\_matkan**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Piirrä #8][8]

### <a name="sub-sampling-the-data-in-sql"></a>Näytteenotto tietoja SQL-osa

Kun valmisteleminen tietojen malli [Azure Machine Learning Studio](https://studio.azureml.net)muodostamassa, voit päättää **SQL-kysely tuontitietoja-moduulin käyttäminen** tai pysyvä selvitetyt ja otokseen tietoja uuteen taulukkoon, jonka avulla [Tietojen tuominen] [ import-data] kanssa yksinkertainen moduuli *mistä *VALITA* < oman\_uusi\_taulukko\_nimi >**.

Tässä osassa luodaan säilyttämään otokseen ja Selvitetyt tiedot uuteen taulukkoon. Mallin luominen suoraan SQL-kysely on esimerkki on tarkoitettu [tietojen ja SQL Serverin ominaisuus Engineering](#dbexplore) -osasta.

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Esimerkki luo taulukon ja täyttää liitetyssä taulukossa % 1. Pudota taulukon ensimmäisen, jos se on olemassa.

Tässä kohdassa olemme Liitä taulukot **nyctaxi\_matkan** ja **nyctaxi\_kuljetusmaksua**purkaa 1 % satunnainen näyte ja otokseen säilyisivät uuden taulukon nimen **nyctaxi\_yksi\_%**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>SQL-kyselyiden avulla IPython muistikirjan tiedot talteenoton

Tässä jaksossa tutustumme näyte 1 %-tietoja, joita säilytetään edellä olemme luotu uusi taulukko käyttämällä tietojen jaot. Huomaa, että samanlaisia explorations voi tehdä käyttämällä alkuperäinen taulukoiden avulla voit rajoittaa etsinnän tulokset tietyn ajan jakson using rajoittamalla malli tai **TABLESAMPLE** **nouto\_päivämäärä ja aika** osioiden, kuten [tietojen ja SQL Serverin ominaisuus Engineering](#dbexplore) -osasta.

#### <a name="exploration-daily-distribution-of-trips"></a>Talteenoton: Päivittäinen jakelu trips

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Etsintä: Matkan medallion Työntekijäjakauma

    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Toiminnon luonti käyttämällä SQL-kyselyiden IPython muistikirjan

Tässä kohdassa emme Luo uudet etiketit ja ominaisuuksia käyttämällä suoraan SQL-kyselyitä, 1 % mallitaulukon käsittelee olemme luotu edellisessä kohdassa.

#### <a name="label-generation-generate-class-labels"></a>Etiketin luonti: Luo luokan otsikot

Seuraavassa esimerkissä olemme Luo kahdet mallinnus käytettävät otsikot:

1. Binary luokan otsikot **Kallistettu** (korrelaatio, jos annetaan Vihje)
2. Multiclass tarrat **kärjen\_luokan** (korrelaatio Vihje varastopaikalle tai alue)

        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''

        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()

        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''

        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Feature suunnittelu: Määrä ominaisuuksia sarakkeiden Categorical

Tässä esimerkissä muuntaa numeerisen kentän kentän categorical korvaamalla tietojen sen esiintymien määrä kussakin luokassa.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Ominaisuus: suunnittelu Varastopaikan ominaisuudet numeerisia sarakkeita

Tässä esimerkissä muuntaa numeerisen kentän jatkuva valmiiksi määritetyn luokan alueita eli muunnoksen numeerisen kentän categorical-kenttään.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Feature suunnittelu: Pura desimaali leveys-ja pituusasteena sijainnin ominaisuudet

Tässä esimerkissä erittelee desimaalimuodossa kentän leveyttä ja pituutta useita eri rakeisuus alueen kenttiin, maa, kaupunki, kaupunki, block jne. Huomaa, että uuden geo-kenttiä ei ole yhdistetty todellisen sijainnin. Varastopaikkojen määritys geocode lisätietoja [Bing Maps muut palvelut](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Tarkista taulukon featurized lopullinen muoto

    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Olemme nyt valmis jatkamaan mallin luominen ja [Azure Machine Learning](https://studio.azureml.net)mallin käyttöönottoa. Tiedot ovat valmiina missään nimittäin aiemmin havaittujen ongelmien perusteella ennakoinnin:

1. Binary luokitus: ennustaa onko Vihje oli maksanut matkan.

2. Multiclass luokitus: ennustaa alueen Vihje maksaa mukaan aiemmin määritettyjä luokkia.

3. Regressio-tehtävä: Vihje maksanut matkan määrä ennustaa.  


## <a name="mlmodel"></a>Building mallit Azure Machine Learning

Aloita mallinnus harjoituksessa kirjautua Azure Machine Learning-työtilassa. Jos et ole luonut Koulujen työtilan kone, katso [Azure Machine Learning työtilan luominen](machine-learning-create-workspace.md).

1. Aloita Azure Machine Learning on [Mikä on Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)

2. Kirjaudu sisään ja [Azure Machine Learning Studio](https://studio.azureml.net).

3. Studio-kotisivu tarjoaa runsaasti tietoa, videoita, opetusohjelmat, linkit viittaus moduuleja ja muita resursseja. Lisätietoja Azure Machine Learning siitä kuulee [Azure-koneen ohjeissa Oppimiskeskus](https://azure.microsoft.com/documentation/services/machine-learning/).

Tyypillinen koulutus-koe muodostuu seuraavista osista:

1. Luo **Uusi +** koe.
2. Saat tietoja Azure Machine Learning.
3. Esikäsittely, muuntaa ja käsitellä tietoja tarpeen mukaan.
4. Luo tarvittavat ominaisuudet.
5. Jaa tiedot DataSet-ryhmien koulutusta/vahvistuksen/testaus (tai eri DataSet-ryhmien kunkin).
6. Valitse yksi tai useampi kone Koulujen algoritmeja oppimisen ongelman ratkaisemiseksi mukaan. Esim binaarinen luokitus, multiclass luokitusta, regression.
7. Opettaa yhden tai useita malleja avulla koulutuksen DataSet-ryhmään.
8. Sija vahvistus DataSet-ryhmän avulla koulutettujen mallit.
9. Arvioida asiaa mittarit oppimisen ongelmaan laskea mallit.
10. Hieno viritys mallit ja paras malli otetaan käyttöön.

Tässä harjoituksessa olemme jo vastuullisina ja SQL Serverin tietoja muutetaan, ja Azure Machine Learning-ingest otoskoon päättäneet. Voit luoda yhden tai useamman päätimme ennakoivat mallit:

1. Azure Machine Learning [Tietojen tuominen] , tietojen[ import-data] moduulin **tietojen syöttö ja tulostus** -osassa. Lisätietoja [Tietojen tuominen] [ import-data] moduuli käsittelevällä sivulla.

    ![Tietojen tuominen Azure Machine Learning][17]

2. Valitse **tietolähteen** **ominaisuuksien** paneelissa **SQL Azure-tietokanta** .

3. Kirjoita **Tietokantapalvelimen nimi** -kenttään tietokannan DNS-nimi. Muoto:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Kirjoita **tietokannan nimi** vastaavassa kentässä.

5. **SQL-käyttäjätunnus** - **aqccount palvelimen käyttäjänimi ja salasana **palvelimen käyttäjän tilin salasana **.

6. Valitse **Hyväksy kaikki palvelimen varmenteen** vaihtoehto.

7. **Kysely** Muokkaa tekstialuetta Liitä kysely, joka tarpeen purkaa tietokannan kenttiä (mukaan lukien mahdolliset laskettuja kenttiä, kuten otsikot) ja alas-näytteet tiedot haluttuun näytteen koko.

Esimerkki tietojen lukeminen SQL Server-tietokantaan suoraan binary luokitus-koe on kuvassa alla. Samankaltaisia kokeita on rakennettava multiclass luokitukseen ja regressio-ongelmia.

![Azure Machine Learning junan][10]

> [AZURE.IMPORTANT] Mallinnus tietojen purkaminen ja näytteenotto kyselyn esimerkkejä säädetty **kolmeen mallipuun harjoituksiin kaikki otsikot sisällytetään kyselyn**edellisessä osiossa. Jokaisessa mallinnus harjoituksiin (pakollinen) tärkeä askel on **pois** tarpeettomat otsikot muita ongelmia ja minkä tahansa muun **kohteen vuodot**. Esim binary luokittelua käytettäessä Käytä otsikko **Kallistettu** ja jättää ne **tip\_luokan**, **Vihje\_summa**, ja **yhteensä\_summan**. Nämä ovat kohde vuodot, koska ne ilmentävät kärki maksettu.
>
> Poista tarpeettomat sarakkeet ja/tai suunnata vuodot, asiakas saa käyttää [Dataset-sarakkeiden valitseminen] [ select-columns] moduuli tai [Muokkaa metatietoja][edit-metadata]. Lisätietoja [Sarakkeiden valitseminen Dataset-] [ select-columns] ja [Muokkaa metatietoja] [ edit-metadata] viittaavat sivut.

## <a name="mldeploy"></a>Azure Machine Learning mallien käyttöönotto

Kun malli on valmis, voidaan helposti käyttää sitä suoraan kokeeseen WWW-palvelulle. Saat lisätietoja Azure Machine Learning Internet-palvelujen käyttöönotto [Azure Machine Learning-verkkopalvelun käyttöönotto](machine-learning-publish-a-machine-learning-web-service.md).

Voit ottaa käyttöön uuden web-palvelun, sinun on:

1. Luo tulosmalli kokeeseen.
2. Ota käyttöön web-palveluun.

Luomaan tulosmalli koe **Valmis** koulutus-koe napsauttamalla pienempi toimintopalkki **Luo PISTEYTYS KOKEILLA** .

![Azure-pisteytys][18]

Azure Machine Learning yrittää luoda tulosmalli kokeen, koulutuksen kokeen osien perusteella. Erityisesti se:

1. Koulutetut malli tallennetaan ja poista mallin koulutus-moduuleja.
2. Määritä edustamaan odotettu syöttötiedot rakenne looginen **tuloliitäntä** .
3. Määritä loogisen **lähtöportti** edustamaan odotettu web service-tulosrakenne.

Tulosmalli kokeen luomisen tarkastettavaksi ja säädä tarvittaessa. Tyypillinen muutos on korvata ilmauksen dataset tai kyselyn jossa suljetaan pois otsikon kentät kuin nämä eivät ole käytettävissä, kun palvelua kutsutaan. Se on myös hyvä syöttö dataset tai kyselyn koon pienentäminen muutaman tietueen, voit vain riitä osoittamaan syötteen rakennetta. Output-portti on pois kaikki syöttökentät ja sisällyttää vain käyttämällä [Valitse sarakkeet Dataset-ryhmän] tulos **Tulos otsikoita** ja **Tulos todennäköisyydet** yhteiset[ select-columns] moduuli.

Koe pisteytys malli on kuvassa alla. Kun se on valmis ottamaan valitsemalla **JULKAISE WEB-palvelun** alempi toiminto-palkissa.

![Julkaise Azure Machine Learning][11]

Luomasi recap, tämä vaihe vaiheelta opetusohjelma, Azure tiedot tieteen ympäristössä, kanssa aivan hankintaa voidaan mallintaa Azure Machine Learning-web-palvelun käyttöönotto ja koulutus-julkinen suuresta tietojoukosta.

### <a name="license-information"></a>Käyttöoikeustietoja

Tässä vaihekuvauksessa näytteen ja sen mukana komentosarjat ja IPython notebook(s) jakamat Microsoft MIT lisenssin nojalla. Tarkista LICENSE.txt-tiedoston kansioon mallikoodia-GitHub lisätietoja.

### <a name="references"></a>Viitteet

• [Andrés Monroy Kokousesitelmän taksin Trips lataussivulle](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing mukaan Chris Whong's Kokousesitelmän taksi matkan tiedot](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Kokousesitelmän taksin ja Umpiauto eli komission tutkimus ja tilastot](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
