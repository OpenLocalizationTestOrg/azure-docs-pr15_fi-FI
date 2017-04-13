<properties
    pageTitle="Keskeyttäminen ja jatkaminen tietojen siirtäminen (Venytys-tietokanta) | Microsoft Azure"
    description="Opettele keskeyttäminen ja jatkaminen Azure tietojen siirto."
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
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="pause-and-resume-data-migration-stretch-database"></a>Keskeyttäminen ja jatkaminen tietojen siirtäminen (Venytys-tietokanta)

Keskeyttäminen ja jatkaminen tietojen siirto Azure, valitse **Venytys** taulukon SQL Server Management Studiossa ja valitse sitten pysähtyvän tietojen siirtäminen **keskeyttäminen** ja **jatkaminen** , kun haluat jatkaa tietojen siirtäminen. Voit myös käyttää Transact\-keskeyttäminen ja jatkaminen tietojen siirtäminen SQL.

Keskeytä tietojen siirtäminen, kun haluat paikallisen palvelimen liittyviä ongelmia tai suurenna käytettävissä kaistanleveys yksittäisiä tauluja.

## <a name="pause-data-migration"></a>Keskeytä tietojen siirtäminen

### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>Siirrä osoitin tietojen siirtäminen SQL Server Management Studiossa avulla

1.  Valitse SQL Server Management Studiossa, objektin Resurssienhallinnassa Venytys\-taulukkoa, jonka haluat keskeyttää tietojen siirtäminen käytössä.

2.  Oikealle\-napsauttamalla ja valitse **Venytys**ja valitse sitten **Keskeytä**.

### <a name="use-transact-sql-to-pause-data-migration"></a>Käytä Transact\-pysähtyvän tietojen siirtäminen SQL
Suorita seuraava komento.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## <a name="resume-data-migration"></a>Jatka tietojen siirtäminen

### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>Jatka tietojen siirtäminen SQL Server Management Studiossa avulla

1.  Valitse SQL Server Management Studiossa, objektin Resurssienhallinnassa Venytys\-taulukkoa, jonka haluat jatkaa tietojen siirtäminen käytössä.

2.  Oikealle\-napsauttamalla ja valitse **Venytys**ja valitse sitten **Jatka**.

### <a name="use-transact-sql-to-resume-data-migration"></a>Käytä Transact\-, kun haluat jatkaa tietojen siirtäminen SQL
Suorita seuraava komento.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## <a name="check-whether-migration-is-active-or-paused"></a>Tarkista, onko siirron aktiivinen tai keskeytetty

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>SQL Server Management Studiossa avulla voit tarkistaa, onko siirron aktiivinen tai keskeytetty
SQL Server Management Studiossa Avaa **Venytys tietokannan näyttö** ja **Siirron tila** -sarakkeen arvo. Saat lisätietoja, [näyttö ja vianmääritys tietojen siirtäminen](sql-server-stretch-database-monitor.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Tarkista, onko siirron aktiivinen tai keskeytetty Transact-SQL avulla
Kyselyn luettelon näkymän **sys.remote_data_archive_tables** ja tarkista **is_migration_paused** -sarakkeen arvo. Saat lisätietoja, [sys.remote_data_archive_tables](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="see-also"></a>Katso myös

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
[näyttö ja vianmääritys tietojen siirtäminen](sql-server-stretch-database-monitor.md)
