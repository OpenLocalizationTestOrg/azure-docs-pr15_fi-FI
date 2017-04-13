<properties
   pageTitle="Kehitä Java-pohjainen topologioissa Apache myrsky | Microsoft Azure"
   description="Opettele luomaan myrsky topologioissa Java-luomalla yksinkertaisen word Laske-topologian."
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
   ms.date="09/14/2016"
   ms.author="larryfr"/>

#<a name="develop-java-based-topologies-for-a-basic-word-count-application-with-apache-storm-and-maven-on-hdinsight"></a>Kehitä Java-pohjainen topologioissa basic sanamäärä-sovelluksen Apache myrsky ja HDInsight-maven-testi

Opettele luomaan Java-pohjainen topologian Apache myrsky HDInsight-varten käyttämällä maven-testi. Käy läpi maven-testi ja Java, jossa topologian on määritetty Java-basic sanamäärä-sovelluksen luominen. Opit sitten käyttämällä valovirta framework topologian määrittäminen.

> [AZURE.NOTE] Valovirta framework on käytettävissä myrsky 0.10.0 tai uudempi versio. Myrsky 0.10.0 on käytettävissä HDInsight 3.3 ja 3.4.

Kun olet suorittanut tämän asiakirjan ohjeita, sinulla on perustiedot topologian, voit ottaa käyttöön Apache myrsky HDInsight.

