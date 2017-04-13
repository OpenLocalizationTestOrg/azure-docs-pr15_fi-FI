<properties
    pageTitle="Siirtää Azure Resurssienhallinta Kehitystyökalut varten HDInsight klustereiden | Microsoft Azure"
    description="Siirtämisestä Azure Resurssienhallinta Kehitystyökalut HDInsight klustereiden varten"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>Siirtyminen HDInsight klustereiden Azure Resurssienhallinta-pohjainen Kehitystyökalut

Hdinsightista on kuinka Azure palvelun hallinta ASM-pohjaisten työkalujen HDInsight varten. Jos olet käyttänyt PowerShellin Azure, Azure CLI tai HDInsight .NET SDK HDInsight klustereiden käyttöä varten, olet käyttää Azure resurssien hallinnan ARM-pohjainen versiot PowerShell CLI ja .NET SDK Exceliä. Tässä artikkelissa on osoittimet siirtäminen uuteen ARM-pohjainen menetelmän. Mahdollisuuksien tässä artikkelissa myös osoittaa, mitä eroa ASM ja ARM tavoista HDInsight varten.

>[AZURE.IMPORTANT] ASM tuki perusteella PowerShell-CLI, ja .NET SDK keskeytetään **2017 1 tammikuussa**käyttöön.

##<a name="migrating-azure-cli-to-azure-resource-manager"></a>Siirtyminen Azure CLI, Azure resurssien hallinta

Azure-CLI oletukset nyt Azure resurssien hallinta (ARM)-tilaan, paitsi jos olet päivittämässä Officen aiempi asennus Tässä tapauksessa kannattaa käyttää `azure config mode arm` ARM-tila-komennolla.

Peruskomennot, joissa on Azure CLI HDInsight-käyttöä varten käyttämällä Azure Service Management (ASM) ovat samat käytettäessä ARM; kuitenkin joitakin parametrit ja valitsimet saattaa olla uusia nimiä ja on useita uusia parametreja käytettävissä ARM käytettäessä. Esimerkiksi voit nyt käyttää `azure hdinsight cluster create` Määritä Azure virtuaaliverkko, klusterin luodaanko tai rakenne- ja Oozie metastore tiedot.

Peruskomennot käsittelyyn HDInsight kautta Azure Resurssienhallinta ovat:

* `azure hdinsight cluster create`– Luo uuden HDInsight-klusterin
* `azure hdinsight cluster delete`-poistaa aiemmin luodun HDInsight-klusterin
* `azure hdinsight cluster show`– Näyttää tietoja aiemmin luodun klusterin
* `azure hdinsight cluster list`-tilauksen Azure Hdinsightiin klustereiden luettelo

Käytä `-h` Siirry tarkastaa jokaisen komennon käytettävissä olevat valitsimet ja parametrit.

###<a name="new-commands"></a>Uusien komentojen

Käytettävissä olevat Azure resurssien hallinnan uudet komennot ovat:

* `azure hdinsight cluster resize`-muuttuu dynaamisesti klusterin työntekijä solmujen määrän
* `azure hdinsight cluster enable-http-access`-kautta voidaan käyttää HTTPs klusterin (Valitse oletusarvon mukaan)
* `azure hdinsight cluster disable-http-access`-HTTPs käyttöoikeus klusterin poistaa käytöstä
* `azure hdinsight-enable-rdp-access`-mahdollistaa Remote Desktop Protocol Windows-pohjaisesta HDInsight-klusterissa
* `azure hdinsight-disable-rdp-access`-poistaa käytöstä Windows-pohjaisesta HDInsight-klusterin Remote Desktop Protocol
* `azure hdinsight script-action`-komennoilla luomista ja hallintaa varten komentosarjatoiminnot klusterissa
* `azure hdinsight config`-komennoilla luot määritystiedoston, joka voidaan käyttää kanssa `hdinsight cluster create` komento määritysten tietoja.

###<a name="deprecated-commands"></a>Vanhentuneen komennot

Jos käytät `azure hdinsight job` komentoja, voit lähettää työt HDInsight-klusterin, ne eivät ole käytettävissä ARM-komennot kautta. Jos haluat lähettää työt ohjelmallisesti komentosarjojen HDInsight-käytettävä sijaan REST API myöntämä Hdinsightista. Lisätietoja lähettämistä työt käyttämällä REST API-kohdassa seuraavat asiakirjat.

* [Suorittaa MapReduce työt Hadoop kanssa käyttämällä kääntö Hdinsightiin](hdinsight-hadoop-use-mapreduce-curl.md)
* [Tee rakenne kyselyt Hadoop kanssa käyttämällä kääntö Hdinsightiin](hdinsight-hadoop-use-hive-curl.md)
* [Suorittaa Possu työt Hadoop kanssa käyttämällä kääntö Hdinsightiin](hdinsight-hadoop-use-pig-curl.md)

