<properties
   pageTitle="Kopioi tiedot ja sieltä pois WASB Lake tietovaraston käyttämällä Distcp | Microsoft Azure"
   description="Kopioi tiedot ja tallennustilaa Azure-BLOB sieltä järvi tietovaraston Distcp-työkalun avulla"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Tietoja tallennustilan Azure-BLOB- ja järvi tietovaraston välillä Distcp avulla

> [AZURE.SELECTOR]
- [DistCp käyttäminen](data-lake-store-copy-data-wasb-distcp.md)
- [AdlCopy käyttäminen](data-lake-store-copy-data-azure-storage-blob.md)


Kun olet luonut HDInsight-klusterin, jolla on pääsy järvi tietosäilö-tili, voit kopioida tiedot **ja** HDInsight klusterin storage (WASB) järvi tietovaraston tilille käyttämällä Hadoop ekosysteemiin työkaluja, kuten Distcp. Tässä artikkelissa on ohjeita siitä, miten voit saavuttaa.

##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).
- **Azure-tilauksen käyttöön** järvi tietovaraston public preview-version. Katso [ohjeet](data-lake-store-get-started-portal.md#signup).
- **Azure HDInsight-klusterin** järvi tietovaraston tilin käyttöoikeus. Lisätietoja on kohdassa [Create HDInsight-klusterin tietojen järvi kaupan](data-lake-store-hdinsight-hadoop-use-portal.md). Varmista, että otat Etätyöpöydän klusterin.

## <a name="do-you-learn-fast-with-videos"></a>Opit nopeasti ja ladattua?

[Tässä videossa](https://mix.office.com/watch/1liuojvdx6sie) Azure tallennustilan BLOB-objektit ja järvi tietovaraston tietojen kopioiminen DistCp avulla.

## <a name="use-distcp-from-remote-desktop-windows-cluster-or-ssh-linux-cluster"></a>Käytä Distcp Etätyöpöytä (Windows-klusterin) tai SSH (Linux-klusterin)

HDInsight-klusterin sisältyy Distcp-apuohjelma, jonka avulla voidaan kopioida tietoja useista lähteistä HDInsight-klusterin. Jos olet määrittänyt HDInsight-klusterin käytettävä järvi tietovaraston Lisää tallennustilaa, Distcp-apuohjelma voi käyttää ulos,-valmiilla haluat kopioida tiedot, ja järvi tietosäilö-tililtä. Tässä osassa on Tarkista Distcp-apuohjelman käyttämisestä.

1. Jos sinulla on Windows-klusterin, remote HDInsight-klusterin kyselyjä, joka sisältää järvi tietovaraston tilin käyttöoikeus. Ohjeita on artikkelissa [etäyhteyden muodostaminen käyttämällä RDP klustereiden](../hdinsight/hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp). Avaa klusterin työpöydän Hadoop-komentoriviltä.

    Jos sinulla on Linux-klusterin, muodostaa yhteyttä klusterin SSH avulla. Artikkelissa [etäyhteyden muodostaminen Linux-pohjaiset HDInsight-klusterin](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster). Komentojen pitäminen SSH kehotteen.

3. Tarkista, onko sinulla on pääsy Azure-tallennustilan BLOB-objektit (WASB). Suorita seuraava komento:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Tämä on tarjottava storage blob sisällysluettelo.

4. Tarkista samalla tavoin, voit käyttää klusterin järvi tietosäilö-tili. Suorita seuraava komento:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    Tämä on tarjottava tiedostot tai kansiot järvi tietovaraston tilin luettelo.

5. Tietojen kopioiminen WASB Lake tietovaraston tilin Distcp avulla.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Tämä **kopioi/Esimerkki/tietojen/gutenberg /** -kansion sisältö-WASB **/myfolder** Lake tietosäilö-tilin avulla.

6. Vastaavasti tietojen kopioiminen järvi tietovaraston tilin WASB Distcp avulla.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Tämä kopioi **/myfolder** sisällön järvi tietovaraston tilin **WASB/Esimerkki/tietojen/gutenberg /** -kansioon.

## <a name="see-also"></a>Katso myös

- [Tietojen kopioiminen Azure-tallennustilan BLOB järvi tietosäilö](data-lake-store-copy-data-azure-storage-blob.md)
- [Tietojen suojaaminen järvi säilössä](data-lake-store-secure-data.md)
- [Azure tietojen järvi Analytics käyttäminen järvi tietosäilö](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure Hdinsightiin käyttäminen järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)