> [AZURE.NOTE] Tässä asiakirjassa luotu topologioissa lopullinen versio on saatavana [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

##<a name="prerequisites"></a>Edellytykset

* <a href="https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" target="_blank">Java Developer Kit (JDK) versio 7</a>

* <a href="https://maven.apache.org/download.cgi" target="_blank">Maven-testi</a>: maven-testi on projektin muodosta järjestelmän projektien Java.

* Tekstieditorissa, esimerkiksi Muistiossa, <a href="http://www.gnu.org/software/emacs/" target="_blank">Emacs<a>, <a href="http://www.sublimetext.com/" target="_blank">Sublime tekstiä</a>, <a href="https://atom.io/" target="_blank">Atom.io</a>, <a href="http://brackets.io/" target="_blank">Brackets.io</a>. Voit esimerkiksi <a href="https://eclipse.org/" target="_blank">Pimennys</a> integroitua ympäristön (IDE) (versio Luna tai uudempi).

    > [AZURE.NOTE] Muokkaajan tai IDE ehkä tiettyjen toimintojen käyttämisen maven-testi, joka on osoitettu ei ole tässä asiakirjassa. Lisätietoja siitä, mitä muokkaaminen ympäristön ominaisuuksia käytössäsi on tuotteen ohjeissa.

##<a name="configure-environment-variables"></a>Määritä ympäristömuuttujat

Seuraavat ympäristömuuttujat voi määrittää, kun asennat Java ja JDK. Kuitenkin kannattaa tarkistaa, että ne ovat olemassa ja että ne sisältävät järjestelmän oikeat arvot.

* **JAVA_HOME** - osoitettava kansion, johon on asennettu suorituksenaikainen Java-ympäristö (JRE). Esimerkiksi Unix- tai Linux hajautuksessa-tunnisteen pitäisi olla samankaltainen kuin arvo `/usr/lib/jvm/java-7-oracle`. Windowsin se on samankaltainen kuin arvo`c:\Program Files (x86)\Java\jre1.7`

* **PATH** - pitäisi olla seuraavat polut:

    * **JAVA_HOME** (tai vastaava polku)

    * **JAVA_HOME\bin** (tai vastaava polku)

    * Kansion, johon on asennettu maven-testi

##<a name="create-a-new-maven-project"></a>Luo uusi projekti maven-testi

Komentorivin, valitse seuraava koodi voit luoda uuden maven-testi projekti nimeltä **WordCount**:

    mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

Tämä vaihtoehto Luo uusi kansio nimeltä **WordCount** nykyisessä paikassa, joka sisältää perusprojektin maven-testi.

**WordCount** hakemiston sisältää seuraavat kohteet:

* **pom.XML**: sisältää asetuksia maven-testi projektin.

* **src\main\java\com\microsoft\example**: on sovelluksen koodi.

* **src\test\java\com\microsoft\example**: sisältää testien sovelluksen. Tässä esimerkissä emme ole luovatko testit.

###<a name="remove-the-example-code"></a>Poista esimerkkikoodi

Koska olemme luovatko tämän sovelluksen, poista luotu numero- ja sovelluksen tiedostojen:

*  **src\test\java\com\microsoft\example\AppTest.Java**

*  **src\main\java\com\microsoft\example\App.Java**

##<a name="add-properties"></a>Lisää ominaisuus

Voit määrittää projektitason arvoja eli ominaisuudet maven-testi. Lisää seuraava jälkeen `<url>http://maven.apache.org</url>` riville:

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--
        Storm 0.10.0 is for HDInsight 3.3 and 3.4.
        To find the version information for earlier HDInsight cluster
        versions, see https://azure.microsoft.com/en-us/documentation/articles/hdinsight-component-versioning/
        -->
        <storm.version>0.10.0</storm.version>
    </properties>

Emme Käytä seuraavia arvoja nyt muihin osiin. Esimerkiksi määritettäessä myrsky components version Microsoft voi käyttää `${storm.version}` sijaan Kova coding arvon.

##<a name="add-dependencies"></a>Lisää riippuvuudet

Koska kyseessä on myrsky topologian, sinun on lisättävä riippuvuus myrsky osien. Avaa **pom.xml** -tiedosto ja lisää seuraavat koodin ** &lt;riippuvuudet >** osassa:

    <dependency>
      <groupId>org.apache.storm</groupId>
      <artifactId>storm-core</artifactId>
      <version>${storm.version}</version>
      <!-- keep storm out of the jar-with-dependencies -->
      <scope>provided</scope>
    </dependency>

Kääntämisen yhteydessä maven-testi käyttää näitä tietoja **myrsky core** maven-testi säilössä hakemiseen. Se näyttää ensin säilössä paikalliseen tietokoneeseen. Tiedostot eivät ole käytettävissä, jos se ladata ne julkisen maven-testi säilöstä ja tallentaa ne paikalliseen säilöön.

> [AZURE.NOTE] Ilmoitus `<scope>provided</scope>` rivi on lisätty-osassa. Tämä kertoo **myrsky core** pois luodaan, PURKKI tiedostoja, koska se toimitetaan järjestelmä maven-testi. Näin paketit hieman pienemmän luominen ja se varmistaa, että ne käyttävät **myrsky core** bittien, jotka sisältyvät myrsky HDInsight-klusterissa.

##<a name="build-configuration"></a>Luo määritys

Maven-testi-laajennusten avulla voit mukauttaa projektin, kuten siitä, miten projekti on käännetty tai pakkaaminen PURKKI tiedostoon muodosta vaiheita. Avaa **pom.xml** -tiedosto ja lisää seuraava koodi suoraan yläpuolella `</project>` rivi.

    <build>
      <plugins>
      </plugins>
      <resources>
      </resources>
    </build>

Tässä osassa käytetään Lisää laajennukset, resurssien ja luo muita asetuksia. Katso __pom.xml__ tiedoston koko viittaus [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

###<a name="add-plug-ins"></a>Lisää laajennukset

Saat myrsky topologioissa <a href="http://mojo.codehaus.org/exec-maven-plugin/" target="_blank">Johto maven-testi laajennuksen</a> on hyötyä, koska sen avulla voit suorittaa helposti topologian paikallisesti kehittäminen-ympäristössä. Lisää seuraavat `<plugins>` **pom.xml** -tiedosto, joka sisältää johto maven-testi-laajennus-osassa:

    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <version>1.4.0</version>
      <executions>
        <execution>
        <goals>
          <goal>exec</goal>
        </goals>
        </execution>
      </executions>
      <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
      </configuration>
    </plugin>

> [AZURE.NOTE] Huomaa, että `<mainClass>` tapahtuma käyttää `${storm.topology}`. Emme ei ole Määritä tämä aiemmin ominaisuudet-osan (mutta emme voi olla.) Sen sijaan on asettaa tämän arvon komentorivillä suoritettaessa topologian kehittäminen ympäristöön myöhemmin.

Toisen hyödyllisiä laajennus on <a href="http://maven.apache.org/plugins/maven-compiler-plugin/" target="_blank">Apache maven-testi kääntäjän laajennuksen</a>, jota käytetään kääntäminen asetusten muuttaminen. Tämä annettava ensisijainen syynä on muutettava Java-versio, joka käyttää maven-testi lähde- ja sovelluksen kohde. Haluamme version 1.7.

Lisää seuraavat toimet `<plugins>` **pom.xml** tiedoston Apache maven-testi kääntäjän laajennus ja lähde- ja -versioiden asettaminen 1.7-kohtaan.

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.3</version>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>

###<a name="configure-resources"></a>Resurssien määrittäminen

Resurssit-osassa voit sisältyy koodi resursseja, kuten tiedostojen tarvitsemia topologian osia. Tässä esimerkissä Lisää seuraavat toimet `<resources>` **pom.xml** tiedoston kohtaan.

    <resource>
        <directory>${basedir}/resources</directory>
        <filtering>false</filtering>
        <includes>
          <include>log4j2.xml</include>
        </includes>
    </resource>

Tämä lisää resursseja hakemiston projektin ylimmällä (`${basedir}`), joka sisältää resurssit ja sisältää tiedosto nimeltä __log4j2.xml__sijainniksi. Määritä, mitä tietoja tallentamat topologian käytetään tätä tiedostoa.

##<a name="create-the-topology"></a>Luo topologian

Java-pohjainen myrsky topologian sisältää kolme osaa, sinun on käsiteltävä (tai viittaus) riippuvuus nimellä.

* **Spouts**: lukee ulkoisten tietojen tietolähteitä ja virtaa tietojen tuominen topologian tietokoneesta kuuluu äänimerkki.

* **Bolts**: suorittaa käsittely lähettämän spouts tai muita Pultit virtaa ja vähintään yksi virtaa tietokoneesta kuuluu äänimerkki.

* **Topologian**: määrittää, miten spouts ja pultit järjestetään ja tarjoaa aloituskohdan topologian.

###<a name="create-the-spout"></a>Luo nokkaan

Voit vähentää: n määrittäminen ulkoisiin tietolähteisiin, seuraavat nokkaan yksinkertaisesti tietokoneesta kuuluu äänimerkki satunnaisia lauseiden. On muokattu versio, joka on annettu [myrsky Starter esimerkkejä](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter)nokkaan.

> [AZURE.NOTE] Esimerkki nokkaan, jossa lukee ulkoisesta tietolähteestä näkyvissä on yksi seuraavasti:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): esimerkki-nokkaan, joka lukee Twitter
>
> * [Myrsky Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): nokkaan, joka lukee Kafka

Luo uusi tiedosto nimeltä **RandomSentenceSpout.java** **src\main\java\com\microsoft\example** hakemistossa nokkaan, ja käyttää sisältöä:

    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     * http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

     /**
      * Original is available at https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/storm/starter/spout/RandomSentenceSpout.java
      */

    package com.microsoft.example;

    import backtype.storm.spout.SpoutOutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichSpout;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Values;
    import backtype.storm.utils.Utils;

    import java.util.Map;
    import java.util.Random;

    //This spout randomly emits sentences
    public class RandomSentenceSpout extends BaseRichSpout {
      //Collector used to emit output
      SpoutOutputCollector _collector;
      //Used to generate a random number
      Random _rand;

      //Open is called when an instance of the class is created
      @Override
      public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
      //Set the instance collector to the one passed in
        _collector = collector;
        //For randomness
        _rand = new Random();
      }

      //Emit data to the stream
      @Override
      public void nextTuple() {
      //Sleep for a bit
        Utils.sleep(100);
        //The sentences that will be randomly emitted
        String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
            "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
        //Randomly pick a sentence
        String sentence = sentences[_rand.nextInt(sentences.length)];
        //Emit the sentence
        _collector.emit(new Values(sentence));
      }

      //Ack is not implemented since this is a basic example
      @Override
      public void ack(Object id) {
      }

      //Fail is not implemented since this is a basic example
      @Override
      public void fail(Object id) {
      }

      //Declare the output fields. In this case, an sentence
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("sentence"));
      }
    }

