<properties
    pageTitle="Tietokannan suorituskyvyn Azure SQL-tietokantaan valvominen | Microsoft Azure"
    description="Tutustu erilaisiin tietokantasi Azure työkalut ja dynaaminen hallinta näkymien seurantaa varten."
    keywords="tietokannan cloud tietokannan suorituskyvyn valvominen"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/27/2016"
    ms.author="carlrab"/>

# <a name="monitoring-database-performance-in-azure-sql-database"></a>Tietokannan suorituskyvyn Azure SQL-tietokantaan valvominen
SQL Azure-tietokannassa suorituskyvyn valvominen alkaa seuranta resurssien käyttö suhteessa tietokannan suorituskykyä valitset taso. Seuranta auttaa määrittämään, onko tietokannan ylimääräisen kapasiteetti tai on on ongelmia, koska resurssit on saavutettu ja päättää, onko aikaa Säädä suorituskyky ja tietokannan [palvelutaso](sql-database-service-tiers.md) . Voit valvoa tietokannan graafisia työkaluja [Azure portal](https://portal.azure.com) -tai SQL- [dynaaminen hallinta näkymiä](https://msdn.microsoft.com/library/ms188754.aspx).

## <a name="monitor-databases-using-the-azure-portal"></a>Näytön tietokantojen Azure-portaalissa

[Azure portaalissa](https://portal.azure.com/)voit valvoa yhden tietokannan käytön valitsemalla tietokannan ja napsauttamalla **Seuranta** -kaavio. Tämä näyttää **arvo** -ikkuna, jossa voit muuttaa **Kaavion Muokkaa** -painiketta. Lisää seuraavat arvot:

- Suorittimen prosentti
- DTU prosentteina
- Tietoja IO prosentti
- Tietokannan koko prosentteina

Kun olet lisännyt nämä arvot, voit edelleen tarkastella niitä **Seuranta** -kaavio, jossa on lisätietoja **metrijärjestelmä** -ikkunassa. Kaikki neljä arvot Näytä tietokannan keskimääräinen käyttö prosentteina suhteessa **DTU** . Lisätietoja DTUs artikkelissa [palvelutasot](sql-database-service-tiers.md) .

![Palvelun taso seuranta tietokannan suorituskykyä.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Voit myös määrittää ilmoituksia suorituskyky-arvojen mukaisesti. **Arvo** -ikkunassa valitsemalla **Lisää-ilmoitus** . Noudata ohjatun toiminnon määrittäminen ilmoituksen. Sinulla on ilmoitus voi, jos määritetty ylittävät tietty määrä tai jos lisätiedot tietty määrä.

Esimerkiksi jos työmäärää kasvattamista tietokannassa, voit määrittää sähköposti-ilmoituksen aina, kun tietokanta saavuttaa 80 % mihin tahansa suorituskyvyn mittarit. Voit käyttää tätä kuin aikainen varoitus selvittää, sinun on ehkä siirtyä seuraavaan suorituskyvyn ylemmälle tasolle.

Suorituskyvyn mittarit avulla voit määrittää, jos et pysty muuntaminen suorituskyvyn alemmalle tasolle. Oletetaan, että käytössäsi on vakio S2 tietokannan ja Näytä kaikki suorituskyvyn mittarit, että tietokanta avataan keskiarvo ei käytä yli 10 prosenttia annetun milloin tahansa. On todennäköistä, että tietokanta toimivat hyvin vakio S1. Kuitenkin huomioon toiminnoista, jotka kasvamaan tai keskiarvokurssin ennen kuin teet päätös Siirrä alemmalle tasolle suorituskykyä.

## <a name="monitor-databases-using-dmvs"></a>Valvonta-tietokantoja, joissa DMVs

Saman mittarit, joka on määritetty portaalissa ovat käytettävissä järjestelmän näkymissä myös: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) looginen **perusmuodon** tietokannan Serverin ja [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) käyttäjän tietokannan. Käytä **sys.resource_stats** , jos haluat seurata vähemmän hajautetun tietojen yli pitkän ajanjakson aikana. Käytä **sys.dm_db_resource_stats** , jos haluat seurata eritellympiä tietojen pienempi aikarajan puitteissa. Lisätietoja on artikkelissa [Azure SQL tietokannan suorituskykyä ohjeita](sql-database-performance-guidance.md#monitoring-resource-use-with-sysresourcestats).

>[AZURE.NOTE] **sys.dm_db_resource_stats** palauttaa tyhjän tuloksen, joka määrittää, kun sitä käytetään Internetin kautta tai Business edition tietokantoja, jotka on poistettu käytöstä.

Joustavasti tietokannan jakavat voit valvoa altaan yksittäisiä tietokantojen tässä osassa kuvataan tapoja, joilla kanssa. Mutta voit myös valvoa kokonaiskoko varanto. Lisätietoja on artikkelissa [näyttö ja hallita joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool-manage-portal.md).
