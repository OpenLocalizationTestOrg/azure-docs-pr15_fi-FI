<properties 
    pageTitle="Kopioi käyttämällä Transact-SQL Azure SQL-tietokanta | Microsoft Azure" 
    description="Luo käyttämällä Transact-SQL Azure SQL-tietokanta" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-transact-sql"></a>Kopioi käyttämällä Transact-SQL Azure SQL-tietokantaan


> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-copy.md)
- [Azure portal](sql-database-copy-portal.md)
- [PowerShellin](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)


Tämä seuraavasti näyttää, miten voit kopioida Transact-SQL SQL-tietokanta samaan palvelimeen tai toiseen palvelimeen. Kopioi tietokantatoiminnon käyttää [Luominen DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) -lause.

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

- Azure tilaus. Jos tarvitset Azure tilauksen yksinkertaisesti Valitse **MAKSUTTOMAN kokeiluversion** tämän sivun yläosassa ja valitse palaa valmis tässä artikkelissa.
- Azure SQL-tietokanta. Jos sinulla ei ole SQL-tietokantaan, luo yhden noudattamalla tämän artikkelin: [ensimmäisen Azure SQL-tietokannan luominen](sql-database-get-started.md).
- [SQL Server Management Studiossa (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx). Jos sinulla ei ole SSMS tai jos tässä artikkelissa kuvatut ominaisuudet eivät ole käytettävissä, [Lataa uusin versio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="copy-your-sql-database"></a>Kopioi SQL-tietokanta

Kirjaudu perusmuodon tietokannan palvelintason pääasiallista kirjautuminen tai kirjautumista varten luotu tietokanta, jonka haluat kopioida. Kirjautumiset, jotka eivät ole palvelintason lyhennys on oltava dbmanager-roolin jäsenet voidaksesi kopioida tietokannat. Saat lisätietoja kirjautumiset ja muodostetaan yhteys palvelimeen [hallinta kirjautumiset](sql-database-manage-logins.md).

Aloita kopioiminen lähdetietokannan sisältävä [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) -lause. Suoritetaan tietosuojatiedoissa aloittaa tietokannan kopioiminen prosessi. Koska tietokannan kopioiminen on asynkronisen prosessin, Luo DATABASE-lause palauttaa ennen kuin tietokanta on valmis kopioidaan.


### <a name="copy-a-sql-database-to-the-same-server"></a>Kopioi SQL-tietokanta-palvelimeen

Kirjaudu perusmuodon tietokannan palvelintason pääasiallista kirjautuminen tai kirjautumista varten luotu tietokanta, jonka haluat kopioida. Kirjautumiset, jotka eivät ole palvelintason lyhennys on oltava dbmanager-roolin jäsenet voidaksesi kopioida tietokannat.

Uuden tietokannan voin Tämä komento kopiot Database1 nimeltä Database2 samaan palvelimeen. Tietokannan koon mukaan kopiointi kestää kauan.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Kopioi SQL-tietokannan eri palvelimeen

Kirjaudu kohdepalvelin Azure SQL-tietokanta-palvelin, missä uusi tietokanta on luoda perusmuodon tietokantaan. Käytä kirjautuminen, jolla on sama nimi ja salasana tietokannan omistaja (DBO) lähdetietokannan lähde Azure SQL-tietokanta-palvelimeen. Kirjautuminen kohdesähköpostipalvelimesta on myös dbmanager-roolin jäsen tai olla palvelintason principal kirjautuminen.

Tämä komento kopioi palvelin1-Database1 nimeltä Database2 palvelin2 uuteen tietokantaan. Tietokannan koon mukaan kopiointi kestää kauan.


    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;
    

## <a name="monitor-the-progress-of-the-copy-operation"></a>Kopiointi etenemisen seuranta

Valvoa kopiointia kyselyt sys.databases ja sys.dm_database_copies näkymät. Kun kopiointi on käynnissä, uuden tietokannan sys.databases-näkymän state_desc-sarakkeessa on määritetty kopioiminen.


- Jos kopiointi epäonnistuu, sys.databases näkymän uuden tietokannan state_desc-sarakkeen arvo on LUOTETTAVA. Tässä tapauksessa suorittaa DROP-lauseen uuden tietokannan ja yritä uudelleen myöhemmin.
- Jos kopiointi onnistuu, sys.databases näkymän uuden tietokannan state_desc-sarakkeen arvo on ONLINE. Tässä tapauksessa kopiointi on valmis, ja uusi tietokanta on tavanomaisessa tietokannassa voi muuttaa lähdetietokannan riippumatta.

> [AZURE.NOTE] – Jos haluat peruuttaa kopioinnin ollessa käynnissä, suorita [PUDOTA DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) -lause uuden tietokannan. Voit myös suoritetaan PUDOTA DATABASE-lause Valitse lähdetietokannan myös peruuttaa kopiointia.


## <a name="resolve-logins-after-the-copy-operation-completes"></a>Ratkaise kirjautumiset kopioinnin jälkeen

Uuden tietokannan jälkeen online kohdesähköpostipalvelimesta uudelleen uuden tietokannan käyttäjiltä kirjautumiset kohdesähköpostipalvelimesta [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) -lause avulla. Voit ratkaista yhteydetön käyttäjät-kohdassa [Vianmääritys yhteydetön Users](https://msdn.microsoft.com/library/ms175475.aspx). Katso myös [Azure SQL-tietokannan suojauksen jälkeen palauttaminen hallintaa](sql-database-geo-replication-security-config.md).

Kaikki käyttäjät uuteen tietokantaan säilyttää, jotka ovat samat kuin lähdetietokannan käyttöoikeudet. Tietokannan kopion aloittanut käyttäjä tulee tietokannan omistaja uuden tietokannan, ja se on määritetty suojaustunnus (SUOJAUSTUNNUS). Kun kopiointi on luotu ja ennen kuin muut käyttäjät ovat määrittää uudelleen, kirjautuminen, joka aloitti kopioiminen, tietokannan omistaja (DBO), voit kirjautua uuden tietokannan.


## <a name="next-steps"></a>Seuraavat vaiheet

- Saat yleiskuvan kopioiminen Azure SQL-tietokantaan [kopioida Azure SQL-tietokantaan](sql-database-copy.md) .
- Katso Azure-portaalissa tietokannan kopioiminen [Kopioi Azure-portaalissa Azure SQL-tietokantaan](sql-database-copy-portal.md) .
- Kohdassa kopioi tietokanta käyttämällä PowerShell [Kopioi Azure SQL-tietokantaan PowerShellin avulla](sql-database-copy-powershell.md) .
- Katso lisätietoja käyttäjien ja kirjautumiset hallinta, kun tietokannan kopioiminen toiseen looginen palvelimeen [Azure SQL-tietokannan suojauksen jälkeen palauttaminen hallintaa](sql-database-geo-replication-security-config.md) .



## <a name="additional-resources"></a>Lisäresursseja

- [Kirjautumiset hallinta](sql-database-manage-logins.md)
- [Yhteyden muodostaminen SQL-tietokantaa, jossa SQL Server Management Studiossa ja suorittaa otoksen T-SQL-kysely](sql-database-connect-query-ssms.md)
- [Vie tietokantaan BACPAC](sql-database-export.md)
- [Liiketoiminnan jatkuvuus yhteenveto](sql-database-business-continuity.md)
- [SQL-tietokanta-asiakirjat](https://azure.microsoft.com/documentation/services/sql-database/)