Lue koodin läpi selvittääksesi, miten tämä nokkaan toimii hetki.

> [AZURE.NOTE] Vaikka tämä topologian on käytössä vain yksi nokkaan, muiden voi olla useita, syötteen tietoja useista lähteistä topologian.

###<a name="create-the-bolts"></a>Luo Pultit

Pultit käsitellä tietojen käsittelyyn. Saat tämän topologian on kaksi Pultit:

* **SplitSentence**: jakaa lähettämän **RandomSentenceSpout** yksittäisiksi sanoiksi lauseiden.

* **WordCount**: laskee, kuinka monta kertaa kunkin sanan on tapahtunut.

> [AZURE.NOTE] Pultit mahdollisuuksista literaaleina mitään, esimerkiksi laskenta, pysyvyyttä tai Ulkoiset komponentit puhuminen.

Luo kaksi uusia tiedostoja, **SplitSentence.java** ja **WordCount.Java** **src\main\java\com\microsoft\example** hakemistossa. Käyttää sisältöä seuraavat tiedostot:

**SplitSentence**

    package com.microsoft.example;

    import java.text.BreakIterator;

    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class SplitSentence extends BaseBasicBolt {

      //Execute is called to process tuples
      @Override
      public void execute(Tuple tuple, BasicOutputCollector collector) {
        //Get the sentence content from the tuple
        String sentence = tuple.getString(0);
        //An iterator to get each word
        BreakIterator boundary=BreakIterator.getWordInstance();
        //Give the iterator the sentence
        boundary.setText(sentence);
        //Find the beginning first word
        int start=boundary.first();
        //Iterate over each word and emit it to the output stream
        for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
          //get the word
          String word=sentence.substring(start,end);
          //If a word is whitespace characters, replace it with empty
          word=word.replaceAll("\\s+","");
          //if it's an actual word, emit it
          if (!word.equals("")) {
            collector.emit(new Values(word));
          }
        }
      }

      //Declare that emitted tuples will contain a word field
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("word"));
      }
    }

