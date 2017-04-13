<properties
   pageTitle="MapReduce ja SSH Hadoop HDInsight-yhteyttä | Microsoft Azure"
   description="Opettele käyttämään SSH suorittamaan MapReduce töitä Hadoop käyttäminen Hdinsightista."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>MapReduce käyttäminen Hadoop-HDInsight SSH kanssa

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Tässä artikkelissa kerrotaan käyttämisestä suojattu runko (SSH) Hadoop HDInsight-klusterissa yhdistäminen ja lähettää MapReduce työt Hadoop komennoilla.

> [AZURE.NOTE] Jos olet jo aiemmin Linux-pohjaiset Hadoop palvelinten käyttäminen, mutta ole aiemmin käyttänyt HDInsight-kohdassa [Linux-pohjaiset HDInsight vihjeitä](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

* Klusterin Linux-pohjaiset HDInsight (Hadoop-HDInsight)

* SSH-asiakas. Linux, Unix ja Mac-käyttöjärjestelmissä noudetaan SSH avulla. Windows-käyttäjien on ladattava asiakas, kuten [painovärit, muste](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Yhdistä SSH

Yhteyden muodostaminen täydellinen toimialuenimi (FQDN) HDInsight-klusterin SSH-komennon avulla. FQDN on annoit klusterin ja sen jälkeen **. azurehdinsight.net**. Esimerkiksi seuraavat muodostamaan nimeltä **myhdinsight**klusterin:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Jos antamasi varmenteen avaimen SSH todennusta varten** , kun olet luonut HDInsight-klusterin, joudut ehkä määrittää yksityinen avain sijainnin asiakas-järjestelmään, esimerkiksi:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Jos antamasi salasanan SSH todennustavaksi** HDInsight-klusterin luonnin yhteydessä, tarvitset Anna pyydettäessä salasana.

SSH käyttämisestä HDInsight, katso lisätietoja [Käytön SSH Linux-pohjaiset Hadoop HDInsight Linux, OS X-ja Unix-kanssa](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-clients"></a>Painovärit, muste (Windows-asiakkaat)

Windows ei tarjoa valmiin SSH asiakas. On suositeltavaa käyttää **painovärit, muste**, joka voi ladata [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Lisätietoja painovärit, muste on artikkelissa [Käyttäminen SSH kanssa Linux-pohjaiset Hadoop-HDInsight Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hadoop"></a>Hadoop-komentojen käyttäminen

1. Kun olet muodostanut yhteyden HDInsight-klusterin, MapReduce työn aloittaminen seuraavat **Hadoop** -komennon avulla:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Tämä käynnistää **wordcount** -luokka, joka sisältyy **hadoop-mapreduce-examples.jar** -tiedosto. Syötteeksi, se käyttää **wasbs://example/data/gutenberg/davinci.txt** asiakirjan ja tulos on tallennettu **wasbs: / / / Esimerkki/tietojen/WordCountOutput**.

    > [AZURE.NOTE] Saat lisätietoja MapReduce työn ja esimerkkitietoja [Käytön MapReduce Hadoop-HDInsight](hdinsight-use-mapreduce.md).

2. Työn tietokoneesta kuuluu äänimerkki tiedot käsittelee, ja se palauttaa tiedot seuraavankaltaiselta kun työ on valmis:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Kun työ on valmis, käytä seuraavaa komentoa luettelon tulosteen tiedostot, jotka tallennetaan **wasbs://example/data/WordCountOutput**:

        hdfs dfs -ls wasbs:///example/data/WordCountOutput

    Tämä pitäisi näyttää kaksi tiedostoa, **_SUCCESS** ja **osa-r-00000**. **Osa-r-00000** tiedosto sisältää tuloste työlle.

    > [AZURE.NOTE] Jotkin MapReduce työt voi jakaa tuloksia **osa-r-###** useita tiedostoja. Jos näin on, ### jälkiliite osoittamaan järjestyksen tiedostot.

4. Voit tarkastella tulosteen, käytä seuraavaa komentoa:

        hdfs dfs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Tämä näyttää luettelon sanat, jotka sisältyvät **wasbs://example/data/gutenberg/davinci.txt** -tiedosto ja kuinka monta kertaa kunkin sanan tapahtui. Seuraavassa on esimerkki tiedot, jotka tiedoston sisältämän:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Yhteenveto

Kuten näet, Hadoop-komentojen avulla on helppo MapReduce töiden suorittaminen HDInsight-klusterin ja tarkastelemalla työn tulos.

##<a id="nextsteps"></a>Seuraavat vaiheet

Yleisiä tietoja MapReduce työt HDInsight:

* [MapReduce käyttäminen HDInsight Hadoop](hdinsight-use-mapreduce.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight:

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)
