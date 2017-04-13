<properties
    pageTitle="Vianmääritys varmuuskopiointi ja palauttaminen Azure SQL-tietokantaan"
    description="Opi palauttamaan cloud tietokannan virheiden ja katkokset käyttämällä varmuuskopiot ja replikoiden Azure SQL-tietokantaan."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="restore-a-database-to-a-previous-point-in-time-restore-a-deleted-database-or-recover-from-a-data-center-outage"></a>Tietokannan palauttaminen aiempaan senhetkinen tai poistetun tietokannan palauttaminen tietojen center-käyttökatkosta palauttaminen

SQL-tietokanta säilyttää replikoita tietokannan, joten voit palauttaa katkokset ja käyttäjä-virhe. Käytettävissä olevat vaihtoehdot määräytyvät tietokannan palvelun tason ja asetuksia. Katso tiedot ja tyyliseikat [Liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md) .

## <a name="to-restore-a-database-to-a-previous-point-in-time"></a>Voit palauttaa tietokannan aiempaan aika
1.  Valitse [Azure Portal](https://azure.microsoft.com/)- **SQL-tietokannat**.
2.  Valitse tietokanta luettelosta ja valitse sitten **Palauta**.
3.  Tietokannalle uuden nimen, valitse päivämäärä ja aika, palauttaminen ja valitse sitten **Create.**
4.  Tee app viittaamaan uuden tietokannan tarpeen mukaan. Katso [palauttaa tietokannan senhetkinen pisteeseen](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="to-restore-an-accidentally-deleted-database"></a>Jos haluat palauttaa vahingossa poistetun tietokannan
1.  Valitse **SQL Server** [Azure-portaalissa](https://azure.microsoft.com/).
2.  Valitse tietokanta luettelosta isännöidään palvelin.
3.  Vieritä alas ja valitse **Poistetut tietokantojen**Server-sivu.
4.  Valitse Palauta tietokanta ja valitse sitten **Luo**.
5.  Tee app viittaamaan uuden tietokannan tarpeen mukaan. Katso [palauttaa poistetun tietokannan](sql-database-recovery-using-backups.md#deleted-database-restore).

## <a name="to-recover-from-a-regional-datacenter-outage"></a>Jos haluat palauttaa alueellisen palvelinkeskuksen käyttökatkosta
Vakio- ja Premium tietokantoja, jos määrität geo replikoida secondaries voit palauttaa nämä secondaries avulla. Näin Palauta tietokanta on vähemmän mahdollisen tietoja voidaan menettää. Katso lisätietoja [palauttaminen käyttämällä automaattista tietokannan varmuuskopioita Azure SQL-tietokantaan](sql-database-disaster-recovery.md) .

Azure sisältää myös varmuuskopioiden jokaisella tietokannan toisella alueella (geo tarpeettomat varmuuskopio). Voit luoda uuden tietokannan nämä varmuuskopioista, jota kutsutaan Geo Palauta, mutta on todennäköistä, että tietojen menettämisen ilmetä, jos sinun on käytettävä tätä menetelmää yksinään.

**Voit palauttaa tietokannan geo palauttaminen:**

- [Azure-portaalin](https://azure.microsoft.com/) **Uusi**, valitse **tiedot ja tallennustilaa**, valitse **SQL-tietokanta**ja valitse lähteenä tietokannan **Varmuuskopiointi** . Katso lisätietoja [palauttaa käyttökatkosta Azure SQL-tietokannan](sql-database-disaster-recovery.md) .