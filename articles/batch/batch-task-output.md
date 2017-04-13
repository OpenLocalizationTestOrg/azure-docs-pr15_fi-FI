<properties
    pageTitle="Työn ja tehtävän tulosteen Azure erä pysyvyyttä | Microsoft Azure"
    description="Opi käyttämään Azuren tallennustilaan kestävät kaupan erätehtävän ja projektin tulosteen ja ota käyttöön tässä pysyviä tulosteen tarkasteleminen Azure-portaalissa."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="persist-azure-batch-job-and-task-output"></a>Pysyvä Azure erän työn ja tehtävän tulos

Suorita osalta yleensä tehtävät tuottaa, joka on tallennettu ja sitten myöhemmin noutamien muiden tehtävien työn asiakassovellus, joka suorittaa työn tai molemmat. Tämä tulos ehkä käsittelemällä syöttötiedot luotuja tiedostoja tai lokitiedostot liittyvän tehtävän suorittaminen. Tässä artikkelissa esitellään .NET luokkakirjasto, joka käyttää nimeämiskäytännön-pohjainen tekniikka jatkuvat näiden tehtävien tulos: Azure-Blob-säiliö saataville senkin jälkeen, kun poistat jakavat, töitä, ja Laske solmujen.

Käyttämällä tekniikka tämän artikkelin voit myös tarkastella tehtävän tulosteen **tallennetut tiedostot** ja [Azure portal] **tallennetut lokit** [portal].

![Tallennetut tiedostot ja tallennettu lokit valitsimien-portaalissa][1]

>[AZURE.NOTE] [Azure erä tiedoston nimeämiskäytännön] [ nuget_package] .NET luokkakirjasto tässä artikkelissa on tällä hetkellä esikatselu. Jotkin kuvatut ominaisuudet saattavat muuttua ennen yleiseen käyttöön.

## <a name="task-output-considerations"></a>Tehtävän tulosteen huomioon otettavia seikkoja

Ota huomioon seuraavat seikat, jotka liittyvät projektin ja tehtävän tulostaa suunnitellessasi erä ratkaisu.

* **Laske solmu elinaika**: Laske solmut ovat yleensä lyhytkestoisia, etenkin Automaattinen skaalaus käyttävä jakavat. Solmun suoritettavien tehtävien tulostaa ovat käytettävissä vain, kun solmu on olemassa, ja vain tiedoston säilytys ajassa määrittämäsi tehtävän. Voit varmistaa, että tehtävä tulokset säilyy, tehtäväsi on siksi niiden tulosteen tiedostojen lataaminen kestävät säilöön, esimerkiksi Azure-tallennustilan.

* **Tulosteen tallennustilan**: jatkuvat kestävät tallennustilan tehtävän tulosteen tietoja, voit [Azure-tallennustilan SDK](../storage/storage-dotnet-how-to-use-blobs.md) tehtävän koodin lataaminen Blob storage säilön tehtävä tulokset. Jos otat säilö ja nimeämiskäytäntö tiedoston, asiakassovellus tai muiden tehtävien työn voit etsiä ja lataa tämän tulostus konferenssi perusteella.

* **Tulosteen noutaminen**: Voit noutaa tehtävän tulosteen suoraan lisääminen resurssivarantoon Laske solmujen tai Azuren tallennustilaan Jos tehtävien jatkuu niiden. Hakemiseen tehtävän tulosteen suoraan Laske-solmu on tiedostonimi ja solmun tulosteen sijainti. Jos tulos Azuren tallennustilaan jatkuvat, edeltävät tehtävät tai asiakassovellus on oltava koko polku tiedoston lataaminen käyttämällä Azure-tallennustilan SDK Azuren tallennustilaan.

* **Tarkasteleminen output**: kun Siirry Azure-portaalissa erätehtävän ja valitse **solmu tiedostoja**, näyttöön tulee kaikki tiedostot, jotka liittyvät, ei ole juuri tulostiedostoja haluat muuttaa. Uudelleen Laske solmujen tiedostot ovat käytettävissä vain, kun solmu on olemassa ja vain tiedoston säilytys ajassa määrittämäsi tehtävän. Jos haluat tarkastella tehtävän tulosteen, joka on samanlainen Azuren tallennustilaan portaalin tai jokin sovellus, esimerkiksi [Azure-tallennustilan Explorer][storage_explorer], on tiedettävä paikkaan ja siirry tiedoston suoraan.

