<properties
    pageTitle="Korjata ja analysoida keon Vedostaa Hadoop-palvelun | Microsoft Azure"
    description="Automaattisesti kerätä keon Vedostaa Hadoop-palvelujen ja sijoittaa Azure Blob storage tilin virheenkorjaus ja analysointia varten."
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
    ms.date="10/19/2016"
    ms.author="jgao"/>


# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Kerää keon kirjoittaa Blob-objektien tallennustilaan virheenkorjaus ja analysoida Hadoop-palvelut

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Keon Vedostaa sisältää tilannevedoksen sovelluksen muistin muuttujan arvot mukaan lukien vedostaminen on luotu aikaan. Jotta ne ovat hyötyä ongelmien, jotka ilmenevät suorituksen aikana. Keon Vedostaa voidaan automaattisesti kerääminen Hadoop-palvelujen ja sijoittaa käyttäjän, valitse HDInsightHeapDumps Azure-Blob storage tilin sisään /. 

Keon Vedostaa eri palveluihin sivustokokoelman on otettava käyttöön yksittäisiä klustereiden palveluille. Tämä ominaisuus oletusarvo on ei ole käytössä klusterin. Nämä keon Vedostaa voi olla suuri, niin on suositeltavaa seurannassa Blob storage tilin kohtaa, johon ne on tallennettu kun kokoelma on otettu käyttöön.

> [AZURE.NOTE] Tämän artikkelin tiedot koskevat vain Windows-pohjaisesta Hdinsightista. Lisätietoja Linux-pohjaiset Hdinsightista on artikkelissa [käyttöön keon Vedostaa Linux-pohjaiset HDInsight Hadoop-palveluille](hdinsight-hadoop-collect-debug-heap-dump-linux.md)

## <a name="eligible-services-for-heap-dumps"></a>Keon Vedostaa olevalla palvelut

Voit ottaa keon Vedostaa seuraavat palvelut:

*  **hcatalog** - tempelton
*  **rakenne** - hiveserver2, metastore, derbyserver
*  **mapreduce** - jobhistoryserver
*  **kuitenkaan** - resourcemanager, nodemanager, timelineserver
*  **hdfs** - datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Määritysten elementtejä, jotka keon Vedostaa ottaminen käyttöön

Voit ottaa käyttöön keon Vedostaa palvelun haluat määrittää oikea elementit palvelun, joka on määritetty **palvelun nimi**mukaan-osassa.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

**Palvelun nimi** -arvo voi olla yllä palveluja: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, tai namenode.

## <a name="enable-using-azure-powershell"></a>Ota käyttöön Azure PowerShellin avulla

Esimerkiksi jos haluat ottaa keon Vedostaa käyttämällä PowerShellin Azure jobhistoryserver, sinun seuraavasti:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Käyttämällä .NET SDK ottaminen käyttöön

Esimerkiksi ottaminen keon Vedostaa käyttämällä Azure Hdinsightiin .NET SDK jobhistoryserver, sinun seuraavasti:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
