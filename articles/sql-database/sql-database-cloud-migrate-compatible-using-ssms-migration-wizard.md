<properties
   pageTitle="SQL Server-tietokannan siirtäminen SQL-tietokannan käyttäminen Microsoft Azure-tietokannan käyttöönotto tietokannan | Microsoft Azure"
   description="Microsoft Azure SQL-tietokanta-tietokannan siirtäminen, ohjattu Microsoft Azure-tietokannan luominen"
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

# <a name="migrate-sql-server-database-to-sql-database-using-deploy-database-to-microsoft-azure-database-wizard"></a>Siirtää SQL Server-tietokannan käyttäminen Microsoft Azure-tietokannan käyttöönotto tietokannan SQL-tietokantaan


> [AZURE.SELECTOR]
- [Ohjattu SSMS siirto](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [BACPAC-tiedoston vieminen](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Tuo BACPAC tiedostosta](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Tapahtumien sallittuja](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

SQL Server Management Studiossa ohjatussa Microsoft Azure-tietokannan käyttöönotto tietokannan siirtää [yhteensopiva SQL Server-tietokantaan](sql-database-cloud-migrate.md) suoraan Azure SQL-tietokanta-palvelimeen.

## <a name="use-the-deploy-database-to-microsoft-azure-database-wizard"></a>Käytä Microsoft Azure-tietokannan käyttöönotto tietokannan

> [AZURE.NOTE] Seuraavissa vaiheissa oletetaan, että sinulla on [valmisteltu SQL-tietokanta](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/).

1. Varmista, että sinulla on SQL Server Management Studiossa uusimman version. Uusien versioiden Management Studiossa päivitetään kuukausittain pysyvän synkronoituina muutoksia Azure-portaaliin.

    > [AZURE.IMPORTANT] On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).

2. Avaa Management Studiossa ja muodosta yhteys SQL Server-tietokannan uuteen objektin Resurssienhallinnassa.
3. Tietokannan Object Explorerissa hiiren kakkospainikkeella, **tehtävät**ja sitten **Käyttöönotto-tietokannan Microsoft Azure SQL-tietokanta...**

    ![Ottaa käyttöön Azure tehtävät-valikossa](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.  Valitse käyttöönoton ohjatussa valitsemalla **Seuraava**ja valitse sitten **Yhdistä** määrittämää yhteyttä SQL-tietokantapalvelimeen.

    ![Ottaa käyttöön Azure tehtävät-valikossa](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. Kirjoita palvelin-valintaikkunassa Yhdistä-yhteystietosi muodostaa yhteyden SQL-tietokantapalvelimeen.

    ![Ottaa käyttöön Azure tehtävät-valikossa](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.  Anna [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) tiedosto, jonka tämä ohjattu toiminto luo siirron aikana seuraavasti:

 - **Uuden tietokannan nimi** 
 - **Edition, Microsoft Azure SQL-tietokanta** ([palvelutaso](sql-database-service-tiers.md))
 - **Tietokannan enimmäiskoko**
 - **Palvelun tavoitteen** (suorituskyvyn taso)
 - **Väliaikaisen tiedostonimi**  

    ![Vie asetukset](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.  Suorita ohjattu. Sen mukaan, koosta ja monimutkaisuudesta tietokannan käyttöönotto voi kestää muutaman minuutin kuluttua-monta tuntia. Jos ohjattu toiminto havaitsee yhteensopivuusongelmat, virheet näkyvät näyttöä ja siirto ei voi jatkaa. Ohjeita siitä, miten tietokannan yhteensopivuusongelmat korjaamiseksi Siirry [tietokannan yhteensopivuusongelmat](sql-database-cloud-migrate-fix-compatibility-issues.md)korjaamiseksi.

7.  Objektin Resurssienhallinnassa muodostaa siirretyt Azure SQL-tietokanta-server-tietokannassa.
8.  Azure portal-ominaisuuden avulla voit tarkastella tietokannassa ja sen ominaisuuksia.

## <a name="next-steps"></a>Seuraavat vaiheet

- [SSDT uusimmassa versiossa](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studiossa uusimmassa versiossa](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md)
- [Transact-SQL osittain tai funktioita, joita ei tueta](sql-database-transact-sql-information.md)
- [Siirtää SQL Server-tietokantoja SQL Server siirron hallinnan käyttäminen](http://blogs.msdn.com/b/ssma/)