## <a name="help-for-persisted-output"></a>Ohjeen pysyviä tulostusta varten

Auttaa Lisää helposti jatkuvat työ- ja tehtävän tulosteen, erä-ryhmän on määritetty ja toteutettu nimeämiskäytännöt joukko sekä .NET-luokkakirjasto, [Azure erä tiedoston nimeämiskäytännön] [ nuget_package] kirjastossa, voit käyttää erä-sovelluksissa. Azure-portaali on lisäksi huomioon nämä nimeämiskäytännöt niin, että löydät helposti käyttämällä kirjaston olet tallennettuja tiedostoja.

## <a name="using-the-file-conventions-library"></a>Kirjaston tiedoston nimeämiskäytännön käytöstä

[Azure erä tiedoston nimeämiskäytännön] [ nuget_package] on .NET luokkakirjasto, erä .NET-sovellusten avulla voit helposti tallentaa ja tarkastella tehtävän tulostus ja sieltä pois Azure-tallennustilan. Se on tarkoitettu käytettäväksi tehtävä- ja asiakkaan koodin--tehtävän koodissa pysyvä tiedostojen ja asiakkaan koodin luettelon ja tarkastella niitä. Tehtävien käyttää myös kirjaston hakemisen tulostaa ylöspäin tehtäviä, kuten [tehtäväriippuvuudet](batch-task-dependencies.md) tilanne.

Nimeämiskäytännön kirjaston kestää varoen varmistaa, että tallennustilan säiliöiden ja tehtävän tulosteen tiedostot ovat nimeltään mukaan konferenssi ja ladataan oikeaan paikkaan, kun samanlainen Azure-tallennustilan. Kun haet tulostus, löytää helposti tulostaa tietyn projektin tai tehtävän päällä tai haetaan tunnuksen ja tarkoitukseen, sen sijaan, että tiedostonimien tai johon se on tallennustilan tulostaa.

Esimerkiksi kirjastossa voit käyttää "näyttää kaikki keskitason tiedostot tehtävän 7" tai "hankkiminen minä esikatselukuva työn *mymovie"*tarvitsematta tiedostonimiä tai tallennustilan tilisi toiseen kohtaan.

### <a name="get-the-library"></a>Hae kirjasto

Voit hankkia kirjastoon, joka sisältää uudet luokat ja laajentaa [CloudJob] [ net_cloudjob] ja [CloudTask] [ net_cloudtask] luokat, joissa uusi menetelmien, [NuGet][nuget_package]. Voit lisätä sen [NuGet kirjaston pakettien hallinta]Visual Studio projektin[nuget_manager].

>[AZURE.TIP] Voit etsiä [lähdekoodin] [ github_file_conventions] .NET säilöön Microsoft Azure SDK-paketissa GitHub Azure erä tiedoston nimeämiskäytännön-kirjastoon.

## <a name="requirement-linked-storage-account"></a>Vaatimus: linkitetyn tallennustilan tilin

