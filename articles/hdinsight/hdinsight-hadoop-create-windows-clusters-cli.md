<properties
   pageTitle="Windows-pohjaisesta Hadoop klustereiden luominen käyttämällä Azure CLI Hdinsightiin"
    description="Opettele luomaan klustereiden käyttämällä Azure CLI Azure Hdinsightiin varten."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>Windows-pohjaisesta Hadoop klustereiden luominen käyttämällä Azure CLI Hdinsightiin

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Opettele luomaan käyttämällä Azure CLI Hdinsightiin klustereiden. Klusterin muiden luontia varten työkaluja ja ominaisuuksia välilehti Valitse sivun ylälaidassa tai saat [klusterin luominen](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Edellytykset:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Ennen kuin aloitat tämän artikkelin ohjeita, sinulla on oltava seuraavasti:

- **Azure tilauksen**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI**.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>Access-ohjausobjektin vaatimukset

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="connect-to-azure"></a>Yhteyden muodostaminen Azure

Käytä seuraavaa komentoa muodostaa Azure:

    azure login

Lisätietoja todennustapa työpaikan tai oppilaitoksen tilillä on artikkelissa [etäyhteyden muodostaminen Azure tilauksen Azure-CLI](../xplat-cli-connect.md).

Käytä seuraavaa komentoa ARM-tilaan siirtyminen:

    azure config mode arm

Jos tarvitset apua, **-h** valitsimella.  Esimerkki:

    azure hdinsight cluster create -h

##<a name="create-clusters"></a>Klustereiden luominen

Sinulla on oltava Azure resurssien hallinta (ARM) ja Azure Blob storage tilin, ennen kuin voit luoda HDInsight-klusterin. Voit luoda HDInsight-klusterin, sinun on määritettävä seuraavasti:

- **Azure resurssiryhmä**: A tietojen järvi Analytics-tili on luotava Azure resurssiryhmä kuluessa. Azure Resurssienhallinta mahdollistaa sovelluksen ryhmänä resurssien käsitteleminen. Voit ottaa käyttöön, Päivitä tai poista kaikki resurssit sovelluksen yhteen, koordinoidun toiminnassa.

    Tilauksen resurssiryhmien luetteloon:

        azure group list

    Luo uusi resurssiryhmä seuraavasti:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **HDInsight-klusterinimi**

- **Sijainti**: yksi, joka tukee HDInsight klustereiden Azure tietojen keskukset. Katso luettelo tuetuista sijainneista, [HDInsight hinnat](https://azure.microsoft.com/pricing/details/hdinsight/).

- **Tallennustilan oletustilin**: HDInsight käyttää Azure-Blob-tallennustilan säiliön käyttöjärjestelmän. Azure-tallennustilan tilin tarvitaan, ennen kuin voit luoda HDInsight-klusterin.

    Voit luoda uuden Azure-tallennustilan tilin seuraavasti:

        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE]Tallennustilan-tili on collocated HDInsight tietokeskuksen kanssa.
    > Tallennustilan tilin tyyppi ei voi olla ZRS, koska ZRS ei tue taulukossa.

    Lisätietoja Azure-tallennustilan tilin luominen käyttämällä Azure-portaalissa on artikkelissa [luominen, hallita tai poistaa tallennustilan tilin] [azure-luominen-storageaccount].

    Jos sinulla on jo tallennustilan tilin, mutta et tiedä tilin nimi ja tili-näppäintä, voit hakea tietoja seuraavista komennoista:

        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    Lisätietoja tietojen käytön mukaan Azure-portaalissa on "Tallennustilan tilin hallinnointi" [tietoja Azure](../storage/storage-create-storage-account#manage-your-storage-account)-tallennustilan tilien kohdassa.

- **(Valinnainen) oletusarvo-Blob-säilö**: **azure hdinsightiin klusterin luominen** -komento luo säilö, jos sitä ei ole. Jos haluat luoda säilön etukäteen, voit käyttää seuraava komento:

    Azure-tallennustilan säiliön luominen--tilin nimi "<Storage Account Name>"--tiliavain tiliavain <Storage Account Key> [ContainerName]

Kun olet valmistellut tallennustilan tilin, olet valmis luomaan klusterin:


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##<a name="create-clusters-using-configuration-files"></a>Luo klustereiden määritystiedostojen käyttäminen
Yleensä voit luoda HDInsight-klusterin työt suoritetaan ja poista klusterin Leikkaa alaspäin kustannukset. Komentorivivalitsimet-liittymän avulla voi tallentaa tiedostoon-määrityksiä niin, että voit käyttää sitä aina, kun luot klusterin.  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

Esimerkki: Luo määritystiedosto, joka sisältää klusterin luotaessa komentosarjoissa-toiminnon.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##<a name="create-clusters-with-script-action"></a>Luo klustereiden komentosarja-toiminnon kanssa

Luo määritystiedosto, joka sisältää klusterin luotaessa komentosarjoissa-toiminnon.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Klusterin luominen komentosarja-toiminnon avulla

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Katso Yleiset komentosarjan toiminnon tietoja, [mukauttaa HDInsight klustereiden komentosarja-toiminnolla (Linux)](hdinsight-hadoop-customize-cluster.md).


## <a name="create-clusters-using-arm-templates"></a>Luo klustereiden ARM-mallien käyttäminen

CLI avulla voit luoda klustereiden soittamalla ARM-mallit. Katso [Azure CLI käyttöönotto](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## <a name="see-also"></a>Katso myös

- [Azure Hdinsightiin käytön aloittaminen](hdinsight-hadoop-linux-tutorial-get-started.md) - ohjeita aloittamisesta HDInsight-klusterin käsitteleminen
- [Lähetä Hadoop työt ohjelmallisesti](hdinsight-submit-hadoop-jobs-programmatically.md) – Katso, miten voit lähettää ohjelmallisesti työt Hdinsightiin
- [Hallitse Hadoop varausyksiköt käyttämällä Azure CLI Hdinsightiin](hdinsight-administer-use-command-line.md)
- [Azure CLI käyttäminen Mac, Linux ja Windows Azure hallinnan kanssa](../virtual-machines-command-line-tools.md)
