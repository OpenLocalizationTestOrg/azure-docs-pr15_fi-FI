<properties
    pageTitle="SQL-tietokantaan joustavasti resurssivarantoon hinnan ja suorituskyky"
    description="Tietyn joustavasti tietokannan jakavat hintatiedot."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="05/27/2016"
    ms.author="srinia"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="elastic-database-pool-billing-and-pricing-information"></a>Joustavasti tietokannan resurssivarantoon laskutus- ja hintatiedot

Joustavasti tietokannan jakavat ovat laskuttaa kohti seuraavat ominaisuudet:

- Joustavasti resurssivarantoon laskutetaan sen luomisen yhteydessä, vaikka altaan ei ole tietokantoja.
- Joustavasti resurssivarantoon laskutetaan kerran tunnissa. Tämä on yhtä kuin suorituskyvyn tasot yksi tietokantojen niin, että usein.
- Jos joustavasti resurssivarantoon kokoa muutetaan eDTUs uusi määrä, sitten ryhmän ei laskutetaan eDTUS uuden summan mukaan, kunnes kokoa toiminto on valmis. Tämä seuraa samoissa muuttuva erillinen tietokantojen suorituskyvyn taso.


- Joustavasti resurssivarantoon hinta perustuu eDTUs altaan määrä. Hinta joustavasti varanto on erillinen luku ja sen sisältämät joustavasti tietokantojen hyödyntäminen.
- Hinta lasketaan mukaan (resurssivarantoon eDTUs määrä) x (eDTU yksikköhinnan).

Joustavasti resurssivarantoon eDTU Yksikköhinta on suurempi kuin palvelun samalla tasolla erillinen tietokannan DTU Yksikköhinta. Lisätietoja on artikkelissa [SQL-tietokantaan hinnat](https://azure.microsoft.com/pricing/details/sql-database/). 


EDTUs ja palvelun tasoa on artikkelissa [SQL-tietokanta-asetukset ja suorituskykyä](sql-database-service-tiers.md).

## <a name="next-steps"></a>Seuraavat vaiheet

- [Joustavasti tietokanta-ryhmän luominen](sql-database-elastic-pool-create-portal.md)
- [Seurata ja hallita joustavasti tietokannan resurssivarantoon kokoa](sql-database-elastic-pool-manage-portal.md)
- [SQL-tietokanta-asetukset ja suorituskyky: ymmärtää, mikä on käytettävissä palvelun kunkin tason](sql-database-service-tiers.md)
- [PowerShell-komentosarjaa yksilöimiseen tietokantojen sopii joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool-database-assessment-powershell.md)
