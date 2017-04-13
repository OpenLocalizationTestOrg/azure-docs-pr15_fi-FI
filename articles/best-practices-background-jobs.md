<properties
   pageTitle="Taustan työt ohjeet | Microsoft Azure"
   description="Tausta on kuvattu tehtäviä, suorittamisen itsenäisesti käyttöliittymän."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="masashin"/>

# <a name="background-jobs-guidance"></a>Taustan työt ohjeet

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Yleiskatsaus

Useita erityyppisiä sovellukset edellyttävät ulkopuolisista käyttäjän käyttöliittymän suoritettavat taustatehtävät. Esimerkkejä tällaisista tiedostoista ovat Ehdota, tehostettu käsittely tehtäviä ja pitkään käynnissä olevien prosessien kuten työnkulkuja. Taustan työt voidaan suorittaa tarvitsematta käyttäjän toimia – sovellusta voi aloittaa työn ja käsitellä vuorovaikutteinen pyyntöjä käyttäjiltä ja jatka. Tämän avulla voit pienentää sovelluksen Käyttöliittymä, joka voi parantaa käytettävyys ja vähentää vuorovaikutteinen vastauksen aikoja kuormituksen.

Esimerkiksi jos sovellus edellyttää kuvia, jotka on ladattu käyttäjät pikkukuvat luomiseen, se voi toiminto taustatyön ja Tallenna pikkukuva tallennustilan kun se on valmis--ilman, että käyttäjällä hänen nimeään tarvitse lisätä odottaa prosessin on tarkoitus valmistua. Samalla tavalla tilauksen käyttäjä voi käynnistää taustan työnkulun, joka käsittelee tilaus, kun Käyttöliittymän avulla käyttäjä voi jatkaa selaamista web Appissa. Kun tausta-työ on valmis, se tallennetut tilaukset-tietojen päivittäminen ja lähettää sähköpostia käyttäjälle, joka vahvistaa tilauksen.

Kun pidät toteuttamaan tehtävän merkitseminen taustan projektin Tärkeimmät perusteet onko onko tehtävä voidaan suorittaa käyttäjän toimia ja Käyttöliittymän ilman hänen nimeään tarvitse lisätä Odota työ on tarkoitus valmistua. Tehtäviä, joihin tarvitaan käyttäjän tai Käyttöliittymän Odota, kunnes ne ovat valmiit ei ehkä ole sopiva tausta työt nimellä.

## <a name="types-of-background-jobs"></a>Taustatöiden tyypit

Taustan työt ovat yleensä vähintään yksi seuraavista työt seuraavanlaisia:

