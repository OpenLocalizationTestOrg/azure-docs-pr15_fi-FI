<properties
    pageTitle="Käytä vuorovaikutteinen HDInsight-rakenne | Microsoft Azure"
    description="Opettele HDInsight käyttämään vuorovaikutteinen rakenne (rakenne-LLAP)."
    keywords=""
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
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="use-interactive-hive-in-hdinsight-preview"></a>Käytä vuorovaikutteinen rakenne HDInsight (ennakkoversio)

Vuorovaikutteinen (tietovälinettä rakenne [Live pitkään ja prosessi]( https://cwiki.apache.org/confluence/display/Hive/LLAP)) on uusi HDInsight- [klusterin tyyppi]( hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Vuorovaikutteinen rakenne sallii välimuistia, joka tekee rakenteen kyselyjen entistä nopeampi ja vuorovaikutteinen. Uuden toiminnon tekee HDInsight jokin seuraavista maailman useimmat performant, joustavia, ja avaa Big datasta ratkaisun kanssa ladatun pilveen välimuistiin (joko rakenne ja ohjattu) ja kehittyneitä analytics laaja integroinnin R-palvelujen kanssa. 

Vuorovaikutteinen rakenne-klusterin poikkeaa Hadoop-klusterin. Rakenne-palvelu sisältää vain. 

> [AZURE.NOTE] MapReduce, Possu, Sqoop, Oozie ja muut palvelut poistetaan klusterin tällaista pian.
Vuorovaikutteinen rakenne-klusterin rakenne-palvelu on ainoastaan Ambari rakenne-näkymä, Beeline ja rakenne ODBC kautta. Se ei voi käyttää rakenne-konsolin, Templeton, Azure CLI ja PowerShellin Azure kautta. 


 


## <a name="create-an-interactive-hive-cluster"></a>Vuorovaikutteinen rakenne-klusterin luominen

Vuorovaikutteinen rakenne-klusterin tuetaan vain Linux-pohjaiset klustereiden. Lisätietoja HDInsight klustereiden luomisesta on artikkelissa [Luo Linux-pohjaiset Hadoop varausyksiköt Hdinsightista](hdinsight-hadoop-provision-linux-clusters.md).


## <a name="execute-hive-from-interactive-hive"></a>Suorittaa rakenteen vuorovaikutteinen rakenne

On erilaisia asetuksia, miten voit suorittaa rakenteen kyselyitä:

- Suorita rakenteen Ambari rakenne-näkymässä

    Lisätietoja rakenne-näkymässä on artikkelissa [Rakenne-näkymä, jossa Hadoop HDInsight]( hdinsight-hadoop-use-hive-ambari-view.md).

- Suorita käyttämällä Beeline rakenne

    Valitse Beeline käyttäminen HDInsight Lisätietoja on artikkelissa [Käytä rakenne ja HDInsight kanssa Beeline Hadoop](hdinsight-hadoop-use-hive-beeline.md).

    Voit käyttää Beeline headnode tai tyhjä reuna-solmu.  On suositeltavaa käyttää Beeline tyhjä reuna-solmun.  Tietoja luomalla HDInsight-klusterin tyhjä edgenode on artikkelissa [Käytä tyhjä reunan solmujen Hdinsightista](hdinsight-apps-use-edge-node.md).

- Suorita käyttämällä rakenne ODBC rakenne

    Käytä rakenne ODBC lisätietoja [Connect Excel to Microsoft rakenne ODBC-ohjaimella Hadoop Excelin](hdinsight-connect-excel-hive-odbc-driver.md).

**Voit etsiä JDBC yhteysmerkkijonon seuraavasti:**

1.  Kirjaudu Ambari käyttämällä seuraavaa URL-Osoitetta: https://<ClusterName>. AzureHDInsight.net.
2.  Valitse vasemmasta valikosta **rakenne** .
3.  Valitse korostettu kuvakkeen URL-Osoitteen kopioiminen:

    ![HDInsight Hadoop vuorovaikutteinen rakenteen LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Katso myös
-   [Luo Linux-pohjaiset Hadoop varausyksiköt HDInsight](hdinsight-hadoop-provision-linux-clusters.md): luominen vuorovaikutteinen rakenne klustereiden Hdinsightista.
-   [Käytä rakenne ja HDInsight kanssa Beeline Hadoop](hdinsight-hadoop-use-hive-beeline.md): Opi käyttämään Beeline Lähetä kyselyt rakenne.
-   [Connect Excel to Microsoft rakenne ODBC-ohjaimella Hadoop Excelin](hdinsight-connect-excel-hive-odbc-driver.md): opettele muodostamaan Excelin rakenne.
