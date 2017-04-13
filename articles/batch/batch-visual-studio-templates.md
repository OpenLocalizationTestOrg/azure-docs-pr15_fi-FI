<properties
    pageTitle="Visual Studio malleja Azure erän | Microsoft Azure"
    description="Katso, miten Visual Studio nämä projektimallit voit toteuttaa ja Suorita Laske paljon työmääriä Azure erän"
    services="batch"
    documentationCenter=".net"
    authors="fayora"
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

# <a name="visual-studio-project-templates-for-azure-batch"></a>Visual Studio projektimallit Azure erän

**Töiden hallinta** ja **tehtävän suoritin Visual Studio malleja** erän avulla voit toteuttaa ja Suorita Laske paljon työmääriä työmäärään pienimmät erän koodi. Tässä asiakirjassa kerrotaan mallit ja on ohjeita siitä, miten voit käyttää niitä.

>[AZURE.IMPORTANT] Tässä artikkelissa käsitellään vain nämä kaksi mallit koskevat tiedot ja oletetaan, että olet tutustunut erä palvelu ja siihen liittyvät avaimen käsitteitä: jakavat, Laske solmujen, töitä ja tehtäviä, Managerin projektitehtävistä, ympäristömuuttujat ja muut tarvittavat tiedot. Lisätietoja [Azure erä perustiedot](batch-technical-overview.md), [erän ominaisuuden yhteenveto kehittäjille](batch-api-basics.md)ja [Azure erä kirjasto .NET käytön aloittaminen](batch-dotnet-get-started.md).

## <a name="high-level-overview"></a>Ylätason yleiskatsaus

Työn esimies ja tehtävän suorittimen mallien avulla voidaan luoda hyödyllisiä kaksi komponenttia:

* Projektin hallinta tehtävän, joka sisältää työn jako-ohjausobjekti, joka voi Jaa työ useita tehtäviä, jotka voidaan suorittaa itsenäisesti rinnakkain.

* Tehtävä-suoritin, jonka avulla voidaan suorittaa esikäsittely ja käsittelyn jälkeinen sovelluksen-komentoriviltä ympärille.

Esimerkiksi elokuvan värien skenaariossa työn jako-ohjausobjekti muuttaa yksittäisen elokuvan työn satoja tai tuhansia erillisiin tehtäviin, jotka käsitellä yksittäisiä kehykset erikseen. Vastaavasti tehtävän suoritin käynnistää värien ja kaikki riippuvaiset prosessit tarvittavat kunkin hahmontamiseen kehys sekä suorittaa muita toimintoja (esimerkiksi hahmontamisen kehyksen kopioiminen tallennussijainnin).

>[AZURE.NOTE] Työn esimies ja tehtävän suorittimen mallit ovat toisistaan erillään, joten voit käyttää molemmat tai molempien mukaan vaatimukset työtäsi Laske- ja asetukset.

Seuraavassa kuvassa esitetyllä mallien käyttävä Laske työ kerrotaan kolme vaihetta:

1. Asiakas-koodin (esimerkiksi sovelluksen, verkkopalvelun, jne.) lähettää työn Azure, määrittämällä sen työn esimies tehtävän projektin hallinta-ohjelman erä-palveluun.

2. Erä-palvelua suoritetaan hallinta projektitehtävä Laske-solmu ja työn jako-ohjausobjekti Avaa tehtävän suoritin tehtävät annettuun määrään käyttöön, kun useita Laske solmujen tarvittavat parametrit ja määritykset työn jakopalkki koodissa perusteella.

3. Tehtävän suoritin tehtävät suorittaa itsenäisesti rinnakkain Käsittele syöttötiedot ja luo tiedot.

![Kaavio kuvaa, miten asiakas-koodi toimii erä-palvelun kanssa][diagram01]

## <a name="prerequisites"></a>Edellytykset

Erän mallien käyttäminen, sinun on seuraavasti:

* Tietokone Visual Studio 2015 tai uudempaan, asennettu valmiiksi.

* Erän malleja, jotka ovat käytettävissä [Visual Studio valikoima] [ vs_gallery] kuin Visual Studio tunnisteet. Saat mallit kahdella tavalla:

  * Asenna mallit **tunnisteet ja päivitykset** -valintaikkunassa Visual Studiossa (Lisätietoja on artikkelissa [etsiminen ja käyttäminen Visual Studio laajennukset][vs_find_use_ext]). Valitse **tunnisteet ja päivitykset** -valintaikkunan Etsi ja lataa seuraavia kahta tunnisteita:

    * Azure erän työn hallinnan työn jakaminen osiin
    * Azure erä tehtävän suoritin

  * Mallien lataaminen online-valikoima Visual Studio: [Microsoft Azure erä Project-mallit][vs_gallery_templates]

