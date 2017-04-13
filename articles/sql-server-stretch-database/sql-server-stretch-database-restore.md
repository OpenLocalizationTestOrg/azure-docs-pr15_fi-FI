<properties
    pageTitle="Palauttaa Venytys käytössä tietokannoissa | Microsoft Azure"
    description="Opettele palauttamaan Venytys\-käytössä tietokannoissa."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="douglasl"/>

# <a name="restore-stretch-enabled-databases"></a>Palauttaa Venytys käyttävä tietokannat

Palauttaa varmuuskopioidun tietokannan tarvittaessa palauttaa useita erityyppisiä epäonnistuu, virheet ja lieventämiseksi.

Saat lisätietoja Varmuuskopiointi [varmuuskopion Venytys käytössä tietokannoissa](sql-server-stretch-database-backup.md).

>   [AZURE.NOTE] Varmuuskopiointi on vain yksi osa valmis suuren käytettävyyden ja liiketoiminnan jatkuvuuden ratkaisu. Saat lisätietoja suuren käytettävyyden [Suuri käytettävyys ratkaisuja](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="restore-your-sql-server-data"></a>SQL Server-tietojen palauttaminen
Palauttaa laitteistovirhe tai vioittumisen, Palauta Venytys\-SQL Server-tietokannan varmuuskopiosta. Voit edelleen käyttää SQL Server-palauttaminen menetelmiä, jota käytät parhaillaan. Saat lisätietoja, [palauttaminen ja palautus yleiskatsaus](https://msdn.microsoft.com/library/ms191253.aspx).

Kun palautat SQL Server-tietokantaan, sinun on käytettävä tallennetun toimintosarjan **sys.sp_rda_reauthorize_db** muodostetaan uudelleen Venytys välinen yhteys\-otettu käyttöön SQL Server-tietokantaan ja Azure etätietokantaan. Saat lisätietoja, [Palauta SQL Server-tietokantaan ja Azure etätietokanta välinen yhteys](#restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database).

## <a name="restore-your-remote-azure-data"></a>Remote Azure tietojen palauttaminen

### <a name="recover-a-live-azure-database"></a>Palauttaa reaaliaikaisia Azure-tietokanta
SQL Server-Venytys tietokannan palveluun Azure tilannevedoksia kaikki ajantasaisten tietojen vähintään kerran Azure-tallennustilan tilannevedosten käyttäminen kahdeksan tuntia. Nämä tilannevedoksia ylläpidetään 7 päivää. Voit palauttaa tiedot vähintään 21 pisteen senhetkinen ylöspäin kellonajan, kun viimeinen tilannevedoksen on otettu viimeisten seitsemän päivän kuluessa.

Voit palauttaa reaaliaikaisia Azure-tietokanta aikaisempaan samanaikaisesti käyttämällä Azure portaalin tekemällä seuraavat kohdat.

1. Kirjaudu sisään Azure-portaaliin.
2. Näytön vasemmassa reunassa valitsemalla **SELAA** ja valitse sitten **SQL-tietokannat**.
3. Siirry tietokanta ja valitse se.
4. Tietokannan sivu yläreunassa valitsemalla **Palauta**.
5. Uuden **tietokannan nimi**, valitse **Palauta-kohtaa** ja valitse sitten **Luo**.
6. Tietokannan palautus alkaa, ja se voidaan valvoa **ilmoitukset**.

### <a name="recover-a-deleted-azure-database"></a>Palauta poistetut Azure-tietokanta
Azure SQL Server-Venytys tietokanta-palvelun kestää tietokannan tilannevedoksen, ennen kuin tietokantaan katkeaa ja säilyttää 7 päivää. Tämän jälkeen säilyy enää tilannevedoksia live-tietokannasta. Voit palauttaa poistetun tietokannan pisteeseen, kun se on poistettu.

Jos haluat palauttaa poistetun Azure-tietokannan pisteeseen, kun se on poistanut Azure-portaalissa, tee seuraavat kohdat.

1. Kirjaudu sisään Azure-portaaliin.
2. Näytön vasemmassa reunassa valitsemalla **SELAA** ja valitse sitten **SQL-palvelimiin**.
3. Siirry palvelimellesi ja valitse se.
4. Vieritä palvelimen sivu toimenpiteet, **Poistetut tietokantojen** -ruutua.
5. Valitse haluat palauttaa poistetun tietokannan.
5. Uuden **tietokannan nimi** ja valitse **Luo**.
6. Tietokannan palautus alkaa, ja se voidaan valvoa **ilmoitukset**.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a>Palauttaa SQL Server-tietokantaan ja Azure etätietokanta välinen yhteys

1.  Jos aiot yhdistäminen palautettu Azure tietokannan eri nimellä tai toisella alueella, suorita tallennetun toimintosarjan [sys.sp_rda_deauthorize_db](https://msdn.microsoft.com/library/mt703716.aspx) katkaiseminen edellisen Azure-tietokanta.  

2.  Suorita uudelleen paikallisen Venytys tallennetun toimintosarjan [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) \-tietokannan Azure-tietokantaan.  

    -   Anna aiemmin määritetty tietokannan tunnistetieto sysname tai varchar\(128\) arvo. \(Älä käytä varchar\(max\).\) Voit etsiä tunnistetietojen nimi näkymän **sys.database\_kohdistettu\_tunnistetietojen**.  

    -   Määritä remote tietojen kopioiminen ja muodostaa yhteyttä (suositus) kopioi.  

    ```tsql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO
    ```  

## <a name="see-also"></a>Katso myös

[Hallinta ja Venytys tietokannan vianmääritys](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Varmuuskopiointi ja palauttaminen SQL Server-tietokannat](https://msdn.microsoft.com/library/ms187048.aspx)
