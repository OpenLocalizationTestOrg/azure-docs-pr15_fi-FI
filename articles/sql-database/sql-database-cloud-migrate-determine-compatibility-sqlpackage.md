<properties
   pageTitle="Määrittää SQL-tietokantaan yhteensopivuuden käyttämällä SqlPackage.exe | Microsoft Azure"
   description="Microsoft Azure SQL-tietokanta-tietokannan siirto, SQL-tietokanta-yhteensopivuus-SqlPackage"
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

# <a name="determine-sql-database-compatibility-using-sqlpackageexe"></a>Määrittää SQL-tietokantaan yhteensopivuuden SqlPackage.exe käyttäminen

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Päivityksen tukipalvelu](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

Tässä artikkelissa kerrotaan, onko yhteensopiva [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) komentorivi-apuohjelmalla SQL-tietokannan siirtäminen SQL Server-tietokantaan.

## <a name="using-sqlpackageexe"></a>SqlPackage.exe käyttäminen

1. Avaa komentorivi-ikkuna ja muuta hakemistona sqlpackage.exe uusin versio. Tämän apuohjelman mukana [SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx) ja [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)uusinta versiota tai voit ladata uusimman version [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) suoraan Microsoft download Centeristä.
2. Suorita seuraavat SqlPackage komento, jossa on seuraavat argumentit-ympäristössä:

    "sqlpackage.exe /Action:Export /ssn: < palvelimen_nimi > /sdn: < Tietokannan_nimi > /tf: < target_file > /p:TableData = < schema_name.table_name >>< tulostetiedosto > 2 > & 1

  	| Argumentti  | Kuvaus  |
  	|---|---|
  	| < palvelimen_nimi >  | lähde-palvelimen nimi  |
  	| < Tietokannan_nimi >  | tietolähteen tietokannan nimi  |
  	| < target_file >  | tiedostonimi ja sijainti BACPAC  |
  	| < schema_name.table_name >  | taulukot, jonka tiedot ovat tulosteen kohdetiedosto  |
  	| < tulostetiedosto >  | tiedostonimi ja sijainti virheitä, jos sellainen on kohdetiedosto  |

    /P:TableName-argumentin syy on, että vain haluat testata tietokannan yhteensopivuuden Azure SQL DB Vipuventtiili V12 viedä sen sijaan, että tietojen vieminen kaikki taulukot. Valitettavasti sqlpackage.exe Vie-argumentti ei tue puretaan nolla taulukot. Sinun täytyy määrittää vähintään yhden taulukon, kuten yhden pienen taulukon. < Tulostetiedosto > sisältää raportin virheet. "> 2 > & 1" merkkijonon piiput vakio tulos ja johtuvat komennon suorituksen määritetyn kohdetiedosto keskivirhe.

    ![Vie tiedot taso-sovelluksen tehtävät-valikosta](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. Avaa kohdetiedosto ja tarkista, yhteensopivuusvirheet, jos sellainen on. 

    ![Vie tiedot taso-sovelluksen tehtävät-valikosta](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- [SSDT uusimmassa versiossa](https://msdn.microsoft.com/library/mt204009.aspx)
[SQL Server Management Studiossa uusimmassa versiossa](https://msdn.microsoft.com/library/mt238290.aspx)
- [Korjaa tietokanta siirron yhteensopivuusongelmat](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Yhteensopivat SQL Server-tietokannan siirtäminen SQL-tietokantaan](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md)
- [Transact-SQL osittain tai funktioita, joita ei tueta](sql-database-transact-sql-information.md)
- [Siirtää SQL Server-tietokantoja SQL Server siirron hallinnan käyttäminen](http://blogs.msdn.com/b/ssma/)
