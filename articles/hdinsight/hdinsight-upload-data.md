<properties
    pageTitle="Lataa tiedot HDInsight Hadoop projekteille | Microsoft Azure"
    description="Lue, miten voit ladata ja käyttää tietoja Hadoop projekteille Azure-CLI sekä Azure-tallennustilan Explorer, PowerShellin Azure, Hadoop-komentoa ja Sqoop Hdinsightista."
    services="hdinsight,storage"
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
    ms.date="08/10/2016"
    ms.author="jgao"/>



#<a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Lataa tiedot HDInsight Hadoop projekteille

Azure Hdinsightiin sisältää täydet toiminnot sisältävien ohjelmien Hadoop distributed tiedostojärjestelmän (HDFS) Azure-Blob-säiliö nähden. Se on suunniteltu HDFS-tiedostotunnistetta sujuvan kokemuksen tarjota asiakkaille. Ottaa käyttöön täydellisen luettelon osia Hadoop ekosysteemissä käsittelet hallitsee tiedot. Azure-Blob-säiliö ja HDFS ovat eri tiedosto, joka on optimoitu tallennustilan tietoja ja funktiolauseita tietoja. Lisätietoja Azure-Blob-säiliö käytön etuja on [käyttäminen Azure-Blob-säiliö HDInsight kanssa][hdinsight-storage].

**Edellytykset**

Huomaa seuraavat vaatimukset, ennen kuin aloitat:

* Azure HDInsight-klusterin. Ohjeita on artikkelissa [Azure Hdinsightiin käytön aloittaminen] [ hdinsight-get-started] tai [säännöstä HDInsight klustereiden][hdinsight-provision].

##<a name="why-blob-storage"></a>Miksi blob storage?

Azure Hdinsightiin klustereiden yleensä käyttöön MapReduce töiden ja varausyksiköiden olevat kohteet poistetaan, nämä työt jälkeen. Säilyttämällä tietoja HDFS klustereiden funktiolauseita määrittämisen jälkeen olisi kallista tapa tallentaa tiedot. Azure-Blob-säiliö on käytettävissä, erittäin skaalattava, suuri kapasiteetti, tiedot, jotka ovat käsitellään HDInsight vähäisen ja jaettavissa tallennuspaikka. Tiedot tallennetaan blob mahdollistaa HDInsight-klustereiden, joita käytetään laskenta turvallisesti julkaistaan tietoja menettämättä.

###<a name="directories"></a>Hakemistojen selaaminen

Azure Blob storage säilöjen tallentaa tiedot avain ja arvon tietoparin ja ei ole hakemisto-hierarkia. Kuitenkin "/"-merkki voidaan sisällä avaimen nimi aluetta, että tiedosto on tallennettuna kansiorakenne. HDInsight näkee ne kuin, jos ne ovat todellinen kansioita.

Blob-objektien avain voi olla esimerkiksi *input/log1.txt*. Todellinen "syötteen" hakemistoa olemassa, mutta vuoksi tavoitettavuustietojen avaimen nimi "/" merkin, se on tiedostopolun ulkoasua.

Tästä syystä Jos käytät Azure Explorerin Työkalut voi olla 0 DBCS-tiedostoja. Nämä tiedostot käytetään kahta tarkoituksiin:

- Jos yhteystietokansioita on tyhjä, ne merkitä kansio olemassaolosta. Azure-Blob-säiliö on kekseliäs tietävän, että tapahtuman nimenä-palkin Blob-objektien olemassa, onko **tapahtuman nimenä**-nimiseen kansioon. Mutta että määräten 0 tavua tiedoston sijainti on ainoa tapa osoittavat tyhjän **tapahtuman nimenä** -kansion.

- Heillä on erityisen metatietojen Hadoop-tiedostojärjestelmässä, erityisesti käyttöoikeudet ja omistajat kansioiden tarvitsemat.

##<a name="command-line-utilities"></a>Komentorivin apuohjelmat

Microsoft toimittaa seuraavat apuohjelmat Azure-Blob-säiliö käyttöä varten:

