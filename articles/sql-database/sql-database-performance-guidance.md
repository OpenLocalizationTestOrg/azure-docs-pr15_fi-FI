<properties
    pageTitle="Azure SQL-tietokantaan ja suorituskyvyn yksittäisen tietokantojen | Microsoft Azure"
    description="Tämän artikkelin avulla voit selvittää, mitä service-tason valitseminen sovelluksen. Se myös suosittelee tapoja paranna sovelluksen käyttämistä eniten Azure SQL-tietokantaan."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/13/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-and-performance-for-single-databases"></a>Azure SQL-tietokantaan ja yksittäisen tietokantojen suorituskyky

Azure SQL-tietokanta on kolme [palvelutasot](sql-database-service-tiers.md): Basic, Vakio ja Premium. Palvelun kunkin tason eristää ehdottoman SQL-tietokantaan voit käyttää ja takaa ennakoitavissa suorituskyvyn kyseisen palvelun käyttöoikeustason resurssit. Tässä artikkelissa Tarjoamme ohjeita, jotka auttavat sinua valitsemaan sovelluksen service-taso. Käsiteltävät aiheet myös, että voit hienosäätää sovelluksesi Azure SQL-tietokanta tehokkaasti tavalla.

>[AZURE.NOTE] Tässä artikkelissa keskitytään suorituskyvyn ohjeet yksittäisen tietokantojen Azure SQL-tietokantaan. Ohjeita suorituskykyyn liittyvät joustavasti tietokannan jakavat on artikkelissa [joustavasti tietokannan jakavat hinnan ja suorituskyvyn Huomioitavaa](sql-database-elastic-pool-guidance.md). Huomaa, että päde monia säätäminen suosituksia tämän artikkelin joustavasti tietokannan varannon, ja poistaa samalla suorituskyvyn etuja.