* Jos aiot käyttää [Sovelluksen pakettien](batch-application-packages.md) -toiminto ottaa käyttöön projektin hallinta ja tehtävän suoritin erään Laske solmujen, haluat linkittää tallennustilan tilin erä-tiliisi.

## <a name="preparation"></a>Valmistelu

On suositeltavaa luominen ratkaisu, joka sisältää työn esimiehesi sekä tehtävän-suoritin, koska tämä voi helpottaa jakaa koodin työn esimies ja tehtävän suoritin ohjelmien välillä. Voit luoda tämän ratkaisun, toimi seuraavasti:

1. Avaa Visual Studio 2015 ja valitse **Tiedosto** > **Uusi** > **projektin**.

2. Valitse **Mallit**-Laajenna **Muut projektityypin luominen**, **Visual Studio**ratkaisut ja valitse sitten **Tyhjä ratkaisu**.

3. Kirjoita sovelluksesi ja ratkaisu (esimerkiksi "LitwareBatchTaskPrograms") tarkoitus kuvaava nimi.

4. Jos haluat luoda uuden ratkaisun, valitse **OK**.

## <a name="job-manager-template"></a>Töiden hallinnan malli

Projektin hallinta-mallin avulla voit toteuttaa hallinta projektitehtävä, jotka voit tehdä seuraavat toimet:

* Työn jakaminen useita tehtäviä.
* Lähetä näiden tehtävien suorittamiseen erän.

