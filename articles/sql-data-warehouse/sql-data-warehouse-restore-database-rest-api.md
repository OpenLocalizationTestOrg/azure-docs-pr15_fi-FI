<properties
   pageTitle="Palauttaa Azure SQL-tietovarasto (REST API) | Microsoft Azure"
   description="REST API tehtävät Azure SQL-tietovarasto palauttamista varten."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Palauttaa Azure SQL-tietovarasto (REST API)

> [AZURE.SELECTOR]
- [Yleiskatsaus][]
- [Portal][]
- [PowerShellin][]
- [MUILLE KÄYTTÄJILLE][]

Tässä artikkelissa kerrotaan palauttamisesta Azure SQL-tietovarasto REST-Ohjelmointirajapinnan käyttäminen.

## <a name="before-you-begin"></a>Ennen aloittamista

**Tarkista DTU kapasiteetti.** SQL Server (kuten myserver.database.windows.net) on oletusarvoinen DTU kiintiön nykyisessä kunkin SQL-tietovarasto.  Ennen kuin voit palauttaa SQL-tietovarasto, varmista, että SQL server on tarpeeksi jäljellä olevan DTU kiintiön palautetaan parhaillaan tietokannan. Lisätietoja DTU tarpeen laskea tai pyydä lisätietoja DTU on artikkelissa [DTU kiintiön muutospyyntö][].

## <a name="restore-an-active-or-paused-database"></a>Aktiivinen tai keskeytetty tietokannan palauttaminen

Voit palauttaa tietokannan seuraavasti:

1. Pyydä tietokannan palauttaminen asioista Get tietokannan palauttaminen pisteitä-toiminnon luettelossa.
2. Aloita napsauttamalla Palauta [Luo tietokannan palauttaminen pyynnön][] -toiminnon avulla.
3. Seurata napsauttamalla Palauta tila [tietokannan toiminnon tila][] -toiminnon avulla.

>[AZURE.NOTE] Kun palautus on valmis, voit määrittää palautetun tietokannan mukaan seuraavat [tietokantasi palautuksen jälkeen asetuksia][].

## <a name="restore-a-deleted-database"></a>Poistetun tietokannan palauttaminen

Voit palauttaa poistetun tietokannan seuraavasti:

1.  Luettelo kaikista restorable poistetut tietokantoja [luettelon restorable poistetusta tietokantojen][] -toiminnon avulla.
2.  Saat käyttämällä [Hae restorable poistetusta tietokanta][] -toimintoa palautettavien poistetun tietokannan tiedot.
3.  Aloita napsauttamalla Palauta [Luo tietokannan palauttaminen pyynnön][] -toiminnon avulla.
4.  Seurata napsauttamalla Palauta tila [tietokannan toiminnon tila][] -toiminnon avulla.

>[AZURE.NOTE] Voit määrittää tietokannan, kun palautus on valmis, katso [tietokantasi palautuksen jälkeen asetuksia][]. 


## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja liiketoiminnan jatkuvuuden ominaisuuksista versioiden Azure SQL-tietokanta, lue [Azure SQL-tietokanta liiketoiminnan jatkuvuuden yhteenveto][].

<!--Image references-->

<!--Article references-->
[Azure SQL-tietokantaan liiketoiminnan jatkuvuuden yhteenveto]: ./sql-database-business-continuity.md
[Muutospyyntö DTU kiintiö]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Määritä tietokantasi palautuksen jälkeen]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[Yleiskatsaus]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShellin]: ./sql-data-warehouse-restore-database-powershell.md
[MUILLE KÄYTTÄJILLE]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Tietokannan palauttaminen pyynnön luominen]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Tietokannan toiminnon tila]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Hae restorable katkeavat tietokanta]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[Luettelon restorable pudotettu tietokannat]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
