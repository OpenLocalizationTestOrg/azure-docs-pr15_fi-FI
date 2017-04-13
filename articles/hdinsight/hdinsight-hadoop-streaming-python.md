<properties
   pageTitle="Kehittää Python MapReduce töiden HDInsight | Microsoft Azure"
   description="Opettele luominen ja suorittaminen-Linux-pohjaiset HDInsight klustereiden Python MapReduce työt."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="develop-python-streaming-programs-for-hdinsight"></a>Kehittää Python streaming ohjelmat HDInsight varten

Hadoop tarjoaa streaming API MapReduce, jonka avulla voit kirjoittaa kartta ja vähentää kielet kuin Java-funktiot. Tässä artikkelissa kerrotaan, miten Python avulla voit suorittaa MapReduce.

> [AZURE.NOTE] Kun Python koodin tässä asiakirjassa voidaan käyttää Windows-pohjaisesta HDInsight-klusterin, tämän asiakirjan ohjeita ovat Linux-pohjaiset klustereiden.

Tässä artikkelissa perustuu tiedot ja esimerkit Michael Noll osoitteessa [kirjoittaminen Python Hadoop MapReduce ohjelman](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/)julkaisija.

##<a name="prerequisites"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

* Linux-pohjaiset Hadoop HDInsight-klusterissa

