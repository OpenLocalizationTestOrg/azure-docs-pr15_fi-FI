<properties
    pageTitle="Azure SQL-tietokannan resurssien rajoitukset"
    description="Tällä sivulla kerrotaan joitakin yleisiä resurssien rajoitukset Azure SQL-tietokantaan."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="10/13/2016"
    ms.author="carlrab" />


# <a name="azure-sql-database-resource-limits"></a>Azure SQL-tietokantaan resurssien rajoitukset

## <a name="overview"></a>Yleiskatsaus

Azure SQL-tietokantaan hallitsee tietokanta käyttämällä kahta eri järjestelmiä resursseista: **Resurssit** - ja **Rajoitukset käyttäminen**. Tässä ohjeaiheessa kerrotaan nämä kaksi pääalueille siirtyminen Resurssienhallinta.

## <a name="resource-governance"></a>Resurssin hallinnon
Yksi Basic, Vakio ja Premium-palvelutasot tavoitteista rakenne on Azure SQL-tietokantaan toimimaan kuin, jos tietokanta on käynnissä omassa tietokoneessa-kokonaan eristetään muista tietokannoista. Resurssin hallinnon emuloi tämän ongelman. Jos koostetun resurssien käyttö saavuttaa käytettävissä suorittimen, muistin, loki i/o ja tietojen i/o tietokannan varattujen resurssien, resurssi hallinnon jonon kyselyjen suorittaminen ja resurssien varaaminen jonossa olevien kyselyjen, kun ne vapauttaminen.

Kuin oma koneessa käyttävien kaikki käytettävissä olevat resurssit johtaa pidempi toteuttamiseen suorittavien kyselyjä, jotka saattavat johtaa komennon aikakatkaisu asiakkaan. Kohdetoimialueen uudelleen logiikan sovellusten ja sovelluksia, jotka suorittavat kyselyitä, jotka perustuvat suuri usein tietokanta saattaa ilmetä virheitä viestien suoritettaessa uusia kyselyjä, kun samanaikaiset pyynnöt enimmäismäärä on saavutettu.

### <a name="recommendations"></a>Suosituksia:
Seurata resurssien käytön sekä keskimääräinen vastauksen ajat, kyselyt, kun lähestyvä tietokannan suurinta käyttöastetta. Kun kohtaat suurempi kyselyn viiveitä sinulla on yleensä kolmesta vaihtoehdosta:

1.  Vähentää pyynnöt estää aikakatkaisu ja kasa ylös-pyyntöihin-tietokantaan.

2.  Määritä tietokannan suorituskykyä ylemmälle tasolle.

3.  Optimoi kyselyjen vähentää resurssien käytön kunkin kyselyn. Lisätietoja on artikkelissa Azure SQL tietokannan suorituskykyä ohjeet kyselyn säädön/Hinting-osassa.

## <a name="enforcement-of-limits"></a>Rajoitukset käyttäminen
Resurssien kuin suorittimen, muistin Log i/o tai tietojen i/o toimialuerekisteröintipalvelun estäminen uusia pyyntöjä, kun rajat saavutetaan. Asiakkaiden saavat [virhesanoma](sql-database-develop-error-messages.md) mukaan rajoitus, joka on saavutettu.

Esimerkiksi yhteyksien SQL-tietokantaan ja samanaikaiset pyynnöt voi käsitellä määrää on rajoitettu. SQL-tietokannan avulla voi olla suurempi kuin samanaikaiset pyynnöt tukemaan yhteysvarannon määrä tietokantaan yhteyksien määrä. Aikana käytettävissä olevat yhteydet määrä voi säätää helposti sovellus, rinnakkain pyynnöt määrä on usein näyttävyyttä arvioida sekä kellonajat. Erityisesti piikin Lataa aikana kun sovellus, joko lähettää liikaa pyynnöt tai tietokannan saavuttaa sen resurssin rajoittaa ja käynnistää kasaantuu työntekijän viestiketjuissa siirtyminen vuoksi pidempi käynnissä kyselyt-virheitä voi esiintyä.

## <a name="service-tiers-and-performance-levels"></a>Palvelutasot ja suorituskyvyn tasoon

On palvelutasot ja suorituskyvyn käyttöoikeustasot, jotka molemmat erillinen tietokanta ja Lisää joustavasti jakavat.

### <a name="standalone-databases"></a>Erillisestä tietokannat

Itsenäisen-tietokannan tietokannan rajat on määritetty tietokannan palvelun taso ja suorituskyvyn tason mukaan. Seuraavassa taulukossa on kuvattu Basic, Vakio ja Premium ominaispiirteet tietokantoja on useita suorituskykyä.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

### <a name="elastic-pools"></a>Joustavasti jakavat

[Joustavasti jakavat](sql-database-elastic-pool.md) Jaa resurssit yli ryhmän ominaisuudet. Seuraavassa taulukossa on kuvattu Basic, Vakio ja Premium joustavasti tietokannan jakavat ominaisuudet.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Kullekin resurssille edellisen taulukoissa laajennettu määrittely artikkelissa kuvatuista sen [palvelun taso ominaisuudet ja rajoitukset](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). Saat yleiskatsauksen palvelutasot [Azure SQL-tietokannan palvelutasot ja suorituskyvyn tasoa](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>SQL-tietokannan muut rajoitukset

| Alue | Raja | Kuvaus |
|---|---|---|
| Tietokantojen käyttäminen automaattisen Vie tilausta kohti | 10 | Automaattinen Vie avulla voit luoda mukautettuja SQL-tietokantoja varmuuskopioiminen aikataulu. Lisätietoja on artikkelissa [SQL-tietokantoja: Automaattinen SQL-tietokannan viennin tuki](http://weblogs.asp.net/scottgu/windows-azure-july-updates-sql-database-traffic-manager-autoscale-virtual-machines).|
| Tietokannan palvelinta kohti | Korkeintaan 5 000 | Enintään 5000 tietokantojen sallitaan Vipuventtiili V12 palvelimissa palvelinta kohti. |  
| DTUs palvelinta kohti | 45000 | 45000 DTUs ovat käytettävissä Vipuventtiili V12 palvelimissa valmistelu tietokantoja, joustavasti jakavat ja tietojen varastoja palvelinta kohti. |



## <a name="resources"></a>Resurssit

[Azure tilaus ja palvelun rajoitukset, kiintiön ja rajoitukset](../azure-subscription-service-limits.md)

[Azure SQL-tietokannan palvelutasot ja suorituskyvyn tasoon](sql-database-service-tiers.md)

[SQL-tietokanta-asiakasohjelmien virhesanomat](sql-database-develop-error-messages.md)
