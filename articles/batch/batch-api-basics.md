<properties
    pageTitle="Azure erän ominaisuuden yhteenveto kehittäjille | Microsoft Azure"
    description="Tietoja sen API-kehittämisen kannalta erä palvelun ominaisuuksia."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/29/2016"
    ms.author="marsma"/>

# <a name="batch-feature-overview-for-developers"></a>Erän ominaisuuden yhteenveto-kehittäjille

Tässä yleiskatsauksessa Azure erä palvelun core osien käsiteltävät aiheet ensisijainen service ominaisuudet ja resurssit, joiden avulla erä kehittäjät voivat luoda laajoja rinnakkain Laske ratkaisuja.

Onko kehittämässä jaetun laskennallinen sovelluksen tai palvelun, joka ongelmat suoraan [REST API] [ batch_rest_api] kutsuja tai on käytössä jokin [Erä SDK: T](batch-technical-overview.md#batch-development-apis), käytät paljon resursseja ja tässä artikkelissa kuvatut ominaisuudet.

> [AZURE.TIP] Katso ylemmän tason Johdatus erä palvelun [Azure erä perusteet](batch-technical-overview.md).

## <a name="batch-service-workflow"></a>Erän palvelun työnkulku

Seuraavat korkean tason työnkulku on tyypillisiä lähes kaikki sovellukset ja palvelut, jotka käyttävät käsittelyyn rinnakkain työmääriä erä-palvelu:

1. Lataa **tiedostot** , jota haluat käsitellä [Azuren tallennustilaan] [ azure_storage] tili. Erän on sisäinen tuki käyttäminen Azure-Blob-säiliö ja tehtäviä voit ladata tiedostot laskemiseen [solmujen](#compute-node) , tehtäviä suoritettaessa.

2. Lataa **sovelluksen tiedostoista** , jotka suorittavat tehtäviä. Nämä tiedostot voi olla binaaritiedostot tai komentosarjojen ja niiden välisten riippuvuuksien ja suorittaa tehtävät työt. Tehtävät voivat ladata nämä tiedostot tallennustilan tililtä tai voit käyttää [sovelluksen pakettien](#application-packages) ominaisuus erän sovellusten hallinta-ja.

3. Laske solmujen [resurssivarannon](#pool) luominen Kun luot resurssivarantoon, Määritä varannon, niiden kokoa ja käyttöjärjestelmän Laske solmujen määrän. Jokaisen tehtävän työtäsi suoritettaessa on myöntänyt johonkin lisääminen resurssivarantoon solmujen suorittaminen.

4. Luo [Projekti](#job). Työn hallitsee joukko tehtäviä. Voit liittää jokaiseen työhön tietyn ryhmään, jossa kyseinen projektitehtävien suoritetaan.

5. [Tehtävien](#task) lisääminen projektiin. Kunkin tehtävän suorittaa sovelluksen tai komentosarjan, joka ladattujen käsittelemään se lataa tallennustilan tililtä datatiedostot. Kun kunkin tehtävän on valmis, se ladata tulos Azure-tallennustilan.

6. Työn edistymistä ja noutaa tehtävä tulokset Azure-tallennustilan.

Seuraavissa osissa käsitellään näitä ja muita resursseja Siirtoerän, jotka mahdollistavat hajautettu laskennallinen skenaarion.

> [AZURE.NOTE] Tarvitset [erä tilin](batch-account-create-portal.md) erä-palvelun käyttöä varten. Lisäksi lähes kaikki ratkaisut käyttöön [Azuren tallennustilaan] [ azure_storage] tiedostojen tallentamisesta ja hakemista. Erän tällä hetkellä tukee vain **yleisiä tarkoitus** tallennustilan tilin tyyppi, [Luo tallennustilan tilin](../storage/storage-create-storage-account.md#create-a-storage-account) [tietoja Azure](../storage/storage-create-storage-account.md)-tallennustilan tilin vaiheessa 5 kuvatulla tavalla.

## <a name="batch-service-resources"></a>Erän service-resurssit

Osa on seuraavissa resursseissa--tilit, Laske solmujen, jakavat, projektien ja tehtävien--edellyttää, että kaikki erän-palvelua käyttävät ratkaisut. Muille käyttäjille, kuten työaikatauluja ja sovelluksen paketit ovat hyödyllisiä, mutta valinnainen, ominaisuuksia.

- [Tilin](#account)
- [Laske solmu](#compute-node)
- [Resurssivarannon](#pool)
- [Työn](#job)

  - [Työaikatauluja](#scheduled-jobs)

- [Tehtävä](#task)

  - [Aloita tehtävä](#start-task)
  - [Projektitehtävä hallinta](#job-manager-task)
  - [Valmistelut ja vapauta projektitehtävät](#job-preparation-and-release-tasks)
  - [Usean esiintymän tehtävän (MPI)](#multi-instance-tasks)
  - [Tehtävien riippuvuudet](#task-dependencies)

- [Sovelluksen-paketit](#application-packages)

## <a name="account"></a>Tilin

Erän-tili on yksilöllisesti määritetyn kohteen erä-palvelun. Kaikki käsittely on liitetty erä-tiliin. Voit suorittaa Siirtoerän-palvelun kanssa, kun tarvitset sekä tilin nimi ja jokin seuraavista sen tilin-näppäimiä. Voit [luoda Azure-portaalissa Azure erä tili](batch-account-create-portal.md).

## <a name="compute-node"></a>Laske solmu

Laske-solmu on Azure virtual-koneen (AM), joka on varattu käsittelyn työmäärää sovelluksen osa. Solmun koon määrittää suorittimen sydämiä, muistin koko ja paikallisen tiedoston järjestelmän koko, joka on kohdistettu solmu määrän. Voit luoda jakavat Windows- tai Linux solmujen Azure pilvipalveluihin tai näennäiskoneiden Marketplace kuvia käyttämällä. Kohdassa [resurssivarantoon](#pool) lisätietoja näistä asetuksista.

Solmujen voit suorittaa minkä tahansa suoritettavat tai komentosarjan, joka tue käyttöjärjestelmäympäristö solmun. Tämä vaihtoehto sisältää \*.exe- \*cmd., \*.bat ja PowerShell-komentosarjojen Windowsin--ja binaaritiedostot, runko ja Python komentosarjojen Linux varten.

Kaikki Laske solmujen osalta, sisältyy myös:

- Vakio [kansiorakenne](#files-and-directories) ja liittyvän [Ympäristömuuttujat](#environment-settings-for-tasks) , jotka ovat käytettävissä viittausta tehtävää varten.
- **Palomuurin** asetuksia, jotka on määritetty hallintaa.
- [Etäkäyttöpalvelimen](#connecting-to-compute-nodes) Windows (Remote Desktop Protocol (RDP)) ja solmujen Linux (suojattu runko (SSH)).

## <a name="pool"></a>Resurssivarannon

Varanto on kokoelma, joka suoritetaan sovelluksen solmujen. Altaan voidaan luoda manuaalisesti sinä tai automaattisesti erä-palvelun määrittäessäsi työtä. Voit luoda ja hallita, joka vastaa resurssin sovelluksen sovellussarjan. Resurssivarantoon voidaan käyttää vain erä-tili, johon se on luotu. Erän tilin voi olla useita resurssivarantoon.

Azure erä jakavat luominen core Azure Laske-ympäristö päälle. Niissä suurissa kohdistus, sovelluksen asennuksen, tietojen jakelu, kunnon seuranta ja joustavia Laske ([Skaalaus](#scaling-compute-resources)) resurssivarantoon sijaitsevien solmujen määrän muuttaminen.

Kunkin solmun, joka on lisätty resurssivarantoon määritetään yksilöivä nimi ja IP-osoite. Kun solmu poistetaan resurssivarannosta-käyttöjärjestelmä tai tiedostoihin tehdyt muutokset menetetään, ja sen nimi ja IP-osoite julkaistaan myöhempää käyttöä varten. Kun solmu poistuu resurssivarantoon, aikana on päättynyt.

Kun luot resurssivarantoon, voit määrittää määritteet:

- Laske solmu **käyttöjärjestelmä** ja **versio**

    Kun valitset käyttöjärjestelmän solmujen oman varanto on kaksi vaihtoehtoa: **Virtuaalikoneen määritys** ja **Cloud Services-kokoonpanon määritys**.

    **Virtuaalikoneen määritys** tarjoaa Linux- ja Windows-kuvat Laske solmujen [Azuren näennäiskoneiden Marketplace][vm_marketplace].
    Luodessasi resurssivarantoon, joka sisältää virtuaalikoneen määritysten solmujen ei vain solmut, mutta myös **virtuaalikoneen kuva viittaus** ja erä **solmu agentti tuote** on asennettava solmut kokoa on määritettävä. Lisätietoja näiden ominaisuuksien määrittämisestä on artikkelissa [säännöstä Linux Laske Azure erä jakavat solmujen](batch-linux-nodes.md).

    **Cloud Services-kokoonpanon määritys** on Windowsin Laske solmujen *vain*. Cloud Services-kokoonpanon määritys jakavat käyttöjärjestelmien näkyvät [Azure Vieras OS versioista ja SDK yhteensopivuuden matriisissa](../cloud-services/cloud-services-guestos-update-matrix.md). Luodessasi resurssivarantoon, joka sisältää pilvipalveluihin solmujen sinun täytyy määrittää vain solmun koon ja sen *OS perhe*. Kun luot Windows jakavat Laske solmujen, pilvipalveluihin käytät useimmin.

    - *OS perheen* määrittää myös .NET-versiot asennetaan käyttöjärjestelmän mukana.
    - Kun työntekijä rooleja pilvipalveluihin, voit määrittää sen *Käyttöjärjestelmäversio* (Lisätietoja työntekijä roolit on kohdassa [Ilmoita tietoja pilvipalveluihin](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) [Cloud Services-palvelujen yleiskatsaus](../cloud-services/cloud-services-choose-me.md)).
    - Kun työntekijä rooleihin, on suositeltavaa, että määrität `*` varten *Käyttöjärjestelmäversio* niin, että solmut päivittyvät automaattisesti ei tarvitse mukauttaminen juuri työtä julkaissut versiot. Ensisijainen Käyttötapaus valitsemiseen tietyn käyttöjärjestelmäversio on yhteensopivuuden, minkä ansiosta testejä ennen kuin sallit versio on päivitettävä suoritettava yhteensopivuuden varmistamiseksi. Kun vahvistus-altaan *Käyttöjärjestelmäversio* voi päivittää, ja uusi OS kuva voi olla asennettu--kaikki käynnissä olevat tehtävät on keskeytetty ja uudelleen.

- **Solmut koosta**

    **Cloud Services-kokoonpanon määritys** Laske solmu koot on lueteltu [koot pilvipalveluihin](../cloud-services/cloud-services-sizes-specs.md). Erän tukee kaikkia pilvipalveluihin koot lukuun ottamatta `ExtraSmall`.

    **Virtuaalikoneen määritysten** Laske solmun koot näkyvät [koot näennäiskoneiden Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) ja [koot näennäiskoneiden Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Erän tukee kaikkia Azure AM koot lukuun ottamatta `STANDARD_A0` ja sisältävän premium storage (`STANDARD_GS`, `STANDARD_DS`, ja `STANDARD_DSV2` sarjan).

    Kun valitset Laske-solmun koon ja tarvittaessa ominaisuudet ja solmut suoritat sovellusten vaatimuksia. Parhaiten sopiva ja edullinen solmu koon määrittäminen ominaisuuksia, kuten onko sovelluksen monisäikeisten ja muistin tarvitaan avulla. On tyypillinen voit valita solmu koon olettaen yksi tehtävä suoritetaan solmu kerrallaan. On kuitenkin mahdollista voi olla useita tehtäviä (ja näin ollen useita Palvelusovellusten esiintymät) [suorittaminen rinnakkain](batch-parallel-node-tasks.md) Laske solmujen, työn suorituksen aikana. Tässä tapauksessa on yhteinen Valitse solmu suurentaminen sopimaan Rinnakkainen tehtävän suorittaminen parantavat tarvittaessa. Lisätietoja on kohdassa [tehtävän ajoitus käytännön](#task-scheduling-policy) .

    Kaikkien solmujen varanto on yhtä suuri. Jos aiot sovellukset toimivat eri järjestelmävaatimukset ja/tai ladata tasoja, suosittelemme, että käytät eri jakavat.

- **Kohteen solmujen määrän**

    Tämä on, jonka haluat ottaa ryhmän Laske solmujen määrän. Tätä kutsutaan *kohde* koska joissakin tapauksissa lisääminen resurssivarantoon ehkä ulotu solmujen haluamasi määrä. Resurssivarantoon ehkä ulotu solmujen haluamasi määrä, jos ohjausobjekti [core kiintiön](batch-quota-limit.md#batch-account-quotas) erä--tilin vai onko Automaattinen skaalaaminen kaava, joka on kohdistettu ryhmään, joka rajoittaa (Katso "Skaalaus käytännön"-osio) solmujen enimmäismäärä.

- **Käytännön skaalaus**

    Lisäksi on staattinen solmujen määrä, joka määrittää, voit sen sijaan kirjoittaminen ja käytä jälkeen [Automaattinen skaalaaminen kaavan](#scaling-compute-resources) resurssivarantoon. Erän palvelun kaava laskee säännöllisesti ja säätää eri resurssivarantoon, työn ja voit määrittää tehtävän parametrien perusteella altaan sijaitsevien solmujen määrän.

- **Tehtävien ajoituksen käytäntö**

    [Max tehtävät kohti solmu](batch-parallel-node-tasks.md) kokoonpano-asetus määrittää tehtävät, jotka voidaan suorittaa rinnakkain kunkin ryhmän Laske oleva solmu enimmäismäärä.

    Oletusmääritys on yksi tehtävä kerrallaan suoritetaan solmun, mutta mahdollista, jossa on hyötyä on useampi kuin yksi tehtävä, joka suoritetaan solmun samanaikaisesti. Katso [Esimerkkitapaus](batch-parallel-node-tasks.md#example-scenario) [samanaikainen solmu tehtävät](batch-parallel-node-tasks.md) -artikkelin näet, miten voit hyötyä useita tehtäviä solmu kohden.

    Voit myös määrittää *Täytön tyyppi* , joka määrittää, onko erä näkymät tehtävät tasaisesti yli resurssivarantoon kaikissa solmuissa vai pakkaa kunkin solmu tehtävien enimmäismäärä ennen tehtävien määrittäminen toisen solmun.

- Laske solmujen **tietoliikenteen tila**

    Useimmissa tapauksissa tehtäviä toimia itsenäisesti ja ei tarvitse toisiinsa. On kuitenkin sovelluksia, jossa tehtävien ilmaistava, kuten [MPI skenaarioita](batch-mpi.md).

    Voit määrittää, jotta se--**HalkaisijaSolmuvälin viestintä**sijaitsevien solmujen välisen resurssivarantoon. Kun HalkaisijaSolmuvälin viestintä on otettu käyttöön, Cloud Services-kokoonpanon määritys jakavat solmujen viestiä keskenään suurempi kuin 1100 ja virtuaalikoneen määritysten jakavat eivät rajoita liikenne mihin tahansa porttiin.

    Huomaa, että HalkaisijaSolmuvälin viestintää ottaminen käyttöön myös vaikuttaa klustereiden sijaitsevien solmujen sijaintia ja resurssivarantoon solmujen enimmäismäärä saattaa rajoittaa käyttöönottoa rajoitusten vuoksi. Jos sovellusta ei edellytä välisen solmujen, erä-palvelun voit kohdistaa mahdollisesti paljon solmujen resurssivarantoon useita eri klustereiden- ja palvelinkeskusten käyttöön kasvattaa rinnakkain käsittely power.

- Laske solmujen **Aloita tehtävä**

    *Aloita tehtävä* valinnainen suorittaa kunkin solmun solmun yhdistää altaan ja aina, kun solmu käynnistetään uudelleen tai reimaged. Käynnistä tehtävä on hyötyä erityisesti Laske solmujen valmisteleminen tehtäviä, kuten tehtävien suorittaminen solmuissa suoritettavien sovellusten asentaminen suorittamisen.

- **Sovelluksen-paketit**

    Voit määrittää [sovelluksen pakettien](#application-packages) ryhmän Laske solmujen käyttöön. Sovelluksen pakettien avulla yksinkertaistetun käyttöönoton ja sovelluksia, jotka tehtävien suorittaminen versiotietojen. Sovelluksen-paketteja, jotka määrität varanto on asennettu jokaisen solmun, joka yhdistää kyseisen ryhmän ja aina, kun solmu on käynnistettävä tai reimaged. Sovelluksen paketit ovat ei tällä hetkellä tueta Linux Laske solmuissa.

- **Verkon määrittäminen**

    Voit määrittää tunnus, jossa ryhmän Laske solmujen Azure [virtual verkon (VNet)](../virtual-network/virtual-networks-overview.md) , luodaanko. Vaatimukset VNet, joka määrittää, että sovellussarjan löytyvät [lisääminen resurssivarantoon tiliin] [ vnet] erä REST API viittauksen.

> [AZURE.IMPORTANT] Kaikki erän-tileillä on oletusarvo- **kiintiön** , joka rajoittaa **sydämiä** (ja täten Laske solmujen) erä tilillä. Voit etsiä oletusarvon kiintiön ja ohjeet (kuten sydämiä erä-tilisi enimmäismäärä) [kiintiön Suurenna](batch-quota-limit.md#increase-a-quota) [kiintiön](batch-quota-limit.md)ja Azure erä palvelun rajoitukset. Jos tiedä, jossa kysytään, "Miksi ei Omat resurssivarantoon saavuttaa yli X solmujen?" Syynä voi olla core-kiintiön.

## <a name="job"></a>Työn

Työ on joukko tehtäviä. Miten laskenta suoritetaan sen tehtävien suorittaminen solmujen resurssivarantoon hallitsee.

- Työn määrittää **resurssivarantoon** , joiden työ on suoritettava. Voit luoda kullekin projektille uusi ryhmä tai käyttää yhteen resurssivarantoon monta työtä. Voit luoda resurssivarantoon kunkin projektin, joka on liitetty projektin aikataulun, tai kaikissa projekteissa, jotka liittyvät projektin aikataulun.

- Voit määrittää valinnainen **työn prioriteetti**. Työn lähettämisen yhteydessä suurempi prioriteetti kuin käynnissä olevat työt, tärkeämpään projektin tehtävät lisätty jonossa ennen tehtävien pienempi prioriteetti-projekteille. Tehtävien pienempi prioriteetti työt, jotka on jo käytössä ei ole pyytävä.

- Voit käyttää projektin **rajoitteiden** määrittäminen työt tiettyjä rajoituksia:

    Voit määrittää **enintään wallclock ajan**niin, että projektin käytetään kauemmin kuin suurin wallclock aika, joka on määritetty, työ- ja kaikki sen tehtävät lopetetaan.

    Erän voit tunnistaa ja yritä epäonnistui tehtävät. Voit määrittää **tehtävän uudelleenyritykset enimmäismäärä** rajoitus, onko tehtävä on *aina* tai *koskaan* uudelleen. Yritetään tehtävän tarkoittaa, että tehtävän uudelleen suorittaa uudelleen.

- Asiakassovellus, voit lisätä tehtäviä projektiin, tai voit määrittää [Projektitehtävä hallinta](#job-manager-task). Hallinnan projektitehtävä sisältää tiedot, jotka on luotava tarvittavat tehtävät, projektin projektitehtävä suoritettavan jonkin ryhmän Laske solmujen hallinnan kanssa. Hallinnan projektitehtävä käsitellään erityisesti erä--mukaan se on jonossa heti, kun työ on luotu ja käynnistetään, jos se ei onnistu. Hallinnan projektitehtävä on *tarvittavat* työt, jotka on luotu [projektin aikataulua](#scheduled-jobs) , koska se on ainoa tapa määrittää tehtäviä, ennen kuin työn esiintymää.

- Oletusarvon mukaan työt pysyvät aktiivinen-tilassa, kun kaikki tehtävät töiden ovat valmiit. Voit muuttaa ongelman niin, että projektin lopetetaan automaattisesti, kun kaikki projektin tehtävät ovat valmiit. Projektin **onAllTasksComplete** ominaisuuden ([OnAllTasksComplete] [ net_onalltaskscomplete] erä .NET-) voit lopettaa työn automaattisesti, kun kaikki sen tehtävien on valmis-tilan *terminatejob* .

    Huomautus erä palvelun katsoo projektin tehtäviä *ei ole* määrittänyt kaikki sen suoritettujen tehtävien kanssa. Tämä asetus käytetään vuoksi yleisimmin [Projektitehtävä hallinnan](#job-manager-task)kanssa. Jos haluat käyttää automaattista työn tilauksen ilman projektin hallinta, sinun olisi alun perin uuden työn **onAllTasksComplete** -ominaisuuden arvoksi *noaction*ja valitse määrittäminen *terminatejob* vasta, kun olet lisännyt tehtäviä projektiin.

### <a name="job-priority"></a>Projektin prioriteetin

Voit määrittää prioriteetti työt, jotka voit luoda erää. Erän palvelun käyttää määrittääkseen järjestyksen ajoitettaessa (tämä ei saa sekoittaa [ajoitetun työn](#scheduled-jobs)) asiakas työn prioriteetti-arvoa. Prioriteetti arvot väliltä 1 000, jossa-1000 on pienin prioriteetti ja 1 000 suurimman-1000. [Päivitä projektin ominaisuuksien] avulla voit päivittää projektin prioriteetin[ rest_update_job] toiminnon (erä REST) tai muokkaamalla [CloudJob.Priority] [ net_cloudjob_priority] ominaisuuden (erä .NET).

Saman tilin tärkeämpään työt on Ajoittaminen ohittaa pienempi prioriteetti työt päälle. Työ-tilien tärkeämpään arvo ei ole Ajoittaminen ohittaa päälle toisen projektin eri tilillä pienempi prioriteetti-arvolla.

Työn ajoittaminen yli jakavat on erillinen. Eri jakavat välillä on ei välttämättä, tärkeämpään työ ajoitetaan ensimmäisen kerran, jos sen liittyvät varanto on käyttämättömänä solmujen lopussa. Saman varannon työt, joilla on sama prioriteettitaso on yhtä mahdollisuutta ajoitus.

### <a name="scheduled-jobs"></a>Ajoitetuissa

[Projektin aikatauluja] [ rest_job_schedules] , joiden avulla voit luoda erää palvelun toistuvan töitä. Projektin aikataulun määrittää, milloin haluat suorittaa töitä, ja se sisältää työt voidaan suorittaa määrityksiä. Voit määrittää keston aikataulun--keston ja kun aikataulu on voimassa--ja kuinka usein, joka työt voidaan luoda ajanjakson aikana.

## <a name="task"></a>Tehtävä

Tehtävä on laskenta, joka on liitetty työn yksikköä. Se toimii solmu. Tehtävät määritetään suorittamisen solmu tai on jonossa, kunnes solmu muuttuu vapaa. Sijoittaa yksinkertaisesti, tehtävän suorittaa ohjelmia tai komentosarjoja Laske solmun tekemässä töitä, sinun on valmis.

Kun olet luonut tehtävän, voit määrittää:

- **Komentorivin** tehtävän. Tämä on komentorivi, joka suoritetaan sovelluksen tai komentosarjan suorittaminen-solmu.

    On tärkeää muistaa, että komentorivin ei varsinaisesti Suorita on kohdassa. Tämän vuoksi se et voi grafiikkatiedostomuotoja voit hyödyntää shell-ominaisuuksia, kuten [ympäristömuuttuja](#environment-settings-for-tasks) laajentamista (mukaan lukien `PATH`). Voit hyödyntää esimerkiksi ominaisuuksia, on suoritat shell komentorivillä--esimerkiksi mukaan avaamista `cmd.exe` Windows solmuissa tai `/bin/sh` Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Jos tehtävät on suoritettava sovellus tai komentosarjan, joka ei ole solmun `PATH` tai viittaus ympäristömuuttujat, Avaa erikseen tehtävän komentorivi-käyttöliittymä.

- **Resurssitiedostoja** , joissa käsitellään tiedot. Nämä tiedostot kopioidaan automaattisesti solmun Blob-objektien tallennustilaan **yleisiä tarkoitus** Azuren tallennustilaan tilillä ennen tehtävän komentoriviltä on suoritettu. Lisätietoja on kohdassa Rivijäsennyksen [Aloita tehtävä](#start-task) ja [tiedostoja ja kansioita](#files-and-directories).

- **Ympäristömuuttujat** , joita tarvitaan sovelluksen mukaan. Lisätietoja on kohdassa [ympäristöasetuksia tehtävien](#environment-settings-for-tasks) .

- **Rajoitukset** vallitessa tehtävän suorittaminen. Esimerkiksi enimmäisajan, tehtävän voi suorittaa kuinka monta kertaa epäonnistui tehtävää yritetään- ja suurin kellonajan kyseisen tehtävän työkansion tiedostot säilyvät.

- **Sovelluksen paketteja** , tehtävä ajoitetaan suorittaa Laske-solmu käyttöön. [Sovelluksen pakettien](#application-packages) avulla yksinkertaistetun käyttöönoton ja sovelluksia, jotka tehtävien suorittaminen versiotiedot. Tehtävätason sovelluksen paketit ovat erityisen hyödyllisiä jaettu resurssivarantoon ympäristöissä, jossa eri työt suoritetaan yhteen resurssivarantoon ja varannon ei poisteta, kun työ on valmis. Jos työsi on vähemmän kuin solmujen tehtäviä ryhmän, tehtävän sovelluksen pakettien pienentää tiedonsiirron jälkeen sovellus otetaan käyttöön ainoastaan solmut, joka suorittaa tehtäviä.

Merkityt tehtävät, voit määrittää tekemässä laskenta-solmu seuraavat määräten tehtävät myös tarjoamien erä-palvelu:

- [Aloita tehtävä](#start-task)
- [Projektitehtävä hallinta](#job-manager-task)
- [Valmistelut ja vapauta projektitehtävät](#job-preparation-and-release-tasks)
- [Usean esiintymän tehtäviä (MPI)](#multi-instance-tasks)
- [Tehtävien riippuvuudet](#task-dependencies)

### <a name="start-task"></a>Aloita tehtävä

Liittäminen resurssivarantoon **käynnistää tehtävää** , voit valmistella sen solmujen käyttöympäristöstä. Voit esimerkiksi suorittaa toimintoja, kuten asentamalla sovellukset, jotka tehtävien suorittaminen ja taustaprosessit alkaen. Käynnistä tehtävän suoritetaan aina, kun solmu käynnistyy, kunhan se jää resurssivarantoon--myös silloin, kun solmu on ensimmäinen lisätään altaan ja kun se käynnistetään uudelleen tai reimaged.

Käynnistä tehtävän ensisijainen etuna on se, että se sisältää kaikki tiedot, joita tarvitaan Laske-solmu ja asentaa sovellukset, joita tehtävän suorittaminen edellyttää. Tämän vuoksi lisääminen resurssivarantoon näkyvien solmujen määrän onnistuu vain määrittämällä uusi kohde solmun Laske--erä jo on tietoja, jotka tarvitaan, jotta voidaan määrittää uusien solmujen ja poistaa ne valmiit tehtävät hyväksymistä varten.

Azure erä mitä tahansa tehtävään, jossa voit määrittää **resurssitiedostojen** luettelo [Azuren]tallennustilaan[azure_storage]- **komento** , joka suoritetaan lisäksi. Erän kopioi ensin solmun resurssitiedostoja Azuren tallennustilaan ja suorittaa sitten komentorivillä. Tiedostoluettelon sisältää yleensä tehtävä-sovellus ja riippuvaa ryhmän Käynnistä-tehtävän.

Se voi myös lisätä viitetiedot käytettävän kaikkien tehtävien suorittaminen solmun käynnissä olevat. Esimerkiksi Käynnistä tehtävän komentoriviltä voi suorittaa `robocopy` toiminto kopioi sovelluksen-tiedostot (joka on määritetty resurssitiedostojen ja ladata solmun) Aloita tehtävä [toimi directory](#files-and-directories) [jaettuun kansioon](#files-and-directories)ja suorita MSI tai `setup.exe`.

> [AZURE.IMPORTANT] Erän tällä hetkellä tukee *vain* **yleisiä tarkoitus** tallennustilan tilin tyyppi-vaiheessa 5 [Luo tallennustilan tilin](../storage/storage-create-storage-account.md#create-a-storage-account) [tietoja Azure](../storage/storage-create-storage-account.md)-tallennustilan tilin kuvatulla tavalla. Erän tehtävien (mukaan lukien vakio tehtävät, Käynnistä tehtävät, projektitehtävät valmistelu ja release projektitehtävien) on määritettävä resurssi-tiedostoihin, jotka sijaitsevat *vain* **yleisiä tarkoitus** tallennustilan tilit.

Yleensä olisi odottamaan Käynnistä tehtävä on valmis ennen kuin harkitset valmis varataan tehtäviä solmu erä-palvelua, mutta voit määrittää tämän.

Jos Käynnistä tehtävän epäonnistuu Laske solmun, sitten solmun tilan päivitetään vastaamaan virheen ja solmu ei ole määritetty tehtäviä. Käynnistä tehtävän saattaa epäonnistua, jos on sen resurssitiedostojen kopioiminen tallennustilan ongelma tai jos sen komentoriviltä suorittaa prosessin palauttaa erisuuri kuin nolla lopetuskoodi.

Jos Lisää tai Päivitä aiemmin *luodun* ryhmän Käynnistä tehtävä, sinun on käynnistettävä uudelleen sen Laske solmujen Käynnistä tehtävän solmut käytettäväksi.

### <a name="job-manager-task"></a>Projektitehtävä hallinta

Voit yleensä käyttää **hallinta projektitehtävä** ohjausobjektiin ja/tai seurata projektin suorittaminen – esimerkiksi luoda ja lähettää projektin tehtäviä, määrittävät tehtävien suorittamiseen ja määrittää työn ollessa. Projektitehtävä hallinta ei kuitenkaan rajoitettu aktiviteetteja. On täysin fledged tehtävä, jonka voit suorittaa toimintoja, jotka tarvittavat resurssit. Hallinnan projektitehtävä voi esimerkiksi Lataa tiedosto, joka on määritetty parametri, tiedoston sisällön analysoi ja lähetä ne sisällön perusteella lisätoimien suorittamista.

Projektitehtävä hallinta aloitetaan ennen muihin tehtäviin. Se sisältää seuraavat ominaisuudet:

- Se lähetetään automaattisesti tehtäväksi erä-palvelun työn luomisen yhteydessä.

- Se on ajoitettu ennen projektin muiden tehtävien suorittamiseen.

- Sen solmun on viimeiseksi altaan on parhaillaan lisätyt resurssivarantoon poistetaan.

- Projektin kaikkien tehtävien päättyminen voi liittää sen tilauksen.

- Hallinnan projektitehtävä annetaan suurimman prioriteetin, kun se on käynnistettävä. Jos käyttämättömänä solmu ei ole käytettävissä, erä-palvelun ehkä Lopeta jokin muiden Vapauta hallinta projektitehtävälle suorittaa varannon käynnissä olevat tehtävät.

- Yhden projektin tehtävän työn hallinta ei ole prioriteetti muiden projektien tehtävien päälle. Yli töitä havaitaan vain työn tason prioriteetit.

### <a name="job-preparation-and-release-tasks"></a>Valmistelut ja vapauta projektitehtävät

Erän on valmistelu projektitehtävien vanhat työn suorittamisen asetukset. Työn release tehtävät ovat jälkeinen työn ylläpito-tai uudelleenjärjestäminen.

- **Projektitehtävä valmistelu**: valmistelu projektitehtävä käytetään kaikissa Laske solmuissa, jotka on suunniteltu toimimaan tehtävien ennen muita projektin tehtäviä suoritetaan. Voit kopioida tietoja, jotka on jaettu kaikkien tehtävien, mutta se on yksilöllinen projektiin, esimerkiksi projektin valmistelu tehtävän.
- **Projektitehtävän vapauttaminen**: kun työ on valmis, Vapauta projektitehtävä toimii kunkin solmun sarjassa, joka suorittaa vähintään yksi tehtävä. Voit poistaa tietoja, jotka on kopioitu valmistelu projektitehtävä mukaan voit pakata ja lataa diagnostiikan tietoja, kuten release projektitehtävä.

Molemmat projektin valmistelu ja vapauta tehtävien avulla voit määrittää komentorivi, joka suoritetaan, kun tehtävä on kutsuttu. Ne tarjoavat ominaisuuksia, kuten tiedostojen lataaminen, laajennettuja suorittamisen, mukautetut ympäristömuuttujat, suurin suorittamisen kesto, yritä määrä ja tiedoston säilytys aikaa.

Saat lisätietoja projektitehtävien valmistelu ja vapauta [asennuksen valmistelu ja valmistuminen projektitehtävien Azure erän Laske solmujen](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Usean esiintymän tehtävä

[Usean esiintymän tehtävän](batch-mpi.md) on tehtävä, joka on määritetty suorittamaan useamman kuin yhden Laske solmun samanaikaisesti. Usean esiintymän tehtäviä voit ottaa tehokas tietojenkäsittely skenaariot, jotka tarvitsevat ryhmän Laske solmujen, joka on kohdistettu yhdessä käsittelemään yksittäisen kuormituksen (kuten viestin kulkeva Interface (MPI)).

Yksityiskohtainen keskustelun suorittamisesta MPI työt osalta erä .NET-kirjastossa käyttämällä lukea [Käytä usean esiintymän tehtävät viestin kulkeva Interface (MPI)-sovelluksia Azure osalta](batch-mpi.md).

### <a name="task-dependencies"></a>Tehtävien riippuvuudet

[Tehtävien riippuvuudet](batch-task-dependencies.md), kuten nimensä avulla voit määrittää tehtävän määräytyy ennen sen suorittamisen muiden tehtävien suorittamista. Tämä toiminto tukee tilanteita, joissa "alaspäin" tehtävän siinä käytetään tulosteen "ylöspäin" tehtävän--tai ylöspäin tehtävän suorittaa joitakin alustus, joita tarvitaan edeltävät tehtävän mukaan. Voit käyttää tätä toimintoa, sinun on otettava tehtäväriippuvuudet erän työn. Valitse kunkin tehtävän toiseen määräytyy eli monista muista, määrittämäsi tehtävät, joita riippuvainen tehtävä.

Tehtävien riippuvuudet, jossa voit määrittää skenaarioita, kuten jompikumpi seuraavista:

* *taskB* määräytyy *taskA* (*taskB* eivät ala suorittamisen ennen kuin *taskA* on valmis).
* *taskC* riippuu *taskA* ja *taskB*.
* *taskD* määräytyy tehtäviä, kuten tehtävien *1* – *10*-alueen ennen.

Tutustu [tehtäväriippuvuudet Azure osalta](batch-task-dependencies.md) ja [TaskDependencies] [ github_sample_taskdeps] koodin otoksen [azure-erä-objektit] [ github_samples] GitHub säilöön yksityiskohtaisempia lisätietoja toiminnosta.

## <a name="environment-settings-for-tasks"></a>Tehtävien ympäristöasetuksia

Kunkin tehtävän suorittaminen erä-palvelu on ympäristömuuttujat, se asettaa Laske solmuissa käyttöoikeus. Tämä vaihtoehto sisältää erä palvelun määrittämät ympäristömuuttujat ([service-määritysten][msdn_env_vars]) ja mukautetun ympäristömuuttujia, voit määrittää tehtäviin. Sovellusten ja komentosarjojen tehtävien suorittaminen on pääsy ympäristömuuttujia suorituksen aikana.

Voit määrittää mukautetun ympäristömuuttujat tehtävän tai projektin tasolla täyttää näiden kohteiden *ympäristöasetuksia* -ominaisuuden avulla. Esimerkiksi näkyviin [Lisää työn tehtävä] [ rest_add_task] käyttö (erä REST API) tai [CloudTask.EnvironmentSettings] [ net_cloudtask_env] ja [CloudJob.CommonEnvironmentSettings] [ net_job_env] erä .NET-ominaisuuksia.

Sovelluksen tai palvelun pystyisivät tehtävän ympäristömuuttujat, sekä palvelun määritetyt ja mukautetut, käyttämällä [tehtävän tietoja] [ rest_get_task_info] toiminnon (erä REST) tai muuttamalla [CloudTask.EnvironmentSettings] [ net_cloudtask_env] ominaisuuden (erä .NET). Prosessit suoritetaan Laske solmun käyttää näitä ja muiden ympäristömuuttujien solmun, esimerkiksi käyttämällä tuttuja `%VARIABLE_NAME%` (Windows) tai `$VARIABLE_NAME` (Linux)-syntaksin.

Löydät luettelon kaikista kaikki palvelun määrittämät ympäristömuuttujat [Laske solmu ympäristömuuttujat][msdn_env_vars].

## <a name="files-and-directories"></a>Tiedostojen ja kansioiden

Kunkin tehtävän on *toimi directory* vallitessa luomilleen nolla tai useita tiedostoja ja kansioita. Tämän työpäivän hakemiston voidaan ohjelmaan, joka suoritetaan tehtävän, joka käsittelee tietoja ja suorittaa käsittely tulosteen tallentamista varten. Kaikki tiedostot ja kansiot tehtävän omistaja tehtävän käyttäjän.

Erän palvelun paljastaa tiedostojärjestelmän solmun osa nimellä *pääkansion*. Tehtävät voivat käyttää pääkansion viittaamalla `AZ_BATCH_NODE_ROOT_DIR` ympäristömuuttuja. Saat lisätietoja ympäristömuuttujien käyttäminen [ympäristöasetuksia tehtävien](#environment-settings-for-tasks).

Pääkansio sisältää seuraava kansiorakenne:

![Laske solmu kansiorakenne][1]

- **jaetun**: Tämä kansio sisältää *kaikki* tehtävät, jotka toimivat solmu luku-/ kirjoitusoikeudet. Mitä tahansa tehtävään, joka suoritetaan solmun voit luoda, lukea, päivittää ja poistaa tämän kansion tiedostoja. Tehtävät voivat käyttää tämän kansion viittaamalla `AZ_BATCH_NODE_SHARED_DIR` ympäristömuuttuja.

- **Käynnistys**: kansion käyttämän Käynnistä tehtävän sen työkansion. Kaikki solmun Käynnistä tehtävän ladattavia tiedostoja tallennetaan tähän. Käynnistä tehtävän voit luoda, lukea, päivittää ja poistaa tiedostoja, valitse tämä kansio. Tehtävät voivat käyttää tämän kansion viittaamalla `AZ_BATCH_NODE_STARTUP_DIR` ympäristömuuttuja.

- **Tehtävät**: kansio luodaan solmu toimii kunkin tehtävän. Asetus on käytettävissä viittaamalla `AZ_BATCH_TASK_DIR` ympäristömuuttuja.

    Kunkin tehtävän hakemistossa erä palvelun Luo käytön hakemiston (`wd`) jonka yksilöllisen polun määrittämän `AZ_BATCH_TASK_WORKING_DIR` ympäristömuuttuja. Tämä kansio on luku-/ kirjoitusoikeudet tehtävään. Tehtävän voit luoda, lukea, päivittää ja poistaa tiedostoja, valitse tämä kansio. Tähän kansioon säilyttää *RetentionTime* -rajoitus, joka on määritetty tehtävälle perusteella.

    `stdout.txt`ja `stderr.txt`: nämä tiedostot kirjoitetaan tehtävien tehtävän suorituksen aikana.

>[AZURE.IMPORTANT] Kun solmu poistetaan olevilla, *kaikki* tiedostot, jotka on tallennettu solmu poistetaan.

## <a name="application-packages"></a>Sovelluksen-paketit

[Sovelluksen pakettien](batch-application-packages.md) -toiminto tarjoaa asetusten hallinta ja että jakavat Laske solmujen sovellusten käyttöönotto. Voit ladata ja hallita useita versioita suorittaa tehtäviä, mukaan lukien niiden binaaritiedostot ja tukitiedostot sovellukset. Sitten voit automaattisesti otetaan käyttöön yhden tai useamman Laske solmut näiden sovellusten lisääminen resurssivarantoon.

Voit määrittää sovelluksen pakettien resurssivarantoon ja tehtävän tasolla. Resurssivarannon sovelluksen pakettien määrittäessäsi sovellus otetaan käyttöön kunkin solmun altaan. Kun määrität tehtävän sovelluksen paketteja, sovellus otetaan käyttöön vain solmut, jotka on suunniteltu toimimaan vähintään yhden projektin tehtävät, ennen kuin tehtävän komentoriviltä suoritetaan.

Erän käsittelee tietoja Azure-tallennustilan, voit tallentaa sovelluksen-paketit ja ota ne laskemiseen solmujen, jotta koodi ja hallinnan katseltavan voidaan yksinkertaistaa käsitteleminen.

Lisätietoa [sovellusten käyttöönoton Azure erä sovelluksen pakettien](batch-application-packages.md)Lisätietoja sovelluksen paketti-ominaisuutta.

>[AZURE.NOTE] Jos lisäät ryhmän sovelluksen pakettien aiemmin *luotuun* ryhmään, sinun on käynnistettävä uudelleen sen Laske solmujen sovelluksen pakettien solmut käyttöön.

## <a name="pool-and-compute-node-lifetime"></a>Resurssivaranto ja Laske solmu elinaika

Suunnitellessasi Azure erä ratkaisu on rakenne päätös tietoja ja tehdä, kun jakavat luodaan, ja kuinka kauan Laske ne jakavat sijaitsevien solmujen säilytetään käytettävissä.

Eri toisessa päässä on luoda varannon kunkin työn, voit lähettää ja poistaa ryhmän heti, kun sen tehtävien suorittaminen loppuun. Tämä suurentaa käytön, koska solmut varataan vain, kun tarvitaan ja sammuta heti, kun ne ovat vapaana. Kun tämä tarkoittaa, että työhön on odotettava solmujen, jonka haluat kohdentaa on tärkeää muistaa, että tehtävät ajoitetaan suorittamista varten heti, kun solmut ovat käytettävissä erikseen kohdistettu, ja Käynnistä tehtävä on suoritettu. Onko erä *ei* Odota, kunnes kaikki resurssivarantoon sijaitsevien solmujen ovat käytettävissä, ennen kuin tehtävien määrittäminen solmut. Tällä varmistetaan, että kaikki käytettävissä olevat solmut suurin sallittu käyttö.

Toisessa päässä kierrettyjen, jos aloittaa heti työt on suurin prioriteetti voit luoda etuajassa resurssivarantoon ja tee sen solmujen käytettävissä, ennen kuin töitä lähetetään. Tässä skenaariossa tehtävät voidaan aloittaa heti, mutta solmujen ehkä istut käyttämättömänä odotettaessa ne liitetään.

Yhdistetty lähestymistapa käytetään yleensä muuttujan, mutta meneillään, kuormituksen käsittelyyn. Sinulla voi olla useita töitä lähetetään, mutta se voi skaalata resurssivarantoon solmujen ylös tai alas mukaan työn määrä lataaminen (Lisätietoja on seuraavassa osassa [Skaalaus Laske resursseja](#scaling-compute-resources) ). Voit tehdä tämän reactively, perusteella nykyisen lataamisen, tai kun muutoksista, jos kuormituksen voi ennustaa.

## <a name="scaling-compute-resources"></a>Laske resurssien skaalaus

[Automaattinen skaalaus](batch-automatic-scaling.md)sijoittamisen erä palvelun Säädä dynaamisesti mukaan Laske-skenaario nykyisen työmäärän ja resurssien käyttö resurssivarantoon Laske näkyvien solmujen määrän. Voit pienentää käynnissä sovelluksen käyttämällä vain tarvittavat resurssit ja vapauttaminen tarpeettomia kokonaiskustannukset.

Voit ottaa käyttöön automaattisen skaalauskertoimen kirjoittamisen jälkeen [Automaattinen skaalauksen kaavan](batch-automatic-scaling.md#automatic-scaling-formulas) ja kaavan liittäminen resurssivarantoon. Erän palvelun käyttää kaavan määrittääkseen altaan solmujen määrän kohde seuraava skaalauksen aikavälin (aikaväli, voit määrittää). Voit määrittää resurssivarantoon automaattinen Skaalausasetusten, kun se on luotu tai otat skaalaus resurssivarantoon myöhemmin. Voit myös päivittää skaalaus käyttävä resurssivarantoon skaalauksen asetuksia.

Esimerkkinä esimerkiksi projektin edellyttää Lähetä tehtävät suoritetaan erittäin suuri määrä. Voit määrittää skaalauksen kaavan, joka perustuu jonossa olevien tehtävien määrä ja projektin tehtävät valmistuminen korkokannan altaan näkyvien solmujen määrän säätää ryhmään. Erän palvelun säännöllisesti laskee kaavan ja mukautuu resurssivarantoon kuormituksen perusteella (solmujen monta jonossa olevien tehtävien lisääminen ja poistaminen ei ole jonossa tai käynnissä tehtävien solmut) ja muita kaavan asetuksiasi.

Skaalauksen kaavan voi perustua seuraavat arvot:

- **Aika-arvot** perustuvat tilastotiedot kerätään minuutin välein viiden tunnin määritetty määrä.

- **Resurssin arvot** perustuvat suorittimen käyttö, kaistanleveyden käytön, muistinkäytön ja solmujen määrän.

- **Tehtävän arvot** perustuvat tehtävän tila, kuten *Active* (jonossa), *käynnissä*tai *Valmis*.

Kun Automaattinen skaalaus pienentää resurssivarantoon Laske näkyvien solmujen määrän, ota huomioon käsittelemisestä vähenemisen toiminnon milloin käynnissä olevat tehtävät. Sopimaan tämä erä on *solmu tilauksen poisto-vaihtoehdon* , jotka voit sisällyttää kaavoissa. Voit esimerkiksi määrittää, että käynnissä olevien tehtävien pysäytetty heti, pysäytetty välittömästi ja sitten uudelleen toisen solmun suorittamista varten vai oikeus valmis ennen solmu poistetaan olevilla.

Saat lisätietoja skaalaus automaattisesti sovelluksen [automaattisesti asteikko Laske solmujen Azure erä resurssivarantoon](batch-automatic-scaling.md).

> [AZURE.TIP] Voit suurentaa Laske resurssien käyttö-Määritä solmujen työn lopussa nolla, mutta salli suoritettavat tehtävät Viimeistele kohde.

## <a name="security-with-certificates"></a>Arvopaperille, jonka varmenteet

Sinun on yleensä varmenteiden käytöstä, kun salaaminen ja salauksen luottamuksellisia tietoja, tehtävien, kuten [Azure-tallennustilan tilin]näppäintä[azure_storage]. Tukee tätä, voit asentaa varmenteet solmuissa. Salatun tietoja välitetään tehtävät komentoriviparametreja kautta tai yksi tehtävä resursseista upotettuna ja purkaa niiden avulla voidaan asennettu varmenteet.

[Lisää varmenteen] käyttää[ rest_add_cert] toiminnon (erä REST) tai [CertificateOperations.CreateCertificate] [ net_create_cert] menetelmä (erä .NET) voit lisätä varmenteen erä-tiliin. Voit yhdistää sitten varmenteen uuteen tai aiemmin luotuun ryhmään. Varmenteen ollessa yhdistettynä resurssivarantoon erä palvelun asentaa varmenteen kunkin ryhmän solmu. Erän palvelu asentaa sopivat sertifikaatit, kun solmu käynnistyy, ennen avaamista (mukaan lukien tehtävän alkamis- ja hallinnan projektitehtävä) tehtävät.

Jos lisäät varmenteet aiemmin *luotuun* ryhmään, sinun on käynnistettävä uudelleen sen Laske solmujen varmenteiden solmut käytettäväksi.

## <a name="error-handling"></a>Virheenkäsittely

Voi olla helpompaa tarvittavat tehtävä- ja sovelluksen virheiden erä-ratkaisussa.

### <a name="task-failure-handling"></a>Tehtävän virheen käsittely
Nämä luokkiin tehtävän virheitä:

- **Ajoituksen virheet**

    Jos tiedostot, jotka on määritetty tehtävälle siirto epäonnistuu jostakin syystä, "ajoituksen virhe" on määritetty tehtävälle.

    Ajoituksen virheitä voi ilmetä, jos siirtänyt tehtävän resurssitiedostojen tallennustilan tiliä ei ole enää käytettävissä tai toinen ongelma ilmeni, joka esti onnistuneen kopiointi tiedostot solmun.

- **Sovellusvirheet**

    Prosessi, joka määrittää tehtävän komentoriviltä saattaa epäonnistua myös. Prosessin katsotaan epäonnistuneen kun erisuuri kuin nolla lopetuskoodi palauttama prosessi, jossa on suorittaa tehtävän (katso *tehtävien koodit Lopeta* seuraavan osion).

    Sovellusvirheet voit määrittää erä automaattisesti uudelleen tehtävän ylöspäin tietyn määrän kertoja.

- **Rajoite-virheet**

    Voit määrittää rajoituksen, joka määrittää projektin tai tehtävän *maxWallClockTime*suurin suorittamisen. Tämä voi olla hyötyä lopetetaan "telineille" tehtävät.

    Kun ajan enimmäismäärä on ylitetty-tehtävä on merkitty *Valmis*, mutta lopetuskoodi on määritetty `0xC000013A` ja *schedulingError* -kenttä on merkitty `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Virheenkorjaus sovellusvirheet

- `stderr`ja`stdout`

    Suorituksen aikana sovellus voi tuottaa diagnostiikan tulosteen, jotka voit tehdä ongelmien vianmäärityksen. Kuten aiemmassa osassa [tiedostoja ja kansioita](#files-and-directories), erä-palvelun kirjoittaa vakio tulostus ja keskivirhe tulosteen `stdout.txt` ja `stderr.txt` tiedostojen hakemistossa tehtävän suorittaminen-solmu. Voit ladata nämä tiedostot Azure portaalin tai jokin erä SDK: T avulla. Voit esimerkiksi noutaa nämä ja muut tiedostot vianmääritystä käyttämällä [ComputeNode.GetNodeFile] [ net_getfile_node] ja [CloudTask.GetNodeFile] [ net_getfile_task] erä .NET-kirjastossa.

- **Tehtävän lopetuskoodit**

    Kuten edellä mainittiin, tehtävä näkyy epäonnistui erä-palvelu, jos prosessi, jossa on suorittaa tehtävän palauttaa erisuuri kuin nolla lopetuskoodi. Kun tehtävän suorittaa prosessin, erä lisää tehtävän Lopeta koodin ominaisuus *palauttaa koodin yhteydessä*. On tärkeää muistaa, että tehtävän lopetuskoodi on **ei** määräytyy erä--palvelun se määritetään prosessin itse tai käyttöjärjestelmä, jossa prosessi suoritetaan.

### <a name="accounting-for-task-failures-or-interruptions"></a>Tehtävän virheet tai keskeytyksiä laskenta

Tehtäviä voi toisinaan epäonnistua tai että sinua häiritään. Tehtävän sovelluksesta voi epäonnistua solmu, jolloin tehtävä on käynnissä ehkä käynnistettävä tai solmun saatetaan poistaa ryhmän koonmuutto-toiminnon aikana Jos ryhmän tilauksen poisto käytäntö määritetään Poista solmujen heti odottamatta tehtävien loppuun. Kaikissa tapauksissa tehtävän voi olla automaattisesti uudelleen mukaan erä toisen solmun suorittamista varten.

On myös mahdollista katkonainen ongelma johtuu jumittua tai kestää liian kauan suorittaa tehtävän. Voit määrittää tehtävän enimmäisajan suorittamisen. Jos se on ylitetty-erä keskeyttää tehtävän-sovellus.

### <a name="connecting-to-compute-nodes"></a>Laskemiseen solmujen yhdistäminen

Voit suorittaa muita virheenkorjaus ja kirjautumalla sisään Laske solmu etäyhteyden vianmääritys. Voit ladata Windows solmujen Remote Desktop Protocol (RDP)-tiedostona ja hankkia suojattu runko (SSH) yhteystiedot Linux solmujen Azure portaalin. Voit tehdä tämän myös käyttämällä erä API – esimerkiksi [Erä .NET] [ net_rdpfile] tai [Erä Python](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] Muodostaa yhteyttä RDP tai SSH solmu sinun on luotava käyttäjän solmun. Voit tehdä tämän käyttämällä Azure portaalin, [Lisää solmu käyttäjätili] [ rest_create_user] käyttämällä erä REST API-kutsu [ComputeNode.CreateComputeNodeUser] [ net_create_user] menetelmä erä .NET tai puhelun aikana [add_user] [ py_add_user] menetelmä erä Python-moduulissa.

### <a name="troubleshooting-bad-compute-nodes"></a>Vianmäärityksen "Virheellinen" Laske solmujen

Tilanteissa, joissa jotkin tehtävät epäonnistuvat erä sovelluksen tai palvelun voit tutkia epäonnistui tehtävät kunnolla solmun tunnistamiseen metatiedot. Kukin solmu resurssivarantoon annetaan yksilöllisen tunnuksen ja solmu, jossa tehtävän käytetään sisältyy tehtävän metatiedot. Kun olet määrittänyt ongelman solmu, voit tehdä useita toiminnoilla:

- **Käynnistä tietokone uudelleen solmu** ([REST][rest_reboot] | [.NET][net_reboot])

    Solmun uudelleenkäynnistyksen joskus poistaa piilevän ongelmista, kuten jumissa tai kaatui prosesseja ylöspäin. Huomaa, että jos lisääminen resurssivarantoon käyttää Käynnistä tehtävän tai työtäsi käyttää valmistelu projektitehtävä, ne suoritetaan solmun käynnistyessä.

- **Solmun reimage** ([REST][rest_reimage] | [.NET][net_reimage])

    Tämä uudelleenasentaa käyttöjärjestelmän solmun. Samoin kuin uudelleenkäynnistyksen solmu tehtävien aloittaminen ja työhön liittyvät tehtävät ovat uudelleen, kun solmu on ollut reimaged.

- **Poista olevilla solmu** ([REST][rest_remove] | [.NET][net_remove])

    Joskus on tarpeen solmu poistaa ryhmän kokonaan.

- **Tehtävän ajoitus solmun käytöstä** ([REST][rest_offline] | [.NET][net_offline])

    Tämä tehokkaasti kestää solmu "offline-tilassa" niin, että liitetään ei ole vielä tehtäviä, mutta sallii solmu ovat käynnissä ja ryhmän. Näin voit suorittaa syynä virheitä edelleen tutkimus epäonnistui tehtävän menetä--ilman ja ilman solmu ilmenneet tehtävän virheet. Voit esimerkiksi poistaa tehtävien ajoituksen solmun [Kirjautuminen etäyhteyden](#connecting-to-compute-nodes) solmun tapahtumalokien tai tehdä muita vianmääritysohjeita. Kun olet lisännyt oman tutkimus, voit tuoda solmu online-tilaan sitten tehtävän ajoitus ottaminen käyttöön ([REST][rest_online] | [.NET][net_online]), tai käyttämällä jotakin edellä kerrottiin muita toimintoja.

> [AZURE.IMPORTANT] Kunkin toiminnon, joka on kuvattu tämän osion--uudelleen, reimage, poista ja poista tehtävän ajoitus – Voit määrittää, miten käynnissä olevat solmun tehtävät käsitellään, kun suoritat toiminnon. Kun poistat tehtävän ajoitus solmun käyttämällä erä .NET asiakas-kirjastossa, voit esimerkiksi määrittää [DisableComputeNodeSchedulingOption] [ net_offline_option] luettelo-arvon, voit määrittää, onko **Lopeta** suoritettavat tehtävät, **Requeue** ne ajoittamisessa muissa solmuissa salliminen käynnissä tehtäviä, ennen kuin suoritat toiminnon (**TaskCompletion**).

## <a name="next-steps"></a>Seuraavat vaiheet

- Käy läpi otoksen erä sovelluksen vaiheittaiset- [.NET Azure-erä kirjaston käytön aloittaminen](batch-dotnet-get-started.md). On myös [Python versio](batch-python-tutorial.md) , joka suoritetaan työmäärä Linux Laske solmuissa opetusohjelmassa.

- Lataa ja luoda [Erää Explorer] [ github_batchexplorer] otoksen projektin aikana erä ratkaisujen kehittämiseen käyttöä varten. Erän-Resurssienhallinnan avulla voit suorittaa seuraavat ja paljon muuta:
  - Valvoa ja käsitellä jakavat, työt ja erä-tilisi tehtäviin
  - Lataa `stdout.txt`, `stderr.txt`, ja muita tiedostoja solmujen
  - Käyttäjien luominen solmuissa ja lataa remote kirjautuminen RDP-tiedostot

- Lue, miten voit [luoda Linux Laske solmujen](batch-linux-nodes.md).

- Käy [Azure erä keskustelupalsta] [ batch_forum] MSDN-sivuston. Neuvoa on voit esittää kysymyksiä, vain oppivat vai on asiantuntija käyttämällä erä.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
