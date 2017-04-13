<properties
   pageTitle="Korjaa SQL Server-tietokannan yhteensopivuusongelmat ennen siirtoa SQL-tietokantaan | Microsoft Azure"
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

# <a name="use-sql-azure-migration-wizard-to-fix-sql-server-database-compatibility-issues-before-migration-to-azure-sql-database"></a>SQL Azure siirron ohjatun toiminnon avulla voit korjata SQL Server-tietokannan yhteensopivuusongelmat ennen siirtoa Azure SQL-tietokantaan

> [AZURE.SELECTOR]
- [SQL Azure siirron](sql-database-cloud-migrate-fix-compatibility-issues.md) ohjatulla
- Käytä [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Käytä [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

Tässä artikkelissa kerrotaan, voit etsiä ja korjata SQL Serverin tietokantaan yhteensopivuusongelmat ohjatulla SQL Azure siirron ennen siirtoa Azure SQL-tietokantaan.

## <a name="using-sql-azure-migration-wizard"></a>SQL Azure-siirron ohjatulla toiminnolla

[Ohjattu SQL Azure-siirto](http://sqlazuremw.codeplex.com/) CodePlex-työkalun avulla voit luoda T-SQL-komentosarja ei ole yhteensopiva lähde-tietokannasta. Tämä komentosarja sitten muunnetaan ohjatussa yhteensopivaksi SQL-tietokantaan. Valitse muodostaa Azure SQL-tietokanta komentosarjan suorittaminen. Tämä työkalu analysoi myös jäljitystiedostoja, voit selvittää yhteensopivuusongelmat. Komentosarja voi luoda vain rakenteen käyttäminen tai lisätä tiedot BCP-muodossa. Lisäohjeita, mukaan lukien vaiheittaiset ohjeet on käytettävissä CodePlex [Ohjattu SQL Azure-siirto](http://sqlazuremw.codeplex.com/)-palvelussa.  

 ![SAMW siirron kaavio](./media/sql-database-cloud-migrate/02SAMWDiagram.png)

  > [AZURE.NOTE] Kaikki ei ole yhteensopiva rakenne, joka havaita ohjatussa voit vahvistaa sen valmiin muunnoksia. Ei ole yhteensopiva komentosarja, joka on osoitettu raportoidaan virheinä, kommentit, jotka on luotu komentosarja ne. Jos monta virheitä löytyy, käytä Visual Studiossa tai SQL Server Management Studiossa käydä läpi ja korjaa ei voitu korjata käyttämällä ohjattua SQL Server-siirron kunkin virheen.

## <a name="next-steps"></a>Seuraavat vaiheet

- [SSDT uusimmassa versiossa](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studiossa uusimmassa versiossa](https://msdn.microsoft.com/library/mt238290.aspx)
- [Yhteensopivat SQL Server-tietokannan siirtäminen SQL-tietokantaan](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md)
- [Transact-SQL osittain tai funktioita, joita ei tueta](sql-database-transact-sql-information.md)
- [Siirtää SQL Server-tietokantoja SQL Server siirron hallinnan käyttäminen](http://blogs.msdn.com/b/ssma/)
