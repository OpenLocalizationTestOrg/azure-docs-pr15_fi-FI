<properties
pageTitle="HBase-sovellus maven-testi muodosta ja ota käyttöön Windows-pohjaisesta HDInsight | Microsoft Azure"
description="Opettele käyttämään luominen Java-pohjainen Apache HBase-sovellus ja sitten käyttöön Windows-pohjaisesta Azure HDInsight-klusterin Apache maven-testi."
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
ms.date="10/03/2016"
ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Käytä maven-testi HBase käyttäminen Windows-pohjaisesta HDInsight (Hadoop) Java-sovellusten rakentamiseen

Lue, miten voit luoda ja luoda [Apache HBase](http://hbase.apache.org/) Java-sovellus käyttämällä Apache maven-testi. Sovelluksen käyttäminen Valitse Azure Hdinsightiin (Hadoop).

[Maven-testi](http://maven.apache.org/) on ohjelmiston projektinhallinnan ja ymmärtämistä työkalu, jonka avulla voit muodostaa ohjelmiston, dokumentaatio ja raporttien Java projektit. Tässä artikkelissa kerrotaan, miten sitä käytetään basic Java-sovelluksen luominen, joka luo, kyselyt ja poistaa HBase-taulukon Azure HDInsight-klusterissa.

> [AZURE.NOTE] Tämän asiakirjan vaiheissa oletetaan, että käytössäsi on Windows-pohjaisesta HDInsight-klusterin. Lisätietoja Linux-pohjaiset HDInsight-klusterin käyttämisestä on artikkelissa [Maven-testi avulla voit luoda Java-sovelluksia, jotka käyttävät HBase Linux-pohjaiset HDInsight kanssa](hdinsight-hbase-build-java-maven-linux.md)

##<a name="requirements"></a>Vaatimukset

* [Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 tai uudempi versio

* [Maven-testi](http://maven.apache.org/)


* [Windows-pohjaisesta HDInsight-klusterin HBase kanssa](hdinsight-hbase-tutorial-get-started.md#create-hbase-cluster)


    > [AZURE.NOTE] Ohjeita tämän asiakirjan on testattu HDInsight-klusterin versioita 3.2 ja 3.3. Esimerkkejä oletusarvojen ovat HDInsight 3.3-klusterin.

##<a name="create-the-project"></a>Projektin luominen

1. Komentoriviltä kehittäminen ympäristön muuttaa kansioiden haluamaasi kohtaan, johon haluat luoda projektin, esimerkiksi `cd code\hdinsight`.

2. __Mvn__ -komento, joka on asennettu maven-testi, avulla voit luoda projektin rakennustelineet.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Tämä komento luo kansion sijainnin, nimellä __artifactID__ -parametrin (**hbaseapp** , operaattori: Tässä esimerkissä.) Tämä kansio sisältää seuraavat kohteet:

    * __pom.XML__:-projektin objektimallin ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) sisältää projektin luonnissa käytetyt tiedot ja määritykset tiedot.

    * __SRC__: __main\java\com\microsoft\examples__ -kansio, johon luot sovelluksen sisältävän kansion.

3. Poista __src\test\java\com\microsoft\examples\apptest.java__ tiedostoa, koska sitä ei käytetä tässä esimerkissä.

##<a name="update-the-project-object-model"></a>Päivitä projekti-objektimalli

1. Muokkaa __pom.xml__ -tiedostoa ja lisää seuraava koodi sisällä `<dependencies>` osassa:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Tässä osassa kerrotaan, että projektin vaatii __hbase asiakkaan__ versio __1.1.2__maven-testi. Käännä milloin tämä riippuvuus ladataan oletusarvon maven-testi säilöstä. Voit käyttää [Maven-testi keskitetyn säilöön Etsi](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) lisätietoja tämän riippuvuuden.

    > [AZURE.IMPORTANT] Versionumero on vastattava HBase, joka on mukana HDInsight-klusterin versio. Seuraavan taulukon avulla voit etsiä oikea versionumero.

  	| HDInsight-klusterin versio | HBase-versiota |
  	| ----- | ----- |
  	| 3.2 | 0.98.4-hadoop2 |
  	| 3.3 | 1.1.2 |

    Saat lisätietoja HDInsight-versiot ja -osien [mitkä ovat käytettävissä HDInsight eri Hadoop-komponentteja](hdinsight-component-versioning.md).

2. Jos käytössäsi on HDInsight 3.3-klusterin, sinun on lisättävä seuraavasti `<dependencies>` osassa:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Tämä riippuvuus ladataan Pori core-osia, jotka käyttävät Hbase versio 1.1.x.

2. Lisää seuraava koodi __pom.xml__ -tiedostoon. Tässä osassa on oltava sisällä `<project>...</project>` tunnisteet-tiedoston, esimerkiksi välillä `</dependencies>` ja `</project>`.

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
              <resource>
                <directory>${basedir}/conf</directory>
                <filtering>false</filtering>
                <includes>
                  <include>hbase-site.xml</include>
                </includes>
              </resource>
            </resources>
          <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                </configuration>
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
          </plugins>
        </build>

    `<resources>` Osa määrittää resurssi (__conf\hbase site.xml__), joka sisältää HBase kokoonpanotietoja.

    > [AZURE.NOTE] Voit myös määrittää määritysten arvot koodin kautta. Artikkelissa kerrotaan, mitä voit tehdä tämän jälkeen tulevaa __CreateTable__ esimerkissä kommentit.

    Tämä `<plugins>` osa määrittää [Maven-testi kääntäjän laajennuksen](http://maven.apache.org/plugins/maven-compiler-plugin/) ja [Maven-testi varjostus-laajennus](http://maven.apache.org/plugins/maven-shade-plugin/). Laajennuksen kääntäjän käytetään topologian Käännä. Laajennuksen varjostus käytetään estää käyttöoikeuden monistaminen PURKKI-paketissa, joka on muodostettu maven-testi. Tätä käytetään syynä on kaksoiskappaleiden käyttöoikeustiedostot aiheuttaa virheen HDInsight-klusterin suorituksen aikana. Käyttämällä maven-testi-varjostus-laajennuksen kanssa `ApacheLicenseResourceTransformer` käyttöönoton estää tämän virheen.

    Maven-testi-varjostus-laajennus tuottaa uber purkki (tai fat purkki), joka sisältää kaikkien riippuvuuksien vaatimat sovellus.

3. Tallenna tiedosto __pom.xml__ .

4. Luo uusi kansio nimeltä __conf__ __hbaseapp__ hakemistossa. __Conf__ -kansioon Luo tiedosto nimeltä __hbase site.xml__. Käyttää tiedoston sisällön seuraavasti:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
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
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Tätä tiedostoa käytetään lataamaan HDInsight-klusterin HBase-määrityksiä.

    > [AZURE.NOTE] Tämä on mahdollisimman vähän hbase-site.xml-tiedosto, ja se sisältää HDInsight-klusterin ajoneuvon pienin asetuksia.

3. Tallenna tiedosto __hbase site.xml__ .

##<a name="create-the-application"></a>Sovelluksen luominen

1. Siirry kansioon, __hbaseapp\src\main\java\com\microsoft\examples__ ja app.java tiedoston nimeksi __CreateTable.java__.

2. Avaa __CreateTable.java__ -tiedosto ja korvaa olemassa olevaa sisältöä seuraava koodi:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    Tämä on __CreateTable__ -luokka, joka luo __henkilöt__ -taulukon ja lisää aikatauluun jotkin ennalta määritetyt käyttäjät.

3. Tallenna tiedosto __CreateTable.java__ .

4. __Hbaseapp\src\main\java\com\microsoft\examples__ kansioon Luo uusi tiedosto nimeltä __SearchByEmail.java__. Käyttää tämän tiedoston sisällön seuraava koodi:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    __SearchByEmail__ -luokan avulla voidaan kyselyyn rivien sähköpostiosoitteen perusteella. Koska se käyttää säännöllinen lauseke-suodatin, voit kirjoittaa merkkijono tai säännöllisen lausekkeen luokan käytettäessä.

5. Tallenna tiedosto __SearchByEmail.java__ .

6. __Hbaseapp\src\main\hava\com\microsoft\examples__ kansioon Luo uusi tiedosto nimeltä __DeleteTable.java__. Käyttää tämän tiedoston sisällön seuraava koodi:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    Tämä luokka on tässä esimerkissä Siivotaan poistaminen käytöstä ja pudottamalla taulukko, joka on luotu __CreateTable__ -luokka.

7. Tallenna tiedosto __DeleteTable.java__ .

##<a name="build-and-package-the-application"></a>Muodosta ja sovelluksen

1. Avaa komentorivi-ikkuna ja muuta kansioiden __hbaseapp__ -kansioon.

2. PURKKI tiedosto, joka sisältää sovelluksen luonnissa käytettävien seuraava komento:

        mvn clean package

    Tämä poistaa kaikki edellisen muodosta palvelutiedot, lataa riippuvuuksia, joka ei ole jo asennettu, sitten muodostaa ja paketteja sovellus.

3. Kun komento on valmis, __hbaseapp\target__ hakemisto sisältää tiedoston nimeltä __hbaseapp 1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] __hbaseapp 1.0-SNAPSHOT.jar__ -tiedosto on uber (kutsutaan joskus fat purkki) purkki, joka sisältää kaikki sovelluksen suorittamiseen tarvittava riippuvuudet.

##<a name="upload-the-jar-file-and-start-a-job"></a>Lataa tiedosto PURKKI ja työn aloittaminen

On useita eri tapoja tiedoston lataaminen HDInsight-klusterin, kuvatulla tavalla [Lataa tiedot Hadoop projekteille Hdinsightista](hdinsight-upload-data.md). Azure PowerShellin käyttäminen päällekkäisten seuraavien ohjeiden mukaisesti.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


1. Asentaminen ja määrittäminen PowerShellin Azure, kun luot uuden tiedoston nimeltä __hbase runner.psm1__. Käyttää tämän tiedoston sisällön seuraavasti:

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>
        
        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,
        
        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        
        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,
        
        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )
        
        Set-StrictMode -Version 3
        
        # Is the Azure module installed?
        FindAzure
        
        # Get the login for the HDInsight cluster
        $creds = Get-Credential
        
        # Get storage information
        $storage = GetStorage -clusterName $clusterName
        
        # The JAR
        $jarFile = "wasbs:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"
        
        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex
        
        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
        }
        
        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>
        
        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,
                
                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,
                
                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,
                
                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )
            
            Set-StrictMode -Version 3
            
            # Is the Azure module installed?
            FindAzure
            
            # Get authentication for the cluster
            $creds=Get-Credential
            
            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }
            
            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName
            
            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }
        
        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }
        
        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}
            
            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey
            
            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    Tiedoston nimi sisältää kaksi moduulit:

    * __Lisää HDInsightFile__ - tiedostojen lataaminen HDInsight käytetään

    * __Käynnistä HBaseExample__ - aiemmin luotu luokkien käyttöä varten

