<properties
    pageTitle="Tehtävien riippuvuudet Azure osalta | Microsoft Azure"
    description="Luo tehtävät, jotka ovat riippuvaisia muista tehtävistä käsittelyn MapReduce tyyli ja samalla big datasta onnistuminen työmääriä Azure osalta."
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
    ms.date="09/28/2016"
    ms.author="marsma" />

# <a name="task-dependencies-in-azure-batch"></a>Tehtävien riippuvuudet Azure osalta

Azure erän tehtävien riippuvuudet-ominaisuus on hyvin ratkaistavaan, jos haluat käsitellä:

- MapReduce tyyli työmääriä pilveen.
- Työt, jonka tietojen käsittely tehtävät voidaan esittää ohjattu asykliset graph (DAG).
- Muut jossa edeltävät tehtävät määräytyvät seuraavat tehtävät tulosteen työ.

Erän tehtäväriippuvuudet avulla voit luoda vasta, kun vähintään yksi tehtävä onnistuminen Laske solmuissa suorittamista varten Ajoitetut tehtävät. Voit esimerkiksi luoda työn, joka hahmontaa kehyksille 3D elokuvan erilliseen, rinnakkain tehtävien. Lopullisessa tehtävän--"Yhdistä tehtävän"--yhdistää hahmontamisen kehykset valmis filmin vasta, kun kaikki kehykset on tehty.

Voit luoda tehtäviä, jotka riippuvat yksi-yhteen- tai yksi-moneen-suhdetta muihin tehtäviin. Voit myös luoda kohtaa, johon tehtävään määräytyy tehtävätunnukset tietyn alueen tehtäviä ryhmän onnistuminen alueen riippuvuus. Voit yhdistää näissä kolmessa basic tilanteessa, jos haluat luoda monta-moneen-yhteyksiä.

## <a name="task-dependencies-with-batch-net"></a>Tehtävien riippuvuudet erä .NET kanssa