Tulostaa tiedoston nimeämiskäytännön kirjaston käytöstä kestävät tallennustilan tallentamiseen ja tarkastella niitä Azure-portaalissa, sinun on [linkki Azure-tallennustilan tilin](batch-application-packages.md#link-a-storage-account) erä-tilillesi. Jos et ole jo, Azure-portaalissa erä tilisi tallennustilan tilin linkittää:

**Erän tili** -sivu > **asetukset** > **Tallennustilan tilin** > **Tallennustilan tilin** (ei mitään) > tilauksen tallennustilan tilin valitseminen

Katso tarkempia tietoja hallintapaketteihin linkittämisestä tallennustilan tilin [Azure erä sovelluksen pakettien sovellusten käyttöön](batch-application-packages.md).

## <a name="persist-output"></a>Pysyvä tulostus

On kaksi ensisijainen toiminnot, jotka suoritetaan tallennettaessa työn ja tehtävän tulosteen tiedoston nimeämiskäytännön kirjastoa: tallennustilan säiliön luominen ja tallentaminen tulosteen säilöön.

>[AZURE.WARNING] Koska kaikki projektin ja tehtävän tulostus on tallennettu samaan säilöön, [tallennustilan rajoitusten rajoitukset](../storage/storage-performance-checklist.md#blobs) saattaa suoritettaviksi Jos paljon tehtäviä yrittää jatkuvat tiedostoja yhtä aikaa.

### <a name="create-storage-container"></a>Luo tallennustilan säilö

Ennen kuin tehtävien aloittaa tallennustilan pysyvä tulosteen, sinun on luotava blob storage säilön, johon ne ladata niiden. Tee näin soittamalla [CloudJob][net_cloudjob]. [PrepareOutputStorageAsync] [net_prepareoutputasync]. Tunniste-menetelmä kestää [CloudStorageAccount] [ net_cloudstorageaccount] parametrina objekti ja Luo nimetty siten, että sen sisältö on löydettävissä Azure portaalin ja noutaminen myöhemmin tässä artikkelissa käsitellään tapoja säilön.

Tämä koodi sijoitetaan yleensä asiakassovellus--sovellus, joka luo jakavat, projektien ja tehtävien.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Tallentaa tehtävän tulostus

Nyt kun olet valmistellut säilön Blob-objektien tallennustilaan, tehtävien säästää tulosteen säilö käyttämällä [TaskOutputStorage] [ net_taskoutputstorage] luokkaa löydy tiedoston nimeämiskäytännön kirjastossa.

Tehtävä-koodisi luomalla ensin [TaskOutputStorage] [ net_taskoutputstorage] objekti, ja kun tehtävä on suoritettu työnsä, soita [TaskOutputStorage][net_taskoutputstorage]. [SaveAsync] [net_saveasync] menetelmä Tallenna tulos Azure-tallennustilan.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

"Tulosteen laji-parametrin luokittelee pysyvän tiedostot. On neljä ennalta [TaskOutputKind] [ net_taskoutputkind] tyypit: "TaskOutput", "TaskPreview", "TaskLog" ja "TaskIntermediate." Voit myös määrittää mukautetun tiedostotyyppejä, jos ne on hyötyä työnkulussa.

Tulosteen tällaisia avulla voit määrittää tulostaa luetteloon, kun voit myöhemmin kyselyn erä pysyviä tulostaa tietyn tehtävän tyyppi. Toisin sanoen, kun tehtävän tulostus-luettelosta voit suodattaa luettelosta jokin tulos. Esimerkiksi "Anna minulle *Esikatselu* tulosteen tehtävän *109*,." Lisää luettelon ja tulostaa hakeminen näkyy [noutaa tulostus](#retrieve-output) artikkelin.

>[AZURE.TIP] Tulosteen oikeuksia määrittää tietyn tiedoston kohtaa, jossa Azure-portaalissa näkyy myös: *TaskOutput*-luokitellut tiedostot näkyvät "Tehtävän tiedostot" ja *TaskLog* tiedostot näkyvät "Tehtävän lokit."

### <a name="store-job-outputs"></a>Tallenna työ tulostaa

Lisäksi tallentaa tehtävän tulostus, voit tallentaa koko projektin liittyvät tulostaa. Yhdistämisen elokuvan värien työn tehtävässä voi esimerkiksi jatkuvat täysin hahmontamisen elokuvan työn tulosteen nimellä. Kun työ on valmis, asiakassovellus voit yksinkertaisesti luettelo ja hakea työn, tulostaa ja ei tarvitse kyselyn yksittäisiksi tehtäviksi.

Tallentaa työn tulosteen soittamalla [JobOutputStorage][net_joboutputstorage]. [SaveAsync] [net_joboutputstorage_saveasync] -menetelmä ja määritä [JobOutputKind] [ net_joboutputkind] ja tiedostonimi:

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Kun TaskOutputKind tehtävän tulostaa varten, jota käytät [JobOutputKind] [ net_joboutputkind] parametrin työn luokittelemista varten on samanlainen tiedostot. Tämä parametri avulla voit myöhemmin kyselyn (luettelo) tietynlaisia tulos. JobOutputKind tulostus- ja esikatselu tyypeistä ja tukee luomista mukautetut sisältötyypit.

### <a name="store-task-logs"></a>Tehtävän lokien tallentaminen

Lisäksi toteaa kestävät tallennustilan tiedoston, kun tehtävän tai projektin on valmis, voi olla helpompaa tarvittavat toistuvat tiedostot, jotka päivittyvät tehtävän--lokitiedostojen suorituksen aikana tai `stdout.txt` ja `stderr.txt`, kuten. Tähän tarkoitukseen Azure erä tiedoston nimeämiskäytännön kirjasto sisältää [TaskOutputStorage][net_taskoutputstorage]. [SaveTrackedAsync] [net_savetrackedasync] menetelmää. Kanssa [SaveTrackedAsync][net_savetrackedasync], voit seurata päivitykset solmu (aikaväli, joka määritetään)-tiedosto ja poistu päivitykset Azure-tallennustilan.

Valitse seuraavat koodikatkelman Käytämme [SaveTrackedAsync] [ net_savetrackedasync] päivittämään `stdout.txt` Azuren tallennustilaan 15 sekunnin välein, tehtävän suorituksen aikana:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
    await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)`on vain tunnus, jolla tehtävän normaalisti suorittaa paikkamerkki. Voit joutua esimerkiksi koodi Lataa Azuren tallennustilaan ja suorittaa sen muunnos tai laskutoimituksen. Tämä koodikatkelman tärkeä osa on osoittaa siitä, miten voit rivittää esimerkiksi koodi `using` estä tiedosto päivitetään säännöllisesti [SaveTrackedAsync][net_savetrackedasync].

`Task.Delay` On tehtävä tämä lopussa `using` Torju, jos haluat varmistaa, että solmu-agentti on aika tyhjentämään vakio ulos sisällön solmu (solmu-agentti on ohjelmaan, joka toimii kunkin ryhmän solmu ja tarjoaa solmu ja erä-palvelun välillä komento ja hallinta-käyttöliittymän) stdout.txt-tiedostoon. Tämä viipymättä on mahdollista tuloste viimeisen muutaman sekunnin jää huomaamatta. Tämä viive ei ehkä ole pakollinen kaikille tiedostoille.

>[AZURE.NOTE] Kun otat tracking SaveTrackedAsync tiedoston, vain seurattujen tiedoston *liittää* on samanlainen Azure-tallennustilan. Käytä tätä menetelmää vain, jos haluat seurata-kiertäminen lokitiedostojen tai tiedostot, jotka lisätään, toisin sanoen, tiedot lisätään vain tiedoston loppuun, kun se päivitetään.

## <a name="retrieve-output"></a>Noutaa tulostus

Kun haet pysyviä jaettavaan Azure erä tiedoston nimeämiskäytännön kirjaston käytöstä, tee niin tehtävän ja työ-keskitettyä tavalla. Voit pyytää tulosteen annetulle tehtävän tai projektin, eikä hänen nimeään tarvitse tietää polkunsa blob Storage tai jopa sen tiedostonimi. Voit jättää sanoa esimerkiksi "Anna minulle tehtävän *109*tulosteen tiedostojen."

Seuraavat koodikatkelman iteroivaa läpi kaikki projektin tehtävät, tulostaa tietoja tehtävän tulostus-tiedostoja ja lataa sen tiedostot säilöstä.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="task-outputs-and-the-azure-portal"></a>Tehtävän tulostus ja Azure-portaalia

Azure portaalin näyttää tehtävän tulostus ja lokit, joka on samanlainen linkitetyn Azure-tallennustilan tilin käyttämällä [Azure erä tiedoston nimeämiskäytännön Lueminut]-kohdan nimeämiskäytännöt[github_file_conventions_readme]. Voit ottaa käyttöön näiden nimeämiskäytännön itse sähköpostikansioon kielellä, tai voit käyttää tiedoston nimeämiskäytännön kirjaston .NET-sovelluksissa.

### <a name="enable-portal-display"></a>Ota käyttöön portaalin näyttö

Näyttää oman tulostaa portaalissa käyttöön on täytettävä seuraavat vaatimukset:

 1. [Linkki Azure-tallennustilan tilin](#requirement-linked-storage-account) erä-tiliisi.
 2. Ennalta määritetyn tallennustilan säiliöiden ja tiedostojen nimeämiskäytännöt noudattaa kun toteaa tulostaa. Voit tarkistaa nämä nimeämiskäytännön määritelmän tiedoston nimeämiskäytännön kirjaston [Lueminut-tiedosto][github_file_conventions_readme]. Jos käytät [Azure erä tiedoston nimeämiskäytännön] [ nuget_package] kirjaston jatkuvat tulosteen, tämä vaatimus täyttyy.

### <a name="view-outputs-in-the-portal"></a>Näytä tulostus-portaalissa

Jos haluat tarkastella tehtävän tulostus ja lokit Azure-portaalissa, siirry tehtävän jonka tulosteen ovat kiinnostuneita ja valitse sitten **tallennetut tiedostot** tai **tallennettu lokit**. Tämä kuva näyttää **tallennetut tiedostot** tehtävän tunnus "007":

![Tehtävän tulostaa sivu Azure-portaalissa][2]

## <a name="code-sample"></a>Koodiesimerkki

[PersistOutputs] [ github_persistoutputs] otoksen projekti on yksi [Azure erä MALLIKOODEJA] [ github_samples] GitHub käyttöön. Visual Studio 2015 ratkaisun esitellään, miten tehtävän tulosteen kestävät tallennustilan jatkuvat Azure erä tiedoston nimeämiskäytännön-kirjaston avulla. Voit suorittaa otosten, toimimalla seuraavasti:

1. Avaa projekti **Visual Studio 2015**.
2. Lisää Siirtoerän ja tallennustilaa **tilin tunnistetiedot** **AccountSettings.settings** Microsoft.Azure.Batch.Samples.Common Projectissa.
3. **Luominen** (mutta ei suoriteta) ratkaisu. Palauttaa kaikki NuGet pakettien pyydettäessä.
4. Käyttämällä Azure portal [sovelluspaketin](batch-application-packages.md) lataaminen **PersistOutputsTask**varten. Sisällytä `PersistOutputsTask.exe` ja sen riippuvaiset kokoonpanoja .zip-paketin Sovellustunnus, "PersistOutputsTask" ja "1.0-sovelluksen paketti-versiota.
5. **Aloittaminen** Projekti (Suorita) **PersistOutputs** .

## <a name="next-steps"></a>Seuraavat vaiheet

### <a name="application-deployment"></a>Sovelluksen käyttöönotto

Erän [sovelluksen pakettien](batch-application-packages.md) -toiminto tarjoaa helposti sekä ottaa käyttöön ja sovelluksia, jotka tehtävien suorittaa Laske solmujen versio.

### <a name="installing-applications-and-staging-data"></a>Sovellusten asentaminen ja väliaikainen tiedot

Tutustu [sovellusten asentaminen ja väliaikaisen erän tietojen laskemiseen solmujen] [ forum_post] Azure erä keskustelupalstalle yleiskatsaus erilaisia menetelmiä oman solmujen valmisteleminen suoritettavat tehtävät kirjataan. Kirjoitettu jollakin Azure erä työryhmän jäsenet, tämä viesti on hyvä askeleet eri tavoista, joilla voit siirtää tiedostojasi (mukaan lukien sovellusten ja tehtävän kenttään annettavat tiedot)-sivulle Laske-solmut ja seikat palauttamista varten.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Tallennetut tiedostot ja tallennettu lokit valitsimien-portaalissa"
[2]: ./media/batch-task-output/task-output-02.png "Tehtävän tulostaa sivu Azure-portaalissa"