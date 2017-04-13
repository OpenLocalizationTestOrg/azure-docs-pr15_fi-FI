<properties
   pageTitle="Suuren käytettävyyden Azure sovellusten | Microsoft Azure"
   description="Teknisiä tietoja ja yksityiskohtaisempiin suunnittelu ja suuren sovellusten luominen Microsoft Azure-tietoihin."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-for-applications-built-on-microsoft-azure"></a>Microsoft Azure rakennettu sovellusten suuri käytettävyys

Hyvin käytettävissä oleva kotimaisella vaihtelujen käytettävyyden, lataa ja tilapäinen virheiden riippuvaiset palvelut ja laitteet. Sovelluksen säilyy toimivat hyväksyttävä käyttäjän ja systeeminen vastauksen tasolla määrittämän business vaatimukset tai sovelluksen palvelun tason sopimuksia (palvelutasosopimuksia).

##<a name="azure-high-availability-features"></a>Azure suuren käytettävyyden ominaisuudet

Azure on monia valmiita platform-ominaisuuksia, jotka tukevat erittäin käytettävissä olevat sovellukset. Tässä osassa käsitellään joitakin näistä keskeisiä ominaisuuksia. Ympäristön monipuolisemman analyysi-kohdassa [Azure vikasietoisuudelle tekniset ohjeet](./resiliency-technical-guidance.md).

###<a name="fabric-controller"></a>Kangasta ohjain

Azure kangasta ohjauskoneen valmistelee ja valvoo Azure Laske esiintymien ehto. Kangasta ohjauskoneen tarkistaa tilan laitteiston ja ohjelmiston isännän ja Vieras machine-esiintymien. Kun se havaitsee virheen, se pakottaa palvelutasosopimuksia mukaan kunnolliselle automaattisesti AM esiintymät. Edelleen vika ja päivität toimialueen käsitteestä tukee Laske SLA.

Useita pilvipalvelussa roolin esiintymiä on otettu käyttöön, kun Azure tunnistus nämä esiintymiä eri vika toimialueet. Virhe toimialueen rajan on lähinnä eri laitteisto-Teline samalla alueella. Vian toimialueiden pienentää todennäköisyyden sille, että lokalisoitu epäonnistui keskeyttää palvelun sovelluksen. Et voi hallita vian toimialuemäärän, joka jaetaan työntekijä tai web roolit. Kangasta ohjauskoneen käytetään erillinen Azure isännöimä sovellusten erillään olevat resursseja. Siinä on 100 prosenttiin käytettävyyttä, koska se on Azure järjestelmän solulimaan. Se valvoo ja hallitsee roolin esiintymät vika toimialueilla.

Seuraavassa kaaviossa on esitetty Azure jaettuja resursseja kangasta ohjauskoneen ottaa käyttöön ja hallitsee eri vika toimialueilla.

![Virhe toimialueen eristyksen yksinkertaistettu näkymä](./media/resiliency-high-availability-azure-applications/fault-domain-isolation.png)

Päivityksen toimialueiden muistuttavat vika toimialueet-funktio, mutta ne tue päivitykset virheiden sijaan. Päivitä toimialueen on looginen mittayksikön esiintymän etäisyyttä, joka määrittää tietyn palvelun esiintymät päivitetään vaiheessa ajassa. Viisi päivityksen toimialueet on määritetty oletusarvoisesti isännöityä service-käyttöönottoa varten. Voit kuitenkin muuttaa palvelun määritystiedoston arvo. Oletetaan esimerkiksi, että web-roolin kahdeksan esiintymät. Käytettävissä on kaksi esiintymää kolme päivityksen Domains ja kaksi esiintymää yhden päivityksen toimialueen. Azure määrittää päivityksen järjestyksen, mutta se perustuu päivityksen toimialuemäärän. Lisätietoja päivityksen toimialueet-kohdassa [Päivitä pilvipalveluun](../cloud-services/cloud-services-update-azure-service.md).

###<a name="features-in-other-services"></a>Muiden palveluiden ominaisuudet

