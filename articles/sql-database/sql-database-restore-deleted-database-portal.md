<properties
    pageTitle="Palauta poistetut Azure SQL-tietokanta (Azure portaalin) | Microsoft Azure"
    description="Palauta poistetut Azure SQL-tietokanta (Azure portaalin)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-using-the-azure-portal"></a>Palauta poistetut Azure SQL-tietokanta Azure-portaalissa

> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-recovery-using-backups.md)
- [**Palauta poistettu DB: portaali**](sql-database-restore-deleted-database-portal.md)
- [Palauta poistettu DB: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="select-the-database-to-restore"></a>Valitse Palauta tietokanta 

Voit palauttaa poistetun tietokannan Azure-portaalissa seuraavasti:

1.  Valitse **Lisää palveluja** [Azure portal](https://portal.azure.com)- > **SQL-palvelimiin**.
3.  Valitse palvelin, joka sisältää palautettavan tietokannan.
4.  Selaa palvelimen sivu **Toiminnot** -osa ja valitse **Poistetut tietokannat**: ![palauttaa Azure SQL-tietokantaan](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
5.  Valitse palautettavan tietokannan.
6.  Määritä tietokannan nimi ja valitse **OK**:

    ![Palauttaa Azure SQL-tietokantaan](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)


## <a name="next-steps"></a>Seuraavat vaiheet

- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)
- Lisätietoja Azure SQL-tietokanta automaattisen varmuuskopioinnin on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin suunnittelu](sql-database-automated-backups.md)
- Lisätietoja palauttaminen automaattisen varmuuskopioinnin käyttämisestä on artikkelissa [tietokannan palvelun käynnistämä-varmuuskopioista palauttaminen](sql-database-recovery-using-backups.md)
- Lisätietoja nopeampi palautusasetukset on artikkelissa [Aktiivinen-Geo-replikointi](sql-database-geo-replication-overview.md)  
- Lisätietoja käyttämisestä automaattisen varmuuskopioinnin arkistointia varten, tutustu artikkeliin [tietokannan kopioiminen](sql-database-copy.md)
