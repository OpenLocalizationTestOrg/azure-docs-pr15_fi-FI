<properties
    pageTitle="Projektin valmistelu ja uudelleenjärjestämisen osalta | Microsoft Azure"
    description="Työn tason valmistelu tehtävien avulla voit pienentää Azure erä Laske solmujen tiedonsiirto, ja vapauta solmu uudelleenjärjestäminen työn lopussa tehtävät."
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
    ms.date="09/16/2016"
    ms.author="marsma" />

# <a name="run-job-preparation-and-completion-tasks-on-azure-batch-compute-nodes"></a>Suorita valmistelu ja valmistuminen projektitehtävien Azure erä Laske solmuissa

 Azure erän työn edellyttää usein asennuksen joitakin lomakkeen ennen sen tehtäviä suoritetaan ja ylläpito jälkeinen projektin tehtävien jälkeen. Voit joutua lataamista yleisten tehtävien syöttötiedot Laske solmujen ja lataaminen Azuren tallennustilaan tehtävän siirtää tietoja, kun työ on valmis. **Työn valmistelu** ja **työn Vapauta** tehtävien avulla voit suorittaa nämä toiminnot.

## <a name="what-are-job-preparation-and-release-tasks"></a>Mitä ovat työn valmistelu ja vapauta tehtäviä?

Ennen kuin projektin tehtävät suoritetaan, valmistelu projektitehtävä suoritetaan kaikki Laske solmut ajoitetut vähintään yksi tehtävä. Kun työ on valmis, Vapauta projektitehtävä toimii kunkin solmun sarjassa, joka suorittaa vähintään yksi tehtävä. Tavallinen erätehtävien avulla voit määrittää komentorivi, joka käynnistyy, kun työn valmistelu tai Vapauta tehtävä on suoritettu.

Projektitehtävien valmistelu ja vapauta tarjoaa tutun erä tehtävän ominaisuudet, kuten tiedostojen lataaminen ([resurssitiedostojen][net_job_prep_resourcefiles]), laajennetut suorittamisen, mukautettu ympäristömuuttujat, suurin suorittamisen kesto, yritä määrä ja tiedoston säilytys aikaa.

Seuraavissa osissa opit käyttämään [JobPreparationTask] [ net_job_prep] ja [JobReleaseTask] [ net_job_release] [Erä .NET] löytynyt luokkia[ api_net] kirjastoon.

> [AZURE.TIP] Projektitehtävien valmistelu ja vapauta on hyötyä "jaettu resurssivarantoon" ympäristöissä, mitkä Laske solmujen resurssivarantoon jatkuu välillä projektin suoritetaan ja käytetään monta työtä.

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Milloin kannattaa käyttää projektin valmistelu ja vapauta tehtävät

Työn valmistelu ja vapauta projektitehtävien sopivat hyvin ratkaistavaan seuraavissa tilanteissa:

**Yleisten tehtävien tietojen lataaminen**

Ehdota edellyttävät usein syötteeksi projektin tehtävien tietojen joukkoa. Esimerkiksi päivittäin riskien analyysin laskelmissa market tiedot työ-kohtaisia, vielä Yleiset kaikki projektin tehtävät. Market tietojen usein useita gigatavun kokoisia, olisi ladataan Laske kunkin solmun vain kerran, minkä tahansa tehtävän, joka suoritetaan solmun voit käyttää sitä. Lataa tiedot kunkin solmun, ennen kuin työn suorittamisen käyttäjän muihin tehtäviin **valmistelu projektitehtävä** avulla.

**Poista työ ja tehtävän tulos**

"Jaettu resurssivarantoon"-ympäristössä, jossa ryhmän Laske solmut eivät ole menetelmiä töiden välillä, joudut ehkä poistaa projektin tietoja suoritetaan välillä. Voit joutua muuttamaan säästää levytilaa solmuissa tai täyttävät organisaation suojauskäytäntöjä. **Projektitehtävän vapauttaminen** avulla voit poistaa tiedot, jotka on valmistelu projektitehtävä ladata tai luoda tehtävän suorituksen aikana.

**Log säilytys**

