<properties
   pageTitle="Twitter-Apache myrsky HDInsight-tykkäysten aiheita | Microsoft Azure"
   description="Opettele Trident avulla voit luoda Apache myrsky-topologian, joka määrittää tykkäysten aiheet Twitter hashtags perusteella."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Määrittää Apache myrsky HDInsight-Twitter tykkäysten aiheita

Opettele Trident avulla voit luoda myrsky topologian, joka määrittää Twitter tykkäysten aiheet (hash-tunnisteet).

Trident on korkean tason otetaan, jossa on työkaluja, kuten liitokset, koosteet, ryhmittely, toiminnot ja suodattimet. Lisäksi Trident Lisää perusalkioiden tekemisestä tilallisten, vaiheittainen käsittely. Tässä esimerkissä näytetään, miten voit luoda topologian, käyttämällä mukautettuja nokkaan, funktio ja Trident myöntämä useita valmiita funktioita.

> [AZURE.NOTE] Tässä esimerkissä on raskaasti perusteella [Trident myrsky](https://github.com/jalonsoramos/trident-storm) Esimerkki Juan Alonso.

##<a name="requirements"></a>Vaatimukset

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java- ja JDK 1.7</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven-testi</a>

* <a href="http://git-scm.com/" target="_blank">Git</a>

* Twitter-Kehitystyökalut-tili

##<a name="download-the-project"></a>Lataa projekti

Seuraava koodi avulla voit kopioida projektin paikallisesti.

    git clone https://github.com/Blackmist/TwitterTrending

##<a name="topology"></a>Topologian

Tässä esimerkissä topologian on seuraavanlainen:

![topologian](./media/hdinsight-storm-twitter-trending/trident.png)

> [AZURE.NOTE] Tämä on topologian yksinkertaistettu näkymä. Useita osia esiintymiä jakautuminen-klusterin solmut.

Trident tunnus, jolla toteuttaa topologian on seuraavanlainen:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Tämä koodi toimii seuraavasti:

1. Luo uusi virta nokkaan. Nokkaan tweets hakee Twitter ja suodattaa niitä tiettyjä avainsanoja (Rakkaus, musiikkia ja esimerkiksi tässä esimerkissä).

2. HashtagExtractor mukautetun funktion käytetään poimia kunkin tweetin hash-tunnisteet. Nämä ovat lähettämän virta.

3. Virta on ryhmitelty hash-tunnisteen ja siirtää kokoaja. Tämä kokoaja Luo määrä, kuinka monta kertaa kunkin hash-tunnisteen on tapahtunut. Tämä tieto on samanlainen muistiin. Lopuksi uusi stream on lähetetty, joka sisältää hash-tunnisteen ja määrä.

4. Koska emme vain kiinnostunut tweets tietyn erän suosituimpia hash-tunnisteet **FirstN** kokoonpanon otetaan käyttöön palauttaa vain 10 ylintä arvoa, Laske-kentän perusteella.

> [AZURE.NOTE] Kuin nokkaan ja HashtagExtractor on käytössä Trident sisäistä toimintoa.
>
> Lisätietoja valmiiden toiminnot on artikkelissa <a href="https://storm.apache.org/apidocs/storm/trident/operation/builtin/package-summary.html" target="_blank">paketin storm.trident.operation.builtin</a>.
>
> Trident tilan käyttöotot kuin MemoryMapState, Lisätietoja on seuraavissa artikkeleissa:
>
> * <a href="https://github.com/fhussonnois/storm-trident-elasticsearch" target="_blank">Myrsky Trident joustavasti haku</a>
>
> * <a href="https://github.com/kstyrc/trident-redis" target="_blank">Trident Redis.txt</a>

###<a name="the-spout"></a>Nokkaan

Nokkaan **TwitterSpout**käyttää <a href="http://twitter4j.org/en/" target="_blank">Twitter4j</a> tweets noutamiseen Twitter. Suodatin luodaan (Rakkaus, musiikkia ja esimerkiksi tässä esimerkissä) ja saapuvan tweets (tila), jotka vastaavat suodatin tallennetaan linkitetyn eston jonossa. (Lisätietoja on artikkelissa <a href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedBlockingQueue.html" target="_blank">Luokan LinkedBlockingQueue</a>.) Lopuksi kohteet vedetään jonossa käytöstä ja voimakkuus topologian.

###<a name="the-hashtagextractor"></a>HashtagExtractor

Voit purkaa hash-tunnisteet <a href="http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--" target="_blank">getHashtagEntities</a> käytetään noutaa kaikki hash-tunnisteet, jotka sisältyvät tweetin. Nämä sitten virta voimakkuus.

##<a name="enable-twitter"></a>Twitter ottaminen käyttöön

Seuraavien vaiheiden avulla voit rekisteröidä uuden Twitter-sovelluksen ja hankkia kuluttaja- ja access tunnuksen tiedot, joita tarvitaan Twitter lukea:

1. Siirry <a href="https://apps.twitter.com" target="_blank">Twitter-sovellukset</a> ja valitse **Luo uusi sovellus** -painike. Kun täyttämällä lomakkeen, jätä **Takaisinsoitto URL** -kenttä tyhjäksi.

2. Kun sovellus on luotu, valitse **näppäimet ja tunnusten Access** -välilehti.

3. Kopioi **Kuluttaja-näppäintä** ja **Kuluttaja salaisen** tiedot.

4. Sivun alareunassa valitsemalla **Luo oma käyttöoikeustietue** , jos tunnusten ei ole. Kun tunnukset on luotu, kopioi **Access-tunnuksen** ja **Access-tunnuksen salaisen** tiedot.

5. Aiemmin monistaa **TwitterSpoutTopology** projektin **resources/twitter4j.properties** -tiedoston, voit kerättyjen tietojen lisääminen edelliset vaiheet ja tallenna sitten tiedosto.

##<a name="build-the-topology"></a>Topologian luominen

Projektin luonnissa käytettävien seuraava koodi:

        cd [directoryname]
        mvn compile

##<a name="test-the-topology"></a>Testaa topologian

Käytä seuraavaa komentoa Testaa topologian paikallisesti:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Topologian käynnistyttyä näkyy vianmäärityksen lisätietoja, joka sisältää hajautuksen tunnisteet ja laskee lähetettyjä topologian. Tulosteen pitäisi näyttää seuraavankaltaiselta:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet testannut topologian paikallisesti, tutustu topologian ottamisesta: [käyttöönotto ja hallita Apache myrsky topologioissa HDInsight-](hdinsight-storm-deploy-monitor-topology.md).

Voit myös tarkastella myrsky seuraavissa ohjeaiheissa:

* [Kehitä Java topologioissa myrsky-HDInsight käyttämällä maven-testi](hdinsight-storm-develop-java-topology.md)

* [Kehitä C# topologioissa myrsky käyttöön Visual Studiossa Hdinsightiin](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Lisää HDinsight myrsky Esimerkkejä:

* [Esimerkki topologioissa myrsky HDInsight-varten](hdinsight-storm-example-topology.md)
