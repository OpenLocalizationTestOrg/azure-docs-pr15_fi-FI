<properties
   pageTitle="SQL-Tietovarasto Varmuuskopioiden | Microsoft Azure"
   description="Lisätietoja SQL-tietovarasto valmiin tietokannan varmuuskopioita, joiden avulla voit palauttaa Azure SQL-tietovarasto palautettava tai eri maantieteellinen alue."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lakshmi1812"
   manager="barbkess"
   editor="monicar"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="lakshmir;barbkess"/>

# <a name="sql-data-warehouse-backups"></a>SQL-tietovarasto varmuuskopiointi

SQL-tietovarasto on osana tietojen varasto varmuuskopion ominaisuuksista sekä paikallisen ja maantieteelliset varmuuskopiot. Näitä ovat Azure Blob-objektien tallennustilaan tilannevedoksia ja geo ylimääräinen. Data warehouse varmuuskopioiden avulla voit palauttaa data warehouse palauttaminen pisteeseen ensisijainen alueen tai palauttaa eri maantieteellinen alue. Tässä artikkelissa kerrotaan SQL-Tietovarasto Varmuuskopioiden yksityiskohtia.

## <a name="what-is-a-data-warehouse-backup"></a>Mikä on data warehouse varmuuskopion?

Data warehouse varmuuskopiointi on tiedot, joiden avulla voit palauttaa tiedot varasto tietyn ajan kuluessa.  SQL-tietovarasto on jaettu järjestelmän, data warehouse varmuuskopion koostuu useita tiedostoja, jotka on tallennettu Azure BLOB-objektit. 

Tietokannan varmuuskopiointi ovat kaikki liiketoiminnan jatkuvuuden ja tietojen palauttamisen strategia olennainen osa, koska tietojen suojaaminen vahingossa vioittumisen tai poistaminen. Lisätietoja on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](../sql-database/sql-database-business-continuity.md).

## <a name="data-redundancy"></a>Tietojen kopion

SQL-tietovarasto suojaa tietojen tiedot tallennetaan paikallisesti tarpeettomat (LRS) Azure Premium tallennustilaan. Azuren tallennustilaan toiminto tallentaa tiedot synkronisen useita kopioita paikallisen tietokeskuksen taata läpinäkyvä tietojen suojaaminen, jos on lokalisoitu virheet. Tietojen kopion varmistaa, että tietojen hengissä infrastruktuurin ongelmat, kuten levyn virheet. Tietojen kopion varmistaa Liiketoiminnan jatkuvuus vikasietoisia ja käytettävyyden infrastruktuurin.

Lisätietoja:

