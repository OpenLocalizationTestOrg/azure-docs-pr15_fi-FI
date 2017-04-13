<properties
    pageTitle="Kopioi Azure SQL-tietokanta | Microsoft Azure"
    description="Luoda kopion Azure SQL-tietokantaan"
    services="sql-database"
    documentationCenter=""
    authors="anosov1960"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/24/2016"
    ms.author="sstein; sashan"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database"></a>Kopioi Azure SQL-tietokanta

> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-copy.md)
- [Azure portal](sql-database-copy-portal.md)
- [PowerShellin](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Azure [SQL-tietokantaan automaattisen varmuuskopioinnin](sql-database-automated-backups.md) avulla voit luoda kopion SQL-tietokantaan. Tietokannan kopion käyttää samaa tekniikkaa geo replikoinnin-ominaisuus. Mutta geo replikoinnin toisin kuin se päättyy replikoinnin-linkki kuin kun seeding vaihe on valmis. Kopioi tietokannan siksi tilannevedoksen lähdetietokannan kopioi pyynnön aikana.  
Voit luoda tietokannan kopion samaan palvelimeen tai eri palvelimeen. Service tason ja suorituskyvyn tason (hinnoittelu taso) tietokannan kopion ovat samat kuin lähdetietokannan oletusarvoisesti. Kun Ohjelmointirajapinnan käyttäminen voit valita eri suorituskyvyn tason sisällä palvelun samalla tasolla (edition). Kun kopio on valmis, kopioi tulee täysin riippumaton tietokanta. Tässä vaiheessa voit päivittää tai muuntaminen mikä tahansa versio. Kirjautumiset, käyttäjät ja käyttöoikeudet voidaan hallita erikseen.  

Kun kopioit tietokannan looginen palvelimeen, sama kirjautumiset voidaan molemmat tietokannat. Lainan voit kopioida tietokanta käyttämällä suojauksen tulee tietokannan omistaja (DBO) uuden tietokannan. Tietokannan kaikki käyttäjät, käyttöoikeudet ja suojauksen tunnuksista (SID) kopioidaan tietokannan kopion.  

Kun kopioit tietokannan eri looginen palvelimeen, pääasiallista uudessa palvelimessa suojaus tulee tietokannan omistaja uuden tietokannan. Jos käytät [sisälsi tietokannan käyttäjiä](sql-database-manage-logins.md) varten tietojen käytön varmistaa ensisijainen ja toissijainen tietokantoja on aina käyttäjän tunnistetietoja, kopioi päättymisen jälkeen voit heti käyttää sitä samoilla tunnuksilla. Jos käytät [Azure Active Directory](../active-directory/active-directory-whatis.md), voit sulkea kokonaan edellyttää kopioi tunnistetietojen hallinta. Kun kopioit tietokannan uuteen palvelimeen, kirjautuminen mukaan access yleensä eivät toimi, koska kirjautumiset ei ole uudessa palvelimessa. Katso lisätietoja hallinnasta kirjautumiset kopioitaessa tietokannan eri looginen palvelimeen [Azure SQL-tietokannan suojauksen jälkeen palauttaminen hallintaa](sql-database-geo-replication-security-config.md) . 

Kopioi SQL-tietokantaan, tarvitset seuraavat:

- Azure tilaus. Jos tarvitset Azure tilauksen yksinkertaisesti Valitse **MAKSUTTOMAN kokeiluversion** tämän sivun yläosassa ja valitse toinen käyttäjä palaa lomaltaan Lopeta tämän artikkelin.
- Kopioi SQL-tietokantaan. Jos sinulla ei ole SQL-tietokantaan, luo yhden noudattamalla tämän artikkelin: [ensimmäisen Azure SQL-tietokannan luominen](sql-database-get-started.md).

## <a name="next-steps"></a>Seuraavat vaiheet

- Katso Azure-portaalissa tietokannan kopioiminen [Kopioi Azure-portaalissa Azure SQL-tietokantaan](sql-database-copy-portal.md) .
- Kohdassa kopioi tietokanta käyttämällä PowerShell [Kopioi Azure SQL-tietokantaan PowerShellin avulla](sql-database-copy-powershell.md) .
- Kohdassa käyttämällä Transact-SQL-tietokannan kopioiminen [Kopioi Azure SQL-tietokantaan käyttämällä T-SQL](sql-database-copy-transact-sql.md) .
- Katso lisätietoja käyttäjien ja kirjautumiset hallinta, kun tietokannan kopioiminen toiseen looginen palvelimeen [Azure SQL-tietokannan suojauksen jälkeen palauttaminen hallintaa](sql-database-geo-replication-security-config.md) .



## <a name="additional-resources"></a>Lisäresursseja

- [Kirjautumiset hallinta](sql-database-manage-logins.md)
- [Yhteyden muodostaminen SQL-tietokantaa, jossa SQL Server Management Studiossa ja suorittaa otoksen T-SQL-kysely](sql-database-connect-query-ssms.md)
- [Vie tietokantaan BACPAC](sql-database-export.md)
- [Liiketoiminnan jatkuvuus yhteenveto](sql-database-business-continuity.md)
- [SQL-tietokanta-asiakirjat](https://azure.microsoft.com/documentation/services/sql-database/)
