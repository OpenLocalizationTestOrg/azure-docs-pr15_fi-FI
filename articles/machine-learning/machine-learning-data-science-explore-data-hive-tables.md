<properties
    pageTitle="Tietojen rakenne taulukoiden rakennetta kyselyjen avulla | Microsoft Azure"
    description="Tutustu rakenteen taulukoiden rakennetta kyselyjen käyttäminen tietojen."
    services="machine-learning"
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Tietojen rakenne taulukoiden rakennetta kyselyjen avulla

Tässä tiedostossa on rakenteen komentosarjamallit, joiden avulla voit tarkastella tietoja HDInsight Hadoop-klusterin taulukoiden rakennetta.

**Valikon** linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit käyttää työkaluja tietojen tutkiminen tallennustilan eri ympäristöissä.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Edellytykset
Tässä artikkelissa oletetaan, että sinulla on:

* Luoda Azure-tallennustilan tilin. Jos tarvitset ohjeita, katso [Azure-tallennustilan tilin luominen](../storage/storage-create-storage-account.md#create-a-storage-account)
* Valmisteltu mukautetun Hadoop-klusterin HDInsight-palvelussa. Jos tarvitset ohjeita, katso [Mukauttaminen Azure Hdinsightiin Hadoop klustereiden Advanced analysoinnissa](machine-learning-data-science-customize-hadoop-cluster.md).
* Tiedot on ladattu Azure Hdinsightiin Hadoop klustereiden rakenne-taulukoihin. Jos se ei ole, noudata ohjeita [luominen ja lataa tiedot rakenteen taulukoihin](machine-learning-data-science-move-hive-tables.md) tietojen lataaminen rakenteen taulukoiden ensin.
* Käytössä klusterin etäkäyttö. Jos tarvitset ohjeita, on artikkelissa [Head solmu, Hadoop klusterin](machine-learning-data-science-customize-hadoop-cluster.md#headnode).
* Jos tarvitset ohjeita siitä, miten voit lähettää rakenteen kyselyt, katso, [miten voit lähettää kyselyjä rakenne](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Esimerkki rakenteen kyselyn komentosarjoja tietojen tarkasteluun

1. Hae huomioita kohti osion määrä `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. Hae päivässä havaintojen määrä `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`

3. Hae categorical sarakkeen tasot  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. Hae haluamasi tasojen määrä kaksi categorical sarakkeen yhdistelmä `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. Numeeristen sarakkeiden jakauman hankkiminen  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. Pura tietueiden liittymästä kaksi taulukkoa

        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Kyselyn komentosarjat taksin työmatkan tietojen skenaariot

Esimerkkejä kyselyitä, jotka ovat [Kokousesitelmän taksin työmatkan tietojen](http://chriswhong.com/open-data/foil_nyc_taxi/) skenaariot toimitetaan myös [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)säilöön. Nämä kyselyt on jo on määritetty tietojen rakenne, ja olet valmis suorittamaan toimitettavat.