2. Tallenna tiedosto __hbase runner.psm1__ .

3. Avaa uuden ikkunan PowerShellin Azure, muuttaa kansioiden __hbaseapp__ -kansioon ja suorita seuraava komento.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Muuta polku aiemmin luotu __hbase runner.psm1__ -tiedoston sijainti. Tämä Rekisteröi moduulin PowerShellin Azure tässä istunnossa.

2. Käytä seuraavaa komentoa __hbaseapp 1.0-SNAPSHOT.jar__ lataaminen HDInsight-klusterin.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Korvaa __hdinsightclustername__ HDInsight-klusterin nimen. Komento lataa __hbaseapp 1.0-SNAPSHOT.jar__ HDInsight-klusterin ensisijainen muistiin __Esimerkki/tölkki__ haluamaasi kohtaan.

3. Kun tiedostot on ladattu, voit luoda taulukon käyttämällä __hbaseapp__seuraava koodi:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Korvaa __hdinsightclustername__ HDInsight-klusterin nimen.

    Tämä komento luo uuden taulukon HDInsight-klusterin __henkilöt__ . Tämä komento ei näy tuloksen konsoli-ikkunassa.

2. Jos haluat etsiä taulukon, käytä seuraavaa komentoa:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Korvaa __hdinsightclustername__ HDInsight-klusterin nimen.

    Tämä komento etsii **SearchByEmail** -luokan kaikki rivit, joiden __contactinformation__ sarakkeen perhe ja __Sähköposti__ -sarake sisältää merkkijonon __contoso.com__avulla. Näyttöön tulee seuraavat tulokset:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Käyttämällä __fabrikam.com__ , `-emailRegex` arvo palauttaa käyttäjät, joilla on __fabrikam.com__ sähköposti-kenttään. Koska tämä haku on toteutettu säännöllisen lausekkeen perustuvan suodattimen avulla, voit myös kirjoittaa säännöllisiä lausekkeita, kuten __^ t__, mitkä palauttaa tapahtumat, jossa sähköpostin alkavat kirjaimella "r".

##<a name="delete-the-table"></a>Poista taulukko

Kun olet saanut esimerkkiä, käytä seuraavaa komentoa PowerShellin Azure-istunnossa poistaa tässä esimerkissä käytetään __henkilöiden__ taulukon:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Korvaa __hdinsightclustername__ HDInsight-klusterin nimen.

##<a name="troubleshooting"></a>Vianmääritys

###<a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Ei tuloksia tai käyttämällä Käynnistä HBaseExample tuottaa odottamattomia tuloksia

Käytä `-showErr` parametri, voit tarkastella keskivirhe (STDERR), joka on valmistettu työn suorittamisen aikana.
