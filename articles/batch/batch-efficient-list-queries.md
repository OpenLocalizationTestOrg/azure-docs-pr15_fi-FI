<properties
    pageTitle="Tehokas kyselyitä Azure osalta | Microsoft Azure"
    description="Parantaa suorituskykyä suodattamalla kyselyissä, kun erä resurssit, kuten jakavat, projektien ja tehtävien tietoja ja laskea solmujen."
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

# <a name="query-the-azure-batch-service-efficiently"></a>Kyselyn Azure erä palvelun tehokkaasti

Tässä videossa kerrotaan niin, että Azure erä-sovelluksen suorituskykyä vähentämällä tietojen, joka palautetaan palvelu, kun kysely töitä, tehtäviä, ja Laske [Erä .NET] solmujen[ api_net] kirjastoon.

Lähes kaikki erän sovellukset on suoritettava jonkin valvonta tai toiminnon, joka tekee erä-palvelun usein säännöllisin väliajoin. Voit selvittää, onko jäljellä olevan työn jonossa olevia tehtäviä, on esimerkiksi saat tietoja työn jokaisen tehtävän. Tilan lisääminen resurssivarantoon solmujen määrittämiseksi saa tietoja kunkin solmun altaan. Tässä artikkelissa kerrotaan, miten voit suorittaa tällaisia kyselyjä mahdollisimman tehokkaasti.

## <a name="meet-the-detaillevel"></a>Täytä DetailLevel

Tuotannon erä sovelluksen kohteiden, kuten töitä, tehtäviä ja Laske solmujen voit numeroida tuhaterottimena. Kun pyydät näiden resurssien tietoja, mahdollisesti suuria määriä tietoa on "toimintojen koneisiin" erä-palvelusta sovelluksen kussakin kyselyssä. Voit suurentaa kyselyissä nopeuden ja näin ollen sovelluksesi suorituskykyä rajoittamalla kohteiden määrän ja kyselyn palauttamien tietojen tyypin.

Tämä [Erä .NET] [ api_net] API koodikatkelman näyttää *jokaisen* tehtävän, joka on liitetty työn, sekä *kaikki* kunkin tehtävän ominaisuudet:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Voit suorittaa luettelon huomattavasti tehokkaampi kyselyn kuitenkin lisäämällä kyselyyn "tiedot"tasolle. Voit tehdä tämän toimittamalla [ODATADetailLevel] [ odata] [JobOperations.ListTasks] objektin[ net_list_tasks] menetelmää. Tämä koodikatkelman palauttaa vain tunnus, komentoriviltä ja Laske solmu ominaisuudet valmiiden tehtävien:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