Tässä artikkelissa käsiteltävät aiheet määrittäminen tehtävien riippuvuudet käyttämällä [Erä .NET] [ net_msdn] kirjastoon. Olemme ensin Näytä voit projekteissa, [Ota käyttöön tehtäväriippuvuuden](#enable-task-dependencies) ja sitten [riippuvuussuhteessa tehtävän](#create-dependent-tasks)määrittämisestä. Lopuksi käsiteltävät aiheet [riippuvuuden skenaariot](#dependency-scenarios) , joka tukee erä.

## <a name="enable-task-dependencies"></a>Tehtävien riippuvuudet ottaminen käyttöön

Tehtävien riippuvuudet käyttämisestä erä-sovellus on on ensin näet erä-palvelun työn käyttää tehtävien riippuvuudet. Erän .NET-ottaa sen käyttöön oman [CloudJob] [ net_cloudjob] määrittämällä sen [UsesTaskDependencies] [ net_usestaskdependencies] -ominaisuuden arvoksi `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Edellisen koodikatkelman "batchClient" on [BatchClient] esiintymä[ net_batchclient] luokka.

## <a name="create-dependent-tasks"></a>Riippuvaiset tehtävien luominen

Voit luoda tehtävän, joka on vähintään yksi tehtävä onnistuminen on riippuvainen näet Siirtoerän, että tehtävän "riippuu" muihin tehtäviin. Määritä erä .NET [CloudTask][net_cloudtask]. [DependsOn] [net_dependson] -ominaisuutta, jolla esiintymän [TaskDependencies] [ net_taskdependencies] luokan:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Tämä koodikatkelman luo tehtävän "Kukka", jotka ajoitetaan voidaan suorittaa Laske solmu vasta, kun tehtävät, joiden tunnukset "Sademäärä" ja "Su" on suoritettu onnistuneesti tunnuksella.

 > [AZURE.NOTE] Tehtävän pidetään päättyy, kun se on **Valmis** -tilassa ja sen **exit-koodi** on `0`. Tämä tarkoittaa erä .NET [CloudTask][net_cloudtask]. [Tila] [net_taskstate] ominaisuuden arvon `Completed` ja CloudTask [TaskExecutionInformation][net_taskexecutioninformation]. [ExitCode] [net_exitcode] ominaisuuden arvo on `0`.

## <a name="dependency-scenarios"></a>Riippuvuus-skenaariot

Kolme tehtävän riippuvuuden mahdollista, joiden avulla voit Azure osalta: yksi-yhteen-yksi-moneen- ja Tehtävätunnus alueen riippuvuuden. Niitä voidaan yhdistää antamaan neljännen skenaarion monta-moneen.

 Skenaario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Esimerkki | |
 :-------------------: | ------------------- | -------------------
 [Yksi-yhteen](#one-to-one) | *taskB* määräytyy *taskA* <p/> ei *taskB* suunnitteilla suorittamisen, kunnes *taskA* onnistui | ![Kaavio: yksi-yhteen tehtäväriippuvuuden][1]
 [Yksi-moneen](#one-to-many) | *taskC* määräytyy *taskA* ja *taskB* <p/> ei *taskC* suunnitteilla suorittamisen, kunnes *taskA* ja *taskB* on suoritettu onnistuneesti | ![Kaavio: yksi-moneen-tehtäväriippuvuuden][2]
 [Tehtävän tunnus-alue](#task-id-range) | *taskD* määräytyy solualueen tehtävät <p/> ei *taskD* suunnitteilla suorittamisen, kunnes tehtävät, joiden tunnukset *1* – *10* on suoritettu | ![Kaavio: Tehtävän tunnus alueen riippuvuus][3]

>[AZURE.TIP] Voit luoda **monta-moneen** -yhteyksiä, kuten jossa C, D, E ja F kunkin tehtävät määräytyvät tehtävät A ja B. Tämä on hyödyllistä esimerkiksi parallelized esikäsittelyn tilanteissa, joissa edeltävät tehtävät määräytyvät sen tulosteesta useita seuraavat tehtävät.

### <a name="one-to-one"></a>Yksi-yhteen

Jos haluat luoda tehtävän, joka on toinen tehtävä onnistuminen riippuvuussuhteessa, sinun on annettava [TaskDependencies]yhden tehtävän Tunnusten[net_taskdependencies]. [OnId] [net_onid] staattinen menetelmä, kun olet täyttänyt [DependsOn] [ net_dependson] [CloudTask]-ominaisuuden[net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Yksi-moneen

Jos haluat luoda tehtävän, joka on useita tehtäviä suoritetaan valmiiksi, sinun on annettava tehtävätunnukset [TaskDependencies]kokoelma[net_taskdependencies]. [OnIds] [net_onids] staattinen menetelmä, kun olet täyttänyt [DependsOn] [ net_dependson] [CloudTask]-ominaisuuden[net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>Tehtävän tunnus-alue

Voit luoda tehtävän, jossa on riippuvuuden toisiinsa liittyviä tehtäviä suoritetaan valmiiksi, jonka tunnukset omistamiselle alueella toimittaa ensimmäisen ja tehtävän viimeksi [TaskDependencies]sivualueen tunnukset[net_taskdependencies]. [OnIdRange] [net_onidrange] staattinen menetelmä, kun olet täyttänyt [DependsOn] [ net_dependson] [CloudTask]-ominaisuuden[net_cloudtask].

>[AZURE.IMPORTANT] Kun käytät tehtävän tunnus alueiden riippuvuudet, tehtävätunnukset alue *on* olla merkkijonon esityksiä kokonaisluvuista. Lisäksi jokaisen tehtävän alueen on suoritettava onnistuneesti riippuvainen tehtävä voidaan ajoittaa suorittamisen.

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>Koodiesimerkki

[TaskDependencies] [ github_taskdependencies] otoksen projekti on yksi [Azure erä MALLIKOODEJA] [ github_samples] GitHub käyttöön. Visual Studio 2015 ratkaisun esitellään, miten tehtäväriippuvuuden projektin käyttöön, luoda tehtäviä, jotka riippuvat muihin tehtäviin ja suorittaa näiden tehtävien suorittaminen solmujen resurssivarantoon.

## <a name="next-steps"></a>Seuraavat vaiheet

### <a name="application-deployment"></a>Sovelluksen käyttöönotto

Erän [sovelluksen pakettien](batch-application-packages.md) -toiminto tarjoaa helposti sekä ottaa käyttöön ja sovelluksia, jotka tehtävien suorittaa Laske solmujen versio.

### <a name="installing-applications-and-staging-data"></a>Sovellusten asentaminen ja väliaikainen tiedot

Tutustu [sovellusten asentaminen ja väliaikaisen erän tietojen laskemiseen solmujen] [ forum_post] kirjaa yleisiä tietoja eri tavoista valmisteleminen oman solmujen tehtävien suorittamiseen Azure erä-keskustelupalstalle. Kirjoitettu jollakin Azure erä työryhmän jäsenet, tämä viesti on hyvä askeleet eri tavoista, joilla voit siirtää tiedostojasi (mukaan lukien sovellusten ja tehtävän kenttään annettavat tiedot)-sivulle Laske-solmut.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Kaavio: yksi-yhteen-riippuvuus"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Kaavio: yksi-moneen-riippuvuus"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Kaavio: tehtävän tunnus alueen riippuvuus"
