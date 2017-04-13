<properties
 pageTitle="Kehittää ole MapReduce töiden maven-testi | Microsoft Azure"
 description="Opettele käyttämään ole MapReduce, työn luominen ja sitten käyttöönotto ja suorittamisesta työn HDInsight-klusterissa Hadoop maven-testi."
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
 ms.date="10/18/2016"
 ms.author="larryfr"/>

# <a name="develop-scalding-mapreduce-jobs-with-apache-hadoop-on-hdinsight"></a>Kehittää ole MapReduce töiden Apache Hadoop-Hdinsightiin

Ole on Scala kirjastoon, joka on helppo luoda Hadoop MapReduce töitä. Siinä yksinkertainen syntaksi sekä integrointi Scala.

Tässä asiakirjassa maven-testi avulla voit luoda Scalding kirjoitetut perustiedot Wordin Laske MapReduce työn Opi. Opit sitten asentamisesta ja suorittaa työn HDInsight-klusterissa.

## <a name="prerequisites"></a>Edellytykset

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **A Windows- tai Linux perustuvat Hadoop HDInsight-klusterissa**. Lisätietoja on kohdassa [säännöstä Linux-pohjaiset Hadoop HDInsight-](hdinsight-hadoop-provision-linux-clusters.md) tai [säännöstä Windows-pohjaisesta Hadoop-Hdinsightista](hdinsight-provision-clusters.md) .

