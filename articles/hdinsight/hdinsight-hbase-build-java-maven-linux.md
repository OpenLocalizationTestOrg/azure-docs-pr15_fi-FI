<properties
    pageTitle="Maven-testi ja Java HBase-sovelluksen luominen ja valitse Ota käyttöön Linux-pohjaiset HDInsight | Microsoft Azure"
    description="Opettele käyttämään Apache maven-testi luominen Java-pohjainen Apache HBase-sovellus ja sitten käyttöön Linux-pohjaiset HDInsight Azure pilveen."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>Käytä maven-testi HBase käyttäminen Linux-pohjaiset HDInsight (Hadoop) Java-sovellusten rakentamiseen

Lue, miten voit luoda ja luoda [Apache HBase](http://hbase.apache.org/) Java-sovellus käyttämällä Apache maven-testi. Sovelluksen käyttäminen Valitse Linux-pohjaiset HDInsight-klusterin.

[Maven-testi](http://maven.apache.org/) on ohjelmiston projektinhallinnan ja ymmärtämistä työkalu, jonka avulla voit muodostaa ohjelmiston, dokumentaatio ja raporttien Java projektit. Tässä artikkelissa kerrotaan, miten sitä käytetään basic Java-sovelluksen luominen, joka luo, kyselyt ja poistaa HBase-taulukon Linux-pohjaiset HDInsight-klusterissa.

> [AZURE.NOTE] Tämän asiakirjan vaiheissa oletetaan, että käytät Linux-pohjaiset HDInsight-klusterin. Lisätietoja Windows-pohjaisesta HDInsight-klusterin käyttämisestä on artikkelissa [Maven-testi avulla voit luoda Java-sovelluksia, jotka käyttävät HBase kanssa Windows-pohjaisesta HDInsight](hdinsight-hbase-build-java-maven.md)

##<a name="requirements"></a>Vaatimukset

* [Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 tai uudempi versio

* [Maven-testi](http://maven.apache.org/)

* [Linux-pohjaiset Azure HDInsight-klusterin HBase kanssa](../hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

    > [AZURE.NOTE] Ohjeita tämän asiakirjan on testattu HDInsight-klusterin versioita 3.2, 3.3 ja 3.4. Esimerkkejä oletusarvojen ovat HDInsight 3.4-klusterin.

* **SSH ja SCP tuntemusta**. Lisätietoja SSH ja SCP käyttäminen Hdinsightista on seuraavissa artikkeleissa:

    * **Linux, Unix tai OS X-asiakkaat**: Katso [Käytä SSH Linux-pohjaiset Hadoop HDInsight Linux, OS X tai Unix-kanssa](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-asiakkaita**: [Käytä SSH kanssa Linux-pohjaiset Hadoop HDInsight Windows-](hdinsight-hadoop-linux-use-ssh-windows.md) kohdassa

##<a name="create-the-project"></a>Projektin luominen

1. Komentorivin kehittäminen-ympäristössä, muuttaa kansioiden haluamaasi kohtaan, johon haluat luoda projektin, esimerkiksi `cd code/hdinsight`.

2. __Mvn__ -komento, joka on asennettu maven-testi, avulla voit luoda projektin rakennustelineet.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Tämä luo uusi kansio nykyiseen kansioon nimellä __artifactID__ -parametrin (**hbaseapp** , operaattori: Tässä esimerkissä.) Tämä kansio sisältää seuraavat kohteet:

    * __pom.XML__:-projektin objektimallin ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) sisältää projektin luonnissa käytetyt tiedot ja määritykset tiedot.

    * __SRC__: kansio, joka sisältää __pää/java/com/microsoft/esimerkkejä__ kansion, johon luot sovelluksen.

3. Poista __src/test/java/com/microsoft/examples/apptest.java__ tiedostoa, koska se ei käytetä tässä esimerkissä.

##<a name="update-the-project-object-model"></a>Päivitä projekti-objektimalli

1. Muokkaa __pom.xml__ -tiedostoa ja lisää seuraava koodi sisällä `<dependencies>` osassa:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Tämä kertoo maven-testi, että projektin vaatii __hbase asiakkaan__ versio __1.1.2__. Käännä milloin tämä ladataan oletusarvon maven-testi säilöstä. Voit käyttää [Maven-testi keskitetyn säilöön Etsi](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) lisätietoja tämän riippuvuuden.

    > [AZURE.IMPORTANT] Versionumero on vastattava HBase, joka on mukana HDInsight-klusterin versio. Seuraavan taulukon avulla voit etsiä oikea versionumero.

  	| HDInsight-klusterin versio | HBase-versiota |
  	| ----- | ----- |
  	| 3.2 | 0.98.4-hadoop2 |
  	| 3.3 ja 3.4 | 1.1.2 |

    Saat lisätietoja HDInsight-versiot ja -osien [mitkä ovat käytettävissä HDInsight eri Hadoop-komponentteja](hdinsight-component-versioning.md).

2. Jos käytössäsi on HDInsight-3.3 tai 3,4 klusterin, sinun on lisättävä seuraavasti `<dependencies>` osassa:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Tämä Lataa Pori core-osat, joita tarvitaan Hbase versiolla 1.1.x.

2. Lisää seuraava koodi __pom.xml__ -tiedostoon. Tämä on oltava sisällä `<project>...</project>` tunnisteet-tiedoston, esimerkiksi välillä `</dependencies>` ja `</project>`.

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

    Tämä määrittää resurssin (__conf/hbase-site.xml__), joka sisältää HBase määritystietoja.

    > [AZURE.NOTE] Voit myös määrittää määritysten arvot koodin kautta. Artikkelissa kerrotaan, mitä voit tehdä tämän jälkeen tulevaa __CreateTable__ esimerkissä kommentit.

    Tämä määrittää myös [Maven-testi kääntäjän laajennuksen](http://maven.apache.org/plugins/maven-compiler-plugin/) ja [Maven-testi varjostus-laajennus](http://maven.apache.org/plugins/maven-shade-plugin/). Laajennuksen kääntäjän käytetään topologian Käännä. Laajennuksen varjostus käytetään estää käyttöoikeuden monistaminen PURKKI-paketissa, joka on muodostettu maven-testi. Tätä käytetään syynä on kaksoiskappaleiden käyttöoikeustiedostot aiheuttaa virheen HDInsight-klusterin suorituksen aikana. Käyttämällä maven-testi-varjostus-laajennuksen kanssa `ApacheLicenseResourceTransformer` käyttöönoton estää tämän virheen.

    Maven-testi-varjostus-laajennus tuottaa uber purkki (tai fat purkki), joka sisältää kaikki sovelluksen vaatii riippuvuudet.

3. Tallenna tiedosto __pom.xml__ .

4. Luo uusi kansio nimeltä __conf__ __hbaseapp__ hakemistossa. Tämä käytetään määritystietoja HBase liittämisestä pitoon.

5. Seuraavalla komennolla kopioi HBase määritysten HDInsight-palvelimesta __conf__ -kansioon. Vaihda **käyttäjänimi** SSH kirjautumisen nimi. Korvaa **CLUSTERNAME** HDInsight-klusterinimi:

        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml

    > [AZURE.NOTE] Jos olet käyttänyt SSH tilin salasanan, voit pyydetään antamaan salasana. Jos olet käyttänyt SSH avain tili, voit joutua käyttämään `-i` parametri avaimen tiedoston polku. Seuraavassa esimerkissä ladataan yksityinen avain `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`

##<a name="create-the-application"></a>Sovelluksen luominen

1. Siirry kansioon, __hbaseapp/src/pää/java/com/microsoft/esimerkkejä__ ja nimeä tiedosto app.java __CreateTable.java__.

2. Avaa __CreateTable.java__ -tiedosto ja korvaa olemassa olevaa sisältöä seuraavasti:

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
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
            
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");

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

4. __Hbaseapp/src/pää/java/com/microsoft/esimerkkejä__ kansioon Luo uusi tiedosto nimeltä __SearchByEmail.java__. Käyttää tämän tiedoston sisällön seuraavasti:

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

6. __Hbaseapp/src/pää/hava/com/microsoft/esimerkkejä__ kansioon Luo uusi tiedosto nimeltä __DeleteTable.java__. Käyttää tämän tiedoston sisällön seuraavasti:

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

2. __Hbaseapp__ -hakemistosta PURKKI tiedosto, joka sisältää sovelluksen luonnissa käytettävien seuraava komento:

        mvn clean package

    Tämä poistaa kaikki edellisen muodosta palvelutiedot, lataa riippuvuuksia, joka ei ole jo asennettu, sitten muodostaa ja paketteja sovellus.

3. Kun komento on valmis, ____ hbaseapp/kohdekansion sisältää tiedoston nimeltä __hbaseapp 1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] __hbaseapp 1.0-SNAPSHOT.jar__ -tiedosto on uber (kutsutaan joskus fat purkki) purkki, joka sisältää kaikki sovelluksen suorittamiseen tarvittava riippuvuudet.

##<a name="upload-the-jar-file-and-run-jobs"></a>Lataa tiedosto PURKKI ja töiden suorittaminen

1. Voit käyttää seuraavaa purkkiin lataaminen HDInsight-klusterin. Vaihda **käyttäjänimi** SSH kirjautumisen nimi. Korvaa **CLUSTERNAME** HDInsight-klusterinimi:

        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Tämä Lataa tiedosto pääkansion SSH käyttäjätilin.

    > [AZURE.NOTE] Jos olet käyttänyt SSH tilin salasanan, voit pyydetään antamaan salasana. Jos olet käyttänyt SSH avain tili, voit joutua käyttämään `-i` parametri avaimen tiedoston polku. Seuraavassa esimerkissä ladataan yksityinen avain `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

2. SSH avulla voit muodostaa yhteyden HDInsight-klusterin. Vaihda **käyttäjänimi** SSH kirjautumisen nimi. Korvaa **CLUSTERNAME** HDInsight-klusterinimi:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] Jos olet käyttänyt SSH tilin salasanan, voit pyydetään antamaan salasana. Jos olet käyttänyt SSH avain tili, voit joutua käyttämään `-i` parametri avaimen tiedoston polku. Seuraavassa esimerkissä ladataan yksityinen avain `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Kun yhteys on muodostettu, voit luoda uuden HBase taulukon Java-sovelluksesta seuraavasti:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable

    Tämä luo uuden HBase-taulukon __henkilöt__, ja lisää tiedot.

4. Etsi tallennettu taulukkoon sähköpostiosoitteet seuraavaksi käyttämällä seuraavaa:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com

    Näyttöön tulee seuraavat tulokset:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

##<a name="delete-the-table"></a>Poista taulukko

Kun olet saanut esimerkkiä, käytä seuraavaa komentoa PowerShellin Azure-istunnossa poistaa tässä esimerkissä käytetään __henkilöt__ -taulukon:

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable

