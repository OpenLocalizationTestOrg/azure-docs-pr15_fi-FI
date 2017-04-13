<properties
   pageTitle="Luo HDInsight klustereiden PowerShellin Azure tietojen järvi kaupan | Azure"
   description="Luominen ja HDInsight klustereiden käyttäminen Azure tietojen järvi Azure PowerShellin avulla"
   services="data-lake-store,hdinsight" 
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
   ms.date="10/21/2016"
   ms.author="nitinme"/>

# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-powershell"></a>Tietosäilö järvi PowerShellin Azure HDInsight-klusterin luominen

> [AZURE.SELECTOR]
- [Portaalissa](data-lake-store-hdinsight-hadoop-use-portal.md)
- [PowerShellin avulla](data-lake-store-hdinsight-hadoop-use-powershell.md)
- [Resurssien hallinnan avulla](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Opettele käyttämään PowerShellin Azure HDInsight-klusterin (Hadoop, HBase tai myrsky) Määritä Azure Lake Tietosäilölle käyttöönsä. Tässä versiossa joitakin tärkeitä asioita:

* **Saat ohjattu varausyksiköt (Linux) ja Hadoop/myrsky varausyksiköt (Windows ja Linux)**, järvi tietovaraston vain voidaan lisätallennustilaa-tilinä. Näiden klustereiden tallennustilan oletustili on Azure tallennustilan BLOB-objektit (WASB).

* **Saat HBase varausyksiköt (Windows ja Linux)**, järvi tietovaraston voidaan oletusarvon tallennustilan tai lisätallennustilaa.

> [AZURE.NOTE] Joitakin tärkeitä seikkoja huomautuksen. 
> 
> * Voit luoda HDInsight klustereiden Lake Tietosäilölle käyttöönsä on käytettävissä vain HDInsight-versioiden 3.2 ja 3.4 (for Hadoop, HBase ja myrsky klustereiden Windows sekä Linux). Ohjattu klustereiden Linux-asetus on käytettävissä vain HDInsight 3.4 klustereiden käyttöön.
>
> * Edellä mainittua järvi tietosäilö on käytettävissä oletusarvon tallennustila klusterin Jotkin tiedostotyypit (HBase) ja lisää tallennustilaa klusterin muuntyyppisten (Hadoop, ohjattu myrsky). Käyttämällä järvi tietovaraston lisätallennustilaa tiliksi ei vaikuta suorituskykyyn tai luku ja kirjoita tallennustilan klusterista mahdollisuus. Tilanteessa, jossa järvi tietovaraston käytetään tallennustilaa klusterin liittyvät tiedostot (kuten lokit, jne.) on kirjoitettu oletusarvon storage (Azure-BLOB), kun järvi tietovaraston tili voidaan tallentaa tiedot, joita haluat käsitellä.


Tässä artikkelissa on valmistella Hadoop-klusterin tietojen järvi kaupan kuin lisätallennustilaa.

Määrittäminen toimimaan järvi tietovaraston HDInsight PowerShellin etenee vaiheittain seuraavasti:

* Luo Azure tietojen järvi-säilö
* Tietosäilö järvi Roolipohjainen käyttöoikeuksien todentamisen määrittäminen
* Luo HDInsight-klusterin järvi tietosäilö-todennus
* Suorita testi työ klusterin

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

- **Azure PowerShell 1.0 tai suurempi**. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).