- Azure Premium tallennuskiintiön, Lue lisää [ohjeaiheista johdanto Azure Premium tallennustilan](../storage/storage-premium-storage.md).
- Paikallisesti tarpeettomat tallennus-kohdassa [Azuren tallennustilaan replikoinnin](../storage/storage-redundancy.md#locally-redundant-storage).


## <a name="azure-storage-blob-snapshots"></a>Azure Storage Blob tilannevedokset

SQL-tietovarasto käyttää kuin käyttämällä Azure Premium tallennustilan etuna, Azure Blob-objektien tallennustilaan tilannevedoksia Varmuuskopioi tietovarasto paikallisesti. Voit palauttaa tiedot varasto tilannevedoksen palauttaminen pisteeseen. Tilannevedoksia aloittaminen vähintään kahdeksan tunnin välein, ja se voi käyttää seitsemän päivän ajan.  

Lisätietoja:

- Azure-blob-tilannevedoksia, katso [blob tilannevedoksen luominen](../storage/storage-blob-snapshots.md).


## <a name="geo-redundant-backups"></a>GEO tarpeettomat varmuuskopiointi

24 tunnin välein SQL-tietovarasto tallentaa koko tietovarasto vakio säilöön. Koko tietovarasto luodaan vastaamaan viimeisen tilannevedoksen aika. Vakio tallennustilan kuuluu geo ylimääräinen-tili, joilla on lukuoikeudet (RA GRS). Azure-tallennustilan RA-GRS-ominaisuus kopioi [pisteparin tietokeskuksen](../best-practices-availability-paired-regions.md)varmuuskopiot. Tämän geo replikoinnin varmistaa, voit palauttaa tietovarasto siltä varalta, että et voi käyttää tilannevedosten ensisijainen alueellasi. 

Tämä toiminto on oletusarvoisesti. Jos et halua käyttää geo tarpeettomat varmuuskopioita, voit valita. 

>[AZURE.NOTE] Azuren tallennustilaan termin *replikoinnin* viittaa tiedostojen kopioiminen sijainnista toiseen. Käyttäjän SQL- *tietokannan replikoiminen* viittaa pitäminen useita toissijainen tietokantoja, jotka on synkronoitu ensisijaisen tietokannan kanssa. 

Lisätietoja:
- GEO tarpeettomat tallennuspaikkaa, katso [Azuren tallennustilaan replikoinnin](../storage/storage-redundancy.md).
- RA GRS tallennuspaikkaa, katso [lukuoikeudet geo tarpeettomat tallennustilan](../storage/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Data warehouse aikataulun ja säilytysaika

SQL-tietovarasto Luo tilannevedoksia online-tietoihin varastoja aina neljän kahdeksan tuntia ja säilyttää kunkin tilannevedoksen seitsemän päivän ajan. Voit palauttaa online-tietokannan palauttaminen pisteen viimeisten seitsemän päivän aikana. 

Näet viimeksi tilannevedoksen käynnistyessään, suorittaa kysely online SQL-tietovarasto. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Jos haluat säilyttää tilannevedoksen yli seitsemän päivän aikana, voit palauttaa Palauta-kohdassa uusi tietovarasto. Kun palautus on valmis, SQL-tietovarasto käynnistyy luomalla uuden tietovarasto tilannevedoksia. Jos uusi tietovarasto ei tehdä muutoksia, tilannevedosten pysy tyhjä ja siksi tilannevedoksen kustannus on pienin. Tietokannan pitämään SQL-tietovarasto luomasta tilannevedoksia voi keskeyttää myös.


### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a>Mitä tapahtuu Omat varmuuskopion säilytys Omat tietovarasto keskeytettynä?

SQL-tietovarasto ei voi luoda tilannevedoksia ja ei vanhene tilannevedoksia data warehouse ollessa keskeytettynä. Tilannevedoksen ikä eivät muutu tietovarasto ollessa keskeytettynä. Tilannevedoksen säilytys perustuu tietovarasto ei ole vuorokauden online-tilassa, päivien määrän.

Jos tietovarasto on keskeytetty lokakuussa 3 4 pm tilannevedoksen alkaa lokakuussa 1 4 pm, tilannevedoksen on kaksi päivää. Aina, kun tietovarasto kirjautuu takaisin tilannevedoksen on kaksi päivää. Jos tietovarasto kirjautuu lokakuussa 5 4 kello, tilannevedoksen kaksi päivää vanhan ja pysyy viisi päivää.

Tietovarasto tullessa online-tilaan SQL-tietovarasto säilyy uusi tilannevedoksia ja päättyy tilannevedoksia, kun niissä on yli seitsemän päivää tiedot.

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a>Kuinka kauan on poistetusta tietovarasto säilytysaika?
Kun tietojen varasto on katkaistu, tietovarasto ja tilannevedosten tallennettu seitsemän päivän ajan ja sitten poistettu. Voit palauttaa tietovarasto kaikkiin tallennetun palauttaminen asioista.

> [AZURE.IMPORTANT] Jos poistat looginen SQL server-esiintymän, kaikki tietokannat, jotka kuuluvat esiintymä poistetaan eikä sitä voi palauttaa. Et voi palauttaa poistettuja palvelimeen.

## <a name="data-warehouse-backup-costs"></a>Tietoja varaston varmuuskopion kustannukset

Ensisijainen tietovarasto ja Azure-Blob-tilannevedoksia seitsemän päivän kokonaiskustannukset pyöristetään lähimpään Teratavua. Esimerkiksi jos data warehouse on 1,5 TT ja tilannevedosten käytetään 100 Gigatavua, sinun on laskuttaa tietojen Azure Premium tallennustilan tahdissa 2 Teratavua. 

>[AZURE.NOTE] Kunkin tilannevedoksen on alun perin tyhjä ja kasvaa, kun teet muutoksia ensisijainen tietovarasto. Koko kasvaa kaikki tilannevedoksia data warehouse muutoksina. Tämän vuoksi tilannevedoksia tallennustilan kustannuksiin kasvaa muuta suuruuden mukaan.

Jos käytössäsi on geo ylimääräinen, saat eri tallennustilan varausta. Geo tarpeettomat tallennustilan laskutetaan Normaalikorvaus lukuoikeudet maantieteellisesti tarpeettomat Storage (RA GRS)-palvelussa.

Katso lisätietoja SQL-tietovarasto hinnoittelua [SQL Data Warehouse hinnoittelua](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Käyttämällä tietokannan varmuuskopiointi

Ensisijainen Käytä SQL data warehouse varmuuskopioiden hakeminen ei palauta tietovarasto jonkin palauttaminen säilytysajan kuluessa.  

- Palauttaa SQL-tietovarasto, on artikkelissa [palauttaa SQL-tietovarasto](sql-data-warehouse-restore-database-overview.md).


## <a name="related-topics"></a>Aiheeseen liittyviä ohjeita

### <a name="scenarios"></a>Skenaariot

- Liiketoiminnan jatkuvuus yleiskatsaus Katso [liiketoiminnan jatkuvuuden yhteenveto](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

- Palauttaa tietovarasto, on artikkelissa [SQL-tietovarasto palauttaminen](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

