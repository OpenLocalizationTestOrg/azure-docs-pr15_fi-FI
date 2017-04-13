<properties
    pageTitle="Mallitiedot Azure Hdinsightiin rakenne taulukoiden | Microsoft Azure"
    description="Näyte Azure Hdinsightiin (Hadopop) rakenne taulukoiden tietojen alaspäin"
    services="machine-learning,hdinsight"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />

# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Mallitiedot Azure Hdinsightiin rakenne-taulukoissa

Tässä artikkelissa on kuvataan, miten alas otoksen rakenteen käyttäminen Azure Hdinsightiin rakenne taulukoihin tallennetut tiedot. Käsittelemme popularly käytetyt esimerkkejä kolmella tavalla:

* Yhtenäisen satunnaisia esimerkkejä
* Satunnainen esimerkkejä ryhmien mukaan
* Käsittelemätöntä esimerkkejä

**Esimerkki siitä, miksi tiedot?**
Jos haluat analysoida tietojoukko on suuri, se on yleensä hyvä alas otoksen voit pienentää koko on pienempi, mutta edustava ja helpommin hallittaviin tiedot. Tämä helpottaa tietojen tietoja, tarkasteluun ja ominaisuus tekniikka. Ryhmän tietojen tiede prosessin sen rooli on nopea prototyyppiä tietojen käsittely-toimintojen ja konepohjaisten oppimistekniikoiden mallit käyttöön.

**Valikon** alla linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit eri tallennustilan ympäristöissä mallitiedot.

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Esimerkkejä tehtävä on vaiheessa [Ryhmän tietojen tiede prosessi (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="how-to-submit-hive-queries"></a>Voit lähettää rakenne-kyselyt
Rakenne-kyselyt voi lähettää Hadoop-klusterin pään solmu Hadoop komentorivi-konsolin. Voit tehdä tämän Kirjaudu Hadoop-klusterin pään solmu, Avaa Hadoop komentorivi-konsolin ja Lähetä sieltä rakenne-kyselyt. Ohjeita rakenteen kyselyitä Hadoop komentoriviltä konsolissa Katso, [miten voit lähettää kyselyjä rakenne](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="uniform"></a>Yhtenäisen satunnaisia esimerkkejä
Yhtenäisen satunnaisia esimerkkejä tarkoittaa, että tietojoukon kunkin rivin näyte on yhtä kuin mahdollisuutta. Tämä voidaan toteuttaa lisäämällä ylimääräisen kentän Funktiot-rand() tietojoukon sisemmän "select" kyselyn ja ulompi "select" kyselyn ehto satunnaisia kentälle.

Tässä on esimerkki kyselyn:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Tässä kohdassa `<sample rate, 0-1>` määrittää tietueet, jotka käyttäjät haluat Esimerkki osuus.

## <a name="group"></a>Satunnainen esimerkkejä ryhmien mukaan

Kun esimerkkejä categorical tietoja, voit halutessasi voit joko lisätä tai poistaa kaikki jonkin tietyn arvon categorical muuttujan esiintymät. Tämä on mitä tarkoitus on "näytteiden ryhmittäin" perusteella.
Esimerkiksi jos categorical muuttujan "Tilan", jossa on arvot NY, MA, CA, NJ, PA jne, jolle samaan tilaan tietueiden aina yhteen, onko ne näyte vai ei.

Näin Esimerkki kyselyn kyseisen objektit-ryhmän:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Käsittelemätöntä esimerkkejä

Satunnainen esimerkkejä on stratified osalta categorical muuttujan kun saatujen näytteiden sisältää arvot, että categorical, jotka ovat samassa suhteessa kuin ylemmän tason populaation, josta malleja on saatu. Edellisen esimerkin kuin edellä oletetaan, että käytössä on aliraportti populaatio tilan mukaan, NJ on 100 huomioita, NY on 60 huomioita ja WA on 300 huomautukset. Jos määrität suuruuden käsittelemätöntä esimerkkejä on 0,5, valitse saatu otosten on noin 50, 30 ja 150 huomioita NJ, NY ja WA tarpeen mukaan.

Tässä on esimerkki kyselyn:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
            rank() over (partition by state order by rand()) as state_rank
        from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Lisätietoja monimutkaisempia esimerkkejä tavoista, jotka ovat käytettävissä rakenne on artikkelissa [LanguageManual esimerkkejä](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).
