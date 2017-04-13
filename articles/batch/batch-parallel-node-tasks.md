<properties
    pageTitle="Suurenna erä solmu käyttäminen rinnakkain tehtävät | Microsoft Azure"
    description="Suurentaa tehokkuutta ja käyttämällä kunkin solmun Azure erä varannon vähemmän Laske solmujen ja käyttöön samanaikaisia tehtäviä"
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
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="maximize-azure-batch-compute-resource-usage-with-concurrent-node-tasks"></a>Suurenna Azure erä Laske resurssien käyttö solmu samanaikaisia tehtäviä

Suorittamalla useita tehtäviä samanaikaisesti Azure erä-ryhmän Laske kukin solmu, voit suurentaa resurssien käyttö-ryhmän solmujen pienempi luku. Jotkin toiminnoista, tällöin lyhentää työn työajat ja pienempi kustannukset.

Joissakin tilanteissa osoitetaan kaikki solmun resurssit yhden tehtävän hyötyä, kun useita tilanteissa hyötyvät salliminen näiden resurssien jakamiseen useita tehtäviä:

 - **Pienentäminen tiedonsiirron** , kun tehtävät voivat jakaa tietoja. Tässä skenaariossa voit vähentää huomattavasti siirron tiedonsiirtomaksuihin kopioimalla jaettujen tietojen solmujen pienemmän määrän ja tehtävien suorittaminen rinnakkain kunkin solmun. Tämä koskee erityisesti, jos tiedot kopioidaan kunkin solmun on siirrettävä maantieteellisten alueiden välillä.

 - Kun tehtäviin on paljon muistia, mutta vain aikana lyhyt aika ja muuttujien aikoina suorituksen aikana **Maximizing muistinkäytön** . Voit käyttää käsittelemään tehokkaasti näiden piikkarit muistia vähemmän, mutta suurempi, Laske solmujen. Nämä solmut on useita tehtäviä, kukin solmu rinnakkain käytössä, mutta kunkin tehtävän hyödyntää sen solmujen vähäistä muistin eri aikoina.

 - **Pienentävät solmun numero rajoitukset** , kun välisten solmu viestintä tarvitaan varannon sisällä. Tällä hetkellä jakavat määritetty välisten solmu viestintä on oikeutettu enintään 50 Laske solmujen. Jos esimerkiksi resurssivarantoon kukin solmu on tehtävien suorittaminen rinnakkain, suurempi määrä tehtäviä voi suorittaa samanaikaisesti.

 - **Laske replikoiminen paikallisen klusterin**, esimerkiksi kun ensin siirryt Laske-ympäristön Azure. Jos nykyinen paikallisen ratkaisun suorittaa useita tehtäviä Laske solmu kohti, voit suurentaa solmu tehtävien peilikuvaksi tarkemmin, määritys enimmäismäärä.

## <a name="example-scenario"></a>Esimerkkitapaus

