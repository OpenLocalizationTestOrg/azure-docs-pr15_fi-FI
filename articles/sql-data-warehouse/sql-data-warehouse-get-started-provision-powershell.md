<properties
   pageTitle="Luo SQL-tietovarasto PowerShellin avulla | Microsoft Azure"
   description="Luo SQL-tietovarasto PowerShell-toiminnolla"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-sql-data-warehouse-using-powershell"></a>Luo SQL-tietovarasto PowerShellin avulla

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShellin](sql-data-warehouse-get-started-provision-powershell.md)

Tässä artikkelissa kerrotaan, miten voit luoda SQL-tietovarasto PowerShellin avulla.

## <a name="prerequisites"></a>Edellytykset

Aluksi sinun on:

- **Azure-tili**: Siirry [Azure maksuttoman kokeiluversion][] tai [MSDN Azure hyvitykset][] Luo tili.
- **Azure SQL server**: Katso lisätietoja [luominen Azure SQL-tietokanta looginen server Azure-portaalissa][] - tai [Azure SQL-tietokanta looginen palvelimen PowerShellin avulla][] .
- **Resurssiryhmä**: käyttää samaa resurssiryhmä Azure SQL server tai Katso, [miten voit luoda resurssiryhmän][].
- **PowerShell versio 1.0.3 tai suurempi**: Voit tarkistaa versiosi suorittamalla **Get-moduuli - ListAvailable-nimen Azure**.  [Microsoftin WWW-ympäristö asennusohjelma][]voidaan asentaa uusin versio.  Katso lisätietoja uusimman version asentamisesta, [miten voit asentaa ja määrittää PowerShellin Azure][].

> [AZURE.NOTE] SQL-tietovarasto luominen voi johtaa uuden laskutettavan palvelun.  Katso lisätietoja hinnat [SQL-tietovarasto hinnat][] .

## <a name="create-a-sql-data-warehouse"></a>SQL-tietovarasto luominen

1. Avaa Windows PowerShell.
2. Tämän cmdlet kirjautumista Azure resurssien hallinta.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Valitse tilaus, jota haluat käyttää nykyisen istunnon.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Luo tietokanta. Tässä esimerkissä Luo tietokanta nimeltä "mynewsqldw" palvelun objektiivisten viereiset "DW400" nimeltä "sqldwserver1", joka sijaitsee nimeltä "mywesteuroperesgp1" resurssiryhmä-palvelimeen.

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

Tarvittavat parametrit ovat seuraavat:

- **RequestedServiceObjectiveName**: [DWU][] pyydät määrää.  Tuetut arvot ovat: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 ja DW6000.
- **Nimi**: SQL-tietovarasto, jota olet luomassa nimi.
- **Palvelimen nimi**:, jota käytät luontia varten palvelimen nimi (on oltava Vipuventtiili V12).
- **ResourceGroupName**: käytät resurssiryhmä.  Voit etsiä käytettävissä resurssin tilauksen ryhmiä käyttämällä Hae AzureResource.
- **Edition**: "DataWarehouse" on oltava SQL-tietovarasto luomiseen.

Valinnaisten parametrien ovat seuraavat:

- **CollationName**: Jos ei ole määritetty Oletuslajittelu on SQL_Latin1_General_CP1_CI_AS.  Lajittelu ei voi muuttaa tietokannan.
- **MaxSizeBytes**: tietokannan oletuskoko enimmäismäärä on 10 Gigatavua.


Lisätietoja parametrin asetukset-kohdassa [Uusi AzureRmSqlDatabase][] ja [Luo tietokanta (Azure SQL-tietovarasto)][].

## <a name="next-steps"></a>Seuraavat vaiheet

Kun SQL-tietovarasto on päättynyt valmistelu haluat ehkä yritä [ladata mallitiedot][] tai Katso, miten voit [kehittää][], [ladata][]tai [siirtää][].

Jos olet kiinnostunut lisää siitä, miten voit hallita SQL-tietovarasto ohjelmallisesti, tutustu tämän artikkelin käyttämisestä [PowerShellin cmdlet-komennot ja REST API][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[siirtää]: ./sql-data-warehouse-overview-migrate.md
[kehittäminen]: ./sql-data-warehouse-overview-develop.md
[lataaminen]: ./sql-data-warehouse-load-with-bcp.md
[Esimerkki tietojen lataaminen]: ./sql-data-warehouse-load-sample-databases.md
[PowerShellin cmdlet-komennot ja REST API]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[Asentaminen ja määrittäminen PowerShellin Azure]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Luo looginen Azure SQL-tietokanta-server Azure-portaalissa]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Luo looginen Azure SQL-tietokanta-palvelimen PowerShellin avulla]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Resurssiryhmä luominen]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[Uusi AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Luo tietokanta (Azure SQL-tietovarasto)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoftin WWW-ympäristö asennusohjelma]: https://aka.ms/webpi-azps
[SQL-tietovarasto hinnoittelua]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure ilmainen kokeiluversio]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure hyvitykset]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
