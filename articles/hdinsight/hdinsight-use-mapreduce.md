<properties
   pageTitle="Hadoop-HDInsight kanssa MapReduce | Microsoft Azure"
   description="Opi suorittamaan Hadoop MapReduce työt HDInsight klustereissa. Suoritat Java MapReduce työn toteutettu perustiedot Wordin Laske-toimintoa."
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

# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>Käytä MapReduce Hadoop-Hdinsightiin

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Tässä artikkelissa kerrotaan suorittaminen MapReduce työt Hadoop-HDInsight klustereissa. Olemme Java MapReduce työn toteutettu perustiedot Wordin Laske toiminnon suorittaminen.

##<a id="whatis"></a>Mikä on MapReduce?

Hadoop MapReduce on ohjelmiston kehys kirjoittamiseen työt, jotka käsittelevät suuria määriä tietoja. Kenttään annettavat tiedot jaetaan riippumaton näkyvissä, jotka sitten käsitellään rinnakkain-yhteyttä klusterin solmut yli. MapReduce työn koostuvat kaksi tehtävää:

* **Mapper**: siinä käytetään syöttötiedot ja analysoi (yleensä ja suodatus ja lajittelu toimintoja) tietokoneesta kuuluu äänimerkki monikot (avain-arvo-pari)
* **Reducer**: siinä käytetään monikot Mapper lähettämän ja tekee yhteenvedon toiminnon, joka luo pienempi, yhdistetyn tuloksen Mapper tiedoista

Seuraavassa kaaviossa on esitetty perustiedot Wordin Laske MapReduce työn Esimerkki:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

Työn tulos on määrä, kuinka monta kertaa kunkin sanan tapahtui teksti, joka on analysoida.

* Mapper otetaan syötteeksi syötteen tekstin kullekin riville ja jakaa sen sanoiksi. Sen tietokoneesta kuuluu äänimerkki avain/arvo pari aina, kun sanan tapahtuu sanan perässä on 1. Tulos on lajiteltu ennen sen lähettämistä reducer.

* Reducer laskee yhteen nämä yksittäiset laskee jokaisen sanan ja tietokoneesta kuuluu äänimerkki avain/Yksiarvoinen pari, joka sisältää summan esiintymien sen perään sana.

MapReduce voidaan toteuttaa useilla kielillä. Java on yleisin toteutus ja käytetään esittelyä tässä asiakirjassa.

### <a name="hadoop-streaming"></a>Hadoop-tietovirta

Kielten tai kehyksiä, jotka perustuvat Java ja Java virtuaalikoneen (esimerkiksi Scalding tai Cascading,) on suorittanut suoraan MapReduce työn, Java-sovelluksen samalla nimellä. Muiden, kuten C# tai Python tai erillisen suoritettavat käytettävä Hadoop streaming.

Hadoop streaming kommunikoi mapper ja reducer STDIN ja STDOUT - mapper ja reducer lukea tietoja rivi kerrallaan standardisyötteestä ja kirjoittaa tulosteen STDOUT. Lue tai lähettämän mapper ja reducer kunkin rivin on oltava avain/arvo pari erotettu toisistaan välilehti charaacter muoto:

    [key]/t[value]