Muita tapoja suorittaa MapReduce, rakenne ja Possu vuorovaikutteisesti tietoja on artikkelissa [Käytön MapReduce Hadoop-HDInsight kanssa](hdinsight-use-mapreduce.md), [Käytä rakenne ja Hadoop HDInsight-](hdinsight-use-hive.md)ja [Käytä Possu Hadoop HDInsight](hdinsight-use-pig.md).

###<a name="examples"></a>Esimerkkejä

__Klusterin luominen__

* Vanha komennon (ASM)-`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Uusi (ARM) - komento`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

__Klusterin poistaminen__

* Vanha komennon (ASM)-`azure hdinsight cluster delete myhdicluster`
* Uusi (ARM) - komento`azure hdinsight cluster delete mycluster -g myresourcegroup`

__Luettelon klustereiden__

* Vanha komennon (ASM)-`azure hdinsight cluster list`
* Uusi (ARM) - komento`azure hdinsight cluster list`

> [AZURE.NOTE] Luettelo-komento määrittäminen resurssien ryhmän käyttämällä `-g` palauttaa vain klustereiden määritettyä resurssia-ryhmässä.

__Klusterin tietojen näyttäminen__

* Vanha komennon (ASM)-`azure hdinsight cluster show myhdicluster`
* Uusi (ARM) - komento`azure hdinsight cluster show myhdicluster -g myresourcegroup`


##<a name="migrating-azure-powershell-to-azure-resource-manager"></a>Siirtyminen Azure PowerShellin avulla Azure resurssien hallinta

Yleisiä tietoja PowerShellin Azure Azure resurssien hallinta (ARM)-tilassa tukikäytännöistä [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md).

Azure PowerShell KÄDESSÄ cmdlet-komennot voivat olla asennettu-rinnakkais suorittamalla ASM cmdlet-komennot. Cmdlet-komentoja-tilasta voidaan erottaa toisistaan niiden nimien perusteella.  ARM-tila on *AzureRmHDInsight* cmdlet-komento nimet vertailu, *AzureHDInsight* ASM-tilassa.  *Uusi AzureRmHDInsightCluster* esimerkiksi ja *Uusi AzureHDInsightCluster*. Parametrit ja valitsimet saattaa olla uutisia nimet ja on useita uusia parametreja käytettävissä ARM käytettäessä.  Esimerkiksi useita cmdlet-komennot edellytä kutsutaan *- ResourceGroupName*uusi valitsinta. 

Ennen kuin voit käyttää HDInsight-cmdlet-komennot, muodosta yhteys Azure-tili ja luo uusi resurssiryhmä:

- Kirjaudu sisään AzureRmAccount tai [Valitse AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx). Katso [todennustapa palvelun lyhennys Azure resurssien hallinta](../resource-group-authenticate-service-principal.md)
- [Uusi AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

###<a name="renamed-cmdlets"></a>Uudelleennimetyt cmdlet-komennot

Windows PowerShell console HDInsight ASM-cmdlet-luetteloon:

    help *azurermhdinsight*

Seuraavassa taulukossa on lueteltu ASM cmdlet-komentojen ja heidän nimensä ARM-tilassa:

|ASM cmdlet-komennot|ARM-cmdlet-komennot|
|------|------|
| Lisää AzureHDInsightConfigValues | [Lisää AzureRmHDInsightConfigValues](https://msdn.microsoft.com/library/mt603530.aspx)|
| Lisää AzureHDInsightMetastore |[Lisää AzureRmHDInsightMetastore](https://msdn.microsoft.com/library/mt603670.aspx)|
| Lisää AzureHDInsightScriptAction |[Lisää AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx)|
| Lisää AzureHDInsightStorage |[Lisää AzureRmHDInsightStorage](https://msdn.microsoft.com/library/mt619445.aspx)|
| Hae AzureHDInsightCluster |[Hae AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619371.aspx)|
| Hae AzureHDInsightJob |[Hae AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603590.aspx)|
| Hae AzureHDInsightJobOutput |[Hae AzureRmHDInsightJobOutput](https://msdn.microsoft.com/library/mt603793.aspx)|
| Hae AzureHDInsightProperties |[Hae AzureRmHDInsightProperties](https://msdn.microsoft.com/library/mt603546.aspx) |
| Myönnä AzureHDInsightHttpServicesAccess | [Myönnä AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619407.aspx)|
| Myönnä AzureHdinsightRdpAccess |[Myönnä AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603717.aspx)|
| Käynnistää AzureHDInsightHiveJob |[Käynnistää AzureRmHDInsightHiveJob](https://msdn.microsoft.com/library/mt603593.aspx)|
| Uusi AzureHDInsightCluster | [Uusi AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx)|
| Uusi AzureHDInsightClusterConfig |[Uusi AzureRmHDInsightClusterConfig](https://msdn.microsoft.com/library/mt603700.aspx)|
| Uusi AzureHDInsightHiveJobDefinition |[Uusi AzureRmHDInsightHiveJobDefinition](https://msdn.microsoft.com/library/mt619448.aspx)|
| Uusi AzureHDInsightMapReduceJobDefinition |[Uusi AzureRmHDInsightMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx)|
| Uusi AzureHDInsightPigJobDefinition |[Uusi AzureRmHDInsightPigJobDefinition](https://msdn.microsoft.com/library/mt603671.aspx)|
| Uusi AzureHDInsightSqoopJobDefinition |[Uusi AzureRmHDInsightSqoopJobDefinition](https://msdn.microsoft.com/library/mt608551.aspx)|
| Uusi AzureHDInsightStreamingMapReduceJobDefinition |[Uusi AzureRmHDInsightStreamingMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx)|
| Poista AzureHDInsightCluster |[Poista AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619431.aspx)|
| REVOKE-AzureHDInsightHttpServicesAccess |[REVOKE-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619375.aspx)|
| REVOKE-AzureHdinsightRdpAccess |[REVOKE-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603523.aspx)|
| Määritä AzureHDInsightClusterSize |[Määritä AzureRmHDInsightClusterSize](https://msdn.microsoft.com/library/mt603513.aspx)|
| Määritä AzureHDInsightDefaultStorage |[Määritä AzureRmHDInsightDefaultStorage](https://msdn.microsoft.com/library/mt603486.aspx)|
| Käynnistä AzureHDInsightJob |[Käynnistä AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603798.aspx)|
| Lopeta AzureHDInsightJob |[Lopeta AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt619424.aspx)|
| Käytä AzureHDInsightCluster |[Käytä AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619442.aspx)|
| Odota AzureHDInsightJob |[Odota AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603834.aspx)|

###<a name="new-cmdlets"></a>Uuden cmdlet-komennot
Seuraavassa on uusi cmdlet-komennot, jotka ovat käytettävissä ARM-tilassa. 

**Komentosarja-toiminnon liittyvä cmdlet-komennot:**
- **Hae AzureRmHDInsightPersistedScriptAction**: noutaa klusterin pysyviä komentosarjan toiminnot ja ne on lueteltu aikajärjestyksessä tai noutaa tiedot määritetyn pysyviä komentosarja-toiminnon. 
- **Hae AzureRmHDInsightScriptActionHistory**: noutaa komentosarja-toiminnon historia klusterin ja se on lueteltu käänteisessä aikajärjestyksessä tai saa aiemmin suoritettu komentosarja-toiminnon yksityiskohdat. 
- **Poista AzureRmHDInsightPersistedScriptAction**: poistaa pysyviä komentosarja-toiminnon HDInsight-klusterin.
- **Määritä AzureRmHDInsightPersistedScriptAction**: määrittää aiemmin suoritettu komentosarja-toiminnon olevan pysyviä komentosarja-toiminnon.
- **Lähetä AzureRmHDInsightScriptAction**: lähettää Azure HDInsight-klusterin uuden komentosarjatoiminnon. 

Ylimääräisten lisätietoja [mukauttaminen Linux-pohjaiset HDInsight klustereiden komentosarja-toiminnon avulla](hdinsight-hadoop-customize-cluster-linux.md).

**Clsuter tunnistetietojen liittyvä cmdlet-komennot:**

- **Lisää AzureRmHDInsightClusterIdentity**: Lisää klusterin tunnistetiedot klusterin kokoonpano-objekti, niin, että HDInsight-klusterin käyttää Azure tietojen järvi Stores. Katso [Luo HDInsight-klusterin tietojen järvi kaupan PowerShellin Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

### <a name="examples"></a>Esimerkkejä

**Klusterin luominen**

Vanha komento (ASM): 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

Uusi komento (ARM):

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials

 
**Klusterin poistaminen**

Vanha komento (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Uusi komento (ARM):

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 
                
**Luettelon klusterin**

Vanha komento (ASM):

    Get-AzureHDInsightCluster
                
Uusi komento (ARM):

    Get-AzureRmHDInsightCluster 

**Näytä klusterin**

Vanha komento (ASM):

    Get-AzureHDInsightCluster -Name $clusterName
                
Uusi komento (ARM):

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


####<a name="other-samples"></a>Muut objektit

- [Luo HDInsight klustereiden](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
- [Lähettää rakenteen työt](hdinsight-hadoop-use-hive-powershell.md)
- [Lähettää Possu työt](hdinsight-hadoop-use-pig-powershell.md)
- [Lähettää Sqoop työt](hdinsight-hadoop-use-sqoop-powershell.md)



##<a name="migrating-to-the-arm-based-hdinsight-net-sdk"></a>ARM-pohjainen HDInsight .NET SDK siirtyminen

Azure hallinnan perustuva [(ASM) HDInsight .NET SDK: ssa](https://msdn.microsoft.com/library/azure/mt416619.aspx) on nyt poistettu. Olet käyttää Azure Resurssienhallinta-pohjainen [(ARM) HDInsight.NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx). Seuraavat ASM perustuva HDInsight-paketit ollaan poistamassa.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`
 

Tässä osassa on lisätietoja osoittimet ARM-pohjainen SDK tiettyjen tehtävien suorittamisesta.

| Miten... käyttämällä ARM-pohjainen HDInsight SDK | Linkit |
| ------------------- | --------------- |
| Luo HDInsight klustereiden .NET SDK: N avulla| Katso [luominen HDInsight klustereiden .NET SDK: N avulla](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)|
| Komentosarja-toiminnon käyttäminen .NET SDK klusterin mukauttaminen | Katso [mukauttaminen HDInsight Linux klustereiden komentosarja-toiminnon käyttäminen](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action) |
| Todentaa Azure Active Directory käyttäminen vuorovaikutteisesti .NET SDK sovellukset| Kohdassa [Suorita rakenne kyselyjen .NET SDK: N avulla](hdinsight-hadoop-use-hive-dotnet-sdk.md). Tässä artikkelissa koodikatkelman käyttää vuorovaikutteisia todennusta-vaihtoehto.|
| Todentaa Azure Active Directory käyttäminen ei-vuorovaikutteisesti .NET SDK sovellukset | Katso [Luo HDInsight - vuorovaikutteinen sovellukset](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| Lähetä .NET SDK työn rakenne| Katso [Lähetä rakenne töitä](hdinsight-hadoop-use-hive-dotnet-sdk.md) |
| Lähetä Possu työn .NET SDK | Katso [Lähetä Possu työt](hdinsight-hadoop-use-pig-dotnet-sdk.md)|
| Lähetä Sqoop työn .NET SDK | Katso [Lähetä Sqoop työt](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |
| Luettelo HDInsight klustereiden .NET SDK: N avulla | Katso [luettelo HDInsight klustereiden](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| Skaalaa HDInsight klustereiden .NET SDK: N avulla | Katso [asteikko HDInsight klustereiden](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| Myönnä/revoke access HDInsight klustereihin .NET SDK: N avulla | Artikkelissa [Grant/revoke käyttöoikeuksien Hdinsightiin klustereiden](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| Päivitä HDInsight klustereiden käyttämällä .NET SDK HTTP käyttäjän tunnistetiedot | Katso [HDInsight klustereiden päivityksen HTTP käyttäjän tunnistetiedot](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| Etsi tallennustilan oletustilin HDInsight klustereiden .NET SDK: N avulla | Katso [HDInsight klustereiden tallennustilan oletustilin etsiminen](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| Poista HDInsight klustereiden .NET SDK: N avulla | Katso [poistaminen HDInsight klustereiden](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Esimerkkejä

Seuraavassa on joitakin esimerkkejä siitä, kuinka toiminto on suoritettu ASM perustuva SDK ja vastaavat koodikatkelman käytön ARM-pohjainen SDK-paketissa.

**Klusterin CRUD asiakkaan luominen**

* Vanha komennon (ASM)

        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
     
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);

* Uusi-komento (ARM) (palvelu pääasiallista luvan)

        //Service principal auth
        //This will log the application in as itself, rather than on behalf of a specific user.
        //For details, including how to set up the application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
         
        var authFactory = new AuthenticationFactory();
         
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
         
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
         
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
         
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
         
        _hdiManagementClient = new HDInsightManagementClient(creds);

* Uusi-komento (ARM) (käyttäjän käyttöoikeuksien)

        //User auth
        //This will log the application in on behalf of the user.
        //The end-user will see a login popup.
         
        var authFactory = new AuthenticationFactory();
         
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
         
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
         
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
         
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
         
        _hdiManagementClient = new HDInsightManagementClient(creds);


**Klusterin luominen**

* Vanha komennon (ASM)

        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);


* Uusi-komento (ARM)

        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Windows,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);


**HTTP-käytön ottaminen käyttöön**

* Vanha komennon (ASM)

        client.EnableHttp(dnsName, "West US", "admin", "*******");

* Uusi-komento (ARM)

        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**Klusterin poistaminen**

* Vanha komennon (ASM)

        client.DeleteCluster(dnsName);

* Uusi-komento (ARM)

        client.Clusters.Delete(resourceGroup, dnsname);