- **Windows SDK**. Voit asentaa sen [täältä](https://dev.windows.com/en-us/downloads). Tämän avulla Luo sertifikaatti.

- **Azure Active Directory-palvelun lyhennys**. Tämän opetusohjelman vaiheita on ohjeet luomisesta Azure AD-palvelun lyhennyksen. On kuitenkin olla Azure AD-järjestelmänvalvoja voi luoda palvelun lyhennyksen. Jos olet Azure AD-järjestelmänvalvoja, voit ohittaa tämän edellytyksenä ja jatka opetusohjelman.
    
    **Jos et ole Azure AD-järjestelmänvalvoja**, osaat ei tarvitse luoda palvelun lyhennys toimien. Siinä tapauksessa Azure AD-järjestelmänvalvojan on luotava palvelun lyhennys ennen kuin voit luoda HDInsight-klusterin järvi tietosäilö. Lisäksi palvelun lyhennys on luotava sertifikaatilla, kuvatulla tavalla, [Luo pääasiallista sertifikaatilla palvelu](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate). 


## <a name="create-an-azure-data-lake-store"></a>Luo Azure tietojen järvi-säilö

Luo järvi tietosäilö seuraavasti.

1. Avaa uuden ikkunan PowerShellin Azure työpöydän ja syötä seuraavat koodikatkelman. Kirjaudu sisään pyydettäessä Varmista, että kirjaudut yhtenä tilauksen admininistrators/omistaja:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    >[AZURE.NOTE] Jos näyttöön tulee virhesanoma `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` kun rekisteröinti tietojen järvi resurssin säilöpalvelu, on mahdollista, että subsrcription ei ole Azure järvi tietovaraston whitelisted. Varmista, että otat Azure tilauksen järvi tietovaraston julkisen esikatselua varten noudattamalla seuraavia [ohjeita](data-lake-store-get-started-portal.md#signup).

3. Azure järvi tietovaraston tili on liitetty Azure-resurssiryhmä. Aloita luomalla Azure-resurssiryhmä.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Azure resurssi-ryhmän luominen] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Azure resurssi-ryhmän luominen")

2. Luo tili Azure Lake Tietosäilölle. Voit määrittää tilin nimen on oltava vain pieniä kirjaimia ja numeroita.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Luo Azure tietojen järvi tili] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Luo Azure tietojen järvi tili")

3. Varmista, että tili on luotu.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Tämä tulos on oltava **Tosi**.

4. Lataa Azure tietojen järvi mallitietoja. Olemme käytetään tämä tämän artikkelin Tarkista, että tiedot ovat käytettävissä olevat HDInsight-klusterin. Jos etsit joitakin mallitietoja lataaminen, saat **Ambulanssi Data** -kansion [Azure tietojen Lake Git säilöön](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).


        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Tietosäilö järvi Roolipohjainen käyttöoikeuksien todentamisen määrittäminen

Jokaisen Azure tilaukseen on liitetty Azure Active Directory. Käyttäjien ja resurssien Azure perinteinen Portal tai Azure Resurssienhallinta API tilauksen käytön palveluita on ensin todentaa kyseisen Azure Active Directory-hakemistosta. Käytettävyys Azure tilaukset ja-palveluihin määrittämällä Azure resurssin asianmukainen rooli.  Services-palvelun lyhennys tunnistaa palvelun-Azure Active Directory (AAD). Tässä osassa kuvataan, miten voit myöntää sovelluksen-palvelun, kuten HDInsight-Azure resurssin (aiemmin luomasi Azure järvi tietosäilö-tili) käyttö luomalla service-lyhennys sovelluksen ja roolien, Azure PowerShellin kautta.

Määrittämään Azure tietojen järvi Active Directory-todennus on suoritettava seuraavat toimet.

* Itse allekirjoitetun varmenteen luominen
* Sovelluksen luominen ja Azure Active Directory-palvelun lyhennyksen.

### <a name="create-a-self-signed-certificate"></a>Itse allekirjoitetun varmenteen luominen

