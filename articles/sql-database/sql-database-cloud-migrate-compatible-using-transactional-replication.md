<properties
   pageTitle="SQL-tietokannan käyttäminen tapahtumien replikoinnin siirtäminen | Microsoft Azure"
   description="Tietokannan, tuominen Microsoft Azure SQL-tietokanta-tietokannan siirto tapahtumien sallittuja"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/23/2016"
   ms.author="carlrab"/>

# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>Siirtää Azure SQL-tietokanta tapahtumien replikoinnin SQL Server-tietokantaan

> [AZURE.SELECTOR]
- [Ohjattu SSMS siirto](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [BACPAC-tiedoston vieminen](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Tuo BACPAC tiedostosta](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Tapahtumien sallittuja](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Tässä artikkelissa kerrotaan yhteensopiva SQL Server-tietokannan siirtäminen Azure SQL-tietokanta käyttämällä SQL Serverin tapahtumien replikoinnin mahdollisimman vähän käyttökatkot kanssa.

## <a name="understanding-the-transactional-replication-architecture"></a>Tietoja tapahtumien replikoinnin-arkkitehtuuri

Kun voit varmuuskopioida poistaminen tuotannon SQL Server-tietokantaan, kun siirto on käynnissä, voit käyttää SQL Serverin tapahtumien replikoinnin siirron ratkaisu. Voit käyttää tätä ratkaisua, voit määrittää Azure SQL-tietokanta tilaajat voivat paikallisen SQL Server ‑esiintymä, johon haluat siirtää. Paikalliset tapahtumien replikoinnin jakelija Synkronoi synkronoitavaksi (publisher), kun uudet tapahtumat jatkaa ilmetä paikallisen tietokannan tiedot. 

Voit myös siirtää alijoukkoa paikallisen tietokannan tapahtumien replikoinnin. Julkaisu, voit toistaa Azure SQL-tietokanta voidaan rajoittaa alijoukkoa replikoinnin estäminen tietokannan taulukoihin. Kullekin taulukolle replikoinnin estäminen/alijoukkoa rivit tai sarakkeet alijoukkoa tietojen rajoittamiseen.

Tapahtumien toistot kaikki tiedot tai rakenteeseen tehdyt muutokset näkyvät Azure SQL-tietokantaan. Kun synkronointi on valmis ja olet valmis siirtämään, muuttaa yhteysmerkkijonon sovellustesi osoittamalla Azure SQL-tietokanta. Kun tapahtumien replikoinnin vierii jää paikallisen tietokannan ja kaikkien sovellustesi osoittamalla Azure DB muutokset, voit poistaa tapahtumien replikoinnin. Azure SQL-tietokanta on nyt tuotannon järjestelmässä.

 ![SeedCloudTR kaavio](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="transactional-replication-requirements"></a>Tapahtumien replikoinnin vaatimukset

Tapahtumien replikoinnin on tekniikka valmiit ja SQL Serverin integrointi SQL Server 6.5 jälkeen. Se on, että suurin osa DBAs tietää, johon ne on kokemusta kehittynyt ja osoitettu tekniikka. [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)on nyt mahdollista määrittää paikallisen julkaisuun [tapahtumien replikoinnin tilaajan](https://msdn.microsoft.com/library/mt589530.aspx) Azure SQL-tietokanta. Hae miten se asennetaan Management Studiossa-toiminto on sama aivan kuin määrität tapahtumien replikoinnin tilaajan paikallisen palvelimessa. Tässä skenaariossa tuki tukee julkaisija ja jälleenmyyjä on vähintään yksi seuraavista SQL Server-versioista:

 - SQL Server 2016 ja yllä 
 - SQL Server 2014 SP1 CU3 ja yllä
 - SQL Server 2014 RTM CU10 ja yllä
 - SQL Server 2012 SP2 CU8 ja yllä
 - SQL Server 2012 SP3 ja yllä


> [AZURE.IMPORTANT] SQL Server Management Studiossa uusimman version avulla voit olla synkronoituja Microsoft Azure ja SQL-tietokantaan. SQL Server Management Studiossa vanhempia versioita ei voi määrittää tilaajan SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="next-steps"></a>Seuraavat vaiheet

- [SQL Server Management Studiossa uusimmassa versiossa](https://msdn.microsoft.com/library/mt238290.aspx)
- [SSDT uusimmassa versiossa](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)

## <a name="additional-resources"></a>Lisäresursseja

- [Tapahtumien sallittuja](https://msdn.microsoft.com/library/mt589530.aspx)
- [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md)
- [Transact-SQL osittain tai funktioita, joita ei tueta](sql-database-transact-sql-information.md)
- [Siirtää SQL Server-tietokantoja SQL Server siirron hallinnan käyttäminen](http://blogs.msdn.com/b/ssma/)
