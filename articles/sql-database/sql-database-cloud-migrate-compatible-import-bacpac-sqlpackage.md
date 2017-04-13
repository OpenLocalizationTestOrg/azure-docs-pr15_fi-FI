<properties
   pageTitle="SQL-tietokantaan SqlPackage BACPAC tiedoston tuominen"
   description="Microsoft Azure SQL-tietokanta-tietokannan siirto-tietokannan tuominen, tuo BACPAC tiedoston sqlpackage"
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
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="import-to-sql-database-from-a-bacpac-file-using-sqlpackage"></a>SQL-tietokantaan SqlPackage BACPAC tiedoston tuominen

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure portal](sql-database-import.md)
- [PowerShellin](sql-database-import-powershell.md)

Tässä artikkelissa kerrotaan, miten tuodaan SQL-tietokanta-apuohjelmalla [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) komentorivin [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) tiedostosta. Tämän apuohjelman mukana [SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx) ja [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)uusinta versiota tai voit ladata uusimman version [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) suoraan Microsoft download Centeristä.


> [AZURE.NOTE] Seuraavissa vaiheissa oletetaan on jo valmisteltu SQL-tietokantapalvelimeen, yhteystiedot on valmiina ja olet varmistanut, että lähdetietokanta on yhteensopiva.

## <a name="import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage"></a>Azure SQL-tietokanta SqlPackage BACPAC tiedoston tuominen

Seuraavien vaiheiden avulla voit tuoda yhteensopiva SQL Server-tietokantaan (tai Azure SQL-tietokantaan) [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) komentorivin-apuohjelman avulla BACPAC-tiedostosta.

> [AZURE.NOTE] Seuraavissa vaiheissa oletetaan, että on jo valmisteltu Azure SQL-tietokanta-palvelin ja yhteystiedot on valmiina.

1. Avaa komentorivi-ikkuna ja muuta hakemistona sqlpackage.exe komentorivin apuohjelma - apuohjelma mukana Visual Studio ja SQL Server.
2. Suorita seuraavat sqlpackage.exe komento, jossa on seuraavat argumentit-ympäristössä:

    `sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`

  	| Argumentti  | Kuvaus  |
  	|---|---|
  	| < palvelimen_nimi >  | kohde-palvelimen nimi  |
  	| < Tietokannan_nimi >  | kohteen tietokannan nimi  |
  	| < käyttäjätunnus >  | Kohdepalvelimen käyttäjänimi |
  	| < salasana >  | käyttäjän salasana  |
  	| < source_file >  | tiedostonimi ja sijainti BACPAC tuotavassa tiedostossa  |

    ![Vie tiedot taso-sovelluksen tehtävät-valikosta](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- [SSDT uusimmassa versiossa](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studiossa uusimmassa versiossa](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md)
- [Transact-SQL osittain tai funktioita, joita ei tueta](sql-database-transact-sql-information.md)
- [Siirtää SQL Server-tietokantoja SQL Server siirron hallinnan käyttäminen](http://blogs.msdn.com/b/ssma/)
