<properties
   pageTitle="SQL Server-tietokantaan siirtyminen Azure SQL-tietokanta | Microsoft Azure"
   description="Microsoft Azure SQL-tietokanta-tietokannan käyttöönotto-tietokannan siirto-tietokannan tuominen Vie tietokantaan, ohjattu siirto"
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

# <a name="import-from-bacpac-to-sql-database-using-ssms"></a>Tuo BACPAC käyttämällä SSMS SQL-tietokantaan

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure portal](sql-database-import.md)
- [PowerShellin](sql-database-import-powershell.md)

Tässä artikkelissa kerrotaan [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) tiedoston tuomisesta SQL-tietokantaan ohjattua Vie tiedot taso sovelluksen SQL Server Management Studiossa.

> [AZURE.NOTE] Seuraavissa vaiheissa oletetaan, että on jo valmisteltu Azure SQL-looginen esiintymän ja yhteystiedot on valmiina.

1. Varmista, että sinulla on SQL Server Management Studiossa uusimman version. Uusien versioiden Management Studiossa päivitetään kuukausittain pysyvän synkronoituina muutoksia Azure-portaaliin.

     > [AZURE.IMPORTANT] On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).

2. Yhdistää Azure SQL-tietokanta-palvelimeen, napsauta **tietokantojen** -kansiota hiiren kakkospainikkeella ja valitse **Tuo tiedot tason sovelluksen...**

    ![Tuo tiedot tason sovelluksen-valikkovaihtoehto](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

3.  Tietokannan luomiseen Azure SQL-tietokantaan, BACPAC tekstitiedoston tuominen kiintolevylle ja valitse Azure-tallennustilan tilin ja säilö, johon olet ladannut BACPAC-tiedosto.

    ![Asetusten tuominen](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

     > [AZURE.IMPORTANT] Kun tuot BACPAC Azure-blob-säiliö, käytä vakio tallennustilan. Tuominen premium tallennustilan BACPAC ei tueta.

4.  Anna **uuden tietokannan nimi** Azure SQL-DB-tietokantaan, **Edition, Microsoft Azure SQL-tietokanta** (service taso), **tietokannan enimmäiskoko**ja **Palvelun tavoitteen** (suorituskyvyn taso) määrittäminen.

    ![Asetusten määrittäminen](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

5.  Valitse **Seuraava** ja valitse sitten **Valmis** BACPAC-tiedoston tuominen uuteen tietokantaan-Azure SQL-tietokantapalvelimeen.

6. Objektin Resurssienhallinnassa muodostaa yhteys siirretyt tietokantaan Azure SQL-tietokanta-palvelimessa.

6.  Azure portal-ominaisuuden avulla voit tarkastella tietokannassa ja sen ominaisuuksia.

## <a name="next-steps"></a>Seuraavat vaiheet

- [SSDT uusimmassa versiossa](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studiossa uusimmassa versiossa](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md)
- [Transact-SQL osittain tai funktioita, joita ei tueta](sql-database-transact-sql-information.md)
- [Siirtää SQL Server-tietokantoja SQL Server siirron hallinnan käyttäminen](http://blogs.msdn.com/b/ssma/)
