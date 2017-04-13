<properties
    pageTitle="SQL-tietokannan suorituskyvyn säätö vihjeitä | Microsoft Azure"
    description="Vinkkejä suorituskyvyn säätö Azure SQL-tietokantaan arviointi ja Käyttömukavuuden."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="SQL-suorituskyvyn säätö tietokannan suorituskyvyn säätö sql-suorituskyvyn säätö vinkkejä on sql-tietokannan suorituskyvyn säätö"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-database-performance-tuning-tips"></a>SQL-tietokannan suorituskyvyn säätäminen vihjeitä
Voit muuttaa yksittäisen tietokannan [palvelutaso](sql-database-service-tiers.md) tai suurenna joustavasti tietokannan resurssivarantoon eDTUs suorituskyvyssä milloin tahansa, mutta voit parantaa ja parantaa kyselyn suorituskykyä ensin mahdollisuuksista. Huonosti optimoitu kyselyjen ja puuttuu indeksit on yleisimpiä syitä heikentävät tietokannan suorituskykyä. Tässä artikkelissa on ohjeita suorituskyvyn säätö SQL-tietokantaan.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="steps-to-evaluate-and-tune-database-performance"></a>Arvioi ja hienosäätää tietokannan suorituskykyä
1.  [Azure-portaalin](https://portal.azure.com) **SQL-tietokantoja**, valitse tietokanta ja Etsi resursseja lähellä niiden enintään seuranta-kaavion avulla. DTU kulutus näkyy oletusarvoisesti. Valitse **Muokkaa** , jos haluat muuttaa aikavälin ja arvot.
2.  Arvioi kyselyjen DTUs [Kyselyn suorituskykyä tietoja](sql-database-query-performance.md) avulla ja tarkastella suosituksia luominen ja poistetaan indeksejä, kyselyparametrien ja rakenteen ongelmien korjaaminen [SQL-tietokannan Advisorin](sql-database-advisor.md) avulla.
3.  Voit dynaaminen hallinta näkymien (DMVs), laajennettu tapahtumat (Xevents) ja SSMS kyselyn kauppa saat suorituskyvyn parametrit reaaliaikainen. On yksityiskohtainen seuranta-ja säätäminen vihjeitä [suorituskyvyn ohjeet aihe](sql-database-performance-guidance.md) .


    > [AZURE.IMPORTANT] On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="steps-to-improve-database-performance-with-more-resources"></a>Voit parantaa tietokannan suorituskykyä ja muita resursseja vaiheet
1.  Yksittäisen tietokantoja voit [muuttaa palvelutasot](sql-database-scale-up.md) Ohjattu tietokannan suorituskyvyn parantamiseksi.
2.  Useiden tietokantojen kannattaa käyttää [joustavasti tietokannan jakavat](sql-database-elastic-pool-guidance.md) skaalata resurssit automaattisesti.
