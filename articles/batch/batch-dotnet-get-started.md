<properties
    pageTitle="Opetusohjelma – Azure erä .NET-kirjaston käytön aloittaminen | Microsoft Azure"
    description="Lue peruskäsitteet Azure Siirtoerän ja niiden lisäämisestä Kehitä Esimerkkitapaus erä palvelua."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-library-for-net"></a>Aloita .NET Azure erä-kirjaston kanssa

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

[Azure erä] perusteet[ azure_batch] ja [Erä .NET] [ net_api] kirjaston tämän artikkelin keskustelua C# sovelluksen malli vaihe vaiheelta. Tarkastellaan kuinka otoksen sovelluksen hyödyntää erä palvelun käsittelemään rinnakkain työmäärää pilvipalvelussa, sekä kuinka se toimii [Azuren tallennustilaan](../storage/storage-introduction.md) kanssa tiedoston väliaikaisen ja hakemista. Opit yleisiä erä sovelluksen työnkulun avulla. Myös artikkelista tärkeimmät osat Siirtoerän, kuten töitä, tehtäviä, jakavat, perus ymmärtämisen ja Laske solmujen.

![Erän ratkaisu työnkulun (basic)][11]<br/>

## <a name="prerequisites"></a>Edellytykset

Tässä artikkelissa oletetaan, että sinulla on kokemusta C# ja Visual Studio. Se myös oletetaan, että et pysty tilin luominen vaatimukset, jotka on määritetty alla Azure ja erä- ja -palvelut.

### <a name="accounts"></a>Tilit