Tässä esimerkissä tilanteessa, jos tuhansia projektin tehtävien toisen kyselyn tuloksia yleensä palautetaan paljon nopeammin kuin ensimmäistä. Lisätietoja käyttämisestä ODATADetailLevel, kun kohteita erä .NET-Ohjelmointirajapinnan kanssa on sisällytetty [alapuolella](#efficient-querying-in-batch-net).

> [AZURE.IMPORTANT]
> Microsoft suosittelee, voit *aina* annettava ODATADetailLevel objektin .NET API luettelon soittaminen varmistaa parhaan mahdollisen tehokkuuden ja sovelluksen suorituskykyä. Määrittämällä yksityiskohtien määrä voi auttaa alemmalle tasolle erä palvelun vastausajat ja parantaa verkon käyttö pienentää muistinkäytön asiakassovelluksissa.

## <a name="filter-select-and-expand"></a>Suodata ja valitse Laajenna

[Erän .NET] [ api_net] ja [Erä Aseta] [ api_rest] API Anna voi pienentää luettelon palautettavien kohteiden määrä- ja palautetaan kunkin tietojen määrä. Voit tehdä määrittämällä **suodattimen**, **Valitse**ja **Laajenna merkkijonot** kyselyitä suoritettaessa.

### <a name="filter"></a>Suodatin
Suodatusmerkkijono on lauseke, joka vähentää palautettavien kohteiden määrän. Esimerkiksi luettelo vain käynnissä olevat tehtävät projektin tai luettelo vain Laske-solmut, jotka haluat suorittaa tehtäviä.

- Suodatusmerkkijono koostuu vähintään yksi lausekkeiden lauseke, joka koostuu ominaisuuden nimi, operaattori ja arvo. Ominaisuudet, jotka voidaan määrittää ovat kunkin kohteen tyyppi, kysely, kuten operaattorit, joita tuetaan kullekin ominaisuudelle.
- Loogisten operaattorien avulla voidaan yhdistää useita lausekkeita `and` ja `or`.
- Tässä esimerkissä suodatinluettelot merkkijonon vain suorittaminen "hahmonnetaan" tehtävät: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Valitse
Valitse merkkijonon rajaa, joka palautetaan kunkin kohteen ominaisuusarvoihin. Luettelon ominaisuuksien nimien määrittäminen ja vain ne ominaisuuksien arvot palautetaan kohteiden kyselyn tuloksissa.

- Valitse merkkijono koostuu ominaisuuksien nimiä CSV-luettelo. Voit määrittää ominaisuuksia ovat kyselyt kohdetyypin.
- Tässä esimerkissä Valitse merkkijono määrittää, että vain kolme ominaisuusarvoihin palauttaa kunkin tehtävän: `id, state, stateTransitionTime`.

### <a name="expand"></a>Laajenna
Laajenna merkkijonon vähentää API-kutsuja, joita tarvitaan tiettyjä tietoja. Laajenna-merkkijono käytettäessä lisätietoja kunkin kohteen saadaan yksi API puhelun. Sen sijaan, että luettelon kohteita, pyytää tietoja kunkin kohteen-luettelosta käyttämällä Laajenna merkkijonon voit palauttaa yhden API puhelun samat tiedot. Vähemmän API-kutsut tarkoittaa suorituskyvyn parantamiseksi.

- Valitse merkkijonon samalla, laajenna merkkijonon ohjaa sisältyykö tiettyjä tietoja luettelon kyselyn tuloksissa.
- Laajenna-merkkijono on tuettu vain, kun sitä käytetään työt, työaikatauluja, tehtävät ja jakavat luettelo. Tällä hetkellä se tukee vain tilastotiedot.
- Kun kaikki ominaisuudet ovat pakollisia ja valitse merkkijonoa ei ole määritetty, laajenna-merkkijono-, *on* käytettävä tilastotiedoista on artikkelissa. Jos valitse merkkijonoa käytetään hankkiminen alijoukkoa ominaisuudet, valitse `stats` voidaan määrittää select merkkijonon ja laajenna merkkijonon ei tarvitse erikseen.
- Tässä esimerkissä Laajenna merkkijono määrittää, että tilastotiedot palauttaa kunkin kohteen luettelosta: `stats`.

> [AZURE.NOTE] Luodessaan jokin kolmesta Kyselytyypit merkkijono (suodattaminen, valitse ja laajenna), sinun on varmistettava ominaisuuksien nimiä ja palvelupyynnön vastaa, niiden REST API-elementin vastineiden. Esimerkiksi kun käsittelet.NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) luokan, Määritä **tilan** sen sijaan, että **tila**, vaikka .NET-ominaisuus on [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Katso alla taulukoiden ominaisuuksien yhdistämismääritykset .NET ja REST API välillä.

### <a name="rules-for-filter-select-and-expand-strings"></a>Säännöt-suodattimen, valitse ja laajenna merkkijonoja

- Ominaisuuksien nimiä suodatin, valitse ja laajenna merkkijonot pitäisi näkyä tavalla kuin [Muiden erä] [ api_rest] API – myös silloin, kun käytät [Erä .NET] [ api_net] tai johonkin muita erä SDK: T.
- Kaikki ominaisuuksien nimien kirjainkoko on merkitsevä, mutta ominaisuuden arvot ovat kirjainkokoa.
- Päivämäärä ja kellonaika merkkijonojen voi olla jokin seuraavista muodoista ja täytyy olla edessä `DateTime`.

  - W3C DTF muodossa Esimerkki:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - RFC 1123 muodossa Esimerkki:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- Totuusarvo merkkijonot on joko `true` tai `false`.
- Jos ominaisuus ei kelpaa tai operaattori on määritetty, `400 (Bad Request)` aiheuttaa virheen.

## <a name="efficient-querying-in-batch-net"></a>Tehokas erä .NET kyselyt

[Erän .NET] sisällä[ api_net] API, [ODATADetailLevel] [ odata] luokkaa käytetään antamisesta suodatin ja valitse Laajenna merkkijonojen luettelon toiminnot. ODataDetailLevel-luokka on kolme julkisen merkkijono-ominaisuudet, jotka on määritetty konstruktorissa tai määrittää suoraan objekti. Valitse välitetään ODataDetailLevel objektin parametrina eri luettelo toiminnoista, kuten [ListPools][net_list_pools], [ListJobs][net_list_jobs], ja [ListTasks][net_list_tasks].

- [ODATADetailLevel][odata]. [FilterClause] [ odata_filter]: Palautettavien kohteiden määrän rajoittaminen.
- [ODATADetailLevel][odata]. [SelectClause] [ odata_select]: Määritä ominaisuusarvojen tulee kopioitava näkymä, jossa kukin kohde.
- [ODATADetailLevel][odata]. [ExpandClause] [ odata_expand]: Noutaa tietoja yksittäisen API kaikkien kohteiden Soita sijaan erillisessä puhelut kunkin kohteen.

Seuraavat koodikatkelman käyttää erä .NET-Ohjelmointirajapinnan kyselyn tehokkaasti jakavat tietyt tilastotietoja erä palvelu. Tässä skenaariossa erä käyttäjällä on numero- ja tuotannon jakavat. Testi-ryhmän tunnukset ovat etuliite "testi" ja tuotannon resurssivarantoon tunnuksista on etuliite "tuot". Koodikatkelman *myBatchClient* on oikein valmisteltu esiintymän [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) -luokka.

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [AZURE.TIP] Esiintymän [ODATADetailLevel] [ odata] , joka on määritetty Valitse ja laajenna lauseet voi myös välittää tarvittaessa Get menetelmiä, kuten [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), voit rajoittaa palautetut tiedot.

## <a name="batch-rest-to-net-api-mappings"></a>Erän LOPUT .NET API yhdistämismääritykset

Ominaisuuksien nimiä suodattimen, valitse ja laajenna merkkijonot *on* vastattava niiden REST API-vastineiden sekä nimi-ja kirjainkoko. Seuraavissa taulukoissa on .NET ja REST API-vastineiden välisten yhdistämismääritysten.

### <a name="mappings-for-filter-strings"></a>Suodatin-merkkijonojen yhdistäminen

- **.NET luettelon menetelmiä**: Kukin sarake .NET API esitetyssä hyväksyy [ODATADetailLevel] [ odata] parametrina objekti.
- **REST-luettelosta pyynnöt**: linkitetty tämän sarakkeen kunkin REST API-sivu sisältää taulukon, jonka ominaisuuksia ja toimintoja, jotka voivat määrittää *suodattimen* merkkijonoja. Näiden ominaisuuksien nimiä ja toiminnot käytetään silloin, kun voit käyttää [ODATADetailLevel.FilterClause] [ odata_filter] merkkijono.

| .NET luettelon menetelmät | REST-luettelosta pyynnöt |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [Luettelo-tilin varmenteet][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [Luettelo tehtävään liittyvät tiedostot][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [Luettelo työn valmistelu ja projektitehtävät release projektin tila][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [Työt-tilin luettelo][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [Solmun tiedostojen luettelo][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [Luettelo työhön liittyvät tehtävät][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [Tilin työaikatauluja luettelo][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [Projektin aikataulun liittyvät työt luettelo][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [Laske solmujen resurssivarantoon luettelo][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [Tilin jakavat luettelo][rest_list_pools]

### <a name="mappings-for-select-strings"></a>Valitse merkkijonojen yhdistäminen

- **Erän .NET tyypit**: erä .NET API tyyppejä.
- **REST API kohteiden**: tämän sarakkeen jokainen sivu sisältää yhdestä tai useasta taulukosta, jotka näyttävät tyyppi REST API-ominaisuuksien nimiä. Näiden ominaisuuksien nimiä käytetään, kun *Valitse* merkkijonot muodosta. Käytät samaa näiden ominaisuuksien nimiä, kun voit käyttää [ODATADetailLevel.SelectClause] [ odata_select] merkkijono.

| Erän .NET tyypit | REST API yksiköt |
|---|---|
| [Varmenne][net_cert] | [Varmennetta koskevien tietojen hakeminen][rest_get_cert] |
| [CloudJob][net_job] | [Tietoja työn][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [Projektin aikataulun koskevien tietojen hakeminen][rest_get_schedule] |
| [ComputeNode][net_node] | [Solmun koskevien tietojen hakeminen][rest_get_node] |
| [CloudPool][net_pool] | [Tietoja resurssivarantoon][rest_get_pool] |
| [CloudTask][net_task] | [Tehtävän koskevien tietojen hakeminen][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Esimerkki: käyttää suodatinmerkkijono

Kun Suodatinmerkkijonon muodosta, [ODATADetailLevel.FilterClause][odata_filter], kuulla edellä olevan taulukon "suodattimen merkkijonojen yhdistämismääritykset-kohdassa Etsi REST-Ohjelmointirajapinnalla ohjeet-sivulle, joka vastaa luettelo-toiminto, jonka haluat suorittaa. Löydät suodatettavia ominaisuudet ja niiden tuetut operaattorit ensimmäisen sivun Monirivinen taulukon. Jos haluat noutaa kaikki tehtävät Lopeta jonka tunnus on erisuuri kuin nolla, esimerkiksi [työhön liittyvät tehtävät] -luettelon rivi[ rest_list_tasks] määrittää ominaisuusruutua merkkijonon ja sallitun operaattoreita:

| Ominaisuus | Sallitut toiminnot | Tyyppi |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

Näin ollen luettelon kaikki tehtävät, joilla ei ole nolla lopetuskoodin Suodatusmerkkijono on seuraava:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Esimerkki: Muodosta Valitse merkkijono

Jos haluat käyttää [ODATADetailLevel.SelectClause][odata_select]ja taulukon yläpuolella "Valitse merkkijonojen yhdistämismääritykset-kohdassa REST API-sivulle, joka vastaa yritys, joka on luettelon tyyppi. Löydät valittavissa olevat ominaisuudet ja niiden tuetut operaattorit ensimmäisen sivun Monirivinen taulukon. Jos haluat hakea vain tunnus ja komentoriviltä luettelon kunkin tehtävän, esimerkiksi huomaat nämä rivit käytettävissä taulukossa [tehtävän tietoja][rest_get_task]:

| Ominaisuus | Tyyppi | Huomautuksia |
| :--- | :--- | :--- |
| `id` | `String` | `The ID of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

Valitse merkkijono, kuten tunnuksen ja komentoriviltä luettelossa kuhunkin tehtävään sitten on seuraava:

`id, commandLine`

## <a name="code-samples"></a>MALLIKOODEJA

### <a name="efficient-list-queries-code-sample"></a>Tehokas kyselyitä koodi malli

Tutustu [EfficientListQueries] [ efficient_query_sample] otoksen projektin GitHub nähdäksesi, kuinka tehokas luettelon kyselyt voi vaikuttaa suorituskykyyn-sovelluksessa. C# konsolisovelluksen Luo ja lisää paljon tehtäviä projektiin. Tämän jälkeen se on useita puhelut [JobOperations.ListTasks] [ net_list_tasks] menetelmä ja siirtoja [ODATADetailLevel] [ odata] objekteja, jotka on määritetty palautetaan tietojen määrä vaihtelee eri ominaisuusarvoihin. Se tuottaa tulosteen seuraavankaltaiselta:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Kulunut ajat esitetyllä kyselyn vastaus kertaa voit pienentää huomattavasti rajoittamalla ominaisuudet ja palautettavien kohteiden määrä. Voit tarkistaa tämän ja muiden otoksen projektien [azure-erä-objektit] [ github_samples] säilöön-GitHub.

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics kirjasto ja koodi-Esimerkki

EfficientListQueries-koodin esimerkin edellä, voit etsiä [BatchMetrics] [ batch_metrics] projektin [azure-erä-objektit] [ github_samples] GitHub säilöön. BatchMetrics otoksen projektin esitellään, miten voit valvoa tehokkaasti Azure erän työn edistymistä erä Ohjelmointirajapinnan käyttäminen.

[BatchMetrics] [ batch_metrics] otoksen sisältyy .NET luokan kirjaston projektin, jotka voit sisällyttää oman projektien ja yksinkertainen komentorivin ohjelma on syytä olla ja kuvaavat kirjastoon.

Projektin sovelluksen malli esittelee seuraavat toimenpiteet:

1. Jotta voit ladata vain ominaisuudet, sinun on erikoisominaisuuksia valitseminen
2. Jotta voit ladata vain muutokset viimeisen kyselyn jälkeen tilan siirtymä kertaa suodattaminen

Esimerkiksi seuraavalla tavalla näkyy BatchMetrics-kirjastossa. Se palauttaa ODATADetailLevel, joka määrittää, että vain `id` ja `state` kohteiden tehdään kysely on saatava ominaisuudet. Se myös määrittää, että vain yritykset, jonka tila on muuttunut määritetyn `DateTime` parametrin palautetaan.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Seuraavat vaiheet

### <a name="parallel-node-tasks"></a>Rinnakkaisia solmu tehtävät

[Suurenna Azure erä Laske samanaikainen solmu tehtävien resurssien käyttö](batch-parallel-node-tasks.md) on toinen erä sovelluksen suorituskykyyn liittyvä artikkeli. Tietyntyyppiset työmääriä voi olla hyötyä suorittaminen rinnakkain tehtävät suuremman--mutta vähemmän--Laske solmujen. Tutustu [Esimerkkitapaus](batch-parallel-node-tasks.md#example-scenario) on artikkelissa Lisätietoja näiden tilanne.

### <a name="batch-forum"></a>Erän keskustelupalsta

[Azure erä keskustelupalstalla] [ forum] MSDN-sivuston on kätevä keskustella Siirtoerän ja esittää kysymyksiä tietoja. Pää-päälle hyödyllisiä "muistilappuja" pylväiden ja lähettää kysymyksiä, kun ne aiheutuvat aikana voit luoda erää ratkaisuja.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

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

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
