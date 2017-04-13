<properties
   pageTitle="Palauttaa Azure SQL-tietovarasto (portaalin) | Microsoft Azure"
   description="Azure portaalin tehtävät Azure SQL-tietovarasto palauttamista varten."
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

# <a name="restore-an-azure-sql-data-warehouse-portal"></a>Palauttaa Azure SQL-tietovarasto (portaalin)

> [AZURE.SELECTOR]
- [Yleiskatsaus][]
- [Portal][]
- [PowerShellin][]
- [MUILLE KÄYTTÄJILLE][]

Tässä artikkelissa kerrotaan palauttamisesta Azure SQL-tietovarasto Azure-portaalissa.

## <a name="before-you-begin"></a>Ennen aloittamista

**Tarkista DTU kapasiteetti.** SQL Server (kuten myserver.database.windows.net) on oletusarvoinen DTU kiintiön nykyisessä kunkin SQL-tietovarasto.  Ennen kuin voit palauttaa SQL-tietovarasto, varmista, että SQL server on tarpeeksi jäljellä olevan DTU kiintiön palautetaan parhaillaan tietokannan. Lisätietoja DTU tarpeen laskea tai pyydä lisätietoja DTU on artikkelissa [DTU kiintiön muutospyyntö][].


## <a name="restore-an-active-or-paused-database"></a>Aktiivinen tai keskeytetty tietokannan palauttaminen

Voit palauttaa tietokannan seuraavasti:

1. Kirjaudu sisään [Azure portal][]
2. Näytön vasemmassa reunassa Valitse **Selaa** ja valitse sitten **SQL-palvelimet**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
    
3. Siirry palvelimellesi ja valitse se
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)

4. Etsi SQL-tietovarasto, jonka haluat palauttaa, ja valitse se
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Valitse tietovarasto sivu yläreunassa **palauttaminen**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)

6. Uuden **tietokannan nimi**
7. Valitse uusimman **Palauttaminen piste**
    1. Varmista, että valitset uusimman Palauta-kohdassa.  Koska palauttaminen pisteet näkyvät UTC-joskus näkyy oletusasetus ei ole uusin Palauta-kohdassa.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)

8. Valitse **OK**
9. Tietokannan palauttaminen prosessi alkaa ja voidaan valvoa **ilmoitukset**

>[AZURE.NOTE] Kun palautus on valmis, voit määrittää palautetun tietokannan mukaan seuraavat [tietokantasi palautuksen jälkeen asetuksia][].


## <a name="restore-a-deleted-database"></a>Poistetun tietokannan palauttaminen

Voit palauttaa poistetun tietokannan seuraavasti:

1. Kirjaudu sisään [Azure portal][]
2. Näytön vasemmassa reunassa Valitse **Selaa** ja valitse sitten **SQL-palvelimet**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)

3. Siirry palvelimellesi ja valitse se
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)

4. Vieritä palvelimen sivu Toiminnot-osaan
5. Napsauta **Poistetut tietokantojen** -ruutua.
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)

6. Valitse haluat palauttaa poistetun tietokannan
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)

7. Uuden **tietokannan nimi**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
    
8. Valitse **OK**
9. Tietokannan palauttaminen prosessi alkaa ja voidaan valvoa **ilmoitukset**

>[AZURE.NOTE] Voit määrittää tietokannan, kun palautus on valmis, katso [tietokantasi palautuksen jälkeen asetuksia][]. 

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja liiketoiminnan jatkuvuuden ominaisuuksista versioiden Azure SQL-tietokanta, lue [Azure SQL-tietokanta liiketoiminnan jatkuvuuden yhteenveto][].

<!--Image references-->

<!--Article references-->
[Azure SQL-tietokantaan liiketoiminnan jatkuvuuden yhteenveto]: ./sql-database-business-continuity.md
[Yleiskatsaus]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShellin]: ./sql-data-warehouse-restore-database-powershell.md
[MUILLE KÄYTTÄJILLE]: ./sql-data-warehouse-restore-database-rest-api.md
[Määritä tietokantasi palautuksen jälkeen]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Muutospyyntö DTU kiintiö]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
