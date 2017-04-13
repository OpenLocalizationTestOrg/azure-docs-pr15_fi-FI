<properties
    pageTitle="Suorita Hadoop-mallit HDInsight | Microsoft Azure"
    description="Aloita Azure HDInsight-palvelun käyttäminen mukana. Käytä PowerShell-komentosarjojen käynnissä olevat tiedot klustereiden MapReduce ohjelmat."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>

#<a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Suorita Hadoop MapReduce näytteiden Windows-pohjaisesta Hdinsightiin

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Esimerkkejä, joiden joukko toimitetaan ohjeiden avulla pääset aloittaminen käynnissä MapReduce työt Hadoop klustereiden Azure Hdinsightiin avulla. Näitä esimerkkejä ovat saatavilla kullakin hallittujen HDInsight-klustereiden, jonka luot. Käynnissä näitä esimerkkejä valintanauhaan voit suorittaa töitä Hadoop klustereiden Azure PowerShellin cmdlet-komennot avulla.

- [**Wordin Laske**][hdinsight-sample-wordcount]: laskee sanan esiintymien tekstitiedostoon.
- [**C#-tietovirta sanamäärän**][hdinsight-sample-csharp-streaming]: laskee sanan esiintymien tekstitiedostoon streaming Hadoop-liittymän avulla.
- [**Pii-arviointi**][hdinsight-sample-pi-estimator]: käyttää tilastollinen (näennäiskorkokauden Monte Carlo) tapaa arvioida piin arvon.
- [**10 Gigatavun Graysort**][hdinsight-sample-10gb-graysort]: suorittaa yleisiä tarkoitus GraySort 10 Gigatavua tiedostoa käyttämällä Hdinsightista. On kolme töitä: Teragen tietoja, voit lajitella tiedot Terasort ja Teravalidate vahvistamaan, että tiedot on lajiteltu oikein.

>[AZURE.NOTE] Lähdekoodin löytyy lisäyksen. 

Hadoop liittyvät tekniikat, kuten Java-pohjainen MapReduce ohjelmoinnin ja streaming ja ohjeista, joita käytetään Windows PowerShellin cmdlet-komennot verkossa on paljon lisäohjeita komentosarjan. Saat lisätietoja näistä resursseista:

- [Kehitä Java MapReduce ohjelmien HDInsight Hadoop](hdinsight-develop-deploy-java-mapreduce-linux.md)
- [Lähetä Hadoop töiden Hdinsightiin](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Johdanto Azure Hdinsightiin][hdinsight-introduction]

Internetissä paljon resursseja valitse rakenne ja Possu MapReduce päälle.  Lisätietoja on artikkelissa:

- [Käytä HDInsight-rakenne](hdinsight-use-hive.md)
- [Possu käyttäminen Hdinsightiin](hdinsight-use-pig.md)
 
**Edellytykset**:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **HDInsight-klusterin**. Katso ohjeet, jossa näiden klustereiden voi luoda monella [luominen Hadoop varausyksiköt Hdinsightista](hdinsight-provision-clusters.md).
- **Työasema Azure PowerShellin avulla**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="hdinsight-sample-wordcount"></a>Laske - Java Word 

Lähetä MapReduce projektin luot MapReduce työn määrityksen. Työ-määrityksessä Määritä purkki MapReduce ohjelmatiedosto ja purkki-tiedoston, joka on **wasbs:///example/jars/hadoop-mapreduce-examples.jar**, luokkanimi ja argumenttien sijainti.  Wordcount MapReduce ohjelma on kaksi argumenttia: lähdetiedosto, jota käytetään sanojen ja tulosteen sijainti laskeminen.

Lähdekoodin löytyy [Liite A](#apendix-a---the-word-count-MapReduce-program-in-java).

Toimissa kehittäminen Java MapReduce-ohjelma, katso - [Hadoop HDInsight kehittää Java MapReduce-ohjelmat](hdinsight-develop-deploy-java-mapreduce-linux.md)
 
**Voit lähettää word Laske MapReduce työ**

1. Avaa **Windows PowerShell ise:**. Katso ohjeet [asentaminen ja määrittäminen PowerShellin Azure][powershell-install-configure].
2. Liitä seuraavaa PowerShell-komentosarjaa:

        $subscriptionName = "<Azure Subscription Name>"
        $resourceGroupName = "<Resource Group Name>"
        $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name
        
        Select-AzureRmSubscription -SubscriptionName $subscriptionName
        
        # Define the MapReduce job
        $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "wordcount" `
                                    -Arguments "wasbs:///example/data/gutenberg/davinci.txt", "wasbs:///example/data/WordCountOutput1"
        
        # Submit the job and wait for job completion
        $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:" 
        $mrJob = Start-AzureRmHDInsightJob `
                            -ResourceGroupName $resourceGroupName `
                            -ClusterName $clusterName `
                            -HttpCredential $cred `
                            -JobDefinition $mrJobDefinition 
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -JobId $mrJob.JobId 
        
        # Get the job output
        $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
        $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
        $defaultStorageContainer = $cluster.DefaultStorageContainer
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -DefaultStorageAccountName $defaultStorageAccount `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultContainer $defaultStorageContainer  `
            -JobId $mrJob.JobId `
            -DisplayOutputType StandardError

        # Download the job output to the workstation
        $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
        Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force
        
        # Display the output file
        cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"

    MapReduce työn tuottaa tiedoston *osa-r-00000*, joka sisältää sanoja ja määrät. Komentosarjan luettelon kaikista sanat, joka sisältää *""* **findstr** -komentoa käyttämällä.

3. Määritä ensin 3 muuttujat ja Suorita komentosarja.

## <a name="hdinsight-sample-csharp-streaming"></a>Laske - C#-tietovirta Word

Hadoop sisältää streaming API MapReduce, jonka avulla voit kirjoittaa kartta ja vähentää kielet kuin Java-funktiot.

> [AZURE.NOTE] Tässä opetusohjelmassa vaiheet koskevat vain Windows-pohjaisesta HDInsight klustereihin. Esimerkki streaming Linux-pohjaiset HDInsight klustereiden varten on artikkelissa [kehittää Python streaming ‑ohjelmien HDInsight](hdinsight-hadoop-streaming-python.md).

Esimerkissä mapper ja reducer ovat suoritettavat, lue syötteen [standardisyötteestä] [ stdin-stdout-stderr] (rivi riviltä) ja lähetyksen [stdout]tulosteet[stdin-stdout-stderr]. Ohjelma laskee kaikki sanat tekstissä.

Kun suoritettavan on määritetty **mappers**, mapper kunkin tehtävän käynnistyy suoritettavan tiedoston erillinen prosessi, kun mapper alustaa. Mapper suoritetaan, kun se muuntaa sen syöttö viivoiksi ja syötteitä rivit, joiden [stdin] [ stdin-stdout-stderr] yhteydessä.

Tässä vaiheessa mapper kerää rivin aloittaminen tulosteen prosessin stdout. Se muuntaa kunkin rivin avain/arvo-pari, johon kerätään mapper tulos. Oletusarvon mukaan etuliite viivan ylöspäin välilehdessä ensimmäinen merkki on avain ja loput (lukuun ottamatta sarkainmerkki) rivi on arvo. Jos rivi ei ole sarkainmerkki, koko rivin pidetään avaimeksi ja arvo on null.

Kun suoritettavan on määritetty **pienennyslaitteita varten**, reducer kunkin tehtävän käynnistyy suoritettavan tiedoston erillinen prosessi, kun reducer alustaa. Reducer suoritetaan, se muuntaa sen avaimen/syöttöarvojen paria rivit ja sen syötteet rivit [stdin] [ stdin-stdout-stderr] yhteydessä.

Sen jälkeen reducer kerää rivin aloittaminen tulosteen [stdout] [ stdin-stdout-stderr] yhteydessä. Se muuntaa kunkin rivin avain/arvo-pari, johon kerätään reducer tulos. Oletusarvon mukaan etuliite viivan ylöspäin välilehdessä ensimmäinen merkki on avain ja loput (lukuun ottamatta sarkainmerkki) rivi on arvo.

Saat lisätietoja Hadoop Streaming-liittymän [Hadoop Streaming] [hadoop-streaming].

**Voit lähettää C# streaming word Laske työ**

- Noudata kohdan [sanamäärä - Java](#word-count-java)ja korvaa työn määritelmän seuraavasti:

        $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                                -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                                -Mapper "cat.exe" `
                                -Reducer "wc.exe" `
                                -InputPath "/example/data/gutenberg/davinci.txt" `
                                -OutputPath "/example/data/StreamingOutput/wc.txt"  


    Kohdetiedosto on:
    
        example/data/StreamingOutput/wc.txt/part-00000      
                                
## <a name="hdinsight-sample-pi-estimator"></a>PII-arviointi

Pii-arviointi käyttää tilastollinen (näennäiskorkokauden Monte Carlo) tapaa arvioida piin arvon. Sijoittaa satunnaisesti sisältyy yksikkö pisteiden kuuluvat neliskulmaiset myös tilavuuteen, neliön sisällä ympyrän ala yhtä todennäköisyydellä ympyrän pii/4. Piin arvo on arvioitu 4R, missä R ympyrää, jotka ovat neliön sisällä pisteiden kokonaismäärän sisällä olevat pisteiden lukumäärä suhde-arvosta. Mitä suurempi pisteiksi käytetään malli sitä parempia arvio on.

Tässä esimerkissä tarkoitettu komentosarja lähettää Hadoop purkki työn ja on käytössä arvolla 16 kartat, kukin ottavat edellytetään parametriarvot laskemiseen 10 miljoonaa otoksen pistettä. Parametriarvot voidaan muuttaa parantamiseksi arvioitu piin arvon. Viittaus-ensimmäiset 10 desimaalit piin ovat 3.1415926535.

**Voit lähettää työn pii arviointi**

- Noudata kohdan [sanamäärä - Java](#word-count-java)ja korvaa työn määritelmän seuraavasti:

        $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "pi" `
                                    -Arguments "16", "10000000"

## <a name="hdinsight-sample-10gb-graysort"></a>10 Gigatavun Graysort

Tässä esimerkissä käytetään vähän 10 gt tietoja niin, että se voidaan suorittaa suhteellisen nopeasti. Tiedostossa käytetään Owen O'Malley ja Arun Murthy, jotka ovat voittaneet vuosittaisen Yleinen ("daytona") teratavun Lajittelu ensisijaisen 2009 0.578 TT/min (100 Teratavua 173 minuutteina) Ylityökorvaus kehittämä MapReduce sovellukset. Lisätietoja tämän ja muiden lajittelu vertailutestit on [Sortbenchmark](http://sortbenchmark.org/) -sivustossa.

Tässä esimerkissä käytetään kolmea MapReduce ohjelmat:

1. **TeraGen** on MapReduce-ohjelma, jonka avulla voit luoda tietorivit, jos haluat lajitella.
2. **TeraSort** esimerkit kenttään annettavat tiedot ja käyttää MapReduce kokonaismäärä tilauksen tietojen lajittelussa. TeraSort on vakio tyyppinen MapReduce Funktiot, lukuun ottamatta mukautetun partitioner, joka käyttää N-1 näyte näppäimet, jotka määrittävät kunkin pienentäminen avaimen alueen lajiteltu luettelo. Erityisesti kaikki avaimet tällaisten, esimerkki [i-1] < = näppäin < otoksen [i] lähetetään vähentää i. Tämä oikeudet tulosteiden vähentää i ovat kaikki alle tulosteen vähentää i + 1.
3. **TeraValidate** on MapReduce, joka vahvistaa, että tulos on lajiteltu yleisesti. Se luo yksi määritys tiedostoa kohden tulostus-kansioon ja kunkin kartan varmistaa, että kukin avain on pienempi tai yhtä suuri edelliseen. Kartta-funktio luo myös kunkin tiedoston ensimmäisen ja viimeisen näppäimet tai tallenteet ja Vähennä-toimintoa varmistaa, että tiedoston i ensimmäiseksi näppäimeksi on suurempi kuin tiedoston i-1 viimeisen avain. Tulos Pienennä, ilmoitettujen ongelmia ja epäjärjestyksessä olevat näppäimet.

Syöttö- ja -muodossa, kaikki kolme sovellusta käyttämä lukee ja kirjoittaa tekstitiedostojen oikeassa muodossa. Tulosteen Vähennä on replikoinnin arvoksi 1, sen sijaan, että oletusarvo on 3, koska ensisijaisen kilpailuun ei edellytä tiedot replikoida voin useita solmujen.

Esimerkki kunkin vastaavan johonkin johdannon MapReduce toimintatavat edellyttämät kolme tehtävää:

1. Luo lajitteluun suorittamalla **TeraGen** MapReduce projektin tiedot.
2. Lajittele tiedot suorittamalla **TeraSort** MapReduce työn.
3. Varmista, että tiedot on lajiteltu oikein suorittamalla **TeraValidate** MapReduce työn.

**Voit lähettää työt**

- Noudata kohdan [sanamäärä - Java](#word-count-java)ja käytä seuraavia Työmääritysten:

    $teragen = uusi AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - LuokanNimi "teragen" "-argumentit"-Dmapred.map.tasks=50 ","100000000","/ Esimerkki/tietojen/10 Gigatavun-lajittelu-syöte"
    
    $terasort = uusi AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - LuokanNimi "terasort" "-argumentit"-Dmapred.map.tasks=50 ","-Dmapred.reduce.tasks=25 ","/ Esimerkki/tietojen/10 Gigatavun-lajittelu-syöte","/ Esimerkki/tietojen/10 Gigatavun-lajittelu-tulokset"
    
    $teravalidate = uusi AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - LuokanNimi "teravalidate" "-argumentit"-Dmapred.map.tasks=50 ","-Dmapred.reduce.tasks=25 ","/ Esimerkki/tietojen/10 Gigatavun-lajittelu-tulokset","/ Esimerkki/tietojen/10 Gigatavun-lajittelu-Vahvista"


##<a name="next-steps"></a>Seuraavat vaiheet 

Tässä artikkelissa ja jokaisen näytteen artikkelit opit suorittaminen kuuluva käyttämällä PowerShellin Azure HDInsight-klustereiden mallit. Hakeminen Possu, rakenne ja MapReduce käyttäminen Hdinsightista on seuraavissa artikkeleissa:

* [Aloita käyttäminen HDInsight-rakenne Hadoop analysointia mobile luuri käyttöä varten][hdinsight-get-started]
* [Possu käyttäminen Hadoop-Hdinsightiin][hdinsight-use-pig]
* [Hadoop HDInsight-rakenteen käyttäminen][hdinsight-use-hive]
* [Lähetä Hadoop töiden Hdinsightiin] [hdinsight-submit-jobs]
* [Azure Hdinsightiin SDK-asiakirjat][hdinsight-sdk-documentation]
* [Korjaa Hadoop HDInsight: virhesanomat] [hdinsight-errors]


## <a name="appendix-a---the-word-count-source-code"></a>Liite A – Word Laske-lähdekoodi

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


## <a name="appendix-b---the-word-count-streaming-source-code"></a>Lisäys B - lähdekoodi streaming sanamäärä

MapReduce-ohjelma käyttää cat.exe sovelluksen määritys-liittymän lähettää tekstin konsolin ja wc.exe sovelluksen nimellä Pienennä-liittymän sanat, jotka asiakirjasta virtauttaa määrän laskeminen. Mapper ja reducer lukea vakio syötteen stream (stdin) merkkiä, rivi riviltä, ja kirjoittaa vakio tulosteen stream (stdout).

    // The source code for the cat.exe (Mapper).

    using System;
    using System.IO;

    namespace cat
    {
        class cat
        {
            static void Main(string[] args)
            {
                if (args.Length > 0)
                {
                    Console.SetIn(new StreamReader(args[0]));
                }

                string line;
                while ((line = Console.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
    }



Cat.cs tiedoston mapper koodi käyttää [StreamReader] [ streamreader] objektin lukemaan saapuvien virta konsoliin, joka kirjoittaa sitten virta vakio tulostus-virtaa, jolla on staattinen [Console.Writeline] merkkien[ console-writeline] menetelmää.


    // The source code for wc.exe (Reducer) is:

    using System;
    using System.IO;
    using System.Linq;

    namespace wc
    {
        class wc
        {
            static void Main(string[] args)
            {
                string line;
                var count = 0;

                if (args.Length > 0){
                    Console.SetIn(new StreamReader(args[0]));
                }

                while ((line = Console.ReadLine()) != null) {
                    count += line.Count(cr => (cr == ' ' || cr == '\n'));
                }
                Console.WriteLine(count);
            }
        }
    }


Wc.cs tiedoston reducer koodi käyttää [StreamReader] [ streamreader] lukeminen merkit, jotka on cat.exe mapper tuloste vakio syötteen virta-objekti. Kun se lukee [Console.Writeline] diftongeja[ console-writeline] menetelmä se laskee sanat mukaan lukien välilyönnit ja rivi merkit kunkin sanan lopussa. Se sitten kirjoittaa kokonaissumma vakio tulosteen virta ja [Console.Writeline] [ console-writeline] menetelmää.





## <a name="appendix-c---the-pi-estimator-source-code"></a>Lisäys C – pii arviointi-lähdekoodi

Pii-arviointi Java-koodia, joka sisältää mapper ja reducer-funktioita ei tarkastus alla. Mapper ohjelma luo asioista sijoittaa satunnaisesti sisältyy yksikön neliön määrä ja laskee, kuinka paljon, ympyrän sisällä olevat. Reducer ohjelma kasvaa pistettä mappers laskema ja laskee kaavan 4R, jossa R on laskettu sisällä, jotka ovat neliön sisällä pisteiden kokonaismäärän ympyrää pisteiden lukumäärä suhde-piin arvon.

    /**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements. See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership. The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License. You may obtain a copy of the License at
    *
    * http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or   implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.math.BigDecimal;
    import java.util.Iterator;

    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.BooleanWritable;
    import org.apache.hadoop.io.LongWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Writable;
    import org.apache.hadoop.io.WritableComparable;
    import org.apache.hadoop.io.SequenceFile.CompressionType;
    import org.apache.hadoop.mapred.FileInputFormat;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.MapReduceBase;
    import org.apache.hadoop.mapred.Mapper;
    import org.apache.hadoop.mapred.OutputCollector;
    import org.apache.hadoop.mapred.Reducer;
    import org.apache.hadoop.mapred.Reporter;
    import org.apache.hadoop.mapred.SequenceFileInputFormat;
    import org.apache.hadoop.mapred.SequenceFileOutputFormat;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;


    //A Map-reduce program to estimate the value of Pi
    //using quasi-Monte Carlo method.
    //
    //Mapper:
    //Generate points in a unit square
    //and then count points inside/outside of the inscribed circle of the square.
    //
    //Reducer:
    //Accumulate points inside/outside results from the mappers.
    //Let numTotal = numInside + numOutside.
    //The fraction numInside/numTotal is a rational approximation of
    //the value (Area of the circle)/(Area of the square),
    //where the area of the inscribed circle is Pi/4
    //and the area of unit square is 1.
    //Then, Pi is estimated value to be 4(numInside/numTotal).
    //

    public class PiEstimator extends Configured implements Tool {
    //tmp directory for input/output
    static private final Path TMP_DIR = new Path(
    PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

    //2-dimensional Halton sequence {H(i)},
    //where H(i) is a 2-dimensional point and i >= 1 is the index.
    //Halton sequence is used to generate sample points for Pi estimation.
    private static class HaltonSequence {
    // Bases
    static final int[] P = {2, 3};
    //Maximum number of digits allowed
    static final int[] K = {63, 40};

    private long index;
    private double[] x;
    private double[][] q;
    private int[][] d;

    //Initialize to H(startindex),
    //so the sequence begins with H(startindex+1).
    HaltonSequence(long startindex) {
    index = startindex;
    x = new double[K.length];
    q = new double[K.length][];
    d = new int[K.length][];
    for(int i = 0; i < K.length; i++) {
    q[i] = new double[K[i]];
    d[i] = new int[K[i]];
    }

    for(int i = 0; i < K.length; i++) {
    long k = index;
    x[i] = 0;

    for(int j = 0; j < K[i]; j++) {
    q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
    d[i][j] = (int)(k % P[i]);
    k = (k - d[i][j])/P[i];
    x[i] += d[i][j] * q[i][j];
    }
    }
    }

    //Compute next point.
    //Assume the current point is H(index).
    //Compute H(index+1).
    //@return a 2-dimensional point with coordinates in [0,1)^2
    double[] nextPoint() {
    index++;
    for(int i = 0; i < K.length; i++) {
    for(int j = 0; j < K[i]; j++) {
    d[i][j]++;
    x[i] += q[i][j];
    if (d[i][j] < P[i]) {
    break;
    }
    d[i][j] = 0;
    x[i] -= (j == 0? 1.0: q[i][j-1]);
    }
    }
    return x;
    }
    }

    //Mapper class for Pi estimation.
    //Generate points in a unit square and then
    //count points inside/outside of the inscribed circle of the square.
    public static class PiMapper extends MapReduceBase
    implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

    //Map method.
    //@param offset samples starting from the (offset+1)th sample.
    //@param size the number of samples for this map
    //@param out output {ture->numInside, false->numOutside}
    //@param reporter
    public void map(LongWritable offset,
    LongWritable size,
    OutputCollector<BooleanWritable, LongWritable> out,
    Reporter reporter) throws IOException {

    final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
    long numInside = 0L;
    long numOutside = 0L;

    for(long i = 0; i < size.get(); ) {
    //generate points in a unit square
    final double[] point = haltonsequence.nextPoint();

    //count points inside/outside of the inscribed circle of the square
    final double x = point[0] - 0.5;
    final double y = point[1] - 0.5;
    if (x*x + y*y > 0.25) {
    numOutside++;
    } else {
    numInside++;
    }

    //report status
    i++;
    if (i % 1000 == 0) {
    reporter.setStatus("Generated " + i + " samples.");
    }
    }

    //output map results
    out.collect(new BooleanWritable(true), new LongWritable(numInside));
    out.collect(new BooleanWritable(false), new LongWritable(numOutside));
    }
    }


    //Reducer class for Pi estimation.
    //Accumulate points inside/outside results from the mappers.
    public static class PiReducer extends MapReduceBase
    implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

    private long numInside = 0;
    private long numOutside = 0;
    private JobConf conf; //configuration for accessing the file system

    //Store job configuration.
    @Override
    public void configure(JobConf job) {
    conf = job;
    }


    // Accumulate number of points inside/outside results from the mappers.
    // @param isInside Is the points inside?
    // @param values An iterator to a list of point counts
    // @param output dummy, not used here.
    // @param reporter

    public void reduce(BooleanWritable isInside,
    Iterator<LongWritable> values,
    OutputCollector<WritableComparable<?>, Writable> output,
    Reporter reporter) throws IOException {
    if (isInside.get()) {
    for(; values.hasNext(); numInside += values.next().get());
    } else {
    for(; values.hasNext(); numOutside += values.next().get());
    }
    }

    //Reduce task done, write output to a file.
    @Override
    public void close() throws IOException {
    //write output to a file
    Path outDir = new Path(TMP_DIR, "out");
    Path outFile = new Path(outDir, "reduce-out");
    FileSystem fileSys = FileSystem.get(conf);
    SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
    outFile, LongWritable.class, LongWritable.class,
    CompressionType.NONE);
    writer.append(new LongWritable(numInside), new LongWritable(numOutside));
    writer.close();
    }
    }

    //Run a map/reduce job for estimating Pi.
    //@return the estimated value of Pi.
    public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
    )
    throws IOException {
    //setup job conf
    jobConf.setJobName(PiEstimator.class.getSimpleName());

    jobConf.setInputFormat(SequenceFileInputFormat.class);

    jobConf.setOutputKeyClass(BooleanWritable.class);
    jobConf.setOutputValueClass(LongWritable.class);
    jobConf.setOutputFormat(SequenceFileOutputFormat.class);

    jobConf.setMapperClass(PiMapper.class);
    jobConf.setNumMapTasks(numMaps);

    jobConf.setReducerClass(PiReducer.class);
    jobConf.setNumReduceTasks(1);

    // turn off speculative execution, because DFS doesn't handle
    // multiple writers to the same file.
    jobConf.setSpeculativeExecution(false);

    //setup input/output directories
    final Path inDir = new Path(TMP_DIR, "in");
    final Path outDir = new Path(TMP_DIR, "out");
    FileInputFormat.setInputPaths(jobConf, inDir);
    FileOutputFormat.setOutputPath(jobConf, outDir);

    final FileSystem fs = FileSystem.get(jobConf);
    if (fs.exists(TMP_DIR)) {
     throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
     + " already exists. Please remove it first.");
     }
     if (!fs.mkdirs(inDir)) {
     throw new IOException("Cannot create input directory " + inDir);
     }

     //generate an input file for each map task
     try {
     for(int i=0; i < numMaps; ++i) {
     final Path file = new Path(inDir, "part"+i);
     final LongWritable offset = new LongWritable(i * numPoints);
     final LongWritable size = new LongWritable(numPoints);
     final SequenceFile.Writer writer = SequenceFile.createWriter(
     fs, jobConf, file,
     LongWritable.class, LongWritable.class, CompressionType.NONE);
     try {
     writer.append(offset, size);
     } finally {
     writer.close();
     }
     System.out.println("Wrote input for Map #"+i);
     }

     //start a map/reduce job
     System.out.println("Starting Job");
     final long startTime = System.currentTimeMillis();
     JobClient.runJob(jobConf);
     final double duration = (System.currentTimeMillis() - startTime)/1000.0;
     System.out.println("Job Finished in " + duration + " seconds");

     //read outputs
     Path inFile = new Path(outDir, "reduce-out");
     LongWritable numInside = new LongWritable();
     LongWritable numOutside = new LongWritable();
     SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
     try {
     reader.next(numInside, numOutside);
     } finally {
     reader.close();
     }

     //compute estimated value
     return BigDecimal.valueOf(4).setScale(20)
     .multiply(BigDecimal.valueOf(numInside.get()))
     .divide(BigDecimal.valueOf(numMaps))
     .divide(BigDecimal.valueOf(numPoints));
     } finally {
     fs.delete(TMP_DIR, true);
     }
     }

    //Parse arguments and then runs a map/reduce job.
    //Print output in standard out.
    //@return a non-zero if there is an error. Otherwise, return 0.
     public int run(String[] args) throws Exception {
     if (args.length != 2) {
     System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
     ToolRunner.printGenericCommandUsage(System.err);
     return -1;
     }

     final int nMaps = Integer.parseInt(args[0]);
     final long nSamples = Long.parseLong(args[1]);

     System.out.println("Number of Maps = " + nMaps);
     System.out.println("Samples per Map = " + nSamples);

     final JobConf jobConf = new JobConf(getConf(), getClass());
     System.out.println("Estimated value of Pi is "
     + estimate(nMaps, nSamples, jobConf));
     return 0;
     }

     //main method for running it as a stand alone command.
     public static void main(String[] argv) throws Exception {
     System.exit(ToolRunner.run(null, new PiEstimator(), argv));
     }
     }
     
## <a name="appendix-d---the-10gb-graysort-source-code"></a>Liite D - 10 gigatavun graysort-lähdekoodi

TeraSort MapReduce ohjelma koodi on jaettu tarkastukseen sisältö.


    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     *     http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

    package org.apache.hadoop.examples.terasort;

    import java.io.IOException;
    import java.io.PrintStream;
    import java.net.URI;
    import java.util.ArrayList;
    import java.util.List;

    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;
    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.filecache.DistributedCache;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.NullWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.Partitioner;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;

    /**
     * Generates the sampled split points, launches the job,
     * and waits for it to finish.
     * <p>
     * To run the program:
     * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
     */

    public class TeraSort extends Configured implements Tool {
      private static final Log LOG = LogFactory.getLog(TeraSort.class);

      /**
       * A partitioner that splits text keys into roughly equal
       * partitions in a global sorted order.
       */

      static class TotalOrderPartitioner implements Partitioner<Text,Text>{
        private TrieNode trie;
        private Text[] splitPoints;

        /**
         * A generic trie node
         */
        static abstract class TrieNode {
          private int level;
          TrieNode(int level) {
            this.level = level;
          }
          abstract int findPartition(Text key);
          abstract void print(PrintStream strm) throws IOException;
          int getLevel() {
            return level;
          }
        }

        /**
         * An inner trie node that contains 256 children based on the next
         * character.
         */
        static class InnerTrieNode extends TrieNode {
          private TrieNode[] child = new TrieNode[256];

          InnerTrieNode(int level) {
            super(level);
          }
          int findPartition(Text key) {
            int level = getLevel();
            if (key.getLength() <= level) {
              return child[0].findPartition(key);
            }
            return child[key.getBytes()[level]].findPartition(key);
          }
          void setChild(int idx, TrieNode child) {
            this.child[idx] = child;
          }
          void print(PrintStream strm) throws IOException {
            for(int ch=0; ch < 255; ++ch) {
              for(int i = 0; i < 2*getLevel(); ++i) {
                strm.print(' ');
              }
              strm.print(ch);
              strm.println(" ->");
              if (child[ch] != null) {
                child[ch].print(strm);
              }
            }
          }
        }

        /**
         * A leaf trie node that does string compares to figure out where the given
         * key belongs between lower..upper.
         */
        static class LeafTrieNode extends TrieNode {
          int lower;
          int upper;
          Text[] splitPoints;
          LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
            super(level);
            this.splitPoints = splitPoints;
            this.lower = lower;
            this.upper = upper;
          }
          int findPartition(Text key) {
            for(int i=lower; i<upper; ++i) {
              if (splitPoints[i].compareTo(key) >= 0) {
                return i;
              }
            }
            return upper;
          }
          void print(PrintStream strm) throws IOException {
            for(int i = 0; i < 2*getLevel(); ++i) {
              strm.print(' ');
            }
            strm.print(lower);
            strm.print(", ");
            strm.println(upper);
          }
        }


        /**
         * Read the cut points from the given sequence file.
         * @param fs the file system
         * @param p the path to read
         * @param job the job config
         * @return the strings to split the partitions on
         * @throws IOException
         */
        private static Text[] readPartitions(FileSystem fs, Path p,
                                             JobConf job) throws IOException {
          SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
          List<Text> parts = new ArrayList<Text>();
          Text key = new Text();
          NullWritable value = NullWritable.get();
          while (reader.next(key, value)) {
            parts.add(key);
            key = new Text();
          }
          reader.close();
          return parts.toArray(new Text[parts.size()]);  
        }

        /**
         * Given a sorted set of cut points, build a trie that will find the correct
         * partition quickly.
         * @param splits the list of cut points
         * @param lower the lower bound of partitions 0..numPartitions-1
         * @param upper the upper bound of partitions 0..numPartitions-1
         * @param prefix the prefix that we have already checked against
         * @param maxDepth the maximum depth we will build a trie for
         * @return the trie node that will divide the splits correctly
         */
        private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                          Text prefix, int maxDepth) {
          int depth = prefix.getLength();
          if (depth >= maxDepth || lower == upper) {
            return new LeafTrieNode(depth, splits, lower, upper);
          }
          InnerTrieNode result = new InnerTrieNode(depth);
          Text trial = new Text(prefix);
          // append an extra byte on to the prefix
          trial.append(new byte[1], 0, 1);
          int currentBound = lower;
          for(int ch = 0; ch < 255; ++ch) {
            trial.getBytes()[depth] = (byte) (ch + 1);
            lower = currentBound;
            while (currentBound < upper) {
              if (splits[currentBound].compareTo(trial) >= 0) {
                break;
              }
              currentBound += 1;
            }
            trial.getBytes()[depth] = (byte) ch;
            result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                         maxDepth);
          }
          // pick up the rest
          trial.getBytes()[depth] = 127;
          result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                        maxDepth);
          return result;
        }

        public void configure(JobConf job) {
          try {
            FileSystem fs = FileSystem.getLocal(job);
            Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
            splitPoints = readPartitions(fs, partFile, job);
            trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
          } catch (IOException ie) {
            throw new IllegalArgumentException("can't read paritions file", ie);
          }
        }

        public TotalOrderPartitioner() {
        }

        public int getPartition(Text key, Text value, int numPartitions) {
          return trie.findPartition(key);
        }

      }

      public int run(String[] args) throws Exception {
        LOG.info("starting");
        JobConf job = (JobConf) getConf();
        Path inputDir = new Path(args[0]);
        inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
        Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
        URI partitionUri = new URI(partitionFile.toString() +
                                   "#" + TeraInputFormat.PARTITION_FILENAME);
        TeraInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        job.setJobName("TeraSort");
        job.setJarByClass(TeraSort.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        job.setInputFormat(TeraInputFormat.class);
        job.setOutputFormat(TeraOutputFormat.class);
        job.setPartitionerClass(TotalOrderPartitioner.class);
        TeraInputFormat.writePartitionFile(job, partitionFile);
        DistributedCache.addCacheFile(partitionUri, job);
        DistributedCache.createSymlink(job);
        job.setInt("dfs.replication", 1);
        TeraOutputFormat.setFinalSync(job, true);
        JobClient.runJob(job);
        LOG.info("done");
        return 0;
      }

      /**
       * @param args
       */

      public static void main(String[] args) throws Exception {
        int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
        System.exit(res);
      }
    }














 




[hdinsight-errors]: hdinsight-debug-jobs.md

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md


[powershell-install-configure]: ../powershell-install-configure.md

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
