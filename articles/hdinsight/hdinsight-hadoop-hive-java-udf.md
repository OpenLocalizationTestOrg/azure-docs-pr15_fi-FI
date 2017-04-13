<properties
pageTitle="Java käyttäjän määrittämä funktio (UDF) käyttäminen HDInsight-rakenne | Microsoft Azure"
description="Lue, miten voit luoda ja käyttää Java käyttäjän määrittämä funktio (UDF) HDInsight-rakenne."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/27/2016"
ms.author="larryfr"/>

#<a name="use-a-java-udf-with-hive-in-hdinsight"></a>Käytä Java UDF kanssa HDInsight-rakenne

Rakenteen soveltuvat erinomaisesti HDInsight tietojen käsitteleminen, mutta joskus tarvitset yleisempien tarkoitukseen kieli. Rakenteen avulla voit luoda käyttäjän määrittämiä funktioita (UDF) erilaisilla ohjelmoinnin kieliä. Tässä asiakirjassa opit käyttämään Java UDF rakenne.

## <a name="requirements"></a>Vaatimukset

* Azure tilauksen

* HDInsight-klusterin (Windows- tai Linux-pohjainen)

    > [AZURE.NOTE] Klusterin molempien tietotyyppien; toimivat useimmissa vaiheet käännetty UDF lataaminen klusterin ja suorittaa sen vaiheet ovat kuitenkin tietyn Linux-pohjaiset klustereihin. Linkit annettavat tiedot, joita voidaan käyttää Windows-pohjaisesta klustereiden.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 tai uudempi (tai sitä vastaavassa, kuten OpenJDK)

* [Apache maven-testi](http://maven.apache.org/)

* Tekstieditorissa tai Java IDE

    > [AZURE.IMPORTANT] Jos käytössäsi on Linux-pohjaiset HDInsight-palvelimeen, mutta Windows työasemassa Python-tiedostojen luominen, sinun on käytettävä muokkaajan, joka käyttää LF rivin loppuun. Jos et ole varma, onko editorin käyttää LF tai CRLF-kohdassa [vianmääritys](#troubleshooting) ohjeet poistamisesta apuohjelmien käyttäminen HDInsight-klusterin CR-merkki.

## <a name="create-an-example-udf"></a>Esimerkki UDF luominen

1. Komentoriviltä avulla voit luoda uuden maven-testi projektin seuraavasti:

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] Jos käytössäsi on PowerShell-on sijoitettava tarjouksia ympärille parametrit. Esimerkiksi `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Tämä vaihtoehto Luo uusi kansio nimeltä __exampleudf__, joka sisältää maven-testi projektin.

2. Kun projekti on luotu, poista __exampleudf/src/testi__ -kansio, joka on luotu osana projektiin. se ei käytetä tässä esimerkissä.

3. Avaa __exampleudf/pom.xml__ja korvaa olemassa olevat `<dependencies>` tapahtuma, jossa on seuraava:

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    Nämä tietueet Määritä version Hadoop ja kuuluva HDInsight 3.3 ja 3,4 klustereiden rakenne. Voit etsiä tietoja Hadoop ja rakenteen mukana HDInsight [HDInsight osan versiotiedot](hdinsight-component-versioning.md) asiakirjasta.

    Lisää `<build>` osan ennen `</project>` rivin tiedoston lopussa. Tässä osassa on oltava seuraavat tiedot:

        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
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
            </plugins>
        </build>
    
    Nämä tietueet määrittää, miten voit luoda projektin. Tarkemmin sanottuna versio, joka käyttää projektiin Java- ja muodostaminen uberjar klusteriin käyttöönottoa varten.

    Tallenna tiedosto, kun muutokset on tehty.

4. Nimeä __exampleudf/src/main/java/com/microsoft/examples/App.java__ __ExampleUDF.java__ja avaa sitten tiedosto-muokkausohjelmassa.

5. Korvaa __ExampleUDF.java__ -tiedoston sisällön seuraavat tiedot, valitse Tallenna tiedosto.

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    Ottaa käyttöön UDF hyväksyy merkkijonoarvo, ja palauttaa merkkijonon pienikirjaiminen versio.

## <a name="build-and-install-the-udf"></a>Muodosta ja asentaa UDF

1. Käytä seuraavaa komentoa haluat kääntää ja pakata UDF:

        mvn compile package

    Tämä vaihtoehto luominen ja pakata UDF __exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__kyselyjä.

2. Käytä `scp` kopioimalla tiedoston HDInsight-klusterin-komennolla.

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    Korvaa __myuser__ yhteyttä klusterin SSH käyttäjätilin. Korvaa __mycluster__ klusterinimeä. Jos olet käyttänyt suojaamiseen SSH tilin salasanan, voit pyydetään antamaan salasana. Jos olet käyttänyt varmennetta, voit joutua käyttämään `-i` parametri, voit määrittää yksityinen avain-tiedosto.

3. Yhdistä käyttäen SSH klusterin. 

        ssh myuser@mycluster-ssh.azurehdinsight.net

    Lisätietoja SSH käyttäminen Hdinsightista on artikkelissa seuraavat asiakirjat.

    * [Linux-pohjaiset Hadoop HDInsight Linux, Unix tai OS X-SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH käyttäminen Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

4. SSH istunnosta kopioi purkki tiedosto HDInsight-tallennustilan.

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>Käytä UDF-rakenne

1. Käynnistä Beeline asiakkaan SSH istunnosta seuraavat avulla.

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Tämä komento oletetaan, että käytit kirjautumistili __järjestelmänvalvojan__ oletusarvon yhteyttä klusterin.

2. Kun olet päässyt `jdbc:hive2://localhost:10001/>` Kysy, kirjoita UDF lisääminen rakenne ja näyttää funktiona seuraavasti.

        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. Muuntaa arvot haetaan taulukosta pieniksi kirjaimiksi merkkijonoja UDF avulla.

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    Tämä laite-ympäristö (Android, Windows, iOS jne.) Valitse taulukosta, merkkijonon muuntaminen pienet kirjaimet, ja näyttää ne. Tulos tulee näkyviin seuraavankaltaiselta.

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Seuraavat vaiheet

Katso rakenteen käyttäminen muilla tavoilla, [Käytä rakenne ja Hdinsightista](hdinsight-use-hive.md).

Lisätietoja Hive User-Defined Funktiot, katso [rakenne operaattorit ja käyttäjän määrittämät funktiot](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) osan rakenne-wiki osoitteessa apache.org.
