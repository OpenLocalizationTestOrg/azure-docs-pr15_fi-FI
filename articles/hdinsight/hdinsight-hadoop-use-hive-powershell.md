<properties
   pageTitle="Hadoop rakenteen käyttäminen PowerShell HDInsight | Microsoft Azure"
   description="PowerShellin avulla voit suorittaa rakenteen kyselyjen Hadoop Hdinsightista."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-powershell"></a>Rakenne-kyselyt PowerShellin avulla

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Tässä tiedostossa on esimerkki käytöstä PowerShellin Azure toimimaan rakenteen kyselyjen Hadoop HDInsight-klusterissa Azure resurssiryhmä-tilassa.

> [AZURE.NOTE] Tässä asiakirjassa ei tarjoa HiveQL lausekkeita, jotka esimerkeissä käytetään mitä yksityiskohtainen kuvaus. Lisätietoja HiveQL, jota käytetään tässä esimerkissä on artikkelissa [Käytä rakenne ja Hadoop-HDInsight](hdinsight-use-hive.md).


**Edellytykset**

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavasti.

- **Azure Hdinsightiin (Hadoop-HDInsight)-klusterin (Windows- tai Linux-pohjainen)**
- **Työasema Azure PowerShellin avulla**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="run-hive-queries-using-azure-powershell"></a>Suorita PowerShellin Azure kyselyjen rakenne

Azure PowerShell tarjoaa *cmdlet-komennot* , jotta voit suorittaa etäyhteyden rakenteen kyselyjen Hdinsightista. Tämä tehdään sisäisesti, käyttämällä muiden puhelut [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (entinen Templeton) HDInsight-klusterin käytössä.

Seuraavat Cmdlet-komentoja voi käyttää suoritettaessa rakenteen kyselyjen remote HDInsight-klusterin:

* **Lisää AzureRmAccount**: todentaa Azure PowerShellin Azure-tilaukseen

* **Uusi AzureRmHDInsightHiveJobDefinition**: Luo uuden *projektin määritelmä* avulla määritetyn HiveQL lauseet

* **Käynnistä AzureRmHDInsightJob**: lähettää työn määritelmän HDInsight, työ käynnistyy ja palauttaa *työn* objektia, jonka avulla voidaan työn tilan tarkistaminen

* **Odota AzureRmHDInsightJob**: tilan tarkistaminen projektin työn avulla. Se odottaa, kunnes työ on valmis tai odotusaika on ylitetty.

* **Hae AzureRmHDInsightJobOutput**: työn tulosteen hakemiseen käytetty

* **Käynnistä AzureRmHDInsightHiveJob**: HiveQL lauseet käyttöä varten. Tämä estää kysely on valmis, ja palauttaa tulokset

* **Käytä AzureRmHDInsightCluster**: määrittää nykyiseen klusteria **Käynnistä AzureRmHDInsightHiveJob** -komennon

Seuraavat vaiheet näytetään, miten työn HDInsight-klusterin käyttää näiden cmdlet-komennot:

1. Seuraava koodi editorilla tallentaminen **hivejob.ps1**. Tilalle HDInsight-klusterin nimen **CLUSTERNAME** .

        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential

        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green

        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Avaa uusi **PowerShellin Azure** komentorivi-ikkuna. Muuta kansioiden **hivejob.ps1** -tiedoston sijainti ja valitse sitten Käytä seuraavaa komentoa komentosarjan suorittamista:

        .\hivejob.ps1

    Kun komentosarja on suoritettu, voit pyydetään antamaan yhteyttä klusterin HTTPS/järjestelmänvalvojan tilin tunnistetiedot. Sinua voidaan pyytää myös Azure-tilaukseen kirjautuminen.
    
7. Kun työ on valmis, sen tulee palauttaa tiedot seuraavankaltaiselta:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Kuten edellä mainittiin, **Käynnistä rakenne** voidaan suorittaa kyselyn ja odottaa vastausta. Käytä seuraavia komentoja ja korvaa **CLUSTERNAME** yhteyttä klusterin nimen:

        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString

    Tulos näyttää seuraavalta:

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] Pidempi HiveQL kyselyjen voit PowerShellin Azure **Tähän merkkijonot** cmdlet-komento tai HiveQL komentosarjatiedostoja. Seuraavat koodikatkelman esitetään, kuinka voit suorittaa HiveQL komentosarjatiedosto **Käynnistä rakenne** -cmdlet-komennolla. HiveQL-komentosarjatiedosto on ladattava wasbs: / /.
    >
    > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > Katso lisätietoja **Tästä merkkijonoja** <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Käyttämällä Windows PowerShellin tähän-merkkijonoja</a>.

##<a name="troubleshooting"></a>Vianmääritys

Jos mitään tietoja kun työ on valmis, virhe on tapahtunut käsiteltäessä. Voit tarkastella virheen tietoja työn Lisää seuraava teksti **hivejob.ps1** tiedoston loppuun, tallenna se ja suorita sitten uudelleen.

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Tämä palauttaa tietoja, jotka on kirjoitettu STDERR palvelimessa suorittaessasi työn, ja se voi auttaa selvittää, miksi työn epäonnistui tuntemattomasta.

##<a name="summary"></a>Yhteenveto

Voit tarkastella, PowerShellin Azure antaa rakenteen kyselyjen suorittaminen HDInsight-klusterin, työ-tilaa ja Nouda tulosteen helposti.

##<a name="next-steps"></a>Seuraavat vaiheet

Yleisiä tietoja HDInsight-rakenne:

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight:

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)

* [Hadoop-HDInsight MapReduce käyttäminen](hdinsight-use-mapreduce.md)
