<properties
   pageTitle="Korjaa SQL Serverin tietokantaan yhteensopivuusongelmat käyttäen SQL Server Management Studiossa ennen siirtoa SQL-tietokantaan | Microsoft Azure"
   description="Microsoft Azure SQL-tietokanta-tietokannan siirto, yhteensopivuuden, ohjattu SQL Azure-siirto"
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

# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>Korjaa SQL Serverin tietokantaan yhteensopivuusongelmat käyttäen SQL Server Management Studiossa ennen siirtoa SQL-tietokantaan

> [AZURE.SELECTOR]
- [SQL Azure siirron](sql-database-cloud-migrate-fix-compatibility-issues.md) ohjatulla
- Käytä [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Käytä [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

Kokeneille käyttäjille voit korjata SQL Serverin tietokantaan yhteensopivuusongelmat käyttäen SQL Server Management Studiossa ennen siirtoa Azure SQL-tietokantaan.


> [AZURE.IMPORTANT] On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="using-sql-server-management-studio"></a>SQL Server Management Studiossa

SQL Server Management Studiossa avulla voit korjata yhteensopivuusongelman käyttämällä Transact-SQL-komentoja, kuten **Muuta TIETOKANTAA**. Tämä menetelmä ensisijaisesti vain kokeneille käyttäjille, jotka ovat perehtynyt työstävät Transact-SQL live tietokannan. Muussa tapauksessa kannattaa käyttää SSDT. 



## <a name="next-steps"></a>Seuraavat vaiheet

- [SSDT uusimmassa versiossa](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studiossa uusimmassa versiossa](https://msdn.microsoft.com/library/mt238290.aspx)
- [Yhteensopivat SQL Server-tietokannan siirtäminen SQL-tietokantaan](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md)
- [Transact-SQL osittain tai funktioita, joita ei tueta](sql-database-transact-sql-information.md)
- [Siirtää SQL Server-tietokantoja SQL Server siirron hallinnan käyttäminen](http://blogs.msdn.com/b/ssma/)