>[AZURE.NOTE] Lisätietoja hallinta projektitehtävien artikkelissa [erä ominaisuus yleiskatsaus kehittäjille](batch-api-basics.md#job-manager-task).

### <a name="create-a-job-manager-using-the-template"></a>Luo projektin esimies mallia käyttämällä

Voit lisätä aiemmin luomasi ratkaisu työn esimies, toimi seuraavasti:

1. Avaa aiemmin luotu ratkaisun Visual Studio 2015.

2. Napsauta ratkaisunhallinnassa-ratkaisun hiiren kakkospainikkeella, valitse **Lisää** > **Uusi projekti**.

3. **Visual C#**- **pilvi**ja valitse sitten **Azure erän työn hallinnan työn jakaminen osiin**.

4. Kirjoita nimi, joka kuvaa sovelluksen ja määrittää tämän projektin työn esimiehenä (esimerkiksi "LitwareJobManager").

5. Voit luoda projektin valitsemalla **OK**.

6. Lopuksi luoda projektin Pakota Visual Studiossa, voit ladata kaikki viitatun NuGet-paketit ja tarkista, että projekti on kelvollinen, ennen kuin aloitat sen muokkaamista.

### <a name="job-manager-template-files-and-their-purpose"></a>Työn esimies mallitiedostot ja niiden tarkoitus

Kun luot projektin työn esimies-mallin avulla, luo koodi tiedostojen kolmeen ryhmään:

* Ohjelman pääikkuna tiedosto (Program.cs). Tämä sisältää ohjelman aloituskohtaa ja ylimmän tason poikkeuksen käsittely. Ei kannata yleensä tarvitse muuttaa.

* Framework-kansiota. Tämä sisältää tiedostot, jotka ovat vastuussa "asiakirjan osien uudelleen käyttäminen"-työskentelyn apuna projektin hallinta-ohjelman – purkamisen parametrit-tehtävien lisääminen erän työn jne. Sinun ei kannata tavallisesti on muokattava nämä tiedostot.

* Työn jakaminen osiin tiedosto (JobSplitter.cs). Tämä on mihin olet sijoittanut oman sovelluksen kielikohtaiset logiikan jakaminen projektin tehtävät.

Kurssin vaaditulla tukemaan työn jakopalkki koodi, työn jakaminen logiikan monimutkaisuudesta riippuen voit lisätä muita tiedostoja.

Malli luo myös vakio .NET projektitiedostoihin, kuten .csproj tiedoston, app.config, packages.config, jne.

Muiden tässä osassa kuvataan eri tiedostoja ja niiden koodin rakennetta ja kerrotaan, mitä kunkin luokan tekee.

![Näyttää projektin hallinta malli-ratkaisun Visual Studio ratkaisunhallinnassa][solution_explorer01]

**Framework tiedostot**

* `Configuration.cs`: Kapseloi työn kokoonpanotietoja, kuten erän tilin tiedot, linkitetyn tallennustilan tilin tunnistetiedot, työn ja tehtävätiedot ja työn parametrit ladataan. Se on myös pääsee erä määrittämät ympäristömuuttujat (Katso tehtävien erä asiakirjoissa ympäristöasetuksia), Configuration.EnvironmentVariable-luokan kautta.

* `IConfiguration.cs`: Käsittelee käyttöönoton määritys-luokka, niin, että voit yksikön Testaa yhteyttä projektin jakopalkki käyttämällä väärennetyt tai mock kokoonpano-objekti.

* `JobManager.cs`: Orchestrates projektin hallinta-ohjelman osat. On vastuussa valmistellaan työn jakopalkki käynnistettäessä työn jako-ohjausobjekti ja lähettämistä palauttamat työn jakopalkki tehtävän lähettäjä tehtävät.

* `JobManagerException.cs`: Edustaa virhe, joka edellyttää työn hallinta lopetetaan. JobManagerException käytetään Rivitä odotettu' virheitä, missä tietyn vianmääritystiedot voidaan suorittaa tilauksen osana.

* `TaskSubmitter.cs`: Tämä luokka on vastuussa palauttamat erän työn työn jakaminen osiin tehtävien lisääminen. JobManager luokan koostefunktioista tehtävien järjestyksen siirtäminen erissä tehokasta, mutta ajoissa lisäyksen projektille sitten kutsuu TaskSubmitter.SubmitTasks taustan viestiketjun kutakin erää.

**Työn jakaminen osiin**

`JobSplitter.cs`: Tämä luokka sisältää sovelluksen kielikohtaiset logiikan jakaminen projektin tehtävät. Puitteissa käynnistää JobSplitter.Split tapa hankkia sarja, tehtäviä, joka lisää työ, kuten menetelmä palauttaa ne. Tämä on luokka, jossa työtäsi logiikan Lisää. Ota käyttöön palauttaa CloudTask esiintymät edustava tehtäviä, johon haluat osion työn järjestyksen Jaa-menetelmää.

**Vakio .NET komentoriviltä project-tiedostot**

* `App.config`: Vakio .NET sovelluksen määritystiedosto.

* `Packages.config`: Vakio NuGet riippuvuuden pakettitiedosto.

* `Program.cs`: Sisältää ohjelman aloituskohtaa ja ylimmän tason poikkeuksen käsittely.

### <a name="implementing-the-job-splitter"></a>Käyttöönoton työn jako-ohjausobjekti

Kun avaat malliprojektin työn esimies, projekti on JobSplitter.cs-tiedosto avataan oletusarvoisesti. Voit toteuttaa Jaa logiikan tehtävien havainnollistamiseen Split() menetelmä Näytä alla avulla:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

>[AZURE.NOTE] Huomautettuja kohdassa `Split()` tapa on työn esimies mallia koodi, joka on tarkoitettu töiden jakaminen erilaisia tehtäviä voit muokata lisäämällä logiikan vain osa. Jos haluat muokata mallia eri osaan, varmista, että ovat familiarized kanssa erä toiminta ja kokeile joitakin [erä MALLIKOODEJA][github_samples].

Split() käyttöympäristösi on pääsy:

* Työn parametrit kautta `_parameters` kentän.
* Projekti, joka edustaa kautta CloudJob objekti `_job` kentän.
* Edustavaa hallinta projektitehtävä kautta CloudTask objektia `_jobManagerTask` kentän.

Oman `Split()` käyttöönoton ei tarvitse lisätä tehtäviä projektiin suoraan. Koodisi tulee palauttaa sarja CloudTask objekteja, ja ne lisätään työn automaattisesti mukaan framework luokat, jotka kutsuvat työn jako-ohjausobjekti. Usein käyttää C# 's kyselymerkkijonon (`yield return`) ominaisuus toteuttamisesta työn jako-objektien kohdistukset, kun näin Aloita käynnissä mahdollisimman pian sen sijaan, että kaikki tehtävät lasketaan odotetaan tehtävät.

**Työn jakaminen osiin virhe**

Jos projektin jakopalkki tapahtuu virhe, se tulee joko:

* Lopeta sarja käyttämällä C# `yield break` ilmoitus, jossa kirjainkoko projektin hallinta, käsitellään onnistui; tai

* Palauttaa poikkeuksen, jossa kirjainkoko työn esimies käsitellään epäonnistui ja käsitellä uudelleen sen mukaan, miten asiakas on määrittänyt sen).

Kummassakin tapauksessa tehtäviä jo palauttama työn jako-ohjausobjekti ja erätyön lisätään voi käyttää suorittamiseen. Jos et halua tätä, valitse voit tehdä seuraavaa:

* Työn jakaminen osiin-työ hakemisen