- **Azure-tili**: Jos sinulla ei vielä ole Azure tilauksen, [vapaa Azure-tilin luominen][azure_free_account].
- **Erän tilin**: kun sinulla on Azure tilaus [Azure erä-tilin luominen](batch-account-create-portal.md).
- **Tallennustilan tilin**: Katso [Lisätietoja Azure](../storage/storage-create-storage-account.md)-tallennustilan tilin [Luo tallennustilan tili](../storage/storage-create-storage-account.md#create-a-storage-account) .

> [AZURE.IMPORTANT] Erän tukee tällä hetkellä *vain* **yleisiä tarkoitus** tallennustilan tilin tyyppi-ohjeiden vaiheessa #5 [tietoja Azure](../storage/storage-create-storage-account.md)-tallennustilan tilin [Luo tallennustilan tili](../storage/storage-create-storage-account.md#create-a-storage-account) .

### <a name="visual-studio"></a>Visual Studio

Sinulla on oltava **Visual Studio 2015** otoksen projektin luonnissa. Löydät maksuttomia ja kokeiluversion versiot Visual Studio [Visual Studio 2015 tuotteiden yleiskatsaus][visual_studio].

### <a name="dotnettutorial-code-sample"></a>*DotNetTutorial* koodi-Esimerkki

[DotNetTutorial] [ github_dotnettutorial] malli on yhtä monta MALLIKOODEJA [azure-erä-objektit] -kohdan[ github_samples] säilöön-GitHub. Voit ladata malli napsauttamalla säilöön aloitussivulla **Lataa ZIP** -painiketta tai valitsemalla [azure-erä-mallit-master.zip] [ github_samples_zip] suoraan latauslinkki. Kun olet purkanut ZIP-tiedoston sisällön, etsii ratkaisun seuraavaan kansioon:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure erä Explorer (valinnainen)

[Azure erä Explorer] [ github_batchexplorer] on ilmainen apuohjelma, joka sisältyy [azure-erä-objektit] [ github_samples] säilöön-GitHub. Kun ei tarvitse suorittaa tässä opetusohjelmassa, voi olla hyödyllistä kehittäminen ja virheenkorjaus erä ratkaisuja.

## <a name="dotnettutorial-sample-project-overview"></a>DotNetTutorial otoksen projektin yleiskatsaus

*DotNetTutorial* esimerkkikoodi on Visual Studio 2015-ratkaisun, joka sisältää kaksi projektia: **DotNetTutorial** ja **TaskApplication**.

- **DotNetTutorial** on asiakassovellus, joka toimii yhdessä Siirtoerän ja tallennustilaa palveluihin suorittamaan rinnakkain työmäärää Laske solmujen (näennäiskoneiden). DotNetTutorial suoritetaan paikallisen työasema.

- **TaskApplication** on ohjelma, joka suoritetaan, Laske solmuissa Azure suorittamiseen todellinen työmäärä. Näytteen `TaskApplication.exe` jäsentää tiedoston tekstin ladattujen Azuren tallennustilaan (input-tiedosto). Valitse se tuottaa tekstitiedosto (kohdetiedosto), joka sisältää ylimmän kolme sanat, jotka näkyvät lähdetiedoston luettelo. Kohdetiedosto luotuaan TaskApplication Lataa tiedosto Azure-tallennustilan. Tällöin käytettävissä Lataa-asiakassovellukseen. TaskApplication suoritetaan rinnakkain useita Laske solmujen erä-palvelussa.

Seuraavassa kaaviossa on kuvattu ensisijainen toiminnot, jotka tehdään asiakassovellus, *DotNetTutorial*ja sovellus, joka suoritetaan tehtävien *TaskApplication*mukaan. Tavallinen tämä työnkulku on tyypillisiä monta Laske-ratkaisuja, jotka on luotu erä. Samalla, kun se osoittaa ei kaikkia ominaisuuksia, jotka ovat käytettävissä erä-palvelussa, erä lähes kaikissa skenaarioissa sisältää samanlaisia prosessit.

![Erän esimerkin][8]<br/>

[**Vaihe 1.**](#step-1-create-storage-containers) Luo **säilöjen** Azure-Blob-objektien tallennustilaan.<br/>
[**Vaihe 2.**](#step-2-upload-task-application-and-data-files) Lataa tehtävän sovelluksen ja syötteen tiedostot säilöjen.<br/>
[**Vaihe 3.**](#step-3-create-batch-pool) Luoda erää **resurssivarantoon**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3 a.** Resurssivarannon **StartTask** Lataa tehtävän binaaritiedostot (TaskApplication) solmujen, kun he liittyä ryhmään.<br/>
[**Vaihe 4.**](#step-4-create-batch-job) Luoda erää **työn**.<br/>
[**Vaihe 5.**](#step-5-add-tasks-to-job) **Tehtävien** lisääminen projektiin.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5 a.** Tehtävät ajoitetaan suorittamaan solmuissa.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5 b.** Kunkin tehtävän sen syöttötiedot lataamisen Azuren tallennustilaan ja valitse suoritus alkaa.<br/>
[**Vaihe 6.**](#step-6-monitor-tasks) Seuraa tehtäviä.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6 a.** Valmiiksi tehtävät ne Lataa tulosteen tietonsa Azure-tallennustilan.<br/>
[**Vaihe 7: ssä.**](#step-7-download-task-output) Lataa tehtävän tulosteen tallennustilan.

Kuten edellä, kaikki erän-ratkaisun suorittaa tarkka seuraavasti ja monista muista voi sisältää, mutta sovelluksen *DotNetTutorial* malli esittelee löytyvät erä ratkaista yleisiä prosesseja.

## <a name="build-the-dotnettutorial-sample-project"></a>*DotNetTutorial* otoksen projektin luominen

Voit suorittaa otosten, sinun on määritettävä Siirtoerän ja tallennustilaa tilin tunnistetiedot *DotNetTutorial* projektin `Program.cs` tiedosto. Jos et ole vielä niin, Avaa ratkaisun Visual Studiossa kaksoisnapsauttamalla `DotNetTutorial.sln` ratkaisutiedosto. Tai avaa se Visual Studion avulla **Tiedosto > Avaa > Project-ratkaisun** valikko.

Avaa `Program.cs` *DotNetTutorial* projektissa. Lisää tunnistetiedot tiedoston yläosassa määritetyllä tavalla:

```
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [AZURE.IMPORTANT] Kuten edellä mainittiin, sinun on määritettävä **yleisiä tarkoitus** tallennustilan tilin tunnistetiedot tällä hetkellä Azuren tallennustilaan. Erän sovellus käyttää Blob-objektien tallennustilaan **yleisiä tarkoitus** tallennustilan tilin. Määritä, joka on luotu valitsemalla *Blob-objektien tallennustilaan* tilityyppi tallennustilan tilin tunnistetiedot.

Löydät Siirtoerän ja tallennustilaa tilin tunnistetiedot kuluessa kunkin palvelun tili-sivu [Azure portal][azure_portal]:

![Erän tunnistetiedot portaalissa][9]
![tallennustilan tunnistetiedot-portaalissa][10]<br/>

Nyt kun olet päivittänyt projektin kanssa tunnistetiedot, napsauta ratkaisunhallinnassa ratkaisu ja valitse **Muodosta ratkaisu**. Vahvista palautus minkä tahansa NuGet-paketti, jos sinua pyydetään.

> [AZURE.TIP] Jos NuGet-pakettien ei palautettu automaattisesti tai jos näyttöön tulee virhe tietoja paketit palauttaa virheen, varmista, että sinulla on [NuGet pakettien hallinta] [ nuget_packagemgr] asennettuna. Valitse Ota käyttöön puuttuu pakettien Lataa. Katso [Ottaminen käyttöön paketti palauttaa aikana luominen] [ nuget_restore] paketin lataaminen käyttöön.

Seuraavissa osissa on ohjeet, joita se suorittaa käsittelemään työmäärää erä palvelussa otoksen sovellus eritellä ja keskusteleminen nämä vaiheet yksityiskohtaisesti. On suositeltavaa viitata Avaa ratkaisun käyttöön Visual Studiossa työskentelyn muiden tässä artikkelissa tene jälkeen ei jokaisen rivin koodin otoksessa käsitellään.

Siirry alkuun `MainAsync` *DotNetTutorial* projektin menetelmä `Program.cs` tiedoston, voit aloittaa vaiheessa 1. Jokaisen vaiheen alla sitten suurin piirtein seuraavasti menetelmä puhelut etenemisen `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Vaihe 1: Luo tallennustilan säiliöiden

![Luo säilöjen Azuren tallennustilaan][1]
<br/>

Erän sisältää valmiita tuen käyttäminen Azure-tallennustilan. Säilöjen tallennustilan tilisi antaa erä tilisi suoritettavien tehtävien tarvitsemia tiedostoja. Säilöjen tarjoavat myös tallennuspaikka tiedot, jotka tuottavat tehtävät. Ensiksi *DotNetTutorial* asiakassovellus onko on kolme säilöjen luominen [Azure-Blob-säiliö](../storage/storage-introduction.md):

- **sovelluksen**: Tämä säilö tallentaa sovelluksen tehtävät sekä mitä tahansa sen riippuvuudet, kuten dll Suorita.
- **syötteen**: tehtävien Lataa käsittelemään *syötteen* säilöstä datatiedostot.
- **tulos**: Kun Tehtävien käsittely tiedoston, he lataavat tulokset *tulosteen* säilöön.

Jotta voit käsitellä tallennustilan tilin ja luo säilöt-Käytämme [Azure tallennustilan asiakkaan kirjasto .NET][net_api_storage]. Microsoft-tilille viittauksen luominen kanssa [CloudStorageAccount][net_cloudstorageaccount], ja, joka luo [CloudBlobClient][net_cloudblobclient]:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Käytämme `blobClient` viitata koko sovelluksen ja välittää sen parametrina useilla eri tavoilla. Esimerkki tämä on koodin estoasetukset välittömästi seuraavan edellä, jossa on kutsu `CreateContainerIfNotExistAsync` voit luoda itse asiassa säilöt.

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Säilöjen on luotu, kun sovellus nyt ladata tiedostoja, jotka käyttävät tehtäviin.

> [AZURE.TIP] [.NET-Blob-objektien tallennustilaan käyttämisestä](../storage/storage-dotnet-how-to-use-blobs.md) on hyvä yleiskatsaus Azure-tallennustilan säiliöiden ja BLOB-objektit. Sen pitäisi olla lukeminen luettelon yläosassa kuin alat erän kanssa.

## <a name="step-2-upload-task-application-and-data-files"></a>Vaihe 2: Tehtävien sovelluksen ja tietojen tiedostojen lataaminen

![Tehtävä-sovelluksen ja syötteen (tiedot)-tiedostojen lataaminen säiliöiden][2]
<br/>

Lataa tiedostotoiminto *DotNetTutorial* määrittää ensin **sovelluksen** ja **syötteen** tiedostopolkujen hallintatyökalua, sellaisena kuin ne ovat paikallisessa tietokoneessa. Valitse Lataa tiedostot säilöjen, jonka loit edellisessä vaiheessa.

```
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Valitse kahdella `Program.cs` , jotka osallistuvat lataus:

- `UploadFilesToContainerAsync`: Tämä menetelmä palauttaa [ResourceFile] kokoelma[ net_resourcefile] objektit (kuvattu alla) ja sisäisesti puhelut `UploadFileToContainerAsync` latautuminen on välitetty parametri *filePaths* tuotaville tiedostoille.
- `UploadFileToContainerAsync`: Tämä on todella suorittaa tiedostojen lataamisen ja luovan [ResourceFile] [ net_resourcefile] objekteja. Ladattuasi tiedoston se hakee tiedoston jaetun access-allekirjoitus (SAS) ja palauttaa ResourceFile-objekti, joka esittää sitä. Jaettu access allekirjoitukset käsitellään myös alla.

```
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles

[ResourceFile] [ net_resourcefile] sisältää tehtävien osalta ja Azure-tallennustilan, jota ladataan Laske-solmu, ennen kuin tehtävä on suoritettu tiedoston URL-osoite. [ResourceFile.BlobSource] [ net_resourcefile_blobsource] ominaisuus määrittää tiedoston täydellinen URL-osoite, kuten Azuren tallennustilaan olemassa. URL-Osoitteen voi olla myös jaetun access-allekirjoitus (SAS), joka sisältää tiedoston suojattu käyttö. Tehtävien useimpien sisällä erä .NET sisältävät *ResourceFiles* -ominaisuutta, mukaan lukien:

- [CloudTask][net_task]
- [StartTask][net_pool_starttask]
- [JobPreparationTask][net_jobpreptask]
- [JobReleaseTask][net_jobreltask]

Sovelluksen DotNetTutorial malli ei käytä JobPreparationTask tai JobReleaseTask tehtävälajit, mutta voit lukea lisää tietoja niiden [asennuksen valmistelu ja valmistuminen projektitehtävien Azure erän Laske solmujen](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Jaettu käyttö allekirjoitus (SAS)

Jaettu käyttö allekirjoitukset ovat merkkijonot joka, kun URL-Osoitteen osana ohjelmistopakettia – tiettyjen säilöjen ja BLOB Azuren tallennustilaan suojattu käyttö. Sovellus käyttää Blob-objektien ja jaetut säilö DotNetTutorial käyttää allekirjoituksen URL-osoitteet ja kerrotaan, miten voit hankkia jaettua käyttöä allekirjoituksen merkkijonon tallennustilan-palvelusta.

- **Blob-objektien jaettu access allekirjoitukset**: ryhmän StartTask-DotNetTutorial käyttää jaettu blob access allekirjoituksia, kun se lataa sovelluksen binaaritiedostot ja syöttötiedot tiedostojen tallennustilan (Katso vaihe 3 alla). `UploadFileToContainerAsync` DotNetTutorial's menetelmä `Program.cs` sisältää koodi, joka hakee kunkin Blob-objektien jaettua käyttöä allekirjoitus. Se tekee soittamalla [CloudBlob.GetSharedAccessSignature][net_sas_blob].

- **Säilön jaettu access allekirjoitukset**: kunkin tehtävän sen käsitelleet Laske-solmu, kun se latauksia sen kohdetiedosto *tulosteen* säilöön Azure-tallennustilan. Voit tehdä TaskApplication käyttää säilö jaetun access-allekirjoitus, joka sisältää kirjoitusoikeudet säilöön osana polku, kun se Lataa tiedosto. Hankkiminen säilö jaetun access-allekirjoituksen samalla tavalla kuin on valmis kun hankkiminen blob jaettu access allekirjoitus. Valitse DotNetTutorial, huomaat, että `GetContainerSasUrl` helper menetelmä soittaa [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] tekemään niin. Lue lisätietoja siitä, miten TaskApplication käyttää säilö jaettu access allekirjoitukseen "Vaihe 6: valvonnan tehtävät."

> [AZURE.TIP] Kaksiosainen sarjat-jaettu käyttö allekirjoituksia, tutustu [osa 1: tietoja jaetun access allekirjoitus (SAS)-mallin](../storage/storage-dotnet-shared-access-signature-part-1.md) ja [osa 2: Luo ja käyttää jaettua käyttöä allekirjoituksen (SAS) Blob-objektien tallennustilaan](../storage/storage-dotnet-shared-access-signature-part-2.md), saat lisätietoja tallennustilan tilisi tiedot suojattu käyttömahdollisuus.

## <a name="step-3-create-batch-pool"></a>Vaihe 3: Luoda erää resurssivarantoon

![Luoda erää varannon][3]
<br/>

Erän **varanto** on kokoelma Laske solmujen (näennäiskoneiden), erä suorittaa projektin tehtävät.

Sen jälkeen, kun se lataa sovellus ja datatiedostot tallennustilan tilin, *DotNetTutorial* käynnistää sen vuorovaikutus erä-palvelussa käyttäen erän .NET-kirjasto. Voit tehdä tämän [BatchClient] [ net_batchclient] luodaan ensimmäisen kerran:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Seuraavaksi Laske solmujen resurssivarantoon luodaan puhelun erä tiliä `CreatePoolAsync`. `CreatePoolAsync`käyttää [BatchClient.PoolOperations.CreatePool] [ net_pool_create] luominen todella resurssivarantoon erä-palvelussa.

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

Kun luot resurssivarantoon [CreatePool][net_pool_create], voit määrittää useita parametreja, kuten Laske solmujen, [solmut koosta](../cloud-services/cloud-services-sizes-specs.md)ja sen solmujen käyttöjärjestelmän määrä. Valitse *DotNetTutorial*Käytämme [CloudServiceConfiguration] [ net_cloudserviceconfiguration] Windows Server 2012 R2 [Cloud Services](../cloud-services/cloud-services-guestos-update-matrix.md)-palveluiden määrittämiseen. Kuitenkin määrittämällä [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] sen sijaan voit luoda jakavat solmujen luotu Marketplace-kuvia, joka sisältää Windows- ja Linux kuvat, saat lisätietoja [säännöstä Linux Laske Azure erä jakavat solmujen](batch-linux-nodes.md) .

> [AZURE.IMPORTANT] Perittävän Laske resurssien osalta. Pienennä kustannukset, voit pienentää `targetDedicated` 1, ennen kuin suoritat otosten.

Fyysinen solmu nämä ominaisuudet sekä voit myös määrittää [StartTask] [ net_pool_starttask] varannon varten. StartTask suorittaa kunkin solmun solmun yhdistää altaan ja aina, kun solmu käynnistetään. StartTask on hyötyä erityisesti sovellusten asentaminen Laske solmujen ennen tehtävien suorittamista. Esimerkiksi jos tehtäväsi käsitellä tietoja käyttämällä Python komentosarjoja, voit käyttää StartTask Python asennetaan Laske solmut.

Esimerkki-sovelluksessa StartTask kopioi tiedostot, jotka se lataamisen tallennustilan (joka on määritetty käyttämällä [StartTask][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] ominaisuus) StartTask käytön hakemiston jaettuun kansioon, joka käyttää solmun *kaikki* tehtävät. Kopioidaan `TaskApplication.exe` ja riippuvaa jaetussa kansiossa sijaitseva kunkin solmun kuin solmu yhdistää resurssivarantoon, jotta kaikki tehtävät, jotka suoritetaan solmun voit käyttää sitä.

> [AZURE.TIP] Azure erän **sovelluksen pakettien** -toiminto tarjoaa toinen tapa hankkia sovelluksen sivulle Laske solmut resurssivarantoon. [Sovelluksen-ympäristö, jonka Azure erä sovelluksen pakettien](batch-application-packages.md) lisätietoja.

Myös huomattavat yllä koodikatkelman on kaksi ympäristömuuttujien StartTask *Komentorivi* -ominaisuuden käyttö: `%AZ_BATCH_TASK_WORKING_DIR%` ja `%AZ_BATCH_NODE_SHARED_DIR%`. Kukin Laske solmu erä varannon määritetään automaattisesti useita ympäristömuuttujat, jotka ovat erä. Prosessi, joka on suorittaa tehtävän käyttää muuttujia ympäristössä.

> [AZURE.TIP] Jos haluat lisätietoja ympäristön muuttujat, jotka ovat käytettävissä Laske solmuissa erä resurssivarantoon ja tehtävien käytön hakemistojen selaaminen, tiedot on kohdissa [ympäristöasetuksia tehtävien](batch-api-basics.md#environment-settings-for-tasks) ja [tiedostojen ja kansioiden](batch-api-basics.md#files-and-directories) [erän ominaisuuden yhteenveto kehittäjille](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Vaihe 4: Luoda erää työ

![Luoda erää työn][4]<br/>

Erän **työ** on kokoelma, tehtävät ja Laske solmujen resurssivarantoon liittyy. Projektin tehtävien suorittaa liittyvät resurssivarantoon Laske solmuissa.

Voit käyttää projektin ei ainoastaan järjestäminen ja tehtäviin liittyvät työmääriä, mutta myös käyttöön tiettyjä rajoituksia – kuten suurin runtime työn (ja sen tehtävien tunnisteesta) sekä projektin prioriteetin suhteessa muihin kohteisiin erä tilin seurantaa varten. Tässä esimerkissä kuitenkin työ on liitetty vain ryhmän, joka on luotu vaiheessa #3. Muita ominaisuuksia ei ole määritetty.

Kaikki erän työt liittyy tiettyjä resurssivarantoon. Tämä kytkentä ilmaisee projektin tehtävät suoritetaan-solmut. Voit määrittää tämän käyttämällä [CloudJob.PoolInformation] [ net_job_poolinfo] ominaisuutta, kuten alla koodikatkelman.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Nyt kun työ on luotu, tehtävät lisätään työn tekemiseen.

## <a name="step-5-add-tasks-to-job"></a>Vaihe 5: Tehtävien lisääminen projektiin

![Tehtävien lisääminen projektiin][5]<br/>
*(1) tehtävät lisätään työ, (2) tehtävät ajoitetaan voidaan suorittaa solmujen ja (3) tehtävät ladata tiedostojen käsittely*

Erän **tehtävät** ovat yksittäisiä yksiköitä tehdyn työn, joka suorittaa Laske solmut. Tehtävä on komentorivin ja suorittaa komentosarjoja tai suoritettavat tiedostot, jotka määrität kyseisen komentoriviltä.

Todellisuudessa tekemässä töitä, tehtäviä on lisättävä projektin. Kunkin [CloudTask] [ net_task] on määritetty käyttämällä komentorivin ominaisuus ja [ResourceFiles] [ net_task_resourcefiles] (kuten ryhmän StartTask kanssa), joka tehtävän Lataa solmu ennen sen komentoriviltä suoritetaan automaattisesti. Projectissa *DotNetTutorial* otoksen kunkin tehtävän käsittelee vain yhden tiedoston. Näin ollen ResourceFiles sen sivustokokoelman on yksi elementti.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [AZURE.IMPORTANT] Kun hän käyttää ympäristömuuttujat kuten `%AZ_BATCH_NODE_SHARED_DIR%` tai suorita jokin sovellus ei löydy solmun `PATH`-komennon tehtävärivien on etuliite `cmd /c`. Tämä suorittaa komennon kääntäjän ja opasta sen komento tarkastuksen jälkeen Lopeta erikseen. Tämä vaatimus ei tarvita, jos tehtävien suorittaminen sovelluksen solmun `PATH` (esimerkiksi *robocopy.exe* tai *powershell.exe*) ja ympäristöä ei ole muuttujaa käytetään.

Sisällä `foreach` yllä koodikatkelman silmukka, voit tarkastella tehtävän komentorivin muodostetaan siten, että kolme komentorivin argumentit välitetään *TaskApplication.exe*:

1. **Ensimmäinen argumentti** on käyttänyt tiedoston polku. Tämä on olemassa solmun paikallisen tiedoston polku. Kun ResourceFile objekti on `UploadFileToContainerAsync` luomisen yläpuolella tiedostonimi on käytetty tämän ominaisuuden (ResourceFile konstruktorin parametrinä). Tämä ilmaisee, että tiedoston löytyvät *TaskApplication.exe*samassa kansiossa.

2. **Toinen argumentti** määrittää, että kohdetiedosto on kirjoitettava ylimmät *N* sanat. Otoksen tämä on koodattu niin, että kohdetiedosto kirjoitetaan yläreunan kolme sanaa.

3. **Kolmas argumentti** on jaettu käyttö allekirjoitus (SAS), joka sisältää Azuren tallennustilaan **tulosteen** säilöön kirjoitusoikeudet. *TaskApplication.exe* käyttää jaettua käyttöä allekirjoituksen URL-osoite, kun se lataa kohdetiedosto Azure-tallennustilan. Voit etsiä tästä koodi `UploadFileToContainer` TaskApplication projektin menetelmä `Program.cs` tiedosto:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Vaihe 6: Näytön tehtävät

![Näytön tehtävät][6]<br/>
*Asiakassovellus (1) valvoo valmistumisprosentin ja success tila tehtävät ja (2) tehtävät lataa tiedot Azuren tallennustilaan*

Kun tehtäviin on lisätty projektiin, ne jonossa ja ajoittaa suorittamisen käyttöön liittyvän työn resurssivarantoon sijaitsevien solmujen suorittaminen automaattisesti. Voit määrittää asetusten perusteella erä käsittelee kaikki tehtävän queuing, ajoitus, yritetään ja muiden tehtävien hallinta-tehtävät puolestasi. On monia tavoista seurantaan tehtävän suorittaminen. DotNetTutorial on hyötyä vain valmistumisprosentin ja tehtävä-virhe tai success perustuvat raportit yksikertaisessa esimerkissä.

Sisällä `MonitorTasks` DotNetTutorial's menetelmä `Program.cs`, on kolme erä .NET-käsitteestä, jotka edellyttävät keskustelun. Ne on lueteltu alla järjestykseen ulkoasu:

1. **ODATADetailLevel**: määrittäminen [ODATADetailLevel] [ net_odatadetaillevel] luettelossa toimintoja (kuten hankkiminen projektin tehtävien luettelon) on tärkeää varmistaa erä sovelluksen suorituskykyä. Lisää lukeminen luetteloon [kyselyn Azure erä palvelun tehokkaasti](batch-efficient-list-queries.md) , jos aio tekevät minkä tahansa Lajittele tilan seuranta erä-sovelluksissa.

2. **TaskStateMonitor**: [TaskStateMonitor] [ net_taskstatemonitor] tarjoaa erä .NET sovellusten helper apuohjelmien tehtävän tilan seuranta. Valitse `MonitorTasks`, *DotNetTutorial* odottaa kaikkien tehtävien saavuttamiseksi [TaskState.Completed] [ net_taskstate] määräajassa. Valitse se päättyy työn.

3. **TerminateJobAsync**: lopetetaan työn [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (tai eston JobOperations.TerminateJob), työn merkitsee valmiiksi. On tärkeää tehdä, jos erä ratkaisu käyttää [JobReleaseTask][net_jobreltask]. Tämä on erityinen tehtävä, joka on kuvattu [valmistelu ja valmistuminen projektitehtävien](batch-job-prep-release.md).

`MonitorTasks` *DotNetTutorial*menetelmää `Program.cs` alapuolella:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>Vaihe 7: Lataa tehtävän tulos

![Lataa tehtävän tulosteen tallennustila][7]<br/>

Nyt kun työ on valmis, tuloste tehtävät voi ladata Azure-tallennustilan. Tämä tehdään puhelun `DownloadBlobsFromContainerAsync` - *DotNetTutorial* `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [AZURE.NOTE] Puhelun `DownloadBlobsFromContainerAsync` *DotNetTutorial* sovellus määrittää, että tiedostot ladataan että `%TEMP%` kansio. Vapaasti muokata tämän tulosteen sijainti.

## <a name="step-8-delete-containers"></a>Vaihe 8: Poista säilöt

Perittävän tiedot, jotka sijaitsevat Azuren tallennustilaan, koska se on aina kannattaa poistaa minkä tahansa BLOB-objektit, erä töiden ei enää tarvita. DotNetTutorial's- `Program.cs`, tämä tehdään helper menetelmä kolme puhelut `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

Menetelmä itse hakee ainoastaan säilö viittaus ja soittaa [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Vaihe 9: Työn ja ryhmän poistaminen

Käyttäjää pyydetään viimeisessä vaiheessa voit poistaa työn ja jotka on luotu DotNetTutorial-sovelluksen sovellussarjan. Vaikka ei peri projektien ja tehtävien itse *ovat* veloitetaan Laske solmujen. Näin on suositeltavaa varata solmujen vain tarvittaessa. Käyttämättömien jakavat poistaminen voi olla ylläpito yhteydessä.

BatchClient [JobOperations] [ net_joboperations] ja [PoolOperations] [ net_pooloperations] molemmissa vastaavia poistaminen menetelmiä, joita kutsutaan, jos käyttäjä vahvistaa poistaminen:

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [AZURE.IMPORTANT] Muista, että Laske resurssien perittävän — poistetaan käyttämättömät jakavat minimoi kustannukset. Huomioon lisäksi, että poistaminen resurssivarantoon poistaa kaikki kyseisen ryhmän Laske solmut ja tietojen käsittelyssä solmut olevan vakava altaan poistamisen jälkeen.

## <a name="run-the-dotnettutorial-sample"></a>Suorita *DotNetTutorial* -Esimerkki

Kun käynnistät sovelluksen malli-konsolin tuloste ovat seuraavankaltaiselta. Suorituksen aikana ilmenee tauon osoitteessa `Awaiting task completion, timeout in 00:30:00...` samalla, kun ryhmän Laske solmujen käynnistetään. Käytä [Azure portal] [ azure_portal] seurannassa lisääminen resurssivarantoon Laske solmujen, työn ja tehtävien aikana ja suorittamisen jälkeen. Käytä [Azure portal] [ azure_portal] tai [Azure-tallennustilan Explorer] [ storage_explorers] tallennustilan resurssit (säilöjä ja BLOB), jotka on luotu sovelluksen tarkastelemiseen.

Tyypillinen suoritusaika on **noin 5 minuuttia** , kun käynnistät sovelluksen sen alkuperäisessä kokoonpanossa.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Seuraavat vaiheet

Vapaasti tehdä muutoksia *DotNetTutorial* ja *TaskApplication* kokeilemaan eri Laske skenaarioita. Kokeile esimerkiksi lisäämällä suorittamisen viive *TaskApplication*, kuten kanssa [Thread.Sleep][net_thread_sleep], voit simuloida pitkään suoritettavien tehtävien ja seurata niitä portaalissa. Kokeile lisäämään useita tehtäviä tai Laske solmujen määrän muuttaminen. Logiikan etsiä ja Salli aiemmin resurssivarantoon, nopeuden suorittamisen ajaksi (*Vihje*: tutustu `ArticleHelpers.cs` [Microsoft.Azure.Batch.Samples.Common] -[ github_samples_common] projektin [azure-erä-objektit][github_samples]).

Nyt kun osaat erä ratkaista basic työnkulussa, se on, kun haluat tarkastella erä palvelun lisäominaisuuksia.

- Lue [erän ominaisuuden yhteenveto kehittäjille](batch-api-basics.md), joka on suositeltavaa kaikille uusille erä-käyttäjille.
- Käynnistä erä kehittäminen artikkeliin kohdassa **Development perusteellisempaa** [erä oppimispolku][batch_learning_path].
- Tutustu eri soveltaminen käsittely "ylimmät N sanat" työmäärää käyttämällä erän [TopNWords] [ github_topnwords] malli.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Luo säilöjen Azuren tallennustilaan"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Tehtävä-sovelluksen ja syötteen (tiedot)-tiedostojen lataaminen säiliöiden"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Luoda erää resurssivarantoon"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Luoda erää työn"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Tehtävien lisääminen projektiin"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Näytön tehtävät"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Lataa tehtävän tulosteen tallennustila"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Erän ratkaisu työnkulku (koko kaavio)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Erän tunnistetiedot-portaalissa"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Tallennustilan tunnistetiedot-portaalissa"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Erän ratkaisu työnkulun (mahdollisimman vähän kaavio)"
