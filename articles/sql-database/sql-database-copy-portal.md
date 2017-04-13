<properties
    pageTitle="Kopioi Azure-portaalissa Azure SQL-tietokanta | Microsoft Azure"
    description="Luoda kopion Azure SQL-tietokantaan"
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



# <a name="copy-an-azure-sql-database-using-the-azure-portal"></a>Kopioi SQL Azure-tietokannan Azure-portaalissa

> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-copy.md)
- [Azure portal](sql-database-copy-portal.md)
- [PowerShellin](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Seuraavat vaiheet näyttää, miten voit kopioida [Azure portal](https://portal.azure.com) SQL-tietokanta samaan palvelimeen tai toiseen palvelimeen.

Kopioi SQL-tietokantaan, tarvitset seuraavat:

- Azure tilaus. Jos tarvitset Azure tilauksen yksinkertaisesti Valitse **MAKSUTTOMAN kokeiluversion** tämän sivun yläosassa ja valitse toinen käyttäjä palaa lomaltaan Lopeta tämän artikkelin.
- Kopioi SQL-tietokantaan. Jos sinulla ei ole SQL-tietokantaan, luo yhden noudattamalla tämän artikkelin: [ensimmäisen Azure SQL-tietokannan luominen](sql-database-get-started.md).


## <a name="copy-your-sql-database"></a>Kopioi SQL-tietokanta

Avaa tietokanta, jonka haluat kopioida SQL-tietokanta-sivun seuraavasti:

1.  Siirry [Azure portal](https://portal.azure.com).
2.  Valitse **Lisää palveluja** > **SQL-tietokantoja**, ja valitse sitten haluamasi tietokanta.
3.  Valitse **Kopioi**SQL-tietokanta-sivulla:

    ![SQL-tietokantaan](./media/sql-database-copy-portal/sql-database-copy.png)

1.  **Kopioi** -sivulla tietokannan oletusnimi on annettu. Kirjoita toinen nimi, jos haluat (kaikki tietokannat palvelimessa on oltava yksilöllinen nimi).
2.  Valitse **kohde-palvelimeen**. Kohde on jossa tietokannan kopion on luotu. Voit kopioida tietokannan samaan palvelimeen tai eri palvelimeen. Voit luoda palvelimen tai valitse olemassa olevan palvelimen luettelosta. 
3.  Kun olet valinnut **kohteen palvelimeen**, **joustavasti tietokannan resurssivarantoon**ja **hinnoittelu taso** asetukset otetaan käyttöön. Jos palvelin on resurssivarantoon, voit kopioida tietokannan.
3.  Valitse **OK** käynnistääksesi kopiointi.

    ![SQL-tietokantaan](./media/sql-database-copy-portal/copy-page.png)


## <a name="monitor-the-progress-of-the-copy-operation"></a>Kopiointi etenemisen seuranta

- Kopioi käynnistyksen jälkeen napsauttamalla lisätietoja portaalin ilmoitusta.

    ![ilmoitus][3]
 
    ![valvonta][4]


## <a name="verify-the-database-is-live-on-the-server"></a>Tarkista tietokannan on live palvelimessa

- Valitse **Lisää palveluja** > **SQL-tietokannat** ja vahvista uusi tietokanta on **Online**.


## <a name="resolve-logins"></a>Ratkaise kirjautumiset

Voit ratkaista kirjautumiset kopiointi päätyttyä, katso [ratkaista kirjautumiset](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="next-steps"></a>Seuraavat vaiheet

- Saat yleiskuvan kopioiminen Azure SQL-tietokantaan [kopioida Azure SQL-tietokantaan](sql-database-copy.md) .
- Kohdassa kopioi tietokanta käyttämällä PowerShell [Kopioi Azure SQL-tietokantaan PowerShellin avulla](sql-database-copy-powershell.md) .
- Kohdassa käyttämällä Transact-SQL-tietokannan kopioiminen [Kopioi Azure SQL-tietokantaan käyttämällä T-SQL](sql-database-copy-transact-sql.md) .
- Katso lisätietoja käyttäjien ja kirjautumiset hallinta, kun tietokannan kopioiminen toiseen looginen palvelimeen [Azure SQL-tietokannan suojauksen jälkeen palauttaminen hallintaa](sql-database-geo-replication-security-config.md) .



## <a name="additional-resources"></a>Lisäresursseja

- [Kirjautumiset hallinta](sql-database-manage-logins.md)
- [Yhteyden muodostaminen SQL-tietokantaa, jossa SQL Server Management Studiossa ja suorittaa otoksen T-SQL-kysely](sql-database-connect-query-ssms.md)
- [Vie tietokantaan BACPAC](sql-database-export.md)
- [Liiketoiminnan jatkuvuus yhteenveto](sql-database-business-continuity.md)
- [SQL-tietokanta-asiakirjat](https://azure.microsoft.com/documentation/services/sql-database/)




<!--Image references-->
[1]: ./media/sql-database-copy-portal/copy.png
[2]: ./media/sql-database-copy-portal/copy-ok.png
[3]: ./media/sql-database-copy-portal/copy-notification.png
[4]: ./media/sql-database-copy-portal/monitor-copy.png

