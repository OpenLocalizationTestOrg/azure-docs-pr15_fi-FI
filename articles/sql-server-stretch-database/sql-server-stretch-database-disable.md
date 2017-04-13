<properties
    pageTitle="Venytä tietokannan käytöstä ja takaisin näkyviin remote data | Microsoft Azure"
    description="Lisätietoja taulukon Venytys tietokannan käytöstä ja voit myös tuoda takaisin remote tietoja."
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
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Venytä tietokannan käytöstä ja takaisin näkyviin remote data

Venytä tietokannan käytöstä taulukon, valitse **Venytys** SQL Server Management Studiossa taulukon. Valitse jokin seuraavista vaihtoehdoista.

-   **Käytöstä | Tietojen tuominen takaisin Azure**. Kopioi taulukko Azure remote tiedot takaisin SQL Server käytöstä Venytys tietokannan sitten taulukko. Tämä toiminto liittyviä tietoja siirron kustannukset ja sitä ei voi peruuttaa.

-   **Käytöstä | Jätä tietojen Azure**. Käytöstä Venytys tietokannan taulukkoon.  Hylkää remote Azure-taulukon tiedot.

Voit myös käyttää Transact\-SQL Venytys tietokannan taulukon tai tietokannan poistaminen käytöstä.

Kun Venytys tietokannan käytöstä taulukon tietojen siirron pysähtyy ja kyselytulokset enää sisällä tuloksia remote-taulukosta.

Jos haluat jättää keskeyttää tietojen siirtäminen, katso [keskeyttäminen ja jatkaminen Venytys tietokannan](sql-server-stretch-database-pause.md).

>   [AZURE.NOTE] Venytä tietokannan poistaminen käytöstä lisääminen taulukkoon tai tietokantaan ei poista remote objekti. Jos haluat poistaa remote taulukon tai etätietokantaan, sinun on pudota se käyttämällä Azure hallinta-portaalin. Remote-objekteille Siirry maksamaan Azure kustannukset, kunnes poistat ne. Saat lisätietoja, [SQL Server Venytys tietokannan hinnat](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-table"></a>Taulukon Venytys tietokannan poistaminen käytöstä

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>Käytä SQL Server Management Studiossa käytöstä Venytys tietokannan taulukkoon

1.  Valitse SQL Server Management Studiossa, objektin Resurssienhallinnassa, jonka haluat poistaa käytöstä Venytys tietokannan taulukkoon.

2.  Oikealle\-napsauttamalla ja valitse **Venytys**ja valitse sitten jokin seuraavista vaihtoehdoista.

    -   **Käytöstä | Tietojen tuominen takaisin Azure**. Kopioi taulukko Azure remote tiedot takaisin SQL Server käytöstä Venytys tietokannan sitten taulukko. Tämä komento ei voi peruuttaa.

        >   [AZURE.NOTE] Remote tietojen kopioiminen taulukon azuren SQL Server takaisin veloitetaan tietojen siirto kustannukset. Saat lisätietoja, [Tietojen siirrot hinnat tiedot](https://azure.microsoft.com/pricing/details/data-transfers/).

        Kun kaikki remote tiedot on kopioitu Azure takaisin SQL Server-Venytys ei ole käytössä taulukko.

    -   **Käytöstä | Jätä tietojen Azure**. Käytöstä Venytys tietokannan taulukkoon.  Hylkää remote Azure-taulukon tiedot.

    >   [AZURE.NOTE] Venytä tietokannan poistaminen käytöstä taulukon Poista remote tietoja tai remote taulukon. Jos haluat poistaa remote taulukon, sinun on pudota se käyttämällä Azure hallinta-portaalin. Remote taulukon säilyy maksamaan Azure kustannukset, kunnes poistat sen. Saat lisätietoja, [SQL Server Venytys tietokannan hinnat](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Käytä Transact\-SQL käytöstä Venytys tietokannan taulukkoon

-   Venytä käytöstä taulukon ja kopioi kaukosäätimen Azure taulukon tiedot takaisin SQL Serveristä, suorita seuraava komento. Kun kaikki remote tiedot on kopioitu Azure takaisin SQL Server-venyttää ei ole käytössä taulukko.

    Tämä komento ei voi peruuttaa.

    ```tsql
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    GO
    ```
    >   [AZURE.NOTE] Remote tietojen kopioiminen taulukon azuren SQL Server takaisin veloitetaan tietojen siirto kustannukset. Saat lisätietoja, [Tietojen siirrot hinnat tiedot](https://azure.microsoft.com/pricing/details/data-transfers/).

-   Venytä käytöstä taulukon ja hylätä remote data, suorita seuraava komento.

    ```tsql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

>   [AZURE.NOTE] Venytä tietokannan poistaminen käytöstä taulukon Poista remote tietoja tai remote taulukon. Jos haluat poistaa remote taulukon, sinun on pudota se käyttämällä Azure hallinta-portaalin. Remote taulukon säilyy maksamaan Azure kustannukset, kunnes poistat sen. Saat lisätietoja, [SQL Server Venytys tietokannan hinnat](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-database"></a>Tietokannan Venytys tietokannan poistaminen käytöstä
Ennen kuin voit poistaa käytöstä Venytys tietokannan tietokantaan, sinun on yksittäisiä Venytys Venytys-tietokannan poistaminen käytöstä\-käytössä tietokannan taulukot.

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>Venytä tietokannan käytöstä tietokannan SQL Server Management Studiossa avulla

1.  Valitse tietokanta, jonka haluat poistaa käytöstä Venytys tietokannan SQL Server Management Studiossa, objektin Resurssienhallinnassa.

2.  Oikealle\-napsauttamalla ja valitse **tehtävät**- ja valitse sitten **Venytys**ja valitse sitten **Poista käytöstä**.

>   [AZURE.NOTE] Venytä tietokannan poistaminen käytöstä tietokantaan ei poista etätietokantaan. Jos haluat poistaa etätietokanta, sinun on pudota se käyttämällä Azure hallinta-portaalin. Etätietokanta säilyy maksamaan Azure kustannukset, kunnes poistat sen. Saat lisätietoja, [SQL Server Venytys tietokannan hinnat](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Käytä Transact\-SQL Venytys tietokannan käytöstä tietokannan
Suorita seuraava komento.

```tsql
ALTER DATABASE <database name>
    SET REMOTE_DATA_ARCHIVE = OFF ;
```

>   [AZURE.NOTE] Venytä tietokannan poistaminen käytöstä tietokantaan ei poista etätietokantaan. Jos haluat poistaa etätietokanta, sinun on pudota se käyttämällä Azure hallinta-portaalin. Etätietokanta säilyy maksamaan Azure kustannukset, kunnes poistat sen. Saat lisätietoja, [SQL Server Venytys tietokannan hinnat](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="see-also"></a>Katso myös

[Muuta TIETOKANNAN asetusten määrittäminen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[Keskeyttäminen ja jatkaminen Venytys tietokanta](sql-server-stretch-database-pause.md)