* Antaa ennen sen palauttamista koko tehtävä-sivustokokoelman (eli palauttaa `ICollection<CloudTask>` tai `IList<CloudTask>` sijaan käyttöönoton käyttämällä C#-kyselymerkkijonon työn oman jakaja)

* Käyttää tehtävien riippuvuudet, jotta kaikki tehtävät määräytyvät onnistuminen töiden hallinta

**Projektin hallinta uudelleenyritykset**

Jos projektin hallinta ei onnistu, se saattaa yritettävä erä-palvelu uudelleen asiakasasetukset mukaan. Tämä on yleensä turvallinen, koska kun puitteissa Lisää tehtäviä projektiin, se ohittaa tehtäviin, jotka on jo olemassa. Kuitenkin jos laskeminen tehtävät on kallista, ei haluta maksamaan kustannusten uudelleenlaskeminen tehtävät, jotka on jo lisätty projektiin; Vastaavasti jos uudelleen suorittamalla ei välttämättä luomiseen saman tehtävätunnukset sitten "Ohita kaksoiskappaleet"-ongelman ei projektin. Tällaisissa tapauksissa kannattaa suunnitella projektin jako-ohjausobjekti esiintyvien työmäärän, joka on jo suoritettu ja toista, kuten suorittamalla CloudJob.ListTasks ennen kuin aloitat tuotto tehtävät.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Lopetuskoodit ja poikkeuksia työn esimies-malli

Lopetuskoodit ja poikkeukset näyttämään määrittämään ulkoasusta ohjelmaa, ja niiden avulla tunnistavan ohjelman suorittamisen ongelmia. Työn esimies mallia toteuttaa tässä osiossa esitettyjä poikkeukset ja lopetuskoodit.

Hallinnan projektitehtävä, jotka on toteutettu työn esimies-mallin avulla voit palauttaa kolme mahdollista lopetuskoodit:

| Koodi | Kuvaus |
|------|-------------|
| 0    | Työn esimies onnistui. Työn jakaminen osiin-koodin suoritettiin loppuun, ja kaikki tehtävät on lisätty projektiin. |
| 1    | Projektitehtävä hallinta epäonnistui ohjelman arvioidut"-osassa. Poikkeus on käännetty JobManagerException vianmääritystiedot kanssa ja, jos mahdollista, ehdotuksia virheen ratkaiseminen. |
| 2    | Projektitehtävä hallinta epäonnistui 'odottamaton' poikkeuksen vuoksi. Poikkeus on kirjautunut vakio tulosteen, mutta työn manager ei voi lisätä diagnostiikka tai korjaus lisätiedot. |

Projektin hallinta tehtävät ei toimi, kyseessä joitakin tehtäviä voi silti on lisätty palveluun ennen virhe. Seuraavat tehtävät suoritetaan normaalisti. Katso "Työn jakopalkki virhe" koodin Tämä polku-keskusteluun.

Kaikki poikkeukset palauttamia tietoja kirjoitetaan stdout.txt ja stderr.txt tiedostoihin. Lisätietoja on artikkelissa [Virheen käsittely](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>Huomioitavaa asiakkaista

Tässä osassa käsitellään joitakin asiakkaan käyttöönoton vaatimukset, kun käynnistettäessä työn esimies mallin pohjalta. Katso, [miten välittää parametrit ja ympäristömuuttujat asiakkaan koodista](#pass-environment-settings) lisätietoja kulkeva parametrit ja ympäristöasetuksia.

**Pakollinen tunnistetiedot**

Jotta voit lisätä tehtäviä Azure erä työ, hallinnan projektitehtävä edellyttää Azure erä tilin URL-osoite ja avaimen avulla. Sinun on välitettävä ympäristön muuttujan YOUR_BATCH_URL ja YOUR_BATCH_KEY. Voit määrittää nämä työn esimies tehtävän ympäristöasetuksia. Valitse esimerkiksi C#-asiakasohjelmassa:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Tallennustilan tunnistetiedot**

Asiakas ei yleensä tarvitse antaa linkitetyn tallennustilan tilin tunnistetietojen hallinta projektitehtävä, koska (a) eniten työn valvojat ei tarvitse erikseen linkitetyn tallennustilan tilin käyttöä ja (b) linkitetyn tallennustilan tilin usein annettu kaikki tehtävät yleisiä ympäristön-asetukseksi työn. Jos jotka eivät tarjoa linkitetyn tallennustilan tilin lisäämiseen yleisiä ympäristöasetuksia ja työn esimiehen voi käyttää linkitetyn tallennustilaasi, valitse olisi annat linkitetyn tallennustilan tunnistetietoja seuraavasti:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Työn hallinnan tehtäväasetukset**

Asiakkaan Määritä projektin hallinta *killJobOnCompletion* Merkitse arvoksi **false**.

On yleensä turvallista asiakkaan *runExclusive* arvoksi **false**.

Asiakkaan tulee käyttää *resourceFiles* tai *applicationPackageReferences* sivustokokoelman työ on suoritettava manager (ja sen tarvittavat dll) käyttöön Laske-solmu.

Oletusarvon mukaan projektin hallinta ei yritetään jos se ei onnistu. Sen mukaan, työ-hallinnan logiikan asiakkaan kannattaa ottaa käyttöön *rajoituksia*kautta uudelleenyritykset/*maxTaskRetryCount*.

**Työn asetukset**

Jos projektin jakopalkki tietokoneesta kuuluu äänimerkki tehtävien riippuvuudet, asiakkaan on määritettävä projektin usesTaskDependencies tosi.

Työn jakaminen osiin-mallissa on epätavallinen asiakkaiden haluat lisätä tehtävät työt lisäksi projektin jakopalkki Luo. Asiakkaan olisi näin ollen normaalisti projektin *onAllTasksComplete* **terminatejob**.

## <a name="task-processor-template"></a>Suoritin mallilla

Tehtävän suoritin mallin avulla voit toteuttaa tehtävän suoritin, joka suorittaa seuraavat toimenpiteet:

* Määritä erä tehtävien suorittamiseen tarvittavat tiedot.
* Kaikki toiminnot vaatii erä kunkin tehtävän suorittaminen
* Tallenna tehtävän tulostaa pysyvän säilön.

Tehtävä-suoritin ei tarvitse suorittaa tehtäviä Siirtoerän, avaimen tehtävän suorittimen käytön etuna on se sisältää paketti toteuttaa kaikki tehtävän suorittaminen toiminnot yhdessä paikassa. Jos haluat suorittaa useita sovelluksia kunkin tehtävän kontekstissa, tai jos haluat kopioida tiedot pysyvät säilöön, kun olet suorittanut kunkin tehtävän esimerkiksi.

Tehtävä-suoritin toimintoja voi olla yksinkertainen tai monimutkaisia, ja niin monta tai kuin vähän havainnollistamiseen tarvittaessa. Lisäksi käyttämällä yksi tehtävä suoritin kaikki Tehtävätoiminnot, voit helposti päivittää tai Lisää toimintoja perusteella sovellusten tai työmäärää vaatimukset muuttamista. Joissakin tapauksissa tehtävä-suoritin ei voi kuitenkin paras mahdollinen ratkaisu käyttöympäristösi se lisätään tarpeettomat monimutkaisuus, esimerkiksi kun käynnissä työt, jotka voi nopeasti käynnistää yksinkertainen komentoriviltä.

### <a name="create-a-task-processor-using-the-template"></a>Luo tehtävän suoritin mallia käyttämällä

Voit lisätä aiemmin luomasi ratkaisu tehtävän suoritin, toimi seuraavasti:

1. Avaa aiemmin luotu ratkaisun Visual Studio 2015.

2. Napsauta ratkaisunhallinnassa-ratkaisun hiiren kakkospainikkeella, valitsemalla **Lisää**ja valitse sitten **Uusi projekti**.

3. **Visual C#**- **pilvi**ja valitse sitten **Azure erä tehtävän suoritin**.

4. Kirjoita nimi, joka kuvaa sovelluksen ja määrittää tämän projektin nimellä tehtävä-suoritin (esimerkiksi "LitwareTaskProcessor").

5. Voit luoda projektin valitsemalla **OK**.

6. Lopuksi luoda projektin Pakota Visual Studiossa, voit ladata kaikki viitatun NuGet-paketit ja tarkista, että projekti on kelvollinen, ennen kuin aloitat sen muokkaamista.

### <a name="task-processor-template-files-and-their-purpose"></a>Tehtävän suoritin mallitiedostot ja niiden tarkoitus

Kun luot projektin tehtävän suoritin mallia, luo koodi tiedostojen kolmeen ryhmään:

* Ohjelman pääikkuna tiedosto (Program.cs). Tämä sisältää ohjelman aloituskohtaa ja ylimmän tason poikkeuksen käsittely. Ei kannata yleensä tarvitse muuttaa.

* Framework-kansiota. Tämä sisältää tiedostot, jotka ovat vastuussa "asiakirjan osien uudelleen käyttäminen"-työskentelyn apuna projektin hallinta-ohjelman – purkamisen parametrit-tehtävien lisääminen erän työn jne. Sinun ei kannata tavallisesti on muokattava nämä tiedostot.

* Tehtävän suoritin tiedosto (TaskProcessor.cs). Tämä on mihin olet sijoittanut sovelluksen kielikohtaiset-logiikkaa tehtävän suorittamiseen (yleensä mukaan soittamista aiemmin suoritettavan). Vanhat ja uudet käsittely koodi, kuten lisätiedot ja tulos tiedostojen lataamista tulee myös tähän.

Kurssin vaaditulla tukemaan tehtävän suoritin koodi, työn jakaminen logiikan monimutkaisuudesta riippuen voit lisätä muita tiedostoja.

Malli luo myös vakio .NET projektitiedostoihin, kuten .csproj tiedoston, app.config, packages.config, jne.

Muiden tässä osassa kuvataan eri tiedostoja ja niiden koodin rakennetta ja kerrotaan, mitä kunkin luokan tekee.

![Näyttää tehtävän suoritin malli-ratkaisun Visual Studio ratkaisunhallinnassa][solution_explorer02]

**Framework tiedostot**

* `Configuration.cs`: Kapseloi työn kokoonpanotietoja, kuten erän tilin tiedot, linkitetyn tallennustilan tilin tunnistetiedot, työn ja tehtävätiedot ja työn parametrit ladataan. Se on myös pääsee erä määrittämät ympäristömuuttujat (Katso tehtävien erä asiakirjoissa ympäristöasetuksia), Configuration.EnvironmentVariable-luokan kautta.

* `IConfiguration.cs`: Käsittelee käyttöönoton määritys-luokka, niin, että voit yksikön Testaa yhteyttä projektin jakopalkki käyttämällä väärennetyt tai mock kokoonpano-objekti.

* `TaskProcessorException.cs`: Edustaa virhe, joka edellyttää työn hallinta lopetetaan. TaskProcessorException käytetään Rivitä odotettu' virheitä, missä tietyn vianmääritystiedot voidaan suorittaa tilauksen osana.

**Tehtävän suoritin**

* `TaskProcessor.cs`: Suorittaa tehtävän. Puitteissa käynnistää TaskProcessor.Run-menetelmää. Tämä on luokan, jossa Lisää sovellus kielikohtaiset logiikan tehtävän. Toteuta asennus noudattamalla:
  * Jäsentää ja tarkistaa tehtävän parametrit
  * Kirjoita kaikki ulkoisen haluat käynnistää ohjelman komentorivi
  * Kirjaudu minkä tahansa vianmääritystiedot, saatat tarvita vianmääritystä varten
  * Aloita kyseisen komentorivin avulla
  * Odota, Lopeta prosessi
  * Sieppaa Lopeta prosessi, jos haluat tarkistaa, onko se onnistui tai epäonnistui koodi
  * Tallenna tiedostot haluat säilyttää pysyvän säilön tulostus

**Vakio .NET komentoriviltä project-tiedostot**

* `App.config`: Vakio .NET sovelluksen määritystiedosto.
* `Packages.config`: Vakio NuGet riippuvuuden pakettitiedosto.
* `Program.cs`: Sisältää ohjelman aloituskohtaa ja ylimmän tason poikkeuksen käsittely.

## <a name="implementing-the-task-processor"></a>Käyttöönoton tehtävä-suoritin

Kun avaat projektin tehtävän suoritin mallia, projekti on TaskProcessor.cs-tiedosto avataan oletusarvoisesti. Voit ottaa käyttöön tehtävien suorittaminen logiikan havainnollistamiseen-menetelmällä Run () alla:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
>[AZURE.NOTE] Run ()-menetelmällä huomautettuja osa on vain osa tehtävän suoritin mallia koodi, joka on tarkoitettu voit muokata lisäämällä tehtävien suorittaminen logiikan havainnollistamiseen. Jos haluat muokata mallia eri osaan, määritä ensin valintanauhaan tutustuminen erä toiminta tarkastelemalla erä asiakirjoista ja kokeilla erä MALLIKOODEJA suoritettavien.

Run ()-menetelmä on vastuussa komentoriviltä käynnistäminen vähintään yksi prosesseja, kaikki prosessi, odotetaan avaamista, tallentamalla tulosten ja lopuksi Lopeta-koodin palauttaminen. Run ()-menetelmä on kohtaa, johon voit ottaa käsittely logiikan tehtäviin. Tehtävän suoritin framework käynnistää menetelmä Run () Sinun ei tarvitse kutsua itseäsi.

Run ()-käyttöönoton on pääsy:

* Tehtävä-parametrit kautta `_parameters` kentän.
* Työn ja tehtävän tunnukset, kautta `_jobId` ja `_taskId` kentät.
* Tehtävä-määritysten kautta `_configuration` kentän.

**Tehtävävirhe**

Virhe, kun Run ()-menetelmä voit sulkea poikkeuksen lähetys, mutta tämä jättää ylimmän tason poikkeuksen käsittelytoiminnon hallita sitä, Lopeta tehtävä-koodi. Jos haluat hallita lopetuskoodi, niin, että voit erottaa erityyppiset virhe, esimerkiksi vianmääritystä varten tai siksi, että jotkin virheen tilat olisi Lopeta työn ja muiden saa olisi poistu Run ()-menetelmä palauttamalla nollasta lopetuskoodi. Tämä on tehtävä exit-koodi.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Lopetuskoodit ja poikkeukset tehtävän suoritin mallissa

Lopetuskoodit ja poikkeukset näyttämään ulkoasusta ohjelmaa määrittämiseen ja niitä voi tunnistaa ohjelman suorittamisen ongelmia. Tehtävän suoritin mallia toteuttaa tässä osiossa esitettyjä poikkeukset ja lopetuskoodit.

Tehtävän suoritin tehtävän, jotka on toteutettu tehtävän suoritin mallia voi palauttaa kolme mahdollista Lopeta-koodeista:

| Koodi | Kuvaus |
|------|-------------|
|  [Process.ExitCode][process_exitcode] | Tehtävä-suoritin suoritettiin loppuun. Huomaa, että tämä ei tarkoita, että voit käynnistää ohjelman onnistui – vain tehtävän suoritin kutsua onnistuneesti ja suorittaa jälkeinen käsittely ilman poikkeuksia. Lopetuskoodin merkitys määräytyy suoritettavaa ohjelmaa – koodin 0 tarkoittaa yleensä ohjelma onnistui ja lopetuskoodin tarkoittaa, että ohjelma ei onnistunut. |
| 1    | Tehtävä-suoritin epäonnistui ohjelman odotettu"-osassa. Poikkeus on käännetty `TaskProcessorException` vianmääritystiedot kanssa ja, jos mahdollista, ehdotuksia virheen ratkaiseminen. |
| 2    | Tehtävä-suoritin epäonnistui 'odottamaton' poikkeuksen vuoksi. Poikkeus on kirjautunut vakio tulosteen, mutta tehtävä-suoritin ei voi lisätä diagnostiikka tai korjaus lisätiedot. |

>[AZURE.NOTE] Jos suoritat ohjelma käyttää lopetuskoodit 1 ja 2 osoittamaan toimittajakohtaiset tilat, lopetuskoodit 1 ja 2 tehtävän suoritin virheet on epäselvä. Voit muuttaa tehtävän nämä suoritin virhekoodit erottamiskykyä lopetuskoodit muokkaamalla poikkeuksen tapauksissa Program.cs-tiedostossa.

Kaikki poikkeukset palauttamia tietoja kirjoitetaan stdout.txt ja stderr.txt tiedostoihin. Lisätietoja on artikkelissa erä asiakirjoissa virheen käsittely.

### <a name="client-considerations"></a>Huomioitavaa asiakkaista

**Tallennustilan tunnistetiedot**

Jos tehtävän suoritin käyttää Azure-blob-säiliö jatkuvat tulostus, käyttämällä esimerkiksi tiedoston nimeämiskäytännön helper kirjastoon, valitse tarvitaan *joko* cloud tallennustilan tilin tunnistetiedot *tai* käyttää blob-säilö URL, joka sisältää jaetun access-allekirjoituksen (SAS). Malli sisältää tuki käyttöoikeudet yleisiä ympäristömuuttujat kautta. Asiakas voi kulua tallennustilan tunnistetietoja seuraavasti:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Tallennustilan-tili on sitten käytettävissä TaskProcessor luokan kautta `_configuration.StorageAccount` ominaisuus.

Jos haluat käyttää säilö URL-Osoitetta ja Suojaussidokset, voit myös siirtää tämä projektin yleiset ympäristön-asetuksen, mutta tehtävän suoritin-mallia ei ole tällä hetkellä tämä tukee.

**Tallennustilan asetukset**

On suositeltavaa, että asiakas- tai projektin hallinta-tehtävän luominen säilöjen vaatii tehtäviä, ennen kuin lisäät tehtäviä projektiin. Tämä on pakollinen, jos käytät säilö URL-osoite ja Suojaussidokset, sellaisenaan URL-osoite ei ole oikeus luoda säilön. On suositeltavaa vaikka välittää tallennustilan tilin tunnistetiedot, kun se tallentaa jokaisen tehtävän tarvitse kutsua CloudBlobContainer.CreateIfNotExistsAsync säilö.

## <a name="pass-parameters-and-environment-variables"></a>Välittää parametrit ja ympäristömuuttujat

### <a name="pass-environment-settings"></a>Välittää ympäristöasetuksia

Asiakas voi välittää tietoja hallinta projektitehtävä ympäristöasetuksia-lomakkeessa. Nämä tiedot sitten voidaan käyttää hallinta projektitehtävä tehtävän suoritin tehtävät, jotka suorittavat Laske työhön luotaessa. Tietoja, jotka voit välittää ympäristön asetukset ovat esimerkiksi seuraavat:

* Tallennustilan tilin nimi ja avaimet
* Erän tilin URL-osoite
* Erän tilin avain

Erän palvelulla yksinkertainen järjestelmä, joka välittää ympäristöasetuksia projektitehtävälle hallinnan avulla `EnvironmentSettings` [Microsoft.Azure.Batch.JobManagerTask]-ominaisuuden[net_jobmanagertask].

Jos esimerkiksi haluat saada `BatchClient` esiintymän erä-tilin, voit siirtää kuin ympäristömuuttujat asiakaskoneesta koodi URL-Osoitteen ja jaetun avaimen tunnistetiedot erä tilin. Vastaavasti tallennustilan tilin, joka on linkitetty erä tilin käyttöä, voit siirtää tallennustilan tilin nimi ja tallennustilaa tilin avaimen kuin ympäristömuuttujia.

### <a name="pass-parameters-to-the-job-manager-template"></a>Välittää parametreja työn esimies-malli

Monissa tapauksissa kannattaa käyttää työn kohti parametrit siirtää työn-hallinnan tehtävän, voit hallita työn jakaminen prosessin tai voit määrittää projektin tehtävät. Voit tehdä tämän lataamalla JSON tiedoston nimeltä parameters.json hallinnan projektitehtävälle resurssi-tiedostona. Parametrien sitten voi tulla käytettävissä `JobSplitter._parameters` töiden hallinnan malli-kenttään.

>[AZURE.NOTE] Valmiin parametrin käsittelijä tukee vain merkkijonon merkkijonon sanastot. Jos haluat siirtää monimutkaisia JSON arvot parametrin arvot, sinun on välitettävä merkkijonoina ja jäsentää niitä työn jako-ohjausobjekti tai muokata framework `Configuration.GetJobParameters` menetelmää.

### <a name="pass-parameters-to-the-task-processor-template"></a>Välittää parametreja tehtävän suoritin-malli

Voit myös siirtää parametrit yksittäisiin tehtäviin, jotka on toteutettu tehtävän suoritin-mallin avulla. Samalla tavalla kuin projektin hallinta-malli-suoritin mallilla etsitään-niminen resurssitiedosto

parameters.JSON ja todennut, että se, jos Lataa kuin parametrisanasto. Muutamalla eri tavalla välittää parametreja tehtävän suoritin tehtävät::

* Käyttää projektin parametrit JSON. Tämä toimii hyvin, jos vain parametrit ovat työn laajuinen luotuja (esimerkiksi hahmontaminen korkeus ja leveys). Voit tehdä tämän luotaessa CloudTask työn jako-ohjausobjekti, Lisää viittaus parameters.json resurssin tiedosto-objektin-työ hallinta tehtävän ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) CloudTask ResourceFiles-kokoelmaan.

* Luo ja Lataa asiakirja-tehtäväkohtaisia parameters.json osana jakopalkki suorittaminen ja viittaa kyseisen tehtävän resurssin tiedostot sivustokokoelman blob. Tämä on tarpeen, jos eri tehtäviä on parametrien. Jos kehyksen indeksi välitetään parametrina tehtävän 3D värien tilanne voi olla esimerkiksi.

>[AZURE.NOTE] Valmiin parametrin käsittelijä tukee vain merkkijonon merkkijonon sanastot. Jos haluat siirtää monimutkaisia JSON arvot parametrin arvot, sinun on välitettävä merkkijonoina ja jäsentää niitä tehtävä-suoritin tai muokata framework `Configuration.GetTaskParameters` menetelmää.

## <a name="next-steps"></a>Seuraavat vaiheet

### <a name="persist-job-and-task-output-to-azure-storage"></a>Pysyvä työn ja tehtävän tulosteen Azuren tallennustilaan

Muuta hyötyä työkalua erä ratkaisu kehitteillä on [Azure erä tiedoston nimeämiskäytännön][nuget_package]. .NET luokan kirjaston (olevan esikatselu) erä .NET-sovellusten avulla voit helposti tallentaa ja tarkastella tehtävän tulostus ja sieltä pois Azure-tallennustilan. [Pysyvä Azure erä työ-ja tehtävä](batch-task-output.md) on kirjastoon ja sen käytön koko keskustelun.

### <a name="batch-forum"></a>Erän keskustelupalsta

[Azure erä keskustelupalstalla] [ forum] MSDN-sivuston on kätevä keskustella Siirtoerän ja esittää kysymyksiä tietoja. Pää-päälle hyödyllisiä "muistilappuja" pylväiden ja lähettää kysymyksiä, kun ne aiheutuvat aikana voit luoda erää ratkaisuja.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