Lisätietoja on artikkelissa [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Esimerkkejä Hadoop HDInsight-tietovirta on seuraavissa artikkeleissa:

* [Kehittää Python MapReduce työt](hdinsight-hadoop-streaming-python.md)

##<a id="data"></a>Esimerkki tiedoista

Tässä esimerkissä tiedot, voit käyttää Leonardo Da Vinci muistikirjat, jotka on tarkoitettu tekstitiedostona HDInsight-klusterin.

Esimerkkitiedot tallennetaan Azure-Blob-säiliö, joka HDInsight käyttää käyttöjärjestelmän Hadoop klustereiden. HDInsight voivat käyttää **wasb** etuliitettä käyttämällä Blob-objektien tallennustilaan tallennettuja tiedostoja. Voit käyttää sample.log-tiedosto, käytä seuraavaa syntaksia:

    wasbs:///example/data/gutenberg/davinci.txt

Koska Azure-Blob-säiliö tallennetaan oletusarvoisesti HDInsight, voit käyttää tiedoston **/example/data/gutenberg/davinci.txt**avulla.

> [AZURE.NOTE] Edellisen syntaksissa **wasbs: / / /** käytetään oletusarvon tallennustilan säiliön HDInsight-klusterin tallennettuja tiedostoja. Jos olet määrittänyt lisätallennustilaa tilit, kun yhteyttä klusterin valmisteltu ja haluat käyttää näissä tileissä tallennettuja tiedostoja, voit käyttää tietoja määrittämällä säilö nimi ja tallennustilaa tilin osoite. Esimerkiksi **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**.

##<a id="job"></a>Esimerkki MapReduce tietoja

MapReduce työ, jota käytetään tässä esimerkissä on osoitteessa **wasbs://example/jars/hadoop-mapreduce-examples.jar**ja se on mukana HDInsight-klusterin. Sisältää sanan Laske-esimerkki, joka vastaan **davinci.txt**suoritetaan.

> [AZURE.NOTE] Klustereiden HDInsight 2.1 tiedostosijainti on **wasbs:///example/jars/hadoop-examples.jar**.

Käyttöä seuraavassa on Wordin Laske MapReduce työn Java-koodi:

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

      public static class TokenizerMapper
           extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
          StringTokenizer itr = new StringTokenizer(value.toString());
          while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
          }
        }
      }

      public static class IntSumReducer
           extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                           Context context
                           ) throws IOException, InterruptedException {
          int sum = 0;
          for (IntWritable val : values) {
            sum += val.get();
          }
          result.set(sum);
          context.write(key, result);
        }
      }

      public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
          System.err.println("Usage: wordcount <in> <out>");
          System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
      }
    }

Katso ohjeet kirjoittaa MapReduce-työ [kehittää Java MapReduce ‑ohjelmien Hdinsightista](hdinsight-develop-deploy-java-mapreduce-linux.md).

##<a id="run"></a>Suorita MapReduce

HDInsight voit suorittaa HiveQL työt usealla eri tavalla. Seuraavan taulukon avulla voit päättää, mitä menetelmä sopii sinulle ja noudata vaiheittainen linkkiä.

| **Käytä tätä**...                                                    | **.. kenen toiminto**                                       | .. .korvaava **klusterin käyttöjärjestelmä** | .. .from **Asiakkaan käyttöjärjestelmä** |
|:-------------------------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md)                       | Hadoop-komennolla **SSH** kautta                  | Linux                                     | Linux, Unix, Mac OS x: ssä tai Windows        |
| [Kääntö](hdinsight-hadoop-use-mapreduce-curl.md)                     | Lähettää työn etäyhteyden käyttämällä **muille käyttäjille**               | Linux- tai Windows                          | Linux, Unix, Mac OS x: ssä tai Windows        |
| [Windows PowerShellin](hdinsight-hadoop-use-mapreduce-powershell.md) | Lähetä työn etäyhteyden **Windows PowerShellin** avulla | Linux- tai Windows                          | Windows                                  |
| [Etätyöpöytä](hdinsight-hadoop-use-mapreduce-remote-desktop)    | Hadoop-komennolla **Etätyöpöydän** kautta       | Windows                                   | Windows                                  |

##<a id="nextsteps"></a>Seuraavat vaiheet

Vaikka MapReduce on tehokas diagnostiikan taidot, se voi olla hankalaa perustyyliin vähän. On useita Java-pohjainen kehyksiä, jotka tiedostojasi on helpompi määrittää MapReduce sovellusten sekä tekniikoita, kuten Possu ja rakenteen, joissa on helpompi tapa käsitellä tietoja Hdinsightista. Lisätietoja on seuraavissa artikkeleissa:

* [Kehitä Java MapReduce ohjelmien Hdinsightiin](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Kehittää Python streaming MapReduce ohjelmien HDInsight varten](hdinsight-hadoop-streaming-python.md)

* [Kehittää ole MapReduce töiden Apache Hadoop-Hdinsightiin](hdinsight-hadoop-mapreduce-scalding.md)

* [Rakenteen käyttäminen Hdinsightiin][hdinsight-use-hive]

* [Possu käyttäminen Hdinsightiin][hdinsight-use-pig]

* [Suorita HDInsight-mallit][hdinsight-samples]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
