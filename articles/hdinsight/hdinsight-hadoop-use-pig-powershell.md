<properties
   pageTitle="Käytä Hadoop Possu HDInsight-PowerShellin avulla | Microsoft Azure"
   description="Lue, miten voit lähettää Hadoop-klusterin PowerShellin Azure HDInsight-Possu työt."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-powershell"></a>Possu töiden PowerShellin avulla

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Tässä tiedostossa on esimerkki Azure PowerShellin avulla voit lähettää Hadoop HDInsight-klusterissa Possu työt. Possu voit kirjoittaa MapReduce työt käyttämällä kielellä (Possu latinalainen), jota malleja tietojen muunnokset sijaan yhdistää ja vähentää funktiot.

> [AZURE.NOTE] Tässä asiakirjassa ei tarjoa esimerkeissä käytetään Possu latinalainen lauseet mitä yksityiskohtainen kuvaus. Tässä esimerkissä käytetään Possu latinalainen tietoja on artikkelissa [Käytön Possu Hadoop-HDInsight](hdinsight-use-pig.md).

##<a id="prereq"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavasti.

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Työasema Azure PowerShellin avulla**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a id="powershell"></a>Possu töiden PowerShellin avulla

Azure PowerShell tarjoaa *cmdlet-komennot* , jotta voit suorittaa etäyhteyden Possu työt Hdinsightista. Tämä tehdään sisäisesti, käyttämällä muiden puhelut [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (entinen Templeton) HDInsight-klusterin käytössä.

Seuraavat cmdlet-komennot, joita käytetään, kun töitä Possu remote HDInsight-klusterin:

* **Kirjaudu sisään AzureRmAccount**: todentaa Azure PowerShellin Azure-tilaukseen

* **Uusi AzureRmHDInsightPigJobDefinition**: Luo uuden *projektin määritelmä* käyttämällä määritettyä Possu latinalainen-lauseita

* **Käynnistä AzureRmHDInsightJob**: lähettää työn määritelmän HDInsight, työ käynnistyy ja palauttaa *työn* objektia, jonka avulla voidaan työn tilan tarkistaminen

* **Odota AzureRmHDInsightJob**: tilan tarkistaminen projektin työn avulla. Se odottaa, kunnes työ on valmis, tai odotusaika on ylitetty.

* **Hae AzureRmHDInsightJobOutput**: työn tulosteen hakemiseen käytetty

Seuraavat vaiheet näytetään, miten työn HDInsight-klusterissa käyttää näiden cmdlet-komennot.

1. Seuraava koodi editorilla tallentaminen **pigjob.ps1**. Tilalle HDInsight-klusterin nimen **CLUSTERNAME** .

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds = Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
        $container = $clusterInfo.DefaultStorageContainer
        $storageAccountKey = (Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasbs:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"


        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. Avaa uusi Windows PowerShell-komentokehote. Muuta kansioiden **pigjob.ps1** -tiedoston sijainti ja valitse sitten Käytä seuraavaa komentoa komentosarjan suorittamista:

        .\pigjob.ps1
        
    Ensin sinun pyydetään Azure-tilaukseen kirjautuminen. Valitse sinua pyydetään HTTPs/järjestelmänvalvojan tilin nimi ja salasana HDInsight-klusterin.

7. Kun työ on valmis, sen tulee palauttaa tiedot seuraavankaltaiselta:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="troubleshooting"></a>Vianmääritys

Jos mitään tietoja kun työ on valmis, virhe on tapahtunut käsiteltäessä. Voit tarkastella virheen tietoja työn Lisää seuraava komento **pigjob.ps1** tiedoston loppuun, tallenna se ja suorita se sitten uudelleen.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Tämä voi palauttaa tietoja, jotka on kirjoitettu STDERR palvelimessa suorittaessasi työn ja se voi auttaa selvittää, miksi työn epäonnistui tuntemattomasta.

##<a id="summary"></a>Yhteenveto

Voit tarkastella, PowerShellin Azure antaa suorittaa Possu työt HDInsight-klusterin, työ-tilaa ja Nouda tulosteen helposti.

##<a id="nextsteps"></a>Seuraavat vaiheet

Yleisiä tietoja HDInsight Possu:

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight:

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

* [Hadoop-HDInsight MapReduce käyttäminen](hdinsight-use-mapreduce.md)
