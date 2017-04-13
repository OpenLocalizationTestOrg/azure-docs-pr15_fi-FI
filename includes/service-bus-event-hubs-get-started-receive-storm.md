## <a name="receive-messages-with-apache-storm"></a>Vastaanottamaan viestejä Apache myrsky

[**Apache myrsky**](https://storm.incubator.apache.org) on jaettu reaaliaikainen laskenta, joka helpottaa luotettavaa rajoittamaton virtaa tietojen käsittely. Tässä osassa esitellään tapahtumien vastaanottaa tapahtuman keskittimet tapahtuman keskittimet myrsky nokkaan avulla. Käytä Apache myrsky, voit jakaa tapahtumien useita prosesseja ylläpidettävä eri solmujen yli. Tapahtuman keskittimet integrointi myrsky yksinkertaistaa tapahtuman kulutus läpinäkyvä checkpointing etenemisen myrsky 's Zookeeper-asennuksen avulla, hallita pysyvä tarkistuspisteet ja rinnakkain niistä tapahtuman keskittimet.

Lisätietoja tapahtuman keskittimet vastaanottaa kuvioita on artikkelissa [tapahtuman keskittimet yleiskatsaus][].

Tässä opetusohjelmassa käytetään [HDInsight myrsky][] -asennus, joka sisältää jo käytettävissä tapahtuman keskittimet nokkaan.

1. Voit luoda uuden HDInsight-klusterin ja muodostaa siihen etätyöpöydän kautta noudattamalla [HDInsight myrsky - pääset alkuun](../articles/hdinsight/hdinsight-storm-overview.md) .

2. Kopioi `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` paikallisen kehitysympäristö tiedoston. Tämä sisältää tapahtumat-myrsky-nokkaan.

3. Seuraavalla komennolla Asenna paketti paikallisen maven-testi säilöön. Näin voit lisätä sen myöhemmin myrsky projektin viitetietoina.

        mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar

4. Valitse Pimennys, maven-testi uuden projektin luominen (Valitse **Tiedosto**, sitten **Uusi**ja sitten **projektin**).

    ![][12]

5. Valitse **Käytä työtilan oletussijainti**ja valitse sitten **Seuraava**

6. Valitse **maven-testi-archetype-pikaopas** archetype ja valitse sitten **Seuraava**

7. Lisää **Ryhmätunnus** ja **ArtifactId**ja valitse sitten **Valmis**

8. Lisää seuraavat riippuvuuksien **pom.xml** `<dependency>` solmu.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-core</artifactId>
            <version>0.9.2-incubating</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.microsoft.eventhubs</groupId>
            <artifactId>eventhubs-storm-spout</artifactId>
            <version>0.9</version>
        </dependency>
        <dependency>
            <groupId>com.netflix.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>1.3.3</version>
            <exclusions>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
            <scope>provided</scope>
        </dependency>

9. **Src** -kansioon Luo tiedosto nimeltä **Config.properties** ja kopioi seuraava sisältö, korvaaminen seuraavat arvot:

        eventhubspout.username = ReceiveRule

        eventhubspout.password = {receive rule key}

        eventhubspout.namespace = ioteventhub-ns

        eventhubspout.entitypath = {event hub name}

        eventhubspout.partitions.count = 16

        # if not provided, will use storm's zookeeper settings
        # zookeeper.connectionstring=localhost:2181

        eventhubspout.checkpoint.interval = 10

        eventhub.receiver.credits = 10

    **Eventhub.receiver.credits** arvo määrittää, kuinka monessa lajissa on erämuotoinen ennen kuin vapautat ne myrsky putkijohto. Tässä esimerkissä määrittää yksinkertaisuuden-arvoksi 10. Tuotannon se on yleensä määritettävä suuremmat arvot; Esimerkki: vähintään 1 024.

10. Luo uusi luokka nimeltä **LoggerBolt** seuraava koodi:

        import java.util.Map;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import backtype.storm.task.OutputCollector;
        import backtype.storm.task.TopologyContext;
        import backtype.storm.topology.OutputFieldsDeclarer;
        import backtype.storm.topology.base.BaseRichBolt;
        import backtype.storm.tuple.Tuple;

        public class LoggerBolt extends BaseRichBolt {
            private OutputCollector collector;
            private static final Logger logger = LoggerFactory
                      .getLogger(LoggerBolt.class);

            @Override
            public void execute(Tuple tuple) {
                String value = tuple.getString(0);
                logger.info("Tuple value: " + value);

                collector.ack(tuple);
            }

            @Override
            public void prepare(Map map, TopologyContext context, OutputCollector collector) {
                this.collector = collector;
                this.count = 0;
            }

            @Override
            public void declareOutputFields(OutputFieldsDeclarer declarer) {
                // no output fields
            }

        }

    Tämä myrsky lukko kirjaa vastaanotettu tapahtumat sisällöstä. Tämä voi laajentaa helposti tallentaa monikot tallennustilan-palvelussa. [HDInsight tunnistimen analyysi opetusohjelma] käyttää samaa tämän menetelmän tietojen HBase tallentamiseen.

11. Luo luokan nimeltä **LogTopology** seuraava koodi:

        import java.io.FileReader;
        import java.util.Properties;
        import backtype.storm.Config;
        import backtype.storm.LocalCluster;
        import backtype.storm.StormSubmitter;
        import backtype.storm.generated.StormTopology;
        import backtype.storm.topology.TopologyBuilder;
        import com.microsoft.eventhubs.samples.EventCount;
        import com.microsoft.eventhubs.spout.EventHubSpout;
        import com.microsoft.eventhubs.spout.EventHubSpoutConfig;

        public class LogTopology {
            protected EventHubSpoutConfig spoutConfig;
            protected int numWorkers;

            protected void readEHConfig(String[] args) throws Exception {
                Properties properties = new Properties();
                if (args.length > 1) {
                    properties.load(new FileReader(args[1]));
                } else {
                    properties.load(EventCount.class.getClassLoader()
                            .getResourceAsStream("Config.properties"));
                }

                String username = properties.getProperty("eventhubspout.username");
                String password = properties.getProperty("eventhubspout.password");
                String namespaceName = properties
                        .getProperty("eventhubspout.namespace");
                String entityPath = properties.getProperty("eventhubspout.entitypath");
                String zkEndpointAddress = properties
                        .getProperty("zookeeper.connectionstring"); // opt
                int partitionCount = Integer.parseInt(properties
                        .getProperty("eventhubspout.partitions.count"));
                int checkpointIntervalInSeconds = Integer.parseInt(properties
                        .getProperty("eventhubspout.checkpoint.interval"));
                int receiverCredits = Integer.parseInt(properties
                        .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                    // (opt)
                System.out.println("Eventhub spout config: ");
                System.out.println("  partition count: " + partitionCount);
                System.out.println("  checkpoint interval: "
                        + checkpointIntervalInSeconds);
                System.out.println("  receiver credits: " + receiverCredits);

                spoutConfig = new EventHubSpoutConfig(username, password,
                        namespaceName, entityPath, partitionCount, zkEndpointAddress,
                        checkpointIntervalInSeconds, receiverCredits);

                // set the number of workers to be the same as partition number.
                // the idea is to have a spout and a logger bolt co-exist in one
                // worker to avoid shuffling messages across workers in storm cluster.
                numWorkers = spoutConfig.getPartitionCount();

                if (args.length > 0) {
                    // set topology name so that sample Trident topology can use it as
                    // stream name.
                    spoutConfig.setTopologyName(args[0]);
                }
            }

            protected StormTopology buildTopology() {
                TopologyBuilder topologyBuilder = new TopologyBuilder();

                EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
                topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                        spoutConfig.getPartitionCount()).setNumTasks(
                        spoutConfig.getPartitionCount());
                topologyBuilder
                        .setBolt("LoggerBolt", new LoggerBolt(),
                                spoutConfig.getPartitionCount())
                        .localOrShuffleGrouping("EventHubsSpout")
                        .setNumTasks(spoutConfig.getPartitionCount());
                return topologyBuilder.createTopology();
            }

            protected void runScenario(String[] args) throws Exception {
                boolean runLocal = true;
                readEHConfig(args);
                StormTopology topology = buildTopology();
                Config config = new Config();
                config.setDebug(false);

                if (runLocal) {
                    config.setMaxTaskParallelism(2);
                    LocalCluster localCluster = new LocalCluster();
                    localCluster.submitTopology("test", config, topology);
                    Thread.sleep(5000000);
                    localCluster.shutdown();
                } else {
                    config.setNumWorkers(numWorkers);
                    StormSubmitter.submitTopology(args[0], config, topology);
                }
            }

            public static void main(String[] args) throws Exception {
                LogTopology topology = new LogTopology();
                topology.runScenario(args);
            }
        }


    Tähän luokkaan Luo uusi tapahtuma-keskittimet nokkaan-ominaisuuksien käyttäminen määritykset instatiate se tiedosto. On tärkeää muistaa, että tässä esimerkissä luo niin monta spouts tehtävien tapahtumaa-toiminnossa osioiden määrää voi käyttää suurin rinnakkaisuus sallii sen, että tapahtumaa-toiminnossa.

<!-- Links -->
[Tapahtuman keskittimet yleiskatsaus]: ../articles/event-hubs/event-hubs-overview.md
[HDInsight myrsky]: ../articles/hdinsight/hdinsight-storm-overview.md
[HDInsight tunnistimen analysis-opetusohjelma]: ../articles/hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/service-bus-event-hubs-get-started-receive-storm/create-storm1.png