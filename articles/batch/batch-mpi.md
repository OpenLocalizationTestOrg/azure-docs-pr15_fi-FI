<properties
    pageTitle="Suorita MPI sovellusten Azure osalta monen esiintymän tehtäviä | Microsoft Azure"
    description="Opi suorittamaan viestin kulkeva Interface (MPI) sovellusten monen esiintymän tehtävälajin Azure osalta."
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
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-azure-batch"></a>Usean esiintymän tehtävien avulla voit suorittaa viestin kulkeva Interface (MPI) sovellusten Azure osalta

Usean esiintymän tehtäviä mahdollistavat Azure erä-tehtävän suorittaminen samanaikaisesti useita Laske solmuissa. Seuraavien toimintojen ottaminen käyttöön tehokas tietojenkäsittely skenaarioita, kuten viestin kulkeva Interface (MPI) sovellusten osalta. Tässä artikkelissa kerrotaan siitä, kuinka monta esiintymää tehtävien [Erä .NET] [ api_net] kirjastoon.

>[AZURE.NOTE] Tämän artikkelin esimerkeissä keskitytään erä .NET-MS-MPI-ja Windows Laske solmujen, tässä käsitellyt monen esiintymän tehtävän käsitteitä sovelletaan muiden ympäristöjen ja -tekniikoiden (Python ja Intel MPI Linux solmuissa, esimerkiksi).

## <a name="multi-instance-task-overview"></a>Usean esiintymän tehtävien yleiskatsaus

Erän kunkin tehtävän on tavallisesti suorittaa yhteen Laske--solmu voit lähettää useita tehtäviä projektiin ja erä palvelun ajoittaa kunkin tehtävän suorittamista solmun varten. Kuitenkin määrittämällä tehtävän **usean esiintymän asetukset**näet erä ensisijainen tehtävällä ja useita alitehtäviä, joka suoritetaan sitten useita solmuissa luomiseen sen sijaan.

![Usean esiintymän tehtävien yleiskatsaus][1]

Lähettäessäsi tehtävän työn asetuksilla monen esiintymän erä suorittaa monen esiintymän tehtäviä yksilöllisiä useita vaiheita:

1. Erän-palvelu luo yksi **ensisijainen** ja useita **alitehtäviä** monen esiintymän asetusten perusteella. Tehtävien (ensisijainen kaikkien alitehtävien plus) kokonaismäärän vastaa lukumäärän **esiintymät** (Laske solmujen) määrität usean esiintymän-asetukset.
1. Erän määrittää jokin Laske solmut **perustyylin**ja ajoittaa ensisijainen tehtävän suorittamiseen dian perustyyliin. Kokouksen suorittamaan kohdistettu monen esiintymän tehtävään, yksi alitehtävien kohti solmu Laske solmujen loput alitehtävät.
1. Pääsivujen ja kaikkien alitehtävien Lataa **yleisiä resurssitiedostojen** määrität usean esiintymän-asetukset.
1. Yleisten resurssien jälkeen tiedostot on ladattu, ensisijaisen ja alitehtävien Suorita **yhteensovittamisesta komento** määrität usean esiintymän-asetukset. Tehtävän suorittaminen solmujen valmisteleminen käytetään yleensä yhteensovittamisesta-komento. Tämä sisältää käynnistetään taustan palveluita (kuten [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) ja tarkistaa, että solmut ovat valmiita Käsittele välisten solmu viestejä.
1. Ensisijainen tehtävä suorittaa **sovelluksen komento** perusmuodon solmu *jälkeen* yhteensovittamisesta-komento on suoritettu onnistuneesti pääsivujen ja kaikki alitehtävät. Sovelluksen-komento on monta esiintymää tehtävän itse komentorivi ja yksinomaan ensisijainen tehtävä on suoritettu. [MS-MPI]-[msmpi_msdn]-pohjainen ratkaisu, tämä on kohtaa, johon voit suorittaa MPI-sovelluksen käyttäminen `mpiexec.exe`.

> [AZURE.NOTE] Vaikka on toiminnallisesti eri, "monen esiintymän tehtävä" ei ole yksilöllisiä tehtävälajin, kuten [StartTask] [ net_starttask] tai [JobPreparationTask][net_jobprep]. Usean esiintymän tehtävä on vain vakio erätehtävän ([CloudTask] [ net_task] erä .NET-) monen esiintymän jonka asetukset on määritetty. Tässä artikkelissa viitataan näin **monen esiintymän tehtävän**.

## <a name="requirements-for-multi-instance-tasks"></a>Usean esiintymän tehtäviä vaatimukset

Usean esiintymän tehtävät edellyttävät resurssivarantoon **välisten solmu viestintä on otettu käyttöön**, ja **samanaikainen tehtävän suorittaminen käytöstä**. Jos yrität monen esiintymän tehtävän suorittaminen resurssivarantoon kanssa Solmuvälin viestintä on poistettu käytöstä tai *maxTasksPerNode* -arvo suurempi kuin 1, tehtävä ajoitetaan koskaan--se pysyy toistaiseksi "aktiivinen"-tilassa. Tämä koodikatkelman näyttää resurssivarannon käyttäminen erä .NET-kirjaston luominen.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

Lisäksi monen esiintymän tehtäviä voit suorittaa *vain* solmujen **jakavat luotu 14 joulukuuta 2015 jälkeen**.

### <a name="use-a-starttask-to-install-mpi"></a>Asenna MPI StartTask avulla

Suorittaa MPI sovelluksia monen esiintymän tehtävälle, sinun on MPI toteutus (MS-MPI tai Intel MPI, esimerkiksi) asennetaan ryhmän Laske solmujen. Tämä on hyvä käyttämään [StartTask][net_starttask], joka suoritetaan aina, kun solmu yhdistää resurssivarantoon tai käynnistetään. Tämä koodikatkelman Luo StartTask, joka määrittää MS-MPI asennuspaketin [resurssitiedoston][net_resourcefile]. Käynnistä tehtävän komentoriviltä suoritetaan, kun olet ladannut resurssitiedoston solmun. Tässä tapauksessa komentorivi suorittaa automaattisen asennuksen, MS-MPI.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Remote suoraan muistin käyttö (RDMA)

Kun valitset Laske solmujen [RDMA-yhteyttä hyödyntäviin kokoa](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) esimerkiksi A9 erä varannon, MPI-sovelluksen voit hyödyntää Azure on tehokas, pieni viive remote suoraan muistin access (RDMA) verkkoon.

Etsi koot määritettyä "RDMA pystyvät" seuraavissa artikkeleissa:

* **CloudServiceConfiguration** jakavat

  * [Koot pilvipalveluihin](../cloud-services/cloud-services-sizes-specs.md) (Vain Windows)

* **VirtualMachineConfiguration** jakavat

  * [Koot näennäiskoneiden Azure-tietokannassa](../virtual-machines/virtual-machines-linux-sizes.md) (Linux)

  * [Koot näennäiskoneiden Azure-tietokannassa](../virtual-machines/virtual-machines-windows-sizes.md) (Windows)

>[AZURE.NOTE] Voit hyödyntää RDMA [Linux Laske solmujen](batch-linux-nodes.md), sinun on käytettävä **Intel MPI** solmuissa. Lisätietoja CloudServiceConfiguration ja VirtualMachineConfiguration jakavat erä ominaisuus yleiskatsauksen kohdassa [resurssivarantoon](batch-api-basics.md#Pool) .

## <a name="create-a-multi-instance-task-with-batch-net"></a>Luo erä .NET usean esiintymä-tehtävän.

Nyt, että olet kattaa resurssivarantoon vaatimukset ja MPI paketin asennuksen, luodaan usean esiintymä-tehtävän. Tämä koodikatkelman on luoda vakio [CloudTask][net_task], määritä sen [MultiInstanceSettings] [ net_multiinstance_prop] ominaisuus. Kuten edellä mainittiin, monen esiintymän tehtävä ei ole eri tehtävälajia, mutta vakio erätehtävän määritetty usean esiintymän asetukset.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Ensisijainen tehtävä- ja alitehtäviä

Kun luot tehtävän monen esiintymän asetuksia, Määritä, joita voit suorittaa tehtävän suorittaminen solmujen määrän. Projektin tehtävän lähettäessäsi erä palvelun Luo yksi **ensisijainen** tehtävä ja tarpeeksi **alitehtävät** , jotka vastaavat määrittämiäsi solmujen määrän yhdessä.

Tehtävät määritetään 0 alueen kokonaislukutunnus *numberOfInstances* - 1. Tehtävä, jonka tunnus on 0 on ensisijainen tehtävä ja kaikki muut tunnukset ovat alitehtävät. Esimerkiksi jos luot tehtävän monen esiintymän seuraavia asetuksia, ensisijainen tehtävä on tunnus on 0 ja alitehtävät on 1 – 9 tunnukset.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Perusmuodon solmu

Usean esiintymän tehtävän lähettäessäsi erä-palvelu nimeää jokin Laske solmut "master-solmu ja ajoittaa ensisijainen tehtävän suorittamiseen perusmuodon solmu. Suorittaa loput monen esiintymän tehtävään varattu solmujen aikataulun alitehtävät.

## <a name="coordination-command"></a>Koordinointi-komento

**Koordinointi-komento** on suorittaa sekä ensisijaisen ja alitehtävät.

Koordinointi-komennon suorittaminen estää--erä ei suorita sovellus-komento, kunnes yhteensovittamisesta-komento on palautettu onnistuneesti kaikki alitehtävät. Koordinointi-komento olisi näin ollen Käynnistä tarvittavat taustan palvelut, varmista, että ne ovat käyttövalmiina ja sulje sitten. Esimerkiksi MS-MPI versio 7 SMPD-palvelu käynnistyy solmun ratkaisua yhteensovittamisesta komennon lopettaa sitten:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Huomaa, `start` koordinointi-komennolla. Tämä ei tarvita, sillä `smpd.exe` sovellus ei palauta välittömästi suorittamisen jälkeen. [Käynnistä] käyttämättä[ cmd_start] komennon yhteensovittamisesta Tämä komento ei palauta ja estää näin ollen sovellus-komennon suorittaminen.

## <a name="application-command"></a>Sovelluksen-komento

Kun ensisijainen tehtävä ja kaikkien alitehtävien muuttanut suoritettaessa yhteensovittamisesta-komentoa, monen esiintymän tehtävän komentoriviltä suoritetaan ensisijainen tehtävä *vain*. Olemme kutsu tämän **sovelluksen komento** erottaa yhteensovittamisesta-komento.

MS-MPI sovellusten komennolla sovelluksen suorittamaan MPI käyttävä sovelluksen mukana `mpiexec.exe`. Esimerkiksi näin sovelluksen-komennon avulla MS-MPI versio 7 ratkaisua:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] Koska MS-MPI's `mpiexec.exe` käyttää `CCP_NODES` muuttujan by default (katso [Ympäristömuuttujat](#environment-variables)) Esimerkki sovelluksen komentoriviltä yllä sen ulkopuolelle.

## <a name="environment-variables"></a>Ympäristömuuttujat

Erän Luo useita [Ympäristömuuttujat] [ msdn_env_var] tietyn monen esiintymän tehtävien suorittaminen solmuissa monen esiintymän tehtävään. Yhteensovittamisesta ja sovelluksen komentorivit voivat viitata muuttujia ympäristössä, kuten komentosarjojen ja he suorittavat ohjelmat voivat.

Seuraavat ympäristömuuttujat luodaan erä-palvelun käyttöä varten monen esiintymän tehtäviä:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Koko näissä ja muiden erä Laske solmu ympäristömuuttujien sekä niiden sisältö ja näkyvyys-lisätietoja [Laske solmu ympäristömuuttujat][msdn_env_var].

>[AZURE.TIP] Erän Linux MPI koodin otosten on esimerkki siitä, miten useita ympäristömuuttujia avulla voidaan. [Yhteensovittamisesta cmd] [ coord_cmd_example] Bash komentosarjan Lataa yleinen sovellus ja input Azuren tallennustilaan-tiedostoja, mahdollistaa verkon tiedostojärjestelmän (NFS) jaettuun perusmuodon solmun ja määrittää muut kuin NFS-asiakkaille monen esiintymän tehtävään varattu solmut.

## <a name="resource-files"></a>Resurssitiedostojen

On kaksi joukkoa resurssitiedostojen huomioitavia monen esiintymän tehtävien: **yhteiset Resurssitiedostot** , jotka *kaikki* tehtävät Lataa (molemmat ensisijainen ja alitehtävien), ja **resurssitiedostojen** usean esiintymän tehtävän itse, mikä *on ensisijainen* tehtävän Lataa.

Voit määrittää yhden tai useita **yleisiä resurssitiedostojen** tehtävän monen esiintymän asetuksia. Yleisten Resurssitiedostot ladataan [Azuren tallennustilaan](./../storage/storage-introduction.md) mukaan ensisijainen kunkin solmun **tehtävän jaettu kansio** ja kaikki alitehtävät. Voit käyttää tehtävän jaetun kansion sovelluksen ja koordinointi komento rivejä käyttämällä `AZ_BATCH_TASK_SHARED_DIR` ympäristömuuttuja. `AZ_BATCH_TASK_SHARED_DIR` Polku on sama kuin jokaisen solmun monen esiintymän tehtävään varattu, Näin voit jakaa yhden yhteensovittamisesta-komento on ensisijainen ja kaikki alitehtävät. Erän "Jaa" hakemiston etäkäyttöpalvelimen tasolla, mutta voit käyttää sitä ilmoittaa tai jakaa tavalla kerrottu ympäristön muuttujien Vihje pisteen.

Resurssitiedostot, jotka määrität monen esiintymän tehtävän itse ladataan tehtävän käytön hakemiston `AZ_BATCH_TASK_WORKING_DIR`, oletusarvoisesti. Kuten edellä, toisin kuin yleisiä resurssitiedostojen ensisijainen tehtävä Lataa resurssitiedostojen määritetty monen esiintymän tehtävän itse.

> [AZURE.IMPORTANT] Käytä aina ympäristömuuttujat `AZ_BATCH_TASK_SHARED_DIR` ja `AZ_BATCH_TASK_WORKING_DIR` viittaamaan näissä kansioissa komento riveillä. Älä yritä muodostaa polut manuaalisesti.

## <a name="task-lifetime"></a>Tehtävän elinaika

Elinkaaren ensisijainen tehtävä määrittää koko monen esiintymän tehtävän elinkaaren. Kun ensisijainen poistuu, voit lopettaa kaikki alitehtävät. Lopeta-koodi on ensisijainen, Lopeta tehtävä koodi ja käytetään vuoksi onnistumisesta tai epäonnistumisesta tehtävän uudelleen tarkoituksiin määrittämiseen.

Jos jokin alitehtävät epäonnistuu, sulkeminen ei ole nolla palautuksen koodi, esimerkiksi koko monen esiintymän tehtävän epäonnistuu. Usean esiintymän tehtävä on sitten keskeytettiin ja uudelleen, yritä raja ylöspäin.

Kun poistat monen esiintymän tehtävän, pääsivujen ja kaikki alitehtävät poistetaan myös erä-palvelu. Kaikki alitehtävän kansioiden ja niiden tiedostot poistetaan Laske-solmut aivan kuin Vakio tehtävän.

[TaskConstraints] [ net_taskconstraints] monen esiintymän tehtävän, kuten [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], ja [RetentionTime] [ net_taskconstraint_retention] ominaisuudet noudatetaan on vakio tehtävän ja koskevat pääsivujen ja kaikki alitehtävät. Jos haluat muuttaa [RetentionTime] [ net_taskconstraint_retention] ominaisuus, kun olet lisännyt monen esiintymän tehtävän projektille muutoksen otetaan käyttöön ainoastaan ensisijainen tehtävä. Kaikki alitehtävät jatkaa alkuperäisen [RetentionTime][net_taskconstraint_retention].

Laske-solmu viimeisimmät tehtäväluettelon kuvastaa alitehtävän tunnus, jos viimeisimmät tehtävä on osa monen esiintymän tehtävän.

## <a name="obtain-information-about-subtasks"></a>Saat tietoja alitehtävät

Saada alitehtävät tietoja käyttämällä erä .NET-kirjaston Soita [CloudTask.ListSubtasks] [ net_task_listsubtasks] menetelmällä. Tämä menetelmä palauttaa kaikkien alitehtävien tiedot ja tietoja Laske solmu, joka suorittaa tehtäviä. Nämä tiedot selviää kunkin alitehtävien pääkansion, ryhmän tunnus, nykyisestä tilasta, lopetuskoodi ja lisää. Voit käyttää näitä tietoja yhdessä [PoolOperations.GetNodeFile] [ poolops_getnodefile] tapa hankkia alitehtävien tiedostot. Huomaa, että tämä menetelmä ei palauta tietoja ensisijainen tehtävän (id 0).

> [AZURE.NOTE] Tarkoittavat, erä .NET menetelmiä, jotka toimivat monen esiintymän [CloudTask] [ net_task] itse koskevat *vain* ensisijainen tehtävä. Kun esimerkiksi soitat [CloudTask.ListNodeFiles] [ net_task_listnodefiles] menetelmä monen esiintymän tehtävän vain ensisijainen tehtävä tiedostot palautetaan.

Seuraavat koodikatkelman voit hankkia alitehtävien tietoja sekä tiedoston sisällön pyytämisestä solmujen, joina he suoritetaan.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Koodiesimerkki

[MultiInstanceTasks] [ github_mpi] koodi-GitHub esimerkissä käyttämisestä monen esiintymän tehtävän suorittamiseen [MS-MPI] [ msmpi_msdn] sovelluksen erän Laske solmujen. Noudattamalla [valmistelu](#preparation) ja [suorittamisen](#execution) mallin suorittamiseen.

### <a name="preparation"></a>Valmistelu

1. [Voit kääntää ja suorittaa yksinkertaisen MS-MPI ohjelman]kaksi ensimmäistä noudattamalla[msmpi_howto]. Tämä täyttää prerequesites seuraavassa vaiheessa.
1. Muodosta *julkaisuversion [MPIHelloWorld] * [ helloworld_proj] otoksen MPI ohjelma. Tämä on ohjelmaan, joka suoritetaan monen esiintymän tehtävän suorittaminen solmuissa.
1. Luo zip-tiedoston sisältävä `MPIHelloWorld.exe` (jonka voi laadittuihin vaiheessa 2) ja `MSMpiSetup.exe` (jonka latasit vaiheessa 1). Voit ladata tämän zip-tiedoston sovelluksen-pakettina seuraavassa vaiheessa.
1. Käytä [Azure portal] [ portal] haluat luoda erää- [sovelluksen](batch-application-packages.md) nimeltä "MPIHelloWorld" ja määrittää zip-tiedoston loit edellisessä vaiheessa sovelluspaketin "1.0-version. Lisätietoja on kohdassa [Lataa ja hallitse sovelluksia](batch-application-packages.md#upload-and-manage-applications) .

>[AZURE.TIP] Muodosta *jonkin julkaisuversion* `MPIHelloWorld.exe` niin, että sinun ei tarvitse lisätä muita riippuvuuksia (esimerkiksi `msvcp140d.dll` tai `vcruntime140d.dll`)-sovelluksen-paketti.

### <a name="execution"></a>Suorittaminen

1. Lataa [azure-erä-objektit] [ github_samples_zip] -GitHub.
1. Avaa MultiInstanceTasks- **ratkaisun** Visual Studio 2015. `MultiInstanceTasks.sln` Ratkaisutiedosto sijaitsee:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`

1. Anna Siirtoerän ja tallennustilaa tilin tunnistetiedot- `AccountSettings.settings` **Microsoft.Azure.Batch.Samples.Common** Projectissa.
1. **Luoda ja suorittaa** MultiInstanceTasks-ratkaisun suorittaminen MPI Esimerkki sovelluksen käyttöön Laske solmujen erä resurssivarantoon.
1. *Valinnainen*: [Azure portal] käyttäminen[ portal] tai [Erä Explorer] [ batch_explorer] tutkia otoksen resurssivarantoon, työn ja tehtävän ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask"), ennen kuin voit poistaa resurssit.

>[AZURE.TIP] Voit ladata [Visual Studio yhteisön] [ visual_studio] maksutta, jos sinulla ei ole Visual Studio.

Tulosteen `MultiInstanceTasks.exe` on seuraavanlainen:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Seuraavat vaiheet

- Microsoftin HPC ja Azure erä työryhmän blogissa kerrotaan [MPI tuki Linux Azure erän][blog_mpi_linux], ja se sisältää käyttöä [OpenFOAM] tietoja[ openfoam] erän. Voit etsiä Python MALLIKOODEJA [GitHub OpenFOAM-Esimerkki][github_mpi].

- Lisätietoja Azure erä MPI ratkaisujen käytettäviksi [luoda Linux Laske solmujen](batch-linux-nodes.md) .

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Usean esiintymän yleiskatsaus"
