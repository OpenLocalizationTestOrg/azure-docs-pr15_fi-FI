<properties
   pageTitle="Luo SQL-tietovarasto TSQL | Microsoft Azure"
   description="Opettele luomaan Azure SQL-tietovarasto TSQL kanssa"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>SQL-tietovarasto tietokannan luominen käyttämällä Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShellin](sql-data-warehouse-get-started-provision-powershell.md)

Tässä artikkelissa kerrotaan, miten voit luoda SQL-tietovarasto käyttämällä T-SQL.

## <a name="prerequisites"></a>Edellytykset

Aluksi sinun on: 

- **Azure-tili**: Siirry [Azure maksuttoman kokeiluversion][] tai [MSDN Azure hyvitykset][] Luo tili.
- **Azure SQL server**: Katso lisätietoja [luominen Azure SQL-tietokanta looginen server Azure-portaalissa][] - tai [Azure SQL-tietokanta looginen palvelimen PowerShellin avulla][] .
- **Resurssiryhmä**: käyttää samaa resurssiryhmä Azure SQL server tai Katso, [miten voit luoda resurssiryhmän][].
- **Ympäristön suorittamaan T-SQL**: Voit käyttää [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][]tai [SSMS][] T-SQL suorittamiseen.

> [AZURE.NOTE] SQL-tietovarasto luominen voi johtaa uuden laskutettavan palvelun.  Katso lisätietoja hinnoittelua [SQL-tietovarasto hinnoittelua][] .

## <a name="create-a-database-with-visual-studio"></a>Tietokannan luominen Visual Studiossa

Jos ole ennen käyttänyt Visual Studiossa, tutustu ohjeartikkeliin [Kyselyn Azure SQL-tietovarasto (Visual Studio)][].  Voit aloittaa, Avaa SQL Server Object Explorerissa Visual Studiossa ja muodostaa yhteyttä palvelimeen, joka isännöi SQL-tietovarasto tietokannan.  Kun yhteys on muodostettu, voit luoda SQL-tietovarasto suorittamalla seuraava SQL-komento **perusmuodon** tietokannalle.  Tämä komento luo tietokannan MySqlDwDb, jossa Service tavoitteen, DW400 ja Salli tietokannan koko on enintään 10 TT kasvaa.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Tietokannan luominen sqlcmd kanssa

Vaihtoehtoisesti voit suorittaa saman komennon sqlcmd kanssa suorittamalla seuraavat komentokehotteeseen.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Kun ei ole määritetty Oletuslajittelu on LAJITTELE SQL_Latin1_General_CP1_CI_AS.  `MAXSIZE` Voi olla – 250 gt 240 Teratavua.  `SERVICE_OBJECTIVE` DW100 ja DW2000 välillä voi olla [DWU][].  Kaikki kelvolliset arvot luettelo-tietokannan [Luominen][]ohjeissa MSDN.  Voit muuttaa [Muuta TIETOKANTAA][] -T-SQL-komennolla MAXSIZE ja SERVICE_OBJECTIVE.  Tietokannan lajittelua ei voi muuttaa luomisen jälkeen.   Varoitus käytetään, kun muuttuva DWU SERVICE_OBJECTIVE vaihtaminen aiheuttaa uudelleenkäynnistyksen Services, joka peruuttaa lennon kaikki kyselyt.  Muuttaminen MAXSIZE ei uudelleen services ennalleen vain yksinkertaisia metatiedot-toimintoa.

## <a name="next-steps"></a>Seuraavat vaiheet

Kun SQL-tietovarasto on päättynyt valmistelun olet voit [ladata mallitiedot][] tai tarkistaa, kuinka voit [kehittää][], [ladata][]tai [siirtää][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Kyselyn Azure SQL-tietovarasto (Visual Studiossa)]: sql-data-warehouse-query-visual-studio.md
[siirtää]: sql-data-warehouse-overview-migrate.md
[kehittäminen]: sql-data-warehouse-overview-develop.md
[lataaminen]: sql-data-warehouse-overview-load.md
[Lataa mallitiedot]: sql-data-warehouse-load-sample-databases.md
[Luo looginen Azure SQL-tietokanta-server Azure-portaalissa]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Luo looginen Azure SQL-tietokanta-palvelimen PowerShellin avulla]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Resurssiryhmä luominen]: ../resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[SQLCMD]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[TIETOKANNAN LUOMINEN]: https://msdn.microsoft.com/library/mt204021.aspx
[MUUTTAA TIETOKANTAA]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL-tietovarasto hinnat]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure ilmainen kokeiluversio]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure hyvitykset]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
