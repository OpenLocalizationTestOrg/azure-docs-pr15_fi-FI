<properties
    pageTitle="Aikapohjaisten Hadoop Oozie koordinaattien käyttäminen HDInsight | Microsoft Azure"
    description="Käytä aikapohjaisten Hadoop Oozie koordinaattien HDInsight-big datasta palvelu. Lue, miten voit määrittää Oozie työnkulut ja koordinaattorit ja lähettää työt."
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
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>Aikapohjaisten Oozie koordinaattien käyttäminen Hadoop HDInsight määrittäminen työnkulut ja koordinoi töitä

Tässä artikkelissa opit määrittäminen työnkulut ja koordinaattorit sekä käynnistämisestä koordinaattien projektit-ajan perusteella. On hyvä käydä läpi [Käyttäminen Oozie kanssa HDInsight] [ hdinsight-use-oozie] ennen kuin voit lukea tämän artikkelin. Lisäksi Oozie voit ajoittaa työt käyttämällä Azure Data Factory. Lisätietoja Azure Data Factory on artikkelissa [käyttö Possu ja rakenteen Data Factory kanssa](../data-factory/data-factory-data-transformation-activities.md).

> [AZURE.NOTE] Tässä artikkelissa edellyttää Windows-pohjaisesta HDInsight-klusterin. Lisätietoja käyttämällä Oozie, mukaan lukien aikapohjaisten työt Linux-pohjaiset klusterissa on [Käyttää Oozie kanssa Hadoop määrittäminen ja Linux-pohjaiset HDInsight-työnkulun suorittaminen](hdinsight-use-oozie-linux-mac.md)

##<a name="what-is-oozie"></a>Mikä on Oozie

Apache Oozie on työnkulun/yhteensovittamisesta, joka hallitsee Hadoop työt. Hadoop-pino on integroitu ja se tukee Hadoop työt Apache MapReduce, Apache Possu, Apache rakenne ja Apache Sqoop. Myös se voidaan ajoittaa työt, jotka ovat järjestelmä, kuten Java-ohjelmia tai shell-komentosarjoja.

Seuraava kuva esittää työnkulun, voit toteuttaa:

![Työnkulkukaavio][img-workflow-diagram]

Työnkulku sisältää kaksi tehtävää:

1. Rakenne-toiminto suoritetaan HiveQL komentosarjan esiintymiskertojen laskeminen log4j lokitiedostoon kunkin lokin tason tyyppi. Kunkin log4j lokin koostuu rivi-kentät, joka sisältää [LOKIN taso]-kenttä näyttää tyyppi ja vakavuus, esimerkiksi:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Rakenne-komentosarjan tulos on:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Lisätietoja rakenne-kohdassa [Käytä rakenne ja HDInsight][hdinsight-use-hive].