* **[Maven-testi](http://maven.apache.org/)**

* **[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 tai uudempi versio**

## <a name="create-and-build-the-project"></a>Luo ja projektin luominen

1. Seuraavalla komennolla voit luoda uuden projektin maven-testi:

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    Tämä komento luo uusi kansio nimeltä **scaldingwordcount**ja luo rakennustelineet Scala-sovellukselle.

2. **Scaldingwordcount** kansioon Avaa **pom.xml** -tiedosto ja korvaa sisältöä seuraavasti:

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
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
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    Tämä tiedosto kuvaa projektin, riippuvuudet ja laajennukset. Seuraavassa on tärkeitä tapahtumia:

    * **maven.Compiler.Source** ja **maven.compiler.target**: määrittää projektin Java-versio

    * **säilöjen tietoihin**: säilöjen tietoihin, jotka sisältävät käyttämät projektin riippuvuuden tiedostot

    * **ole core_2.11** ja **hadoop-core**: Tämä projektin suorittaminen riippuu Scalding ja Hadoop core-paketit

    * **maven-testi-scala-laajennuksen**: laajennuksen kääntää scala sovellukset

    * **maven-testi-varjostus-laajennuksen**: Luo laajennuksen Varjostettu (fat) tölkki. Laajennus sovelletaan suodattimien ja muunnoksia; specificially:

        * **suodattimet**: käytettävien suodattimien muokkaaminen purkki tiedoston mukana metatietoja. Jos haluat estää allekirjoitus poikkeukset suorituksen aikana, tämä ulkopuolelle eri allekirjoitus-tiedostoja, jotka saattavat olla mukana riippuvuudet.

        * **suorituskerran**: paketin vaiheen suorittamisen määritys määrittää **com.twitter.scalding.Tool** luokan tärkeimmät luokaksi paketille. Muuten tarvitse määrittää com.twitter.scalding.Tool sekä luokka, joka sisältää sovelluksen-logiikkaa, kun suoritetaan hadoop-komennolla.

3. Poista **src/testi** -kansio ei ole luot testien tässä esimerkissä kanssa.

4. Avaa **src/main/scala/com/microsoft/example/App.scala** -tiedosto ja korvaa sisältöä seuraavasti:

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    Ottaa käyttöön perustiedot Wordin työn määrä.

5. Tallenna ja sulje tiedostot.

6. Käytä seuraavaa komentoa **scaldingwordcount** hakemiston luominen ja sovelluksen:

        mvn package

    Kun työ on valmis, sisältävän WordCount sovelluksen pakkauksen tukikäytännöistä **target/scaldingwordcount-1.0-SNAPSHOT.jar**.

## <a name="run-the-job-on-a-linux-based-cluster"></a>Suorita työ Linux-pohjaiset klusterissa

> [AZURE.NOTE] Seuraavat vaiheet käyttää SSH ja Hadoop-komento. Saat muista tavoista MapReduce töitä, [Käytä MapReduce Hadoop-HDInsight](hdinsight-use-mapreduce.md).

1. Käytä seuraavaa komentoa paketin lataaminen HDInsight-klusterin:

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    Tämä kopioi tiedostot paikallisesta järjestelmästä pään solmu.

    > [AZURE.NOTE] Jos olet käyttänyt salasana suojaa SSH tili, voit pyydetään antamaan salasana. Jos olet käyttänyt SSH-näppäintä, saatat joutua käyttämään `-i` parametrin ja polun yksityinen avain. Esimerkiksi`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. Käytä seuraavaa komentoa muodostaa yhteyttä klusterin pään solmu:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Jos olet käyttänyt salasana suojaa SSH tili, voit pyydetään antamaan salasana. Jos olet käyttänyt SSH-näppäintä, saatat joutua käyttämään `-i` parametrin ja polun yksityinen avain. Esimerkiksi`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. Kun yhteys pään solmu, käytä seuraavaa komentoa word Laske työn suorittamista varten

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    Tämä suorittaa aiemmin toteutettu WordCount-luokka. `--hdfs`Ohjaa työn käyttämään HDFS. `--input`määrittää syötteen tekstitiedoston, kun `--output` määrittää tulostuskohde.

4. Kun työ on valmis, seuraavat avulla voit tarkastella tulos.

        hdfs dfs -text wasbs:///example/wordcountout/*

    Tässä näkyvät tiedot seuraavankaltaiselta:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="run-the-job-on-a-windows-based-cluster"></a>Suorita Windows-pohjaisesta klusterissa

Windows PowerShellin käyttäminen päällekkäisten seuraavien ohjeiden mukaisesti. Saat muista tavoista MapReduce töitä, [Käytä MapReduce Hadoop-HDInsight](hdinsight-use-mapreduce.md).

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. Käynnistä PowerShellin Azure ja kirjaudu sisään Azure-tiliisi. Kun olet antanut tunnistetiedot-komento palauttaa tietoa tilistäsi.

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. Jos sinulla on useita tilauksia, antaa Tilaustunnus, jota haluat käyttää käyttöönottoa varten.

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] Voit käyttää `Get-AzureRMSubscription` saat luettelon kaikkien tilausten tilisi, joka sisältää kullekin tilauksen tunnus.

4. Käytä seuraavaa komentosarjaa ladataan ja WordCount suoritetaan. Korvaa `CLUSTERNAME` oman HDInsight nimellä klusterin ja varmista, että `$fileToUpload` on oikeat __scaldingwordcount 1.0-SNAPSHOT.jar__ -tiedoston polku.

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = Read-Host -Prompt "Enter the HDInsight cluster name"
        $fileToUpload = Read-Host -Prompt "Enter the path to the scaldingwordcount-1.0-SNAPSHOT.jar file"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential -Message "Enter the login credentials for the cluster"
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                        "--input", `
                        "wasbs:///example/data/gutenberg/davinci.txt", `
                        "--output", `
                        "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     Kun suoritat komentosarjan, voit pyydetään antamaan HDInsight-klusterin järjestelmänvalvojan käyttäjänimi ja salasana. Työn ollessa käynnissä mahdolliset virheet kirjataan konsoliin.
     
6. Kun työ on valmis, tuloste ladataan tiedoston- __output.txt__ nykyiseen kansioon. Seuraavalla komennolla näyttää tulokset.

        cat output.txt

    Tiedostossa pitäisi olla arvot seuraavankaltaiselta:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt oppinut voit luoda MapReduce töitä varten HDInsight Scalding avulla, on seuraavissa linkeissä avulla voit tutkia muita esimerkkejä Azure Hdinsightiin.

* [Rakenteen käyttäminen Hdinsightiin](hdinsight-use-hive.md)

* [Possu käyttäminen Hdinsightiin](hdinsight-use-pig.md)

* [MapReduce työt käyttäminen Hdinsightiin](hdinsight-use-mapreduce.md)
