<properties
    pageTitle="Azure erä palvelun perustoiminnot | Microsoft Azure"
    description="Opi käyttämään suurissa rinnakkain ja HPC työmääriä erä Azure-palvelu"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/22/2016"
    ms.author="marsma"/>

# <a name="basics-of-azure-batch"></a>Azure erä perusteet

Azure erä avulla voit suorittaa suurissa rinnakkais- ja tehokas tietojenkäsittely (HPC) sovelluksia tehokkaasti pilveen. Platform-palvelu, joka ajoittaa Laske kuormittavaa työtä voidaan suorittaa näennäiskoneiden hallitun kokoelma on, ja voit automaattisesti asteikko Laske resurssien tarpeiden työt.

Erän-palvelussa Azure Laske resurssien sovellusten suorittaminen rinnakkain ja mittakaava määrittäminen. Voit suorittaa tarvittaessa tai ajoitetun työt, etkä tarvitse manuaalisesti, määrittäminen ja luominen sekä hallinta HPC-klusterin, yksittäisiä näennäiskoneiden, virtual verkkojen tai monimutkaisia projektin tehtävän ajoitus infrastruktuurin.

## <a name="use-cases-for-batch"></a>Erän käyttötapauksiin

Erän on hallitun Azure-palvelu, jota käytetään *erän käsittelyn* tai *erä tietojenkäsittely*--käynnissä päivittäin erittäin paljon samoja tehtäviä, saat haluamasi tuloksen. Erän tietojenkäsittely käytetään useimmin organisaatioissa, joissa säännöllisesti käsitellä, muuntaminen ja analysoida suuria tietomääriä.

Erän toimii hyvin sisäisesti rinnakkain (tunnetaan myös nimellä "embarrassingly rinnakkainen")-sovelluksia ja toiminnoista. Sisäisesti rinnakkain työmääriä jaetaan helposti useita tehtäviä, jotka tekemässä töitä samanaikaisesti useissa tietokoneissa.

![Rinnakkaisia tehtävät][1]<br/>

Esimerkkejä toiminnoista, jotka usein käsitellään avulla tällä menetelmällä ovat seuraavat:

* Riskit-mallinnus
* Siitä ja hydrology tietojen analysointi
* Kuvan värien, analysointia ja käsittely
* Media-koodaus ja koodauksen muuntamisen
* Geneettisen järjestys-analyysi
* Tekniset funktiot kuormitus analyysi
* Ohjelmiston testaaminen

Erän voit myös rinnakkaisia laskutoimituksia ja vähennä vaiheen lopussa ja suorita monimutkaisia HPC-toiminnoista, kuten [Viestin kulkeva Interface (MPI)](batch-mpi.md) -sovelluksia.

Katso vertailun Siirtoerän ja muut HPC ratkaisun asetukset Azure [Siirtoerän ja HPC ratkaisuja](batch-hpc-solutions.md).

## <a name="developing-with-batch"></a>Erän kehittäminen