**WordCount**

    package com.microsoft.example;

    import java.util.HashMap;
    import java.util.Map;
    import java.util.Iterator;

    import backtype.storm.Constants;
    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;
    import backtype.storm.Config;

    // For logging
    import org.apache.logging.log4j.Logger;
    import org.apache.logging.log4j.LogManager;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class WordCount extends BaseBasicBolt {
        //Create logger for this class
        private static final Logger logger = LogManager.getLogger(WordCount.class);
        //For holding words and counts
        Map<String, Integer> counts = new HashMap<String, Integer>();
        //How often we emit a count of words
        private Integer emitFrequency;

        // Default constructor
        public WordCount() {
            emitFrequency=5; // Default to 60 seconds
        }

        // Constructor that sets emit frequency
        public WordCount(Integer frequency) {
            emitFrequency=frequency;
        }

        //Configure frequency of tick tuples for this bolt
        //This delivers a 'tick' tuple on a specific interval,
        //which is used to trigger certain actions
        @Override
        public Map<String, Object> getComponentConfiguration() {
            Config conf = new Config();
            conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
            return conf;
        }

        //execute is called to process tuples
        @Override
        public void execute(Tuple tuple, BasicOutputCollector collector) {
            //If it's a tick tuple, emit all words and counts
            if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
                    && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
                for(String word : counts.keySet()) {
                    Integer count = counts.get(word);
                    collector.emit(new Values(word, count));
                    logger.info("Emitting a count of " + count + " for word " + word);
                }
            } else {
                //Get the word contents from the tuple
                String word = tuple.getString(0);
                //Have we counted any already?
                Integer count = counts.get(word);
                if (count == null)
                    count = 0;
                //Increment the count and store it
                count++;
                counts.put(word, count);
            }
        }

        //Declare that we will emit a tuple containing two fields; word and count
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            declarer.declare(new Fields("word", "count"));
        }
    }

Koodin kommentit ymmärtää kunkin lukko toiminta kuvattuihin hetki.

###<a name="define-the-topology"></a>Topologian määrittäminen

Topologian sitoo spouts ja bolts yhteen yhdeksi kaavion, joka määrittää, kuinka tiedot jatkuu osien välillä. Se sisältää myös rinnakkaisuus vihjeet, jotka myrsky käyttää luodessaan klusterin osia esiintymät.

Seuraavassa on Peruskaavio kaavion osien tämän topologian.

