<properties
   pageTitle="Luo SQL-tietovarasto Azure-portaalissa | Microsoft Azure"
   description="Opettele luomaan Azure SQL-tietovarasto Azure-portaalissa"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="barbkess;lodipalm;sonyama"/>

# <a name="create-an-azure-sql-data-warehouse"></a>Luo Azure SQL-tietovarasto

> [AZURE.SELECTOR]
- [Azure portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShellin](sql-data-warehouse-get-started-provision-powershell.md)

Tässä opetusohjelmassa käytetään Azure portaalin luomaan SQL-tietovarasto, joka sisältää AdventureWorksDW-mallitietokanta.


## <a name="prerequisites"></a>Edellytykset

Aluksi sinun on:

- **Azure-tili**: Siirry [Azure maksuttoman kokeiluversion][] tai [MSDN Azure hyvitykset][] Luo tili.
- **Azure SQL server**: Katso lisätietoja [luominen Azure SQL-tietokanta looginen server Azure-portaalissa][] .

> [AZURE.NOTE] SQL-tietovarasto luominen saattaa aiheuttaa uuden laskutettavan palvelun.  Katso lisätietoja [SQL-tietovarasto hinnoittelua][] .

## <a name="create-a-sql-data-warehouse"></a>SQL-tietovarasto luominen

1. Kirjautuminen [Azure portal](https://portal.azure.com).

2. Valitse **+ Uusi** > **tietojen + tallennustilan** > **SQL-tietovarasto**.

    ![Luo](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. **SQL-tietovarasto** -sivu-tiedot Täytä tarvittavat ja paina näppäinyhdistelmää luominen, jos haluat luoda.

    ![Tietokannan luominen](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Palvelin**: suosittelemme Valitse palvelimellesi.  

    - **Tietokannan nimi**: nimi, jota käytetään viittaamaan SQL-tietovarasto.  Se on oltava yksilöllinen palvelimeen.
    
    - **Suorituskyvyn**: suosittelemme 400 [DWUs]alkaen[DWU]. Voit siirtää liukusäädintä vasemmalle tai oikealle, voit säätää data warehouse suorituskykyä tai skaalata ylös tai alas luonnin jälkeen.  Lisätietoja DWUs on Microsoftin ohjeissa [Skaalaus](./sql-data-warehouse-manage-compute-overview.md) tai tutustu [hinnat sivun][SQL-tietovarasto hinnat]. 

    - **Tilaus**: Valitse [tilaus] , tämä SQL-tietovarasto laskuttaa.

    - **Resurssiryhmä**: [resurssiryhmiä] [ Resource group] on suunniteltu helpottamaan hallinnassa Azure resurssien säilöjen. Lue lisää [resurssiryhmiä](../azure-resource-manager/resource-group-overview.md).

    - **Valitse lähde**: Valitse **lähde** > **malli**. Azure täyttää automaattisesti **Valitse malli** -asetus, jossa AdventureWorksDW.

> [AZURE.NOTE] SQL-tietovarasto Oletuslajittelu on SQL_Latin1_General_CP1_CI_AS. Jos eri lajittelu tarvita, [T-SQL][] voidaan kanssa eri lajittelu tietokannan luomiseen.

4. Valitse **Luo** SQL-tietovarasto luomiseen.

5. Odota muutama minuutti. Kun data warehouse on valmis, on palautettava [Azure portal](https://portal.azure.com). Löydät SQL-tietovarasto raporttinäkymät-luettelossa, valitse SQL-tietokannat tai resurssiryhmän, jota käytit asiakirja sitten kannattaa luoda. 

    ![portaalin näkymä](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut SQL-tietovarasto, [Yhdistä](./sql-data-warehouse-connect-overview.md) haluat ja aloittaa kyselyt.

Ladata tietoja SQL-tietovarasto, on artikkelissa [Yleistä ladataan](./sql-data-warehouse-overview-load.md).

Jos yrität siirtää aiemmin luodun tietokannan SQL-tietovarasto, artikkelissa [siirron yleiskuvaus](./sql-data-warehouse-overview-migrate.md) tai [Siirto](./sql-data-warehouse-migrate-migration-utility.md)-apuohjelman.

Palomuurisäännöt voidaan määrittää myös Transact-SQL avulla. Lisätietoja on artikkelissa [sp_set_firewall_rule][] ja [sp_set_database_firewall_rule][].

Kannattaa myös hyvä katsomalla [parhaita käytäntöjä][].

<!--Article references-->
[Luo looginen Azure SQL-tietokanta-server Azure-portaalissa]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Create an Azure SQL Database logical server with PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[resource groups]: ../resource-group-template-deploy-portal.md
[Parhaat käytännöt]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[tilauksen]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md
 
<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL-tietovarasto hinnat]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure ilmainen kokeiluversio]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure hyvitykset]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

