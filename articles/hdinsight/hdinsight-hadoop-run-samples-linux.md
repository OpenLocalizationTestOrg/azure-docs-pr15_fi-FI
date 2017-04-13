<properties
    pageTitle="Suorita Hadoop MapReduce näytteiden Linux-pohjaiset HDInsight | Microsoft Azure"
    description="Aloita käyttäminen MapReduce näytteiden Linux-pohjaiset Hdinsightista. SSH avulla voit muodostaa yhteyttä klusterin ja valitse Suorita otoksen työt Hadoop-komennon avulla."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>




#<a name="run-the-hadoop-samples-in-hdinsight"></a>Suorita HDInsight Hadoop-mallit

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Linux-pohjaiset HDInsight klustereiden antaa MapReduce esimerkkejä, joiden avulla voit tutustua Hadoop MapReduce töitä. Tässä asiakirjassa tietoja käytettävissä olevat mallit ja käy läpi muutamat käynnissä.

##<a name="prerequisites"></a>Edellytykset

- **Azure tilaus**: Katso [Hae Azure ilmainen kokeiluversio](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

- **A Linux-pohjaiset HDInsight-klusterin**: Katso [Hadoop Linux HDInsight-rakenne ja käytön aloittaminen](hdinsight-hadoop-linux-tutorial-get-started.md)

- **SSH asiakkaan**: Lisätietoja SSH käyttäminen Hdinsightista on seuraavissa artikkeleissa:

    - [Linux-pohjaiset Hadoop HDInsight Linux, Unix tai OS X-SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [SSH käyttäminen Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="the-samples"></a>Mallit ##

**Sijainti**: mallit sijaitsevat osoitteessa **/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar** HDInsight-klusterin

**Sisältö**: arkisto sisältää seuraavat esimerkit:

- **aggregatewordcount**: kooste perusteella kartta ja vähentää ohjelmaan, joka laskee sanat input-tiedostoissa
- **aggregatewordhist**: kooste perusteella kartta ja vähentää ohjelmaan, joka laskee syötteen tiedostot sanoja Histogrammi
- **bbp**: kartta ja vähentää ohjelma käyttää Bailey Borwein-Plouffe laskemiseen piin vertaa numeroa
- **dbcount**: esimerkki-työstä, joka laskee Sivunäkymä lokitiedostot on tallennettu tietokantaan
- **distbbp**: kartta ja vähentää ohjelma käyttää laskemiseen tarkka bittien piin BBP tyyppi-kaava
- **grep**: kartta ja vähentää ohjelmaan, joka laskee regex Input vastineet
- **Liity**: työn, joka tehosteet liitoksen yli lajiteltu, osioitu tasaisesti tietojoukkoja
- **multifilewc**: työn, joka laskee sanat useita tiedostoista
- **pentomino**: kartta ja vähentää vierekkäin koskevista ohjelma pentomino ongelmien selvittämiseen, ratkaisujen etsimiseen
- **pii**: kartta ja vähentää ohjelmaan, joka laskee pii käyttämällä näennäiskorkokauden Monte Carlo menetelmä
- **randomtextwriter**: kartta ja vähentää ohjelmaan, joka kirjoittaa 10 gt: n satunnaisia tekstitiedot solmu kohden
- **randomwriter**: kartta ja vähentää ohjelmaan, joka kirjoittaa 10 Gigatavua kohden solmu satunnaisia tietoja
- **secondarysort**: esimerkki määrittää toissijaisen lajittelun pienentäminen
- **Lajittele**: kartta ja vähentää ohjelmaan, joka lajittelee satunnaisia kirjoittajan kirjoittamat tiedot
- **sudoku**: sudoku Ratkaisin
- **teragen**: Luo terasort tiedot
- **terasort**: Suorita terasort
- **teravalidate**: tarkistuksen tulosten terasort
- **wordcount**: kartta ja vähentää ohjelmaan, joka laskee sanat input-tiedostoissa
- **wordmean**: kartta ja vähentää ohjelmaan, joka laskee syötteen tiedostot sanoja keskipituuden
- **wordmedian**: kartta ja vähentää ohjelmaan, joka laskee syötteen tiedostot sanoja mediaani pituus
- **wordstandarddeviation**: kartta ja vähentää ohjelmaan, joka laskee keskihajonnan syötteen tiedostot sanoja pituuden

**Lähdekoodin**: näitä esimerkkejä lähdekoodin sisältyy HDInsight-klusterin osoitteessa **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples**

> [AZURE.NOTE] `2.2.4.9-1` Polku on HDInsight-klusterin Hortonworks tietojen ympäristön-versio ja saattavat muuttua, kun Hdinsightista on päivitetty.

## <a name="how-to-run-the-samples"></a>Voit suorittaa mallit ##

1. Yhdistä HDInsight käyttämällä SSH ohjeiden mukaisesti seuraavissa artikkeleissa:

    - [Linux-pohjaiset Hadoop HDInsight Linux, Unix tai OS X-SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [SSH käyttäminen Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Kohteesta `username@#######:~$` Kysy, käytä seuraavaa komentoa luettelon mallit:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar

    Esimerkki luettelosta Luo tämän asiakirjan edellisestä osasta.

3. Käytä seuraavaa komentoa tietyn otoksen ohjeita. Tässä tapauksessa **wordcount** otosten:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount

    Näyttöön tulee seuraava sanoma:

        Usage: wordcount <in> [<in>...] <out>

    Tämä ilmaisee, että voit antaa useita syötteen polut asiakirjoja. Lopullinen polku on where tulos (lähde-asiakirjoihin sanojen määrä) on tallennettu.

4. Voit laskea kaikki sanat-muistikirjoja, Leonardo Da Vinci, jotka on esitetty mallitiedot yhteyttä klusterin kanssa käyttämällä seuraavaa:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount

    Työn syöte on luettu **wasbs:///example/data/gutenberg/davinci.txt**.

    Tulostus-tallennettuja tässä esimerkissä **wasbs: / / / Esimerkki/tietojen/davinciwordcount**.

    > [AZURE.NOTE] Wordcount otosten ohjeessa mainittuja voit myös määrittää syötteen useita tiedostoja. Esimerkiksi `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` sanamäärän davinci.txt ja ulysses.txt.

5. Kun työ on valmis, seuraava komento avulla voit tarkastella tulos:

        hdfs dfs -cat /example/data/davinciwordcount/*

    Tämä yhdistää kaikki työ tuottamat tulostus-tiedostot ja tuoda niitä. Tavallinen tässä esimerkissä on vain yhden tiedoston, mutta jos käytettävissä on useita Tämä komento käytöstä kaikista niistä.

    Tulos on seuraavanlainen:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Kunkin rivin edustaa sanan ja kuinka monta kertaa, se tapahtui syöttötiedot.

## <a name="sudoku"></a>Sudoku

Esimerkki Sudoku on hieman unhelpful käyttöohjeet; "Sisällytä pulmapelin komentorivillä."

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) on yhdeksän 3 x 3-ruudukoiden koostuu logiikan-pulmapelin. Jotkin ruudukon solujen lukuja, muut käyttäjät ovat tyhjiä ja lähinnä ratkaisemiseksi tyhjien solujen. Yllä oleva linkki on tietoja siitä, pulmapelin, mutta tässä esimerkissä on tyhjiä soluja, ratkaise. Microsoftin input pitäisi olla niin tiedosto, joka on seuraavanlainen:

- Yhdeksän yhdeksän sarakkeet riveille

- Kunkin sarakkeen voi olla joko numero tai `?` (joka osoittaa tyhjä solu)

- Solut on erotettu toisistaan välilyönnillä

Ei voi muodostaa Sudoku palapelit; tietyllä tavalla sarakkeen tai rivin luvun ei voi toistaa. HDInsight-klusterin, jonka rakenne on oikein on esimerkki. Se on osoitteessa **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta** , ja se sisältää seuraavat:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

> [AZURE.NOTE] `2.2.4.9-1` Polun saattavat muuttua, kun päivitykset tehdään HDInsight-klusterin.

Voit suorittaa tämän Sudoku Esimerkki kautta käyttämällä seuraava komento:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/2.2.9.1-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta

Tulosten pitäisi näyttää seuraavankaltaiselta:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi-"></a>Pii (n: n)

Pii-esimerkissä käytetään tilastollinen (näennäiskorkokauden Monte Carlo) tapaa arvioida piin arvon. Sijoittaa satunnaisesti sisältyy yksikkö pisteiden kuuluvat neliskulmaiset myös tilavuuteen, neliön sisällä ympyrän ala yhtä todennäköisyydellä ympyrän pii/4. Piin arvo on arvioitu 4R, missä R ympyrää, jotka ovat neliön sisällä pisteiden kokonaismäärän sisällä olevat pisteiden lukumäärä suhde-arvosta. Mitä suurempi pisteiksi käytetään malli sitä parempia arvio on.

Tässä esimerkissä mapper luo satunnaisesti yksikön neliön sisällä pisteiden lukumäärä ja laskee, kuinka paljon, ympyrän sisällä olevat.

Reducer kasvaa pistettä mappers laskema sitten ja laskee kaavan 4R, jossa R on laskettu sisällä, jotka ovat neliön sisällä pisteiden kokonaismäärän ympyrää pisteiden lukumäärä suhde-piin arvon.

Seuraavalla komennolla voit suorittaa tämän mallin. Tämä käyttää 16 kartat 10,000,000 näytteiden ja arvioida piin arvon:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000

Tämä arvo on oltava samalla **3.14159155000000000000**. Viittaukset-ensimmäiset 10 desimaalit piin ovat 3.1415926535.

##<a name="10gb-greysort"></a>10 Gigatavua Greysort

GraySort on ensisijainen lajittelu, jonka arvo on Lajittele nopeus (TT/minuutti), joka on saavutettu aikana lajittelu hyvin suuria tietomääriä, yleensä 100 TT pienin.

Tässä esimerkissä käytetään vähän 10 gt tietoja niin, että se voidaan suorittaa suhteellisen nopeasti. Tiedostossa käytetään Owen O'Malley ja Arun Murthy, jotka ovat voittaneet vuosittaisen Yleinen ("daytona") teratavun Lajittelu ensisijaisen 2009 0.578 TT/min (100 TT 173 minuutteina) Ylityökorvaus kehittämä MapReduce sovellukset. Lisätietoja tämän ja muiden lajittelu vertailutestit on [Sortbenchmark](http://sortbenchmark.org/) -sivustossa.

Tässä esimerkissä käytetään kolmea MapReduce ohjelmat:

- **TeraGen**: MapReduce-ohjelmaan, joka luo tietoriviä lajitteleminen

- **TeraSort**: esimerkit kenttään annettavat tiedot ja käyttää MapReduce tietojen lajittelussa yhteensä tilaukseen

    TeraSort on vakio tyyppinen MapReduce Funktiot, lukuun ottamatta mukautetun partitioner, joka käyttää N-1 näyte avaimet, jotka määrittävät kunkin pienentäminen avaimen alueen lajiteltu luettelo. Erityisesti kaikki avaimet tällaisten, esimerkki [i-1] < = näppäin < otoksen [i] lähetetään vähentää i. Tämä oikeudet tulosteiden vähentää i ovat kaikki alle tulosteen vähentää i + 1.

- **TeraValidate**: MapReduce-ohjelmaan, joka vahvistaa, että tulos on lajiteltu yleisesti

    Se luo yksi määritys tiedostoa kohden tulostus-kansioon ja kunkin kartan varmistaa, että kukin avain on pienempi tai yhtä suuri edelliseen. Kartta-funktio luo myös kunkin tiedoston ensimmäisen ja viimeisen näppäimet tai tallenteet ja Vähennä-toimintoa varmistaa, että tiedoston i ensimmäiseksi näppäimeksi on suurempi kuin tiedoston i-1 viimeisen avain. Tulos Pienennä, ilmoitettujen ongelmia ja epäjärjestyksessä olevat näppäimet.

Käytä seuraavia vaiheita luoda tietoja, lajitella ja tarkista sitten tulos:

1. Luo 10 gt: n tietoja, jotka tallennetaan HDInsight-klusterin oletusarvon tallennustilan **wasbs: / / / esimerkki / / 10 Gigatavun-lajittelu-tietosyöte**:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input

    `-Dmapred.map.tasks` Siitä Hadoop siitä, kuinka monta käyttämään työlle kartan tehtävät. Lopullinen kaksi parametria 10 gt enemmän tietoja luoda ja tallentaa sen osoitteessa työn opasta **wasbs: / / / esimerkki / / 10 Gigatavun-lajittelu-tietosyöte**.

2. Seuraavalla komennolla voit lajitella tiedot:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output

    `-Dmapred.reduce.tasks` Hadoop kertoo, kuinka monta vähentää käyttämään projektin tehtävät. Lopullinen kaksi parametria ovat vain syöttö- ja tiedoille.

3. Lajittele luoma tietojen kelpoisuuden käyttämällä seuraavaa:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate

##<a name="next-steps"></a>Seuraavat vaiheet ##

Tästä artikkelista oppimiasi suorittaminen Linux-pohjaiset HDInsight-klustereiden mukana mallit. Hakeminen Possu, rakenne ja MapReduce käyttäminen Hdinsightista on seuraavissa artikkeleissa:

* [Possu käyttäminen Hadoop-Hdinsightiin][hdinsight-use-pig]
* [Hadoop HDInsight-rakenteen käyttäminen][hdinsight-use-hive]
* [Hadoop-HDInsight MapReduce käyttäminen] [hdinsight-use-mapreduce]



[hdinsight-errors]: hdinsight-debug-jobs.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