Lisäksi ympäristön ominaisuuksia, jotka tukevat hyvin Laske käytettävyys Azure upottaa sen muiden palvelujen tuominen suuren käytettävyyden ominaisuuksia. Esimerkiksi Azuren tallennustilaan ylläpitää kolme replikoiden blob, taulukon ja jonon tiedot. Sen avulla myös geo replikoinnin voi tallentaa varmuuskopioita BLOB-objektit ja taulukoiden toissijainen alueen. Azure sisällön toimituksen verkko sallii BLOB välimuistiin redundancy ja skaalattavuus maailmaa. Azure SQL-tietokantaan ylläpitää useita replikoita.

Lisäksi [Vikasietoisuudelle tekniset ohjeet](https://aka.ms/bctechguide) sarja artikkeleihin Katso [Parhaat käytännöt rakenne, suuren mittakaavan palvelut Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/) -raportti. Näissä on tarkempaa keskustelun Azure ympäristö käytettävyys ominaisuuksia.

Azure on useita toimintoja, jotka tukevat suuren käytettävyyden, mutta se on tärkeää ymmärtää niiden rajoitukset:

- Laske-Azure takaa, että roolit ovat käytettävissä ja käytössä, mutta se ei tiedä, jos sovellus on käynnissä tai ylikuormitettu.
- Azure SQL-tietokannan tietojen replikoinnin synkronoidusti alueella. Voit valita aktiivisen geo-replikoinnin, joka sallii enintään neljä toisen tietokannan kopiota saman alueen (tai eri alueilta). Nämä Tietokantareplikoita eivät ole ajankohta varmuuskopiot. SQL-tietokantoja on ajankohta varmuuskopioinnin ominaisuuksia. Lisätietoja SQL tietokanta-ajankohta ominaisuuksia, lue [Azure SQL tietokanta pisteen aika palauttaminen](https://azure.microsoft.com/blog/azure-sql-database-point-in-time-restore/).
- Azure-tallennustilan taulukko-ja blob on replikoida oletusarvoisesti vaihtoehtoinen alue. Kuitenkin ei voi käyttää replikat, kunnes Microsoft valitsee epäonnistuu vaihtoehtoinen sivuston kautta. Alueen automaattisesti tapahtuu vain pitkäaikaisesta alueen laajuisen keskeytetty kyseessä ja ei ole SLA geo automaattisesti ajan. On myös tärkeää muistaa, että kaikki tietovirheitä näkymät nopeasti replikoita.

Seuraavien syiden takia on täydentää sovelluksen kielikohtaiset käytettävyys ominaisuuksilla ympäristön käytettävyys-ominaisuuksia. Sovelluksen kielikohtaiset käytettävyyden ominaisuuksia kummassakin luominen Blob-objektien tietojen varmuuskopioinnin ajankohta Blob-objektien tilannevedos-toiminto.

###<a name="availability-sets-for-azure-virtual-machines"></a>Käytettävyys määrittää Azuren näennäiskoneiden

Tässä artikkelissa useimpia keskitytään pilvipalveluihin, jonka avulla ympäristö service (PaaS)-mallina. Välillä on kuitenkin myös tiettyjen käytettävyyden ominaisuuksia varten Azuren näennäiskoneiden, joka käyttää infrastruktuuri service (IaaS)-mallina. Tavoitteet näennäiskoneiden suuri käytettävyys, sinun on käytettävä käytettävyys joukot. Käytettävyys määrittäminen toimii samalla funktion vika ja päivität toimialueeseen. Käytettävyys-asettaa Azure sijoittaa näennäiskoneiden niin, että estää lokalisoitu laitteiston virheitä ja ylläpitotoimet joka tuo kaikki kyseisen ryhmän koneet alaspäin. Käytettävyys joukot tarvitaan näennäiskoneiden käytettävyyden Azure-SLA saavuttamiseksi.

Seuraavassa kaaviossa on kuvaus käytettävyys kahdet kyseisen ryhmän web ja näennäiskoneiden SQL Server-tarpeen mukaan.

![Käytettävyys määrittää Azuren näennäiskoneiden](./media/resiliency-high-availability-azure-applications/availability-set-for-azure-virtual-machines.png)

>[AZURE.NOTE] SQL Server on asennettu ja käytössä näennäiskoneiden edellä olevassa kaaviossa. Tämä on erilainen kuin Azure SQL-tietokantaan, joka sisältää tietokannan hallitun palveluna edelliseen keskusteluun.

##<a name="application-strategies-for-high-availability"></a>Sovelluksen strategioita suuri käytettävyys

Useimmat sovelluksen strategioita suuren käytettävyyden liittyä redundancy tai kiintolevyn sovelluksen osia väliset riippuvuudet poistamista. Sovelluksen suunnittelussa tulisi tukevat vikasietoa arviointi käyttökatkot Azure tai kolmannen osapuolen palvelujen aikana. Seuraavissa kohdissa kuvataan sovelluksen kuvioiden määrittäminen lisääntyvien cloud Services-palvelut käytettävyyttä.

###<a name="asynchronous-communication-and-durable-queues"></a>Asynkroninen viestintää ja kestävät olevien

Harkitse asynkroninen tietoliikenne niin, että Azure sovellusten käytettävyyden löyhästi kytkettyjä palveluiden välillä. Kirjoita tämä kaava viestien tallennustilan olevien tai Azure palvelun Bus olevien myöhempää käsittelyä varten. Kun kirjoitat viestin jonossa, ohjausobjektin palauttaa heti viestin lähettäjälle. Toisen tason sovelluksen käsittelee viestin käsittelyn, yleensä toteutettu työntekijän rooli. Jos työntekijä-roolin, viestit kertyy jonossa, kunnes käsittelypalvelu on palautettu. Kun jonossa on käytettävissä, ei ei ole suoraa riippuvuuden edusta lähettäjän ja viestin suorittimen välillä. Näin ei synkronoitu service-puheluihin, jotka voidaan siirtonopeuden pullonkaula jaetut sovellukset-taso.

Tämä variaatiota käyttää automaattisesti sijainniksi Azuren tallennustilaan (BLOB-objektit, taulukot, olevien) tai palvelun Bus olevien vioittuneen tietokannan kutsuihin. Esimerkiksi sovelluksesta toiseen palveluun (kuten Azure SQL-tietokanta) synkronisen kutsu epäonnistuu toistuvasti. Voit ehkä onnistu tiedot kestävät varastoon. Myöhemmin joskus palvelu tai tietokanta on online-tilaan, kun sovellus uudelleen lähettää pyynnön säilöstä. Tämän mallin ero on keskitason sijainti ei ole sovelluksen työnkulun pysyvien osa. Sitä käytetään vain tietyissä skenaarioissa, virhe.

Skenaariot, asynkroninen tietoliikenne ja keskitason tallennustilan estää downed taustatietokantaan palvelu joka tuo koko sovelluksen alaspäin. Olevien yhteyshenkilönä looginen edustaja. Lisää ohjeita oikea jonotuspalvelu valitsemisesta on artikkelissa [Azure olevien ja Azure palvelun Bus olevien--verrattuna ja asiaan](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

###<a name="fault-detection-and-retry-logic"></a>Vian tunnistaminen ja yritä logiikka

Avaimen vaiheessa hyvin käytettävissä oleva rakenne on uudelleen logiikkaa koodin avulla voit käsitellä tilanteen palvelu, joka on väliaikaisesti poissa käytöstä. [Lyhytkestoisia vika käsittely sovelluksen lohko](https://msdn.microsoft.com/library/hh680934.aspx), Microsoft Patterns ja kuvaukset-ryhmän kehittämä auttaa sovelluskehittäjät prosessia. Sanan "lyhytaikainen" tarkoittaa tilapäinen, joka kestää vain suhteellisen lyhyt aika. Tämän artikkelin kontekstissa lyhytkestoisia virheiden käsittelyä on käytettävissä sovelluksen osa. Esimerkkejä lyhytkestoisia ehdoista katkonainen verkon virheiden ja tietokantayhteyksiä menetetään.

Lyhytkestoisia vika käsittely sovelluksen-lohko on yksinkertaistettu siten, että koodiin virheiden kaksivaiheista tavalla. Sen avulla voit parantaa sovellusten käytettävyyttä lisäämällä tehokkaat lyhytkestoisia virheen käsittely-logiikkaa. Useimmissa tapauksissa uudelleen logiikan käsittelee lyhyt keskeytymisen ja lähettäjä ja vastaanottaja yhdistää yhden tai useamman epäonnistuneiden yritysten jälkeen. Onnistuneiden uudelleen yrityksellä yleensä siirtyy jäädä huomaamatta sovelluksen käyttäjille.

Kehittäjät on kolme vaihtoehtoa, yritä niiden logiikan hallintaan: vaiheittainen, kiinteisiin aikaväli ja eksponentiaalisen. Vaiheittainen odottaa enää ennen kunkin uudelleen kasvava lineaarisen tavalla (esimerkiksi 1, 2, 3 ja 4 sekuntia). Kiinteä aikavälin odottaa saman kunkin uudelleen (esimerkiksi 2 sekuntia) välisen ajan. Lisää satunnaisia asetus Eksponentiaalinen Edellinen-käytöstä odottaa enää uudelleenyritykset välillä. Kuitenkin käyttää eksponentiaalisen toiminta (esimerkiksi 2, 4, 8 ja 16 sekuntia).

Oman koodiin korkean tason strategia on:

1. Määritä uudelleen strategia ja käytännön.
1. Yritä toimintoa, joka saattaa aiheuttaa lyhytkestoisia vika.
1. Jos lyhytkestoisia virhe ilmenee, käynnistää uudelleen käytännön.
1. Jos kaikki uudelleenyritykset epäonnistuu, todellisen lopullinen poikkeuksen.

Testaa uudelleen logiikan Simuloitu epäonnistuu, varmista, että uudelleenyritykset peräkkäisiä, ei johtaa odottamattomiin pitkä viive. Tee tämä ennen kuin päätät epäonnistua yleinen tehtävän.

###<a name="reference-data-pattern-for-high-availability"></a>Viittaus tietojen kuvio suuri käytettävyys

Viitetiedot on vain luku-sovelluksen tiedot. Tämä tieto on business kontekstissa, jossa sovellus luo tapahtumatietojen business-toiminnon aikana. Tapahtumatietojen on viitetiedot ajankohta funktio. Tämän vuoksi voimassaolo määräytyy viitetiedot tilannevedoksen tapahtumaan aikaa. Tämä on hieman irtoaa määritys, mutta se tulee riittää Microsoftin tarkoitusta varten.

Sovelluksen toimintaan tarvitaan viitetiedot sovelluksen kontekstissa. Vastaaviin sovellukset luovat ja ylläpitävät viitetiedot; Perustietojen management (MDM) järjestelmien usein suorittaa tämän toiminnon. Nämä järjestelmät ovat vastuussa viitetiedot elinkaaren. Esimerkkejä viitetiedot ovat tuoteluettelon, työntekijän perustyyli, osien perustyyli ja laitteiden perustyyli. Viitetiedot myös peräisin organisaation ulkopuolella, kuten postinumeroiden tai ALV-kurssit. Strategioita lisääntyvien viitetiedot käytettävyyttä on yleensä vähemmän vaikea kuin ne tapahtumatietojen varten. Viitetiedot on se etu, että enimmäkseen pysyvä.

Voit tehdä Azure Internetin kautta tai työntekijä roolit, joilla on tarjoaman viitetiedot yksipuolisia suorituksen ottamalla viittaus tiedot sovelluksen kanssa. Jos paikallisen tallennustilan koon sallii näiden käyttöönoton, tämä on paras mahdollinen tila. Upotettu tietokannat (SQL, NoSQL) tai XML-tiedostoja, jotka on otettu käyttöön paikallisessa tiedostojärjestelmässä auttaa Azure Laske aikayksikön itsemääräämisoikeus. Kuitenkin on oltava, jolla kussakin roolissa tietojen päivittäminen tarvitsematta lukea. Voit tehdä tämän Aseta päivityksiä viitetiedot cloud tallennustilan päätepistettä (esimerkiksi Azure-Blob-säiliö tai SQL-tietokanta). Lisää koodi kunkin rooli tietojen päivitykset ladataan kyselyjä Laske solmut roolin käynnistyksen yhteydessä. Voit myös lisätä koodi, jonka avulla järjestelmänvalvoja voi suorittaa kyselyjä roolin esiintymät pakotettu latauksen.

Käytettävyys parantamiseksi roolien on myös oltava viittaus tietojoukolle siltä varalta, että tallennus on alaspäin. Näin roolit käynnistämisestä niin, että käytössä on basic viitetiedot, kunnes tallennustilan resurssi on käytettävissä olevat päivitykset.

![Sovelluksen suuren käytettävyyden yksipuolisia Laske solmujen kautta](./media/resiliency-high-availability-azure-applications/application-high-availability-through-autonomous-compute-nodes.png)

Yksi huomioon tätä mallia on käyttöönotto- ja käynnistys nopeuden oman roolit. Jos otat tai ladata suuria määriä viitetiedot käynnistyksen yhteydessä, tämä Lisää latausaikaan asettamasi uuden ominaisuuksissa tai rooli esiintymien määrää. Tämä voi johtua hyväksyttävä tarjoa itsenäisyyttä ottaa heti käytettävissä viitetiedot rooleille sen sijaan, että sen mukaan, ulkoisille palveluja varten.

###<a name="transactional-data-pattern-for-high-availability"></a>Tapahtumatietojen kuvio suuri käytettävyys

Tapahtumatietojen on tiedot, jotka sovellus luo business kontekstissa. Tapahtumatietojen on joukko liiketoimintaprosesseja, joissa sovelluksen toteuttaa ja viitetiedot, joka tukee nämä prosessit yhdistelmä. Tapahtumatietojen voit esimerkiksi tilaukset, Lisäasetukset lähetyksen ilmoitukset, laskuja ja asiakas yhteyden hallintapalvelut mahdollisuudet. Näin luoda tapahtumien tiedot fed ulkoisten järjestelmien tietojärjestelmä tai lisäkäsittelyä varten.

Ota huomioon, jotka viittaavat muuttaa tietoja, jotka ovat vastuussa tiedoista järjestelmien puitteissa. Tästä syystä tapahtumatietojen on tallennettava ajankohta viittaus tietojen kontekstia niin, että se on mahdollisimman vähän riippuvuussuhteet semanttinen johdonmukaisuutta varten. Esimerkkinä muutaman kuukauden sen jälkeen, kun tilaus on täytetty tuotteen ja luettelon poistaminen. Paras käytäntö on upottaa tapahtuman mahdollisimman paljon viittaus tietojen kontekstia kuin on mahdollista. Tämä säilyttää semantiikkaan liittyvien, tapahtumaan liittyvä vaikka viittaus tiedot muuttuvat, kun tapahtuma kirjataan.

Kuten edellä mainittiin, arkkitehtuureihin, jotka käyttävät irtoaa kytkentä ja asynkroninen tietoliikenne lainata itse käytettävyys korkeammat tasot. Tämä koskee myös tapahtumatietojen, mutta käyttöönoton on monimutkaisempi. Perinteinen tapahtumien huomautukset luottavat yleensä tietokannan voimassaolevan tapahtuma. Kun esittelet keskitason kerrokset, sovellus-koodi on käsiteltävä tietoliikenteen kerroksiin siten varmistaa riittävät kestävyyttä tiedoissa oikein.

Työnkulun, joka erottaa sen käsittely tapahtumatietojen sieppaus on kuvattu seuraavassa järjestyksessä:

1. Internet-Laske solmu: esitä viitetiedot.
1. Ulkoisille: Tallenna keskitason tapahtumatietojen.
1. Internet-Laske solmu: loppukäyttäjien tapahtuma valmiiksi.
1. Internet-Laske solmu: Lähetä valmiit tapahtumien tiedot viittauksen tietojen yhteydessä, sekä kestävät väliaikaisten, joka on varmasti ennakoitavissa vastauksen antaa.
1. Internet-Laske solmu: signaalin tapahtuman loppukäyttäjien suorittamista.
1. Taustan Laske solmu: Poimi tapahtumien tiedot, käsitellä niitä tarvittaessa jälkeinen ja lähetä se sen lopullinen tallennuspaikka nykyisen järjestelmän.

Seuraavassa kaaviossa on yksi tämän rakenteen mahdollista soveltaminen Azure isännöimä pilvipalvelussa.

![Suuren käytettävyyden irtoaa kytkentä kautta](./media/resiliency-disaster-recovery-high-availability-azure-applications/application-high-availability-through-loose-coupling.png)

Edellä olevassa kaaviossa katkoviiva nuolet osoittavat asynkroninen käsittely. Edusta-WWW-roolia ei huomioon tässä asynkroninen käsittely. Tämä johtaa tallennustilan tapahtuman osoitteessa lopulliseen kohteeseen nykyisen järjestelmän mukaisesti. Vuoksi, tämä asynkroninen malli esittelee viive tapahtumatietojen ei ole heti käytettävissä kyselyä varten. Vuoksi tapahtumatietojen kunkin yksikön on tallennettava välimuistiin tai käyttäjän istuntoon täyttävän heti Käyttöliittymän tarvitsee.

Web-rooli on yksipuolisia infrastruktuurin muusta. Käytettävyys sen profiili on web rooli ja Azure jonossa ja koko infrastruktuurin yhdistelmän. Tämän menetelmän avulla lisäksi suuren käytettävyyden skaalata vaakasuunnassa, ulkopuolisista taustatietokantaan tallennustilan web-rooli. Suuren käytettävyyden tämän mallin voi olla vaikutusta toimintojen rakennusmenetelmän. Lisäosia, kuten Azure olevien ja työntekijä roolit voivat vaikuttaa kuukausittain käyttökustannukset.

Huomaa, että edellisen kaaviossa on esitetty yhden soveltaminen tapahtumatietojen erilliset tätä tapaa. On monia muita mahdollista käyttöotot. Seuraavassa luettelossa on muita vaihtoehtoja:

 * Työntekijän rooli saatetaan web rooli ja tallennustilaa jonossa välillä.
 * Palvelun Bus jono voidaan Azuren tallennustilaan jonon sijaan.
 * Lopullinen kohde voi olla Azuren tallennustilaan tai toisesta tietokannasta palveluntarjoaja.
 * Azure välimuistin voidaan säätää heti välimuistiin vaatimukset tapahtuman jälkeen web-tasolla.

###<a name="scalability-patterns"></a>Skaalattavuus kuviot

On tärkeää muistaa, että pilvipalvelussa skaalattavuus vaikuttaa suoraan käytettävyyttä. Jos kasvaa kuormituksen aiheuttaa palvelussa on ei vastaa, käyttäjän vaikutelman siitä, että sovellus on alaspäin. Noudattamalla parhaat käytännöt skaalattavuus perusteella arvioidut sovelluksen lataaminen ja tulevien odotusten mukaisesti. Suurin asteikon liittyy monia huomioon otettavia seikkoja, kuten yhteen usean tallennustilan tilin jakaminen useiden tietokantojen välillä ja tallentamisesta välimuistiin strategioita ja käyttö. Katso nämä kuviot perusteellisempaa katsaus [Rakenne, suuren mittakaavan palvelut Azure pilvipalveluihin parhaat käytännöt](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on kohdistettu [palauttaminen ja suuren käytettävyyden sovellusten Microsoft Azure rakennettu](./resiliency-disaster-recovery-high-availability-azure-applications.md)artikkelisarja osa. Seuraava artikkeli sarjassa on [Microsoft Azure-sovellusten palauttaminen](./resiliency-disaster-recovery-azure-applications.md).
