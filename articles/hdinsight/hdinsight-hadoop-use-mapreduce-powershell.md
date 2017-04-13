<properties
   pageTitle="MapReduce ja PowerShellin käyttäminen Hadoop | Microsoft Azure"
   description="Opettele PowerShellin avulla voit suorittaa etäyhteyden HDInsight MapReduce työt Hadoop kanssa."
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
   ms.date="08/29/2016"
   ms.author="larryfr"/>

# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>Suorittaa MapReduce työt kanssa Hadoop HDInsight PowerShellin avulla

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Tässä tiedostossa on esimerkiksi Azure PowerShellin avulla voit suorittaa MapReduce työn Hadoop HDInsight-klusterissa.

##<a id="prereq"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

- **Azure Hdinsightiin (Hadoop-HDInsight)-klusterin (Windows- tai Linux-pohjainen)**

- **Työasema Azure PowerShellin avulla**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a id="powershell"></a>Suorita MapReduce työn PowerShellin Azure

Azure PowerShell tarjoaa *cmdlet-komennot* , jotta voit suorittaa etäyhteyden MapReduce työt Hdinsightista. Tämä tehdään sisäisesti, käyttämällä muiden puhelut [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (entinen Templeton) HDInsight-klusterin käytössä.

Seuraavat cmdlet-komennot käytetään, kun MapReduce töitä remote HDInsight-klusterin.

* **Kirjaudu sisään AzureRmAccount**: todentaa Azure PowerShellin Azure-tilaukseen

* **Uusi AzureRmHDInsightMapReduceJobDefinition**: Luo uuden *projektin määritelmä* määritetyn MapReduce tietojen avulla

* **Käynnistä AzureRmHDInsightJob**: lähettää työn määritelmän HDInsight, työ käynnistyy ja palauttaa *työn* objektia, jonka avulla voidaan työn tilan tarkistaminen

* **Odota AzureRmHDInsightJob**: tilan tarkistaminen projektin työn avulla. Se odottaa, kunnes työ on valmis tai odotusaika on ylitetty.

* **Hae AzureRmHDInsightJobOutput**: työn tulosteen hakemiseen käytetty

Seuraavat vaiheet näytetään, miten työn HDInsight-klusterin käyttää näiden cmdlet-komennot.

1. Seuraava koodi editorilla tallentaminen **mapreducejob.ps1**. Tilalle HDInsight-klusterin nimen **CLUSTERNAME** .

        #Specify the values
        $clusterName = "CLUSTERNAME"
                
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Login-AzureRmAccount
        }

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
            
        #Define the MapReduce job
        #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
        # -JarFile = the JAR containing the MapReduce application
        # -ClassName = the class of the application
        # -Arguments = The input file, and the output directory
        $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
            -ClassName "wordcount" `
            -Arguments `
                "wasbs:///example/data/gutenberg/davinci.txt", `
                "wasbs:///example/data/WordCountOutput"

        #Submit the job to the cluster
        Write-Host "Start the MapReduce job..." -ForegroundColor Green
        $wordCountJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $wordCountJobDefinition `
            -HttpCredential $creds

        #Wait for the job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $wordCountJob.JobId `
            -HttpCredential $creds
        # Download the output
        Get-AzureStorageBlobContent `
            -Blob 'example/data/WordCountOutput/part-r-00000' `
            -Container $container `
            -Destination output.txt `
            -Context $context
        # Print the output
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Avaa uusi **PowerShellin Azure** komentorivi-ikkuna. Muuta kansioiden **mapreducejob.ps1** -tiedoston sijainti ja valitse sitten Käytä seuraavaa komentoa komentosarjan suorittamista:

        .\mapreducejob.ps1
    
    Kun suoritat komentosarjan, sinua voidaan pyytää tarkistamiseen Azure-tilaukseen. Myös sinua pyydetään antamaan HDInsight-klusterin HTTPS/järjestelmänvalvojan tilin käyttäjänimi ja salasana.

3. Kun työ on valmis, näyttöön tulee tulosteen seuraavankaltaiselta:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Tämä tulos ilmaisee työn onnistui.

    > [AZURE.NOTE] Jos **ExitCode** on muu kuin 0, katso [vianmääritys](#troubleshooting).

    Tässä esimerkissä myös tallentaa ladatut tiedostot **output.txt** -tiedostoon, kun suoritat komentosarja-kansiossa.

###<a name="view-output"></a>Näytä tulos

Avaa **output.txt** tiedostoa tekstieditorissa näet sanat ja tuottamat työn arvo.

> [AZURE.NOTE] MapReduce työn tulostus-tiedostoja ei voi muuttaa. Niin tässä esimerkissä uudelleen, jos haluat muuttaa kohdetiedosto nimi.

##<a id="troubleshooting"></a>Vianmääritys

Jos mitään tietoja kun työ on valmis, virhe on tapahtunut käsiteltäessä. Voit tarkastella virheen tietoja työn Lisää seuraava komento **mapreducejob.ps1** tiedoston loppuun, tallenna se ja suorita se sitten uudelleen.

    # Print the output of the WordCount job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Tämä palauttaa tietoja, jotka on kirjoitettu STDERR palvelimessa suorittaessasi työn, ja se voi auttaa selvittää, miksi työn epäonnistui tuntemattomasta.

##<a id="summary"></a>Yhteenveto

Voit tarkastella, PowerShellin Azure antaa suorittaa MapReduce työt HDInsight-klusterin, työ-tilaa ja Nouda tulosteen helposti.

##<a id="nextsteps"></a>Seuraavat vaiheet

Yleisiä tietoja MapReduce työt HDInsight:

* [MapReduce käyttäminen HDInsight Hadoop](hdinsight-use-mapreduce.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight:

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)
