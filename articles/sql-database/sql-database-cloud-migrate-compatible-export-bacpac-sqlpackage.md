<properties
   pageTitle="SQL Server-tietokantaan vieminen BACPAC tiedoston SqlPackage | Microsoft Azure"
   description="Microsoft Azure SQL-tietokanta-tietokannan siirto vieminen tietokantaan, vie BACPAC tiedoston sqlpackage"
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

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sqlpackage"></a>Vie BACPAC-tiedoston SqlPackage SQL Server-tietokantaan

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

Tässä artikkelissa kerrotaan, miten SQL Server-tietokantaan vieminen [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) komentorivin apuohjelmalla [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) tiedostoon. Tämän apuohjelman mukana [SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx) ja [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)uusinta versiota tai voit ladata uusimman version [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) suoraan Microsoft download Centeristä.

1. Avaa komentorivi-ikkuna ja muuta hakemistona sqlpackage.exe komentorivin apuohjelma - apuohjelma mukana Visual Studio ja SQL Server. Etsi polku ympäristössäsi tietokoneeseen haun avulla.
2. Suorita seuraavat sqlpackage.exe komento, jossa on seuraavat argumentit-ympäristössä:

    "sqlpackage.exe /Action:Export /ssn: < palvelimen_nimi > /sdn: < Tietokannan_nimi > /tf: < target_file >

  	| Argumentti  | Kuvaus  |
  	|---|---|
  	| < palvelimen_nimi >  | lähde-palvelimen nimi  |
  	| < Tietokannan_nimi >  | tietolähteen tietokannan nimi  |
  	| < target_file >  | tiedostonimi ja sijainti BACPAC  |

    ![Vie tiedot taso-sovelluksen tehtävät-valikosta](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- [SSDT uusimmassa versiossa](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studiossa uusimmassa versiossa](https://msdn.microsoft.com/library/mt238290.aspx)
- [Tuo BACPAC Azure SQL-tietokanta SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Tuo BACPAC Azure SQL-tietokannan SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Tuo BACPAC Azure SQL-tietokannan Azure-portaaliin](sql-database-import.md)
- [Tuo BACPAC PowerShellin Azure SQL-tietokanta](sql-database-import-powershell.md)

## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md)
- [Transact-SQL osittain tai funktioita, joita ei tueta](sql-database-transact-sql-information.md)
- [Siirtää SQL Server-tietokantoja SQL Server siirron hallinnan käyttäminen](http://blogs.msdn.com/b/ssma/)
