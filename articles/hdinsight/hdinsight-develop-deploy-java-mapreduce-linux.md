<properties
    pageTitle="Kehitä Java MapReduce ohjelmat Linux-pohjaiset HDInsight | Microsoft Azure"
    description="Opettele kehittää Java MapReduce ohjelmat ja ota ne Linux-pohjaiset Hdinsightista."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="Blackmist"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight-linux"></a>Kehitä Java MapReduce ohjelmien Hadoop HDInsight Linux

Tämä asiakirja käydään läpi MapReduce-sovelluksen luominen ja valitse Ota käyttöön ja suoritetaan Linux-pohjaiset Hadoop HDInsight-klusterissa Apache maven-testi avulla.

##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 tai uudempi (tai sitä vastaavassa, kuten OpenJDK)

- [Apache maven-testi](http://maven.apache.org/)

- **Azure tilauksen**

- **Azure CLI**

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="configure-environment-variables"></a>Määritä ympäristömuuttujat

Seuraavia ympäristömuuttujia voi määrittää, kun asennat Java ja JDK. Kuitenkin kannattaa tarkistaa, että ne ovat olemassa ja että ne sisältävät järjestelmän oikeat arvot.

* **JAVA_HOME** - osoitettava kansion, johon on asennettu suorituksenaikainen Java-ympäristö (JRE). Esimerkiksi OS X-, Unix- tai Linux-järjestelmässä tunnisteen pitäisi olla samankaltainen kuin arvo `/usr/lib/jvm/java-7-oracle`. Windowsin se on samankaltainen kuin arvo`c:\Program Files (x86)\Java\jre1.7`

* **PATH** - pitäisi olla seuraavat polut:

    * **JAVA_HOME** (tai vastaava polku)

    * **JAVA_HOME\bin** (tai vastaava polku)

    * Kansion, johon on asennettu maven-testi

##<a name="create-a-new-maven-project"></a>Luo uusi projekti maven-testi

1. Muuttaa pääte istunnon tai komentoriviltä kehittäminen-ympäristössä, hakemistojen selaaminen haluat tallentaa projektin kohteeseen.

3. __Mvn__ -komento, joka on asennettu maven-testi, avulla voit luoda projektin rakennustelineet.

        mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Tämä vaihtoehto Luo uusi kansio nykyiseen kansioon-niminen __artifactID__ -parametrin (**wordcountjava** , operaattori: Tässä esimerkissä.) Tämä kansio sisältää seuraavat kohteet:

    * __pom.xml__ - [Projektin Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) , joka sisältää tiedot ja määritykset projektin luonnissa käytetyt tiedot.

    * __src__ - kansio, joka sisältää __pää/java/organisaatiokaavion/apache/hadoop/esimerkkejä__ kansion, johon luot sovelluksen.

3. Poista __src/test/java/org/apache/hadoop/examples/apptest.java__ -tiedosto, kun sitä ei käytetä tässä esimerkissä.

##<a name="add-dependencies"></a>Lisää riippuvuudet

1. Muokkaa __pom.xml__ -tiedostoa ja lisää seuraava sisällä `<dependencies>` osassa:

        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-client-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>

    Tämä kertoo maven-testi, että projektin vaatii kirjastot (lueteltu &lt;artifactId\>) tietyn versiolla (lueteltu &lt;versio\>). Käännä milloin tämä ladataan oletusarvon maven-testi säilöstä. [Maven-testi säilöön haun](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) avulla voit tarkastella Lisää.

    `<scope>provided</scope>` Kertoo maven-testi, että nämä riippuvuuksien olisi voi pakata sovelluksen kanssa, kun ne toimitetaan HDInsight-klusterin suorituksen aikana.

2. Lisää seuraavat __pom.xml__ -tiedostoon. Tämä on oltava sisällä `<project>...</project>` tunnisteet-tiedostoa. esimerkiksi välillä `</dependencies>` ja `</project>`.

        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
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
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
               <source>1.7</source>
               <target>1.7</target>
              </configuration>
            </plugin>
          </plugins>
        </build>

    Ensimmäinen laajennus määrittää [Maven-testi varjostus laajennuksen](http://maven.apache.org/plugins/maven-shade-plugin/), koon muuttamiseen tarkoitettu luominen uberjar (kutsutaan joskus fatjar), joka sisältää riippuvuuksien vaatimat sovellus. Lisäksi se estää käyttöoikeuksien purkki pakkauksessa, jotka saattavat aiheuttaa ongelmia joidenkin järjestelmien kopioinnista.

    Toinen laajennus määrittää maven-testi kääntäjän koon muuttamiseen tarkoitettu asettaminen Java vaatii tämän sovelluksen HDInsight-klusterin käytettyyn versioon.

3. Tallenna tiedosto __pom.xml__ .

##<a name="create-the-mapreduce-application"></a>MapReduce-sovelluksen luominen

1. Siirry kansioon, __wordcountjava/src/pää/java/organisaatiokaavion/apache/hadoop/esimerkkejä__ ja nimeä tiedosto __App.java__ __WordCount.java__.

2. Avaa __WordCount.java__ tiedostoa tekstieditorissa ja korvaa sisältöä seuraavasti:

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

    Ilmoitus pakettinimi on **org.apache.hadoop.examples** ja luokkanimi on **WordCount**. Käyttää näitä nimiä, kun lähetät MapReduce työn.

3. Tallenna tiedosto.

##<a name="build-the-application"></a>Sovelluksen luominen

1. Siirry __wordcountjava__ -kansioon, jos et ole jo.

2. Sovelluksen sisältävän PURKKI tiedoston luonnissa käytettävien seuraava komento:

        mvn clean package

    Tämä Tyhjennä kaikki edellisen muodosta palvelutiedot, lataa riippuvuuksia, joka ei ole jo asennettu, ja luoda ja sovelluksen.

3. Kun komento on valmis, ____ wordcountjava/kohdekansion sisältää tiedoston nimeltä __wordcountjava 1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] __wordcountjava 1.0-SNAPSHOT.jar__ -tiedosto on uberjar, joka sisältää paitsi WordCount työn, mutta myös riippuvuudet, joka työn edellyttää suorituksen aikana.


##<a id="upload"></a>Lataa purkkiin

Käytä seuraavaa komentoa purkki-tiedoston lataaminen HDInsight-headnode:

    scp wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

Tämä kopioi tiedostot paikallisesta järjestelmästä pään solmu.

> [AZURE.NOTE] Jos olet käyttänyt salasana suojaa SSH tili, voit pyydetään antamaan salasana. Jos olet käyttänyt SSH-näppäintä, saatat joutua käyttämään `-i` parametrin ja polun yksityinen avain. Esimerkiksi `scp -i /path/to/private/key wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

##<a name="run"></a>Suorita MapReduce työ

1. Yhdistä HDInsight käyttämällä SSH ohjeiden mukaisesti seuraavissa artikkeleissa:

    - [Linux-pohjaiset Hadoop HDInsight Linux, Unix tai OS X-SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [SSH käyttäminen Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. SSH istunnosta Käytä seuraavaa komentoa MapReduce-sovelluksen käyttämiseen:

        yarn jar wordcountjava.jar org.apache.hadoop.examples.WordCount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/wordcountout

    Tämä davinci.txt tiedoston sanamäärän WordCount MapReduce-sovelluksen avulla, ja tallentaa tulokset __wasbs: / / / Esimerkki/tietojen/wordcountout__. Syöttö- ja tulos on tallennettu klusterin oletusarvo-tallennustilan.

3. Kun työ on valmis, käytä seuraavat tulokset:

        hdfs dfs -cat wasbs:///example/data/wordcountout/*

    Näyttöön tulee luettelo sanat ja laskee arvoilla seuraavankaltaiselta:

        zeal    1
        zelus   1
        zenith  2

##<a id="nextsteps"></a>Seuraavat vaiheet

Tässä asiakirjassa olet oppinut kehittää Java MapReduce työn. Katso seuraavat tiedostot muita tapoja käyttää Hdinsightista.

- [Rakenteen käyttäminen Hdinsightiin][hdinsight-use-hive]
- [Possu käyttäminen Hdinsightiin][hdinsight-use-pig]
- [MapReduce käyttäminen Hdinsightiin](hdinsight-use-mapreduce.md)

Lisätietoja on Katso myös [Java Developer Center](https://azure.microsoft.com/develop/java/).

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

