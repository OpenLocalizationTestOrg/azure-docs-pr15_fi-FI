<properties
    pageTitle="Siirrä tietokannat palvelinten välillä tilaukset ja Azure-ja uloskirjautuminen."
    description="Vaiheittaiset ohjeet, jos haluat kopioida, siirtää ja siirtää tiedot ja tietokantojen Azure SQL-tietokantaan."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="move-databases-between-servers-between-subscriptions-and-in-and-out-of-azure"></a>Siirrä tietokannat palvelinten välillä tilaukset ja Azure-ja uloskirjautuminen

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]
##<a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Tietokannan siirtäminen toiseen saman tilauksen palvelimeen
- [Azure-portaalin](https://portal.azure.com) **SQL-tietokantoja**, valitse tietokanta luettelosta ja valitse sitten **Kopioi**. Saat tarkempia tietoja [kopioida Azure SQL-tietokantaan](sql-database-copy.md) .

## <a name="to-move-a-database-between-subscriptions"></a>Tietokannan siirtäminen tilausten välillä
- [Azure-portaali](https://portal.azure.com)valitsemalla **SQL-palvelimet** ja valitse sitten luettelosta tietokannan isännöivän palvelimen. Valitse **Siirrä**ja valitse sitten Siirrä resurssit ja siirry tilaus.

## <a name="to-migrate-a-sql-database-into-azure"></a>Siirtää Azure SQL-tietokanta
- Määrittää tietokannan yhteensopivuus ja valitse tarvittavat oikean siirtomenetelmä. Seuraa ohjeita ja asetusten [siirtäminen SQL Server-tietokantaan](sql-database-cloud-migrate.md).

## <a name="to-create-a-copy-of-a-database-for-use-outside-of-azure"></a>Jos haluat luoda tietokannan käyttöä Azure ulkopuolella
- [Vie BACPAC-tiedosto.](sql-database-export.md)
