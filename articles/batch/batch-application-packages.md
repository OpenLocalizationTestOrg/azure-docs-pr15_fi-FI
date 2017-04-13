<properties
    pageTitle="Helpon asennus- ja Azure erä hallinnan | Microsoft Azure"
    description="Käytä sovelluksen pakettien ominaisuutta Azure erän voit hallita useita sovelluksia ja erä asennuksen versio Laske solmujen."
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

# <a name="application-deployment-with-azure-batch-application-packages"></a>Sovelluksen-ympäristö, jonka Azure erä sovelluksen pakettien

Azure erän sovelluksen pakettien-toiminto tarjoaa tehtävän sovellusten helposti hallinnointi ja niiden käyttöönottoa Laske solmujen lisääminen resurssivarantoon. Sovelluksen paketteja, jossa voit ladata ja useita versioita suorittaa tehtäviä, mukaan lukien niiden tukitiedostot sovellusten hallinta. Voit sitten automaattisesti otetaan käyttöön yhden tai useamman Laske solmut näiden sovellusten lisääminen resurssivarantoon.

Tässä artikkelissa kerrotaan, kuinka ladataan ja hallita sovelluksen pakettien Azure-portaalissa. Opit sitten asentamisesta ryhmän Laske solmujen [Erä .NET] [ api_net] kirjastoon.

> [AZURE.NOTE] Tässä kuvattuja sovelluksen pakettien ominaisuus ohittaa käytettävissä aiemmissa versioissa palvelun "erä sovellukset-ominaisuus.

## <a name="application-package-requirements"></a>Sovelluksen pakkauksen vaatimukset