| Työkalu | Linux | OS X: SSÄ | Windows |
| ---- |:-----:|:----:|:-------:|
| [Azure käyttöliittymä][azurecli] | ✔ | ✔ | ✔ |
| [Azure PowerShell][azure-powershell] | | | ✔ |
| [AzCopy][azure-azcopy] | | | ✔ |
| [Hadoop-komento](#commandline) | ✔ | ✔ | ✔ |

> [AZURE.NOTE] Kun Azure CLI, PowerShellin Azure ja AzCopy voivat kaikki voi käyttää ulkopuolelta Azure, Hadoop-komento on käytettävissä HDInsight-klusterin vain ja sallii vain tiedot ladataan Azure-Blob-säiliö paikallisessa tiedostojärjestelmässä.

###<a id="xplatcli"></a>Azure CLI

Azure-CLI on Office kaikissa ympäristöissä työkalu, jonka avulla voit hallita Azure palveluita. Tietojen lataaminen Azure-Blob-säiliö noudattamalla seuraavia ohjeita:

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Asentaminen ja määrittäminen Mac-, Linux-ja Windows Azure-CLI](../xplat-cli-install.md).

2. Avaa komentokehote, bash tai muihin käyttöliittymän ja käyttämällä seuraavaa tarkistamiseen Azure-tilaukseen.

        azure login

    Kun sinulta kysytään, kirjoita tilauksen käyttäjänimi ja salasana.

3. Kirjoita seuraava komento tilauksen tallennustilan-tilien luettelo:

        azure storage account list

4. Valitse tallennustilan-tili, joka sisältää haluat käsitellä Blob-objektien ja seuraavalla komennolla voit noutaa tilin:

        azure storage account keys list <storage-account-name>

    Tämä palautetaan ** **Perus** -ja** . Kopioi **perusavaimen** arvo, koska sitä käytetään seuraavia vaiheita.

5. Käytä seuraavaa komentoa ja Hae luettelo blob storage tilin säilöjä:

        azure storage container list -a <storage-account-name> -k <primary-key>

6. Lataa ja ladata tiedostoja blob käyttää seuraavia komentoja:

    * Lataa tiedosto:

            azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>

    * Voit ladata tiedoston seuraavasti:

            azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Jos olet työskentelee aina sama tallennustilan-tilillä, voit määrittää sen sijaan, että tili, joka määrittää seuraavat ympäristömuuttujat ja näppäintä jokaisen komennon:
>
> * **AZURE\_TALLENNUSTILAN\_tilin**: tallennustilan tilin nimi
>
> * **AZURE\_TALLENNUSTILAN\_ACCESS\_AVAIMEN**: tallennustilan tilin avain

###<a id="powershell"></a>Azure PowerShell