Varmista, että sinulla on [Windows SDK](https://dev.windows.com/en-us/downloads) -asennus, ennen kuin jatkat tämän osan vaiheet. On myös luomasi kansion, kuten **C:\mycertdir**, jossa varmenteen luodaan.

1. PowerShell-ikkunasta Etsi sijainti, johon on asennettu Windows SDK (yleensä `C:\Program Files (x86)\Windows Kits\10\bin\x86` ja käyttää [MakeCert] [ makecert] apuohjelma, voit luoda itse allekirjoitetun varmenteen ja yksityinen avain. Käytä seuraavia komentoja.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        $startDate = (Get-Date).ToString('MM/dd/yyyy')
        $endDate = (Get-Date).AddDays(365).ToString('MM/dd/yyyy')

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -b $startDate -e $endDate -r -len 2048

    Voit pyydetään yksityisen avaimen salasana. Kun komento suoritetaan onnistuneesti, näkyy **CertFile.cer** ja **mykey.pvk** määrittämäsi varmenteen hakemistossa.

4. Käytä [Pvk2Pfx] [ pvk2pfx] apuohjelma muuntaa .pvk ja .cer-tiedostoja, jotka MakeCert luotu .pfx-tiedosto. Suorita seuraava komento.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Kun sinulta kysytään entisellään yksityisen avaimen salasana. **Ostotilauksen -** parametrille määritetty arvo on salasanaa, jolla on liitetty .pfx-tiedosto. Kun komento onnistuu näkyy myös CertFile.pfx määrittämäsi varmenteen hakemistossa.

###  <a name="create-an-azure-active-directory-and-a-service-principal"></a>Azure Active Directory-ja palvelun lyhennyksen.

Tässä osassa suorittaa luoda palvelu pääasiallista Azure Active Directory-sovelluksen ja Määritä rooli pääasiallista palvelun todennetaan palvelun lyhennys antamalla varmenteen vaiheet. Suorita seuraavat komennot Azure Active Directory-sovelluksen luominen.

1. Liitä seuraavat cmdlet-komennot PowerShell console-ikkunassa. Varmista, että määrität **Näyttönimi -** ominaisuuden arvo on yksilöllinen. Myös **- Aloitussivu** ja **- IdentiferUris** arvot ovat paikkamerkin arvoja ja ei ole vahvistettu.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
                    -DisplayName "HDIADL" `
                    -HomePage "https://contoso.com" `
                    -IdentifierUris "https://mycontoso.com" `
                    -KeyValue $credential  `
                    -KeyType "AsymmetricX509Cert"  `
                    -KeyUsage "Verify"  `
                    -StartDate $startDate  `
                    -EndDate $endDate

        $applicationId = $application.ApplicationId

2. Luo service-lyhennys käyttämällä sovelluksen ID-tunnuksellasi.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id

3. Myönnä järvi tietovaraston tiedostoa tai kansiota, jota käytät HDInsight klusterista palvelun pääasiallista käyttöoikeus. Alla koodikatkelman pääsee ylimmällä järvi tietosäilö-tili.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All

    Kehottaessa Kirjoita Vahvista **Y** .

## <a name="create-an-hdinsight-cluster-with-authentication-to-data-lake-store"></a>Luo HDInsight-klusterin järvi tietosäilö-todennus

Tässä osassa on luoda HDInsight Hadoop-klusterin. Tässä versiossa HDInsight-klusterin ja järvi tietovaraston on oltava samassa sijainnissa (Itä US 2).

1. Aloita haetaan tilauksen vuokraajan ID-tunnuksellasi. Tarvitset, myöhemmin.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Tässä versiossa Hadoop-klusterin järvi tietovaraston voi käyttää vain Lisää tallennustilaa klusterin. Oletus-tallennustila on tallennustilan Azure-BLOB-objektit (WASB). On ensin Luo tallennustilan tilin ja tallennustilan säiliöiden klusterin vaaditaan.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName | %{ $_.Key1 }
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext

3. Luo HDInsight-klusterin. Käytä seuraavat cmdlet-komennot.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $rdpCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.2" -RdpCredential $rdpCredentials -RdpAccessExpiry (Get-Date).AddDays(14) -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Kun cmdlet onnistuu pitäisi näkyä tulos tältä:

        Name                      : hdiadlcluster
        Id                        : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/hdiadlgroup/providers/Mi
                                    crosoft.HDInsight/clusters/hdiadlcluster
        Location                  : East US 2
        ClusterVersion            : 3.2.7.707
        OperatingSystemType       : Windows
        ClusterState              : Running
        ClusterType               : Hadoop
        CoresUsed                 : 16
        HttpEndpoint              : hdiadlcluster.azurehdinsight.net
        Error                     :
        DefaultStorageAccount     :
        DefaultStorageContainer   :
        ResourceGroup             : hdiadlgroup
        AdditionalStorageAccounts :

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Suorita testi työt HDInsight-klusterin käyttämään järvi tietosäilö

Kun olet määrittänyt HDInsight-klusterin, voit suorittaa testin työt klusterin Testaa, että HDInsight-klusterin käyttää järvi tietosäilö. Voit tehdä olemme suoritetaan otoksen rakenteen työn, joka luo taulukon, voit ladata aiemmin järvi tietovaraston mallitietoja käyttämällä.

### <a name="for-a-linux-cluster"></a>Linux-klusterin

Tässä osassa on SSH klusterin ja suorita rakenne esimerkkikyselyn. Windows ei tarjoa valmiin SSH asiakas. On suositeltavaa käyttää **painovärit, muste**, joka voi ladata [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Lisätietoja painovärit, muste on artikkelissa [Käyttäminen SSH kanssa Linux-pohjaiset Hadoop-HDInsight Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Kun yhteys on muodostettu, Käynnistä rakenne-CLI käyttämällä seuraava komento:

        hive

2. Käytä CLI, kirjoita seuraavista väittämistä mallitietojen avulla järvi tietovaraston **ajoneuvot** uuden taulukon luominen:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Näyttöön tulee tulos seuraavankaltaiselta:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


### <a name="for-a-windows-cluster"></a>Windows-klusterin

Seuraavat cmdlet-komennot avulla voit suorittaa kyselyn rakenne. Kyselyn luominen taulukon tiedoista järvi tietovaraston ja suorita valintakysely luodun taulukon.

    $queryString = "DROP TABLE vehicles;" + "CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://$dataLakeStoreName.azuredatalakestore.net:443/';" + "SELECT * FROM vehicles LIMIT 10;"

    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString

    $hiveJob = Start-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $httpCredentials

    Wait-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $httpCredentials

Tämä on seuraava tulos. **ExitValue** 0 tulosteesta ehdottaa työn onnistui.

    Cluster         : hdiadlcluster.
    HttpEndpoint    : hdiadlcluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1445386885331_0012
    ParentId        :
    PercentComplete :
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done

Noutaa tulosteen työn seuraavat cmdlet-komennolla:

    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $hiveJob.JobId -DefaultContainer $containerName -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -ClusterCredential $httpCredentials

Työn tulos näyttää jotakuinkin seuraavasti:

    1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
    1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
    1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
    1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
    1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
    1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
    1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
    1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
    1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
    1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


## <a name="access-data-lake-store-using-hdfs-commands"></a>Accessin Lake Tietosäilölle HDFS-komentojen käyttäminen

Kun olet määrittänyt käyttämään järvi tietovaraston HDInsight-klusterin, voit HDFS shell-komennot käyttämään kauppa.

### <a name="for-a-linux-cluster"></a>Linux-klusterin

Tässä osassa sinun tulee SSH klusterin kyselyjä ja suorita HDFS-komennot. Windows ei tarjoa valmiin SSH asiakas. On suositeltavaa käyttää **painovärit, muste**, joka voi ladata [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Lisätietoja painovärit, muste on artikkelissa [Käyttäminen SSH kanssa Linux-pohjaiset Hadoop-HDInsight Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Kun yhteys on muodostettu, järvi tietovaraston tiedostojen HDFS tiedostojärjestelmän-komennon avulla.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Tiedosto, jonka voit ladata aiemmin järvi tietovaraston näyttöön tulevat.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Voit käyttää myös `hdfs dfs -put` joidenkin tiedostojen lataaminen järvi tietovaraston ja käytä sitten komento `hdfs dfs -ls` voit tarkistaa, onko tiedostojen lataaminen onnistui.


### <a name="for-a-windows-cluster"></a>Windows-klusterin

1. Kirjaudu uudessa [Azure-portaalissa](https://portal.azure.com).

2. Valitsemalla **Selaa**ja valitse **HDInsight klustereiden**, jonka loit HDInsight-klusterin.

3. Klusterin-sivu- **Etätyöpöytä**ja valitse sitten **Yhdistä** **Etätyöpöytä** -sivu.

    ![Remote HDI klusterin tuominen] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.HDI.PS.Remote.Desktop.png "Azure resurssi-ryhmän luominen")

    Kirjoita pyydettäessä antamasi remote työpöydän käyttäjän tunnistetiedot.

4. Käynnistä Windows PowerShell Etäistunto ja HDFS tiedostojärjestelmän komentojen avulla voit näyttää järvi Azure tietovaraston tiedostot.

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    Tiedosto, jonka voit ladata aiemmin järvi tietovaraston näyttöön tulevat.

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/vehicle1_09142014.csv

    Voit käyttää myös `hdfs dfs -put` joidenkin tiedostojen lataaminen järvi tietovaraston ja käytä sitten komento `hdfs dfs -ls` voit tarkistaa, onko tiedostojen lataaminen onnistui.

## <a name="see-also"></a>Katso myös

* [Portaali: Luo HDInsight-klusterin käyttämään järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