Nämä ovat kolme Azure SQL-tietokanta-palvelutasot, voit valita (suorituskyvyn mitataan tietokannan siirtonopeuden yksiköt tai [DTUs](sql-database-what-is-a-dtu.md):

- **Perustiedot**. Perustietoja palvelun taso tarjouksia hyvän suorituskyvyn ennustettavuutta jokaiselle tietokannalle, tunti tunti. Perus-tietokannan riittävät resurssit tukevat hyvän suorituskyvyn pieni tietokannassa, jossa ei ole useita samanaikaiset pyynnöt.
- **Vakio**. Normaali palvelutaso on parannettu suorituskyky ennustettavuutta ja aiheuttaa palkin tietokantoja, joissa on useita samanaikaiset pyynnöt, kuten työryhmän ja web-sovellukset. Valitessasi vakiorivejä taso-tietokanta-tietokantasovelluksen ennakoitavissa suorituskyvyn perusteella voit muuttaa kokoa minuutit minuutin aikana.
- **Premium**. Premium-palvelutaso sisältää ennakoitavissa suorituskyvyn toisen päälle toiseksi Premium kunkin tietokannalle. Kun valitset Premium-palvelutaso, voit kokoa tietokantasovelluksen tietokannan huippu-kuormituksen perusteella. Suunnitelman poistaa tapauksissa mitä suorituskyvyn varianssi voi aiheuttaa pienen kyselyjä kestää odotettua viive-luottamukselliset toimintoja. Tämän mallin voit yksinkertaistamaan kehittäminen ja tuotteen kelpoisuustarkistus jaksot sovelluksissa, jotka täytyy tehdä vahva laskelmia piikin resurssin tarpeisiin, suorituskyvyn varianssi tai kyselyviive.

Palvelun kunkin tason on määritettävä suorituskyky-tason niin, että voit halutessasi maksat kapasiteettia, sinun on. Voit [säätää kapasiteettia](sql-database-scale-up.md), ylös tai alas työmäärää muutoksina. Jos tietokannan työmäärää on suuri Edellinen koulun ostoskoriin Vuodenaika-kentät, voit ehkä suurentaa tietokannan suorituskyky-tason määritetyn ajan syyskuu heinäkuun. Voit pienentää sen päättyessä piikin Vuodenaika-kentät. Voit pienentää mitä optimointi cloud ympäristön kausivaihteluksi, yrityksen maksat. Tämän mallin toimii myös hyvin ohjelmiston tuotteen release jaksot. Testi-ryhmän voi kohdistaa kapasiteetin samalla, kun se testaus on suoritettu, ja vapauta sitten kyseisen kapasiteettia, kun testaus on valmis. Kapasiteetin pyynnön mallissa maksat kapasiteetin tarvitse sitä ja välttää tehtäviinsä erillinen resursseja, joita voi käyttää vain harvoin.

## <a name="why-service-tiers"></a>Miksi palvelun tasoa?

Vaikka tietokanta kunkin työmäärää voivat olla erilaiset, palvelutasot tarkoituksena on antamaan suorituskyvyn ennustettavuutta suorituskyvyn eri tasoilla. Asiakkaat, joilla suurissa tietokannan resurssivaatimusten voidaan käyttää Lisää erillinen tietojenkäsittely-ympäristöön.

### <a name="common-service-tier-use-cases"></a>Yleisiä palvelutaso käyttötapauksiin

#### <a name="basic"></a>Perustoiminnot

- **Sinulla on käytön aloittaminen Azure SQL-tietokantaan**. Sovellukset, jotka ovat usein kehitteillä ei tarvitse erinomainen suorituskyky tasot. Tavallinen tietokantoja on ihanteellinen ympäristössä tietokannan kehittämiseen, alin hinta-kohtaan.
- **Yksi käyttäjä tietokanta on**. Sovellukset, jotka yksittäisen käyttäjän liittäminen tietokannan yleensä ei ole hyvin samanaikainen ja suorituskykyä koskevat vaatimukset. Nämä sovellukset on hakevien Basic palvelutaso.

#### <a name="standard"></a>Vakio

- **Tietokannassa on useita samanaikaiset pyynnöt**. Sovellukset, jotka palvelun useamman kuin yhden käyttäjän yleensä kerrallaan on suurempi suorituskyvyn tasot. Sivustot, jotka haluat Keskitaso liikenne tai osaston sovellukset edellyttävät Lisää resursseja ovat esimerkiksi käytettävyys vakiorivejä aikayksikön.

#### <a name="premium"></a>Premium

Premium palvelun taso Käytä useimmiten on vähintään yksi seuraavia ominaisuuksia:

- **Suuri piikin ladata**. Suorittimen, muistin tai i (i/o) toimintojen suorittamiseen paljon edellyttävän sovelluksen tarvitaan oma, tehokkaan tasolle. Esimerkiksi tunnetut tarjoaman usean suorittimen sydämiä tavoittamattomissa tietokantatoiminnon on candidate Premium palvelun aikayksikön.
- **Monta samanaikaiset pyynnöt**. Tietokannan sovelluksia palvelun monta samanaikaiset pyynnöt, esimerkiksi käsittelevä sivusto, joka on suuri tietoliikenne-asema. Perus- ja Standard palvelutasot enimmäismäärä samanaikaiset pyynnöt tietokantaa kohden. Sovellukset edellyttävät enemmän yhteyksiä on tarvittavat varaus käsittelemiseen tarvittavat pyynnöt enimmäismäärä koon valitseminen.
- **Pieni viive**. Jotkin sovellukset on takaa tietokannan vastausta mahdollisimman vähän samanaikaisesti. Jos tietyn tallennetun toimintosarjan kutsutaan osana laajempi asiakas-toimintoa, sinun on ehkä pyyntö on enintään 20 millisekunteina 99 prosenttia tuotto puhelua. Tällaista Premium palvelutaso, varmista, että tarvittavat tietojenkäsittely power on käytettävissä sovelluksen eduista.

Palvelutaso, sinun on SQL-tietokantaan määräytyy kunkin resurssidimension piikin kuormituksen vaatimukset. Jotkin sovellukset käyttää koottuja trivial määrä, vaikka merkittäviä muita resursseja koskevat vaatimukset.

## <a name="service-tier-capabilities-and-limits"></a>Palvelun taso ominaisuudet ja rajoitukset
Service-tason ja suorituskyvyn tasoissa on liitetty eri rajoitukset ja suorituskyvyn ominaisuudet. Tässä taulukossa on kuvattu nämä ominaisuudet yhden tietokannan.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

Seuraavissa osissa on lisätietoja siitä, miten voit tarkastella käyttöön liittyviä nämä raja-arvot.

### <a name="maximum-in-memory-oltp-storage"></a>Tallennustilan enimmäismäärä ladatun OLTP

Voit valvoa Azure ladatun tallennustilan käyttö **sys.dm_db_resource_stats** -näkymä. Saat lisätietoja valvonnasta [näytön ladatun OLTP tallennustilan](sql-database-in-memory-oltp-monitoring.md).

>[AZURE.NOTE] Tällä hetkellä Azure muistissa online tapahtuman käsittely (OLTP) esikatselu tuetaan vain yhden tietokannan. Et voi käyttää sitä joustavasti tietokannan jakavat tietokannoissa.

### <a name="maximum-concurrent-requests"></a>Suurin samanaikaiset pyynnöt

Samanaikaiset pyynnöt määrä näkyviin Suorita Transact-SQL-kyselyn SQL-tietokantaan:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Jos haluat analysoida paikallisen SQL Server-tietokantaan työmäärää, muokata tätä kyselyä suodatettavan tietokannan avaamiseksi haluat analysoida. Esimerkiksi jos sinulla on nimeltä OmaTietokanta paikallisen tietokannan, Tämä Transact-SQL-kysely palauttaa samanaikaiset pyynnöt määrää tietokannan:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Tämä on vain yhden vaiheessa tilannevedoksen samanaikaisesti. Saat paremman käsityksen siitä, työmäärää ja samanaikainen pyynnön vaatimukset, sinun on monta näytteiden kerääminen ajan kuluessa.

### <a name="maximum-concurrent-logins"></a>Suurin samanaikaiset kirjautumiset

Voit analysoida käyttäjä- ja -kuvioiden saat kirjautumiset taajuus verrata. Voit myös suorittamalla tosielämän Lataa testiympäristössä varmistaaksesi, että olet olet ei pallolla tämä tai muut rajoitukset tässä artikkelissa käsiteltävät aiheet. Ei yhteen kyselyn tai dynaaminen hallinta-näkymässä (DMV), näet samanaikainen kirjautuminen laskee tai historia.

Jos useat asiakkaat käyttää samaa yhteysmerkkijonoa, palvelun todentaa kunkin kirjautuminen. Jos 10 käyttäjää muodostaa tietokannan samanaikaisesti saman käyttäjänimen ja salasanan avulla, olisi 10 samanaikaiset kirjautumiset. Tämä rajoitus koskee vain kestoa todennus ja kirjaudu sisään. Jos sama 10 käyttäjää muodostaa tietokannan järjestyksessä, samanaikaiset kirjautumiset määrän koskaan olisi suurempi kuin 1.

>[AZURE.NOTE] Tällä hetkellä tämä rajoitus ei koske joustavasti tietokannan jakavat tietokannoissa.

### <a name="maximum-sessions"></a>Istuntojen enimmäismäärä

Voit tarkastella nykyisen istuntojen määrän suorittamalla Transact-SQL-kyselyn SQL-tietokantaan:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Jos olet analysoiminen paikallisen-SQL Server työmäärää, muokkaat kyselyä voit keskittyä tiettyyn tietokantaan. Tämän kyselyn avulla voit määrittää mahdolliset istunnon tarpeiden tietokannan, jos harkitset siirtäminen Azure SQL-tietokanta.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Uudelleen nämä kyselyt palauttaa ajankohta: Laske. Jos useita näytteiden kerää ajan kuluessa, on paras tietoja istunnosta avulla.

SQL-tietokantaan analyysia varten saat historiallisten tilasto-istuntoja. Kyselyn **sys.resource_stats**ja **active_session_count** -sarakkeessa. Lisätietoja on kohdassa Lisätietoja tässä näkymässä.

## <a name="monitor-resource-use"></a>Resurssien käytön valvominen
Voit seurata resurssien suhteessa sen palvelutaso SQL-tietokanta käytöstä kaksi näkymää avulla:

- [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
- [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Voit käyttää [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) näkymän jokaisen SQL-tietokantaan. **Sys.dm_db_resource_stats** -näkymässä näkyvät viimeisimmät käyttö resurssitietojen suhteessa palvelutaso. Keskimääräinen prosentit suorittimen, tietojen i/o, loki kirjoituksia ja muistin kirjataan 15 sekunnin välein ja ylläpidetään on 1 tunti.

Koska tämä näkymä sisältää eritellympiä katsaus resurssien käyttöä, käytä **sys.dm_db_resource_stats** ensimmäisen nykyisen tilan analyysiin tai vianmääritysvaiheita. Esimerkiksi tämä kysely näkyy keskiarvo- ja resurssien käyttöä avoinna olevan tietokannan edellisen tunnin kautta:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Muut kyselyjen [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)esimerkkejä.

### <a name="sysresourcestats"></a>sys.resource_stats

**Perusmuodon** tietokannan [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) -näkymässä on lisätietoja, joiden avulla voit valvoa suorituskykyä SQL-tietokantaan tietyn palvelun taso ja suorituskyvyn tasolla. Tiedot on koottu 5 minuutin välein ja ylläpidetään 35 päivää. Tätä näkymää voi hyödyntää kahdeksalla historiallisten analyysi, miten SQL-tietokantaan käytetään resursseja.

Seuraavassa kuvassa näkyy viikon suorittimen resurssien käyttö Premium tietokantaan P2 suorituskyvyn viereiset tunnin välein. Tämä kaavio alkaa maanantaina, näyttää 5 työpäiviä ja näyttää viikonlopun, kun paljon pienempi tapahtuu, kun sovellus.

![SQL-tietokannan resurssien käyttöä](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Tämä tietokanta on tällä hetkellä vain yli 50 prosenttia huippu-Suoritin lataamista tiedoista, suorittimen käytön suhteessa P2 suorituskyvyn tason (midday-tiistai). Jos Suoritin on hallitseva kerroin sovelluksen resurssin profiilin voit päättää, että P2 on oikea suorituskyvyn taso voit varmistaa, ettei työmäärää aina kesto. Jos arvelet sovelluksen määrä kasvaa ajan myötä, on hyvä on ylimääräisiä resurssin puskurin niin, että sovellus ei koskaan saavuttaa suorituskyvyn tason rajoitus. Jos kasvatat suorituskyky, voit auttaa välttämään asiakkaan näkyvissä virheitä, joita voi ilmetä, kun tietokanta ei ole tarpeeksi virtaa Käsittele pyynnöt tehokkaasti, erityisesti viive-luottamukselliset ympäristöissä. Esimerkki on tietokantaan, joka tukee sovellus, joka maalit verkkosivuille tietokannan kutsujen tulosten perusteella.

Huomaa, että sovelluksen muuntyyppisten voi tulkita samassa kaaviossa eri tavalla. Esimerkiksi jos sovellus yrittää käsitellä palkanlaskennan tietojen päivittäin ja samassa kaaviossa, "erän työn" mallin tällaista kannattaa tehdä tietoa P1 suorituskyvyn tasolla. P1 suorituskyvyn ryhmittelytasolla on 100 DTUs verrattuna 200 DTUs P2 suorituskyvyn tasolla. P1 suorituskyvyn taso sisältää puolet P2 suorituskyvyn tason suorituskykyä. Näin on 50 prosenttia suorittimen käytön P2 vastaa arvoa 100 prosenttiin suorittimen käytön P1. Jos sovellusta ei ole aikakatkaisu, se ei ehkä merkitystä, jos työn on 2 tunnin tai 2,5 tuntia, kun haluat lopettaa, jos se saa tärkeille samana päivänä. Tähän luokkaan sovellus käyttää todennäköisesti P1 suorituskyvyn taso. Hyödynnä siitä, että on aikaa, kun resurssien käyttöä on pienempi, niin, että kaikki "suuri piikin" ehkä pienennys yhdeksi syvennyksiin myöhemmin päivä-päivän aikana. P1 suorituskyvyn taso voi olla hyvin tyyppi-sovelluksen (ja Tallenna raha), kunhan työt saadaan päätökseen ajan päivässä.

Azure SQL-tietokantaan näyttää kulutettu kunkin aktiivisen tietokantaikkunan **sys.resource_stats** -näkymässä kunkin palvelimen **perusmuodon** tietokannan tiedot. Taulukon tiedot yhdistetään 5 minuutin välein. Basic, Vakio ja Premium-palvelutasot kanssa tiedot voi kestää yli 5 minuuttia näkyvät taulukossa, joten tämä tieto on enemmän hyötyä historiallisten analyysi lähellä-reaaliajassa analyysin sijaan. Kyselyn **sys.resource_stats** tietokannan historiatietojen ja tarkista, onko varaus, jonka valitsit toimitettu suorituskyvyn haluamasi näkymä tarpeen mukaan.

>[AZURE.NOTE] Sinun on oltava yhdistettynä looginen SQL-tietokantapalvelimen kysely seuraavissa esimerkeissä **sys.resource_stats** **perusmuodon** tietokannan.

Tässä esimerkissä kerrotaan, kuinka tiedot tässä näkymässä näkyvät:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Sys.resource_stats luettelo-näkymä](./media/sql-database-performance-guidance/sys_resource_stats.png)

Seuraavassa esimerkissä kerrotaan eri tavoista, joilla voit käyttää **sys.resource_stats** luettelon näkymän saat tietoja siitä, miten SQL-tietokantaan käytetään resursseja:

1. Jotta voit tarkastella resurssin edellisen viikon käyttää tietokannan userdb1, voit suorittaa tämän kyselyn:

        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;

2. Arvioida, kuinka hyvin havainnollistamiseen sopii suorituskyky-, sinun täytyy siirtyä eri vaiheissa resurssin arvot alirakenteeseen: suorittimen, lukuja, kirjoituksia, työntekijöiden määrä ja istuntojen. Tässä on muokattu kyselyn raportin resurssin nämä arvot keskiarvo- ja suurin-arvojen **sys.resource_stats** avulla:

        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

3. Nämä ohjeet kunkin resurssin metrijärjestelmä, keskiarvo- ja suurin-arvoja voit arvioida, kuinka hyvin havainnollistamiseen esittämiseen valitsit suorituskyvyn taso. Yleensä **sys.resource_stats** keskimääräinen arvot antaa hyvä perusaikataulun käyttämään vastaan kohteen koon. Sen pitäisi olla ensisijainen mitta sauvan. Esimerkki käytät ehkä vakiorivejä taso ja S2 suorituskyvyn taso. Keskiarvo käyttää prosenttien suorittimen ja i/o lukee kirjoituksia on 40 prosentin alapuolelle, keskiarvo työntekijöiden määrä on alle 50 ja istuntojen keskimääräinen määrä on alle 200. Havainnollistamiseen ehkä mahdu S1 suorituskyvyn taso. On helppo nähdä, onko tietokannan sopii työntekijä ja istunnon rajoituksia. Näet tietokannan esittämiseen suorituskyvyn alemmalle tasolle suorittimen, määräpäiviä lukee ja kirjoituksia, Jaa suorituskyvyn suojaustasoa suorituskyvyn tasoa DTU määrällä DTU määrän ja sitten kerro tulos luvulla 100:

    * *S1 DTU / S2 DTU* 100 = 20 / 50* 100 = 40 **

    Tulos on suhteellinen suorituskyvyn ero kaksi suorituskyvyn tasojen prosentti. Jos resurssien käyttöä ei ole suurempi kuin summa-havainnollistamiseen ehkä mahtuu suorituskyvyn alemmalle tasolle. Kuitenkin haluat tarkastella kaikkien alueiden resurssien käyttö-arvojen ja määrittää prosenttiarvolla, kuinka usein tietokanta työmäärää mahdu suorituskyvyn alemmalle tasolle. Seuraava kysely tulostaa Sovita prosentti / resurssidimension, raja-arvo on 40 %, joka on laskettu tämän esimerkin mukaan:

        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Tietokannan palvelun tason tavoitteelle (Hitaissa) perusteella, voit päättää havainnollistamiseen esittämiseen suorituskyvyn alemmalle tasolle. Jos tietokannan havainnollistamiseen Hitaissa 99,9 prosenttiin ja kysely palauttaa arvot suurempi kuin kaikkien kolmen resurssin dimensioiden 99,9 prosenttiin, havainnollistamiseen todennäköisesti mahtuu suorituskyvyn alemmalle tasolle.

    Sovita prosentti katsoo myös avulla voit huomioon onko sinun on siirrettävä edistynyt suurempi suorituskyvyn oman Hitaissa mukaiseksi. Esimerkiksi userdb1 näyttää edellisen viikon seuraavat suorittimen käytön:

  	| Keskimääräinen suorittimen prosenttia | Suurin suorittimen prosenttia |
  	|---|---|
  	| 24.5 | 100,00 |

    Suorittimen keskiarvo on neljänneksellä rajoitus suorituskyvyn tason, jotka sopii hyvin tietokannan suorituskykytaso. Mutta suurimman arvon näyttää tietokannan saavuttaa suorituskyvyn tason rajoitus. Sinun on siirrettävä seuraava suorituskyvyn ylemmälle tasolle Sinun on Katso kuinka monta kertaa työmäärää saavuttaa 100 prosenttiin ja vertaa sitä tietokannan havainnollistamiseen Hitaissa.

        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent’
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Jos tämä kysely palauttaa arvon alle 99,9 prosenttiin jokin kolmesta resurssin dimensiot-joko Siirrä edistynyt suurempi suorituskyvyn tai alentaminen sovelluksen säätäminen tekniikoita kuormituksen SQL-tietokantaan.

4. Tämä Harjoitus katsoo suunniteltu työmäärä kasvaa myös myöhemmin.

## <a name="tune-your-application"></a>Paranna sovelluksen

Perinteinen paikallisen SQL Server-prosessi, jossa ensimmäinen kapasiteetin suunnittelua usein erotetaan tuotannon sovelluksen suorittaminen loppuun. Laitteiston ja tuotteen käyttöoikeudet ostetaan ensin ja suorituskyvyn säätö on valmis jälkeenpäin. Kun käytät Azure SQL-tietokantaan, on interweave sovelluksen suorittaminen ja säätäminen sen ‑toimintoa kannattaa. Maksaa kapasiteetin pyydettäessä mallia voit hienosäätää sovelluksesi voi käyttää pienin resurssit, joita tarvitaan nyt sijaan overprovisioning laitteistoon ehdotus tulevissa kasvu suunnitelmien sovelluksen, jotka ovat usein virheellinen perusteella. Asiakkaita voi valita ei paranna sovelluksen ja valitsemalla sen sijaan overprovision laitteistoresurssit. Tätä tapaa kannattaa ehkä hyvä, jos et halua muuttaa avaimen sovelluksen varattu jaksolla. Mutta säätäminen sovelluksen pienentää resurssivaatimusten ja pienempi kuukausittain laskut käytettäessä palvelutasot Azure SQL-tietokantaan.

### <a name="application-characteristics"></a>Sovelluksen ominaisuudet

Vaikka voit parantaa suorituskykyä vakauteen ja ennustettavuutta sovelluksen on suunniteltu Azure SQL-tietokanta palvelutasot, parhaista käytännöistä avulla voit hienosäätää sovelluksesi paremmin hyödyntää resurssien suorituskyvyn tasolla. Vaikka monet sovellukset yksinkertaisesti vaihtamalla siirtäminen ylemmälle tasolle suorituskyvyn merkittäviä etuja tai palvelutaso-sovelluksia on muita säätäminen hyötyvät palvelun ylemmälle tasolle. Harkitse suorituskykyä Lisää sovellus parantaminen sovelluksia, jotka on seuraavia ominaisuuksia:

- **Sovellukset, joiden vuoksi "chatty" toiminta hidasta**. Chatty sovellusten Varmista liikaa tietoja access-toiminnoista, joita verkon latenssin merkitsevä. Voit joutua muuttamaan tällaiset sovellukset voivat vähentää tiedot access-toimintoa SQL-tietokantaan. Voit esimerkiksi parantaa sovelluksen suorituskykyä tekniikoita, kuten jonottaminen ad-hoc kyselyiden tai kyselyt siirtäminen tallennettujen toimintosarjojen avulla. Lisätietoja on kohdassa [erä kyselyjen](#batch-queries).
- **Tietokantojen tehostettu työmäärää, joka ei tue koko yhteen tietokoneeseen**. Tietokannoissa, jotka ylittävät Premium suorituskyvyn parhaan resurssien hyötyä laajentaminen työmäärää. Lisätietoja on artikkelissa [rajat-tietokannan sharding](#cross-database-sharding) ja [toiminnallisia jakaminen](#functional-partitioning).
- **Sovellukset, jotka on lainkaan kyselyjä**. Sovellukset, erityisesti kaltaisia kerroksen tiedot access-, joka on huonosti virittää kyselyjen ei hyötyä suorituskyvyn ylemmälle tasolle. Tämä sisältää kyselyjen ei ole WHERE-lauseen, on indeksit puuttuu tai on vanhentunut tilastotiedot. Nämä sovellukset hyötyvät vakio kyselyn suorituskyvyn säätö avulla. Lisätietoja on artikkelissa [puuttuu indeksejä](#missing-indexes) tai [kyselyn parantaminen ja kirjasinkoista](#query-tuning-and-hinting).
- **Sovellukset, jotka on lainkaan tietoja käyttää rakenne**. Sovellukset, jotka on luontaisen tiedot access samanaikainen ongelmat, kuten deadlocking ei hyötyä suorituskyvyn ylemmälle tasolle. Harkitse pienentämisestä PYÖRISTÄ-funktiota trips Azure SQL-tietokannalle Azure välimuisti-palvelun tai toisen välimuistiin tekniikka asiakaspuolen välimuistin tiedot. Katso [sovelluksen taso välimuistiin](#application-tier-caching).

## <a name="tuning-techniques"></a>Säätö tekniikat
Tässä osassa on Tarkista joitakin tekniikoita, joiden avulla voit hienosäätää Azure SQL-tietokannan artikkelista sovelluksen parhaan suorituskyvyn ja suorita alimmalla mahdollisella tasolla. Joitain näistä menetelmistä vastaa perinteinen SQL Server säätäminen parhaita käytäntöjä, mutta muut käyttäjät ovat Azure SQL-tietokantaan. Joissakin tapauksissa voit tutkia tietokannan paranna ja laajentaa perinteinen SQL Server-tekniikoiden toimimaan Azure SQL-tietokantaan tarkemmin alueiden etsiminen kulutettu resurssit.

### <a name="azure-portal-tools"></a>Azure portaalin Työkalut
Löydät kaksi Työkalut Azure-portaalissa, joiden avulla voit analysoida ja korjata suorituskykyongelmia SQL-tietokantaan:

- [Kyselyn suorituskykyä tietoja](sql-database-query-performance.md)
- [SQL-tietokannan tukipalvelu](sql-database-advisor.md)

Azure-portaalissa on lisätietoja sekä työkaluja ja kuinka niitä käytetään. Tehokkaasti ja korjaa ongelmia, on suositeltavaa, yritä ensin Työkalut Azure-portaalissa. On suositeltavaa, että käytät säätäminen tapaa, joilla käsiteltävät aiheet seuraavaksi puuttuvien indeksit ja kyselyn säätäminen tietyissä tapauksissa manuaalinen.

### <a name="missing-indexes"></a>Puuttuvat indeksit
OLTP tietokannan suorituskykyä yleinen ongelma liittyy fyysinen tietokannan rakenteeseen. Usein tietokanta rakenteet on suunniteltu ja toimitettu ilman testaamista asteikko (joko kuormituksen tai data-asema). Valitettavasti perussuunnitelman kyselyn suorituskykyä voi hyväksyä pieni asteikolla mutta heikentää huomattavasti tuotannon tason tietomääriä-kohdassa. Tämä ongelma yleisimmät lähde on tarvittavat indeksit suodattimia tai muiden rajoitusten kyselyn puuttuminen. Puuttuvat indeksit luettelot taulukoksi Tarkista usein yhteydessä indeksi-haku voi riittää.

Tässä esimerkissä valitun kyselyn suunnitelman käyttää tarkistus, kun seek riittää:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![Kyselysuunnitelma puuttuu indeksit kanssa](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Azure SQL-tietokantaan auttavat Etsi ja korjaa yleisiä puuttuu indeksoida ehdot. DMVs, jotka ovat valmiina Azure SQL-tietokanta Tarkista kyselyn käännösten, jossa indeksin vähentää kyselyn suorittaminen arvioidut kustannukset. Kyselyn suorituksen aikana SQL-tietokantaan seuraa kuinka usein kunkin kyselyn suunnittelu on suoritettu, ja seuraa arvioitu välin suoritetaan kyselyn suunnittelu ja imagined yksi missä indeksi olemassa. Nämä DMVs avulla voit nopeasti arvata fyysinen tietokannan rakenteen muutokset voi parantaa työmäärää kokonaiskustannukset tietokannan ja sen todellinen työmäärä.

Tämän kyselyn avulla voit arvioida mahdolliset puuttuvat indeksit:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

Tässä esimerkissä kyselyn tuloksena ehdotus:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Sen luomisen jälkeen, että samassa SELECT-lauseessa valitsee jokin muu palvelupaketti, joka käyttää sen sijaan, että tarkistuksen haku ja suorittaa suunnitelman tehokkaammin:

![Kyselyn suunnittelu, korjattu indeksit kanssa](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

Avaimen tietoja siitä, että jaettu, tärkeämpää järjestelmän i/o kapasiteetti on vähemmän kuin, joka on erillinen server machine. Valitse pienentäminen tarpeettomat i/o voit hyödyntää järjestelmä-Azure SQL-tietokanta-palvelutasot suorituskyvyn kunkin tason DTU on premium. Fyysinen tietokannan rakenne vaihtoehtojen merkittävästi parantaa yksittäisiä kyselyjen viive parantaa samanaikaiset pyynnöt kohti asteikko käsitellään liikenteen ja minimoida tarvitse kyselyä kustannukset. Saat lisätietoja puuttuu hakemiston DMVs [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Kyselyn parantaminen ja kirjasinkoista
Perinteinen SQL Server-kyselyn optimointitoiminnon muistuttaa kyselyn optimointitoiminnon Azure SQL-tietokantaan. Useimmat parhaita käytäntöjä säätäminen kyselyitä ja tietoa tarkoitetun kyselyn optimointitoiminnon mallia rajoitukset koskevat myös Azure SQL-tietokantaan. Jos paranna kyselyjen Azure SQL-tietokantaan, voit saada myös hyötyä vähentää kooste resurssin vaatimukset. Sovelluksen ehkä voi suorittaa pienempi kuin untuned vastaava kustannus, koska se voidaan suorittaa suorituskyvyn alemmalla tasolla.

Esimerkki, joka on yleisiä SQL Serverin ja joka koskee myös Azure SQL-tietokanta on kuinka kyselyn optimointitoiminnon "sniffs" parametrit. Kääntämisen, aikana kyselyn optimointitoiminnon arvioi nykyisen arvon parametrin määrittääksesi, se voi luoda lisää optimaalisen kyselyn suunnittelu. Vaikka tämä strategia usein voi aiheuttaa kyselyn palvelupakettiin, joka on huomattavasti nopeampaa kuin käännetty ilman tunnetut parametriarvot suunnitelma, tällä hetkellä se toimii imperfectly sekä SQL Serverin ja Azure SQL-tietokantaan. Joskus parametri ei sniffed, ja joskus parametrin sniffed mutta luotu suunnitelma on käyttää kaikkia käytettävissä olevia parametriarvot työmäärä lainkaan. Microsoft on kyselyn vihjeitä (direktiivejä), niin, että voit määrittää palveluita Lisää tahallisesti ja ohittaa parametrin tutkinnan oletustoiminnon. Usein, jos käytät vihjeitä, voit korjata tapaukset, jossa SQL Server tai Azure SQL-tietokanta oletustoiminnan on tietyn asiakkaan kuormituksen epätäydellisiä kopioita.

Seuraavassa esimerkissä esitetään, kuinka kyselyn luo suunnitelma, joka vastaa lainkaan sekä suorituskyvyn ja Resurssivaatimusten. Tässä esimerkissä näytetään myös, jos käytät kyselyn vihje, voit pienentää kyselyn suorittaminen aika- ja vaatimukset SQL-tietokantaan:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

Asetukset-koodi Luo taulukko, jossa on Vino jakautuman. Optimaalisen kyselyn suunnitelman vaihtelee käytettävän parametri on valittuna. Suunnitelman välimuistiin toiminta ei valitettavasti aina Käännä yleisimmät parametriarvo perustuva kysely. Näin on on mahdollista välimuistin ja käyttää arvojen, myös silloin, kun jokin muu palvelupaketti ehkä paremmin suunnitelman keskimäärin lainkaan suunnitelma. Kyselyn suunnittelu luo kaksi tallennettujen toimintosarjojen, jotka ovat samanlaiset, sillä erotuksella, että kukaan määräten kyselyn vihje.

**Esimerkki, osa 1**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times to show the performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Esimerkki, osa 2**

(Suosittelemme odota vähintään 10 minuutin ennen kuin aloitat esimerkiksi osa 2 niin, että tulokset ovat eri telemetriatietojen tuloksena olevat tiedot.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Tässä esimerkissä jokaisen osan yrittää toimia Parametroitu Lisää lauseen 1 000 kertaa (luomiseen riittävästi kuormituksen käyttämään testi tiedot joukkona). Kun se suoritetaan tallennettujen toimintosarjojen, kyselyn tutkii parametriarvo, joka siirretään menettelyä sen ensimmäisen laadittaessa (parametri "tutkinnan"). Suoritin välimuistiin tuloksena suunnitelman ja käyttää sitä uudempaa ohjelmarakennekaaviossa myös jos parametriarvo ei ole. Optimaalisen suunnitelman ei voi käyttää kaikissa tapauksissa. Joskus sinun täytyy valita suunnitelma, joka on parempi keskimääräinen kirjainkokoa tapaukseen sijaan-kun kysely on ensin käännetty optimointitoiminnon opas. Tässä esimerkissä alkuperäisen suunnitelman Luo "tarkistuksen" suunnitelma, joka lukee kaikki rivit, voit etsiä kunkin arvon, joka vastaa parametrin:

![Kyselyn avulla tarkistuksen suunnitelma säätäminen](./media/sql-database-performance-guidance/query_tuning_1.png)

Koska menettelyä suorittama käyttämällä arvon 1, tuloksena olevat suunnitelman on paras mahdollinen arvo 1, mutta ei lainkaan kaikki taulukon muut arvot. Todennäköisesti tulos ei ole, haluat valita kunkin suunnitelman satunnaisesti, koska suunnitelman suorittaa hitaammin ja käyttää enemmän resursseja on.

Jos suoritat testi `SET STATISTICS IO` asettaminen `ON`, tässä esimerkissä loogisen tarkistus-työ on valmis taustalla. Näet, ettei tietoalueessa 1,148 lukee vaatii palvelupaketti (joka on tehotonta, onko keskimääräinen tapaus palauttaa vain yhden rivin):

![Kyselyn avulla looginen tarkistuksen säätäminen](./media/sql-database-performance-guidance/query_tuning_2.png)

Esimerkki toista osaa käytetään kyselyn Vihje onko optimointitoiminnon käyttämään tietyn arvon kääntämisen aikana. Pakottaa tässä tapauksessa Ohita arvo, joka on välitetty parametri, kun kyselysuoritin ja sen sijaan olettaa `UNKNOWN`. Tämä tarkoittaa arvo, joka on keskiarvo taajuus (jakauman.vinous-funktio ohittaa)-taulukossa. Tuloksena oleva suunnitelma on seek-palvelupaketti, joka on nopeampaa ja käyttää vähemmän resursseja keskimäärin palvelupaketti kuin tässä esimerkissä 1-osassa:

![Kyselyn käyttämällä kyselyn Vihje säätäminen](./media/sql-database-performance-guidance/query_tuning_3.png)

Näet **sys.resource_stats** taulukon tehoste (ei viiveen Suorita testi ja kun tietojen täyttää taulukon ajasta). Tämä esimerkki, osa 1 suorittamisen aikana 22:25:00 aikaikkunan ja osa 2 suoritetaan 22:35:00. Huomaa, että aiemmissa aikaikkunan käyttää Lisää resursseja, aika-ikkunassa kuin myöhemmin näkymän (vuoksi suunnitelman tehokkuutta parannukset).

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Kyselyn säätäminen esimerkkitulokset](./media/sql-database-performance-guidance/query_tuning_4.png)

>[AZURE.NOTE] Äänenvoimakkuuden tässä esimerkissä on tarkoituksella pieni, mutta lainkaan parametrit vaikutus voi olla huomattava, erityisesti suurten tietokantojen käyttöön. Ero erittäin tapauksissa voi olla nopea tapauksissa sekuntia ja tuntia hidas tapauksissa välillä.

Voit tutkia, voit selvittää, käyttääkö testin resurssin enemmän tai vähemmän resursseja kuin toisen testin **sys.resource_stats** . Kun vertaat tietoja, erota testien ajoituksen niin, että ne eivät ole samassa 5 minuutin-ikkunassa **sys.resource_stats** -näkymässä. Käyttöoikeuden tavoitteena on Pienennä käytettävien resurssien kokonaismäärä ja ei, jos haluat pienentää huippu-resursseja. Yleensä optimointi osan viive koodi myös pienentää resurssien käyttöä. Varmista, että sovellukseen tekemäsi muutokset ovat tarpeen ja muutoksia ei heikentää henkilölle, joka voi käyttää kyselyn vihjeitä sovelluksen asiakkaan käyttökokemusta.

Jos työmäärä on toistuva kyselyiden joukon, usein kannattaa ottaa ja vahvistaa suunnitelman valintasi optimality, koska se ohjaa isännöimiseen tietokannan tarvitaan vähintään resurssin koon yksikkö. Sen jälkeen, kun se tarkistaa ajoittain reexamine suunnitelmat voit varmistaa, että ne on heikentynyt ei. Voit lukea lisää [kyselyn vihjeitä (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Usean tietokannan sharding
Koska Azure SQL-tietokanta suoritetaan tärkeämpää laitteistoon, yhden tietokannan kapasiteetin rajat on pienempi kuin perinteisen paikallisen SQL Server-version. Jotkin asiakkaat avulla sharding tekniikoita levitä useiden tietokantojen tietokannan toimintoja, kun toimintoja ei mahdu sisään yhden tietokannan Azure SQL-tietokantaan rajoitukset. Useimmat asiakkaat, jotka käyttävät sharding tekniikat Azure SQL-tietokannan jakaminen tietonsa yhden dimension useiden tietokantojen. Tämä vaihtoehto sinun on ymmärtää OLTP-sovellusten usein suorittamaan tapahtumia, jotka koskevat vain yhden rivin tai vain tiettyjen rivien rakenteessa.

>[AZURE.NOTE] SQL-tietokanta on nyt kirjaston sharding helpottamiseksi. Lisätietoja on artikkelissa [joustavasti tietokannan asiakkaan kirjaston yleiskatsaus](sql-database-elastic-database-client-library.md).

Esimerkiksi jos tietokanta on asiakkaan nimi, tilaus ja tilauksen tiedot (kuten perinteinen Esimerkki Northwind-tietokanta, joka sisältää SQL Server), voi jaettu tiedoista useiden tietokantojen ryhmittelemällä asiakkaaseen liittyvät järjestystä ja tilauksen tiedot. Voit varmistaa, että asiakkaan tiedot ovat varmasti yhden tietokannan. Sovelluksen jakaa eri asiakkaiden tietokantoja, levittäminen tehokkaasti koko useiden tietokantojen Lataa. Sharding asiakkaat ei vain voivat välttää tietokannan kokorajoitus, mutta Azure SQL-tietokanta myös käsitellä toiminnoista, jotka ovat merkittävästi suurempi kuin rajoituksia suorituskyvyn eri tasojen, kunhan yksittäisiä tietokantojen esittämiseen sen DTU.

Tietokannan sharding ei vähentää ratkaista kooste resurssikapasiteettia, mutta se on erittäin tehokkaasti tukevat useiden tietokantojen jakaantuu erittäin suuri Solutions-sivustossa. Tietokantojen suorittamisen tasolla suorituskyvyn eri tukemaan erittäin suuri, suuri resurssivaatimusten "voimassa"-tietokantojen.

### <a name="functional-partitioning"></a>Toimintojen jakaminen
SQL Server-käyttäjien yhdistää usein yksittäinen tietokannan monia funktioita. Esimerkiksi jos sovellus on logiikan hallitsemaan varastotietoja säilön, tietokannan voi olla logiikan liittyvän varaston seuranta osto, tallennettujen toimintosarjojen ja indeksoiduista tai sattui näkymät, joka kuukausi raportoinnin hallinta. Tätä tapaa helpottaa hallinta toimintoja, kuten varmuuskopion tietokannasta, mutta se myös edellyttää, että kokoa laitteiston käsittelemään piikin Lataa kaikki funktiot sovelluksen kautta.

Jos käytät asteikko uloskuittaus-arkkitehtuuri Azure SQL-tietokantaan, on hyvä Jaa eri tietokantoihin sovelluksen toiminnot. Käyttämällä tätä tapaa kunkin sovelluksen Skaalaa erikseen. Sovellus on enemmän ilmoituksia (ja tietokannan kuormitus kasvaa), järjestelmänvalvoja voi valita riippumaton suorituskyvyn käyttöoikeustasot, jotka kukin funktio-sovelluksessa. Rajoita tämän rakenteen, sovellus voi olla suurempi kuin yhden tärkeämpää machine voit käsitellä, koska lataus on jaettu useita tietokoneissa.

### <a name="batch-queries"></a>Erän kyselyt
Sovellusten, joka käyttää tietoja käyttämällä paljon usein ad hoc-kyselyt, vastausajan merkittäviin määrä jakautuu verkkoliikennettä sovelluksen taso ja Azure SQL-tietokanta-tason välillä. Myös silloin, kun sovellus ja Azure SQL-tietokanta on samassa tietokeskuksen, verkon välinen viive ehkä voi suurentaa tietojen suuri määrä Accessin toimintojen. Voit pienentää verkon PYÖRISTÄ-funktiota trips tiedot access-toiminnot, harkitse vaihtoehto erän joko itsenäisten kyselyjen tai kääntää ne mahdollisimman tallennettujen toimintosarjojen. Jos luonnoslehtiössä kyselyt erän, voit lähettää useiden kyselyjen yhtä suuria erä yksittäisen matkan Azure SQL-tietokantaan. Voit kääntää itsenäisten kyselyjen tallennetun toimintosarjan, voi saat saman tuloksen kuin, jos ne erä. Tallennetun toimintosarjan avulla myös tutustutaan etu suurentamisesta mahdollisuuksia välimuistiin tallentamisen kyselyn palvelupaketin Azure SQL-tietokantaan, jotta voit käyttää tallennetun toimintosarjan uudelleen.

Jotkin sovellukset ovat paljon kirjoitusoikeudet. Joskus voit pienentää tietokannan yhteensä i/o kuormituksen sivustokokoelmatyyppien miten erän kirjoituksia yhdessä. Tämä on usein helposti käyttämällä eksplisiittinen tapahtumat sen sijaan, että automaattinen Vahvista tapahtumat tallennettujen toimintosarjojen ja ad hoc erissä. Katso arvio eri tapoja, joilla [Batching Azure SQL-tietokanta-sovelluksen avulla](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Voit kokeilla jonottaminen oikean mallin etsiminen Omat työmäärää. Muista ymmärtää mallin voi olla hieman erilainen tapahtumien yhdenmukaisuuden oikeudet. Löydä oikeaa työmäärää, jonka minimoi resurssien käyttö edellyttää etsiminen yhdenmukaisuutta ja suorituskyvyn valinnat oikea yhdistelmä.

### <a name="application-tier-caching"></a>Sovelluksen tason välimuisti
Tietokannan sovelluksia on luku näkyvä toiminnoista. Välimuistiin kerroksia voi pienentää tietokannan kuormitus ja mahdollisesti voi pienentää tarvitaan tukemaan Azure SQL-tietokannan käyttämällä tietokannan suorituskykytaso. [Azure Redis välimuisti](https://azure.microsoft.com/services/cache/)-ja jos luku näkyvä työmäärän, voit lukea tietoja kerran (tai ehkä sovellus tason machine sen mukaan, miten se on määritetty kerran), ja tallentaa tiedot SQL-tietokannan ulkopuolella. Näin voit pienentää tietokannan kuormituksen (suorittimen ja luku i/o), mutta ei tapahtumien yhdenmukaisuuden vaikutusta, koska luettavan välimuistista tietoja voi olla synkronoitu tietokannan tietoja. Vaikka monet sovellukset ristiriidassa jonkin tason ovat oikein, joka ei ole tosi, jotta kaikki työmäärät. Sovelluksen vaatimukset pitäisi ymmärtää täysin, ennen kuin otat välimuistiin sovelluksen taso-strategia.

## <a name="next-steps"></a>Seuraavat vaiheet

- Saat lisätietoja palvelutasot [SQL-tietokanta-asetukset ja suorituskyky](sql-database-service-tiers.md)
- Saat lisätietoja joustavasti tietokannan jakavat [joustavasti Azure-tietokanta-ryhmän ominaisuudet?](sql-database-elastic-pool.md)
- Lisätietoja suorituskyky ja joustavasti tietokannan jakavat kohdassa [milloin kannattaa harkita joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool-guidance.md)