![kaavio, jossa näkyy spouts ja pultit järjestystä](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

Toteuttamisesta topologian, Luo uusi tiedosto nimeltä **WordCountTopology.java** **src\main\java\com\microsoft\example** hakemistossa. Käyttää seuraavia sisältö tiedoston:

    package com.microsoft.example;

    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.topology.TopologyBuilder;
    import backtype.storm.tuple.Fields;

    import com.microsoft.example.RandomSentenceSpout;

    public class WordCountTopology {

      //Entry point for the topology
      public static void main(String[] args) throws Exception {
      //Used to build the topology
        TopologyBuilder builder = new TopologyBuilder();
        //Add the spout, with a name of 'spout'
        //and parallelism hint of 5 executors
        builder.setSpout("spout", new RandomSentenceSpout(), 5);
        //Add the SplitSentence bolt, with a name of 'split'
        //and parallelism hint of 8 executors
        //shufflegrouping subscribes to the spout, and equally distributes
        //tuples (sentences) across instances of the SplitSentence bolt
        builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
        //Add the counter, with a name of 'count'
        //and parallelism hint of 12 executors
        //fieldsgrouping subscribes to the split bolt, and
        //ensures that the same word is sent to the same instance (group by field 'word')
        builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

        //new configuration
        Config conf = new Config();
        //Set to false to disable debug information
        // when running in production mode.
        conf.setDebug(false);

        //If there are arguments, we are running on a cluster
        if (args != null && args.length > 0) {
          //parallelism hint to set the number of workers
          conf.setNumWorkers(3);
          //submit the topology
          StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
        }
        //Otherwise, we are running locally
        else {
          //Cap the maximum number of executors that can be spawned
          //for a component to 3
          conf.setMaxTaskParallelism(3);
          //LocalCluster is used to run locally
          LocalCluster cluster = new LocalCluster();
          //submit the topology
          cluster.submitTopology("word-count", conf, builder.createTopology());
          //sleep
          Thread.sleep(10000);
          //shut down the cluster
          cluster.shutdown();
        }
      }
    }

Koodin kommentit selvittääksesi, miten topologian määritetään ja lähettää sitten klusterin kuvattuihin hetki.

###<a name="configure-logging"></a>Kirjauksen määrittäminen

Myrsky käyttää Apache Log4j lokitiedot. Jos kirjaaminen ei määritetä, topologian lähettää diagnostiikan tietoja, joka voi olla vaikea lukea. Voit määrittää, mikä on kirjautunut, Luo tiedosto nimeltä __log4j2.xml__ __resurssit__ -kansiossa. Määritä seuraavat tiedoston sisällön.

    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration>
    <Appenders>
        <Console name="STDOUT" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.microsoft.example" level="trace" additivity="false">
            <AppenderRef ref="STDOUT"/>
        </Logger>
        <Root level="error">
            <Appender-Ref ref="STDOUT"/>
        </Root>
    </Loggers>
    </Configuration>

Tämä määrittää uuden lokin __com.microsoft.example__ luokan, joka sisältää tässä esimerkissä topologian osia. Taso on määritetty jäljittää tämän lokin, joka Poimi tämän topologian osia lähettämän kirjaaminen tiedot. Jos voit selata takaisin tämän projektin koodi, huomaat, että vain WordCount.java tiedoston toteuttaa kirjaamisen; se kirjaa sanojen määrä.

`<Root level="error">` Osa määrittää Logging (kaikki kuin __com.microsoft.example__) kirjautua vain virhetietojen ylimmälle tasolle.

> [AZURE.IMPORTANT] Kun tämä vähentää huomattavasti kirjautuneena, kun testaus on verkkotopologia kehittäminen ympäristön tiedot, se ei poista kaikki, kun tietojenkäsittelyklusterissa vaatii yhteyden tietojenkäsittelyklusteriin tuotannon virheenkorjaustiedot. Pienennä tiedot on myös määritettävä, virheenkorjaus lähetetty klusterin määritys on EPÄTOSI. Katso tämän asiakirjan Esimerkki WordCountTopology.java-koodi. 

Lisätietoja kirjaaminen, jotta Log4j määrittämisestä on artikkelissa [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [AZURE.NOTE] Myrsky versio 0.10.0 käyttää Log4j 2.x. Vanhempien versioiden myrsky käytetään Log4j 1.x, joka käyttää eri tiedostomuotoon lokin määritys. Lisätietoja vanhempia määritys on artikkelissa [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

##<a name="test-the-topology-locally"></a>Testaa topologian paikallisesti

Kun tallennat tiedostoja, Testaa topologian paikallisesti seuraavan komennon avulla.

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology

Kun se suoritetaan, topologian näkyy käynnistys tietoja. Valitse se alkaa näyttää seuraavankaltaiselta rivit lauseiden peräisin nokkaan ja pultit käsitelleet.

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Katsomalla WordCount lukko lähettämän kirjaaminen on näkee, ' ja ' on on lähetetty 113 kertaa. Siirry ylöspäin, kunhan topologian suoritetaan, koska nokkaan jatkuvasti tietokoneesta kuuluu äänimerkki saman lauseiden edelleen määrä.

Myös ilmenee 5 toisen välin sanojen lähetys- ja laskee välillä. Tämä johtuu siitä __WordCount__ -komponentti on määritetty tulostaminen tiedot vain, kun jakoviivojen monikon saapuu, ja sitä pyytää, kuten monikot toimitetaan vain viiden sekunnin välein oletusarvoisesti.

## <a name="convert-the-topology-to-flux"></a>Muuntaa topologian valovirta

Valovirta on käytettävissä myrsky 0.10.0, jonka avulla voidaan erottaa määritysten ympäristöstä Uusi kehys. Java määritetään edelleen komponentit (Pultit ja spouts,), mutta topologian on määritetty YAML-tiedoston avulla.

YAML tiedoston määrittää osat käytettävä topologian, kuinka tiedot jatkuu niiden ja mitä arvot, jos haluat käyttää, kun valmistellaan osien välillä. Voit lisätä YAML tiedoston sisältävä projektin käyttöönottoa tai voit käyttää ulkoisen YAML tiedoston käynnistyessä topologian purkki tiedosto osana.

1. Siirrä projektin ulos __WordCountTopology.java__ -tiedosto. Aiemmin tämä määritetty topologian, mutta olemme ei voi käyttää niitä valovirta.

2. __Resurssit__ -kansioon Luo uusi tiedosto nimeltä __topology.yaml__. Määritä seuraavat tämän tiedoston sisällön.

        # topology definition

        # name to be used when submitting. This is what shows up...
        # in the Storm UI/storm command-line tool as the topology name
        # when submitted to Storm
        name: "wordcount"

        # Topology configuration
        config:
        # Hint for the number of workers to create
        topology.workers: 1

        # Spout definitions
        spouts:
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            # parallelism hint
            parallelism: 1

        # Bolt definitions
        bolts:
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1

        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 10
            parallelism: 1

        # Stream definitions
        streams:
        - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            # The stream emitter
            from: "sentence-spout"
            # The stream consumer
            to: "splitter-bolt"
            # Grouping type
            grouping:
            type: SHUFFLE

        - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
            # field(s) to group on
            args: ["word"]

    Hetki lukea ja ymmärtää jokaisessa osassa mitä ja miten se liittyy Java-pohjainen määritelmän __WordCountTopology.java__ -tiedostossa.

3. Tee seuraavat muutokset __pom.xml__ -tiedosto.

    * Lisää seuraavat uuden riippuvuuden `<dependencies>` osassa:

            <!-- Add a dependency on the Flux framework -->
            <dependency>
                <groupId>org.apache.storm</groupId>
                <artifactId>flux-core</artifactId>
                <version>${storm.version}</version>
            </dependency>

    * Lisää seuraavat laajennuksen `<plugins>` osa. Tämä laajennus käsittelee pakkauksen (jar-tiedosto) projektin luomisen ja koskee tietyn joitakin muunnoksia valovirta paketin luotaessa.

            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                        <!-- Keep us from getting a "can't overwrite file error" -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                        <!-- We're using Flux, so refer to it as main -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>org.apache.storm.flux.Flux</mainClass>
                        </transformer>
                    </transformers>
                    <!-- Keep us from getting a bad signature error -->
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

    * __Suoritus maven-testi-laajennus__ - `<configuration>` -osassa arvo, jota haluat muuttaa `<mainClass>` , `org.apache.storm.flux.Flux`. Näin valovirta käsittelemään topologian käynnistyvän olemme suorita se paikallisesti kehitteillä.

    * Valitse `<resources>` -osassa Lisää seuraavasti `<includes>`. Tämä sisältää YAML-tiedosto, joka määrittää topologian projektin osana.
    
            <include>topology.yaml</include>

## <a name="test-the-flux-topology-locally"></a>Testaa valovirta topologian paikallisesti

1. Seuraavat avulla voit kääntää ja suorita valovirta topologian käyttämällä maven-testi.

        mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    
    Jos käytössäsi on PowerShell, toimimalla seuraavasti:
    
        mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"

    Jos ovat Linux/Unix/OS X-järjestelmän ja on [asennettu myrsky kehittäminen-ympäristössä](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), voit käyttää seuraavia komentoja:

        mvn compile package
        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml

    `--local` Parametri toimii topologian oman kehitysympäristö paikallisen tilassa. `-R /topology.yaml` Parametrin käyttää `topology.yaml` tiedoston resurssin Määritä topologian purkki-tiedostosta.

    Kun se suoritetaan, topologian näkyy käynnistys tietoja. Valitse se alkaa näyttää seuraavankaltaiselta rivit lauseiden peräisin nokkaan ja käsitellä Pultit.

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    
    Ole 10 sekunnin viivettä välillä erissä kirjautuneena tietoja muodossa `topology.yaml` tiedoston välittää arvo `10` WordCount-osan luotaessa. Tämä määrittää jakoviivojen monikon viive välin 10 sekuntia.

2.  Tee kopio `topology.yaml` tiedoston projektista. Soita suunnilleen `newtopology.yaml`. Etsi seuraava osan ja muuta arvo-tiedostosta `10` , `5`. Muutos säteilevä erissä 5-10 sekuntia word laskee väli.

          - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 5
            parallelism: 1

3. Voit suorittaa topologian, käytä seuraavaa komentoa:

        mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"

    Tai jos olet luonut myrsky Linux/Unix/OS X-kehitysympäristö:

        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml

    Muuta `/path/to/newtopology.yaml` edellisessä vaiheessa luomasi newtopology.yaml tiedoston polku. Tämä komento on käytettävä newtopology.yaml topologian määritelmän. Vaikka emme kaavioon `compile` parametrin maven-testi uudelleen edellisessä vaiheessa luotu projektin versio.

    Kun topologian käynnistyy, olisi huomaat, että välisen lähetettyjä erissä ajan muuttuu vastaamaan newtopology.yaml arvo. Jotta näet, että voit muuttaa kokoonpanosi YAML-tiedoston avulla eikä sinun tarvitse topologian Käännä.

On useita muita ominaisuuksia, että valovirta tarjoaa, joka ei ole käsitellään tähän, kuten muuttujien korvaaminen parametrien perusteella YAML tiedostossa välitetty suorituksen aikana tai ympäristömuuttujat. Saat lisätietoja näiden ja muiden ominaisuuksien valovirta Frameworkin [valovirta (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

##<a name="trident"></a>Trident

Trident on korkean tason otetaan, joka tarjoaa myrsky. Se tukee tilallisten käsittely. Trident ensisijainen etuna on se, että se takaa, että jokainen viesti, Lisää topologian käsitellään vain kerran. Tämä on vaikea raaka Java-topologian, mikä takaa on, että viestit käsitellään vähintään kerran saavuttamiseksi. Saatavilla on myös muita eroja, kuten valmiit osat, joita voidaan käyttää sen sijaan, että Pultit. Pienempi kuin Yleinen osia, kuten suodattimia tai funktioita ennusteiden kokonaan itse asiassa korvataan Pultit.

Trident sovellusten voi luoda käyttämällä maven-testi projektit. Voit käyttää samaa perusvaiheet esitetään tämän artikkelin — vain koodi on eri. Trident myös ei (tällä hetkellä) voi käyttää valovirta Framework.

Saat lisätietoja Trident <a href="http://storm.apache.org/documentation/Trident-API-Overview.html" target="_blank">Trident API yleiskatsaus</a>.

Esimerkki Trident-sovellus on artikkelissa [Twitter-Apache myrsky HDInsight-tykkäysten aiheita](hdinsight-storm-twitter-trending.md).

##<a name="next-steps"></a>Seuraavat vaiheet

Kun olet oppinut luomaan myrsky topologian Java. Nyt Lue, miten voit:

* [Käyttöönotto ja Apache myrsky topologioissa HDInsight-hallinta](hdinsight-storm-deploy-monitor-topology.md)

* [Kehitä C# topologioissa Apache myrsky käyttöön Visual Studiossa Hdinsightiin](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Voit etsiä esimerkki myrsky topologioissa ohjesisältöä [Esimerkki topologioissa myrsky HDInsight-varten](hdinsight-storm-example-topology.md).
