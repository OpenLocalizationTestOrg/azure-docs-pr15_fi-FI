<properties
    pageTitle="Palauttaa Azure SQL-tietokantaan automaattisen varmuuskopioinnin (Azure portaalin) | Microsoft Azure"
    description="Palauttaa Azure SQL-tietokantaan automaattisen varmuuskopioinnin (Azure portaalin)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/18/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-an-azure-sql-database-from-an-automatic-backup-using-the-azure-portal"></a>Palauttaa Azure SQL-tietokantaan automaattisen varmuuskopioinnin Azure-portaalissa


> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-recovery-using-backups.md#geo-restore)
- [GEO palauttaminen: PowerShell](sql-database-geo-restore-powershell.md)

Tässä artikkelissa kerrotaan, miten palauttaa tietokannan [automaattisen varmuuskopioinnin](sql-database-automated-backups.md) uusi Serveriin kanssa [Geo palauttaa](sql-database-recovery-using-backups/.md#geo-restore) Azure-portaalissa.

## <a name="select-a-database-to-restore"></a>Valitse Palauta tietokanta

Jos haluat palauttaa tietokannan Azure-portaalissa, toimi seuraavasti:

1.  Siirry [Azure portal](https://portal.azure.com).
2.  Valitse näytön vasemmassa reunassa **+ Uusi** > **tietokantojen** > **SQL-tietokantaan**:

    ![Palauttaa Azure SQL-tietokantaan](./media/sql-database-geo-restore-portal/new-sql-database.png)

3.  Valitse **Varmuuskopioi** lähteenä ja valitse sitten palautettavan varmuuskopion. Määritä tietokannan nimi, palvelimen palautettavan tietokannan käännetään, ja valitse sitten **Luo**:
  
    ![Palauttaa Azure SQL-tietokantaan](./media/sql-database-geo-restore-portal/geo-restore.png)

Seurata tilaa palautustoiminto valitsemalla sivun oikeassa yläkulmassa olevaa ilmoituskuvaketta. 


## <a name="next-steps"></a>Seuraavat vaiheet

- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)
- Lisätietoja Azure SQL-tietokanta automaattisen varmuuskopioinnin on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin suunnittelu](sql-database-automated-backups.md)
- Lisätietoja palauttaminen automaattisen varmuuskopioinnin käyttämisestä on artikkelissa [tietokannan palvelun käynnistämä-varmuuskopioista palauttaminen](sql-database-recovery-using-backups.md)
- Lisätietoja nopeampi palautusasetukset on artikkelissa [Aktiivinen-Geo-replikointi](sql-database-geo-replication-overview.md)  
- Lisätietoja käyttämisestä automaattisen varmuuskopioinnin arkistointia varten, tutustu artikkeliin [tietokannan kopioiminen](sql-database-copy.md)