Voit säilyttää kopion, joka luo tehtävien lokitiedostojen tai kaatumisen ehkä vedostiedostot, jotka on luotu epäonnistui-sovelluksissa. Käyttää **projektitehtävän vapauttaminen** tällaisissa tapauksissa pakkaaminen ja Lataa tämä tieto [Azuren tallennustilaan] [ azure_storage] tili.

>[AZURE.TIP] Toinen tapa lokit ja muut työn ja tehtävien jatkuu tulosteen tiedot ovat [Azure erä tiedoston nimeämiskäytännön](batch-task-output.md) -kirjasto.

## <a name="job-preparation-task"></a>Projektitehtävä valmistelu

Projektin tehtävien suorittaminen, ennen kuin erä suorittaa valmistelu projektitehtävä kunkin Laske solmun, joka on aikataulun mukaan saatava tehtävän suorittaminen. Oletusarvon mukaan erä palvelun odottaa valmistelu projektitehtävä voi viimeistellä, ennen kuin suoritat suorittamaan solmun Ajoitetut tehtävät. Voit kuitenkin määrittää palvelun ei odota. Jos solmu käynnistyy, valmistelu projektitehtävä suoritetaan uudelleen, mutta voit poistaa käytöstä myös tämän ongelman.

Projektitehtävä valmistelu suoritetaan vain solmujen, joka on aikataulun mukaan saatava tehtävän suorittaminen. Tällöin tarpeettomat suorittamisen valmistelu tehtävän siltä varalta, että solmu ei ole määritetty tehtävä. Näin voi käydä, kun tehtävien työn määrä on pienempi kuin resurssivarantoon näkyvien solmujen määrän. Se koskee myös, kun [samanaikaiset tehtävän suorittaminen](batch-parallel-node-tasks.md) on käytössä, jotka jättää joidenkin solmujen käyttämättömänä, jos tehtävien määrä on pienempi kuin mahdollista samanaikaisia tehtäviä yhteensä. Suorittamalla ei valmistelu projektitehtävä käyttämättömänä solmuissa, luomiseen vähemmän rahaa tiedonsiirtomaksuihin siirto.

> [AZURE.NOTE] [JobPreparationTask] [ net_job_prep_cloudjob] [CloudPool.StartTask] eroaa[ pool_starttask] siten, että JobPreparationTask suorittaa kunkin projektin alkuun olisi StartTask suorittaa vain, kun Laske solmun ensin yhdistää resurssivarantoon tai käynnistää.

## <a name="job-release-task"></a>Projektitehtävän vapauttaminen

Kun työ on merkitty valmiiksi, työn Vapauta tehtävä on suoritettu kunkin solmun sarjassa, joka suorittaa vähintään yksi tehtävä. Voit merkitä työn suoritetuksi Lopeta pyynnön. Erän palvelun asettaa työn *lopetetaan*, lopettaa liittyvän työn aktiivinen tai käynnissä olevat tehtävät ja suorittaa työn Vapauta tehtävä. Työn siirtyy *Valmis* -tilaan.

> [AZURE.NOTE] Työn poistamista suorittaa myös työn Vapauta tehtävä. Jos kuitenkin työ on jo päättynyt, Vapauta tehtävä on ei suorittamalla toisen kerran poistettua työn myöhemmin.

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Projektin prep ja vapauta erä .NET tehtävät

Käyttämään valmistelu projektitehtävä määrittää [JobPreparationTask] [ net_job_prep] objektin oman työn [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] ominaisuus. Alusta vastaavasti [JobReleaseTask] [ net_job_release] ja sen lisääminen oman työn [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] -ominaisuuden arvoksi määritetty projektin Vapauta tehtävä.

Valitse tämä koodikatkelman `myBatchClient` on [BatchClient]esiintymä[net_batch_client], ja `myPool` on aiemmin resurssivarantoon, erä tilin.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Kuten edellä mainittiin, Vapauta tehtävä on suoritettu, kun työ on lopetettu tai poistetaan. Lopeta työ kanssa [JobOperations.TerminateJobAsync][net_job_terminate]. Poista työ kanssa [JobOperations.DeleteJobAsync][net_job_delete]. Lopeta tavallisesti tai poistaa työn, kun sen tehtävät saadaan valmiiksi tai aikakatkaisu, jotka olet määrittänyt on saavutettu.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Valitse GitHub otoksen koodi

