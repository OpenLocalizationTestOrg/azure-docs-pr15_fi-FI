<properties
    pageTitle="Luo suosituksia Mahout ja WIndows-pohjaisesta HDInsight | Microsoft Azure"
    description="Opettele Apache Mahout-konepohjaisten oppimistekniikoiden kirjaston avulla voit luoda elokuvan suositukset Windows-pohjaisesta HDInsight (Hadoop)."
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
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight"></a>Luo elokuvan suosituksia käyttämällä Apache Mahout HDInsight Hadoop

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Opettele käyttämään [Apache Mahout](http://mahout.apache.org) -konepohjaisten oppimistekniikoiden Azure Hdinsightiin kirjastosta elokuvan suosituksia luomiseen.

> [AZURE.NOTE] Ohjeita tämän asiakirjan vaativat Windows-asiakas- ja Windows-pohjaisesta HDInsight-klusterin. Lisätietoja Mahout Linux, OS X tai Unix-asiakasohjelman käyttäminen Linux-pohjaiset HDInsight-klusterin on artikkelissa [Luo elokuvan suosituksia käyttämällä Apache Mahout Linux-pohjaiset Hadoop-HDInsight kanssa](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>Mitä opit

Mahout on [konepohjaisten oppimistekniikoiden] [ ml] Apache Hadoop-kirjastossa. Mahout sisältää algoritmit tietoja, kuten suodatus, luokittelu- ja klusterointi käsittelyä varten. Tässä artikkelissa suositus ohjelma käyttää elokuvan suositukset, jotka perustuvat elokuvat ystäviesi tuliko luomiseen. Opit myös tietoja luokitukset päätös metsää kanssa. Tämä opeta voit seuraavasti:

* Voit suorittaa Mahout töitä Windows PowerShellin avulla

* Voit suorittaa Mahout töitä Hadoop komentoriviltä

* Mahout asentamisesta HDInsight 3.0- ja HDInsight 2.0 klustereiden

    > [AZURE.NOTE] Mahout annetaan varausyksiköiden HDInsight 3.1-version kanssa. Jos käytössäsi on aiemmassa versiossa HDInsight-kohdassa [Asenna Mahout](#install) ennen kuin voit jatkaa.

##<a name="prerequisites"></a>edellytykset

- **Windows-pohjaisesta Hadoop klusterin Hdinsightista**. Lisätietoja luominen on [Hadoop HDInsight käytön aloittaminen][getstarted]
- **Työasema Azure PowerShellin avulla**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="recommendations"></a>Luo suosituksia Windows PowerShellin avulla

> [AZURE.NOTE] Vaikka työ käytetyissä tässä osassa toimii Windows PowerShellin avulla, monia Mahout mukana luokkia ei tällä hetkellä toimi Windows PowerShellin avulla ja ne on suoritettava Hadoop komentorivin avulla. Luettelo luokat, jotka eivät toimi Windows PowerShellin avulla on kohdassa [vianmääritys](#troubleshooting) .
>
> Esimerkki Mahout töiden Hadoop komentorivin avulla on kohdassa [luokitusta tiedot Hadoop-komentorivin avulla](#classify).

Funktioita, joka tarjoaa Mahout on suositus ohjelma. Tämä versio voidaan käyttää tietojen muoto `userID`, `itemId`, ja `prefValue` (käyttäjien tilaksi kohteen). Mahout sitten suorittaa muiden occurance analyysi määrittämään: _käyttäjät, joilla on kohteen asetus on myös muita kohteita ensisijaisuus_. Valitse mahout määrittää käyttäjät, joilla on tykkää-kohteen asetukset, joita voidaan käyttää suosituksia.

Seuraavassa on erittäin yksinkertaisia esimerkki, joka käyttää elokuvat:

* __Muiden occurance__: Esko, Anneli ja Pekka kaikki tykännyt _tähti sotien_ _Empire joina takaisin_ja _palauttamista Jedi_. Mahout määrittää, että käyttäjät, jotka, kuten jokin näistä elokuvat myös, kuten muuhun rajoitteeseen.

* __Muiden occurance__: Teemu ja Anneli myös tykännyt _Haamuksi Menace_ _kloonit hyökkäyksen_ja _Sith Revenge_. Mahout määrittää, että käyttäjät, jotka tykännyt edellisen kolme elokuvat myös, kuten nämä kolme.

* __Samanlaiset suositus__: koska Esko tykännyt kolme ensimmäistä elokuvat-elokuvat tarkastelee Mahout, joita muut kanssa samalla olevaa tykännyt, mutta ei Esko on seurattavat (tykännyt/luokitellut). Tässä tapauksessa Mahout suosittelee _Haamu Menace_ _kloonit hyökkäyksen_ja _Sith Revenge_.

###<a name="understanding-the-data"></a>Tietoja tiedot

Kätevästi, [GroupLens Oheistiedot] [ movielens] on luokitus tietoja muodossa, joka on yhteensopiva Mahout elokuvat. Nämä tiedot ovat käytettävissä yhteyttä klusterin oletusarvon tallennustilan `/HdiSamples/MahoutMovieData`.

On kaksi tiedostoa `moviedb.txt` (elokuvat-tiedot) ja `user-ratings.txt`. Käyttäjän ratings.txt tiedoston käytetään analyysi-aikana, kun moviedb.txt käytetään antamaan käyttäjäystävällinen tekstin infromation analyysin tulokset näytetään.

Käyttäjän ratings.txt sisältämät tiedot, rakenne on `userID`, `movieID`, `userRating`, ja `timestamp`, joka kertoo us miten keskeiset kunkin käyttäjän aiheet elokuvan. Tässä on esimerkki tiedot:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

###<a name="run-the-job"></a>Suorita työ

Seuraavia Windows PowerShell-komentosarjaa avulla voit suorittaa työn, joka käyttää Mahout suositus ohjelma elokuvan tiedot:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get HTTPS/Admin credentials for submitting the job later
    $creds = Get-Credential
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
            
    # NOTE: The version number in the file path
    # may change in future versions of HDInsight.
    $jarFile =  "file:///C:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasbs:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasbs:///HdiSamples/MahoutMovieData/user-ratings.txt",
                    "--output", "wasbs:///example/out",
                    "--tempDir", "wasbs:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Mahout töitä ei Poista väliaikaiset käsiteltäessä työn luotavaan tietojen. `--tempDir` -Parametri on määritetty Esimerkki työn eristää väliaikaiset tiedostot määritettyyn kansioon.

Mahout työn ei palauta tulosteen STDOUT. Sen sijaan se tallentaa sen määritetyn tulosteen hakemistossa __osa-r-00000__. Komentosarjan Lataa tämän tiedoston __output.txt__ työasemalle nykyiseen kansioon.

Seuraavassa on esimerkki tämän tiedoston sisällön:

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

Ensimmäinen sarake näyttää `userID`. Sisältämiä arvoja ' ["ja"]' ovat `movieId`:`recommendationScore`.

###<a name="view-the-output"></a>Näytä tulos

Luotu tulos voi olla OK sovelluksen käyttöä, vaikka se ei ole hyvin luettavissa. `moviedb.txt` Palvelimesta avulla voidaan ratkaista `movieId` elokuvan nimi, mutta se on ensin ladattava se ja luokitukset-tiedoston palvelimesta, käyttämällä seuraavaa komentosarjaa:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
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
    #Download the files
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/moviedb.txt" `
    -Container $container `
    -Destination moviedb.txt `
    -Context $context
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/user-ratings.txt" `
    -Container $container `
    -Destination user-ratings.txt `
    -Context $context

Kun olet ladannut tiedostoja, käytä seuraavaa PowerShell-komentosarjaa Näytä suositukset elokuvan nimillä:

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "user-ratings.txt"
            -movieFile "moviedb.txt"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

Seuraavassa on esimerkki komentosarja:

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

Tulosteen pitäisi näyttää seuraavankaltaiselta:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="classify"></a>Luokitella tietoja Hadoop-komentorivin avulla

Käytettävissä Mahout luokitus tavoilla on [satunnainen metsää]muodostaa[forest]. Tämä on useita toimenpide, joka liittyy Luo päätös puita, joita käytetään sitten luokitella tietoja koulutus tietojen avulla. Tämä käyttää myöntämä Mahout __org.apache.mahout.classifier.df.tools.Describe__ -luokka. Se tällä hetkellä on suoritettava Hadoop komentoriviltä.

###<a name="load-the-data"></a>Lataa tiedot

1. Lataa [NSL KDD tietojoukon](http://nsl.cs.unb.ca/NSL-KDD/)seuraavat tiedostot.

  * [KDDTrain +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): koulutus-tiedosto

  * [KDDTest +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): testitiedot

2. Avaa tiedostoille ja Poista rivit, joiden nimet alkavat yläreunassa '@', ja tallentaa tiedostoja. Jos nämä ei poisteta, näyttöön tulee virhesanomia käytettäessä Mahout tiedoista.

2. Lataa tiedostot __esimerkkitietoja /__. Voit tehdä tämän käyttämällä seuraavaa komentosarjaa. Korvaa __CLUSTERNAME__ HDInsight-klusterin nimen. Vaihda nimi Tiedostonimi ladattava tiedosto fontin.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
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

###<a name="run-the-job"></a>Suorita työ

1. Työn edellyttää Hadoop-komentoriviltä. Etätyöpöytä käyttöön HDInsight-klusterin ja muodostaa siihen voidaan [muodostaa käyttämällä RDP HDInsight klustereihin](hdinsight-administer-use-management-portal.md#rdp)ohjeita noudattamalla.

3. Kun yhteys on muodostettu, Avaa Hadoop-komentoriviltä __Hadoop komentorivi__ -kuvakkeen avulla:

    ![hadoop cli][hadoopcli]

3. Seuraavalla komennolla voit luoda-tiedoston kuvaus (__KDDTrain + .info__), joka käyttää Mahout.

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasbs:///example/data/KDDTrain+.arff" -f "wasbs:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` Tiedot tiedoston kuvaa. Esimerkiksi L osoittaa otsikko.

4. Hakemiston luominen metsää päätös puiden avulla seuraava komento:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasbs:///example/data/KDDTrain+.arff -ds wasbs:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    Tämän toiminnon tulos on tallennettu __nsl metsää__ kansioon, joka sijaitsee HDInsight-klusterin osoitteessa __ wasbs://user/ muistiin&lt;käyttäjänimi > / nsl-metsää/nsl-forest.seq. &lt;Käyttäjänimi > on käyttäjänimi, jota käytit Etätyöpöytä-istunnossa. Tätä tiedostoa ei voi lukea ihmisiin.

5. Testaa metsää luokittelemalla __KDDTest + .arff__ tietojoukko. Kirjoita seuraava komento:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasbs:///example/data/KDDTest+.arff -ds wasbs:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasbs:///example/data/predictions

    Tämä komento palauttaa luokituksen prosessin yhteenvetotiedot seuraavankaltaiselta:

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  Työn tuottaa myös osoitteessa __wasbs:///example/data/predictions/KDDTest+.arff.out__tiedoston. Tätä tiedostoa ei kuitenkaan voi lukea ihmisiin.

> [AZURE.NOTE] Mahout työt korvaa tiedostoja. Jos haluat suorittaa nämä työt uudelleen, sinun on poistettava edellisen töiden luotuja tiedostoja.

##<a name="troubleshooting"></a>Vianmääritys

###<a name="install"></a>Asenna Mahout

Mahout on asennettu HDInsight 3.1 klustereiden ja se voidaan asentaa manuaalisesti HDInsight 3.0- tai HDInsight 2.1 varausyksiköiden mukaan seuraavasti:

1. Mahout käyttämään version määräytyy yhteyttä klusterin HDInsight-versiota. Voit etsiä klusterin-version avulla klusterin ominaisuuksien tarkasteleminen Azure-portaalissa.

  * __HDInsight 2.1__, voit ladata Java-arkisto (JAR)-tiedosto, joka sisältää [Mahout 0,9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar).

  * __HDInsight 3.0__, sinun täytyy [luoda Mahout lähteestä] [ build] ja määritä myöntämä HDInsight Hadoop-versio. Asenna muodosta sivulla luetellut edellytykset, lataa lähde ja luo Mahout purkki tiedostot seuraava komento avulla:

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] Kun Mahout 1.0 on julkaistu, sinun pitäisi käyttää valmiiksi pakettien HDInsight 3.0.

2. __Esimerkki/tölkki__ oletusarvon muistiin yhteyttä klusterin purkki-tiedoston lataaminen. Korvaa CLUSTERNAME seuraavassa HDInsight-klusterin nimi ja __mahout-coure-0,9-job.jar__ -tiedoston polku ja tiedostonimi...

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
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
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###<a name="cannot-overwrite-files"></a>Tiedostoja ei voi korvata.

Mahout töitä ei Puhdista väliaikaiset tiedostot, jotka on luotu käsiteltäessä. Lisäksi töitä ei korvaa olemassa olevan kohdetiedosto.

Voit välttää virheet Mahout töitä, Poista väliaikaiset- ja tiedostoja suoritetaan välillä tai käytä yksilöllinen tilapäinen- ja -kansioiden nimiä.

###<a name="cannot-find-the-jar-file"></a>JAR-tiedostoa ei löydy

HDInsight 3.1 klustereiden sisältävät Mahout. Polku ja tiedostonimi ovat Mahout, johon on asennettu klusterin versionumero. Tässä opetusohjelmassa Windows PowerShellin Esimerkki-komentosarja käyttää polku on kelvollinen saavuttaman marraskuun 2015, mutta versionumero muuttuu tulevaisuudessa päivitykset Hdinsightista. Voit määrittää nykyisen yhteyttä klusterin Mahout JAR-tiedoston polku, käytä Windows PowerShell-komentoa ja muokkaa sitten viitataan tiedostopolku, joka palautetaan komentosarjaa:

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasbs:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="nopowershell"></a>Luokat, jotka eivät toimi Windows PowerShellin avulla

Mahout työt, jotka käyttävät seuraavat luokat palauttaa virhesanomia erilaisia, jos niitä käytetään Windows PowerShellin:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Voit suorittaa työt, jotka käyttävät kyseisten luokkien, muodosta yhteys HDInsight-klusterin ja suorita työt Hadoop komentorivin avulla. Kohdassa [luokitusta tiedot Hadoop komentorivin](#classify) esimerkki.

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt oppinut voit Mahout käyttämisestä, tutustu muita tapoja HDInsight tietojen käyttäminen:

* [Rakenne ja Hdinsightiin](hdinsight-use-hive.md)
* [Possu Hdinsightiin](hdinsight-use-pig.md)
* [MapReduce HDInsight kanssa](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