Rinnakkaisia työmääriä erän käsittely tehdään tavallisesti ohjelmallisesti käyttämällä jotakin [Erä API](#batch-development-apis). Erän-ohjelmointirajapinnan kanssa voit luoda jakavat Laske solmujen (näennäiskoneiden) ja hallita töiden ajoittaminen ja tehtävät voidaan suorittaa näiden solmujen. Asiakassovellukseen tai palvelu, jonka luot käyttää erä-ohjelmointirajapinnan vaihtamaan erä-palvelussa.

Voit tehokkaasti käsitellä suurissa työmääriä organisaation tai Lisää palvelun edusta asiakkaille, jolloin niitä voi käyttää projektien ja tehtävien--pyydettäessä tai aikataulun--yhden, sataan tai jopa tuhansia solmujen. Voit käyttää myös erä suurempi työnkulun työkaluilla, kuten [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md)hallitsee osana.

> [AZURE.TIP] Kun olet valmis, voit tarkastella erä-Ohjelmointirajapinta tarkemmin tietoja se sisältää ominaisuuksia, tutustu [erän ominaisuuden yhteenveto kehittäjille](batch-api-basics.md).

### <a name="azure-accounts-youll-need"></a>Sinun on Azure tilit

Kun kehität erä ratkaisuja, tarvitset seuraavien tilien Microsoft Azure.

- **Azure-tili ja tilauksen** - sinulla ei vielä ole Azure tilaus, voit ottaa yhteyttä [MSDN-tilaaja, hyötyvät][msdn_benefits], tai rekisteröityä [ilmaisen Azure tilin][free_account]. Tiliä luodessasi oletusarvo-tilaus on luonut puolestasi.

- **Erän tili** – kun sovellustesi käsitellä erä-palvelun tilin nimi, URL-osoite tili ja pikanäppäin käytetään tunnistetiedot. Kaikki erän jakavat, kuten resurssien Laske solmuissa töitä, ja tehtäviin liittyvät erä-tili. Voit [luoda erää tilin](batch-account-create-portal.md) Azure-portaalissa.

- **Tallennustilan tilin** - erä on sisäinen tuki tiedostojen käyttäminen [Azuren tallennustilaan][azure_storage]. Erän lähes kaikissa skenaarioissa käyttää Azuren tallennustilaan--väliaikaisen ohjelmista, joita tehtävien suorittaminen ja käsittelyn tiedot ja siirtää tietoja, jotka he luovat tallennustilaan. Tallennustilan tilin luomisesta on artikkelissa [tietoja Azure-tallennustilan tilit](./../storage/storage-create-storage-account.md).

### <a name="batch-development-apis"></a>Erän kehittäminen API

Sovellusten ja palveluiden voi myöntää REST API puheluita, käytä yhtä tai useampaa seuraavat asiakas-kirjastot tai molempia hallittavan Laske resurssien ja suorita rinnakkain työmääriä tasolla erä-palvelun avulla.

| OHJELMOINTIRAJAPINTA    | API-viittaus | Lataa | MALLIKOODEJA |
| ----------------- | ------------- | -------- | ------------ |
| **Erän muille käyttäjille** | [MSDN][batch_rest] | PUUTTUU | [MSDN][batch_rest] |
| **Erän .NET**    | [MSDN][api_net] | [NuGet][api_net_nuget] | [GitHub][api_sample_net] |
| **Erän Python**  | [readthedocs.IO][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Erän Node.js** | [github.IO][api_nodejs] | [npm][api_nodejs_npm] | - |
| **Erän Java** (ennakkoversio) | [github.IO][api_java] | [Maven-testi][api_java_jar] | [GitHub][api_sample_java] |

### <a name="batch-resource-management"></a>Erän Resurssienhallinta

Asiakkaan ohjelmointirajapinnan lisäksi myös voit seuraavat resurssien erä-tilisi.

- [Erän PowerShellin cmdlet-komennot][batch_ps]: Azure erä cmdlet-komennoista [Azure PowerShell](../powershell-install-configure.md) -moduulin avulla voit hallita erä resurssien PowerShellin avulla.

- [Azure CLI](../xplat-cli-install.md): Azure käyttöliittymä (Azure CLI) on Office kaikissa ympäristöissä toolset, joka sisältää useita Azure palveluja, kuten erä käyttäminen shell-komennot.

- [Erän hallinta .NET](batch-management-dotnet.md) asiakkaan kirjaston: käytettävissä myös kautta [NuGet][api_net_mgmt_nuget], erä hallinta .NET asiakas-kirjaston avulla voit hallita ohjelmallisesti erä tilit, kiintiön ja sovelluksen pakettien. Viittaus hallinta-kirjasto on [MSDN]-sivuston[api_net_mgmt].

### <a name="batch-tools"></a>Erän Työkalut

Ei tarvitse luoda erää käyttämällä ratkaisuja, seuraavassa on joitakin tärkeitä työkaluja, luomiseen ja virheenkorjaus erä sovellusten ja palveluiden käyttää.

 - [Azure portal][portal]: Voit luoda, valvoa ja poistaa Siirtoerän jakavat, työt ja Azure portal erä lavat tehtävät. Voit tarkastella näiden tilatiedot ja muita resursseja, kun suoritat työt ja jopa ladata tiedostoja oman jakavat Laske solmujen (Lataa epäonnistui tehtävän `stderr.txt` vianmäärityksen esimerkiksi aikana). Voit ladata Remote Desktop (RDP)-tiedostoja, joiden avulla voit kirjautua sisään solmujen laskemiseen.

 - [Azure erä Explorer][batch_explorer]: erä Explorer sisältää samanlaisia erä resurssin hallintatoiminnot kuin Azure-portaalissa, mutta yksittäisen Windows Presentation Foundation (WPF)-asiakassovellukseen. Jokin erä .NET otoksen sovelluksista käytettävissä [GitHub][github_samples], voit luoda sen Visual Studio 2015 tai edellä ja käyttää sitä ja hallinnoimaan erä tilisi resurssit, kun kehittäminen ja virheenkorjaus erä ratkaisuja. Näytä työ, resurssivarantoon ja tehtävän tiedot-tiedostojen lataaminen Laske solmujen ja Yhdistä solmujen etäyhteyden käyttämällä Remote Desktop (RDP)-tiedostoja, voit ladata erä Resurssienhallinnassa.

 - [Microsoft Azure-tallennustilan Explorer][storage_explorer]: ei ole täysin Azure erä työkalu, kun tallennustilan Explorer on toisen arvokkaita työkalu, vaikka kehität ja virheenkorjaus erä ratkaisuja.

## <a name="scenario-scale-out-a-parallel-workload"></a>Skenaario: Rinnakkainen työmäärää, asteikko

Yleisiä ratkaisuun, joka käyttää erä-ohjelmointirajapinnan Käsittele erä-palvelussa liittyy laajentaminen sisäisesti rinnakkain työlle – kuten 3D Projectissa – Valitse Laske solmujen resurssivarantoon kuvat muodostamisen. Laske solmujen kannan voi olla "hahmontaminen klusterin", joka sisältää lähimpään kymmeneen, sataan tai jopa tuhansia sydämiä värien työtäsi esimerkiksi.

Seuraavassa kaaviossa on esitetty yleisiä erä työnkulun asiakassovellus tai isännöityä palveluun suorittaminen rinnakkain työmäärää erä avulla.

![Erän ratkaisu työnkulku][2]

Yleisiä tässä skenaariossa sovelluksen tai palvelun käsittelee laskennallinen työmäärää Azure osalta suorittamalla seuraavat vaiheet:

1. Lataa **syöte** ja **sovellus** , joka käsittelee Azure-tallennustilan tilin kyseiset tiedostot. Syötteen tiedostoja voi olla tietoja, jotka sovelluksen käsittelee, kuten talousrakennetta tietoja tai videotiedostot on koodausta. Sovelluksen tiedostojen voi olla jokin sovellus, jota käytetään tiedon, kuten 3D värien sovelluksen tai media transcoder käsittelyä varten.

2. Luoda erää **resurssivarantoon** Laske solmujen erä--tilisi nämä solmut ovat näennäiskoneiden, joka suorittaa tehtäviä. Azuren tallennustilaan sovelluksen asentaminen, kun solmut liittyä ryhmään (sovellus, joka ladattujen vaiheessa #1) määrittää ominaisuuksien, kuten [solmun koon](./../cloud-services/cloud-services-sizes-specs.md), niiden käyttöjärjestelmä ja sijainti. Voit myös määrittää [automaattisesti skaalaamaan](batch-automatic-scaling.md)--resurssivarantoon dynaamisesti altaan--vastauksena työmäärää, joka luo tehtävien suorittaminen solmujen määrän.

3. Luo erä **työ** voidaan suorittaa työmäärää Laske solmujen resurssivarantoon. Kun luot projektin, voit liittää sen erä resurssivarantoon.

4. **Tehtävien** lisääminen projektiin. Kun lisäät tehtäviä projektiin, erä-palvelun ajoittaa automaattisesti tehtävät-ryhmän Laske solmujen suorittamista varten. Kunkin tehtävän käyttää sovellus, jossa ladattujen syötteen tiedostoja.

    - 4 a. Ennen tehtävän suoritetaan, se voi ladata tiedot (input-tiedostoja), joka on Laske-solmu osoitetaan prosessin. Jos sovellusta ei ole jo asennettu solmun (Katso vaihe #2), se voi ladata tätä sijaan. Kun ladattavat ovat valmiit, niiden varattujen solmuissa suorittaa tehtävät.

5. Kun tehtävät suoritetaan, voit tehdä kyselyn erä työ ja sen tehtävien edistymistä. Asiakassovellus tai palvelu kommunikoi erä-palvelussa https-VÄLITYSPALVELIMEN kautta ja voi seuranta tuhansia tehtävien suorittaminen solmujen tuhansia käytössä, koska muista [kyselyn erä palvelun tehokkaasti](batch-efficient-list-queries.md).

6. Valmis tehtävinä ne ladata niiden tiedot Azure-tallennustilan. Voit hakea tiedostoja myös suoraan Laske solmujen.

7. Kun oman valvonta havaitsee työtäsi tehtävät määrittänyt-sovelluksen tai palvelun ladata lisäkäsittelyä tai arviointiin tiedot.

Pidä mielessä tämä on vain yksi tapa käyttää erää ja Tämä skenaario vain muutaman sen käytettävissä olevia ominaisuuksia. Esimerkiksi voit suorittaa [useita tehtäviä samanaikaisesti](batch-parallel-node-tasks.md) Laske jokaisen solmun ja voit käyttää [valmistelu ja valmistuminen projektitehtävien](batch-job-prep-release.md) solmut valmisteleminen työt ja sitten Puhdista jälkeenpäin.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut Siirtoerän-palvelun yleiskatsaus, se on, kun haluat tarkastella tarkemmin – lisätietoja siitä, miten voit käyttää sen käyttänyt Laske paljon-rinnakkainen toiminnoista.

- Lue [erän ominaisuuden yhteenveto kehittäjille](batch-api-basics.md), kaikille erä käytön valmisteleminen keskeiset tiedot. Artikkelissa on lisätietoja erä palvelun resurssit, kuten jakavat, solmut, projektit- ja tehtävät ja monia API-ominaisuuksia, joita voit käyttää muodostettaessa erä sovelluksen.

- Opettele käyttämään C#- ja erä .NET-kirjaston suorittaa yksinkertaisen työmäärää, Yleiset erä-työnkulkua käyttämällä [.NET Azure erä kirjasto käytön aloittaminen](batch-dotnet-get-started.md) . Tässä artikkelissa on oltava jokin oman ensimmäisen pysäytyskohdan aikana opettelemalla erä-palvelun käyttöä varten. On myös opetusohjelman [Python versio](batch-python-tutorial.md) .

- Lataa [MALLIKOODEJA-GitHub] [ github_samples] nähdäksesi, kuinka C# sekä Python voit rajapinta erän aikataulun ja prosessin otoksen toiminnoista.

- Tutustu [Oppimispolkuun erä] [ learning_path] Saat käytettävissä olevien resurssien verrata sinulle, kun opit erä-käyttöä varten.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