Esimerkkinä kuvaavat Rinnakkainen tehtävän suorittaminen edut oletetaan, että sovelluksesi tehtävän suorittimen ja muistin järjestelmävaatimukset ovat siten, että [Vakio\_D1](../cloud-services/cloud-services-sizes-specs.md#general-purpose-d) solmujen on riittävä. Mutta valmis työ tarvittavan ajan, jotta 1 000 nämä solmujen tarvitaan.

Sijaan Standard\_D1 solmut, joiden 1 suorittimen core, voit käyttää [Vakio\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) solmujen, joka on 16 Sydämiä ja ota käyttöön Rinnakkainen tehtävän suorittaminen. Siten voidaan käyttää *16 kertaa vähemmän solmujen* – 1 000 solmujen sijaan, vain 63 on pakollinen. Lisäksi, jos suuren sovelluksen tiedostoja tai viitetiedot tarvittavat kunkin solmun, työn keston ja tehokkuuden ovat uudelleen entistä paremmat jälkeen tiedot kopioidaan vain 16 solmujen.

## <a name="enable-parallel-task-execution"></a>Ota käyttöön Rinnakkainen tehtävän suorittaminen

Voit määrittää suorittaminen rinnakkain tehtävän suorittaminen solmut ryhmän tasolla. Erän .NET-kirjaston kanssa määrittää [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] ominaisuutta, kun luot resurssivarantoon. Jos käytössäsi on erän REST-Ohjelmointirajapinta, määrittää [maxTasksPerNode] [ rest_addpool] elementin pyynnön tekstissä ryhmän luomisen aikana.

Azure erä avulla voit määrittää enintään tehtäviä solmu kohden enintään neljä kertaa (4 x) solmu sydämiä määrä. Esimerkiksi jos altaan on määritetty solmut kokoa "Suuri" (neljä sydämiä), valitse `maxTasksPerNode` voi määrittää 16. Lisätietoja kunkin solmun koot sydämiä enimmäismäärä on artikkelissa [koot pilvipalveluihin](../cloud-services/cloud-services-sizes-specs.md). Lisätietoja palvelun rajoitukset on artikkelissa [kiintiön ja rajoitukset Azure erä-palveluun](batch-quota-limit.md).

> [AZURE.TIP] Ottaa huomioon, että `maxTasksPerNode` arvo, kun jälkeen [Automaattinen skaalaus-kaava] käyttää[ enable_autoscaling] lisääminen resurssivarantoon varten. Esimerkiksi kaava, joka palauttaa `$RunningTasks` tehtäviä kohden solmu suureneminen saattaa vaikuttaa huomattavasti. Lisätietoja on kohdassa [automaattisesti asteikko Laske solmujen Azure erä resurssivarantoon](batch-automatic-scaling.md) .

## <a name="distribution-of-tasks"></a>Jakautumisen tehtävät

Kun resurssivarantoon Laske solmujen voit suorittaa tehtäviä samanaikaisesti, on tärkeää Määritä, kuinka tehtävät altaan solmujen jaetaan.

Käyttämällä [CloudPool.TaskSchedulingPolicy] [ task_schedule] ominaisuutta, voit määrittää, että tehtävät määritetään yli kaikki solmut ("levittäminen") sarjassa tasaisesti. Tai voit määrittää, että mahdollisimman monta tehtävät määritetään kunkin solmun ennen tehtävät osoitetaan toisaalla resurssivarantoon ("pakkaus").

Esimerkki siitä, miten tämä ominaisuus on hyödyllinen, harkitse olevaa [Vakio\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) solmujen (yllä olevassa esimerkissä), joka on määritetty [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] arvo on 16. Jos [CloudPool.TaskSchedulingPolicy] [ task_schedule] on määritetty [ComputeNodeFillType] [ fill_type] *Pack*, se Suurenna kaikki 16 sydämiä kunkin solmun käyttö ja Salli [automaattisen skaalauksen poistaminen resurssivarantoon](batch-automatic-scaling.md) tyhjentää käyttämättömät solmujen varannon (solmujen ilman mitään tehtävät). Tämä pienentää resurssien käyttö ja rahaa.

## <a name="batch-net-example"></a>Erän .NET-Esimerkki

Tämä [Erä .NET] [ api_net] API koodikatkelman näkyvät pyynnön, joka sisältää neljä tehtäviä solmu kohden enintään neljä suuri solmujen resurssivarantoon luomiseen. Määrittää tehtävän ajoitusta, jotka täyttävät kunkin solmun tehtävien ennen tehtävien määrittäminen toisaalla altaan käytännön. Katso lisätietoja lisäämisestä jakavat käyttämällä erä .NET-Ohjelmointirajapinnan [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicated: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Erän REST-Esimerkki

Tämä [Erä muiden] [ api_rest] API koodikatkelman näkyy pyyntö luoda varannon, joka sisältää kaksi suuri solmujen enintään neljä tehtäviä solmu kohden. Katso lisätietoja lisäämisestä jakavat käyttämällä REST-Ohjelmointirajapinnalla [lisääminen resurssivarantoon tiliin][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicated":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [AZURE.NOTE] Voit määrittää `maxTasksPerNode` elementin ja [MaxTasksPerComputeNode] [ maxtasks_net] ominaisuuden vain resurssivarantoon luontiaikaa. Hän ei voi muokata, kun varanto on jo luotu.

## <a name="code-sample"></a>Koodiesimerkki

[ParallelNodeTasks] [ parallel_tasks_sample] GitHub projektin kuvaa [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] ominaisuus.

C# Konsolisovellus käyttää [Erä .NET] [ api_net] kirjaston luominen resurssivarantoon vähintään yksi Laske solmujen kanssa. Se suorittaa näiden solmuissa simuloida muuttujan kuormituksen määritettäviä monta tehtävää. Sovelluksen määrittää solmut suorittaa kunkin tehtävän. Sovellus on myös työn parametrit ja kesto. Kahden otoksen sovelluksen eri peräkkäisiä tulosteen yhteenveto osa näkyy alla.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Sovelluksen malli ensimmäinen suoritus osoittaa, että yksittäinen solmu altaan ja yksi tehtävä yhtä solmun oletusasetus kanssa työn kesto on yli 30 minuuttia.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

Toinen suorittaa malli näkyy vähentyä merkittävästi työn kesto. Tämä johtuu siitä altaan on määritetty neljä tehtäviä solmu, joka mahdollistaa Rinnakkainen tehtävän suorittaminen Viimeistele työn lähes vuosineljänneksen aika kohden.

> [AZURE.NOTE] Työn keston yllä tiivistelmät-Älä sisällytä resurssivarantoon luontiaikaa. Kunkin edellä työt toimitettiin aiemmin luodun jakavat, joiden suorittaminen solmujen on *vapaa* -tilaan esittämisen aikana.

## <a name="next-steps"></a>Seuraavat vaiheet

### <a name="batch-explorer-heat-map"></a>Erän Explorer Lämpökartta

[Azure erä Explorer][batch_explorer], jokin Azure erä [otoksen sovellusten][github_samples], sisältää *Lämpökartta* ominaisuus, joka sisältää visualisoinnin tehtävän suorittaminen. Kun olet suoritetaan [ParallelTasks] [ parallel_tasks_sample] sovelluksen malli voit tehdä Lämpökartta-ominaisuus visualisointi helposti kunkin solmun rinnakkain tehtävien suorittaminen.

![Erän Explorer Lämpökartta][1]

*Erän Explorer Lämpökartta resurssivarantoon neljä solmujen, kukin solmu suorittavien neljä tehtävien näyttäminen*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