[Linkki Azure-tallennustilan tilin](#link-a-storage-account) on käyttää sovelluksen pakettien erä-tiliisi.

Tässä artikkelissa kuvatut sovelluksen pakettien ominaisuus on yhteensopiva *vain* erä jakavat, jotka on luotu 10 maaliskuussa 2016 kanssa. Sovelluksen pakettien ei oteta käyttöön laskemiseen solmujen jakavat, jotka on luotu ennen tätä päivää.

Tämä ominaisuus otettiin käyttöön [Erä REST API] [ api_rest] 2015 – 12-01.2.2-versio ja vastaavan [Erän .NET] [ api_net] kirjaston versio 3.1.0. On suositeltavaa, että käytät API uusin aina, kun käsittelet erä.

> [AZURE.IMPORTANT] Tällä hetkellä vain *CloudServiceConfiguration* jakavat tukea sovelluksen paketit. Et voi käyttää sovelluksen pakettien jakavat luotu VirtualMachineConfiguration kuvia. Katso lisätietoja kahdesta eri määritykset, [säännöstä Linux Laske Azure erä jakavat solmujen](batch-linux-nodes.md) [virtuaalikoneen Tietolähdemääritykset](batch-linux-nodes.md#virtual-machine-configuration) -osasta.

## <a name="about-applications-and-application-packages"></a>Sovellusten ja sovelluksen-paketit

Azure erä sisällä *sovelluksen* viittaa versiotietoja binaaritiedostot voi automaattisesti ladata lisääminen resurssivarantoon Laske solmujen joukko. *Sovelluspaketin* binaaritiedostot *tiettyjä* viittaa, ja se on tietyn *version* sovelluksen.

![Sovellusten ja sovelluksen pakettien korkean tason kaavio][1]

### <a name="applications"></a>Sovellukset

Sovelluksen osalta on yksi tai Lisää sovellus pakkaa ja määrittää sovelluksen asetukset. Sovelluksen voit esimerkiksi määrittää oletusarvoinen sovelluksen paketin versio asennetaan Laske solmujen ja onko sen paketit voidaan päivittää tai poistaa.

### <a name="application-packages"></a>Sovelluksen-paketit

Sovelluksen-paketti on .zip-tiedosto, joka sisältää sovelluksen binaaritiedostot ja tukitiedostot, jotka ovat mukaan tehtävien suorittaminen edellyttää. Kunkin sovelluspaketin edustaa sovelluksen tietyn version.

Voit määrittää sovelluksen pakettien resurssivarantoon ja tehtävän tasolla. Voit määrittää vähintään yksi näistä paketeista sekä (valinnainen) versio, kun olet luonut ryhmän tai tehtävän.

* **Resurssivarannon sovelluksen pakettien** otetaan käyttöön *kunkin* ryhmän solmun. Sovellukset on otettu käyttöön, kun solmu yhdistää resurssivarantoon ja kun se on käynnistettävä tai reimaged.

    Resurssivarannon sovelluksen paketit ovat sopivia, kun kaikki solmut resurssivarantoon Suorita projektin tehtävät. Voit määrittää vähintään yksi sovellus-pakettien, kun luot resurssivarantoon, ja voit lisätä tai päivittää aiemmin resurssivarantoon paketteja. Jos päivität olemassa olevan ryhmän sovelluksen pakettien, sinun on käynnistettävä sen solmujen Asenna uusi paketti.

* **Tehtävän sovelluksen paketit** ovat käyttöön vain Laske solmu ajoittanut tehtävän juuri, ennen kuin suoritat tehtävän komentoriviltä. Jos määritetty sovelluspaketin ja versio on jo solmu, se ei ole myymälät ja pakettiin käytetään.

    Tehtävän sovelluksen paketit ovat hyötyä jaettu resurssivarantoon ympäristöissä, jossa eri työt suoritetaan yhteen resurssivarantoon ja varannon ei poisteta, kun työ on valmis. Jos työsi on pienempi kuin solmujen tehtäviä ryhmän, tehtävän sovelluksen pakettien pienentää tiedonsiirron jälkeen sovellus otetaan käyttöön ainoastaan solmut, joka suorittaa tehtäviä.

    Muita tilanteita, joissa voit hyötyä tehtävän sovelluksen paketit ovat työt, jotka käyttävät erityisen suuren sovelluksen, mutta vain vähän tehtävät. Esimerkiksi valmiiksi käsittely vaihe tai Yhdistä tehtävän, missä vanhat käsittely tai yhdistämisen sovelluksen painavat.

> [AZURE.IMPORTANT] Ei rajoituksia sovellukset ja erä tilin sekä suurin sovelluksen paketin koko sovelluksen pakettien määrän. Artikkelissa [kiintiön ja Azure erä-palvelun rajoitukset](batch-quota-limit.md) lisätietoja nämä raja-arvot.

### <a name="benefits-of-application-packages"></a>Sovelluksen pakettien edut

Sovelluksen pakettien voit yksinkertaistaa erä-ratkaisun koodi ja alemmalle tasolle ja tarvittavien tehtävien suoritettavien sovellusten hallinta.

Voit määrittää yksittäisen resurssitiedostojen solmut asennetaan pitkä luettelo ei ole lisääminen resurssivarantoon Käynnistä tehtävä. Sinun ei tarvitse manuaalisesti Versionhallinta useita sovelluksen tiedostojen Azuren tallennustilaan tai oman solmuissa. Ja sinun ei tarvitse huolehtia luodaan tiedostojen tallennustila-tilisi pääsy [SAS URL-osoitteet](../storage/storage-dotnet-shared-access-signature-part-1.md) . Erän toimii taustalla Azuren tallennustilaan sovelluksen pakettien tallentamiseen ja ota ne laskemiseen solmujen kanssa.

## <a name="upload-and-manage-applications"></a>Lataa ja sovellusten hallinta

Voit käyttää [Azure portal] [ portal] tai hallita erä tilisi sovelluksen-pakettien [Erä hallinta .NET](batch-management-dotnet.md) -kirjasto. Seuraava muutamia osista on ensin tallennustilan asiakkaan linkittäminen sitten lisätään sovelluksia ja pakettien ja niiden hallinta-portaalissa.

### <a name="link-a-storage-account"></a>Tallennustilan asiakkaan linkittäminen

Jos haluat käyttää sovelluksen paketteja, linkitä ensin Azure-tallennustilan tilin erä-tiliisi. Jos et ole vielä määrittänyt tallennustilan tilin erä tilissäsi, Azure portaalin näkyy varoituksen ensimmäistä kertaa valitset **erä tili** -sivu **sovelluksia** -ruutu.

> [AZURE.IMPORTANT] Erän tällä hetkellä tukee *vain* **yleisiä tarkoitus** tallennustyyppi tilin vaiheessa 5, [Luo tallennustilan tili](../storage/storage-create-storage-account.md#create-a-storage-account), [tietoja Azure](../storage/storage-create-storage-account.md)-tallennustilan tilin kuvatulla tavalla. Kun linkität Azure-tallennustilan tilin erä tiliisi, linkki *vain* **yleisiä tarkoitus** -tallennustilan tilin.

![Tallennustilan tiliä ei ole määritetty varoitus Azure-portaalissa][9]

Erän-palvelu käyttää liittyvän tallennustilan tilin tallennus-ja sovelluksen pakettien hakemista. Kun olet linkittänyt kahdelle tilille, erä automaattisesti voit ottaa käyttöön Laske solmujen liitetyn tallennustilan tilin tallennetuista pakettien. Valitse **Varoitus** -sivu- **tallennustilan tilin asetukset** ja valitse sitten **Tallennustilan tilin** -tallennustilan tilin linkittäminen erä tilisi **Tallennustilan tili** -sivu.

![Valitse tallennustilan tilin sivu Azure-portaalissa][10]

Suosittelemme, että voit luoda tallennustilan tilin *erityisesti* käytettäväksi erä-tilisi ja valitse tähän. Lisätietoja tallennustilan tilin luomisesta on artikkelissa-tallennustilan tilin luominen" [tietoja Azure](../storage/storage-create-storage-account.md)-tallennustilan tilin. Kun olet luonut tallennustilan tilin, voit sitten linkittää se erä-tiliisi käyttämällä **Tallennustilan tili** -sivu.

> [AZURE.WARNING] Koska erän käytetään Azuren tallennustilaan tallentaa sovelluksen-paketteja, olet [veloitetaan normaaliin] [ storage_pricing] estä blob tiedoille. Muista voit kokoa ja sovelluksen pakettien määrä ja poistaa säännöllisesti poistetuista paketteja Pienennä kustannukset.

### <a name="view-current-applications"></a>Nykyinen näkymä-sovellukset

Voit tarkastella sovellukset erä-tilisi valitsemalla vasemmanpuoleisessa valikossa **sovellukset** -valikkovaihtoehto tarkasteltaessa **erä tili** -sivu.

![Sovelluksia-ruutu][2]

Tämä avaa **sovellukset** -sivu:

![Luettelon sovellukset][3]

**Sovellukset** -sivu näyttää kunkin sovelluksen tunnus tilisi ja seuraavat ominaisuudet:

* **Pakettien**--versioiden liittyvät tämän sovelluksen kanssa.
* **Oletusversio**--oleva versio asennetaan, jos et määritä versio resurssivarantoon hakeminen määritettäessä. Tämä asetus on valinnainen.
* **Salli päivitykset**– arvo, joka määrittää, onko paketti päivittää, poistot ja lisäykset sallitaan. Jos tämä on määritetty **ei**-paketin päivitykset ja poistot poistetaan käytöstä sovelluksen. Vain uuden sovelluksen paketin versioita voidaan lisätä. Oletusarvo on **Kyllä**.

### <a name="view-application-details"></a>Näytä hakemuksen tiedot

Valitse **sovellukset** -sivu sovelluksen Avaa sivu, joka sisältää kyseisen ohjelman tiedot.

![Hakemuksen tiedot][4]

Voit määrittää seuraavat asetukset sovelluksen sovelluksen tiedot-sivu.

* **Salli päivitykset**– Määritä, onko sen sovelluksen paketit voidaan päivittää tai poistaa. Katso tämän artikkelin "Päivitä tai poista sovelluspaketin".
* **Oletusversio**– Määritä oletusarvoinen sovelluspaketin, ottamaan solmujen laskemiseen.
* **Näyttönimi**– Määritä "friendly" nimi, jonka erä ratkaisu käyttää, kun näyttöön tulee tietoja sovellus, kuten palvelu, joka antaisit asiakkaillesi kautta erä käyttöliittymässä.

### <a name="add-a-new-application"></a>Lisää uusi sovellus

Jos haluat luoda uuden sovelluksen, Lisää sovellus-paketti ja määritä uuden, yksilöllisen sovelluksen on. Ensimmäinen sovelluspaketin, voit lisätä uuden sovelluksen tunnuksella myös Luo uusi sovellus.

Valitse **Lisää** **Uusi sovellus** -sivu Avaa **sovellukset** -sivu.

![Uuden sovelluksen sivu Azure-portaalissa][5]

**Uusi sovellus** -sivu sisältää seuraavat kentät uuden sovelluksen ja sovelluspaketin-asetusten määrittämiseen.

**Tunnus**

Tämä kenttä määrittää uuden sovelluksen, eli Vakio Azure erän tunnus kelpoisuussääntöjen tunnus:

* Voi olla mikä tahansa yhdistelmä aakkosnumeerisia merkkejä, kuten väliviivoja ja alaviivoja.
* Voi olla enintään 64 merkkiä.
* On oltava yksilöllinen erä-tili.
* Ei palvelupyynnön säilyttäminen ja kirjainkoko.

**Versio**

Määrittää lataat sovelluspaketin version. Version merkkijonot käsitellään seuraavia kelpoisuussääntöjä:

* Voi olla yhdistelmän aakkosnumeerisia merkkejä, kuten tavuviivoja, alaviivoja ja pisteitä.
* Voi olla enintään 64 merkkiä.
* On oltava yksilöllinen sovelluksen.
* Palvelupyynnön säilyttäminen ja kirjainkokoa.

**Sovelluspaketin**

Tämä kenttä määrittää .zip-tiedosto, joka sisältää sovelluksen binaaritiedostot ja tukitiedostot, jotka tarvitaan sovelluksen suorittamiseen. Valitse **Valitse tiedosto** -ruutuun tai Selaa ja valitse .zip-tiedosto, joka sisältää sovelluksen tiedostot kansiokuvaketta.

Sen jälkeen, kun olet valinnut tiedoston, valitse **OK** , jos haluat aloittaa Azuren tallennustilaan lataamisen. Kun lataus-toiminto on valmis, saat tiedon ja sulkee sivu. Sen mukaan, johon lataat tiedoston kokoa ja verkkoyhteys nopeutta tämä toiminto voi kestää jonkin aikaa.

> [AZURE.WARNING] Sulje **Uusi sovellus** -sivu, ennen kuin Lataa-toiminto on valmis. Tällöin lopettaa lataus.

### <a name="add-a-new-application-package"></a>Lisää uusi sovelluspaketin

Jos haluat lisätä uuden sovelluksen paketin version olemassa olevalle, valitse sovellus **sovellukset** -sivu, valitse **pakettien**ja valitse sitten **Lisää** Avaa **Lisää paketti** -sivu.

![Lisää sovellus paketin sivu Azure-portaalissa][8]

Kuten näet, kentät vastaavat **Uusi sovellus** -sivu, mutta **sovellustunnus** -ruutu ei ole käytössä. Samoin kuin uuden sovelluksen, Määritä uusi paketti- **Version** **sovelluspaketin** .zip-tiedosto ja valitse sitten **OK** paketin lataaminen.

### <a name="update-or-delete-an-application-package"></a>Päivitä tai poista sovellus-paketti

Päivitä tai poista aiemmin sovelluspaketin, avaa sovelluksen tiedot-sivu valitsemalla **pakettien** Avaa **pakettien** -sivu, valitse **kolme pistettä** , jota haluat muokata sovelluspaketin riville ja valitse toiminto, jonka haluat suorittaa.

![Päivitä tai poista paketin Azure-portaalissa][7]

**Päivitys**

Napsauttaessasi **päivityksen** *päivityspakettia* -sivu tulee näkyviin. Tämä sivu on kuitenkin *Uusi sovelluspaketin* -sivu muistuttaa pakkauksen valintakenttä on käytössä, joilla voit määrittää uuden ZIP-tiedoston lataaminen.

![Päivitä paketin sivu Azure-portaalissa][11]

**Poista**

Kun valitset **Poista**, sinua pyydetään vahvistamaan poiston paketti-version ja erä paketin poistaa Azure-tallennustilan. Jos poistat sovelluksen oletusversio, **oletusversio** -asetus on poistettu sovelluksen.

![Sovelluksen poistaminen][12]

## <a name="install-applications-on-compute-nodes"></a>Laske solmut-sovellusten asentaminen

Nyt kun nähnyt hallinnasta sovelluksen pakettien Azure-portaalissa, käsiteltävät aiheet ne toimivat erätehtävät ja Laske solmujen ottamisesta.

### <a name="install-pool-application-packages"></a>Asenna resurssivarantoon sovelluksen paketit

Asenna sovelluspaketin kaikissa Laske solmuissa resurssivarantoon, Määritä vähintään yksi sovellus paketin *viittaukset* varannon. Sovelluksen-paketteja, jotka voit määrittää varanto on asennettu Laske kunkin solmun solmun yhdistää altaan ja kun solmu on käynnistettävä tai reimaged.

Määritä erä .NET vähintään yksi [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref] kun luot uuden ryhmän tai aiemmin luodun ryhmän. [ApplicationPackageReference] [ net_pkgref] luokan määrittää Sovellustunnus ja version asentamisesta ryhmän Laske solmujen.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicated: "1",
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package will be installed on each.
await myCloudPool.CommitAsync();
```

>[AZURE.IMPORTANT] Jos sovelluksen paketin käyttöönoton epäonnistuu jostakin syystä, erä-palvelun merkitsee-solmu [käyttökelvoton][net_nodestate], ja ei ole tehtävät ajoitetaan-solmun suorittamista varten. Tässä tapauksessa sinun tulee **uudelleen** solmu viestitiedoston paketin käyttöönotto. Myös uudelleenkäynnistyksen solmu ottaa tehtävän ajoitus uudelleen solmun.

### <a name="install-task-application-packages"></a>Asenna tehtävä sovelluksen paketit

Samalla resurssivarantoon, voit määrittää tehtävän sovelluksen paketin *viittauksia* . Kun tehtävä ajoitetaan voidaan suorittaa solmu-paketin ladataan ja uuttaa ennen kuin tehtävän komentoriviltä suoritetaan. Jos määritetty pakkaus ja versio on jo asennettu solmu-paketin ei ole ladattu ja pakettiin käytetään.

Asenna tehtävä sovelluspaketin, määrittää tehtävän [CloudTask][net_cloudtask]. [ApplicationPackageReferences] [net_cloudtask_pkgref] ominaisuus:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>Suorita asennetut sovellukset

Paketteja, jotka olet määrittänyt ryhmän tai tehtävän ladataan ja purettu nimetystä kansiosta `AZ_BATCH_ROOT_DIR` solmun. Erän luo myös ympäristömuuttuja, joka sisältää nimetty kansion polku. Komennon tehtävärivien käyttää tämä ympäristömuuttuja viittaat solmun sovelluksen. Muuttuja on seuraavanlainen:

`AZ_BATCH_APP_PACKAGE_APPLICATIONID#version`

`APPLICATIONID`ja `version` ovat arvoja, jotka vastaavat sovelluksen ja pakkaus version olet määrittänyt käyttöönottoa varten. Jos olet määrittämäsi sovelluksen *Blenderissä* version 2.7 pitäisi olla esimerkiksi asennettu-komennon tehtävärivien käyttäisi tämä ympäristömuuttuja sen tiedostoja:

`AZ_BATCH_APP_PACKAGE_BLENDER#2.7`

Jos määrität oletusversio sovelluksen, voit jättää version liitteen. Esimerkiksi jos määrität "2.7" sovelluksen *Blenderissä*oletusversio, tehtäviä voit viitata seuraavat ympäristömuuttuja, ja ne suoritetaan versioon 2.7:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Seuraavat koodikatkelman näyttää Esimerkki-tehtävän komentorivin, joka käynnistää oletusversio *Blenderissä* sovelluksen:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [AZURE.TIP] Katso lisätietoja Laske solmu ympäristöasetuksia [erän ominaisuuden yhteenveto](batch-api-basics.md) [ympäristöasetuksia tehtävien](batch-api-basics.md#environment-settings-for-tasks) .

## <a name="update-a-pools-application-packages"></a>Päivitä resurssivarantoon sovelluksen pakettien

Jos aiemmin varanto on jo määritetty sovelluksen-paketti, voit määrittää uuden pakkaaminen ryhmään. Jos määrität sitten uusi paketti-viittaus resurssivarantoon, seuraavissa tilanteissa:

* Kaikki, jotka liittyä ryhmään solmut ja aiemmin luotu solmu, joka on käynnistettävä tai reimaged asennetaan juuri määritetty pakkaus.
* Laske solmut, jotka ovat jo sarjassa kun paketti viittaukset Asenna uusi sovelluspaketin automaattisesti. Nämä Laske käynnistettävä solmujen tai vastaanottamaan uusi paketti reimaged.
* Kun uusi paketti otetaan käyttöön, luotu ympäristömuuttujat vastaavat uuden sovelluksen paketin viittauksia.

Tässä esimerkissä aiemmin varanto on määritetty jokin sen [CloudPool] *Blenderissä* sovelluksen 2.7 version[net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref]. Päivitä ryhmän solmut 2.76b-versiolla, Määritä uusi [ApplicationPackageReference] [ net_pkgref] uusi versio ja Vahvista muutos.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Nyt kun uusi versio on määritetty, *Uusi* solmu, joka yhdistää altaan on 2.76b ottaa sen käyttöön versiosta. Asenna 2.76b solmuissa, jotka ovat *jo* -ryhmään, Käynnistä uudelleen tai reimage ne. Huomaa, että uudelleen käynnistetystä solmujen säilyttävät edellisen paketin ominaisuuksissa tiedostot.

## <a name="list-the-applications-in-a-batch-account"></a>Erän tilillä sovellusten luettelosta

Voit lisätä sovellukset ja niiden pakettien erä-tilin avulla [ApplicationOperations][net_appops]. [ListApplicationSummaries] [net_appops_listappsummaries] menetelmää.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Rivitä

Sovelluksen pakettien kanssa auttaa asiakkaille, valitse Ohjelmat, niiden töiden ja määrittää tarkan version käyttää käsiteltäessä työt, joilla on käytössä-palvelussa. Voit myös antaa mahdollisuuden asiakkaillesi ladataan ja seurata omia sovelluksia-palvelussa.

## <a name="next-steps"></a>Seuraavat vaiheet

* [Erän REST API] [ api_rest] tukee myös sovelluksen pakettien käyttöä varten. Katso esimerkiksi [applicationPackageReferences] [ rest_add_pool_with_packages] osan [lisääminen resurssivarantoon tiliin] [ rest_add_pool] määrittäminen pakettien asentaminen avulla REST-Ohjelmointirajapinnalla tietoja. Katso [sovellusten] [ rest_applications] lisätietoja hakemuksen tiedot hankkiminen erä REST-Ohjelmointirajapinnalla avulla.

* Lisätietoja ohjelmallisesti [Azure erä tilit ja kiintiön kanssa erä hallinta .NET hallinta](batch-management-dotnet.md). [Erän hallinta .NET] [ api_net_mgmt] kirjaston ottaa tilin luominen ja poistaminen ominaisuuksista erä sovelluksen tai palvelun.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Sovelluksen pakettien korkean tason kaavio"
[2]: ./media/batch-application-packages/app_pkg_02.png "Azure-portaalissa sovelluksia-ruutu"
[3]: ./media/batch-application-packages/app_pkg_03.png "Sovellusten sivu Azure-portaalissa"
[4]: ./media/batch-application-packages/app_pkg_04.png "Sovelluksen tiedot-sivu Azure-portaalissa"
[5]: ./media/batch-application-packages/app_pkg_05.png "Uuden sovelluksen sivu Azure-portaalissa"
[7]: ./media/batch-application-packages/app_pkg_07.png "Päivitä tai poista pakettien avattavasta Azure-portaalissa"
[8]: ./media/batch-application-packages/app_pkg_08.png "Uuden sovelluksen paketin sivu Azure-portaalissa"
[9]: ./media/batch-application-packages/app_pkg_09.png "Ei ole linkitetty tallennustilan tili-ilmoitus"
[10]: ./media/batch-application-packages/app_pkg_10.png "Valitse tallennustilan tilin sivu Azure-portaalissa"
[11]: ./media/batch-application-packages/app_pkg_11.png "Päivitä paketin sivu Azure-portaalissa"
[12]: ./media/batch-application-packages/app_pkg_12.png "Poista paketin vahvistusvalintaikkuna Azure-portaalissa"