Azure PowerShell on komentosarjat ympäristössä, joiden avulla voit automatisoida käyttöönotto- ja Azure oman työmääriä hallinnointi ja hallita. Lisätietoja PowerShellin Azure suorittamaan työaseman on [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Paikallinen tiedosto ladataan Azure-Blob-säiliö**

1. Avaa PowerShellin Azure-konsolin, kuten [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).
2. Määritä seuraavaa komentosarjaa ensimmäiset viisi muuttujien arvoihin:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext

3. Komentosarjan liittäminen suorittaa tiedoston PowerShellin Azure-konsolin.

Esimerkiksi PowerShell-komentosarjojen luotu HDInsight-käyttöä varten katso [HDInsight-Työkalut](https://github.com/blackmist/hdinsight-tools).

###<a id="azcopy"></a>AzCopy

AzCopy on komentorivin työkalua, joka on suunniteltu yksinkertaistaa sisään ja ulos Azure-tallennustilan tilin tietojen siirtäminen. Voit käyttää sitä erillinen työkalun tai lisääminen sovellukseen tämä työkalu. [Lataa AzCopy][azure-azcopy-download].

AzCopy syntaksi on seuraava:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Lisätietoja on artikkelissa [AzCopy - Azure-BLOB-tiedostot ladataan/lataaminen][azure-azcopy].


###<a id="commandline"></a>Hadoop komentoriviltä

Hadoop-komentorivin on hyödyllinen vain projektiin tietojen Blob-objektien tallennustilaan, kun tiedot on jo klusterin pään solmu.

Jotta voit käyttää Hadoop-komentoa, sinun on ensin muodostettava yhteys headnode jollakin seuraavista tavoista:

* **Windows-pohjaisesta HDInsight**: [Yhdistä Etätyöpöydän käyttäminen](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)

* **Linux-pohjaiset HDInsight**: muodostaa yhteyden SSH ([SSH-komentoa](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster) tai [painovärit, muste](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster))

Kun yhteys on muodostettu, voit käyttää seuraavaa syntaksia Lataa tiedosto säilöön.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Esimerkiksi`hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Koska oletusarvon tiedostojärjestelmän Hdinsightista on Azure-Blob-säiliö, /example/data.txt on todella Azure-Blob-objektien tallennustilaan. Voit myös katsoa tiedosto:

    wasbs:///example/data/data.txt

tai

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Muut Hadoop-komennot, jotka tiedostoja luettelo on artikkelissa [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [AZURE.WARNING] Valitse HBase varausyksiköiden oletusarvo estä koon, jota käytetään, kun tiedot on 256 kt. Kun tämä toimii moitteettomasti käytettäessä HBase API tai REST API, avulla `hadoop` tai `hdfs dfs` komentoja, joilla voit kirjoittaa tiedot noin 12 megatavun aiheuttaa virheen. On lisätietoja alla [Kirjoitus-Blob-objektien tallennustilaan poikkeus](#storageexception) -osassa.

##<a name="graphical-clients"></a>Graafisen asiakkaat

Saatavilla on myös useita sovelluksia, jotka tarjoavat graafisessa käyttöliittymässä käsittelyyn Azure-tallennustilan. Seuraavassa on muutamia näiden sovellusten luettelo:

| Asiakkaan | Linux | OS X: SSÄ | Windows |
| ------ |:-----:|:----:|:-------:|
| [Microsoft Visual Studio Tools for Hdinsightiin](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) | ✔ | ✔ | ✔ |
| [Azure-tallennustilan Explorer](http://storageexplorer.com/) | ✔ | ✔ | ✔ |
| [Cloud tallennustilan Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | | ✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | | ✔ |
| [Azure Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | | ✔ |
| [Cyberduck](https://cyberduck.io/) |  | ✔ | ✔ |

###<a name="visual-studio-tools-for-hdinsight"></a>Visual Studio Tools for Hdinsightiin

Lisätietoja on artikkelissa [Siirtyminen linkitettyjen resurssit](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

###<a id="storageexplorer"></a>Azure-tallennustilan Explorer

*Azure-tallennustilan Explorer* on hyödyllinen työkalu tarkistetaan ja BLOB tietojen muuttamista. On ilmainen, Avaa lähde-työkalua, joka voi ladata [http://storageexplorer.com/](http://storageexplorer.com/). Lähdekoodin on saatavana tätä linkkiä.

Ennen työkalun avulla, sinun on tiedettävä Azure-tallennustilan tilin nimi ja tilin avainta. Ohjeet hakeminen nämä tiedot "Toimintaohje: näkymässä, kopioi ja muodosta uudelleen sarjanumerot tallennustilan valintanäppäimet" osa [luominen, hallinta- tai poistaa tallennustilan tilin][azure-create-storage-account].  

1. Suorita Azure-tallennustilan hallinta. Jos tämä on ensimmäinen kerta, kun sinulla on suoritettiin tallennustilan Resurssienhallinta, sinua pyydetään ___tallennustilan tilin nimi__ ja __tallennustilaa tilin avain__. Jos sinulla on suorittanut se ennen käyttöä __Lisää__ -painiketta Lisää uusi tallennustilan tilin nimi ja avaimen avulla.

    Kirjoita nimi ja avain tallennustilan tilin käyttämän HDinsight-klusterin ja valitse sitten __Tallenna ja AVAA__.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]

5. Valitse vasemmalla puolella liittymän säilöjä-luettelosta säilö, joka on liitetty HDInsight-klusterin nimi. Oletusarvoisesti tämä on HDInsight-klusterin nimen, mutta saattavat olla erilaiset, jos olet määrittänyt tietyn nimi klusterin luotaessa.

6. Valitse työkalurivillä latauksen kuvake.

    ![Työkalurivin Lataa-kuvake korostettuna](./media/hdinsight-upload-data/toolbar.png)

7. Määritä ladattava tiedosto ja valitse sitten **Avaa**. Kun sinulta kysytään, valitse __Lataa__ tiedosto ladataan tallennustilan säiliön ylimmällä. Jos haluat ladata tiedoston tiettyyn kansioon, kirjoita polku __kohdekentän__ ja valitse sitten __Lataa__.

    ![Tiedoston lataaminen-valintaikkuna](./media/hdinsight-upload-data/fileupload.png)
    
    Kun tiedosto on valmis lataamisesta, voit käyttää projektien HDInsight-klusterin.

##<a name="mount-azure-blob-storage-as-local-drive"></a>Ota Azure-Blob-säiliö kuin paikalliseen asemaan

Katso [Ota Azure-Blob-säiliö kuin paikalliseen asemaan](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

##<a name="services"></a>Palvelut

###<a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory-palvelu on täysin hallitun palvelu laadinta tallennustilan ja tietojen käsittely siirto tietopalvelujen virtaviivaistettu, skaalattava ja luotettavat tiedot tuotannon putkistot kyselyjä.

Azure Data Factory voidaan, siirry Azure-Blob-säiliö tietoja tai tietoja putkistot, jotka käyttävät suoraan HDInsight-ominaisuuksia, kuten rakenne ja Possu.

Katso lisätietoja [Azure Data Factory ohjeissa](https://azure.microsoft.com/documentation/services/data-factory/).

###<a id="sqoop"></a>Apache Sqoop

Sqoop on suunniteltu tietojen siirtäminen Hadoop ja relaatiotietokannasta välillä. Voit käyttää sitä tietojen tuominen relaatiotietokannasta hallintajärjestelmän (RDBMS), kuten SQL Server tai Oracle Hadoop distributed-tiedosto (HDFS)-järjestelmään MySQL Muunna Hadoop MapReduce tai rakenteen tiedot ja vie tiedot sitten takaisin RDBMS.

Saat lisätietoja, [Käytä Sqoop kanssa HDInsight][hdinsight-use-sqoop].

##<a name="development-sdks"></a>Kehittäminen SDK: T

Azure-Blob-säiliö voi käyttää myös käyttämällä Azure-SDK-ohjelmoinnin seuraavilla kielillä:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Katso lisätietoja Azure SDK: T asentamisesta [Azure-lataukset](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Vianmääritys

### <a id="storageexception"></a>Tallennustilan poikkeus-Blob-objektien kirjoittamista varten

__Ongelmia__: käytettäessä `hadoop` tai `hdfs dfs` tiedostoja, jotka ovat ~ 12 Gigatavua kirjoitus-komentojen tai suurempi HBase-klusterissa viestejä seuraavan virheilmoituksen: 

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

__Syy__: HBase HDInsight-klusterit oletusarvon 256 kt estä koosta, Azure tallennustilan kirjoitettaessa. Tämä koskee HBase API tai REST API, kun se aiheuttaa virheen käytettäessä `hadoop` tai `hdfs dfs` komentorivin apuohjelmat.

__Ratkaisu__: Käytä `fs.azure.write.request.size` Määritä suuremmaksi lohko. Voit tehdä tämän käyttökertakustannuksia välein käyttämällä `-D` parametria. Seuraavassa on esimerkki käyttämällä parametrin `hadoop` komento:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Voit myös lisätä arvon `fs.azure.write.request.size` yleisesti Ambari käyttämällä. Seuraavat vaiheet voidaan muuttaa Ambari Web-Käyttöliittymän arvo:

1. Siirry selaimella yhteyttä klusterin Ambari Web-Käyttöliittymän. Tämä on https://CLUSTERNAME.azurehdinsight.net, missä __CLUSTERNAME__ yhteyttä klusterin nimen.

    Kun sinulta kysytään, kirjoita klusterin järjestelmänvalvojan käyttäjänimi ja salasana.

2. Näytön vasemmasta reunasta Valitse __HDFS__ja valitse sitten __kokoonpanomääritysten yhteydessä__ -välilehti.

3. Kirjoita __Suodata...__ -kenttään `fs.azure.write.request.size`. Tässä näkyvät kentän ja nykyisen arvon sivun keskellä.

4. Muuttaa arvon 262144 (256 kt) uusi arvo. Esimerkiksi 4194304 (4 Mt).

![Kuva Ambari Web-Käyttöliittymän välityksellä arvon muuttaminen](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Lisätietoja Ambari on artikkelissa [Hallitse HDInsight klustereiden Ambari Web-Käyttöliittymän avulla](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun osaat saat tietoja HDInsight mukaan, lue lisätietoja analysoinnissa on seuraavissa artikkeleissa:

* [Azure Hdinsightiin käytön aloittaminen][hdinsight-get-started]
* [Lähettää Hadoop työt ohjelmallisesti][hdinsight-submit-jobs]
* [Rakenteen käyttäminen Hdinsightiin][hdinsight-use-hive]
* [Possu käyttäminen Hdinsightiin][hdinsight-use-pig]




[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: ../powershell-install-configure.md

[azurecli]: ../xplat-cli-install.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