Lisätietoja työn valmistelu ja vapauta tehtävät-toiminto on Kuittaa ulos [JobPrepRelease] [ job_prep_release_sample] otoksen projektin GitHub. Tämä Konsolisovellus toimii seuraavasti:

1. Luo kaksi "pieni" solmujen resurssivarantoon.
2. Luo projektin työn valmistelu, Vapauta ja vakio tehtävät.
3. Suorittaa tehtävän työn valmistelu, joka kirjoittaa ensin Solmutunnus tekstitiedostoon solmun "jaettu-kansio.
4. Suorittaa tehtävän kunkin solmun, joka kirjoittaa sen Tehtävätunnus saman tekstitiedosto.
5. Kun kaikki tehtävät saadaan valmiiksi (tai aikakatkaisun saavutetaan) tulostaa kunkin solmun tekstitiedosto konsoliin sisällön.
6. Kun työ on valmis, suorittaa työn Vapauta tehtävä, voit poistaa tiedoston solmu.
6. Tulostaa lopetuskoodit valmistelu ja vapauta projektitehtävät kunkin solmun, johon ne on suoritettu.
7. Keskeyttää suorittamisen, jotta työ-ja/tai resurssivarantoon poiston vahvistaminen.

Esimerkki sovelluksen on seuraavanlainen:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] Vuoksi muuttujan luomista ja Käynnistä ajan uusi ryhmä (Joidenkin solmujen ovat valmiita tehtäviä, ennen kuin muiden) solmujen näkyviin voi tulla eri tulos. Tarkemmin sanottuna koska tehtävien suorittamista jokin ryhmän solmut voivat suorittaa kaikki projektin tehtävät. Jos näin tapahtuu, huomaat, että projektin valmistelu ja solmu, joka suorittaa tehtäviä ei ole release tehtävät.

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Tarkasta työn valmistelu ja vapauta tehtäviä Azure-portaalissa

Kun suoritat sovelluksen malli, voit käyttää [Azure portal] [ portal] työ ja sen tehtävien ominaisuuksien tarkasteleminen tai ladata jopa jaetun tekstitiedosto, joka on muutettu projektin tehtävät.

Seuraavassa näyttökuvassa näkyy **valmistelu tehtävät sivu** Azure-portaalissa malli-sovelluksen asennuksen jälkeen. Siirry *JobPrepReleaseSampleJob* ominaisuuksiin jälkeen tehtävät määrittänyt (mutta ennen kuin poistat työn ja resurssivarantoon) ja valitse **valmistelu tehtävät** tai **Release tehtävät** niiden ominaisuuksien tarkasteleminen.

![Työn valmistelu ominaisuuksien Azure-portaalissa][1]

## <a name="next-steps"></a>Seuraavat vaiheet

### <a name="application-packages"></a>Sovelluksen-paketit

Valmistelu projektitehtävä lisäksi voit käyttää myös erä [sovelluksen pakettien](batch-application-packages.md) -toiminnon suorittaminen solmujen valmisteleminen tehtävän suorittaminen. Tämä ominaisuus on hyödyllinen käyttöönoton sovelluksia, jotka eivät vaadi asennusohjelmaa, sovellukset, jotka sisältävät useita tiedostoja (100 +) tai sovellukset edellyttävät tarkka Versionhallinta.

### <a name="installing-applications-and-staging-data"></a>Sovellusten asentaminen ja väliaikainen tiedot

Tämä MSDN vastaukseksi esitetään yleiskatsaus useita eri tapoja tehtävien suorittaminen oman solmujen valmisteleminen:

[Sovellusten asentaminen ja väliaikaisen erän tietojen laskemiseen solmujen][forum_post]

Kirjoitettu jollakin Azure erä työryhmän jäsenet, se käsitellään, joiden avulla voit asentaa sovellukset ja tietojen laskemiseen solmujen useilla eri tavoilla.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
