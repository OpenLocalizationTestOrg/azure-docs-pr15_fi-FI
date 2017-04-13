<properties
   pageTitle="Python osien käyttäminen myrsky-topologian HDinsight | Microsoft Azure"
   description="Katso, miten voit käyttää Python osien poistamisesta Apache myrsky Azure HDInsight. Opit käyttämään Python osat sekä Java perusteella ja Clojure perusteella myrsky topologian."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="python"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Kehittää Apache myrsky topologioissa Python käyttäminen Hdinsightiin

Apache myrsky tukee useita kieliä, myös jonka avulla voi yhdistää useita kieliä yhden topologian-osat. Tässä asiakirjassa kerrotaan Python osien käyttämisestä Java- ja Clojure perustuva myrsky-topologioissa HDInsight.

##<a name="prerequisites"></a>Edellytykset

* Python 2.7 tai uudempi versio

* Java JDK 1.7 tai uudempi versio

* [Leiningen](http://leiningen.org/)

##<a name="storm-multi-language-support"></a>Myrsky monikielisyyden tukea

Myrsky on suunniteltu toimimaan osia käyttämällä tahansa ohjelmointikielellä kirjoitetut kuitenkin edellyttää osat ymmärtää käsittelemisestä [myrsky Thrift määritys](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift). Python-moduulin toimitetaan Apache myrsky projekti, jonka avulla voit helposti käyttöliittymä, jossa myrsky osana. Löydät tämän moduulin [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Apache myrsky on Java-prosessi, joka suoritetaan Java virtuaalikoneen (JVM), kirjoitettu muilla kielillä osat ovat suorittaa aliprosessit. Käytössä JVM myrsky bittien yhteydessä nämä aliprosessien käyttämällä stdin/stdout JSON sanomia. Lisätietoja välisen osat löytyvät [Providerin protokolla](https://storm.apache.org/documentation/Multilang-protocol.html) -ohjeista.

###<a name="the-storm-module"></a>Myrsky-moduuli

Myrsky-moduuli (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py) sisältää bittien Python osat, jotka toimivat myrsky luomiseksi.

Tämä on esimerkiksi `storm.emit` monikot, tulostaminen ja `storm.logInfo` kirjoittaminen lokit. Voit lukea tämän tiedoston kohdalle ja se antaa tietoja rohkaista.

##<a name="challenges"></a>Haasteita

Käytä __storm.py__ -moduulia, voit luoda Python spouts, jotka käyttävät tiedot ja pultit, käsittelevät tietoja, mutta yleinen myrsky topologian määritystä, verkkokaapeleita käytävää viestintää osien välillä on edelleen kirjoitettu Java tai Clojure. Lisäksi, jos käytät Java, sinun on luotava myös Java osat, jotka näyttävät liittymän Python osiin.

Myös, koska myrsky klustereiden suorittaa hajautettu tavalla, sinun on varmistettava, että moduulit vaatii Python osat ovat käytettävissä kaikissa työntekijä solmuissa klusterin. Myrsky ei ole mitään helposti tehdä tämä Providerin resurssien – sinun on joko jättää kaikki riippuvuudet topologian purkki tiedostolle tai riippuvuudet asentaminen kunkin työntekijän solmun klusterin manuaalisesti.

###<a name="java-vs-clojure-topology-definition"></a>Java ja Clojure topologian määritys

Näistä tavoista määrittämisessä on verkkotopologia Clojure on mennessä helpoin/tyhjästä voi suoraan lisätietolähteet python osien topologian määrityksessä. Java-pohjainen topologian määrityksiä Määritä myös Java-osat, jotka käsitellään esimerkiksi määritteleminen monikot kentät palauttama Python osat.

Molemmista menetelmistä on kuvattu tämän asiakirjan sekä esimerkki projektit.

##<a name="python-components-with-a-java-topology"></a>Java-topologian Python osia

> [AZURE.NOTE] Tässä esimerkissä on saatavilla kohdassa [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) __JavaTopology__ hakemistossa. Tämä on projektin maven-testi perusteella. Jos olet perehtynyt maven-testi, saat lisätietoja luomisesta myrsky topologian maven-testi projektille [kehittää Java-pohjainen topologioissa Apache myrsky HDInsight-kanssa](hdinsight-storm-develop-java-topology.md) .

Java-topologian, joka käyttää Python (tai JVM kielen muut osat) näkyy aluksi käyttämään Java-osia. mutta jos näyttää kunkin Java spouts/pultit, näkyviin tulee koodi seuraavankaltaiselta:

    public SplitBolt() {
        super("python", "countbolt.py");
    }

Tämä on missä Java käynnistää Python ja suorittaa komentosarjan, joka sisältää todellinen lukko logiikan. Java spouts/Pultit (esimerkiksi tämä) määritellä yksinkertaisesti monikon, pohjana Python osan lähettämän kentät.

Todellinen Python tiedostot on tallennettu `/multilang/resources` tässä esimerkissä hakemistoon. `/multilang` Directory viitataan __pom.xml__:

<resources>
    <resource>
        <!-- Where the Python bits are kept -->
        <directory>${basedir} / multilang</directory>
    </resource>
</resources>

Tämä vaihtoehto sisältää kaikki tiedostot `/multilang` kansio, joka rakennetaan tästä projektista purkkiin.

> [AZURE.IMPORTANT] Huomaa, että vain määrittää `/multilang` directory eikä `/multilang/resources`. Myrsky odottaa-JVM resursseista `resources` kansioon, jotta se on jo sisäisesti etsitään. Osien sijoittamisesta tämän kansion avulla voit vain viittaus Java-koodia nimen mukaan. Esimerkiksi `super("python", "countbolt.py");`. Toinen tapa Ajattele on myrsky näkee `resources` kansio kuin pääkansio (/) Providerin resurssien käytettäessä.
>
> Esimerkki tälle projektille `storm.py` moduulin sisältyy `/multilang/resources` hakemisto.

###<a name="build-and-run-the-project"></a>Muodosta ja Suorita projekti

Voit suorittaa tämän projektin paikallisesti käyttämällä maven-testi seuraava komento voivat laatia ja paikallisen tilassa:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

Ctrl + c avulla voit lopettaa prosessin.

Ottamaan projektin suorittaminen Apache myrsky HDInsight-klusterin noudattamalla seuraavia ohjeita:

1. Laadi uber jar:

        mvn package

    Tämä vaihtoehto Luo tiedosto nimeltä __WordCount--1.0 SNAPSHOT.jar__ - `/target` kansio tälle projektille.

2. Jar-tiedoston lataaminen Hadoop-klusterin jollakin seuraavista tavoista:

    * __Linux-pohjaiset__ HDInsight klustereiden varten: Käytä `scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar` purkki-tiedoston kopioiminen klusterin korvaamalla käyttäjänimi SSH käyttäjänimi ja CLUSTERNAME HDInsight-klusterinimi.

        Kun tiedosto on valmis lataamisesta, muodosta yhteys käyttämällä SSH klusterin ja topologian käytön aloittaminen`storm jar WordCount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`

    * For __Windows-pohjaisesta__ HDInsight klustereiden: yhdistäminen myrsky Raporttinäkymät-ikkunan siirtymällä HTTPS://CLUSTERNAME.azurehdinsight.net/ selaimessa. Korvaa CLUSTERNAME HDInsight-klusterinimi ja annettava järjestelmänvalvojan käyttäjänimi ja salasana pyydettäessä.

        Lomakkeen avulla voit tehdä seuraavat toimet:

        * __Jar tiedosto__: Valitse __Selaa__ja valitse sitten __WordCount 1.0-SNAPSHOT.jar__ -tiedosto
        * __Luokkanimi__: Kirjoita`com.microsoft.example.WordCount`
        * __Lisää Paramters__: Kirjoita kutsumanimi `wordcount` topologian tunnistaminen

        Valitse lopuksi __Lähetä__ topologian alkavan.

> [AZURE.NOTE] Kun alkanut, myrsky topologian suoritetaan, kunnes pysäytetään (lopetettujen.) Voit lopettaa topologian, jommallakummalla `storm kill TOPOLOGYNAME` komennon komentorivin (SSH istunnosta Linux-klusterin esimerkiksi) tai käyttämällä myrsky-Käyttöliittymä, valitse topologiasta ja valitse sitten __Poista__ -painiketta.

##<a name="python-components-with-a-clojure-topology"></a>Clojure topologian Python osia

> [AZURE.NOTE] Tässä esimerkissä on saatavilla kohdassa [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) __ClojureTopology__ hakemistossa.

Tämä topologian on luotu käyttämällä [Leiningen](http://leiningen.org) [Clojure uuden projektin luominen](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project). Tämän jälkeen tekemät muutokset scaffolded projektiin seuraavat muutokset:

* __Project.CLJ__: lisätty riippuvuudet myrsky ja poikkeukset viittaamaan kohteisiin, joita saattaa aiheuttaa ongelmia, kun käyttöön HDInsight-palvelimeen.
* __resurssien ja resurssit__: Leiningen luo oletusarvoisesti `resources` kansio, mutta tallennetut tiedostot näkyvät tästä projektista purkki luodulla ylimmällä lisätään ja myrsky odottaa aliraportti hakemiston tiedostojen `resources`. Jotta alikansio, on lisätty ja Python tiedostot tallennetaan `resources/resources`. Suorituksen aikana, jos tämä käsitellään pääkansio (/) Python osien käyttämiseen.
* __SRC/wordcount/Core.CLJ__: tiedosto sisältää topologian määrityksen ja __project.clj__ tiedostosta viittauksia. Lisätietoja myrsky topologian määrittäminen Clojure avulla on artikkelissa [Clojure DSL](https://storm.apache.org/documentation/Clojure-DSL.html).

###<a name="build-and-run-the-project"></a>Muodosta ja Suorita projekti

__Muodosta ja suorita projektin paikallisesti__, kirjoita seuraava komento:

    lein clean, run

Lopettaa topologian, käytä __Näppäinyhdistelmää Ctrl + C__.

__Uberjar muodosta ja ota käyttöön HDInsight,__toimi seuraavasti:

1. Uberjar sisältävä topologiasta ja tarvittavat riippuvuuksien luominen

        lein uberjar

    Tämä vaihtoehto Luo tiedosto nimeltä `wordcount-1.0-SNAPSHOT.jar` - `target\uberjar+uberjar` hakemisto.
    
2. Käytä jollakin seuraavista tavoista käyttöönotto ja suorita topologian HDInsight-klusterin:

    * __Linux-pohjaiset Hdinsightiin__
    
        1. Kopioi tiedosto HDInsight klusterin pään solmu käyttämällä `scp`. Esimerkki:
        
                scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
                
            Korvaa käyttäjänimi SSH käyttäjän klusterin ja CLUSTERNAME HDInsight-klusterin nimesi kanssa.
            
        2. Kun tiedosto on kopioitu klusterin, avulla muodostaa yhteyttä klusterin ja lähettää työn SSH. Lisätietoja SSH käyttämisestä Hdinsightista on artikkelissa jompikumpi seuraavista:
        
            * [SSH käyttäminen Linux-pohjaiset HDInsight Linux, Unix tai OS x: ssä](hdinsight-hadoop-linux-use-ssh-unix.md)
            * [SSH käyttäminen Linux-pohjaiset HDInsight Windowsista](hdinsight-hadoop-linux-use-ssh-windows.md)
            
        3. Kun yhteys on muodostettu, voit käynnistää topologian käyttämällä seuraavaa:
        
                storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount
    
    * __Windows-pohjaisesta Hdinsightiin__
    
        1. Yhteyden muodostaminen myrsky Raporttinäkymät-ikkunan siirtymällä HTTPS://CLUSTERNAME.azurehdinsight.net/ selaimessa. Korvaa CLUSTERNAME HDInsight-klusterinimi ja annettava järjestelmänvalvojan käyttäjänimi ja salasana pyydettäessä.

        2. Lomakkeen avulla voit tehdä seuraavat toimet:

            * __Jar tiedosto__: Valitse __Selaa__ja valitse sitten __wordcount 1.0-SNAPSHOT.jar__ -tiedosto
            * __Luokkanimi__: Kirjoita`wordcount.core`
            * __Lisää Paramters__: Kirjoita kutsumanimi `wordcount` topologian tunnistaminen

            Valitse lopuksi __Lähetä__ topologian alkavan.

> [AZURE.NOTE] Kun alkanut, myrsky topologian suoritetaan, kunnes pysäytetään (lopetettujen.) Voit lopettaa topologian, jommallakummalla `storm kill TOPOLOGYNAME` komennon komentorivin (SSH istunnosta Linux-klusterin) tai käyttämällä myrsky-Käyttöliittymä, valitse topologiasta ja valitse sitten __Poista__ -painiketta.

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä asiakirjassa opit, miten Python osien käyttäminen myrsky topologian. Katso muita tapoja käyttää Python HDInsight seuraavat tiedostot:

* [Miten voit käyttää Python streaming MapReduce työt](hdinsight-hadoop-streaming-python.md)
* [Python käyttäjän määrittämät funktiot (UDF) Possu ja rakenteen käyttäminen](hdinsight-python.md)
