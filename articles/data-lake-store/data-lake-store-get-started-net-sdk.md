<properties
   pageTitle="Sovellusten tietojen järvi kaupan .NET SDK avulla | Microsoft Azure"
   description="Sovellusten kehittämiseen Azure tietojen järvi kaupan .NET SDK: N avulla"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Azure Lake Tietosäilölle käyttämällä .NET SDK: N käytön aloittaminen

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShellin](data-lake-store-get-started-powershell.md)
- [.NET SDK-PAKETISSA](data-lake-store-get-started-net-sdk.md)
- [Java SDK-paketissa](data-lake-store-get-started-java-sdk.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Opettele käyttämään [Azure tietojen järvi kaupan .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx) suorittamaan peruslaskutoimituksia, kuten luoda kansioita, lataa ja lataa datatiedostojen jne. Saat lisätietoja tietojen järvi [Azure järvi tietosäilö](data-lake-store-overview.md).

## <a name="prerequisites"></a>Edellytykset

* **Visual Studio 2013: n tai 2015**. Alla olevien ohjeiden avulla Visual Studio 2015.

* **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

* **Azure järvi tietovaraston tili**. Ohjeita siitä, miten voit luoda tili on artikkelissa [Azure järvi tietovaraston käytön aloittaminen](data-lake-store-get-started-portal.md)

* **Azure Active Directory-sovelluksen luominen**. Azure AD-sovelluksen avulla voit todentaa Azure AD järvi tietosäilö-sovellukseen. Useilla eri tavoilla Azure AD-todentamismenetelmä on **käyttäjän todennusta** tai **palvelu todennusta**. Ohjeita ja lisätietoja tarkistamiseen on artikkelissa [tietojen järvi kaupan Tarkista Azure Active Directoryn avulla](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>.NET-sovelluksen luominen

1. Avaa Visual Studio ja console-sovelluksen luominen.

2. **Tiedosto** -valikosta **Uusi**ja valitse sitten **Projekti**.

3. **Uuden projektin**Kirjoita tai valitse seuraavat arvot:

  	| Ominaisuus | Arvo                       |
  	|----------|-----------------------------|
  	| Luokka | Mallit ja Visual C# / Windows |
  	| Malli | Console-sovellus         |
  	| Nimi     | CreateADLApplication        |

4. Valitse **OK** , jos haluat luoda projektin.

5. Nuget-pakettien lisääminen projektiin.

    1. Projektin nimeä Solution Explorerissa hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**.
    2. **Nuget pakettien hallinta** -välilehdessä Varmista, että **paketti lähde** on määritetty **nuget.org** , **Sisällytä prerelease** -valintaruutu on valittuna.
    3. Etsi ja asenna seuraavat NuGet paketit:

        * `Microsoft.Azure.Management.DataLake.Store`– Tässä opetusohjelmassa käytetään v0.12.5 esikatselu.
        * `Microsoft.Azure.Management.DataLake.StoreUploader`– Tässä opetusohjelmassa käytetään v0.10.6 esikatselu.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication`– Tässä opetusohjelmassa käytetään v2.2.8 esikatselu.

        ![Lisää Nuget lähde] (./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Luo uusi Azure tietojen järvi tili")

    4. Sulje **Nuget pakettien hallinta**.

6. Avaa **Program.cs**, Poista olemassa oleva koodi ja Sisällytä seuraavista väittämistä Lisää nimitilan viittauksia.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. Määritä muuttujat alla kuvatulla tavalla ja sisältävät arvot järvi tietovaraston ja resurssien ryhmänimen, jotka ovat jo. Varmista myös, että tietokoneessa on oltava antaisit tähän paikallinen polku ja tiedostonimi. Lisäämällä seuraavat koodikatkelman nimitilamäärityksiä.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

Näet käytettävissä olevat .NET menetelmät avulla voit suorittaa toimintoja, kuten todennus-tiedoston lataaminen-artikkelin jäljellä oleviin kohtiin.

## <a name="authentication"></a>Todennus

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Jos käytössäsi on käyttäjän todennusta (suositellaan Tässä opetusohjelmassa)

Käytä tätä Azure AD "Native Client" sovellukseen; on sellaisen alla. Avulla voit suorittaa tässä opetusohjelmassa nopeammin, on suositeltavaa käyttää tätä tapaa.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Tärkeää tietää tietoja edellä tämän koodikatkelman tapahtuu.

* Avulla voit suorittaa opetusohjelman nopeammin tämän koodikatkelman käyttää Azure AD, toimialueen ja asiakkaan ID, jolla on oletusarvoisesti kaikki Azure tilaukset. Näin on, voit tehdä **käyttää tätä koodikatkelman nimellä-sovelluksesi sijaitsee**.
* Jos haluat käyttää omia Azure AD toimialueen ja sovelluksen Asiakastunnus, voit kuitenkin on alkuperäisen Azure AD-sovelluksen luominen ja sitten Azure AD-toimialuetta Asiakastunnus- ja uudelleenohjata URI loit sovelluksen. Saat ohjeet [Active Directory-sovelluksen luominen](../resource-group-create-service-principal-portal.md#create-an-active-directory-application) .

>[AZURE.NOTE] Ohjeita edellä linkit ovat Azure AD-web-sovelluksen. Vaiheet ovat kuitenkin täsmälleen sama myös silloin, kun haluat luoda native client-sovelluksen sijaan. 

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Jos käytössäsi on palvelu todennusta ja asiakkaan salaisuus 

Seuraavat koodikatkelman voidaan suorittaa todennusta sovelluksesi ei-vuorovaikutteisesti käyttävä salainen / -näppäintä sovelluksen tai palvelun lyhennyksen. Käytä aiemmin luotua [Azure AD "Web App-sovelluksen](../resource-group-create-service-principal-portal.md)kanssa.

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Jos käytät sertifikaatilla palvelun todennus

Kun annetaan kolmas vaihtoehto, seuraavat koodikatkelman voidaan todentaa sovelluksesi ei-vuorovaikutteisesti sovelluksen tai palvelun lyhennys sertifikaatilla. Käytä aiemmin luotua [Azure AD "Web App-sovelluksen](../resource-group-create-service-principal-portal.md)kanssa.

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>Luo asiakkaan objektit

Seuraavat koodikatkelman Luo järvi tietovaraston tilin ja tiedostojärjestelmän asiakkaan objekteja, joita käytetään pyytää antamaan palvelu.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Luettele kaikki tilauksen järvi tietovaraston tilit

Seuraavat koodikatkelman näyttää kaikki tietyn Azure-tilauksen piiriin kuuluvien järvi tietovaraston tilit.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## <a name="create-a-directory"></a>Hakemiston luominen

Koodikatkelman seuraavassa `CreateDirectory` menetelmä, jota voit käyttää hakemiston järvi tietosäilö-tilin luominen.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Tiedoston lataaminen

Seuraavassa koodikatkelman `UploadFile` menetelmä, jota voit käyttää tiedostojen lataaminen järvi tietosäilö-tili.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader`tukee rekursiivinen Lataa ja lataa paikallisen tiedostopolku ja järvi tietovaraston tiedostopolusta välillä.    

## <a name="get-file-or-directory-info"></a>Tiedoston tai kansion hankkiminen

Koodikatkelman seuraavassa `GetItemInfo` , joiden avulla voit hakea tietoja tiedoston tai kansion käytettävissä järvi säilössä menetelmää. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Luettelon tai kansioita

Koodikatkelman seuraavassa `ListItem` menetelmä, jota voit käyttää luettelon tiedostojen ja kansioiden järvi tietovaraston tilillä.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>KETJUTA-tiedostot

Koodikatkelman seuraavassa `ConcatenateFiles` menetelmä, jota käytetään KETJUTA tiedostot. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Tiedoston liittäminen

Koodikatkelman seuraavassa `AppendToFile` menetelmä, jota voit käyttää tietojen liittäminen järvi tietovaraston tilillä jo tallennetun tiedoston.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>Lataa tiedosto

Koodikatkelman seuraavassa `DownloadFile` menetelmä, jota käytetään ladata tiedoston järvi tietovaraston tilin.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## <a name="next-steps"></a>Seuraavat vaiheet

- [Tietojen suojaaminen järvi säilössä](data-lake-store-secure-data.md)
- [Azure tietojen järvi Analytics käyttäminen järvi tietosäilö](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure Hdinsightiin käyttäminen järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Tietosäilö järvi .NET SDK-ohje](https://msdn.microsoft.com/library/mt581387.aspx)
- [Tietosäilö järvi muiden viittauksen](https://msdn.microsoft.com/library/mt693424.aspx)
