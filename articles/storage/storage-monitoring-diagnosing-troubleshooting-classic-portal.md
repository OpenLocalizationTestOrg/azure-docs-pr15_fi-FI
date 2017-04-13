<properties
    pageTitle="Valvonta, vianmääritys ja tallennustilaa vianmääritys | Microsoft Azure"
    description="Voit käyttää toimintoja, kuten tallennustilan analytics, asiakkaan kirjaaminen ja muiden kolmansien osapuolten työkalujen laji, diagnosointi ja Azure-tallennustilan liittyvien ongelmien vianmääritys."
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="jahogg"/>

# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Valvonta, diagnosointi ja vianmääritys Microsoft Azuren tallennustilaan

[AZURE.INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Yleiskatsaus

Ohjelmistossa ja vianmääritys hajautettu sovelluksessa ylläpidettävä cloud-ympäristössä, jossa on monimutkaisempi kuin perinteisen ympäristöissä. Sovellusten voidaan ottaa käyttöön PaaS tai IaaS infrastruktuuri paikallinen mobiililaitteessa tai jotkin seuraavista yhdessä. Yleensä sovelluksen verkkoliikennettä voi siirtää julkisista ja yksityisistä verkkojen ja sovellus voi käyttää useita tallennustilan tekniikoita, kuten Microsoft Azure-tallennustilan taulukot, BLOB-objektit, olevien tai tiedostoja muiden tietojen lisäksi tallentaa sellaisia kuin relaatio ja asiakirjan tietokannat.

Voit hallita näiden sovellusten onnistuneesti pitäisi seurata niitä itse ja kuinka määrittää ja ratkaista ne ja niiden alisteiset tekniikat kaikkia ominaisuuksia. Olisi Azuren tallennustilaan palvelujen käyttäjänä jatkuvasti seurata liittyviä palveluja, että sovellus käyttää (kuten hitaammin tavallista vastauksen kertaa) toiminta odottamattomia muutokset ja käyttää lokiin kirjaamista yksityiskohtaisempia tietojen keräämiseen ja analysointiin perusteellisesti ongelma. Hankit sekä seuranta ja kirjaamisen diagnostiikka-tietojen avulla voit pääkansion sovelluksen havaitsi ongelman syy. Voit sitten ongelman vianmäärityksen ja mitä voit tehdä sen riskien parantaminen toimia. Azuren tallennustila on core Azure-palvelu ja lomakkeiden ratkaisuja, jotka asiakkaiden käyttöön Azure infrastruktuurin useimpia tärkeä osa. Azure-tallennustilan sisältää yksinkertaistaa valvonta, ohjelmistossa ja tallennustilaa vianmääritys cloud-sovelluksissa.

> [AZURE.NOTE] Tallennustilan tili ja replikoinnin tyyppi, vyöhykkeen tarpeettomat Storage (ZRS) ei ole arvot tai virheloki-ominaisuus on otettu käyttöön tällä hetkellä. 

Saat käytännön oppaan Azuren tallennustilaan sovellusten vianmääritys lopusta loppuun [-Kattavan vianmääritys Azure-tallennustilan arvot ja kirjaaminen, AzCopy, ja viestin Analyzer](storage-e2e-troubleshooting.md).

+ [Johdanto]
    + [Kuinka tässä oppaassa on järjestetty]
+ [Tallennustilan palvelun seuranta]
    + [Palvelun kunnon valvonta]
    + [Seurannan kapasiteetti]
    + [Käytettävyys seuranta]
    + [Suorituskyvyn valvominen]
+ [Ohjelmistossa tallennustilan ongelmat]
    + [Palvelun ongelmia]
    + [Suorituskykyongelmia]
    + [Ohjelmistossa virheet]
    + [Tallennustilan emulaattorin ongelmat]
    + [Tallennustilan kirjaaminen Työkalut]
    + [Kirjaaminen työkaluilla]
+ [Lopusta loppuun-jäljitys]
    + [Korreloivaa lokitiedot]
    + [Pyyntö Ostajantunnus]
    + [Pyynnön tunnus]
    + [Aikaleimat]
+ [Ohjeet vianmääritys]
    + [Arvot high AverageE2ELatency ja pienen AverageServerLatency näyttäminen]
    + [Arvot näyttää pienen AverageE2ELatency ja pienen AverageServerLatency mutta asiakkaan ilmenee odotusaika]
    + [Näytä arvot high AverageServerLatency]
    + [On ilmennyt odottamaton viipeitä viestin toimituksen jonon]
    + [Arvot näkyvät kasvua PercentThrottlingError]
    + [Arvot näkyvät kasvua PercentTimeoutError]
    + [Arvot näkyvät kasvua PercentNetworkError]
    + [Asiakkaan lähetetään HTTP 403 (kielletty)-viestit]
    + [Asiakkaan lähetetään HTTP 404 (ei löydy)-viestit]
    + [Asiakkaan vastaanottaa viestejä 409 HTTP (ristiriita)]
    + [Arvot näyttää pienen PercentSuccess tai analytics lokimerkintöjä on toimintojen ClientOtherErrors tapahtuman tila]
    + [Kapasiteetin arvot näyttäminen odottamattomia kasvua tallennustilan kapasiteetin käyttö]
    + [On ilmennyt odottamaton käynnistyy, joissa on paljon liitetty näennäiskiintolevyjen näennäiskoneiden on]
    + [Ongelma ilmenee käyttämästä tallennustilan emulaattori kehitys- tai]
    + [Kohtaat ongelmia asentamisessa Azure SDK .NET varten]
    + [Sinulla tallennuspalvelu eri ongelma]
+ [Lisäykset]
    + [Lisäys 1: HTTP- ja HTTPS-liikenne siepata Fiddler avulla]
    + [Lisäys 2: Sieppaaminen verkkoliikennettä Wiresharkin avulla]
    + [Lisäys 3: Microsoft viestin analysoimisen avulla voit siepata verkkoliikennettä]
    + [Lisäys 4: Excelin avulla voit tarkastella arvot ja kirjaudu tiedot]
    + [Lisäys 5: Hakemuksen tiedot Visual Studio Team Services-palveluiden seurantaa]

## <a name="introduction"></a>Johdanto

Tässä oppaassa kerrotaan, miten ominaisuuksia Azure-tallennustilan Analytics, kuten kirjautumisesta Azure tallennustilan asiakas-kirjaston ja muita kolmannen osapuolen työkaluja määrittäminen ja vianmääritys vianmääritys Azuren tallennustilaan asiakaspuolen liittyvät ongelmat.

![][1]

*Kuva 1 valvonta-diagnostiikka- ja vianmääritys*

Tämän oppaan eivät voi lukea ensisijaisesti kehittäjät online-palveluista, jotka käyttävät Azure-tallennustilan Services ja IT-ammattilaisille hallinnoinnista tällaisten verkkopalveluihin. Tämän oppaan tavoitteet ovat seuraavat:

- Voit säilyttää kunto ja Azure-tallennustilan asiakkaiden suorituskyvyn.
- Jos haluat antaa sinulle tarvittavat prosesseja ja työkaluja, joiden avulla voit päättää, jos seurantakohdetta tai ongelmaa sovelluksen liittyvät Azure-tallennustilan.
- Jos haluat antaa sinulle suoritettavia ohjeet Azuren tallennustilaan liittyvien ongelmien ratkaiseminen.

### <a name="how-this-guide-is-organized"></a>Kuinka tässä oppaassa on järjestetty

"[Tallennustilan palvelun seuranta]-osassa kerrotaan, miten seurannassa kuntoa ja suorituskyvyn Azuren tallennustilaan palvelujen käyttämällä Azure tallennustilan Analytics arvot (tallennustilan arvot).

"[Diagnosing tallennustilan ongelmat]"-osassa kerrotaan, miten käyttämällä Azure tallennustilan Analytics Logging (tallennustilan kirjaaminen) ongelmien vianmääritys. Tässä artikkelissa myös ottamisesta käyttöön asiakaspuolen kirjaaminen tilat asiakas-kirjastoihin ja käyttämällä esimerkiksi tallennustilan asiakas-kirjaston .NET tai Java Azure-SDK.

"[Lopusta loppuun-jäljitys]"-osassa kuvataan, siitä, miten voit yhdistää tietoja eri lokitiedostot ja arvot tiedot.

"[Vianmääritys ohjeita]"-osassa on yleisiä tallennustilan liittyvistä ongelmista voi tulla vianmäärityksen lisäohjeita.

"[Lisäykset]: n" sisältää tietoja käyttämällä muita työkaluja, kuten Wiresharkissa ja Netmon verkon paketin tietojen analysointiin HTTP/HTTPS-viestejä ja Microsoft viestin Analyzer for hajautettuna lokitiedot Fiddler analysoimista varten.


## <a name="monitoring-your-storage-service"></a>Tallennustilan palvelun seuranta

Jos olet tottunut käyttämään Windowsin suorituskyky seuranta, voit ajatella tallennustilan arvot siten, että Windowsin suorituskyvyn valvonta laskureita Azuren tallennustilaan-vastikkeen. Tallennustilan arvot-saat näkyviin kattavaa arvot (laskureita Windowsin suorituskyvyn valvonta termejä) kuten palvelun käytettävyys-palveluun pyyntöjen määrä tai palveluun onnistuneiden pyyntöjen prosenttiosuus (täysi luettelo käytettävissä olevat arvot näkyvät <a href="http://msdn.microsoft.com/library/azure/hh343264.aspx" target="_blank">Tallennustilan Analytics arvot Taulukkorakenteen</a> MSDN). Voit määrittää, haluatko tallennuspalvelu voit kerätä ja koostaa arvot tunnissa tai minuutin välein. Saat lisätietoja arvot ja seurata tallennustilan asiakkaiden MSDN-sivuston <a href="http://go.microsoft.com/fwlink/?LinkId=510865" target="_blank">ottaminen käyttöön tallennustilan arvot</a> .

Voit valita, mitä haluat näyttää Azure perinteinen portaalissa ja Määritä säännöt, jotka järjestelmänvalvojia, jotka ilmoittavat tunnin välein arvot sähköpostin aina, kun tunnin välein metrijärjestelmä tietyn kynnysarvo (Lisätietoja on artikkelissa sivun <a href="http://msdn.microsoft.com/library/azure/dn306638.aspx" target="_blank">miten: ilmoitusten vastaanottaminen ja hallinta ilmoitusten Azure säännöt</a>). Tallennustilan palvelun kerää käyttämällä parhaat työmäärään arvot, mutta jokaisen tallennustilan-toimintoa ei voi tallentaa.

Kuva 2 alla näkyy näytön missä voit katsella arvot, kuten käytettävyyden, pyyntöjen kokonaismäärä ja keskimäärin numerot tallennustilan tilin Azure perinteinen portaalissa. Ilmoitus sääntö on myös määritetty ilmoittamaan järjestelmänvalvoja, jos käytettävyys laskee tietyn tason alapuolelle. Näkemästä näitä tietoja tutkimuksen mahdollista alueissa on taulukon palvelun success on alle 100 % (Lisätietoja on kohdassa "[arvot näyttää pienen PercentSuccess tai analytics lokimerkintöjä on toimintoja, joiden tapahtuman tila on ClientOtherErrors]").

![][2]

*Kuva 2 tarkasteleminen tallennustilan arvot perinteinen Azure-portaalissa*

Olisi jatkuvasti seuraa Azure sovellustesi, että ne ovat kunnossa ja menestyneet oikein mukaan:

- Jotkin perusaikataulun mittaukset sovellusta, joiden avulla voit verrata nykyiset tiedot ja Määritä Azure tallennustilan ja sovelluksen toimintaa merkittävät muutokset. Perusaikataulun arvot arvoja, monissa tapauksissa tulee sovelluksen tietyn ja muodostaa ne suorituskykyä voi testata sovelluksen aikana.
- Minuutit arvot tallentamisesta ja käytöstä ne seurannassa aktiivisesti odottamattomia virheitä ja poikkeamia, kuten virhe piikkarit laskee tai pyydä korvaukset.
- Tunnin välein arvot tallentamisesta ja käytöstä ne seurannassa keskiarvot kuten keskimääräinen virhe määrät ja pyydät korvaukset.
- Mahdollisia ongelmia työkaluilla, kuten edellä myöhemmin "[Diagnosing tallennustilan ongelmien]."-osassa diagnostiikka tutkiminen

Alla olevassa kuvassa 3 kaavioiden osoittavat, miten keskiarvon, joka toteutuu tunnin välein arvot piilottaa piikkarit toimintaa. Tunnin välein arvot näkyvät näyttämään tasaisen korvaus pyyntöihin-arvot paljastaa vaihteluista, jotka ovat todella meneillään minuutin aikana.

![][3]

Loput tässä osassa kuvataan, mitä pitäisi seurata arvot ja miksi.

### <a name="monitoring-service-health"></a>Palvelun kunnon valvonta

[Azure perinteinen Portal](https://manage.windowsazure.com) avulla voit tarkastella kunto tallennuspalvelu (ja muiden Azure) Azure alueilla eri puolilla maailmaa. Näin näet välittömästi, jos ohjausobjektin ulkopuolella ongelma vaikuttavia tallennuspalvelu käytettävään sovelluksen alueen.

Perinteinen Azure-portaalissa voit antaa myös tapaukset, jotka vaikuttavat eri Azure palveluiden ilmoitukset.
Huomautus: Nämä tiedot oli aiemmin saatavana sekä historiatietoja osoitteessa <a href="http://status.azure.com" target="_blank">http://status.azure.com</a>Azure Service-koontinäytössä.

Kun Azure perinteinen portaalin kerää kunto tiedot sisällä Azure palvelinkeskusten (inside out seuranta), voit myös harkita antamista ulkopuolella-toimintatavan synteettisiä tapahtumia, jotka käyttävät säännöllisesti monista eri paikoista Azure isännöidään web-sovelluksen luomiseen. <a href="http://www.keynote.com/solutions/monitoring/web-monitoring" target="_blank">Esityksen</a>, <a href="https://www.gomeznetworks.com/?g=1" target="_blank">Gomez</a>ja sovelluksen havainnollistamisen Visual Studio Team Services-palveluiden tukemia palveluita on esimerkkejä ulkopuolella-tämän menetelmän. Saat lisätietoja Visual Studio Team Services-sovelluksen tiedot lisäyksen "[Lisäys 5: seurantaa sovelluksen tietoa Visual Studio Team Services]."

### <a name="monitoring-capacity"></a>Seurannan kapasiteetti

Tallennustilan arvot tallentaa vain kapasiteetin mittaukset blob-palvelu, koska BLOB tili yleensä tallennettuja tietoja suurimman osuus (kirjoittaminen hetkellä ei ole mahdollista tallennustilan arvot avulla voit valvoa taulukoita ja olevien kapasiteetti). Löydät tiedoista **$MetricsCapacityBlob** taulukossa, jos olet ottanut Blob-palvelun seuranta. Tallennustilan arvot tallentaa tiedot kerran päivässä ja **RowKey** arvon voit selvittää, onko rivi sisältää kohteen, jotka liittyvät käyttäjän (arvon **tiedot**) tai analytics-tiedot (arvo **analytics**). Tallennetun kunkin kohteen sisältää tietoja tallennustilan käytetään (**kapasiteetin** mitattuna tavua) ja määrä säilöjen (**ContainerCount**) ja BLOB-objektit (**ObjectCount**)-tallennustilan tilin käytössä. Saat lisätietoja **$MetricsCapacityBlob** taulukkoon kapasiteetin arvot MSDN- <a href="http://msdn.microsoft.com/library/azure/hh343264.aspx" target="_blank">Tallennustilan Analytics arvot Taulukkorakenteen</a> .

> [AZURE.NOTE] Valvoo, että on lähellä kapasiteetin rajoituksia tallennustilan tilin aikainen varoitus arvoja. Perinteinen Azure-portaalissa tallennustilan tilissäsi **valvonta** -sivulla voit lisätä ilmoittaa, jos kooste tallennustilan käyttö suurempi tai pienempi raja-arvot, jotka voit määrittää ilmoitusten sääntöjä.

Lisätietoja arvioidaan eri kuten BLOB storage objektien kokoa on blogimerkinnässä <a href="http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx" target="_blank">Tietoja Azure tallennustilan Laskutus – kaistanleveyden, tapahtumia, ja kapasiteetti</a>.

### <a name="monitoring-availability"></a>Käytettävyys seuranta

Olisi seurata liittyviä palveluja käytettävyyttä tallennustilan tilisi seuraamalla tunti ja minuutti arvot taulukoiden **käytettävyys** -sarakkeen arvo – **$MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. **Käytettävyys** -sarake sisältää prosenttiarvon, joka ilmaisee palvelun tai vastaavan rivin ( **RowKey** näyttää, onko rivi sisältää arvot palvelun kokonaisuutena tai tietyn API-toiminnon) API-toiminnon käytettävyyttä.

Mikä tahansa arvo on pienempi kuin 100 %: n ilmaisee, että jotkin tallennustilan pyynnöt epäonnistuvat. Näet, miksi ne epäonnistuvat tarkastelemalla arvot tiedot, jotka pyynnöt käyttämällä eri virhekoodi **ServerTimeoutError**kuten numeroiden näyttäminen muut sarakkeet. On todennäköisesti **käytettävyys** alemmaksi tilapäisesti 100 %: n syistä, kuten lyhytkestoisia palvelimen aikakatkaisu samalla, kun palvelua siirtyy osioiden paremmin kuormituksen pyyntö; Yritä logiikan asiakassovellus käsittelee katkonainen edellytyksistä. Sivun <a href="http://msdn.microsoft.com/library/azure/hh343260.aspx" target="_blank"></a> näyttää tapahtuman, joka sisältää **käytettävyys** laskennan tallennustilan arvot.

Perinteinen Azure-portaalissa tallennustilan tilissäsi **valvonta** -sivulla voit lisätä ilmoittamaan, jos palvelun **käytettävyys** raja-arvon, voit määrittää ilmoitusten sääntöjä.

Tämän oppaan "[vianmääritys ohjeita]"-osassa käsitellään joitakin yleisiä tallennustilan-palveluongelmat käyttöön liittyviä.

### <a name="monitoring-performance"></a>Suorituskyvyn valvominen

Voit valvoa suorituskykyä liittyviä palveluja, tunti ja minuutti arvot taulukoista seuraavat arvot.

- **AverageE2ELatency** ja **AverageServerLatency** arvot näyttää keskimääräinen aika tallennuspalvelu tai Käsittele pyynnöt kestää API toiminnon tyyppi. **AverageE2ELatency** mittaa lopusta loppuun-viive, joka sisältää pyyntö ja lähetä vastaus lisäksi pyynnön kesto Kesto (tämän vuoksi myös verkon latenssin kun pyynnön saavuttaa tallennuspalvelu); **AverageServerLatency** arvojoukon käsittelyn kesto ja ulkopuolelle sen vuoksi kaikki verkon latenssin liittyvät yhteydessä asiakkaan kanssa. Kohdassa "[arvot Näytä suuri AverageE2ELatency ja pienen AverageServerLatency]" tämän oppaan keskusteluun syy voi olla merkitystä, nämä kaksi arvoa.
- **TotalIngress** ja **TotalEgress** -sarakkeen arvojen näyttäminen tietojen kokonaismäärä tavuina saapua ja mennä ulos tallennustilan-palvelussa tai API toiminnon tietyntyyppinen kautta.
- **TotalRequests** -sarakkeen arvojen näyttäminen, joka vastaanottaa tallennuspalvelu API-toiminnon pyyntöjen kokonaismäärä. **TotalRequests** on pyyntöjen, joka tallennuspalvelu vastaanottaa kokonaismäärä.

Yleensä sinulla valvoo muutokset johonkin näistä arvoista kuin ilmaisin, joka edellyttää tutkimuksen ongelma jatkuu.

Perinteinen Azure-portaalissa tallennustilan tilissäsi **valvonta** -sivulla voit lisätä ilmoittaa, jos jokin suorituskyvyn mittarit tämän palvelun alemmaksi tai suurempi kuin kynnysarvo, voit määrittää ilmoitusten säännöt.

Tämän oppaan "[vianmääritys ohjeita]"-osassa käsitellään joitakin Yleiset suorituskykyyn liittyvät tallennustilan-palveluongelmat.


## <a name="diagnosing-storage-issues"></a>Ohjelmistossa tallennustilan ongelmat

Useilla tavoilla, voit ehkä tullut tietoinen ongelman sovelluksessa, näihin kuuluvat:

- Pää-virhe, joka aiheuttaa sovelluksen kaatumaan tai toimimasta.
- Merkittäviä muutoksia perusaikataulun arvot arvot ovat seuranta "[tallennustilan palvelun seurantaa]." edellisessä kohdassa kuvatulla tavalla
- Raportit-sovelluksesi käyttäjät jonkin tietyn toiminnon ei täytä odotetulla tavalla tai jotkin ominaisuudet eivät toimi.
- Sovelluksen virheiden, jotka näkyvät lokitiedostojen tai jonkin muun ilmoituksen menetelmän mukaan.

Yleensä seurantakohteita Azure tallennustilan palvelut kuuluvat yhteen neljään luokkaan:

- Sovellus on suorituskykyongelma ilmoittaa käyttäjille tai suorituskyvyn mittarit muutokset näkyviin.
- Tällä yhteen tai useampaan Azure Storage-infrastruktuurin ongelma.
- Sovelluksen olettaa virheen ilmoittaa käyttäjille tai kasvua seurata virhe Laske-arvot näkyviin.
- Aikana kehittäminen ja testaa käytössäsi on ehkä tallentaa paikallisesti; emulaattorin Voit kohdata seikkoja, jotka liittyvät erityisesti tallennustilan emulaattori käyttö.

Seuraavissa osissa, noudata ohjeita ja kunkin neljä luokan ongelmien vianmääritys. Tämän oppaan "[vianmääritys ohjeita]"-osassa on joitakin yleisiä ongelmia, voit kohdata tarkemmin.

### <a name="service-health-issues"></a>Palvelun ongelmia

Palvelun ongelmia ovat yleensä ohjausobjektin ulkopuolella. Perinteinen Azure-portaalissa on tietoja meneillään oleviin ongelmista tallennustilan palveluja Azure-palvelujen kanssa. Jos olet valinnut lukuoikeudet Geo tarpeettomat tallennuksessa tallennustilan tiliä luodessasi, jos käytössä ei ole käytettävissä ensisijainen sijainti tiedoissa sovelluksesi voi vaihtaa sitten väliaikaisesti toissijainen sijainnissa vain luku-kopiona. Voit tehdä tämän sovelluksen on voi vaihtaa ohjatulla ensisijainen ja toissijainen tallennussijaintia ja voivat käyttää rajoitetun toiminnan tilassa vain luku-tiedoilla. Azure-tallennustilan asiakas-kirjastojen avulla voit määrittää käytännön, jotka voivat lukea toissijainen tallennusväline siltä varalta, että luku ensisijainen säilöstä epäonnistuu uudelleen. Sovellus on myös huomioon, että toissijainen sijainnin tiedot vastaa myöhemmin. Lisätietoja blogimerkinnässä <a href="http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/04/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx" target="_blank">Azure Redundancy tallennusasetukset ja lukuoikeudet Geo tarpeettomat-tallennustilan</a>.

### <a name="performance-issues"></a>Suorituskykyongelmia

Sovelluksen suorituskyky voi olla Subjektiivinen erityisesti käyttäjän näkökulmasta. Tämän vuoksi on tärkeää on perusaikataulun arvot voi auttaa sinua huomaamaan, jos sinun on ehkä määritettävä suorituskykyyn liittyvä ongelma. Monet eri tekijät voivat vaikuttaa suorituskykyyn Azure tallennuspalvelu asiakkaan sovelluksen näkökulmasta. Seikoista saattaa toimia tallennuspalvelu, asiakkaan tai verkkoinfrastruktuuria; Tämän vuoksi on tärkeää strategia yksilöimiseen suorituskykyyn liittyvä ongelma alkuperän.

Kun olet määrittänyt suorituskykyyn liittyvä ongelma arvot-syy todennäköisesti sijainti, sitten voit lokitiedostojen löydät yksityiskohtaiset tiedot, voit määrittää ja ratkaista ongelma tarkemmin.

Kohta "[vianmääritys ohjeita]" tämän oppaan sivustossa on lisätietoja joitakin Yleiset suorituskykyyn liittyvät ongelmat, voit kohdata.

### <a name="diagnosing-errors"></a>Ohjelmistossa virheet

Sovelluksesi käyttäjät voi lähettää ilmoituksen asiakassovellus ilmoittaa virheistä. Tallennustilan arvot tallentaa myös eri virhekoodi tyypit tallennustilan Services-palveluista, kuten **NetworkError**, **ClientTimeoutError**tai **AuthorizationError**määrät. Kun tallennustilan arvot-vain tietueiden määrät eri virhekoodi tyypit, voit hankkia lisätietoja yksittäisten pyyntöjen tarkastamalla palvelinpuolen, asiakkaan ja verkon lokit. Yleensä tallennustilan palvelun palauttama HTTP-tilakoodin avulla selvitys siitä, miksi pyyntö epäonnistui.

> [AZURE.NOTE] Muista, että olisi odottaa näkeväni katkonainen virheitä: esimerkiksi virheiden vuoksi lyhytkestoisia verkon ehtoja tai sovellusvirheet.

MSDN-sivuston on seuraavissa resursseissa on hyötyä ymmärtämään tallennustilan liittyvät tila- ja virhetietoja koodit.

- <a href="http://msdn.microsoft.com/library/azure/dd179357.aspx" target="_blank">REST API-virhe koodeja</a>
- <a href="http://msdn.microsoft.com/library/azure/dd179439.aspx" target="_blank">BLOB-palvelun virhekoodit</a>
- <a href="http://msdn.microsoft.com/library/azure/dd179446.aspx" target="_blank">Jonon palvelun virhekoodit</a>
- <a href="http://msdn.microsoft.com/library/azure/dd179438.aspx" target="_blank">Taulukon palvelun virhekoodit</a>

### <a name="storage-emulator-issues"></a>Tallennustilan emulaattorin ongelmat

Azure-SDK sisältää tallennustilan-emulaattorin, voit suorittaa kehittäminen työasemassa. Tämä emulaattorin useimmat Azure liittyviä palveluja toiminnan varmistaminen ja on hyötyä aikana kehittäminen ja testaa, voit suorittaa ilman, että Azure tilaus ja Azure-tallennustilan tilin Azure tallennustilan palveluita käyttävät sovellukset.

Tämän oppaan "[vianmääritys ohjeita]"-osassa käsitellään joitakin yleisiä ongelmia havaittu käyttävän tallennustilan emulaattori.

### <a name="storage-logging-tools"></a>Tallennustilan kirjaaminen Työkalut

Tallennustilan kirjaaminen on palvelinpuolen kirjaaminen tallennustilan pyyntöjen Azure-tallennustilan tilin. Tietoja ottaminen käyttöön ja palvelinpuolen kirjaaminen lokitiedot, katso lisätietoja <a href="http://go.microsoft.com/fwlink/?LinkId=510867" target="_blank">käyttäminen: palvelinpuolen kirjaamisen</a> MSDN-sivuston.

Tallennustilan asiakkaan kirjasto .NET avulla voit kerätä asiakkaan lokitiedot, joka liittyy maksettavan korvauksen sovelluksen tallennustilan toimintoja. Tietoja ottaminen käyttöön ja asiakaspuolen kirjaaminen lokitiedot, katso lisätietoja <a href="http://go.microsoft.com/fwlink/?LinkId=510868" target="_blank">asiakaspuolen kirjaaminen tallennustilan asiakas-kirjaston käytön</a> MSDN-sivuston.

> [AZURE.NOTE] Joissakin tapauksissa (kuten SAS todennus epäonnistuu) käyttäjä saa ilmoittaa virheestä, jonka voit tarkistaa pyyntö ei ole tietoja palvelinpuolen tallennustilan lokit. Voit tutkia, jos ongelman syy on asiakkaan tallennustilan asiakkaan kirjaston kirjaaminen-ominaisuuksien käyttäminen tai verkon Valvontatyökalut avulla voit tutkia verkkoon.

### <a name="using-network-logging-tools"></a>Kirjaaminen työkaluilla

Voit siepata liikenne on tarkkoja tietoja varmaankaan asiakkaan ja palvelimen tiedot ja pohjana Verkkoehdot asiakkaan ja palvelimen välillä. Hyödyllisiä verkkotyökalut kirjaaminen ovat seuraavat:

- Fiddler (<a href="http://www.telerik.com/fiddler" target="_blank">http://www.telerik.com/fiddler</a>) on ilmainen sivuston virheenkorjaus välityspalvelin, jonka avulla voit tutkia ylä- ja paketin tiedoista HTTP ja HTTPS pyynnön ja vastauksen viestejä. Lisätietoja on artikkelissa "[Lisäys 1: Fiddler avulla voit siepata HTTP- ja HTTPS-liikenne]".
- Microsoft verkon näyttö (Netmonissa) (<a href="http://www.microsoft.com/download/details.aspx?id=4865" target="_blank">http://www.microsoft.com/download/details.aspx?id=4865</a>) että Wiresharkissa (<a href="http://www.wireshark.org/" target="_blank">http://www.wireshark.org/</a>) ovat maksuttomia verkon protokollan analyzers, joiden avulla voit tarkastella yksityiskohtaisia paketin tietoja verkkoprotokollien monien. Saat lisätietoja Wiresharkin "[Lisäys 2: käyttämällä Wiresharkin sieppaaminen verkkoliikennettä]".
- Microsoft viestin Analyzer-työkalua, joka ohittaa Netmon ja joka lisäksi tallentaa paketin verkkotietojen avulla voit tarkastella ja analysoida oteta talteen, muista työkaluista lokitiedot Microsoftin. Lisätietoja on artikkelissa "[lisäys 3: käyttämällä Microsoft viestin Analyzer kannattaa tallentaa verkkoliikennettä]".
- Jos haluat suorittaa basic Yhteystesti tarkistamaan, että asiakaskoneen muodostaa yhteyden Azure tallennuspalvelu verkon kautta, et voi tehdä tämän asiakkaan Vakio **ping** -työkalun avulla. Voit kuitenkin Tarkista yhteys **tcping** -työkalun avulla. **Tcping** on ladattavissa <a href="http://www.elifulkerson.com/projects/tcping.php" target="_blank">http://www.elifulkerson.com/projects/tcping.php</a>.

Monissa tapauksissa lokitiedot tallennustilan kirjaamisesta ja tallennustilaa asiakkaan kirjaston riittää ongelman vianmäärityksen, mutta joissakin tilanteissa, voit joutua tarkempia tietoja, jotka nämä verkon kirjaaminen-työkalujen avulla. Esimerkiksi Fiddler HTTP- ja HTTPS viestien lukemista varten avulla voit tarkastella ylä- ja paketti ja tallennustilaa-palveluista, jotka avulla voit tarkistaa, kuinka asiakassovellus uudelleenyrityksiä tallennustilan toiminnot sieltä lähetetyt tiedot. Protokollan analyzers, kuten Wiresharkissa toimivat paketin tasolla, joten voit tarkastella TCP-tietoja, jotka antaa sinulle perille ja yhteysongelmien vianmääritys. Viestin Analyzer toimii HTTP- ja TCP kerrokset-palvelussa.

## <a name="end-to-end-tracing"></a>Lopusta loppuun-jäljitys

Lopusta loppuun-jäljityksen lokitiedostojen erilaisilla on hyötyä tekniikka tutkia mahdolliset ongelmat. Voit käyttää Pvm. / klo-tietoja arvot tiedoista osoittimena mistä aloittaisi tutkimalla lokitiedostojen yksityiskohtaiset tiedot, joiden avulla voit tehdä ongelman vianmäärityksen.

### <a name="correlating-log-data"></a>Korreloivaa lokitiedot

Asiakassovelluksissa lokit tarkastellessasi verkon jäljittää ja tallennustilaa palvelinpuolen kirjaaminen on ehdottoman tärkeää voi yhdistää pyytää eri lokitiedostojen yli. Lokitiedostojen sisältävät useita eri kenttiä, jotka ovat hyödyllisiä korrelaatio tunnisteina. Pyyntö Asiakastunnus on eniten hyötyä kentän avulla voit yhdistää eri lokien merkinnät. Mutta joskus voi olla hyötyä pyynnön tunnus tai aikaleimat. Seuraavissa osissa on lisätietoja näistä asetuksista.

### <a name="client-request-id"></a>Pyyntö Ostajantunnus

Tallennustilan asiakkaan kirjaston luo automaattisesti yksilöllisen asiakkaan-pyynnön tunnus aina.

- Asiakkaan lokitiedoston, joka luo tallennustilan asiakas-kirjasto pyyntö OstajanTunnus näkyy jokaisen lokitapahtuman pyyntöä koskevan **Pyynnön Asiakastunnus** -kentässä.
- Verkon seurantatiedoista esimerkiksi jokin Fiddler: llä pyyntö Asiakastunnus on näkyvissä viesteillä **x-ms-asiakas-pyynnön-id** HTTP otsikon arvona.
- Palvelinpuolen tallennustilan kirjaaminen lokiin pyyntö OstajanTunnus näkyy asiakkaan pyynnön tunnussarake.

> [AZURE.NOTE]Se on mahdollista useita pyyntöjä jakamaan saman Asiakastunnus-pyynnön, koska asiakas, voit määrittää tämän arvon (vaikka tallennustilan asiakkaan kirjaston määrittää uuden arvon automaattisesti). Kyseessä uudelleenyritykset asiakaskoneesta kaikki yritykset jakaa saman Asiakastunnus-pyynnön. Asiakkaan lähettämät erän, kyseessä erän on yhden asiakasohjelman pyynnön tunnus.


### <a name="server-request-id"></a>Pyynnön tunnus

Tallennustilan palvelun luo automaattisesti palvelimen pyynnön tunnukset.

- Palvelinpuolen tallennustilan kirjaaminen lokiin palvelimen pyynnön tunnus tulee näkyviin **pyynnön tunnus otsikko** -sarakkeessa.
- Palvelimen pyynnön tunnus näkyy verkkoseurannan, kuten jokin Fiddler: llä, vastaus viestien **x-ms-pyynnön-id** HTTP otsikon arvona.
- Asiakkaan lokitiedoston, joka luo tallennustilan asiakas-kirjasto palvelimen pyynnön tunnus näkyy lokitapahtuman, joissa näkyy tietoja palvelimen vastaus **Toiminnon teksti** -sarakkeessa.

> [AZURE.NOTE] Tallennustilan palvelun aina määrittää yksilölliset palvelimen pyynnön tunnus jokaisen se saa pyynnön siten, että asiakaskoneesta uudelleen jokaisen yrityksen ja jokaisen erän toiminto on yksilöivä pyynnön-tunnus.

Jos tallennustilaa asiakkaan kirjaston ilmoittaa **StorageException** työasemaohjelmassa, **RequestInformation** -ominaisuus sisältää **RequestResult** -objekti, joka sisältää **ServiceRequestID** -ominaisuuden. Voit myös käyttää **RequestResult** objektin **OperationContext** esiintymästä.

Alla olevassa koodi-esimerkissä esitetään, kuinka voit määrittää mukautetun **ClientRequestId** arvon liittämällä tallennuspalvelu **OperationContext** objektin pyynnön. Se näyttää myös voit noutaa vastausviesti **ServerRequestId** -arvo.

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
    OperationContext oc = new OperationContext();
    oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

    try
    {
        CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
        ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
        var downloadToPath = string.Format("./{0}", blob.Name);
        using (var fs = File.OpenWrite(downloadToPath))
        {
            blob.DownloadToStream(fs, null, null, oc);
            Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
        }
    }
    catch (StorageException storageException)
    {
        Console.WriteLine("Storage exception {0} occurred", storageException.Message);
        // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
        foreach (var result in oc.RequestResults)
        {
                Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
        }
    }


### <a name="timestamps"></a>Aikaleimat

Voit käyttää myös aikaleimat, Etsi liittyvät lokimerkintöjä, mutta älä minkä tahansa kellon jakauman.vinous-funktio asiakkaan ja palvelimen, joka saattaa olla välillä. Olisi Etsi plus tai miinus 15 minuuttia, vastaavat palvelinpuolen tapahtumat asiakkaan aikaleiman perusteella. Muista BLOB-objektit, jotka sisältävät arvot blob-metatiedot ilmaisee arvot tallennetaan Blob-objektien; aika-alue Tämä on kätevä, jos sinulla on paljon arvot BLOB samaan minuutin tai tunti.

## <a name="troubleshooting-guidance"></a>Ohjeet vianmääritys

Tämän osan avulla voit vianmäärityksen ja vianmäärityksen joitakin yleisiä ongelmia sovelluksen kohdata, kun Azure tallennustilan Services-palvelujen avulla. Etsi ongelmaasi tiedot alla olevan luettelon avulla.

**Päätöspuun vianmääritys**

----------

Ongelmaa liittyvät suorituskyvyn tallennustilan palvelu?

- [Arvot high AverageE2ELatency ja pienen AverageServerLatency näyttäminen]
- [Arvot näyttää pienen AverageE2ELatency ja pienen AverageServerLatency mutta asiakkaan ilmenee odotusaika]
- [Näytä arvot high AverageServerLatency]
- [On ilmennyt odottamaton viipeitä viestin toimituksen jonon]

----------

Ongelmaa liittyvät jokin tallennustilan-palvelut käyttöön?

- [Arvot näkyvät kasvua PercentThrottlingError]
- [Arvot näkyvät kasvua PercentTimeoutError]
- [Arvot näkyvät kasvua PercentNetworkError]

----------

Asiakassovellus lähetetään HTTP-4XX (kuten 404) vastauksen tallennustilan-palvelusta?

- [Asiakkaan lähetetään HTTP 403 (kielletty)-viestit]
- [Asiakkaan lähetetään HTTP 404 (ei löydy)-viestit]
- [Asiakkaan vastaanottaa viestejä 409 HTTP (ristiriita)]

----------

[Arvot näyttää pienen PercentSuccess tai analytics lokimerkintöjä on toimintojen ClientOtherErrors tapahtuman tila]

----------

[Kapasiteetin arvot näyttäminen odottamattomia kasvua tallennustilan kapasiteetin käyttö]

----------

[On ilmennyt odottamaton käynnistyy, joissa on paljon liitetty näennäiskiintolevyjen näennäiskoneiden on]

----------

[Ongelma ilmenee käyttämästä tallennustilan emulaattori kehitys- tai]

----------

[Kohtaat ongelmia asentamisessa Azure SDK .NET varten]

----------

[Sinulla tallennuspalvelu eri ongelma]

----------

### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Arvot high AverageE2ELatency ja pienen AverageServerLatency näyttäminen

Azure perinteinen Portal seuranta-työkalun kuva-bEi tärkeä näyttää esimerkin missä **AverageE2ELatency** huomattavasti suurempi kuin **AverageServerLatency**.

![][4]

Huomaa, että tallennuspalvelu vain laskee onnistuneiden pyyntöjen lisätiedot **AverageE2ELatency** , toisin kuin **AverageServerLatency**, on aika asiakkaan Vie tiedot lähettämiseen ja vastaanottamiseen kuittaus tallennustilan-palvelusta. Tämän vuoksi **AverageE2ELatency** ja **AverageServerLatency** välinen ero voi olla vuoksi on hidas vastaavan asiakassovellus tai vuoksi ehdot verkossa.

> [AZURE.NOTE] Voit myös tarkastella **E2ELatency** ja **ServerLatency** yksittäisiä tallennustilan toiminnoille tallennustilan lokiin kirjaaminen-lokitiedot.

#### <a name="investigating-client-performance-issues"></a>Asiakkaan suorituskykyongelmia tutkiminen

Vastaa hitaasti asiakkaan mahdollisia syitä ovat on rajoitettu määrä käytettävissä olevat yhteydet tai viestiketjuissa siirtyminen. Voit ehkä ratkaista ongelman muokkaamalla asiakkaan koodin tehokkaampi (esimerkiksi käyttämällä asynkroninen puhelut tallennuspalvelu) tai käyttämällä suurempi Virtual Machine (sekä Lisää Sydämiä ja muistia).

Taulukon ja jono-palveluiden Nagle algoritmin voi johtua hyvin **AverageE2ELatency** verrattuna **AverageServerLatency**: Lisätietoja artikkelissa <a href="http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx" target="_blank">Nagle's algoritmin on helpossa muodossa ei olisi pieniä pyyntöjä</a> kirjaa Microsoft Azure tallennustilan tiimin blogia. Voit estää koodin Nagle-algoritmin **System.Net** nimitilan **ServicePointManager** -luokan avulla. Sinun pitäisi tehdä, ennen kuin teet puhelujen taulukkoon tai jonon services sovelluksesi, koska tämä ei vaikuta jo olevat yhteydet Avaa. Seuraavassa esimerkissä tulee Työntekijä-roolin **Application_Start** -menetelmää.

    var storageAccount = CloudStorageAccount.Parse(connStr);
    ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
    tableServicePoint.UseNagleAlgorithm = false;
    ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
    queueServicePoint.UseNagleAlgorithm = false;

Sinun on otettava asiakaspuolen lokeista nähdäksesi, kuinka monta pyytää asiakassovellus lähettäminen ja tarkista Yleiset .NET, jotka liittyvät suorituskyvyn pullonkaulat asiakkaalle, kuten suorittimen, .NET-muistista, verkon käyttö tai muistin (vianmäärityksen .NET-asiakassovellusten pohjana, katso <a href="http://msdn.microsoft.com/library/7fe0dd2y(v=vs.110).aspx" target="_blank">Virheenkorjaus jäljitys- ja profilointi</a> MSDN).

#### <a name="investigating-network-latency-issues"></a>Viive verkko tutkiminen

Verkon aiheutuvat lopusta loppuun odotusaika on yleensä lyhytkestoisia ehdot. Voit tutkia esimerkiksi hylätyt paketit sekä lyhytkestoisia ja pysyvät verkko työkaluilla, kuten Wiresharkissa tai Microsoft viestin Analyzer.

Katso lisätietoja käyttämisestä Wiresharkin verkko-ongelmien vianmääritystä "[Lisäys 2: Wiresharkin avulla voit siepata verkkoliikennettä]."

Katso lisätietoja käyttämisestä Microsoft viestin Analyzer verkko-ongelmien vianmääritystä "[lisäys 3: Microsoft Message analysoimisen avulla voit siepata verkkoliikennettä]."

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Arvot näyttää pienen AverageE2ELatency ja pienen AverageServerLatency mutta asiakkaan ilmenee odotusaika

Tässä skenaariossa Todennäköisin syy on tallennuspalvelu kurottavat tallennustilan pyyntöjä viive. Pitäisi tutkia, miksi asiakas-pyynnöt ovat ei jolloin kautta blob-palveluun.

Mahdollisia syitä viivästyy lähetetään pyyntöjä asiakkaan ovat on rajoitettu määrä käytettävissä olevat yhteydet tai viestiketjuissa siirtyminen. Kannattaa myös Tarkista, onko asiakkaan suorittaman useita uudelleenyritykset ja syyn, jos näin on. Voit tehdä tässä ohjelmallisesti **OperationContext** objektiin liittyvät pyynnön ja **ServerRequestId** arvon hakeminen näyttöä. Lisätietoja on artikkelissa koodin otosten osan "[pyynnön tunnus]."

Jos asiakas ei ole ongelmia, tarkista kuten pakettien verkon ongelmia. Voit tutkia verkko työkaluilla, kuten Wiresharkissa tai Microsoft viestin Analyzer.

Katso lisätietoja käyttämisestä Wiresharkin verkko-ongelmien vianmääritystä "[Lisäys 2: Wiresharkin avulla voit siepata verkkoliikennettä]."

Katso lisätietoja käyttämisestä Microsoft viestin Analyzer verkko-ongelmien vianmääritystä "[lisäys 3: Microsoft Message analysoimisen avulla voit siepata verkkoliikennettä]."

### <a name="metrics-show-high-AverageServerLatency"></a>Näytä arvot high AverageServerLatency

Suuri **AverageServerLatency** blob Lataa pyyntöjen, kyseessä ole toistuvat pyynnöt saman blob (tai joukko BLOB) tallennustilan lokiin kirjaaminen-lokit avulla. Saat blob ladata pyynnöt, kannattaa tutkia tietoja estä koon asiakas on käytössä (esimerkiksi estää alle 64 kilotavun kokoinen voi johtaa yleiskulut paitsi lukee ovat myös alle 64 kilotavua joukkojen), ja jos useiden asiakkaiden on lataaminen lohkot saman Blob-objektien rinnakkain. Kannattaa myös tarkistaa piikkarit pyynnöt, joka johtaa suurempi kuin määrä minuutissa mittaukset toinen skaalattavuus kohteet kohden: Katso myös "[arvot Näytä nostaa PercentTimeoutError]."

Jos näe hyvin **AverageServerLatency** blob, lataa pyynnöt, kun on toistuva pyynnöt samaan Blob-objektien tai joukko BLOB-objektit, valitse Ota huomioon välimuistiin nämä BLOB Azure välimuistin tai Azure sisällön toimituksen verkon (CDN) avulla. Lataa pyyntöjen voit parantaa siirtonopeuden käyttämällä suuremmaksi lohko. Kyselyjen taulukoihin on myös mahdollista toteuttamisesta asiakaspuolen välimuisti-asiakkaat, jotka suorittavat kyselyn toiminnoista ja mihin tiedot eivät muutu usein.

Suuret **AverageServerLatency** arvot voidaan myös päätyy yleensä huonosti suunniteltu taulukoita tai kyselyjä tarkistus tuloksen tai että noudattamalla Liitä/koskevan Anti kuvio. Lisätietoja on kohdassa "[arvot Näytä nostaa PercentThrottlingError]".

> [AZURE.NOTE] Voit etsiä täydellinen tarkistusluettelon suorituskyvyn tarkistusluettelon tähän: [Microsoft Azure tallennustilan suorituskyky ja skaalattavuus tarkistusluettelon](storage-performance-checklist.md).

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>On ilmennyt odottamaton viipeitä viestin toimituksen jonon

Jos kohtaat viive sovelluksen Lisää jonon viestin ja ajankohdan käytettävyys jonossa lukea, sinun olisi otettava määrittää ongelman tekemällä seuraavat toimet:

- Tarkista sovelluksen onnistuneesti Lisää viestit jonossa. Tarkista, että sovellus on ei yritetään **AddMessage** menetelmä useita kertoja ennen onnistumisen. Tallennustilan asiakkaan kirjaston lokissa näkyvät kaikki toistuvien uudelleenyritykset tallennustilan-toimintoa.
- Tarkista ei ole kellon jakauman.vinous välillä työntekijän rooli, joka lisää viestin jonossa ja työntekijän rooli, joka lukee sanoman, joka helpottaa jonossa näkyvät ikään käsittely ei viiveen.
- Tarkista Työntekijä rooli, joka lukee jonossa viestit eivät läpäise. Jos jonon asiakkaan kutsuu **GetMessage** menetelmää, mutta lakkaa vastaamasta kuittausta kanssa, viesti säilyy näkymätön jonossa, kunnes **invisibilityTimeout** kulunut. Tässä vaiheessa viesti on käytettävissä käsittely uudelleen.
- Tarkista, jos jonon pituuden kasvaa ajan kuluessa. Näin voi käydä, jos sinulla ei ole riittäviä työntekijöiden käsittelemiseen kaikki viestit, jotka muut työntekijät ovat markkinoille jonossa. Kannattaa myös tarkistaa nähdäksesi, jos Poista pyyntöjä epäonnistumista ja viestit, jotka saattavat tarkoittaa dequeue-Laske toistuvat arvot epäonnistui yrittää poistaa viestin.
- Tarkista kaikki jonon toiminnot, jotka on suurempi kuin odotettu **E2ELatency** ja **ServerLatency** arvot pitkän ajanjakson ajan tavallista tallennustilan lokiin kirjaaminen-lokit.


### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Arvot näkyvät kasvua PercentThrottlingError

Rajoittava virheitä ilmetä, kun ylität tallennustilan palvelun skaalattavuus kohteet. Tallennustilan palvelun toimii Näin voit varmistaa, että ei ole yhteen asiakkaaseen tai vuokraajan voi käyttää palvelua muiden kustannuksella. Katso lisätietoja, lisätietoja tallennustilan tilien skaalattavuus kohteet ja suorituskyvyn kohteet osioiden tallennustilan tilien sisältämien <a href="http://msdn.microsoft.com/library/azure/dn249410.aspx" target="_blank">Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet</a> .

**PercentThrottlingError** metrijärjestelmä näyttää kasvua, epäonnistuvat rajoittava virheen pyynnöt prosentteina, jos haluat tutkia jokin kuvauksista:

- [PercentThrottlingError lyhytkestoisia suureneminen]
- [Pysyvä kasvu prosentteina PercentThrottlingError virhe]

Nostaa **PercentThrottlingError** tapahtuu usein samaan aikaan kuin tallennustilan määrä kasvaa tai kun alun perin lataaminen testaaminen sovelluksen. Tämä voi myös luettelon itse "503 palvelimen varattu" tai "500 toiminnon aikakatkaisu" HTTP tilasanomat tallennustilan toimintojen-asiakassovelluksessa.

#### <a name="transient-increase-in-PercentThrottlingError"></a>PercentThrottlingError lyhytkestoisia suureneminen

Jos näe piikkarit **PercentThrottlingError** arvo, jotka vastaavat hyvin tehtävän Jaksojen sovelluksen, olisi Toteuta eksponentiaalisen (ei lineaarinen) Edellinen käytöstä uudelleenyritykset strategia asiakkaan: Tämä osioon heti kuormituksen vähentämiseksi ja liikenne piikkarit pehmentää sovelluksen avulla. Lisätietoja tallennustilan asiakas-kirjaston käytön uudelleen käytännöt ottamisesta käyttöön on MSDN-sivuston <a href="http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx" target="_blank">Microsoft.WindowsAzure.Storage.RetryPolicies Namespace</a> .

> [AZURE.NOTE] Voit myös nähdä piikkarit **PercentThrottlingError** arvo, joka ei ole sama kuin suuri tehtävän Jaksojen sovelluksen: Todennäköisin syy on siirtämällä osioita parantamiseksi kuormituksen tasaamisen tallennuspalvelu.

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Pysyvä kasvu prosentteina PercentThrottlingError virhe

Jos näe johdonmukaisesti suuri arvo **PercentThrottlingError** seuraavan pysyvä Suurenna tapahtuman tietomääristä tai alkuperäinen kuormituksen suoritettaessa testit sovelluksesi ja valitse haluat arvioida, kuinka sovellus käyttää tallennustilan osiot ja se on lähellä skaalattavuus kohteet tallennustilan tilin. Esimerkiksi jos näe rajoittimen virheiden jonossa (joka laskee yhteen osioksi), valitse Ota huomioon muita olevien avulla voit levittää tapahtumat useita osioiden välillä. Jos näe rajoittimen virheitä taulukkoa, haluat kannattaa käyttää eri osiointimalli levittämällä liiketapahtumiasi useita osioiden välillä käyttämällä monenlaisille osion avainarvot. Yksi tämän ongelman yleinen syy on prepend/liittäminen Anti kuvion kohtaa, johon osion avaimeksi Valitse päivämäärä ja valitse kaikki tiedot tiettynä päivänä kirjoitetaan yksi osio: kuormitus, tämä saattaa johtaa kirjoittaminen pullonkaula. Tulee joko toinen osioinnin rakenne kannattaa tai arvioida, onko Blob-objektien tallennustilaan käyttäminen saattaa olla parempi ratkaisu. Kannattaa myös Tarkista, onko rajoitin tapahtuu tuloksena tietoliikenteestä piikkarit ja tutkia tavalla tasoitus pyyntöjen kuvion.

Jos liiketapahtumiasi jakaa useita osioita, sinun on edelleen otettava huomioon skaalattavuus raja-arvot tallennustilan tilin. Esimerkiksi jos olet käyttänyt kymmenen olevien käsittelyn enintään 2 000 1 kt viestien sekunnissa, voi 20 000 viestien sekunnissa tallennustilan tilin Yleinen raja-palvelussa. Jos haluat käsitellä enemmän kuin 20 000 kohteiden sekunnissa, ota huomioon useita tallennustilan tilien kautta. Kannattaa myös oltava huomioon, että koko pyynnöt ja kohteiden on vaikutusta kun tallennuspalvelu throttles asiakkaat: Jos sinulla on suurempi pyyntöjä ja kohteiden, voit saattaa olla rajoitettu nopeammin.

Tehoton kyselyn rakennenäkymä voi johtua osumien taulukon osioiden skaalattavuus rajoitukset. Kysely, joka sisältää suodattimen, vain valitsee yhden prosentin kohteiden osion, mutta joka etsii kaikki osion kohteet on esimerkiksi käyttämään kunkin kohteen. Jokaisen kohteen lukeminen laskea kyseisen osion; tapahtumien määrä kohti sen vuoksi voit siirtyä helposti skaalattavuus kohteet.

> [AZURE.NOTE] Suorituskyvyn testaaminen olisi paljastaminen tehotonta kyselyn rakenteita-sovelluksessa.

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Arvot näkyvät kasvua PercentTimeoutError

Oman arvot näkyvät kasvua **PercentTimeoutError** johonkin liittyviä palveluja. Asiakkaan saa yhtä aikaa "500 toiminnon aikakatkaisu" HTTP tilasanomat määrää tallennustilan toimintoja.

> [AZURE.NOTE] Voit nähdä aikakatkaisuvirheitä tilapäisesti tallennustilan palveluna ladata saldojen pyynnöt siirtämällä osion uuteen palvelimeen.

**PercentTimeoutError** mittari kooste seuraavat arvot: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**ja **SASServerTimeoutError**.

Palvelimen aikakatkaisu virheet virheen palvelimessa. Asiakkaan aikakatkaisu johtua toiminnon palvelimessa on ylittänyt määrittämää asiakas; aikakatkaisu esimerkiksi asiakas, joka käyttää tallennustilan asiakas-kirjastossa, voit määrittää toiminnon aikakatkaisu **QueueRequestOptions** luokan **ServerTimeout** -ominaisuuden avulla.

Palvelimen aikakatkaisu osoittavat, joka edellyttää edelleen tutkimuksen tallennuspalvelu ongelma. Voit arvot, jos ovat pallolla palvelun skaalattavuus rajoitukset ja tunnistaa liikenteestä, joka saattaa aiheuttaa ongelman minkä tahansa piikkarit. Jos ongelma on katkonainen, se saattaa johtua kuormituksen tasaamisen tehtävän-palvelussa. Jos ongelma on pysyvä ja ei aiheuta pallolla skaalattavuus rajoituksia palvelun sovelluksen, kannattaa korottaa tuki on ongelma. Asiakkaan aikakatkaisu on päätät Jos aikakatkaisun on määritetty arvo asiakkaan ja joko muuta-asiakasohjelman määrittäminen aikakatkaisun tai tutkia, miten voit parantaa suorituskykyä tallennuspalvelu, toimintojen esimerkiksi optimointi taulukon kyselyjen tai viestit koon pienentäminen.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Arvot näkyvät kasvua PercentNetworkError

Oman arvot näkyvät kasvua **PercentNetworkError** johonkin liittyviä palveluja. **PercentNetworkError** mittari kooste seuraavat arvot: **NetworkError**, **AnonymousNetworkError**ja **SASNetworkError**. Nämä ilmetä, kun tallennustilan-palvelu tunnistaa verkkovirhe, kun asiakkaan tallennustilan pyynnön.

Yleisimmät tämän virheen syynä on asiakkaan yhteyden katkaiseminen aikakatkaisu vanhenee tallennustilan-palvelussa. Pitäisi tutkia koodi-asiakkaan ymmärtämään, miksi ja milloin asiakas katkaisee tallennustilan-palvelusta. Voit käyttää myös Wiresharkin, Microsoft viestin Analyzer tai Tcping tutkia verkon yhteysongelmat asiakasohjelmassa. Nämä työkalut on kuvattu [lisäykset].

### <a name="the-client-is-receiving-403-messages"></a>Asiakkaan lähetetään HTTP 403 (kielletty)-viestit

Jos asiakassovellus on yliheittävä HTTP 403 (kielletty) virheitä, todennäköisesti syy on, että asiakas käyttää vanhentunut jaettu Access allekirjoitus (SAS) kun se lähettää pyynnön storage (vaikka muut mahdollisia syitä voivat olla kellon jakauman.vinous, virheellinen näppäimet ja tyhjä otsikot). Jos syynä on vanhentunut SAS-avain merkintöjä palvelinpuolen tallennustilan kirjaaminen lokiin tietojen ei näytetä. Seuraavassa taulukossa on otoksen tallennustilan asiakkaan kirjasto, joka on kuvattu tämän ongelman ilmenemisen luoma asiakkaan lokista:

Lähde|Saa|Saa|Pyyntö OstajanTunnus|Toiminnon teksti
---|---|---|---|---
Microsoft.WindowsAzure.Storage|Tietoja|3|85d077ab-...|Toiminnon alkaen sijainti ensisijainen sijainti tilassa PrimaryOnly kohden.
Microsoft.WindowsAzure.Storage|Tietoja|3|85d077ab-...|Aloitetaan painikkeen pyyntö https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;sr = c&amp;palvelun käyttäjätietojen = mypolicy&amp;allekirjoitus OFnd4Rd7z01fIvh % 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % = 3D&amp;api-versio = 2014 02 – 14.
Microsoft.WindowsAzure.Storage|Tietoja|3|85d077ab-...|Odotetaan vastausta.
Microsoft.WindowsAzure.Storage|Varoitus|2|85d077ab-...|Kun Odotetaan vastausta poikkeus: etäpalvelin palautti virheen: (403)-käyttö estetty...
Microsoft.WindowsAzure.Storage|Tietoja|3|85d077ab-...|Vastauksesta. Tilakoodin = 403, pyydä ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, sisällön MD5 = ETag =.
Microsoft.WindowsAzure.Storage|Varoitus|2|85d077ab-...|Toiminnon aikana poikkeus: etäpalvelin palautti virheen: (403)-käyttö estetty...
Microsoft.WindowsAzure.Storage|Tietoja|3 |85d077ab-...|Tarkistetaan, jos toimintoa uudelleen. Laske uudelleen = 0, HTTP-tilakoodi = 403, poikkeus = etäpalvelin palautti virheen: (403)-käyttö estetty...
Microsoft.WindowsAzure.Storage|Tietoja|3|85d077ab-...|Seuraavaan kohteeseen on määritetty ensisijaiseksi, sijainnin perusteella.
Microsoft.WindowsAzure.Storage|Virhe|1|85d077ab-...|Yritä käytäntö ei ollut mahdollista yritä uudelleen. Epäonnistumista kanssa etäpalvelin palautti virheen: (403)-käyttö estetty.

Tässä skenaariossa pitäisi tutkia, miksi SAS tunnuksen vanheneminen, ennen kuin asiakas lähettää tunnuksen palvelimeen:

- Yleensä ei määritä alkamisaika, kun luot SAS asiakkaan käyttämään heti. Jos määritettynä on pieni kello luodaan Suojaussidokset nykyisen kellonajan ja tallennuspalvelu, se on mahdollista tallennuspalvelu vastaanottamaan SAS, joka ei ole vielä voimassa host erot.
- Älä aseta lyhyet voimassaolon ajan Suojaussidokset. Uudelleen pieni kello erot luodaan Suojaussidokset ja tallennustilaa palvelun host voi aiheuttaa SAS, vanhentumassa mahdollisia vanhempia kuin odotettavissa.
- Onko Suojaussidosten avaimen versio-parametrin (esimerkiksi **AV = 2012-02 – 12**) vastaa-versiota käytät tallennustilan asiakkaan kirjaston. Käytä aina uusimman version tallennustilan asiakas-kirjasto. Saat lisätietoja SAS suojaustunnuksen versiotietojen [Mikä on uutta Microsoft Azuren tallennustilaan](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/14/what-s-new-for-microsoft-azure-storage-at-teched-2014.aspx).
- 
- Jos yhteyttä tallennustilan pikanäppäimet (Valitse **Hallitse pikanäppäinten** millä tahansa sivulla tallennustilan tilisi Azure perinteinen portaalissa) uudelleen Tämä vahingoittaa kaikki olemassa olevat Suojaussidokset tunnusten. Tämä saattaa olla ongelma, jos SAS tunnusten pitkä voimassaolon ajan, välimuistin asiakassovellukset Luo.

Jos käytössäsi on tallennustilan asiakkaan kirjaston SAS tunnusten luomiseen on helppo luoda kelvollinen tunnus. Jos käytät tallennustilan REST-Ohjelmointirajapinnalla ja luomisesta SAS tunnusten käsin huolellisesti Lue aiheessa MSDN-sivuston <a href="http://msdn.microsoft.com/library/azure/ee395415.aspx" target="_blank">Delegoiminen käyttää ja jakaa Access-allekirjoitus</a> .

### <a name="the-client-is-receiving-404-messages"></a>Asiakkaan lähetetään HTTP 404 (ei löydy)-viestit
Jos asiakassovellus vastaanottaa HTTP 404-ei löydy-sanoman palvelimeen, tämä tarkoittaa sitä, että asiakas on yrittää käyttää (kuten kohteen, taulukko blob, säilön tai jonon) objektia ei ole tallennuspalvelu. On useita mahdollisia syitä, kuten:

- [Asiakas- tai toinen prosessi aiemmin poistettu objekti]
- [Jaettu Access allekirjoitus (SAS) todennus-ongelma]
- [Asiakkaan JavaScript-koodia ei ole oikeutta käyttää objekti]
- [Verkkovirhe]

#### <a name="client-previously-deleted-the-object"></a>Asiakas- tai toinen prosessi aiemmin poistettu objekti
Skenaariot, jossa asiakkaan yrittää lukea, päivittää tai poistaa tietoja tallennustilan-palvelussa on yleensä helppo tunnistaa palvelinpuolen lokeja edellisen toiminnon, joka poistaa kyseisen objektin tallennustilan-palvelusta. Kovin usein lokitiedot näkyy, että toinen käyttäjä tai prosessi poistettu objekti. Palvelinpuolen tallennustilan kirjaaminen lokiin-toiminnon tyyppi ja pyytää-objekti-avain sarakkeiden Näytä kun asiakkaan poistanut objektin.

Tilanteessa, jossa on jokin muu sähköpostiohjelma yrittää objektin lisääminen se ei ehkä heti ilmeisimmät miksi tuloksena on HTTP 404 (ei löydy)-vastauksen, koska asiakas luodaan uusi objekti. Kuitenkin jos asiakkaan luodaan blob, sen on oltava löydät blob-säilö, jos asiakkaan luodaan on oltava löydät jonon viestin ja asiakkaan lisätään rivin sen on oltava löydät taulukon.

Tallennustilan asiakkaan kirjastosta asiakkaan lokiin avulla voit saada tarkempia tietoja kun asiakas lähettää tietyn pyynnöt tallennustilan-palveluun.

Seuraavat asiakkaan lokin luoma tallennustilan asiakas-kirjasto on kuvattu ongelma, kun asiakas ei löydä säilö se luodaan Blob-objektien. Tämä loki sisältää tallennustilan seuraavia toimintoja:

Pyynnön tunnus|Toiminto
---|---
07b26a5d-...|**DeleteIfExists** menetelmä Poista blob-säilö. Huomaa, että tämä toiminto sisältää **pää** pyynnön, tarkista, onko säilö.
e2d06d78...|**CreateIfNotExists** menetelmä luo blob-säilö. Huomaa, että tämä toiminto sisältää **pää** -pyynnön, joka tarkistaa, onko säilö. **PÄÄTÄ** palauttaa 404 viestin, mutta jatkaa.
de8b1c3c-...|**UploadFromStream** menetelmä Blob-objektien luomiseen. **SIJOITA** pyynnön epäonnistuu 404 viestillä

Lokimerkintöjä:

Pyynnön tunnus |  Toiminnon teksti
---|---
07b26a5d-...|Aloitetaan painikkeen pyyntö https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
07b26a5d-...|StringToSign = HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 kesä 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-...|Odotetaan vastausta.
07b26a5d-... | Vastauksesta. Tilakoodin = 200, pyydä ID = eeead849-... Sisällön MD5 = ETag = &quot;0x8D14D2DC63D059B&quot;.
07b26a5d-... | Vastausten otsikot käsiteltiin onnistuu, eteneminen toiminto loppuun.
07b26a5d-... | Lataaminen vastauksen teksti.
07b26a5d-... | Toiminto on suoritettu.
07b26a5d-... | Aloitetaan painikkeen pyyntö https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
07b26a5d-... | StringToSign = DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 kesä 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-... | Odotetaan vastausta.
07b26a5d-... | Vastauksesta. Tilakoodin = 202, pyydä ID = 6ab2a4cf-..., sisällön MD5 = ETag =.
07b26a5d-... | Vastausten otsikot käsiteltiin onnistuu, eteneminen toiminto loppuun.
07b26a5d-... | Lataaminen vastauksen teksti.
07b26a5d-... | Toiminto on suoritettu.
e2d06d78-... | Aloitetaan https://domemaildist.blob.core.windows.net/azuremmblobcontainer asynkroninen pyyntö.</td>
e2d06d78-... | StringToSign = HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 03 kesä 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-...| Odotetaan vastausta.
de8b1c3c-... | Aloitetaan painikkeen pyyntö https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... |  StringToSign = HYLLYTETTY... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-BLOB-Type:BlockBlob.x-MS-Client-Request-ID:de8b1c3c-...x-MS-Date:tue 03 kesä 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... | Valmistellaan pyynnön tietojen kirjoittaminen.
e2d06d78-... | Kun Odotetaan vastausta poikkeus: etäpalvelin palautti virheen: (404) ei löydy...
e2d06d78-... | Vastauksesta. Tilakoodin = 404, pyydä ID = 353ae3bc-..., sisällön MD5 = ETag =.
e2d06d78-... | Vastausten otsikot käsiteltiin onnistuu, eteneminen toiminto loppuun.
e2d06d78-... | Lataaminen vastauksen teksti.
e2d06d78-... | Toiminto on suoritettu.
e2d06d78-... | Aloitetaan https://domemaildist.blob.core.windows.net/azuremmblobcontainer asynkroninen pyyntö.
e2d06d78-...|StringToSign = HYLLYTETTY... 0...x-MS-Client-Request-ID:e2d06d78-...x-MS-Date:tue 03 kesä 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-... | Odotetaan vastausta.
de8b1c3c-... | Kirjoittaminen pyynnön tiedot.
de8b1c3c-... | Odotetaan vastausta.
e2d06d78-... | Kun Odotetaan vastausta poikkeus: etäpalvelin palautti virheen: (409) ristiriita...
e2d06d78-... | Vastauksesta. Tilakoodin = 409, pyydä ID = c27da20e-..., sisällön MD5 = ETag =.
e2d06d78-... | Lataamisen virhe vastauksen tekstissä.
de8b1c3c-... | Kun Odotetaan vastausta poikkeus: etäpalvelin palautti virheen: (404) ei löydy...
de8b1c3c-... | Vastauksesta. Tilakoodin = 404, pyydä ID = 0eaeab3e-..., sisällön MD5 = ETag =.
de8b1c3c-...| Toiminnon aikana poikkeus: etäpalvelin palautti virheen: (404) ei löydy...
de8b1c3c-... | Yritä käytäntö ei ollut mahdollista yritä uudelleen. Epäonnistumista kanssa etäpalvelin palautti virheen: (404) ei löydy...
e2d06d78-... | Yritä käytäntö ei ollut mahdollista yritä uudelleen. Epäonnistumista kanssa etäpalvelin palautti virheen: (409) ristiriita...

Tässä esimerkissä lokin näkyy, että asiakas on välilehdet pyynnöt **CreateIfNotExists** -menetelmän (pyynnön tunnus e2d06d78...) **UploadFromStream** menetelmä pyyntöihin (de8b1c3c-...); Tämä johtuu, koska asiakassovellus on käynnistettäessä näistä tavoista asynkronisesti. Muokkaa asynkroninen koodin varmistaa, että se luo säilö ennen kuin yrität tietojen lataaminen kyseisen säilön blob-asiakassovelluksessa. Ihannetapauksessa sinun on luotava kaikki säilöt etukäteen.

#### <a name="SAS-authorization-issue"></a>Jaettu Access allekirjoitus (SAS) todennus-ongelma

Jos asiakassovellus yrittää käyttää Suojaussidosten avainta, joka ei ole tarvittavia käyttöoikeuksia toiminnon, tallennustilan-palvelu palauttaa HTTP 404 (ei löydy)-viesti asiakkaalle. Samanaikaisesti myös näkyy kuin nolla, **SASAuthorizationError** arvot.

Seuraavassa taulukossa on palvelinpuolen lokin esimerkkiviesti tallennustilan lokiin kirjaaminen-lokitiedostosta:

<table>
  <tr>
    <td>Pyydä aloitusaika</td>
    <td>2014-05-30T06:17:48.4473697Z</td>
  </tr>
  <tr>
    <td>Toiminto</td>
    <td>GetBlobProperties</td>
  </tr>
  <tr>
    <td>Pyynnön tila</td>
    <td>SASAuthorizationError</td>
  </tr>
  <tr>
    <td>HTTP-tilakoodi</td>
    <td>404</td>
  </tr>
  <tr>
    <td>Todennustyyppi</td>
    <td>Suojaussidokset</td>
  </tr>
  <tr>
    <td>Palvelutyyppi</td>
    <td>BLOB</td>
  </tr>
  <tr>
    <td>Pyyntö URL-osoite</td>
    <td>
    https://domemaildist.BLOB.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;amp; sr = c&amp;amp; palvelun käyttäjätietojen = mypolicy&amp;amp; allekirjoitus = XXXXX&amp;amp; api-version = 2014 02 – 14&amp;amp;</td>
  </tr>
  <tr>
    <td>Pyydä tunnuksen otsikkoa</td>
    <td>a1f348d5-8032-4912-93ef-b393e5252a3b</td>
  </tr>
  <tr>
    <td>Pyyntö Ostajantunnus</td>
    <td>2d064953-8436-4ee0-aa0c-65cb874f7929</td>
  </tr>
</table>

Pitäisi tutkia, miksi asiakassovellus yrittää suorittaa toiminnon, se ei ole myönnetty käyttöoikeudet.

#### <a name="JavaScript-code-does-not-have-permission"></a>Asiakkaan JavaScript-koodia ei ole oikeutta käyttää objekti

Jos käytät JavaScript-asiakasohjelmaa ja tallennustilan-palvelu palauttaa HTTP 404 viestejä, voit tarkistaa seuraavat JavaScript-virheitä selaimessa:

    SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
    SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.

> [AZURE.NOTE] Voit Internet Explorerin F12-Kehitystyökalut jäljittää vaihtavat selaimessa ja tallennustilaa palvelun asiakkaan JavaScript-ongelmien vianmäärityksessä viestejä.

Nämä virheet johtua siitä, että selain toteuttaa <a href="http://www.w3.org/Security/wiki/Same_Origin_Policy" target="_blank">saman origin käytännön</a> suojaus-rajoitus, joka estää verkkosivun kutsumista Ohjelmointirajapinnan toimialueesta toimialueen sivu tulee.

Voit kiertää ongelman JavaScript, voit määrittää toimintojen Origin resurssien jakaminen (CORS) asiakas käyttää tallennuspalvelu. Lisätietoja <a href="http://msdn.microsoft.com/library/azure/dn535601.aspx" target="_blank">usean Origin resurssien jakaminen (CORS) tuki Azure liittyviä palveluja</a> MSDN-sivuston.

Seuraava koodi malli näkyy blob-palvelun sallimaan JavaScript Contoso toimialueen käyttämään blob-Blob-objektien tallennustila-palvelun määrittäminen:

    CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
    // Set the service properties.
    ServiceProperties sp = client.GetServiceProperties();
    sp.DefaultServiceVersion = "2013-08-15";
    CorsRule cr = new CorsRule();
    cr.AllowedHeaders.Add("*");
    cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
    cr.AllowedOrigins.Add("http://www.contoso.com");
    cr.ExposedHeaders.Add("x-ms-*");
    cr.MaxAgeInSeconds = 5;
    sp.Cors.CorsRules.Clear();
    sp.Cors.CorsRules.Add(cr);
    client.SetServiceProperties(sp);

#### <a name="network-failure"></a>Verkkovirhe

Joissakin tapauksissa menettää verkkopakettien voi aiheuttaa HTTP 404 viestien palaaminen asiakkaan tallennuspalvelu. Esimerkiksi kun asiakassovellus poistaa kohteen taulukon-palvelusta näet palauttaa tallennustilan poikkeuksen raportoinnin asiakas-"HTTP 404 (ei löydy)"-taulukko-palvelun tila-sanoma. Kun tarkastelet taulukon tallennuspalvelu juuri, näet palvelun poistaa kohteen, kun pyydetty.

Poikkeuksen tiedot työasemaohjelmassa Sisällytä määrittämän taulukon palvelun pyynnön pyynnön-tunnus (7e84f12d...): tämän toiminnon avulla voit etsiä pyynnön tiedot tallennustilan palvelinpuolen lokeja hakemalla lokitiedosto **pyynnön-tunnus-otsikko** -sarakkeessa. Määritetty avulla voit myös huomaamaan, jos esimerkiksi virheet ilmetä ja hae sitten lokitiedostojen perusteella määritetty tallennettu tämä virhe. Lokista näkyy, että poisto epäonnistui "HTTP (404)-asiakasohjelman muu virhe"-tila-sanoma. Saman lokitapahtuman myös pyynnön tunnus luoma asiakkaan **asiakkaan pyynnön tuotetunnus -** sarakkeen (813ea74f...).

Palvelinpuolen lokiin myös toiseen hakusanaan saman **asiakkaan pyynnön tuotetunnus -** arvo (813ea74f...) onnistuneen poistotoiminnon saman kohteen ja saman asiakasohjelmassa. Tämä onnistuu poistotoiminto on tapahtunut välittömästi ennen epäonnistunutta poistaa pyynnön.

Tässä skenaariossa Todennäköisin syy on, että asiakas lähettää kohteen poistopyyntö taulukko-palvelun, joka onnistui, mutta ei mennyt kuittausta (esimerkiksi vuoksi väliaikaiset verkko-ongelma)-palvelimesta. Valitse asiakkaan automaattisesti uudelleen toiminnon (joko samalla **asiakkaan pyynnön tuotetunnus-**) ja tämän uudelleen epäonnistui, koska jo kohde on poistettu.

Jos tämä ongelma ilmenee usein, tutki miksi asiakkaan epäonnistuu kuittaussanomien vastaanottaminen taulukko-palvelusta. Jos ongelma on katkonainen, olisi etsiä "HTTP (404) ei löydy"-Virhe ja kirjaudu työasemaohjelmassa mutta salli asiakkaan Jatka.

### <a name="the-client-is-receiving-409-messages"></a>Asiakkaan vastaanottaa viestejä 409 HTTP (ristiriita)

Seuraavassa taulukossa on kaksi asiakastoiminnot palvelinpuolen lokista ote: **DeleteIfExists** heti sen perään **CreateIfNotExists** blob säilö nimeä. Huomaa, että kunkin asiakkaan toiminnon tulokset kaksi pyyntöjä lähetti palvelimeen, ensin **GetContainerProperties** pyynnön, tarkista, onko olemassa säilö, seuraa **DeleteContainer** tai **CreateContainer** pyynnön.

Aikaleima|Toiminto|Tulos|Säilön nimi|Pyyntö OstajanTunnus
---|---|---|---|---
05:10:13.7167225|GetContainerProperties|200|mmcont|c9f52c89-...
05:10:13.8167325|DeleteContainer|202|mmcont|c9f52c89-...
05:10:13.8987407|GetContainerProperties|404|mmcont|bc881924-...
05:10:14.2147723|CreateContainer|409|mmcont|bc881924-...

Poistaa ja luo nimeä blob-säilö heti asiakassovelluksessa koodia: **CreateIfNotExists** -menetelmän (asiakkaan pyynnön tunnus bc881924-...) ilmestyy epäonnistuu 409 HTTP (ristiriita)-virheen. Kun asiakkaan poistaa blob säilöjen, taulukoita tai on lyhyt ajanjakso nimen edessä olevien taas käytettävissä.

Asiakassovellus Käytä yksilöllinen säilön nimet aina, kun se luo uuden säilöjen Jos poistaa ja luo kuvion yhteistä kohdetta.

### <a name="metrics-show-low-percent-success"></a>Arvot näyttää pienen PercentSuccess tai analytics lokimerkintöjä on toimintojen ClientOtherErrors tapahtuman tila

**PercentSuccess** metrijärjestelmä sieppaa tehdyn toiminnoista, joita ei onnistunut niiden HTTP-tilakoodin perusteella. Toimintoja, joiden tilakoodin 2XX, voit laskea onnistuu, toimintoja, joiden tilakoodit 3XX, 4XX ja 5XX alueiden lasketaan epäonnistua ja pienentää **PercentSucess** kustannusarvo. Palvelinpuolen tallennustilan-lokitiedostojen näitä toimintoja tallennetaan **ClientOtherErrors**tapahtuman tilassa.

On tärkeää muistaa, että nämä toiminnot on suoritettu onnistuneesti ja näin ollen eivät vaikuta muita tietoja, kuten käytettävyys. Toimintojen suorittaminen onnistui, mutta, joka johtaa epäonnistua HTTP-tilakoodin esimerkiksi:
- **ResourceNotFound** (Ei löydy 404), kuten kuin GET-pyyntö Blob-objektien, jota ei ole.
- **ResouceAlreadyExists** (Ristiriita 409), esimerkiksi-kohtaa, johon resurssi on jo **CreateIfNotExist** -toimintoa.
- **ConditionNotMet** (Ei muokattu 304), esimerkiksi-ehdollinen toimintoa, esimerkiksi kun asiakas lähettää **ETag** -arvo ja HTTP- **Jos-ei mitään-vastine** -otsikko, joka pyytää kuvan vain, jos se on päivitetty edellisen toiminnon jälkeen.

Voit etsiä yleisiä REST API-virhekoodit, jotka liittyviä palveluja palauttavat sivulla <a href="http://msdn.microsoft.com/library/azure/dd179357.aspx" target="_blank">Yleiset REST API-virhekoodit</a>luettelo.

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Kapasiteetin arvot näyttäminen odottamattomia kasvua tallennustilan kapasiteetin käyttö


Jos näet äkillinen, kapasiteetin käyttö tallennustilan tilisi odottamattomia muutokset haluat tutkia syyt katsomalla ensimmäisen käytettävyys-arvot; esimerkiksi pyynnöt saattaa johtaa kasvua rahamäärän Blob-objektien tallennustilaan sovelluksen tietyn uudelleenjärjestäminen toiminnot voivat olla oletat voidaan vapauttaminen ylimääräistä tilaa ei ehkä toimi odotetulla tavalla (esimerkiksi silloin, kun käytetään vapauttamiseen SAS tunnusten on vanhentunut), kun käytössäsi on epäonnistunut Poista määrä kasvaa.

### <a name="you-are-experiencing-unexpected-reboots"></a>On ilmennyt odottamaton käynnistyy, Azuren näennäiskoneiden, joka on liitetty näennäiskiintolevyjen suuri määrä

Jos Azure virtuaalikoneen (AM) on liitetty näennäiskiintolevyjen, joka on saman tallennustilan tilin paljon, voi ylittää skaalattavuus kohteet ilmenneet epäonnistuu AM yksittäisiä tallennustilan tilin. Sinun on otettava tallennustilan tilin minuutit mittaukset (**TotalRequests**/**TotalIngress**/**TotalEgress**), jotka ylittävät skaalattavuus kohteet tallennustilan tilin piikkarit varten. Katso kohta "[arvot Näytä nostaa PercentThrottlingError]" apua selvittämisestä, onko rajoitusta on tapahtunut tallennustilan-tililläsi.

Yleensä kunkin yksittäisiä syötteen tai tulosteen toiminnon näennäiskiintolevyllä Virtual tietokoneesta vastaa pohjana olevan sivun Blob-objektien **Hae** tai **Sijoita sivu** toimenpiteet. Voit vuoksi paranna montako näennäiskiintolevyjen voit yksittäisen tallennustilaa tilillä perustuu sovelluksen toiminnot arvioitu IOPS ympäristössä. Ei ole suositeltavaa ottaa yli 40 levyjen yksittäisen tallennustilaa tilillä. Lisätietoja <a href="http://msdn.microsoft.com/library/azure/dn249410.aspx" target="_blank">Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet</a> nykyiseen skaalattavuus kohteita, tallennustilan tilien erityisesti korko yhteensä pyynnön ja käytät tallennustilan tilin tyypin yhteensä kaistanleveys.
Jos ylittävät skaalattavuus kohteet tallennustilan tilin, sijoita oman näennäiskiintolevyjen useita eri tallennustilan tilien vähentää aktiviteetti jokaiselle tilille erikseen.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Ongelma ilmenee käyttämästä tallennustilan emulaattori kehitys- tai

Tavallisesti käyttää tallennustilan emulaattori kehityksen aikana ja testaa Azure-tallennustilan tilin vaatimus välttämiseksi. Yleisiä ongelmia, jotka tapahtuvat käyttäessäsi tallennustilan emulaattori ovat seuraavat:

- ["X" toiminto ei toimi tallennustilan emulaattori]
- [Virhe "jokin HTTP-otsikot-arvo ei ole oikeassa muodossa" tallennustilan emulaattori käytettäessä]
- [Tallennustilan emulaattori suorittaminen edellyttää järjestelmänvalvojan oikeuksia]

#### <a name="feature-X-is-not-working"></a>"X" toiminto ei toimi tallennustilan emulaattori

Tallennustilan emulaattori ei tue kaikkia tiedoston palvelun Azure tallennustilan palveluihin ominaisuuksia. Katso lisätietoja, MSDN-sivuston <a href="http://msdn.microsoft.com/library/azure/gg433135.aspx" target="_blank">erot välillä tallennustilan emulointikoneen ja Azure liittyviä palveluja</a> .

Käytä näitä ominaisuuksia tallennustilan emulaattori tue Azure tallennuspalvelu pilveen.

#### <a name="error-HTTP-header-not-correct-format"></a>Virhe "jokin HTTP-otsikot-arvo ei ole oikeassa muodossa" tallennustilan emulaattori käytettäessä

Testaat sovelluksesi, jotka käyttävät vastaan paikallisen säilön emulaattori tallennustilan asiakas-kirjaston ja menetelmä kutsuu, kuten **CreateIfNotExists** eivät toimi ja näyttöön tulee virhesanoma "johonkin HTTP-otsikoiden arvo ei ole oikeassa muodossa." Tämä ilmaisee käytät tallennustilan emulaattorin versio ei tue tallennustilan asiakas-kirjastoon, käytössäsi on versio. Tallennustilan asiakkaan kirjaston Lisää otsikko **x-ms-versio** kaikki tekemänsä pyynnöt. Jos tallennustilaa emulaattori ei tunnista **x-ms-version** -otsikko-arvo, hylkää pyynnön.

Voit käyttää tallennustilan kirjaston asiakkaan lokitiedot **x-ms-version-otsikko** , se lähettää arvo. Näet myös **x-ms-versio otsikon** arvo, jos jäljitys pyynnöt asiakassovellus Fiddler avulla.

Näin tapahtuu yleensä, jos asennat ja käyttää tallennustilan asiakkaan kirjaston uusimman version tallennustilan emulaattori päivittämättä. On joko tallennustilan emulaattori uusimman version asentaminen tai käytä pilvitallennustilaa sijaan emulaattori kehittämisen ja testi.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Tallennustilan emulaattori suorittaminen edellyttää järjestelmänvalvojan oikeuksia

Sinua kehotetaan järjestelmänvalvojan tunnistetietoja, kun suoritat tallennustilan emulaattori. Tämä tapahtuu vain silloin, kun ovat valmistellaan tallennustilan emulaattori ensimmäistä kertaa. Kun olet alustaa tallennustilan emulaattori, järjestelmänvalvojan oikeudet suorittaa uudelleen ei tarvitse.

Katso lisätietoja, (Voit myös alustaa tallennustilan emulaattori Visual Studiossa, jotka edellyttävät lisäksi järjestelmänvalvojan oikeudet) MSDN-sivuston <a href="http://msdn.microsoft.com/library/azure/gg433132.aspx" target="_blank">valmistelu tallennustilan emulaattori komentorivivalitsimet-työkalun avulla</a> .

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Kohtaat ongelmia asentamisessa Azure SDK .NET varten

Kun yrität asentaa SDK, se epäonnistuu asennettaessa tallennustilan emulaattori paikallisessa tietokoneessa. Asennuksen lokin on jokin seuraavista ilmoituksista:

- CAQuietExec: Virhe: ei voi käyttää SQL-esiintymä
- CAQuietExec: Virhe: tietokannan luominen ei onnistu

Syynä on olemassa olevan LocalDB asennuksen ongelma. Tallennustilan emulaattori käyttää oletusarvon mukaan LocalDB säilyttää tiedot, kun se varmistaminen Azure liittyviä palveluja. Voit palauttaa LocalDB esiintymän suorittamalla seuraavat komennot ennen kuin yrität asentaa SDK komentokehote ikkunassa.

    sqllocaldb stop v11.0
    sqllocaldb delete v11.0
    delete %USERPROFILE%\WAStorageEmulatorDb3*.*
    sqllocaldb create v11.0

**Poista** -komennon poistaa vanhoja tietokannan tiedostoja tallennustilan emulaattori aiempia asennuksia.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Sinulla tallennuspalvelu eri ongelma

Jos vianmäärityksen edellisten kohtien sisälly tallennustilan-palvelun kanssa on ongelma, kannattaa antaa seuraavat hallintatavan ohjelmistossa ja vianmääritys ongelmaa.

- Tarkista, ovatko tapahtuu muutoksia base sitoutuvat toiminta-kohdassa arvot. Valitse arvot voit ehkä määrittää, onko ongelma on lyhytkestoisia tai pysyvästi ja tallennustilaa mitä toimintoja ongelman vaikuttavia.
- Arvot-toiminnon avulla voi auttaa virheitä pitkäksi aikaa tarkempia tietoja palvelinpuolen lokin tietojen hakua. Nämä tiedot auttavat Määritä ja ratkaise ongelma.
- Jos palvelinpuolen lokeja tietoja ei ole riittäviä ongelman vianmäärityksen onnistuneesti, voit tutkia asiakassovellus ja työkaluilla, kuten Fiddler, Wiresharkin ja Microsoft viestin Analyzer tutkia verkon toiminnan tallennustilan asiakkaan kirjaston asiakaspuolen lokeista.

Lisätietoja Fiddler käyttämisestä on artikkelissa "[Lisäys 1: Fiddler avulla voit siepata HTTP- ja HTTPS-liikenne]."

Lisätietoja Wiresharkin käyttämisestä on artikkelissa "[Lisäys 2: Wiresharkin avulla voit siepata verkkoliikennettä]."

Lisätietoja Microsoft viestin analysoimisen avulla, katso "[lisäys 3: Microsoft Message analysoimisen avulla voit siepata verkkoliikennettä]."

## <a name="appendices"></a>Lisäykset

Lisäykset kuvaavat useita työkaluja, joilla saattaa olla hyödyllistä, kun ohjelmistossa ja Azure-tallennustilan (ja muut palvelut)-ongelmien vianmääritys. Nämä työkalut eivät kuulu Azuren tallennustilaan ja kolmansien osapuolten tuotteet ovat. Sellaisenaan voi olla Microsoft Azure tai Azuren tallennustilaan tuki sopimuksen ei koske nämä lisäykset kuvatut työkalut ja siksi arvioinnin yhteydessä kannattaa tarkistaa käyttöoikeudet ja tuki tarjoajien käytettävissä olevista asetuksista työkaluja.

### <a name="appendix-1"></a>Lisäys 1: HTTP- ja HTTPS-liikenne siepata Fiddler avulla

Fiddler on hyödyllinen työkalu analysointiin HTTP- ja HTTPS-liikenteen asiakassovellus ja käytät Azure tallennuspalvelu välillä. Voit ladata Fiddler <a href="http://www.telerik.com/fiddler" target="_blank">http://www.telerik.com/fiddler</a>.

> [AZURE.NOTE] Fiddler voit toistaa HTTPS-liikenne; Lue huolellisesti selvittääksesi, miten se tekee tämän ja tietoturvaan ymmärtää Fiddler-asiakirjat.

Tämä liite sisältää lyhyt vaiheista määrittäminen Fiddler kannattaa tallentaa liikenne paikalliseen tietokoneeseen, johon on asennettu Fiddler ja Azure tallennustilan-palveluiden välillä.

Kun Fiddler on käynnistetty se käynnistyy sieppaus HTTP- ja HTTPS-liikenne paikalliseen tietokoneeseen. Seuraavassa on muutamia hyödyllisiä komentoja ohjaukseen Fiddler:

- Lopeta ja Käynnistä sieppaus liikenne. Päävalikosta Valitse **Tiedosto** ja valitse sitten **Siepata liikenne** Vaihda sieppaus käyttöön ja poistaminen käytöstä.
- Tallenna siepatun liikennetiedot. Siirry päävalikosta, **tiedoston**, valitse **Tallenna**ja valitse sitten **Kaikissa istunnoissa**: Tämän avulla voit tallentaa liikenne istunnon arkistotiedostoja. Voit ladata istunnon-arkisto myöhemmin analyysia varten tai lähettää sen pyydettäessä Microsoft-tuen.

Voit rajoittaa liikenteestä, joka kuvaa Fiddler, suodattimia, joita voit määrittää **suodattimet** -välilehti. Seuraavassa näyttökuvassa näkyy suodatin, joka sisältää vain liikenne lähettää **contosoemaildist.table.core.windows.net** tallennustilan päätepisteen:

![][5]

### <a name="appendix-2"></a>Lisäys 2: Sieppaaminen verkkoliikennettä Wiresharkin avulla

Wireshark on verkko-protokollan analysoiminen, jonka avulla voit tarkastella yksityiskohtaisia paketin verkkoprotokollien monien. Voit ladata Wiresharkin <a href="http://www.wireshark.org/" target="_blank">http://www.wireshark.org/</a>.

Seuraavien ohjeiden avulla voit siepata paketin yksityiskohtaiset tiedot tietoliikenteen paikallisesta tietokoneesta johon Wiresharkin asensit, taulukko-palveluun Azure-tallennustilan tilin.

1.  Käynnistä Wiresharkin paikallisessa tietokoneessa.
2.  Valitse **Aloitus** -osaan paikalliseen verkkoon kirjauduttaessa käyttöliittymän tai liittymät, jotka ovat yhteydessä Internetiin.
3.  Valitse **sieppaa asetukset**.
4.  Suodattimen lisääminen **Siepata suodatin** -tekstiruutuun. Esimerkiksi **isännän contosoemaildist.table.core.windows.net** määritetään Wiresharkin kannattaa tallentaa vain paketteja, jotka on lähetetty tai taulukon palvelun päätepisteestä **contosoemaildist** tallennustilan tilin. Katso luettelo kaikista siepata suodattimia <a href="http://wiki.wireshark.org/CaptureFilters" target="_blank">http://wiki.wireshark.org/CaptureFilters</a>.

    ![][6]

5.  Napsauta **Käynnistä-painiketta**. Wireshark nyt sieppaa kaikki paketit lähettää tai taulukon palvelun päätepisteestä, kun käytät asiakassovellus paikallisessa tietokoneessa.
6.  Kun olet valmis, Valitse päävalikosta **siepata** ja valitse sitten **Lopeta**.
7.  Tiedoston Wiresharkin siepata siepatun tietojen tallentamiseen päävalikosta Valitse **Tiedosto** ja valitse sitten **Tallenna**.

WireShark korostaa virheitä, jotka ovat **packetlist** -ikkuna. Voit käyttää myös **Asiantuntija tietoja** -ikkuna (Valitse **Analysoi**ja sitten **Asiantuntija tiedot**) virheiden ja varoitusten yhteenveto.

![][7]

Voit myös valita Näyttää TCP-tietoja, kuten sovelluskerroksen näkee hiiren kakkospainikkeella TCP-tietoihin ja valitse **seuraa TCP-muodossa**. Tämä on erityisen hyödyllinen, jos oman dump ilman sieppaus suodatin tallennetaan. Katso <a href="http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html" target="_blank">tähän</a> lisätietoja.

![][8]

> [AZURE.NOTE] Lisätietoja Wiresharkin on <a href="http://www.wireshark.org/docs/wsug_html_chunked/" target="_blank">Wiresharkin käyttäjien opas</a>.

### <a name="appendix-3"></a>Lisäys 3: Microsoft viestin analysoimisen avulla voit siepata verkkoliikennettä

Voit HTTP- ja HTTPS-liikenne samalla tavalla kuin Fiddler, käytä Microsoft viestin Analyzer ja siepata verkkoliikennettä Wiresharkin tapaan.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Määritä web jäljitys-istunnon Microsoft viestin analysoimisen avulla

Voit määrittää Microsoft viestin Analyzer HTTP- ja HTTPS tietoliikenteen web jäljitys-istunnon Microsoft viestin Analyzer-sovelluksen käyttämiseen ja valitse sitten **Tiedosto** -valikon **Capture/Jäljitä**. Valitse käytettävissä jäljitys skenaariot luettelo **-WWW-välityspalvelimen**. Valitse **Jäljitä skenaarion määritys** -ruudussa **HostnameFilter** -tekstiruutuun Lisää oman tallennustilan päätepisteet (Voit etsiä seuraavat nimet Azure perinteinen portaalissa) nimet. Esimerkiksi jos Azure-tallennustilan tilin nimi on **contosodata**, kannattaa lisätä seuraavat **HostnameFilter** tekstiruudun:

    contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net

> [AZURE.NOTE] Välilyönnin erottaa isäntänimet.

Kun olet valmis aloittamaan jäljitystietojen kerääminen, napsauta **Käynnistä-painiketta ja** -painiketta.

Lisätietoja Microsoft viestin Analyzer- **Web-välityspalvelimen** jäljitys on TechNetin <a href="http://technet.microsoft.com/library/jj674814.aspx" target="_blank">PEF WebProxy toimittaja</a> .

Valmiin **WWW-välityspalvelimen** jäljitys, Microsoft viestin analysoinnissa perustuu Fiddler; se voi kerätä asiakkaan HTTPS-liikenne ja näyttää salaamaton HTTPS-viestit. **Web-välityspalvelimen** seurannasta toimii määrittämällä paikallisen välityspalvelimen kaikki HTTP- ja HTTPS-liikenteen, jonka avulla se käyttää salata viestejä.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Microsoft viestin analysoimisen avulla verkko-ongelmien vianmäärityksessä

Lisäksi Microsoft viestin Analyzer- **Web-välityspalvelimen** jäljityksen avulla voit tallentaa seurantakohteeseen HTTP/HTTPs-liikenne välille asiakassovellusta ja tallennustilaa palvelun tiedot, voit käyttää myös valmiin **Paikallisen Link Layer** seurannasta kannattaa tallentaa verkon paketin tiedot. Tämän avulla voit kerätä tietoja samaan tapaan kuin, jossa voit siepata Wiresharkin kanssa ja vianmääritys verkon ongelmat kuten hylätyt paketit.

Seuraavassa näyttökuvassa näkyy esimerkki **Paikallisen Link Layer** jäljitys joitakin **tiedoksi** viestien mukana **DiagnosisTypes** -sarakkeessa. Valitsemalla kuvake **DiagnosisTypes** -sarakkeessa näkyy viestin tietoja. Tässä esimerkissä palvelimeen uudelleen viestin #305, koska se ei mennyt kuittausta asiakaskoneesta:

![][9]

Kun luot jäljitysistunnon Microsoft viestin Analyzer, voit määrittää vähentää melunlähteistä seurannasta suodattimet. **Siepata / Jäljitä** sivulla, jossa voit määrittää seurannasta Valitse **Microsoft-Windows-NDIS-PacketCapture** **määrittäminen** valinnan. Seuraavassa näyttökuvassa näkyy määritys, joka suodattaa TCP-liikenne kolme tallennustilan palvelua IP-osoitteet:

![][10]

Lisätietoja Microsoft viestin Analyzer paikallisen Link Layer jäljitys on TechNetin <a href="http://technet.microsoft.com/library/jj659264.aspx" target="_blank">PEF NDIS-PacketCapture tarjoaja</a> .

### <a name="appendix-4"></a>Lisäys 4: Excelin avulla voit tarkastella arvot ja kirjaudu tiedot

Useita työkaluja mahdollistavat tallennustilan arvot-tietojen lataaminen Azure-taulukkotallennus erotinmerkkejä muodossa, joka on helppo lataatko tiedot Exceliin, tarkasteleminen ja analysointia varten. Azure-blob-säiliö tallennustilan kirjaaminen tiedot on jo erotinmerkkejä muotoa, jota olet ladannut Exceliin. Kuitenkin tarvitset lisää asianmukaiset sarakeotsikot, tiedot <a href="http://msdn.microsoft.com/library/azure/hh343259.aspx" target="_blank">Analytics Log Tallennusmuoto</a> ja <a href="http://msdn.microsoft.com/library/azure/hh343264.aspx" target="_blank">Tallennustilaa Analytics arvot Taulukkorakenteen</a>mukaan.

Voit tuoda tallennustilan kirjaaminen tiedot Exceliin, kun se ladataan Blob-objektien tallennustilaan:

- Valitse **tiedot** -valikko, **Tekstistä**.
- Siirry lokitiedosto haluat tarkastella, ja valitse **Tuo**.
- Valitse **Ohjatussa tekstin tuomisessa**vaiheessa 1 **erotettu**.

Valitse Vaihe 1 **Ohjattu tekstin tuominen**, valitse vain erottimen **puolipiste** ja valitse double-tarjouksen **tekstin tarkenteeksi**. Valitse **Valmis** ja valitse, mihin haluat sijoittaa tiedot työkirjassasi.

### <a name="appendix-5"></a>Lisäys 5: Hakemuksen tiedot Visual Studio Team Services-palveluiden seurantaa

Voit käyttää myös sovelluksen tiedot-toiminnon Visual Studio Team Services-palveluiden osana suorituskyky ja käytettävyyden seuranta. Tämä työkalu voi:

- Varmista, että web-palvelu on käytettävissä ja vastaa. Riippumatta siitä, onko sovellus sivuston tai laite-sovellusta, joka käyttää web-palvelu, se Testaa URL-osoitteesi muutaman minuutin välein sijainneista eri puolilla maailmaa ja ilmoittamaan, jos on ongelma.
- Vianmääritys nopeasti haluamasi suorituskykyongelmia tai poikkeukset-web-palvelussa. Selvitä, jos suorittimen tai muut resurssit ovat parhaillaan sitä venytetään, pyydä pinon jäljittää poikkeukset ja helposti etsiä log jäljittää. Jos sovelluksen suorituskyvyn laskee rajoissa, Microsoft lähettää sinulle sähköpostiviestin. Voit valvoa .NET ja Java-verkkopalveluihin.

AT aika kirjoittamisen hakemuksen tiedot on esikatselussa. Löydät lisätietoja <a href="http://msdn.microsoft.com/library/azure/dn481095.aspx" target="_blank">Visual Studio Team Services MSDN-sovelluksen tietoa</a>.


<!--Anchors-->
[Johdanto]: #introduction
[Kuinka tässä oppaassa on järjestetty]: #how-this-guide-is-organized

[Tallennustilan palvelun seuranta]: #monitoring-your-storage-service
[Palvelun kunnon valvonta]: #monitoring-service-health
[Seurannan kapasiteetti]: #monitoring-capacity
[Käytettävyys seuranta]: #monitoring-availability
[Suorituskyvyn valvominen]: #monitoring-performance

[Ohjelmistossa tallennustilan ongelmat]: #diagnosing-storage-issues
[Palvelun ongelmia]: #service-health-issues
[Suorituskykyongelmia]: #performance-issues
[Ohjelmistossa virheet]: #diagnosing-errors
[Tallennustilan emulaattorin ongelmat]: #storage-emulator-issues
[Tallennustilan kirjaaminen Työkalut]: #storage-logging-tools
[Kirjaaminen työkaluilla]: #using-network-logging-tools

[Lopusta loppuun-jäljitys]: #end-to-end-tracing
[Korreloivaa lokitiedot]: #correlating-log-data
[Pyyntö Ostajantunnus]: #client-request-id
[Pyynnön tunnus]: #server-request-id
[Aikaleimat]: #timestamps

[Ohjeet vianmääritys]: #troubleshooting-guidance
[Arvot high AverageE2ELatency ja pienen AverageServerLatency näyttäminen]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Arvot näyttää pienen AverageE2ELatency ja pienen AverageServerLatency mutta asiakkaan ilmenee odotusaika]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Näytä arvot high AverageServerLatency]: #metrics-show-high-AverageServerLatency
[On ilmennyt odottamaton viipeitä viestin toimituksen jonon]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Arvot näkyvät kasvua PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[PercentThrottlingError lyhytkestoisia suureneminen]: #transient-increase-in-PercentThrottlingError
[Pysyvä kasvu prosentteina PercentThrottlingError virhe]: #permanent-increase-in-PercentThrottlingError
[Arvot näkyvät kasvua PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Arvot näkyvät kasvua PercentNetworkError]: #metrics-show-an-increase-in-PercentNetworkError

[Asiakkaan lähetetään HTTP 403 (kielletty)-viestit]: #the-client-is-receiving-403-messages
[Asiakkaan lähetetään HTTP 404 (ei löydy)-viestit]: #the-client-is-receiving-404-messages
[Asiakas- tai toinen prosessi aiemmin poistettu objekti]: #client-previously-deleted-the-object
[Jaettu Access allekirjoitus (SAS) todennus-ongelma]: #SAS-authorization-issue
[Asiakkaan JavaScript-koodia ei ole oikeutta käyttää objekti]: #JavaScript-code-does-not-have-permission
[Verkkovirhe]: #network-failure
[Asiakkaan vastaanottaa viestejä 409 HTTP (ristiriita)]: #the-client-is-receiving-409-messages

[Arvot näyttää pienen PercentSuccess tai analytics lokimerkintöjä on toimintojen ClientOtherErrors tapahtuman tila]: #metrics-show-low-percent-success
[Kapasiteetin arvot näyttäminen odottamattomia kasvua tallennustilan kapasiteetin käyttö]: #capacity-metrics-show-an-unexpected-increase
[On ilmennyt odottamaton käynnistyy, joissa on paljon liitetty näennäiskiintolevyjen näennäiskoneiden on]: #you-are-experiencing-unexpected-reboots
[Ongelma ilmenee käyttämästä tallennustilan emulaattori kehitys- tai]: #your-issue-arises-from-using-the-storage-emulator
["X" toiminto ei toimi tallennustilan emulaattori]: #feature-X-is-not-working
[Virhe "jokin HTTP-otsikot-arvo ei ole oikeassa muodossa" tallennustilan emulaattori käytettäessä]: #error-HTTP-header-not-correct-format
[Tallennustilan emulaattori suorittaminen edellyttää järjestelmänvalvojan oikeuksia]: #storage-emulator-requires-administrative-privileges
[Kohtaat ongelmia asentamisessa Azure SDK .NET varten]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Sinulla tallennuspalvelu eri ongelma]: #you-have-a-different-issue-with-a-storage-service

[Lisäykset]: #appendices
[Lisäys 1: HTTP- ja HTTPS-liikenne siepata Fiddler avulla]: #appendix-1
[Lisäys 2: Sieppaaminen verkkoliikennettä Wiresharkin avulla]: #appendix-2
[Lisäys 3: Microsoft viestin analysoimisen avulla voit siepata verkkoliikennettä]: #appendix-3
[Lisäys 4: Excelin avulla voit tarkastella arvot ja kirjaudu tiedot]: #appendix-4
[Lisäys 5: Hakemuksen tiedot Visual Studio Team Services-palveluiden seurantaa]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/overview.png
[2]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/portal-screenshot.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/mma-screenshot-2.png
