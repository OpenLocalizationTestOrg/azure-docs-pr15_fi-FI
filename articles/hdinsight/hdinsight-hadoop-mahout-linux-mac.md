<properties
    pageTitle="Luo suosituksia Mahout ja Linux-pohjaiset HDInsight | Microsoft Azure"
    description="Opettele Apache Mahout-konepohjaisten oppimistekniikoiden kirjaston avulla voit luoda elokuvan suositukset Linux-pohjaiset HDInsight (Hadoop)."
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
    ms.date="08/30/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight"></a>Luo elokuvan suosituksia käyttämällä Apache Mahout Linux-pohjaiset Hadoop-Hdinsightiin

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Opettele käyttämään [Apache Mahout](http://mahout.apache.org) -konepohjaisten oppimistekniikoiden Azure Hdinsightiin kirjastosta elokuvan suosituksia luomiseen.

Mahout on [konepohjaisten oppimistekniikoiden] [ ml] Apache Hadoop-kirjastossa. Mahout sisältää algoritmit tietoja, kuten suodatus, luokittelu- ja klusterointi käsittelyä varten. Tässä artikkelissa suositus ohjelma käyttää elokuvan suositukset, jotka perustuvat elokuvat ystäviesi tuliko luomiseen.

> [AZURE.NOTE] Tässä asiakirjassa vaiheet pätevät Linux-pohjaiset Hadoop HDInsight-klusterissa. Lisätietoja Mahout käyttämisestä Windows-pohjaisesta klusterin on artikkelissa [Luo elokuvan suosituksia Apache Mahout kanssa Windows-pohjaisesta Hadoop-HDInsight avulla](hdinsight-mahout.md)

##<a name="prerequisites"></a>Edellytykset

* Linux-pohjaiset Hadoop HDInsight-klusterissa. Lisätietoja luominen on [Linux-pohjaiset Hadoop-HDInsight käyttäminen][getstarted]

##<a name="mahout-versioning"></a>Mahout versiotiedot

Saat lisätietoja HDInsight-klusterin mukana Mahout version [HDInsight-versiot ja Hadoop osat](hdinsight-component-versioning.md).

> [AZURE.WARNING] Samalla, kun se on mahdollista Mahout eri version lataaminen HDInsight-klusterin, vain HDInsight-klusterin mukana osat ovat tuettuja, ja Microsoft Support auttavat eristämään ja ratkaista ongelmat, jotka liittyvät komponentit.
>
> Mukautettujen osien saa järkevän tukea helpottavat edelleen ongelman vianmäärityksen. Tämä saattaa aiheuttaa ratkaisemiseksi tai sinulta kysytään, haluatko osallistuminen käytettävissä olevat kanavat Avaa lähde-tekniikoiden laaja osaamisalueet, tekniikkaa löytyi. Esimerkiksi ovat yhteisön sivustoja, joita voidaan käyttää, kuten: [HDInsight MSDN-keskustelupalsta](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Myös Apache projektien on projektisivustojen [http://apache.org](http://apache.org), esimerkiksi: [Hadoop](http://hadoop.apache.org/), [Ohjattu](http://spark.apache.org/).

##<a name="recommendations"></a>Tietoja suositukset

Funktioita, joka tarjoaa Mahout on suositus ohjelma. Tämä versio voidaan käyttää tietojen muoto `userID`, `itemId`, ja `prefValue` (käyttäjien tilaksi kohteen). Mahout sitten suorittaa muiden occurance analyysi määrittämään: _käyttäjät, joilla on kohteen asetus on myös muita kohteita ensisijaisuus_. Valitse mahout määrittää käyttäjät, joilla on tykkää-kohteen asetukset, joita voidaan käyttää suosituksia.

Seuraavassa on erittäin yksinkertaisia esimerkki, joka käyttää elokuvat:

* __Muiden occurance__: Esko, Anneli ja Pekka kaikki tykännyt _tähti sotien_ _Empire joina takaisin_ja _palauttamista Jedi_. Mahout määrittää, että käyttäjät, jotka, kuten jokin näistä elokuvat myös, kuten muuhun rajoitteeseen.

* __Muiden occurance__: Teemu ja Anneli myös tykännyt _Haamuksi Menace_ _kloonit hyökkäyksen_ja _Sith Revenge_. Mahout määrittää, että käyttäjät, jotka tykännyt edellisen kolme elokuvat myös, kuten nämä kolme.

* __Samanlaiset suositus__: koska Esko tykännyt kolme ensimmäistä elokuvat-elokuvat tarkastelee Mahout, joita muut kanssa samalla olevaa tykännyt, mutta ei Esko on seurattavat (tykännyt/luokitellut). Tässä tapauksessa Mahout suosittelee _Haamu Menace_ _kloonit hyökkäyksen_ja _Sith Revenge_.

###<a name="understanding-the-data"></a>Tietoja tiedot

Kätevästi, [GroupLens Oheistiedot] [ movielens] on luokitus tietoja muodossa, joka on yhteensopiva Mahout elokuvat. Nämä tiedot ovat käytettävissä yhteyttä klusterin oletusarvon tallennustilan `/HdiSamples/HdiSamples/MahoutMovieData`.

On kaksi tiedostoa `moviedb.txt` (elokuvat-tiedot) ja `user-ratings.txt`. Käyttäjän ratings.txt tiedoston käytetään analyysi-aikana, kun moviedb.txt käytetään antamaan käyttäjäystävällinen tekstin infromation analyysin tulokset näytetään.

Käyttäjän ratings.txt sisältämät tiedot, rakenne on `userID`, `movieID`, `userRating`, ja `timestamp`, joka kertoo us miten keskeiset kunkin käyttäjän aiheet elokuvan. Tässä on esimerkki tiedot:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

##<a name="run-the-analysis"></a>Suorittaa analyysia

Käytä seuraavaa komentoa suositus työn suorittamista varten:

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] Työn voi kestää useita minuutteja, ja voi suorittaa useita MapReduce töitä.

##<a name="view-the-output"></a>Näytä tulos

1. Kun työ on valmis, seuraava komento avulla voit tarkastella luotu tulos:

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    Tulos tulee näkyviin seuraavasti:

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    Ensimmäinen sarake näyttää `userID`. Sisältämiä arvoja ' ["ja"]' ovat `movieId`:`recommendationScore`.

2. Voit käyttää tuloksessa, sekä moviedb.txt, saat näkyviin lisätietoja käyttäjän helpossa muodossa. Ensin annettava kopioida tiedostot paikallisesti seuraavia komentoja:

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
        hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .

    Tulosta tiedot kopioidaan **recommendations.txt** nykyiseen kansioon, sekä elokuvan datatiedostot-tiedostoon.

3. Seuraavalla komennolla voit luoda uuden Python komentosarjan, joka näyttää elokuvan nimet tietojen suositusten tuloksissa:

        nano show_recommendations.py

    Kun editorin avautuu, käyttää tiedoston sisällön seuraavasti:

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    Paina **CTRL + X** **Y**ja lopuksi **Enter** tietojen tallentamiseen.

3. Seuraavalla komennolla voit määrittää suoritettavan tiedoston:

        chmod +x show_recommendations.py

4. Suorita Python komentosarja. Seuraavassa oletetaan on kansiossa, jossa kaikki tiedostot on ladattu:

        ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt

    Tämä näyttää luotu käyttäjän tunnus 4 suositukset-palvelussa.

    * **Käyttäjän ratings.txt** tiedoston käytetään elokuvia, jotka on luokiteltu käyttäjän hakemiseen
    * **Moviedb.txt** -tiedoston avulla elokuvat Nouda
    * Hae tämän käyttäjän elokuvan suosituksia käytetään **recommendations.txt**

    Tämä komento tulosteen on seuraavanlainen:

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##<a name="delete-temporary-data"></a>Poista väliaikaiset tiedot

Mahout töitä ei Poista väliaikaiset käsiteltäessä työn luotavaan tietojen. `--tempDir` -Parametri on määritetty Esimerkki työn eristää väliaikaiset tiedostot helposti poistettavaksi tietyn polkua. Poista väliaikaiset tiedostot, käytä seuraavaa komentoa:

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] Jos haluat suorittaa komennon uudelleen, sinun on poistettava myös tulostus-kansio. Voit poistaa tämän kansion käyttämällä seuraavaa:
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt oppinut voit Mahout käyttämisestä, tutustu muita tapoja HDInsight tietojen käyttäminen:

* [Rakenne ja Hdinsightiin](hdinsight-use-hive.md)
* [Possu Hdinsightiin](hdinsight-use-pig.md)
* [MapReduce HDInsight kanssa](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
