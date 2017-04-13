<properties
    pageTitle="Hallitse Azure SQL-tietokanta Azure-portaalissa | Microsoft Azure"
    description="Opettele Azure-portaalin avulla voit hallita relaatiotietokannasta pilvipalvelussa Azure-portaalissa."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.date="09/19/2016"
    ms.author="sstein"/>


# <a name="managing-azure-sql-databases-using-the-azure-portal"></a>Azure SQL-tietokantoja Azure-portaalissa hallinta


> [AZURE.SELECTOR]
- [Azure portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShellin](sql-database-manage-powershell.md)

[Azure portaalissa](https://portal.azure.com/) voit luoda ja seurata Azure SQL-tietokannat ja palvelinten hallinta. Tässä artikkelissa on lyhyt kuvaus ja linkkejä yleisimpiä tehtäviä tietoja.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Azure SQL-tietokantoja, palvelinten ja jakavat tarkasteleminen

Jos haluat tarkastella SQL-tietokanta-palveluista, valitse **Lisää palveluja**ja kirjoita **SQL** hakuruutuun:

![SQL-tietokantaan](./media/sql-database-manage-portal/sql-services.png)


## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Kuinka luoda tai tarkastella Azure SQL-tietokantoja?

Avaa **SQL-tietokantoja** -sivu, valitse **SQL-tietokantoja**, ja valitse haluamasi tietokanta tai napsauttamalla **+ Lisää** SQL-tietokannan luominen. Lisätietoja on artikkelissa [Luo SQL-tietokanta käyttämällä Azure portaalin minuutteina](sql-database-get-started.md).


![SQL-tietokannat](./media/sql-database-manage-portal/sql-databases.png)


## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Kuinka luoda tai tarkastella Azure SQL-palvelimet?

Avaa **SQL Server** -sivu, valitse **SQL Server**- ja haluat käsitellä palvelin tai valitse **+ Add** luo SQL server. Lisätietoja on artikkelissa [Luo SQL-tietokanta käyttämällä Azure portaalin minuutteina](sql-database-get-started.md).

![SQL-palvelimet](./media/sql-database-manage-portal/sql-servers.png)


## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Kuinka luoda tai tarkastella SQL joustavasti jakavat?

Avaa **joustavasti jakavat SQL** -sivu, **SQL joustavasti jakavat**, valitse ja valitse varanto haluat käsitellä tai napsauttamalla **+ Lisää** resurssivarantoon luomiseen. Lisätietoja on artikkelissa [Create joustavasti tietokanta-ryhmän Azure-portaalissa](sql-database-elastic-pool-create-portal.md).

![SQL-joustavasti jakavat](./media/sql-database-manage-portal/elastic-pools.png)



## <a name="how-do-i-update-or-view-sql-database-settings"></a>Miten päivitettävää tai tarkasteltavaa SQL-tietokanta-asetukset?

Voit tarkastella tai päivittää tietokanta-asetukset, valitse SQL-tietokanta-sivu, valitse haluamasi asetus:


![SQL-tietokanta-asetukset](./media/sql-database-manage-portal/settings.png)


## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Mistä löydän SQL tietokantojen palvelimen täydellinen nimi?

Saat tietokantojen-palvelimen nimen selvittäminen **SQL-tietokanta** -sivu- **Yleistä** ja palvelimen nimi:


![SQL-tietokanta-asetukset](./media/sql-database-manage-portal/server-name.png)


## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Miten voin hallita palomuurisäännöt, joiden avulla voit hallita SQL server- ja tietokannan?

Voit tarkastella, luoda tai päivittää palomuurisäännöt, valitse **Määritä palomuurin** **SQL-tietokanta** -sivu. Lisätietoja on artikkelissa [Azure-portaalissa Azure SQL-tietokanta on palvelintason palomuurisääntö määrittäminen](sql-database-configure-firewall-settings.md).


![palomuurisäännöt](./media/sql-database-manage-portal/sql-database-firewall.png)


## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Miten voin muuttaa oma SQL tietokanta service taso tai suorituskyvyn taso?


Jos haluat päivittää palvelun taso tai suorituskyvyn taso SQL-tietokantaan, valitse **SQL-tietokanta** -sivu **hinnoittelu taso (DTUs-asteikko)** . Lisätietoja on artikkelissa [palvelun taso ja suorituskyvyn tason muuttaminen (hinnoittelu taso) SQL-tietokantaan](sql-database-scale-up.md).


![hinnat tasoa](./media/sql-database-manage-portal/pricing-tier.png)


## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Miten voin valvonnan asetusten määrittäminen ja uhkien tunnistus SQL-tietokantaan?

Määritä valvonta- ja uhkien tunnistus SQL-tietokantaan, napsauttamalla **valvonta ja uhkien tunnistus** **SQL-tietokanta** -sivu. Lisätietoja on artikkelissa [aloittaminen SQL-tietokannan tarkistaminen](sql-database-auditing-get-started.md)ja [SQL-tietokannan uhkien tunnistus käytön aloittaminen](sql-database-threat-detection-get-started.md).


## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Miten masking SQL-tietokantaan dynaamiset tiedot määritetään?

Valitse **Dynamic data masking** määrittämiseen dynaamiset tiedot masking SQL-tietokantaan **SQL-tietokanta** -sivu. Lisätietoja on artikkelissa [SQL tietokanta Dynamic Data-Masking käytön aloittaminen](sql-database-dynamic-data-masking-get-started.md).


## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Miten SQL-tietokantaan läpinäkyvä salauksen (TDE) määritetään?

Määritä läpinäkyvä tietojen SQL-tietokannan salauksen, valitse **Läpinäkyvä salauksen** **SQL-tietokanta** -sivu. Lisätietoja on artikkelissa [Ota käyttöön TDE portaalissa tietokantaa](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Miten voi tarkastella tai muuttaa SQL-tietokannan enimmäiskoko?

Voit tarkastella tai muuttaa kokoa SQL-tietokantaan, valitse **tietokannan koko** **SQL-tietokanta** -sivu. Päivitä tietokannan enimmäiskoko muuttamalla palvelun taso tai suorituskyvyn taso. Lisätietoja on artikkelissa [palvelun taso ja suorituskyvyn tason muuttaminen (hinnoittelu taso) SQL-tietokantaan](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Miten valvoa ja parantaa suorituskykyä SQL-tietokantaan?

Valitse **Suorituskyky yleiskatsaus** ja parantaa suorituskykyä ominaispiirteistä SQL-tietokantaan, **SQL-tietokanta** -sivu. Lisätietoja on artikkelissa [SQL-tietokannan suorituskykyä tietoja](sql-database-performance.md).


## <a name="how-do-i-configure-geo-replication"></a>Miten Geo replikoinnin määritetään?

Valitse **Geo replikoinnin** määrittämään Geo replikoinnin SQL-tietokantaan **SQL-tietokanta** -sivu. Lisätietoja on artikkelissa [Määrittäminen Geo-replikoinnin Azure SQL-tietokantaan Azure-portaalissa](sql-database-geo-replication-portal.md).


## <a name="how-do-i-failover-to-a-geo-replicated-sql-database"></a>Miten voin automaattisesti geo replikoida SQL-tietokantaan?

Voit siirtää geo replikoida toissijainen, napsauttamalla **Geo replikoinnin** **SQL-tietokanta** -sivu ja valitse sitten **automaattisesti**. Lisätietoja on artikkelissa [aloittaa suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokantaan Azure-portaalissa](sql-database-geo-replication-failover-portal.md).


## <a name="how-do-i-copy-a-sql-database"></a>Miten SQL-tietokannan kopioiminen?

Kopioi SQL-tietokantaan, valitse **Kopioi** **SQL-tietokanta** -sivu. Lisätietoja on artikkelissa [kopioida Azure-portaalissa Azure SQL-tietokantaan](sql-database-copy-portal.md).


![SQL-tietokanta-asetukset](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Miten Azure SQL-tietokantaan BACPAC tiedostoon arkistoida?

Luo BACPAC SQL-tietokannan, valitse **Vie** **SQL-tietokanta** -sivu. Lisätietoja on artikkelissa [Azure SQL-tietokantaan Azure-portaalissa BACPAC tiedostoon arkisto](sql-database-export.md).


![SQL-tietokannan vienti](./media/sql-database-manage-portal/sql-database-export.png)



## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Miten palauttaminen SQL-tietokannan aiempaan samanaikaisesti

Voit palauttaa SQL-tietokantaan, valitse **Palauta** **SQL-tietokanta** -sivu. Lisätietoja on artikkelissa [Palauta edellinen pisteeseen senhetkinen Azure-portaalissa Azure SQL-tietokanta](sql-database-point-in-time-restore-portal.md).


![SQL-tietokanta-asetukset](./media/sql-database-manage-portal/sql-database-restore.png)


## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Miten voin luoda Azure SQL-tietokantaan BACPAC tiedostosta?

SQL-tietokannan luominen BACPAC tiedoston, valitse **Tuo tietokanta** **SQL server** -sivu. Lisätietoja on artikkelissa [Azure SQL-tietokannan luominen BACPAC tiedoston tuominen](sql-database-import.md).


![SQL server](./media/sql-database-manage-portal/server-commands.png)


## <a name="how-do-i-restore-a-deleted-sql-database"></a>Kuinka poistetun SQL-tietokannan palauttaminen?

Palauttaa poistetun SQL-tietokantaan, valitse **SQL server** -sivu (SQL server, joka sisältää haluamasi tietokanta on poistettu) **poistaa tietokannat** . Lisätietoja on artikkelissa [Palauta poistetut Azure SQL-tietokanta Azure-portaalissa](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Miten voin poistaa SQL-tietokantaan?

Jos haluat poistaa SQL-tietokantaan, valitse **Poista** **SQL-tietokanta** -sivu. 

![SQL-tietokanta-asetukset](./media/sql-database-manage-portal/sql-database-delete.png)



## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokantaan](sql-database-technical-overview.md)
- [Seurata ja hallita joustavasti tietokanta-ryhmän Azure-portaalissa](sql-database-elastic-pool-manage-portal.md)