* Tekstieditorissa

    > [AZURE.IMPORTANT] Tekstieditorissa on käyttää LF rivin loppuun. Jos se on käytössä CRLF, tämä aiheuttaa virheitä suoritettaessa MapReduce työn Linux-pohjaiset HDInsight klustereiden. Jos et ole varma, Muunna kaikki CRLF LF [Suorita MapReduce](#run-mapreduce) -osassa valinnainen vaihe käyttämällä.

* Painovärit, muste ja PSCP Windows-asiakkaille. Näiden apuohjelmien ovat käytettävissä <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">Painovärit, muste lataussivulta</a>.

##<a name="word-count"></a>Sanamäärä

Tässä esimerkissä Toteuta basic sanamäärän laskeminen käyttämällä mapper ja reducer. Mapper jakaa lauseiden yksittäisiksi sanoiksi ja reducer kokoaa sanat ja laskee, tulos.

Seuraava vuokaavio kuvaa, mitä tapahtuu, jos kartan aikana ja vähentää vaiheet.

![kartan kuva pienentäminen](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

##<a name="why-python"></a>Miksi Python?

Python on yleinen, korkean tason ohjelmointikieli, jonka avulla voit express käsitteiden vähemmän kuin muiden kielten koodin rivit. Se on viimeksi tuli Suositut tietojen tutkijoiden käyttöönotetuista prototyyppiä ja koska sen tulkittava laatu, dynaaminen kirjoittamalla ja tyylikkäitä syntaksi ansiosta sopii nopea sovellusten kehittäminen.

Kaikki HDInsight klustereiden Python on asennettu.

##<a name="streaming-mapreduce"></a>Streaming MapReduce

Voit määrittää kartan sisältävän tiedoston ja vähentää logiikan, jota käytetään työn Hadoop. Kartan tietyn vaatimukset ja vähentää logiikan ovat:

* **Syötteen**: kartan ja vähentää osat on luettava syöttötiedot standardisyötteestä.

* **Tulos**: kartan ja vähentää osat on kirjoitettava siirtää tietoja STDOUT.

* **Tietojen muoto**: kulutettu ja tuottaa tietojen on oltava avain/arvo pair erotettu sarkainmerkin.

Python helposti voit käsitellä näitä vaatimuksia käyttämällä **sys** -moduulin STDIN lukea ja **Tulosta** voit tulostaa STDOUT. Jäljellä oleva tehtävä on vain muotoilu merkinnästä tiedot (`\t`) merkki avain ja arvon välillä.

##<a name="create-the-mapper-and-reducer"></a>Luo mapper ja reducer

Mapper ja reducer ovat tekstitiedostoja, palvelupyynnön **mapper.py** ja **reducer.py** (minkä ansiosta Poista joka onko mitä). Voit luoda näihin valittua editorilla.

###<a name="mapperpy"></a>Mapper.PY

Luo uusi tiedosto nimeltä **mapper.py** ja käyttää sisältöä seuraava koodi:

    #!/usr/bin/env python

    # Use the sys module
    import sys

    # 'file' in this case is STDIN
    def read_input(file):
        # Split each line into words
        for line in file:
            yield line.split()

    def main(separator='\t'):
        # Read the data using read_input
        data = read_input(sys.stdin)
        # Process each words returned from read_input
        for words in data:
            # Process each word
            for word in words:
                # Write to STDOUT
                print '%s%s%d' % (word, separator, 1)

    if __name__ == "__main__":
        main()

Lue koodin avulla, jotta ymmärrät kuvaus hetki.

###<a name="reducerpy"></a>Reducer.PY

Luo uusi tiedosto nimeltä **reducer.py** ja käyttää sisältöä seuraava koodi:

    #!/usr/bin/env python

    # import modules
    from itertools import groupby
    from operator import itemgetter
    import sys

    # 'file' in this case is STDIN
    def read_mapper_output(file, separator='\t'):
        # Go through each line
        for line in file:
            # Strip out the separator character
            yield line.rstrip().split(separator, 1)

    def main(separator='\t'):
        # Read the data using read_mapper_output
        data = read_mapper_output(sys.stdin, separator=separator)
        # Group words and counts into 'group'
        #   Since MapReduce is a distributed process, each word
        #   may have multiple counts. 'group' will have all counts
        #   which can be retrieved using the word as the key.
        for current_word, group in groupby(data, itemgetter(0)):
            try:
                # For each word, pull the count(s) for the word
                #   from 'group' and create a total count
                total_count = sum(int(count) for current_word, count in group)
                # Write to stdout
                print "%s%s%d" % (current_word, separator, total_count)
            except ValueError:
                # Count was not a number, so do nothing
                pass

    if __name__ == "__main__":
        main()

##<a name="upload-the-files"></a>Lataa tiedostot

**Mapper.py** ja **reducer.py** on pään solmun klusterin, ennen kuin niitä emme voi suorittaa. Helpoin tapa ladata ne on käytettävä **scp** (**pscp** Jos käytössäsi on Windows-asiakas).

Käytä seuraavaa komentoa-asiakasohjelmassa **mapper.py** ja **reducer.py**samassa kansiossa. Korvaa **käyttäjänimi** SSH käyttäjän ja **clustername** yhteyttä klusterin nimi.

    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:

Tämä kopioi tiedostot paikallisesta järjestelmästä pään solmu.

> [AZURE.NOTE] Jos olet käyttänyt salasana suojaa SSH tili, voit pyydetään antamaan salasana. Jos olet käyttänyt SSH-näppäintä, saatat joutua käyttämään `-i` parametrin ja yksityinen avain, esimerkiksi polku `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

##<a name="run-mapreduce"></a>Suorita MapReduce

1. Yhteyden muodostaminen klusterin SSH avulla:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Jos olet käyttänyt salasana suojaa SSH tili, voit pyydetään antamaan salasana. Jos olet käyttänyt SSH-näppäintä, saatat joutua käyttämään `-i` parametrin ja yksityinen avain, esimerkiksi polku `ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`.

2. (Valinnainen) Jos olet käyttänyt tekstieditorissa, joka käyttää CRLF mapper.py ja reducer.py-tiedostojen luomiseen, jonka pääte rivinä tai ei tiedä, mitä rivi-loppu editorin käyttää, käyttää seuraavaa komennot esiintymää CRLF mapper.py ja reducer.py muuntaminen LF.

        perl -pi -e 's/\r\n/\n/g' mappery.py
        perl -pi -e 's/\r\n/\n/g' reducer.py

2. Seuraavalla komennolla voit käynnistää MapReduce työn.

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input wasbs:///example/data/gutenberg/davinci.txt -output wasbs:///example/wordcountout

    Tämä komento on seuraavat osat:

    * **hadoop streaming.jar**: käytetään, kun streaming MapReduce-toimintoja. Se liittymät Hadoop antaisit ulkoisen MapReduce koodin.

    * **-tiedostoja**: kertoo määritetyn tiedostot tarvitaan MapReduce työn, ja ne olisi kopioi työntekijä solmujen Hadoop.

    * **-mapper**: kertoo Hadoop mapper käyttää tiedosto.

    * **-reducer**: kertoo Hadoop reducer käyttää tiedosto.

    * **-syötteen**: tiedostosta, joka on olisi lasketaan sanoja kohteesta.

    * **-tulosteen**: hakemiston, tulos on kirjoitettu.

        > [AZURE.NOTE] Tämä kansio luodaan projektin.

On useita **tiedot** väittämistä työn käynnistyy ja katso lopuksi **kartta** ja **vähentää** toiminto näkyy prosentteina.

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

Saat projektin tilatietoja, kun se on valmis.

##<a name="view-the-output"></a>Näytä tulos

Kun työ on valmis, seuraava komento avulla voit tarkastella tulos:

    hdfs dfs -text /example/wordcountout/part-00000

Tämä näyttää luettelon sanojen ja kuinka monta kertaa sana on tapahtunut. Seuraavassa on esimerkki tiedot:

    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt oppinut voit streaming MapRedcue työt käyttäminen HDInsight-seuraavat linkkien avulla voit tutkia muita esimerkkejä Azure Hdinsightiin.

* [Rakenteen käyttäminen Hdinsightiin](hdinsight-use-hive.md)
* [Possu käyttäminen Hdinsightiin](hdinsight-use-pig.md)
* [MapReduce työt käyttäminen Hdinsightiin](hdinsight-use-mapreduce.md)
