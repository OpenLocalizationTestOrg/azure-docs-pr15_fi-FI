<properties
   pageTitle="SQL Server Management Studiossa avulla voit määrittää SQL-tietokanta ennen siirtoa Azure SQL-tietokantaan yhteensopivuuden | Microsoft Azure"
   description="Microsoft Azure SQL-tietokanta-tietokannan siirto, SQL-tietokantaan yhteensopivuuden, Vie tiedot taso sovelluksen ohjattu"
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
   ms.date="08/29/2016"
   ms.author="carlrab"/>

# <a name="use-sql-server-management-studio-to-determine-sql-database-compatibility-before-migration-to-azure-sql-database"></a>SQL Server Management Studiossa avulla voit määrittää SQL-tietokantaan yhteensopivuuden ennen siirtoa Azure SQL-tietokantaan

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Päivityksen tukipalvelu](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
 
Tässä artikkelissa kerrotaan, voit selvittää, onko SQL Server-tietokantaan yhteensopiva siirtäminen SQL-tietokantaan ohjattua Vie tiedot taso sovelluksen SQL Server Management Studiossa.

## <a name="using-sql-server-management-studio"></a>SQL Server Management Studiossa

1. Varmista, että sinulla on SQL Server Management Studiossa uusimman version. Uusien versioiden Management Studiossa päivitetään kuukausittain pysyvän synkronoituina muutoksia Azure-portaaliin.

     > [AZURE.IMPORTANT] On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).

2. Avaa Management Studiossa ja muodosta yhteys Object Explorer lähdetietokannan.
3. Napsauta lähdetietokannan Object Explorerissa, **tehtävät**ja valitse **Vie tiedot tason sovellukseen**

    ![Vie tiedot taso-sovelluksen tehtävät-valikosta](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Ohjattu vientitoiminto-Valitse **Seuraava**ja määritä sitten **asetukset** -välilehdessä Tallenna BACPAC joko paikalliseen levyasemaan sijaintiin tai Azure-blob Vie. BACPAC-tiedosto on tallennettu, jos sinulla ei ole tietokannan yhteensopivuusongelmat. Jos tiedostossa on yhteensopivuusongelmia, ne näkyvät konsolissa.

    ![Vie asetukset](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS02.png)

5. Voit ohittaa tietojen viemisestä, valitse **Lisäasetukset-välilehti** ja poista **Valitse kaikki** -valintaruutu. Tavoitteenamme on tässä vaiheessa vain, jos haluat testata yhteensopivuuden.

    ![Vie asetukset](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS03.png)

6. Valitse **Seuraava** ja valitse sitten **Valmis**. Tietokannan yhteensopivuusongelmia, jos näkyvät kun ohjattu toiminto tarkistaa rakenne.

    ![Vie asetukset](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS04.png)

7. Jos virheitä ei näy, että tietokanta on yhteensopiva ja haluat siirtää. Jos sinulla on virheitä, sinun on niiden ratkaisemiseksi. Saat virheet valitsemalla **Validating**rakenteen **Virhe** . 
    ![Vie asetukset](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS05.png)

8.  Jos *. BACPAC tiedosto luodaan onnistuneesti, valitse tietokantasi on yhteensopiva SQL-tietokantaan ja haluat siirtää.

## <a name="next-steps"></a>Seuraavat vaiheet

- [SSDT uusimmassa versiossa](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studiossa uusimmassa versiossa](https://msdn.microsoft.com/library/mt238290.aspx)
- [Korjaa tietokanta siirron yhteensopivuusongelmat](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Yhteensopivat SQL Server-tietokannan siirtäminen SQL-tietokantaan](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md)
- [Transact-SQL osittain tai funktioita, joita ei tueta](sql-database-transact-sql-information.md)
- [Siirtää SQL Server-tietokantoja SQL Server siirron hallinnan käyttäminen](http://blogs.msdn.com/b/ssma/)
