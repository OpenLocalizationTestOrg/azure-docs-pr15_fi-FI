
<properties
   pageTitle="SQL Server-tietokantaan vieminen SQL Server Management Studiossa BACPAC tiedostoon | Microsoft Azure"
   description="Microsoft Azure SQL-tietokanta-tietokannan siirto vieminen tietokantaan, vie BACPAC tiedosto, Vie tiedot taso-sovelluksen ohjattu toiminto"
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
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio"></a>SQL Server-tietokantaan vieminen SQL Server Management Studiossa BACPAC-tiedostoon

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

 
Tässä artikkelissa kerrotaan, miten voit viedä SQL Server-tietokantaan ohjatulla Vie taso sovelluksen SQL Server Management Studiossa [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) tiedostoon. 

1. Varmista, että sinulla on SQL Server Management Studiossa uusimman version. Uusien versioiden Management Studiossa päivitetään kuukausittain pysyvän synkronoituina muutoksia Azure-portaaliin.

     > [AZURE.IMPORTANT] On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).

2. Avaa Management Studiossa ja muodosta yhteys Object Explorer lähdetietokannan.

    ![Vie tiedot taso-sovelluksen tehtävät-valikosta](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. Napsauta lähdetietokannan Object Explorerissa, **tehtävät**ja valitse **Vie tiedot tason sovellukseen**

    ![Vie tiedot taso-sovelluksen tehtävät-valikosta](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Ohjattu vientitoiminto määritettävä tallentaa BACPAC tiedoston joko paikalliseen levyasemaan sijainti tai voit Azure blob Vie. Viedyt BACPAC sisältää aina valmiina tietokannan rakennetta ja oletusarvoisesti kaikkien taulukoiden tietoja. Käytä Lisäasetukset-välilehti, jos haluat jättää pois tietoja joistakin tai kaikista taulukoista. Haluat ehkä, esimerkiksi viedä vain tiedot viittauksen taulukoiden sijaan kaikkien taulukoiden.

***Tärkeää*** Vietäessä BACPAC Azure-blob-säiliö käyttämällä vakio-tallennustilan. Tuominen premium tallennustilan BACPAC ei tueta.

    ![Export settings](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)


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