2.  Sqoop toiminto Vie HiveQL toiminnon tulos Azure SQL-tietokannan taulukkoon. Saat lisätietoja Sqoop [Käyttäminen Sqoop kanssa HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Katso HDInsight klustereiden tuetut Oozie versiot, [uudet HDInsight myöntämä klusterin versioissa?] [hdinsight-versions].


##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Työasema Azure PowerShellin avulla**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **HDInsight-klusterin**. Lisätietoja HDInsight-klusterin luomisesta on artikkelissa [Luo HDInsight klustereiden][hdinsight-provision], tai [Aloita HDInsight][hdinsight-get-started]. Tarvitset käsitellään opetusohjelman seuraavat tiedot:

    <table border = "1">
    <tr><th>Klusterin-ominaisuus</th><th>Windows PowerShellin muuttujan nimi</th><th>Arvo</th><th>Kuvaus</th></tr>
    <tr><td>HDInsight-klusterinimi</td><td>$clusterName</td><td></td><td>HDInsight-klusterin, joina suoritat tässä opetusohjelmassa.</td></tr>
    <tr><td>HDInsight-klusterin käyttäjänimi</td><td>$clusterUsername</td><td></td><td>HDInsight-klusterin käyttäjänimi. </td></tr>
    <tr><td>HDInsight klusterin käyttäjän salasana </td><td>$clusterPassword</td><td></td><td>HDInsight klusterin käyttäjän salasana.</td></tr>
    <tr><td>Azure-tallennustilan tilin nimi</td><td>$storageAccountName</td><td></td><td>Azure-tallennustilan tilin käytettävissä HDInsight-klusterin. Käytä tässä opetusohjelmassa tallennustilan oletustilin, jonka olet määrittänyt klusteri valmisteleminen aikana.</td></tr>
    <tr><td>Azure Blob-säilö nimi</td><td>$containerName</td><td></td><td>Tässä esimerkissä käytetään Azure-Blob-tallennustilan säilö, jota käytetään oletusarvon HDInsight-klusterin tiedostojärjestelmässä. Oletusarvon mukaan se on sama nimi kuin HDInsight-klusterin.</td></tr>
    </table>

- **Azure SQL-tietokantaan**. Sinun on määritettävä SQL-tietokantapalvelinta työaseman käyttö sallitaan palomuurisäännön. Lisätietoja luomisesta Azure SQL-tietokannan ja määrittäminen palomuuri, katso [käyttäminen Azure SQL-tietokantaan][sqldatabase-get-started]. Tässä artikkelissa on Windows PowerShell-komentosarjaa luomiseen, jotka tarvitset Tässä opetusohjelmassa Azure SQL-tietokannan taulukkoon.

    <table border = "1">
    <tr><th>SQL-tietokanta-ominaisuus</th><th>Windows PowerShellin muuttujan nimi</th><th>Arvo</th><th>Kuvaus</th></tr>
    <tr><td>SQL-tietokannan nimi</td><td>$sqlDatabaseServer</td><td></td><td>SQL-tietokantapalvelin, johon Sqoop Vie tiedot. </td></tr>
    <tr><td>SQL-tietokannan käyttäjätunnus</td><td>$sqlDatabaseLogin</td><td></td><td>SQL-tietokantaan kirjautumisnimi.</td></tr>
    <tr><td>SQL-tietokannan kirjautumissalasana</td><td>$sqlDatabaseLoginPassword</td><td></td><td>SQL-tietokannan kirjautumissalasana.</td></tr>
    <tr><td>SQL-tietokannan nimi</td><td>$sqlDatabaseName</td><td></td><td>Azure SQL-tietokanta, johon Sqoop Vie tiedot. </td></tr>
    </table>

    > [AZURE.NOTE] Oletusarvoisesti Azure SQL-tietokantaan sallii yhteydet Azure-palvelut, kuten Azure Hdinsightista. Jos palomuuriasetus on poistettu käytöstä, sinun on otettava Azure-portaalista. Katso lisätietoja SQL-tietokannan luominen ja määrittäminen palomuurisäännöt-kohdassa [Create and määrittäminen SQL-tietokantaan][sqldatabase-get-started].


> [AZURE.NOTE] Fill-in taulukoiden arvoja. Se on hyödyllinen Tässä opetusohjelmassa loppuun.


##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Määritä Oozie työnkulun ja Aiheeseen liittyvät HiveQL komentosarja

Oozie työnkulkujen määritelmät kirjoitetaan hPDL (XML prosessin definition language). Työnkulun oletusnimi on *workflow.xml*.  Haluat tallentaa paikallisesti työnkulun tiedoston ja käyttöön sen HDInsight-klusterin käyttämällä PowerShellin Azure jäljempänä tässä opetusohjelmassa.

Työnkulun rakenne-toiminto kutsuu HiveQL komentosarjatiedosto. Komentosarjatiedoston nimi sisältää kolme HiveQL lausetta:

1. **PUDOTA TABLE-lause** poistaa log4j rakennetaulukko, jos se on olemassa.
2. **CREATE TABLE-lause** Luo log4j rakenteen ulkoisen taulukosta, joka osoittaa log4j; lokitiedosto sijaintiin
3.  **Log4j log-tiedoston sijainti**. Kenttäerotin on ";". Rivin oletuserotin on "\n". Ulkoisen rakennetaulukko käytetään välttää tiedosto poistetaan alkuperäisestä sijainnista, siltä varalta, jonka haluat suorittaa työnkulun Oozie useita kertoja.
3. **Lause Lisää korvaa** laskee kunkin lokin tason tyypin log4j rakennetaulukko esiintymät, ja se tallentaa tulosteen Azure-Blob-tallennuspaikkaan.

**Huomautus**: rakenne polku tunnettu ongelma. Tämä ongelma tulla Oozie työn lähetettäessä. Ohjeet ongelman korjaaminen löytyy TechNet wikissä: [HDInsight-rakenne-virhe: ei voi nimetä uudelleen][technetwiki-hive-error].

**Voit määrittää työnkulun voi kutsua HiveQL-komentosarjatiedosto**

1. Luo tekstitiedosto seuraavat sisältöä:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    On kolme muuttujaa käytetään komentosarja:

    - ${hiveTableName}
    - ${hiveDataFolder}
    - ${hiveOutputFolder}

    Työnkulun määritystiedosto (workflow.xml Tässä opetusohjelmassa) välittää nämä arvot HiveQL komentosarjan suorituksen aikana.

2. Tiedoston tallentaminen **C:\Tutorials\UseOozie\useooziewf.hql** käyttämällä ANSI (ASCII) koodaus. (Käytä Muistio, jos tekstieditori ei sisällä asetus.) Tämä komentosarjatiedosto otetaan käyttöön HDInsight-klusterin myöhemmin-opetusohjelman.



**Voit määrittää työnkulun**

1. Luo tekstitiedosto seuraavat sisältöä:

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
            <start to = "RunHiveScript"/>

            <action name="RunHiveScript">
                <hive xmlns="uri:oozie:hive-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.job.queue.name</name>
                            <value>${queueName}</value>
                        </property>
                    </configuration>
                    <script>${hiveScript}</script>
                    <param>hiveTableName=${hiveTableName}</param>
                    <param>hiveDataFolder=${hiveDataFolder}</param>
                    <param>hiveOutputFolder=${hiveOutputFolder}</param>
                </hive>
                <ok to="RunSqoopExport"/>
                <error to="fail"/>
            </action>

            <action name="RunSqoopExport">
                <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.compress.map.output</name>
                            <value>true</value>
                        </property>
                    </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveOutputFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\001"</arg>
                </sqoop>
                <ok to="end"/>
                <error to="fail"/>
            </action>

            <kill name="fail">
                <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>

           <end name="end"/>
        </workflow-app>

    Käytettävissä on kaksi työnkulun määrittämät toimintoja. Käynnistä voit-toiminto on *RunHiveScript*. Jos toiminnon suorittaminen *OK*, seuraava toimi on *RunSqoopExport*.

    RunHiveScript on useita muuttujia. Välittää arvot, kun lähetät Oozie työn työaseman Azure PowerShell-toiminnolla.

    <table border = "1">
    <tr><th>Työnkulun muuttujat</th><th>Kuvaus</th></tr>
    <tr><td>${jobTracker}</td><td>Määritä Hadoop Työn seuranta URL-osoite. Käytä <strong>jobtrackerhost:9010</strong> HDInsight klusterin versiot 3.0- ja 2.0.</td></tr>
    <tr><td>${nameNode}</td><td>Määritä Hadoop nimi-solmu URL-osoite. Käytä oletusarvoista tiedoston järjestelmän wasbs: / / Verkko-osoitteissa, kuten <i>wasbs: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${queueName}</td><td>Määrittää työn lähetetään jonon nimen. Käytä <strong>oletusarvoa</strong>.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Rakenne-toiminnon muuttuja</th><th>Kuvaus</th></tr>
    <tr><td>${hiveDataFolder}</td><td>Luo taulukko rakenne-komennon lähdekansio.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Lisää korvaa-lauseen Tulostekansio.</td></tr>
    <tr><td>${hiveTableName}</td><td>Rakennetaulukko, joka viittaa log4j datatiedostojen nimi.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Sqoop toiminnon muuttuja</th><th>Kuvaus</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>SQL-tietokannan yhteysmerkkijonon.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>Azure SQL-tietokantataulukko, johon tiedot viedään.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Lisää korvaa rakenne-lauseen Tulostekansio. Tämä on samassa kansiossa Sqoop viennin (dir Vie).</td></tr>
    </table>

    Lisätietoja Oozie työnkulun ja työnkulkutoimintojen käyttäminen [Apache Oozie 4.0] ohjeissa[ apache-oozie-400] (for HDInsight klusterin versio 3.0) tai [Apache Oozie 3.3.2 ohjeista] [ apache-oozie-332] (for HDInsight klusterin versio 2.1).

2. Tiedoston tallentaminen **C:\Tutorials\UseOozie\workflow.xml** käyttämällä ANSI (ASCII) koodaus. (Käytä Muistio, jos tekstieditori ei sisällä asetus.)

**Voit määrittää koordinaattien**

1. Luo tekstitiedosto seuraavat sisältöä:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
           <action>
              <workflow>
                 <app-path>${wfPath}</app-path>
              </workflow>
           </action>
        </coordinator-app>

    Viisi muuttujaa käytetään määritystiedostossa on:

  	| Muuttujan          | Kuvaus |
  	| ------------------|------------ |
  	| ${coordFrequency} | Työn keskeyttäminen aika. Taajuus ilmaistaan aina minuuttia. |
  	| ${coordStart}     | Työn aloitusaika. |
  	| ${coordEnd}       | Työn päättymisaika. |
  	| ${coordTimezone}  | Oozie liiketoimintaprosessien kiinteä aikavyöhykkeen koordinaattien töitä ei ole kesäajan (yleensä vastaa käyttämällä UTC-ajan). Tämä aikavyöhyke kutsutaan "Oozie käsittely aikavyöhyke." |
  	| ${wfPath}         | Workflow.xml polku.  Jos työnkulun tiedostonimi ei ole tiedoston oletusnimeä (workflow.xml), sinun on määritettävä. |

2. Tiedoston tallentaminen **C:\Tutorials\UseOozie\coordinator.xml** käyttämällä koodaus ANSI (ASCII). (Käytä Muistio, jos tekstieditori ei sisällä asetus.)

##<a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Ota käyttöön Oozie-projekti ja opetusohjelman valmisteleminen

Voit suorittaa Azure PowerShell-komentosarjaa, seuraavien toimien suorittamiseen:

- Kopioi HiveQL komentosarja (useoozie.hql) Azure-Blob-säiliö wasbs:///tutorials/useoozie/useoozie.hql.
- Kopioi workflow.xml wasbs:///tutorials/useoozie/workflow.xml.
- Kopioi coordinator.xml wasbs:///tutorials/useoozie/coordinator.xml.
- Kopioi tiedot-tiedosto (/ example/data/sample.log), wasbs:///tutorials/useoozie/data/sample.log.
- Luo Azure SQL-tietokannan taulukkoa Sqoop Vie tietojen tallentamista varten. Taulukkonimi on *log4jLogCount*.

**Tietoja HDInsight-tallennustilan**

HDInsight käyttää Azure-Blob-säiliö tietojen tallentamista varten. wasbs: / / on Microsoftin toteutus Azure-Blob-säiliö Hadoop distributed tiedostojärjestelmän (HDFS). Lisätietoja [käytön Azure-Blob-säiliö ja HDInsight][hdinsight-storage].

Kun HDInsight-klusterin valmistella, Azure Blob storage tili ja tietyn säilön kyseiseltä tililtä on määritetty käyttöjärjestelmän, kuten: ÄÄN. Tallennustilan tilin lisäksi voit lisätä lisätallennustilaa tilit samaan Azure tilaus tai eri Azure tilaukset valmistelun yhteydessä. Ohjeet lisätallennustilaa tilien lisäämisestä on artikkelissa [säännöstä HDInsight klustereiden][hdinsight-provision]. Helpottaa käytetty tässä opetusohjelmassa Azure PowerShell-komentosarja kaikki tiedostot on tallennettu osoitteessa */tutorials/useoozie*oletusarvo-tiedoston järjestelmän säilö. Oletusarvoisesti tämä säilö on sama nimi kuin HDInsight-klusterinimi.
Syntaksi on:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [AZURE.NOTE] Vain *wasb: / /* syntaksi on tuettu HDInsight klusterin 3.0-versiota. Vanhempi *ilmastusta: / /* syntaksi voi käyttää HDInsight 2.1 ja 1,6 klustereiden, mutta sitä ei tueta HDInsight 3.0 klustereissa.

> [AZURE.NOTE] Wasb: / / polku on virtual. Katso lisätietoja [Käytä Azure-Blob-säiliö ja HDInsight][hdinsight-storage].

Tiedosto, joka on tallennettu oletusarvon järjestelmässä tiedostosäilön voi käyttää HDInsight jollakin seuraavista URI (käytän workflow.xml esimerkkinä):

    wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasbs:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Jos haluat käyttää tiedostoa suoraan tallennustilan tilin tiedoston blob nimi on:

    tutorials/useoozie/workflow.xml

**Tietoja rakenteen sisäisten ja ulkoisten taulukot**

Tarvitset lisätietoja rakenteen sisäisten ja ulkoisten taulukoiden muutamia asioita:

- Luo taulukko-komento luo sisäinen taulukko tunnetaan myös nimellä hallitun taulukon. Tiedosto on sijaittava oletusarvo-säilössä.
- Luo taulukko-komento siirtyy datatiedoston /hive/varasto/<TableName> kansion oletusarvo-säilössä.
- Luo ulkoinen taulukko-komento luo ulkoisen taulukon. Datatiedosto voi sijaita oletusarvo-säilö ulkopuolella.
- Luo ulkoinen taulukko-komento ei siirry datatiedoston.
- Luo ulkoinen taulukko-komento ei salli alikansiot kohdasta kansio, joka on määritetty sijainti-lauseessa. Tämä on syytä, miksi opetusohjelman tekee sample.log-tiedoston kopio.

Lisätietoja on artikkelissa [HDInsight: rakenne sisäisten ja ulkoisten taulukoiden johdanto][cindygross-hive-tables].

**Opetusohjelman valmisteleminen**

1. Avaa Windows PowerShell ise: (Windows 8: n Käynnistä-näytön Kirjoita **PowerShell_ISE**ja valitse sitten **Windows PowerShell ise:**. Lisätietoja on artikkelissa [Käynnistä Windows PowerShellin Windows 8: ssa ja Windows][powershell-start]).
2. Ala-ruudussa suorittamalla seuraavan komennon muodostaa Azure tilauksen:

        Add-AzureAccount

    Voit pyydetään antamaan Azure tilin tunnistetiedot. Tätä menetelmää tilauksen yhteyden lisääminen aikakatkaistaan ja 12 tunnin jälkeen sinun on suoritettava cmdlet uudelleen.

    > [AZURE.NOTE] Jos sinulla on useita tilauksia Azure ja oletusarvo-tilausta ei ole se, jota haluat käyttää, <strong>Valitse AzureSubscription</strong> cmdlet-komennon avulla voit valita tilauksen.

3. Kopioi seuraava komentosarja script-ruutu ja määritä sitten kuusi muuttujat:

        # WASB variables
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<BlobStorageContainerName>"

        # SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"  
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  
        $sqlDatabaseTableName = "log4jLogsCount"

        # Oozie files for the tutorial
        $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
        $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
        $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

        # WASB folder for storing the Oozie tutorial files.
        $destFolder = "tutorials/useoozie"  # Do NOT use the long path here


    Lisää kuvaukset muuttujien on kohdassa [edellytykset](#prerequisites) Tässä opetusohjelmassa.

3. Liitä komentosarja script-ruudussa seuraavasti:

        # Create a storage context object
        $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        function uploadOozieFiles()
        {
            Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
            Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
            Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
            Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
        }

        function prepareHiveDataFile()
        {
            Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
            Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
        }

        function prepareSQLDatabase()
        {
            # SQL query string for creating log4jLogsCount table
            $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [Level] [nvarchar](10) NOT NULL,
                    [Total] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                [Level] ASC
                )
                )"

            #Create the log4jLogsCount table
            Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
            $conn = New-Object System.Data.SqlClient.SqlConnection
            $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
            $conn.open()
            $cmd = New-Object System.Data.SqlClient.SqlCommand
            $cmd.connection = $conn
            $cmd.commandtext = $cmdCreateLog4jCountTable
            $cmd.executenonquery()

            $conn.close()
        }

        # upload workflow.xml, coordinator.xml, and ooziewf.hql
        uploadOozieFiles;

        # make a copy of example/data/sample.log to example/data/log4j/sample.log
        prepareHiveDataFile;

        # create log4jlogsCount table on SQL database
        prepareSQLDatabase;

4. **Suorita komentosarja** tai paina **F5** komentosarjan suorittaminen. Tulosteesta tulee seuraavanlainen:

    ![Ohjelman valmistelun tulostus][img-preparation-output]

##<a name="run-the-oozie-project"></a>Suorita Oozie projekti

Azure PowerShell tällä hetkellä ei voi minkä tahansa cmdlet-komennot, jolla määritetään Oozie töitä. **Käynnistä RestMethod** cmdlet-komennon avulla voit käynnistää Oozie verkkopalvelut. Oozie web services-Ohjelmointirajapinnan on HTTP muiden JSON-Ohjelmointirajapinnan. Lisätietoja Oozie verkkopalvelut API [Apache Oozie 4.0] ohjeissa[ apache-oozie-400] (for HDInsight klusterin versio 3.0) tai [Apache Oozie 3.3.2 dokumentaatio] [ apache-oozie-332] (for HDInsight klusterin versio 2.1).

**Voit lähettää Oozie työ**

1. Avaa Windows PowerShell ise: (Kirjoita Windows 8: n Käynnistä-näytössä **PowerShell_ISE**ja valitse sitten **Windows PowerShell ise:**. Lisätietoja on artikkelissa [Käynnistä Windows PowerShellin Windows 8: ssa ja Windows][powershell-start]).

3. Kopioi seuraava komentosarja script-ruudussa ja määritä sitten ensin neljäntoista muuttujat (Ohita kuitenkin **$storageUri**).

        #HDInsight cluster variables
        $clusterName = "<HDInsightClusterName>"
        $clusterUsername = "<HDInsightClusterUsername>"
        $clusterPassword = "<HDInsightClusterUserPassword>"

        #Azure Blob storage (WASB) variables
        $storageAccountName = "<StorageAccountName>"
        $storageContainerName = "<BlobContainerName>"
        $storageUri="wasbs://$storageContainerName@$storageAccountName.blob.core.windows.net"

        #Azure SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  

        #Oozie WF/coordinator variables
        $coordStart = "2014-03-21T13:45Z"
        $coordEnd = "2014-03-21T13:45Z"
        $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
        $coordTimezone = "UTC"  #UTC/GMT

        $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
        $waitTimeBetweenOozieJobStatusCheck=10

        #Hive action variables
        $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
        $hiveTableName = "log4jlogs"
        $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
        $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

        #Sqoop action variables
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
        $sqlDatabaseTableName = "log4jLogsCount"

        $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    Lisää kuvaukset muuttujien on kohdassa [edellytykset](#prerequisites) Tässä opetusohjelmassa.

    $coordstart ja $coordend ovat työnkulun alkamis- ja päättymisaika. Voit selvittää UTC-ajan/GMT aika, Etsi bing.com "UTC-ajan aika". Kuinka usein haluat suorittaa työnkulun minuutteina on $coordFrequency.

3. Liitä seuraava komentosarja. Tämän osan määrittää Oozie tiedot:

        #OoziePayload used for Oozie web service submission
        $OoziePayload =  @"
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

           <property>
               <name>nameNode</name>
               <value>$storageUrI</value>
           </property>

           <property>
               <name>jobTracker</name>
               <value>jobtrackerhost:9010</value>
           </property>

           <property>
               <name>queueName</name>
               <value>default</value>
           </property>

           <property>
               <name>oozie.use.system.libpath</name>
               <value>true</value>
           </property>

           <property>
               <name>oozie.coord.application.path</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>wfPath</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>coordStart</name>
               <value>$coordStart</value>
           </property>

           <property>
               <name>coordEnd</name>
               <value>$coordEnd</value>
           </property>

           <property>
               <name>coordFrequency</name>
               <value>$coordFrequency</value>
           </property>

           <property>
               <name>coordTimezone</name>
               <value>$coordTimezone</value>
           </property>

           <property>
               <name>hiveScript</name>
               <value>$hiveScript</value>
           </property>

           <property>
               <name>hiveTableName</name>
               <value>$hiveTableName</value>
           </property>

           <property>
               <name>hiveDataFolder</name>
               <value>$hiveDataFolder</value>
           </property>

           <property>
               <name>hiveOutputFolder</name>
               <value>$hiveOutputFolder</value>
           </property>

           <property>
               <name>sqlDatabaseConnectionString</name>
               <value>&quot;$sqlDatabaseConnectionString&quot;</value>
           </property>

           <property>
               <name>sqlDatabaseTableName</name>
               <value>$SQLDatabaseTableName</value>
           </property>

           <property>
               <name>user.name</name>
               <value>admin</value>
           </property>

        </configuration>
        "@

    >[AZURE.NOTE] Suurin ero verrattuna työnkulun lähetyksen paketti-tiedosto on muuttujan **oozie.coord.application.path**. Kun lähetät työnkulkutyön, voit käyttää **oozie.wf.application.path** sijaan.

4. Liitä seuraava komentosarja. Tämän osan tarkistaa Oozie web palvelutila:

        function checkOozieServerStatus()
        {
            Write-Host "Checking Oozie server status..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieServerSatus = $jsonResponse[0].("systemMode")
            Write-Host "Oozie server status is $oozieServerSatus..."

            if($oozieServerSatus -notmatch "NORMAL")
            {
                Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
                exit 1
            }
        }

5. Liitä seuraava komentosarja. Tässä osassa Luo Oozie työ:

        function createOozieJob()
        {
            # create Oozie job
            Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
            Write-Host "`n--------`n$OoziePayload`n--------"
            $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieJobId = $jsonResponse[0].("id")
            Write-Host "Oozie job id is $oozieJobId..."

            return $oozieJobId
        }

    > [AZURE.NOTE] Kun lähetät työnkulkutyön, sinun on tehtävä toiseen verkkopalvelun kutsu aloittaa työn projektin luomisen jälkeen. Tässä tapauksessa koordinaattien työn käynnistäminen ajan mukaan. Työn käynnistyy automaattisesti.

6. Liitä seuraava komentosarja. Tämän osan tarkistaa Oozie työn tila:

        function checkOozieJobStatus($oozieJobId)
        {
            # get job status
            Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

            Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
            $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")

            while($JobStatus -notmatch "SUCCEEDED|KILLED")
            {
                Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
                Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
                $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
                $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
                $JobStatus = $jsonResponse[0].("status")
            }

            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
            if($JobStatus -notmatch "SUCCEEDED")
            {
                Write-Host "Check logs at http://headnode0:9014/cluster for detais."
                exit -1
            }
        }

7. (Valinnainen) Liitä seuraava komentosarja.

        function listOozieJobs()
        {
            Write-Host "Listing Oozie jobs..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

            write-host "Job ID                                   App Name        Status      Started                         Ended"
            write-host "----------------------------------------------------------------------------------------------------------------------------------"
            foreach($job in $response.workflows)
            {
                Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
            }
        }

        function ShowOozieJobLog($oozieJobId)
        {
            Write-Host "Showing Oozie job info..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
            write-host $response
        }

        function killOozieJob($oozieJobId)
        {
            Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
            $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
            $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
        }

7. Liitä komentosarja seuraavasti:

        checkOozieServerStatus
        # listOozieJobs
        $oozieJobId = createOozieJob($oozieJobId)
        checkOozieJobStatus($oozieJobId)
        # ShowOozieJobLog($oozieJobId)
        # killOozieJob($oozieJobId)

    Poista #-merkit, jos haluat suorittaa muita toimintoja.

7. Jos HDinsight-klusterin on versio 2.1, korvaa "https://$clusterName.azurehdinsight.net:443/oozie/v2/" ja "https://$clusterName.azurehdinsight.net:443/oozie/v1/". HDInsight klusterin versio 2.1 ei ole tukee versio 2 WWW-palveluista.

7. **Suorita komentosarja** tai paina **F5** komentosarjan suorittaminen. Tulosteesta tulee seuraavanlainen:

    ![Opetusohjelma suorittaa työnkulun tulostus][img-runworkflow-output]

8. Yhteyden muodostaminen SQL-tietokantaan voit tarkastella vietyjä tietoja.

**Voit tarkistaa työn virheloki**

Vianmääritys työnkulun Oozie lokitiedosto on osoitteessa C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log-klusterin headnode. Lisätietoja RDP on ohjeaiheessa [käyttäjien hallinta HDInsight klustereiden Azure-portaalissa][hdinsight-admin-portal].

**Opetusohjelman uudelleen**

Voit suorittaa työnkulun, on suoritettava seuraavat toimet:

- Poista rakenne komentosarjan kohdetiedosto.
- Poista log4jLogsCount-taulukon tiedot.

Tässä on esimerkki Windows PowerShell-komentosarja, joiden avulla voit:

    $storageAccountName = "<AzureStorageAccountName>"
    $containerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()


##<a name="next-steps"></a>Seuraavat vaiheet
Tässä opetusohjelmassa opit määrittäminen Oozie-työnkulku ja Oozie koordinaattien ja Oozie-koordinaattien projektin suorittaminen Azure PowerShell-toiminnolla. Lisätietoja on seuraavissa artikkeleissa:

- [HDInsight käytön aloittaminen][hdinsight-get-started]
- [Azure-Blob-säiliö käyttäminen Hdinsightiin][hdinsight-storage]
- [Hallita käyttämällä PowerShellin Azure Hdinsightiin][hdinsight-admin-powershell]
- [Tietojen lataaminen Hdinsightiin][hdinsight-upload-data]
- [Sqoop käyttäminen Hdinsightiin][hdinsight-use-sqoop]
- [Rakenteen käyttäminen Hdinsightiin][hdinsight-use-hive]
- [Possu käyttäminen Hdinsightiin][hdinsight-use-pig]
- [Kehitä Java MapReduce ohjelmien Hdinsightiin][hdinsight-develop-java-mapreduce]



[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563


[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png  

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
