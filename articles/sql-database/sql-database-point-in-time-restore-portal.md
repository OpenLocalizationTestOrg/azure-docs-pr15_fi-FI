<properties
    pageTitle="Palauttaa Azure SQL-tietokantaan edellisessä kohdassa aika (Azure portaalin) | Microsoft Azure"
    description="Palauttaa Azure SQL-tietokantaan edellisessä kohdassa samanaikaisesti."
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


# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-the-azure-portal"></a>Palauttaa Azure SQL-tietokantaan aiempaan ajankohtaan Azure-portaalissa


> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-recovery-using-backups.md)
- [Ajankohta: Palauta: PowerShell](sql-database-point-in-time-restore-powershell.md)

Tässä artikkelissa kerrotaan, miten voit palauttaa tietokannan aikaisempaan senhetkinen [SQL-tietokantaan automaattisen varmuuskopioinnin](sql-database-automated-backups.md) Azure-portaalissa.

## <a name="restore-a-sql-database-to-a-previous-point-in-time"></a>Palauta SQL-tietokanta aiempaan samanaikaisesti

Valitse tietokanta palauttaa Azure-portaalissa:

1.  Avaa [Azure portal](https://portal.azure.com).
2.  Valitse näytön vasemmassa reunassa **Lisää palveluja** > **SQL-tietokannat**.
3.  Valitse palautettavan tietokannan.
4.  Tietokannan sivun yläosassa valitsemalla **Palauta**:

    ![Palauttaa Azure SQL-tietokantaan](./media/sql-database-point-in-time-restore-portal/restore.png)

5.  **Palauttaa** sivulla Valitse päivämäärä ja kellonaika (UTC-ajan), jos haluat palauttaa tietokannan ja valitse sitten **OK**:

    ![Palauttaa Azure SQL-tietokantaan](./media/sql-database-point-in-time-restore-portal/restore-details.png)

## <a name="monitor-the-restore-operation"></a>Näytön palautus

1. Kun olet valinnut **OK** edellisessä vaiheessa, napsauta sivun oikeassa yläkulmassa ilmoituskuvaketta ja valitse Lisätietoja **palauttaminen SQL-tietokanta** -ilmoitus.

    ![Palauttaa Azure SQL-tietokantaan](./media/sql-database-point-in-time-restore-portal/notification-icon.png)

2. Palauta tilan tietoja sisältävä palauttaminen SQL tietokanta-sivu avautuu. Voit myös valita rivin kohteen Lisätietoja:

    ![Palauttaa Azure SQL-tietokantaan](./media/sql-database-point-in-time-restore-portal/inprogress.png)

 

## <a name="next-steps"></a>Seuraavat vaiheet

- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)
- Lisätietoja Azure SQL-tietokanta automaattisen varmuuskopioinnin on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin suunnittelu](sql-database-automated-backups.md)
- Lisätietoja palauttaminen automaattisen varmuuskopioinnin käyttämisestä on artikkelissa [tietokannan palvelun käynnistämä-varmuuskopioista palauttaminen](sql-database-recovery-using-backups.md)
- Lisätietoja nopeampi palautusasetukset on artikkelissa [Aktiivinen-Geo-replikointi](sql-database-geo-replication-overview.md)  
- Lisätietoja käyttämisestä automaattisen varmuuskopioinnin arkistointia varten, tutustu artikkeliin [tietokannan kopioiminen](sql-database-copy.md)
