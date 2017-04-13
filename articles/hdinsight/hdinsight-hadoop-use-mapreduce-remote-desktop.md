<properties
   pageTitle="MapReduce ja etätyöpöydän kanssa Hadoop HDInsight | Microsoft Azure"
   description="Opettele käyttämään etätyöpöydän muodostaa yhteyden Hadoop HDInsight-ja suorita MapReduce työt."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Käytä MapReduce Hadoop-HDInsight etätyöpöydän kanssa

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Tässä artikkelissa kerrotaan yhdistäminen Hadoop HDInsight-klusterissa Etätyöpöydän avulla ja suorita MapReduce työt Hadoop-komennon avulla.

##<a id="prereq"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

* Windows-pohjaisesta HDInsight (Hadoop-HDInsight)-klusterin

* Asiakastietokone, Windows 10: ssä, Windows 8: ssa tai Windows 7: ssä

##<a id="connect"></a>Etätyöpöytä yhteydessä

Etätyöpöytä käyttöön HDInsight-klusterin ja muodostaa siihen voidaan [muodostaa käyttämällä RDP HDInsight klustereihin](hdinsight-administer-use-management-portal.md#rdp)ohjeita noudattamalla.

##<a id="hadoop"></a>Hadoop-komennolla

Kun yhteys on muodostettu HDInsight-klusterin työpöydälle, seuraavien vaiheiden avulla voit suorittaa MapReduce työn Hadoop-komennolla:

1. HDInsight työpöydältä aloittaa **Hadoop komentoriviltä käsin**. Tämä avaa uuden komentokehote- **c:\apps\dist\hadoop-&lt;versionumero >** hakemisto.

    > [AZURE.NOTE] Versionumero muuttuu, kun Hadoop päivitetään. **HADOOP_HOME** -ympäristömuuttuja avulla voidaan etsiä polku. Esimerkiksi `cd %HADOOP_HOME%` muuttuu kansioiden tarvitsematta tietää versionumero Hadoop-kansioon.

2. **Hadoop** -komennon avulla voit suorittaa Esimerkki-MapReduce työn, käytä seuraavaa komentoa:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    Tämä käynnistää **wordcount** -luokka, joka sisältyy **hadoop-mapreduce-examples.jar** tiedoston nykyiseen kansioon. Syötteeksi, se käyttää **wasbs://example/data/gutenberg/davinci.txt** asiakirjan ja tulos on tallennettu: **wasbs: / / / Esimerkki/tietojen/WordCountOutput**.

    > [AZURE.NOTE] Saat lisätietoja MapReduce työn ja esimerkkitietoja <a href="hdinsight-use-mapreduce.md">Käytön MapReduce HDInsight Hadoop</a>.

2. Työn tietokoneesta kuuluu äänimerkki tiedot se käsitellään, ja se palauttaa tiedot seuraavankaltaiselta työn ollessa:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Kun työ on valmis, käytä seuraavaa komentoa **wasbs://example/data/WordCountOutput**säilytettävä tulosteen tiedostojen luetteloa:

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    Tämä pitäisi näyttää kaksi tiedostoa, **_SUCCESS** ja **osa-r-00000**. **Osa-r-00000** tiedosto sisältää tuloste työlle.

    > [AZURE.NOTE] Jotkin MapReduce työt voi jakaa tuloksia **osa-r-###** useita tiedostoja. Jos näin on, ### jälkiliite osoittamaan järjestyksen tiedostot.

4. Voit tarkastella tulosteen, käytä seuraavaa komentoa:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Tämä näyttää luettelon sanat, jotka sisältyvät **wasbs://example/data/gutenberg/davinci.txt** tiedoston, ja kuinka monta kertaa kunkin sanan tapahtui. Seuraavassa on esimerkki tiedot, jotka tiedoston sisältämän:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Yhteenveto

Kuten näet, Hadoop-komento on suorittaa MapReduce työt HDInsight-klusterin ja tarkastelemalla työn tulosteen helposti.

##<a id="nextsteps"></a>Seuraavat vaiheet

Yleisiä tietoja MapReduce työt HDInsight:

* [MapReduce käyttäminen HDInsight Hadoop](hdinsight-use-mapreduce.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight:

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)
