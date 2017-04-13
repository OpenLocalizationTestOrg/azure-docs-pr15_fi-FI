<properties
   pageTitle="SQL-tietovarasto palauttaminen | Microsoft Azure"
   description="Tietokannan palauttaminen asetukset palautetaan Azure SQL-tietovarasto tietokannan yleiskatsaus."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/29/2016"
   ms.author="lakshmir;barbkess;sonyama"/>


# <a name="sql-data-warehouse-restore"></a>SQL-tietovarasto palauttaminen

> [AZURE.SELECTOR]
- [Yleiskatsaus][]
- [Portal][]
- [PowerShellin][]
- [MUILLE KÄYTTÄJILLE][]

SQL-tietovarasto on sekä paikallisen ja maantieteelliset palauttaa osana sen data warehouse tietojen palauttaminen ominaisuuksia. Palauttaa data warehouse palauttaminen pisteeseen ensisijainen alueen data warehouse varmuuskopioiden avulla tai geo tarpeettomat varmuuskopioiden avulla voit palauttaa eri maantieteellinen alue. Tässä artikkelissa kerrotaan palauttaminen data warehouse yksityiskohtia.

## <a name="what-is-a-data-warehouse-restore"></a>Mikä on data warehouse palautuksen?

Data warehouse palautuksen on uusi tietovarasto, joka on luotu varmuuskopiosta olemassa olevan tai poistetun tietovarasto. Palautetun tietovarasto Luo uudelleen varmuuskopioidut tietovarasto tiettynä ajankohtana. SQL-tietovarasto on jaettu järjestelmän, data warehouse palautuksen luodaan useita varmuuskopion tiedostoja, jotka on tallennettu Azure BLOB-objektit. 

Tietokannan palauttaminen on olennainen osa minkä tahansa liiketoiminnan jatkuvuuden ja tietojen palauttamisen strategia, koska se luo tiedot uudelleen vahingossa vioittumisen tai poistamisen jälkeen.

Lisätietoja on artikkelissa:

-  [SQL-tietovarasto varmuuskopiointi](sql-data-warehouse-backups.md)
-  [Liiketoiminnan jatkuvuus yhteenveto](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Varaston palauttaminen arvopisteet

SQL-tietovarasto käyttää kuin käyttämällä Azure Premium tallennustilan etuna, Azure Blob-objektien tallennustilaan tilannevedoksia Varmuuskopioi ensisijainen tietovarasto. Kunkin tilannevedoksen on Palauta-kohdassa, joka edustaa tilannevedoksen aloitusaika. Palauttaa tietovarasto, valitse Palauta-kohdassa ja palauttaminen-komennon.  

SQL-tietovarasto palauttaa varmuuskopio aina uuden tietovarasto. Voit pitää palautettu tietovarasto ja nykyisen rivin tai poistaa ne. Jos haluat korvata nykyisen tietovarasto palautettu tietovarasto, voit nimetä sen uudelleen.

Jos haluat palauttaa poistetun tai keskeytetty tietovarasto, voit [luoda tuki lippu](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a>GEO tarpeettomat palauttaminen

Jos käytät geo tarpeettomat tallennustilaa, voit palauttaa tietovarasto [pisteparin tietokeskuksen](../best-practices-availability-paired-regions.md) eri maantieteellisen alueen. Tietovarasto palautetaan viimeisen päivän varmuuskopiosta. 

## <a name="restore-timeline"></a>Palauttaa aikajana

Voit palauttaa tietokannan mihin tahansa kohtaan käytettävissä palauttaminen viimeisten seitsemän päivän aikana. Tilannevedoksia Käynnistä aina neljän kahdeksan tuntia ja ovat käytettävissä seitsemän päivän ajan. Kun tilannevedoksen on yli seitsemän päivää vanhoja, se päättyy ja sen palauttaminen-kohta ei ole enää käytettävissä.

## <a name="restore-costs"></a>Palauttaa kustannukset

Palautetun tietovarasto tallennustilan maksu laskutetaan Azure Premium tallennustilan määrä. 

Jos viet hiiren palautetulle tietovarasto, perittävän tallennustilan Azure Premium tallennustilan määrä. Keskeyttäminen etuna on se ei peri DWU tietojenkäsittely resurssien.

Katso lisätietoja SQL-tietovarasto hinnat [SQL Data Warehouse hinnat](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>Palauta käyttötapoja

Varaston palautettavaksi ensisijainen käytetään, kun tietojen palauttaminen jälkeen tietoja vahingossa menetys tai vaurio.

Voit käyttää myös tietojen varasto palauttaminen säilyttää varmuuskopion yli seitsemän päivää. Kun varmuuskopio on palautettu, sinulla on tietovarasto verkossa ja Pysäytä se jatkuvasti, jos haluat tallentaa Laske kustannukset. Keskeytetty tietokannan veloitetaan tallennustilan Azure Premium tallennustilan määrä. 

## <a name="related-topics"></a>Aiheeseen liittyviä ohjeita

### <a name="scenarios"></a>Skenaariot

- Liiketoiminnan jatkuvuus yleiskatsaus Katso [liiketoiminnan jatkuvuuden yhteenveto](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

Data warehouse palauttamisen suorittamiseen palauttaminen avulla:

- Azure-portaaliin, katso [palauttaa tietovarasto, Azure-portaalissa](sql-data-warehouse-restore-database-portal.md)
- PowerShellin cmdlet-komennot, katso [palauttaa tietovarasto, käyttämällä PowerShell cmdlet-komennot](sql-data-warehouse-restore-database-powershell.md)
- VIE ohjelmointirajapinnan on artikkelissa [palauttaa tietovarasto, REST-ohjelmointirajapinnan käyttäminen](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Yleiskatsaus]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShellin]: ./sql-data-warehouse-restore-database-powershell.md
[MUILLE KÄYTTÄJILLE]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
