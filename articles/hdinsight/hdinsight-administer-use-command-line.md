<properties
    pageTitle="Hallitse Hadoop klustereiden käyttämällä Azure CLI | Microsoft Azure"
    description="Opit käyttämään Azure-CLI Hadoop varausyksiköt HDIsight hallinta"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Hallitse Hadoop varausyksiköt käyttämällä Azure CLI Hdinsightiin

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Opettele [Azure käyttöliittymä](../xplat-cli-install.md) avulla voit hallita Hadoop varausyksiköt Azure Hdinsightista. Azure-CLI on toteutettu Node.js. Sen avulla voidaan ympäristössä, joka tukee Node.js, kuten Windows, Mac tai Linux.

Tässä artikkelissa käsitellään vain käyttäminen Azure CLI Hdinsightista. Lisätietoja ja Yleiset Azure CLI käyttämisestä on artikkelissa [asentaminen ja määrittäminen Azure CLI][azure-command-line-tools].

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI** – Katso [asentaminen ja määrittäminen Azure-CLI](../xplat-cli-install.md) asennus-ja määritystietoja.
- **Yhteyden muodostaminen Azure**-käyttämällä seuraava komento:

        azure login

    Lisätietoja todennustapa työpaikan tai oppilaitoksen tilillä on artikkelissa [etäyhteyden muodostaminen Azure tilauksen Azure-CLI](xplat-cli-connect.md).
    
- **Siirry Azure Resurssienhallinta-tilassa**käyttämällä seuraava komento:

        azure config mode arm

Jos tarvitset apua, **-h** valitsimella.  Esimerkki:

    azure hdinsight cluster create -h
    
##<a name="create-clusters"></a>Klustereiden luominen

Katso [luominen Linux-pohjaiset varausyksiköt käyttämällä Azure CLI Hdinsightista](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

##<a name="list-and-show-cluster-details"></a>Luettelo ja klusterin tietojen näyttäminen
Voit käyttää seuraavia komentoja luettelo ja klusterin tietojen näyttäminen:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI. CLIListCluster][image-cli-clusterlisting]


##<a name="delete-clusters"></a>Klustereiden poistaminen

Käytä seuraavaa komentoa klusterin poistaminen:

    azure hdinsight cluster delete <Cluster Name>

Voit myös poistaa klusterin poistamalla, joka sisältää klusterin resurssiryhmä. Huomaa, tämä toiminto poistaa kaikki resurssit ryhmästä, mukaan lukien tallennustilan oletustilin.

    azure group delete <Resource Group Name>

##<a name="scale-clusters"></a>Klustereiden asteikko

Voit muuttaa Hadoop klusteri seuraavasti:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>Ottaa käyttöön/poistaa käytöstä klusterin HTTP-käyttö

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Klusterin käyttöoikeuden RDP ottaminen käyttöön tai poistaminen käytöstä

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##<a name="next-steps"></a>Seuraavat vaiheet
Tässä artikkelissa olet oppinut eri HDInsight klusterin hallinnollisten tehtävien suorittamiseen. Lisätietoja on seuraavissa artikkeleissa:

* [Hallita HDInsight Azure-portaalissa] [hdinsight-admin-portal]
* [Hallita käyttämällä PowerShellin Azure Hdinsightiin] [hdinsight-admin-powershell]
* [Azure Hdinsightiin käytön aloittaminen] [hdinsight-get-started]
* [Azure-CLI käyttäminen] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "Luettelo- ja Näytä klustereiden"