- Suoritinta kuormittavaa työt, kuten matemaattisia laskutoimituksia tai rakenteellisia mallin analyysi.
- Voin/O-paljon töitä, kuten suoritetaan tallennustilan tapahtumien sarjaan tai indeksoinnin tiedostot.
- Erän työt, esimerkiksi yöllä tietojen päivittämisen tai ajoitetun käsittely.
- Pitkään käynnissä olevien työnkulkujen esimerkiksi tilauksen toteutusta tai valmistelu palveluiden ja -järjestelmien.
- Luottamuksellisia tietoja käsittely, jos tehtävä on jaettu käsittelyyn entistä turvalliseen paikkaan. Olet ehkä halua käsitellä luottamuksellisten tietojen verkkosovellukseen. Sen sijaan voit käyttää kuviota, kuten [Gatekeeper](http://msdn.microsoft.com/library/dn589793.aspx) voit siirtää tietoja erillään tausta-prosessi, jolla on pääsy Suojattu tallennuspaikka.

## <a name="triggers"></a>Käynnistimien

Taustan työt voidaan aloittaa useilla eri tavoilla. Kuuluvat johonkin seuraavista ryhmistä:

- [**Tapahtumaohjattu käynnistimet**](#event-driven-triggers). Tehtävän aloitetaan tapahtuman, yleensä käyttäjä tai työnkulun vaiheen suoritettu toiminto.
- [**Aikataulun perustuva käynnistimet**](#schedule-driven-triggers). Aikataulun mukaan ajastin käynnistyy tehtävän. Tämä voi johtua toistuvan aikataulun tai kertakäyttöisen kutsun, joka on määritetty myöhemmin uudelleen.

### <a name="event-driven-triggers"></a>Tapahtumaohjattu Käynnistimet

Tapahtumaohjattu kutsu käyttää käynnistimen Aloita taustatehtävä. Esimerkkejä tapahtumaohjattu käynnistimet ovat:

- Käyttöliittymän tai toisen työn sijoittaa viestin jonossa. Viesti sisältää toiminnon, joka on tapahtunut, kuten tilauksen käyttäjän tietoja. Taustatehtävän seuraa Tämä jono ja tunnistaa uuden viestin saapumista. Se lukee viestin ja käyttää tietoja ei taustan työn syötteenä.
- Käyttöliittymän tai toisen työn tallentaa tai päivittää tallennustilan arvo. Taustatehtävän valvoo tallennustilaa ja havaitsee muutoksia. Se lukee tiedot ja käyttää sitä taustan työn syötteenä.
- Käyttöliittymän tai toisen työn tekee pyyntö päätepisteen, kuten HTTPS-URI tai Ohjelmointirajapinta, joka on määritetty WWW-palvelulle. Se välittää tietoja, joita tarvitaan pyynnön osana taustatehtävän suorittamiseen. Päätepisteen tai WWW-palvelun käynnistää taustatehtävän, joka käyttää tietoja sen syötteenä.

Tyypillinen tehtäviä, jotka sopivat tapahtumaohjattu kutsu esimerkiksi käsittelyyn, työnkulut-tietojen lähettäminen remote-palveluihin, sähköpostiviestien lähettäminen ja uusien käyttäjien multitenant sovellusten valmistelu kuva.

### <a name="schedule-driven-triggers"></a>Aikataulun perustuva Käynnistimet

Aikataulun perustuva kutsu käyttää ajastin Aloita taustatehtävä. Esimerkkejä aikataulun perustuva käynnistimet ovat:

- Ajastin, joka on käytössä paikallisesti sovelluksesta tai sovelluksen käyttöjärjestelmän osana käynnistää taustatehtävän säännöllisesti.
- Ajastin, jossa on käytössä toiseen sovellukseen tai ajastin-palvelun Azure aikataulutus, kuten lähettää pyynnön säännöllisesti API tai web-palveluun. API tai WWW-palvelun käynnistää taustatehtävän.
- Erillinen prosessi tai sovellus käynnistyy ajastin, joka aiheuttaa taustan tehtävän voi käynnistää kerran määritetyn viiveen jälkeen tai tiettynä ajankohtana.

Tyypillinen tehtävät, jotka sopivat aikataulun perustuva kutsu esimerkkejä eräkäsittely rutiinit (kuten päivittäminen liittyviä tuotteita luetteloiden käyttäjille heidän viimeisimmät toimintatapojesi perusteella), säännöllisestä tietojen käsittely tehtävät (esimerkiksi indeksien päivittämistä tai luodaan kertynyt tulokset), tietojen analysointi päivittäin raportteja, tietojen säilytys uudelleenjärjestäminen ja tietojen kelpoisuus tarkistukset.

Jos käytät aikataulun perustuva tehtävän, joka on suoritettava yksittäisen esiintymän, huomioon seuraavasti:

- Jos Laske esiintymää, jossa on käytössä (esimerkiksi virtual tietokoneeseen, käyttämällä Windows ajoitettujen tehtävien) ajoituksen skaalata, sinun on käynnissä ajoituksen useita kertoja. Nämä voi käynnistää tehtävän useita kertoja.
- Jos tehtävät suorittaa kauemmin kuin kauden ajoituksen tapahtumien välillä, scheduler ehkä käynnisty esiintymässä tehtävän käynnissä edelliseen silti.

## <a name="returning-results"></a>Palauttaa tuloksia

Taustan työt suorittaa asynkronisesti erillinen prosessi tai jopa eri paikkaan, Käyttöliittymän tai prosessi, joka käynnistää taustatehtävän. Ihannetapauksessa taustatehtävät ovat "Kyselysäännön ja unohda" toiminnot ja edistymistä suorittamisen ei ole vaikutusta Käyttöliittymän tai puheluja prosessi. Tämä tarkoittaa, että puheluja prosessin ei odota tehtävien suorittamiseen. Tämän vuoksi se ei tunnista automaattisesti tehtävän päättyessä.

Jos asetat taustatehtävän pitää yhteyttä puheluja tehtävän edistymisen vai valmistumista, tämän ominaisuuden käyttöön järjestelmä. On joitakin esimerkkejä:

- Kirjoita tilan ilmaisin arvon, että ne ovat käytettävissä Käyttöliittymän tai soittajan tehtävään, jonka voit valvoa tai tarkistaa tämän arvon, tarvittaessa. Muita tietoja, jotka taustatehtävä on palautettava soittajan voi asettaa samaan varastoon.
- Muodostaa vastausjono, joka seuraa Käyttöliittymän tai kutsuja. Taustatehtävän lähettää viestejä, jotka ilmaisevat tila- ja jonossa. Tiedot, jotka taustatehtävä on palautettava soittajan voi asettaa sitten viesteihin. Jos käytössäsi on Azure palvelun Bus, voit toteuttaa tätä ominaisuutta **ReplyTo** ja **CorrelationId** ominaisuudet. Lisätietoja on artikkelissa [korrelaatio-palvelun Bus se Messaging](http://www.cloudcasts.net/devguide/Default.aspx?id=13029).
- Näytä API tai taustan tehtävästä, voit hankkia tilatiedot voit käyttää Käyttöliittymän tai soittajan päätepiste. Tiedot, jotka taustatehtävä on palautettava soittajan voidaan lisätä vastausta.
- On taustan soittaa takaisin tilan ennalta määritettyjä pisteisiin tai valmiiksi Käyttöliittymän tai soittajan API-Liittymän kautta. Tämä voi olla käynnistettyjen paikallisesti tapahtumien tai Julkaise ja tilaa järjestelmä, jonka kautta. Pyyntö tai tapahtuman tiedot voidaan lisätä tietoja, jotka taustatehtävä on palautettava soittajalle.

## <a name="hosting-environment"></a>Isännöintiympäristö

Voit ylläpitää taustaprosessit eri Azure platform-palvelujen avulla:

- [**Azure Web Apps-sovellusten ja WebJobs**](#azure-web-apps-and-webjobs). Voit käyttää WebJobs mukautettuja töitä perustuvan alueen erityyppisiä komentosarjoja tai ohjelmia verkkosovellukseen puitteissa suorittamaan.
- [**Azure pilvipalveluihin Internetin kautta tai työntekijä roolit**](#azure-cloud-services-web-and-worker-roles). Voit kirjoittaa koodin rooli, joka suorittaa taustalla tehtävän kuluessa.
- [**Azure-Virtuaalikoneissa**](#azure-virtual-machines). Jos käytössäsi on Windows-palvelun tai haluat käyttää Windows-ajoitetut, on yhteinen isännöidä tausta-tehtäviin varatun virtual machine.
- [**Azure erä**](./batch/batch-technical-overview.md). Platform-palvelu, joka ajoittaa Laske kuormittavaa työtä voidaan suorittaa näennäiskoneiden hallitun kokoelma on, ja voit automaattisesti asteikko Laske resurssien tarpeiden työt.

Seuraavissa osissa kuvataan tarkemmin näistä vaihtoehdoista, ja Sisällytä huomioon otettavia seikkoja avulla voit valita haluamasi vaihtoehto.

## <a name="azure-web-apps-and-webjobs"></a>Azure Web Apps-sovellusten ja WebJobs

Azure WebJobs avulla voit suorittaa mukautettuja töitä kuin taustatehtävien Azure Web App-sovelluksessa. WebJobs suoritetaan jatkuvan prosessin koodiin kontekstissa. WebJobs myös suorittamalla käynnistimen tapahtuman saatuaan Azure ajoituksen tai Ulkoiset tekijät, kuten tallennustilan BLOB-objektit ja viestin olevien muutokset. Työt voidaan käynnistää ja pysäyttää pyydettäessä ja Sammuta tilanteen. Jos jatkuvasti käynnissä WebJob epäonnistuu, sen automaattisesti käynnistyksen yhteydessä. Yritä ja virheen toiminnot ovat määritettävissä.

Kun määrität WebJob:

- Jos haluat vastata tapahtumaohjattu käynnistimen työ, määritä se kuin **jatkuvasti**. Komentosarjan tai ohjelman tallennetaan kansio nimeltä sivuston/wwwroot/app_data/työt/jatkuva.
- Jos haluat vastata aikataulun perustuva käynnistin työ, määritä se kuin **suorittaa aikataulun**. Komentosarjan tai ohjelman tallennetaan sivuston/wwwroot/app_data/työt/saatu niminen kansio.
- Jos valitset **pyydettäessä Suorita** -vaihtoehto, kun määrität työn, se suorittaa saman koodin **suorittaminen aikataulun** -asetus käynnistyskerralla.

Azure WebJobs suorittaa eristettyjä web Appin. Tämä tarkoittaa, että ne käyttää ympäristömuuttujat ja jakaa tietoja, kuten yhteyden merkkijonoja, web-sovelluksen kanssa. Työn käyttää tietokoneessa, jossa on projektin yksilöllinen. Nimetty **AzureWebJobsStorage** yhteysmerkkijonon on pääsy sovelluksen tietoja Azure tallennustilan olevien BLOB-objektit ja taulukot ja palvelun Bus käyttöoikeuden viestintä- ja viestintä. Työn toiminnon lokitiedostojen nimeltä **AzureWebJobsDashboard** yhteysmerkkijonon pääsee.

Azure WebJobs on seuraavat ominaisuudet:

- **Suojaus**: WebJobs on suojattu web Appin käyttöönotto-tunnistetiedot.
- **Tuetut tiedostotyypit**: Voit määrittää WebJobs käyttämällä komento komentosarjoja (Cmd.), erä-tiedostot (.bat), PowerShell-komentosarjojen (.ps1), bash shell-komentosarjoja (.sh), PHP komentosarjoja (.php), Python komentosarjoja (.py), JavaScript-koodia (.js) ja ohjelmia (.exe, .jar ynnä muuta).
- **Käyttöönoton**: Voit ottaa käyttöön komentosarjat ja suoritettavat tiedostot käyttämällä Azure-portaalissa [WebJobsVs](https://visualstudiogallery.msdn.microsoft.com/f4824551-2660-4afa-aba1-1fcc1673c3d0) -apuohjelman Visual Studiossa tai [Visual Studio 2013 Päivitä 4](http://www.visualstudio.com/news/vs2013-update4-rc-vs) (Voit luoda ja ottaa käyttöön tällä asetuksella), käyttämällä [Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)tai kopioimalla ne suoraan seuraavista sijainneista:
  - Saat saatu suorittamisen: sivuston/wwwroot/app_data/työt/saatu / {projektin nimi}
  - Jatkuva suorittamista varten: sivuston/wwwroot/app_data/työt/jatkuva / {projektin nimi}
- **Lokiin kirjaaminen**: Console.Out käsitellään tiedot (merkitty). Console.Error käsitellään virhe. Voit käyttää seuranta-ja diagnostiikka mukaan Azure-portaalissa. Voit ladata lokitiedostojen suoraan-sivustosta. Ne on tallennettu johonkin seuraavista sijainneista:
  - Saat saatu suorittamisen: Vfs/tietojen/työt/saatu/työ
  - Jatkuva suorittamista varten: Vfs/tietojen/työt/jatkuva/työ
- **Määritys**: Voit määrittää WebJobs portaalin, REST API ja PowerShellin avulla. Voit säätää työn määritystietoja nimeltä settings.job sama kuin työn komentosarja pääkansiossa määritystiedostoa. Esimerkki:
  - {"stopping_wait_time": 60}
  - {"is_singleton": TOSI}

### <a name="considerations"></a>Huomioon otettavia seikkoja

- Oletusarvon mukaan WebJobs skaalata web-sovelluksen kanssa. Voit kuitenkin määrittää työt toimimaan yhden esiintymän määrittämällä **is_singleton** määritys-ominaisuuden arvoksi **true**. Yksittäisen esiintymän WebJobs on hyötyä viittaamaan tehtäviin, joita et halua suorittaa samanaikaisesti useita tai skaalata esiintymät, kuten uudelleenindeksointi, tietojen analysointi ja samoja tehtäviä.
- Pienennä työt web Appin suorituskykyyn vaikuttavat, kannattaa ehkä luoda uuden sovelluksen palvelun suunnitelman host WebJobs, joka voi olla pitkä käynnissä tai resurssiin tyhjä Azure Web App-esiintymän paljon resursseja.

### <a name="more-information"></a>Lisätietoja

- [Azure WebJobs suositellaan resurssien](./app-service-web/websites-webjobs-resources.md) näyttää useiden ohjeista, lataamista ja näytteiden WebJobs varten.

## <a name="azure-cloud-services-web-and-worker-roles"></a>Azure pilvipalveluihin Internetin kautta tai työntekijä roolit

Voit suorittaa taustaprosessit web-roolin tai erillisen Työntekijä-roolin. Kun ovat päätät käyttää työntekijän rooli, harkitse skaalattavuus ja joustavuus vaatimukset, tehtävän elinaika, Vapauta cadence, suojaus, vikasietoa, ristiriita, monimutkaisuus ja looginen arkkitehtuuri. Lisätietoja on kohdassa [Laske resurssin koonnin kuvio](http://msdn.microsoft.com/library/dn589778.aspx).

Cloud Services-roolin taustatehtävien toteuttamisesta seuraavilla tavoilla:

- Toteutus **RoleEntryPoint** -luokan luominen rooli ja sen menetelmiä avulla voit suorittaa taustatehtävät. Tehtävien suorittaminen WaIISHost.exe kontekstissa. Lataa asetukset, he voivat käyttää **CloudConfigurationManager** luokan **GetSetting** -menetelmää. Lisätietoja on artikkelissa [elinkaari (pilvipalveluihin)](#lifecycle-cloud-services).
- Käynnistyksen tehtävien avulla voit suorittaa taustaprosessit, kun sovellus käynnistyy. Pakota tehtävät voidaan suorittaa taustalla-ominaisuuden **taskType** **Tausta** (Jos et tee tämä-sovelluksen käynnistys-prosessi pysäyttää ja odota, kunnes tehtävän loppuun). Lisätietoja on artikkelissa [Azure käynnistys tehtävien suorittaminen](./cloud-services/cloud-services-startup-tasks.md).
- WebJobs SDK avulla voit toteuttaa tausta-toiminnot, kuten WebJobs, jotka on aloitettu käynnistys-tehtäväksi. Lisätietoja on artikkelissa [Azure WebJobs SDK: N käytön aloittaminen](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md).
- Asenna Windows-palvelun, joka suorittaa yhden tai useamman taustaprosessit käynnistys tehtävän avulla. Sinun on määritettävä **taskType** -ominaisuuden **Tausta** , niin, että palvelun suorittaa taustalla. Lisätietoja on artikkelissa [Azure käynnistys tehtävien suorittaminen](./cloud-services/cloud-services-startup-tasks.md).

### <a name="running-background-tasks-in-the-web-role"></a>Web-roolin taustatehtävien suorittaminen

Tärkein etu käynnissä taustaprosessit web rooli on hosting kustannukset, koska ei tarvitse ottaa käyttöön lisää roolit tallentamista.

### <a name="running-background-tasks-in-a-worker-role"></a>Taustatehtävien suorittaminen Työntekijä-roolin

Taustatehtävien suorittaminen Työntekijä-roolin on monia etuja:

- Sen avulla voit hallita skaalaus-roolin mistäkin erikseen. Esimerkiksi sinun on ehkä esiintymiä web-roolin tukemaan nykyisen lataamisen, mutta työntekijän rooli, joka suorittaa taustaprosessit vähemmän esiintymät. Skaalauksen taustan tehtävän suorittaminen esiintymät erikseen Käyttöliittymän rooleista, voit pienentää isännöintipalvelu kustannukset säilyttäen hyväksyttävällä suorituskykyä.
- Se offloads käsittely katseltavan tausta-toimintoja, kuten web-rooli. Web-rooli, joka sisältää Käyttöliittymän voi olla vastaa ja saattaa tarkoittaa vähemmän esiintymät tarvitaan tietyn aseman pyyntöjen käyttäjiltä.
- Sen avulla voit toteuttaa erottaminen koskee. Rooli kunkin tyyppiselle ottaa käyttöön tiettyjä selkeästi määritelty ja Aiheeseen liittyviä tehtäviä. Näin suunnittelu ja ylläpito koodin helpompaa, koska on vähemmän koodi ja toimintojen välinen rooleille väliset yhteydet.
- Se voi auttaa eristää luottamuksellisia prosessit ja tiedot. Esimerkiksi web-roolit, jotka toteuttavat Käyttöliittymän ei tarvitse käyttää tietoja, jotka hallitaan ja hallinnoida työntekijän rooli. Tämä voi olla hyödyllistä vahvistaminen suojaus, erityisesti silloin, kun käytät esimerkiksi [Gatekeeper kuvio](http://msdn.microsoft.com/library/dn589793.aspx)kuvion.  

### <a name="considerations"></a>Huomioon otettavia seikkoja

Seuraavat seikat huomioon, kun valitsemalla mistä ja mihin ottamaan taustaprosessit käytettäessä pilvipalveluihin Internetin kautta tai työntekijä rooleja:

- Isännöinnin taustaprosessit-web-roolille tallentaa eri työntekijän rooli vain näiden tehtävien suorittamiseen kustannukset. On kuitenkin todennäköisesti vaikuttavat suorituskykyä ja käytettävyyden sovelluksen, jos on ristiriita käsittely ja muita resursseja. Web-roolin käyttämällä eri työntekijä roolia suojaa pitkään käynnissä olevien tai paljon resursseja taustatehtävien vaikutuksen.
- Jos isännöit taustaprosessit **RoleEntryPoint** -luokan avulla, voit siirtää tämän helposti jonkin muun roolin. Esimerkiksi jos luokan luominen web-rooli ja myöhemmin, että sinun on suoritettava Työntekijä-roolin tehtävät, voit siirtää **RoleEntryPoint** luokan käyttöönotto Työntekijä-rooliin.
- Käynnistyksen tehtävät on suunniteltu ohjelman tai komentosarjan suorittaminen. Taustatyön olevan ohjelmatiedosto käyttöönotto voi olla vaikeaa, erityisesti, jos se vaatii riippuvaiset kokoonpanojen käyttöönotto. Sinun on ehkä helpompi ottaminen käyttöön ja määrittää taustatyön, kun käytät Käynnistys tehtäviä komentosarjan avulla.
- Poikkeukset, jotka aiheuttavat taustatehtävän voi epäonnistua on eri vaikutus, sen mukaan, miten ne sijaitsevat:
  - Jos käytät **RoleEntryPoint** luokan lähestymistapa, epäonnistunut tehtävän aiheuttaa rooli, jotta voit aloittaa niin, että tehtävä käynnistyy automaattisesti. Tämä voi vaikuttaa sovelluksen käytettävyyttä. Voit estää tämän varmistaa Sisällytä tehokkaat poikkeuksen **RoleEntryPoint** -luokka ja kaikki taustatehtävät käsittely. Käynnistä tehtävät, jotka eivät läpäise, jos se on tarvittavat koodin avulla ja palauttaa poikkeuksen käynnistämään roolin vain, jos ei tilanteen palauttaminen virheen oman koodiin.
  - Jos käytät Käynnistys tehtävät, olet vastuussa hallinta tehtävän suorittaminen ja tarkistetaan, jos se ei onnistu.
- Käynnistyksen tehtävien hallintaa sekä on vaikeaa **RoleEntryPoint** luokan menetelmän asemesta. Azure WebJobs SDK sisältää kuitenkin helpompi hallita WebJobs, kun käynnistät käynnistys perustoiminnoista raporttinäkymä.

### <a name="more-information"></a>Lisätietoja

- [Laske resurssin koonnin kuvio](http://msdn.microsoft.com/library/dn589778.aspx)
- [Azure WebJobs SDK: N käytön aloittaminen](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)

## <a name="azure-virtual-machines"></a>Azure-Virtuaalikoneissa

Taustaprosessit voi toteuttaa niin, että ne estää otetaan käyttöön Azure verkkosovelluksissa tai pilvipalveluihin tai nämä asetukset eivät välttämättä ole helposti. Tyypillinen esimerkit Windows services- ja muiden valmistajien ja ohjelmat. Toinen esimerkki ehkä suorittamisen-ympäristössä, joka on muussa kuin isännöinnin sovelluksen ohjelmia. Voi olla esimerkiksi Unix- tai Linux-ohjelma, jonka haluat suorittaa Windows- tai .NET-sovelluksesta. Voit valittavana Azure virtuaalikoneen käyttöjärjestelmät solualueen ja suorittaa palvelun tai suoritettavat kyseisen virtuaalikoneen.

Avulla voit valita käyttäminen näennäiskoneiden on [Azure App Services, pilvipalveluihin ja näennäiskoneiden vertailu](./app-service-web/choose-web-site-cloud-service-vm.md). Lisätietoja näennäiskoneiden-asetuksista on artikkelissa [Azure virtuaalikoneen ja pilvipalvelussa koot](http://msdn.microsoft.com/library/azure/dn197896.aspx). Saat lisätietoja käyttöjärjestelmät ja valmiin kuvia, jotka ovat käytettävissä näennäiskoneiden [Azuren näennäiskoneiden Marketplacesta](https://azure.microsoft.com/gallery/virtual-machines/).

Aloittaa taustan tehtävän erillisessä virtual machine sinulla solualueen asetukset:

- Voit suorittaa tehtävän pyydettäessä suoraan sovelluksen lähettämällä pyynnön päätepiste, joka paljastaa tehtävän. Tämä välittää tietoja, jotka tehtävä vaatii. Tämä päätepiste käynnistää tehtävän.
- Voit määrittää tehtävän suorittamiseen aikataulun ajoituksen tai ajastin, joka on käytettävissä valitsemasi-käyttöjärjestelmässä käyttämällä. Esimerkiksi Windows voit käyttää Windowsin Tehtävien ajoituksen komentosarjojen ja tehtävien suorittamiseen. Tai jos sinulla on SQL Server virtuaalikoneen asennettu, voit käyttää SQL Server Agent komentosarjojen ja tehtävien suorittamiseen.
- Voit aloittaa tehtävän lisäämällä viestin jono, joka seuraa tehtävän tai lähettämällä pyynnön API, joka paljastaa tehtävän Azure ajoitus.

On kohdassa aiempien [käynnistimien](#triggers) lisätietoja siitä, miten voit aloittaa taustatehtävät.  

### <a name="considerations"></a>Huomioon otettavia seikkoja

Kun ovat päätät ottamaan taustaprosessit-Azure virtuaalikoneen, ota huomioon seuraavat seikat:

- Taustaprosessit erillisessä Azure virtual machine-isännöintipalvelu tarjoaa joustavuutta ja määrittää tarkasti Aloituslomakkeen, suorittaminen, järjestäminen ja resurssivaraukset avulla. Kuitenkin se kasvattaa runtime kustannukset, jos virtual machine on otettava käyttöön vain, jos haluat suorittaa taustatehtävät.
- Ei ole seurannassa Azure portaalin ja epäonnistui tehtävien--automaattinen uudelleenkäynnistys ei ominaisuuksien tehtävät, vaikka voit seurata virtuaalikoneen basic tila sekä hallita sitä [Azure palvelun hallinnan cmdlet-komennot](http://msdn.microsoft.com/library/azure/dn495240.aspx). On kuitenkin ei tilat prosessit ja viestiketjuissa siirtyminen Laske solmuissa. Yleensä käyttämisestä virtual machine edellyttävät muita työmäärään toteuttamisesta toimintoa, joka kerää tietoja tehtävän instrumentation ja virtuaalikoneen käyttöjärjestelmästä. Yksi ratkaisu, joka voi olla sopiva on käyttää [System Center Management Pack for Azure](http://technet.microsoft.com/library/gg276383.aspx).
- Harkitse luomista seurantaa keräysputkien, joka on määritetty HTTP päätepisteet kautta. Nämä keräysputkien koodi voi suorittaa kuntotietojen tarkistus, kerätä toiminnalliset tiedot ja tilastot--tai lajittelua virheiden tiedot ja palaa hallintasovellus. Lisätietoja on artikkelissa [Kunto päätepisteen seuranta kuvio](http://msdn.microsoft.com/library/dn589789.aspx).

### <a name="more-information"></a>Lisätietoja

- Azuren [näennäiskoneiden](https://azure.microsoft.com/services/virtual-machines/)
- [Azure-Virtuaalikoneissa usein kysytyt kysymykset](virtual-machines/virtual-machines-linux-classic-faq.md)

## <a name="design-considerations"></a>Tyyliseikat

On useita keskeisiä tekijöitä ottaa huomioon, kun suunnittelet taustatehtävät. Seuraavissa osissa käsitellään jakaminen, ristiriidat ja yhteensovittamisesta.

## <a name="partitioning"></a>Jakaminen

Jos haluat sisällyttää taustatehtävien suorittaminen olemassa olevaan esiintymään (kuten web app, web-rooli, aiemmin työntekijän rooli tai virtuaalikoneen), ota huomioon, miten tämä vaikuttaa laatu määritteiden Laske esiintymän ja taustatehtävän itse. Seikoista avulla voit päättää, tehtävät, joiden suorittaminen nykyisen esiintymän colocate tai erota ne erillisessä Laske esiintymä:

- **Käytettävyys**: taustaprosessit ehkä tarvitse käytettävyys muihin-osina sovelluksen samalla tasolla erityisesti Käyttöliittymän ja muita osia, jotka osallistuvat suoraan käyttäjän toimia. Taustatehtävät voi olla aiempaa varmatoimisempia viive-yritetään uudelleen yhteys epäonnistuu, ja muut tekijät vaikuttavat siihen, että käytettävyys, koska toiminnot voivat olla jonossa. Kuitenkin on oltava riittävä kapasiteetti estää pyynnöt, jotka saattavat estää olevien ja vaikuttavat kokonaisuutena varmuuskopion.
- **Skaalattavuus**: taustaprosessit on todennäköisesti eri skaalattavuus vaatimus kuin Käyttöliittymä ja sovelluksen vuorovaikutteinen osista. Käyttöliittymän skaalaus täytyy mahdollisesti demand-päät mukaiseksi, kun avoin taustaprosessit voi valmistua aikana vähemmän aikoja vähemmän lukumäärä Laske esiintymät.
- **Vikasietoisuudelle**: virhe, joka isännöi vain taustaprosessit Laske esiintymän eivät ehkä fatally vaikuta sovellus kokonaisuudessaan Jos pyynnöt näiden tehtävien jonossa- tai siirretty myöhemmäksi, kunnes tehtävä on jälleen käytettävissä. Jos Laske esiintymän ja/tai tehtäviä voit käynnistettävä sopivan ajan kuluessa, sovelluksen käyttäjiä ei voi vaikuttaa.
- **Suojaus**: taustaprosessit voi olla eri suojausvaatimukset tai rajoituksia kuin Käyttöliittymän tai sovelluksen muihin osiin. Käyttämällä eri Laske esiintymän voit määrittää eri suojaus-ympäristön tehtäville. Voit käyttää myös kuviot, kuten Gatekeeper eristää jotta Suurenna suojaus- ja tausta Laske esiintymät käyttöliittymässä.
- **Suorituskyvyn**: Voit valita Laske esiintymän tyypin, tausta tehtävien erityisesti vastine vaatimukset tehtävät. Tämä saattaa tarkoittaa käytössä vähemmän kallista Laske-vaihtoehto, jos tehtävät eivät edellytä samat käsittely mahdollisuudet Käyttöliittymän tai suurempi esiintymä, jos ne vaativat muita kapasiteetin ja resursseja.
- **Hallinta**: taustaprosessit voi olla eri kehittämällä ja valinnut ensisijaista Sovelluskoodin tai Käyttöliittymän. Niiden käyttöönottoa erillisessä Laske esiintymän voidaan yksinkertaistaa päivitykset ja versiotiedot.
- **Kustannukset**: lisääminen Laske esiintymät suorittamaan taustan tehtävät kasvaa isännöinnin kustannukset. Ota huomioon huolellisesti trade-off muita kapasiteetin ja nämä lisäkustannuksia välillä.

Lisätietoja on artikkelissa [Täytemerkki osavaltioiden kuvio](http://msdn.microsoft.com/library/dn568104.aspx) ja [Pikaluistelukilpailuissa kuluttajille kuvio](http://msdn.microsoft.com/library/dn568101.aspx).

## <a name="conflicts"></a>Ristiriidat

Jos sinulla on useita kertoja taustatyön, on mahdollista, että ne kilpailemaan käytön resursseja ja palveluja, kuten ja tallennustilaa. Tämä samanaikainen käyttö voi aiheuttaa resurssiristiriita, joka voi esiintyä ristiriitoja käytettävyys palveluja ja tallennustilaa tietojen eheyteen. Voit ratkaista resurssiristiriita Pessimistinen lukitsemalla menetelmän avulla. Tämä estää pikaluistelukilpailuissa tehtävän samanaikaisesti käyttämästä palvelu tai teeskentelee tietojen esiintymät.

Toinen vaihtoehto ratkaiseminen on määrittääksesi taustaprosessit Yksiarvoinen, niin, että ei koskaan vain yksi esiintymä on käynnissä. Näin ei kuitenkaan luotettavuutta ja suorituskykyä edut, jotka tarjoavat useita esiintymä-määritys. Tämä on erityisen TOSI, jos Käyttöliittymän voit antaa riittävästi pitämään useampaan kuin yhteen taustan tehtävään varattu työ.

On tärkeää varmistaa taustatehtävän voi automaattisesti uudelleen ja että se on riittävän kapasiteetin voitava tarvittaessa kohdalle. Voit tehostaa kohdistamalla Laske-esiintymän riittävästi tiedoilla käyttöönoton asetetaan jonoon toimintoa, joka voidaan tallentaa pyyntö myöhemmin suorittamisen, kun demand pienenee tai käyttämällä yhdistelmän näistä menetelmistä.

## <a name="coordination"></a>Koordinointi

Taustatehtävät voi olla monimutkaisia ja saattaa edellyttää useiden yksittäisten tehtävien suorittamiseen kaavaa tai kaikki täyttävät. On yleisiä näissä tilanteissa tehtävä jakautuvan pienempi hienovaraista vaiheiden tai alitehtävät, jotka voidaan suorittaa useita kuluttajille. Multistep työt voi olla tehokkaampaa ja joustavampaa, koska yksittäisiä vaiheet voivat erota Uudelleenkäytettävän useita töitä. On myös lisätä, poistaa tai vaiheiden järjestyksen muuttaminen on helppoa.

Yhteensovittamisesta useita vaiheita ja tehtäviä voi olla haastavaa, mutta sillä on kolme Yleiset mallit, joiden avulla voit opastaa ratkaisun toteutusta:

- **Tehtävän useita Uudelleenkäytettävän vaiheiden decomposing**. Sovellus voi olla tarpeen suorittaa erilaisia tehtäviä erilaisten monimutkaisia tietoja, jotka käsittelee. Helppoa, mutta joustamattoman lähestymistapa toteuttamisesta tämän sovelluksen ehkä tämä käsittelyn kuin yksiosaiset moduuli. Tämä vaihtoehto on todennäköisesti vähentää mahdollisuuksia optimointi koodi ja sen käyttäminen uudelleen, jos saman käsittely osat ovat pakollisia muualla-sovelluksessa. Lisätietoja on artikkelissa [putket ja suodattimet kuvio](http://msdn.microsoft.com/library/dn568100.aspx).
- **Tehtävän vaiheiden suorittamisen hallinta**. Sovelluksen voi suorittaa tehtäviä, joihin sisältyy useita vaiheita (osa niistä voi käynnistää etätyöpöytä tai remote resursseihin). Yksittäisten vaiheet voivat erota toisistaan erillään, mutta ne ovat koordinoitu sovelluksen logiikkaa, joka sisältää tehtävän mukaan. Lisätietoja on artikkelissa [Ajoituksen agentti valvojan kuvio](http://msdn.microsoft.com/library/dn589780.aspx).
- **Tehtävän vaiheet, jotka epäonnistua palauttamisen hallinta**. Sovelluksen joutua muuttamaan Kumoa työmäärän, joka suoritetaan tietyt vaiheet (joka ilmestyy yhdenmukaisia toiminnon määrittäminen yhdessä), jos yksi tai useita vaiheita epäonnistuu. Lisätietoja on artikkelissa [Jalostettujen tapahtuman kuvio](http://msdn.microsoft.com/library/dn589804.aspx).

## <a name="lifecycle-cloud-services"></a>Elinkaari (pilvipalveluihin)

 Jos päätät toteuttamisesta taustan työt Cloud Services-sovelluksissa, jotka käyttävät Internetin kautta tai työntekijä roolit **RoleEntryPoint** -luokan avulla, on tärkeää ymmärtää tähän luokkaan elinkaari, jotta voit käyttää oikein.

Internetin kautta tai työntekijä roolit käymällä läpi joukko eri vaiheet, Käynnistä-painiketta, suorittaa ja lopettaa. **RoleEntryPoint** luokan paljastaa tapahtumia, jotka osoittavat, kun nämä vaiheet ovat määrä sarjaa. Käytä näitä alustaa, suorittaa, ja Lopeta mukautetun taustatehtävät. Valmis jakso on:

- Azure Lataa roolin kokoonpanon ja etsii luokka, joka johdetaan **RoleEntryPoint**.
- Jos ohjelma löytää tähän luokkaan, puheluiden **RoleEntryPoint.OnStart()**. Voit ohittaa tämä menetelmä alustaa taustatehtävät.
- **OnStart** menetelmä päätyttyä Azure puhelut **Application_Start()** sovelluksen yleinen tiedostoon, jos tämä on esitä (esimerkiksi käytössä ASP.NET web-roolin Global.asax).
- Azure kutsuu **RoleEntryPoint.Run()** uusi edusta-viestiketjun, joka suorittaa **OnStart()**kanssa samanaikaisesti. Voit ohittaa tällä menetelmällä voit aloittaa taustatehtävät.
- Suorita menetelmä päättyessä Azure soittaa ensin **Application_End()** sovelluksen yleinen mallitiedosto, jos tämä ei sisällä tietoja ja soittaa **RoleEntryPoint.OnStop()**. Voit ohittaa lopettaa taustatehtävien, resurssien Puhdista, luovuttaa objektit ja sulje yhteydet, jotka tehtävät ovat käyttäneet **OnStop** -menetelmää.
- Azure työntekijän rooli host-prosessi on pysäytetty. Tässä vaiheessa rooli ole kuluessa ja käynnistetään uudelleen.

Lisätietoja ja Esimerkki menetelmillä **RoleEntryPoint** -luokka on artikkelissa [Laske resurssin koonnin kuvio](http://msdn.microsoft.com/library/dn589778.aspx).

## <a name="considerations"></a>Huomioon otettavia seikkoja

Kun suunnittelet miten taustaprosessit suoritat verkossa tai Työntekijä-roolin, ota huomioon seuraavat seikat:

- Oletusarvoinen **Suorita** menetelmä käyttöönoton **RoleEntryPoint** luokan sisältää **Thread.Sleep(Timeout.Infinite)** , joka säilyttää rooli toiminnassa jatkuvasti kutsu. Voit ohittaa **Suorita** -menetelmän (joka on yleensä tarvitse suorittaa taustaprosessit), jos sinun on sallittava ei poistu tavan, ellet halua Roskakorin roolin esiintymän koodi.
- **Suorita** -menetelmä tyypillinen soveltaminen sisältyy koodi Aloita kunkin taustatehtävät ja silmukan rakenteen, joka tarkistaa taustatehtävien tila. Se uudelleen mitään, epäonnistuvat tai valvoa peruutus tunnuksia, jotka ilmaisevat, että työt on valmis.
- Taustatehtävän ilmoittaa käsittelemättömän poikkeuksen vuoksi, jos tehtävä on oltava kuluessa samalla muiden taustaprosessit rooli Jatka. Kuitenkin jos poikkeuksen aiheuttivat objektien ulkopuolella tehtävän, kuten jaettua tallennustilan vioittumisen poikkeuksen käsittelemien **RoleEntryPoint** -luokan kaikki tehtävät voidaan peruuttaa ja voi **suorittaa** -menetelmä lopettaa. Azure sitten käynnistää rooli.
- **OnStop** menetelmän avulla voit keskeyttää tai lopettaa taustaprosessit ja resurssien Puhdista. Tämä saattaa liittyä pysäyttäminen pitkään käynnissä olevien tai multistep tehtäviä. On tärkeä ottaa huomioon, kuinka voit tehdä tämän tietojen epäyhtenäisyydet välttämiseksi. Roolin esiintymän lakkaa jostain syystä kuin käyttäjän käynnistämä Sammuta, **OnStop** menetelmä suoritettavan koodin on täytettävä viisi minuuttia, ennen kuin se katkaistaan asiakasistuntojen sisällä. Varmista, että koodisi voi valmistua kyseiseen aikaan tai hyväksyt työn kesto päivinä ei suoriteta.  
- Azure kuormituksen käynnistyy, joka ohjaa liikenne rooli-esiintymässä, kun **RoleEntryPoint.OnStart** -menetelmä palauttaa arvon **true**. Harkitse vuoksi niin, että rooli esiintymät, joka ei onnistunut alustaa saa liikenteen kentän osoittamassa kaikki koodisi alustus **OnStart** -menetelmää.
- Voit käyttää käynnistys menetelmiä **RoleEntryPoint** luokan lisäksi tehtävät. Käytettävä käynnistys tehtävät alustaa asetuksia, jotka haluat muuttaa Azure kuormituksen, koska nämä tehtävät suoritetaan ennen roolin vastaanottaa pyyntöihin. Lisätietoja on artikkelissa [Azure käynnistys tehtävien suorittaminen](./cloud-services/cloud-services-startup-tasks.md).
- Jos käynnistys tehtävän tapahtuu virhe, se saattaa pakottaa rooli jatkuvasti uudelleen. Näin voit estää suorittamiseen virtual IP (VIP) osoite Vaihda takaisin aiemmin vaiheistettu version, koska swap edellyttää yksityisesti roolin. Ei voi hakea, kun rooli käynnistetään. Voit ratkaista ongelman:
    -  Lisää seuraava koodi tehtäväsi **OnStart** ja **Suorita** esitetyssä alkuun:

    ```C#
    var freeze = CloudConfigurationManager.GetSetting("Freeze");
    if (freeze != null)
    {
        if (Boolean.Parse(freeze))
        {
            Thread.Sleep(System.Threading.Timeout.Infinite);
        }
    }
    ```

   - Lisää **Kiinnitä** -asetuksen kuvauksen totuusarvon ServiceDefinition.csdef ja ServiceConfiguration. *.cscfg tiedostoja roolin ja aseta sen arvoksi* *Epätosi* *. Jos roolin siirtyy toistuvien uudelleenkäynnistyksen-tilaan, voit muuttaa asetusta * *Tosi** kiinnittäminen roolin suorittamisen ja sen voi vaihtaa aiemmalla versiolla.

## <a name="resiliency-considerations"></a>Vikasietoisuudelle huomioon otettavia seikkoja

Taustatehtävät on oltava joustavat luotettava palvelujen sovellukseen. Kun suunnittelu ja suunnittelet taustaprosessit, ota huomioon seuraavat seikat:

- Taustaprosessit voivat käsitellä tilanteen ilman teeskentelee tietojen tai esittely ristiriidassa sovellukseen rooli tai palvelu käynnistyy. Pitkään suoritettavien tai multistep tehtävien kannattaa käyttää mukaan tallentamista töiden tilan pysyvän säilön tai viesteinä jonossa tätä vaihtoehtoa, jos _valitsemalla Tarkista_ . Voit esimerkiksi jatkuvat tilatietoja viestin jonossa ja Päivitä vaiheittainen tilatietojen tehtävän edistymisen niin, että tehtävä voidaan käsitellä viimeisen tunnetut hyvä tarkistuspiste--sijaan uudelleenkäynnistyksen alusta. Azure-palvelu Bus olevien käytettäessä voit käyttää viestin istunnot käyttöön sama skenaario. Istunnot avulla voit tallentaa ja hakea sovelluksen käsittely tilan [SetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.setstate.aspx) ja [GetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.getstate.aspx) tavoilla. Lisätietoja luotettava multistep prosesseja ja työnkulkuja artikkelissa [Ajoituksen agentti valvojan kuvio](http://msdn.microsoft.com/library/dn589780.aspx).
- Kun verkossa tai työntekijä roolit isännöimiseen useita taustaprosessit, suunnitella käyttäjän ohitus **Suorita** menetelmän epäonnistui tai pysäytetty tehtävien valvoa ja käynnistä ne uudelleen. Jos tämä ei kannata ja käytät työntekijän rooli, pakottaa työntekijän rooli Käynnistä lopettamisen **Suorita** -menetelmää.
- Kun käytät olevien pitää yhteyttä taustatehtävät olevien voi toimia puskurin tallentamiseen pyyntöjä, jotka lähetetään tehtävät, kun sovellus on suurempi kuin tavanomainen lataaminen-kohdassa. Näin vähemmän varattu aikoina niin, että Käyttöliittymä keskusteluviestit tehtävät. Se tarkoittaa sitä, että kierrättäminen rooli ei estä Käyttöliittymän. Lisätietoja on artikkelissa [jonon perustuva kuormituksen tasaaminen kuvio](http://msdn.microsoft.com/library/dn589783.aspx). Jos jotkin tehtävät ovat tulostusjälkeä tärkeämpi, harkitse [Prioriteetti jonon kuvion](http://msdn.microsoft.com/library/dn589794.aspx) varmistaa, että nämä tehtävät suoritetaan ennen vähemmän tärkeitä niistä.
- Taustan tehtäviin, jotka ovat aloittamia viestejä tai käsitellä viestejä suunniteltava käsittelemään epäyhtenäisyydet, kuten epäjärjestyksessä saapuvat viestit tai viestit, jotka aiheuttavat toistuvasti virheen (kutsutaan usein _sanomien_) viestit toimitetaan useita kertoja. Toimi seuraavasti:
  - Viestit, jotka on käsiteltävä tietyssä järjestyksessä, kuten ne, jotka muuttavat tietoja (esimerkiksi arvon lisääminen olemassa olevaan arvoon) aiemmin tietoarvon perusteella, ehkä ei tule perille alkuperäinen tilaus, johon ne on lähetetty. Voit myös ne voivat käsitellä eri esiintymien taustan tehtävän vuoksi erilaisten Lataa jokaiselle esiintymälle eri järjestyksessä. Viestit, jotka on käsiteltävä tietyssä järjestyksessä projektityypin järjestysnumero, avaimen tai muun ilmaisin, taustaprosessit avulla voit varmistaa, että niitä käsitellään oikeassa järjestyksessä. Jos käytössäsi on Azure palvelun Bus, voit taata toimituksen järjestyksen viestin istuntojen. On kuitenkin yleensä tehokkaampaa, jos mahdollista, suunnitella prosessi, jotta viesti järjestyksellä ei ole merkitystä.
  - Taustatehtävän yleensä vilkaista sanomia jonossa, joka piilottaa ne väliaikaisesti viestin muut käyttäjät. Sitten se poistaa viestejä, kun ne on käsitelty onnistuneesti. Jos taustatehtävän epäonnistuu, jos viestin käsittelyn, viestin palautetaan jonossa pikanäkymä aikakatkaisun päätyttyä. Se käsitellään toiseen käynnissä tehtävän tai sen aikana seuraavan käsittely jakson, tämän esiintymän mukaan. Jos viestin aiheuttaa virheen johdonmukaisesti kuluttajien, se estää tehtävän, jonossa ja tekstin sovelluksesta jonossa tullessa täyteen. Tämän vuoksi on tärkeä havaitsemaan ja poistamaan sanomien jonossa. Jos käytössäsi on Azure palvelun Bus, viestit, jotka aiheuttavat virheen voidaan siirtää automaattisesti tai manuaalisesti liittyvät perille kirjain jonossa.
  - _Vähintään kerran_ toimitus-järjestelmiä osoitteessa on varmasti olevien, mutta niitä voi tuottaa saman viestin useita kertoja. Lisäksi jos taustatehtävän epäonnistuu viestin käsittelyn jälkeen mutta ennen kuin poistat jonossa, näyttöön tulee saataville käsittelyyn uudelleen. Taustatehtävät on oltava idempotent, mikä tarkoittaa, että sama viestin käsittelyn useammin kuin kerran ei aiheuttaa virheen tai ristiriidassa sovelluksen tiedot. Jotkin toiminnot ovat luonnollisesti idempotent, kuten tallennettu arvo asettaminen tietyn uuden arvon. Kuitenkin toimintoja, kuten arvon lisääminen aiemmin tallennetun arvon tarkistamatta tallennettu arvo on edelleen sama kuin kun viesti alun perin lähetettiin aiheuttavat epäyhtenäisyydet. Azure palvelun Bus olevien voi määrittää automaattisesti Poista kaksoiskappaleet viestejä.
  - Tekstiviesti järjestelmien, kuten Azure tallennustilan olevien ja Azure palvelun Bus olevien tukevat de-queue Laske-ominaisuutta, joka osoittaa, kuinka monta kertaa viesti on luettu jonossa. Tämä voi olla hyödyllistä toistuvia ja torjuttujen viestien käsittelyyn. Lisätietoja on artikkelissa [Asynkroninen Messaging askeleet](http://msdn.microsoft.com/library/dn589781.aspx) ja [Ainutkertaisuus kuviot](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/).

## <a name="scaling-and-performance-considerations"></a>Skaalaus ja suorituskyvyn huomioon otettavia seikkoja

Taustatehtävät on annettava riittävästi suorituskyvyn, jotta ne eivät estää sovelluksen tai aiheuttaa epäyhtenäisyydet viivästyneet toiminnon vuoksi, kun järjestelmä on kohdassa Lataa. Yleensä suorituskykyä on parannettu mukaan skaalaus Laske esiintymät, jotka isännöivät taustatehtävät. Kun suunnittelu ja suunnittelet taustaprosessit, ota huomioon skaalattavuus ja suorituskyvyn ympärille seuraavat seikat:

- Nykyinen demand ja lataa--perusteella Azure tukee automaattisen skaalauksen poistaminen (sekä skaalaus ulos ja takaisin sisään skaalaus) tai valmiin aikataulussa verkkosovelluksissa pilvipalveluihin web ja työntekijä rooleista ja näennäiskoneiden isännöidään ominaisuuksissa. Tämän toiminnon avulla voit varmistaa, että sovellus kokonaisuudessaan on riittävän suorituskykyä ja pienentäminen runtime kustannukset.
- Jos taustatehtävät on eri suorituskyky-ominaisuuksien Cloud Services-sovellus (esimerkiksi Käyttöliittymän tai osia, kuten tiedot access-layer) muista osista, isännöinnin taustatehtävät yhteen erilliseen Työntekijä-roolin avulla Käyttöliittymän ja tausta tehtävän roolien hallinta lataa skaalata erikseen. Jos useita taustaprosessit on merkittävästi eri suorituskykyä toisistaan, harkitse jakamalla ne erillisessä työntekijä rooleihin ja skaalausta roolin kunkin tyyppiselle itsenäisesti. Huomaa kuitenkin, että tämä saattaa kasvattaa runtime kustannukset verrattuna vähemmän rooleja taulukon yhdistämällä kaikki tehtävät.
- Skaalaus yksinkertaisesti roolit ei välttämättä riitä estämään kuormituksen suorituskyvyn menetyksiä. Joudut ehkä myös skaalata tallennustilan olevien ja muita resursseja, voit estää yleinen yksittäisen kohdan käsittelyn ketju tulemasta pullonkaula. Ota huomioon myös muita rajoituksia, kuten suurin nopeus tallennustilan ja muut palvelut, jotka perustuvat sovellus ja taustatehtävät.
- Taustatehtävät on suunniteltava skaalauksen. Ne voivat esimerkiksi tunnistaa dynaamisesti kuuntelevat tai lähettää viestejä sopivan jonon käytössä olevien tallennustilan määrä.
- Oletusarvon mukaan WebJobs skaalata niiden liittyvän Azure Web Apps-esiintymää. Jos haluat suorittaa vain yhden esiintymän WebJob, voit kuitenkin luoda Settings.job JSON-tietoja sisältävän tiedoston **{"is_singleton": TOSI}**. Tämä pakottaa Azure toimimaan vain yhden esiintymän WebJob, vaikka on liitetty online useita kertoja. Tämä voi olla hyötyä tekniikka ajoitetun projekteille, jotka on suoritettava vain yksi esiintymä.

## <a name="related-patterns"></a>Aiheeseen liittyvät kuviot

- [Asynkroninen tekstiviesti askeleet](http://msdn.microsoft.com/library/dn589781.aspx)
- [Automaattisen skaalauksen poistaminen ohjeita](http://msdn.microsoft.com/library/dn589774.aspx)
- [Jalostettujen tapahtuman kuvio](http://msdn.microsoft.com/library/dn589804.aspx)
- [Kilpailevien kuluttajille kuvio](http://msdn.microsoft.com/library/dn568101.aspx)
- [Laske jakaminen ohjeet](http://msdn.microsoft.com/library/dn589773.aspx)
- [Laske resurssin koonnin kuvio](http://msdn.microsoft.com/library/dn589778.aspx)
- [Gatekeeper kuvio](http://msdn.microsoft.com/library/dn589793.aspx)
- [Täytemerkki osavaltioiden kuvio](http://msdn.microsoft.com/library/dn568104.aspx)
- [Putket ja suodattimet-kaava](http://msdn.microsoft.com/library/dn568100.aspx)
- [Prioriteetti jonon kuvio](http://msdn.microsoft.com/library/dn589794.aspx)
- [Jonon perustuva kuormituksen tasaamisen kuvio](http://msdn.microsoft.com/library/dn589783.aspx)
- [Ajoituksen agentti valvojan kuvio](http://msdn.microsoft.com/library/dn589780.aspx)

## <a name="more-information"></a>Lisätietoja

- [Skaalaus Azure sovellusten työntekijä roolit](http://msdn.microsoft.com/library/hh534484.aspx#sec8)
- [Taustatehtävien suorittaminen](http://msdn.microsoft.com/library/ff803365.aspx)
- [Azure roolin käynnistys elinkaari](http://blog.syntaxc4.net/post/2011/04/13/windows-azure-role-startup-life-cycle.aspx) (blogimerkintä)
- [Azure Cloud Services-roolin elinkaari](http://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Windows-Azure-Cloud-Services-Role-Lifecycle) (video)
- [Azure WebJobs SDK: N käytön aloittaminen](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- [Azure olevien ja palvelun Bus olevien - verrattuna ja asiaan](./service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Diagnostiikan pilvipalvelussa ottaminen käyttöön](./cloud-services/cloud-services-dotnet-diagnostics.md)
