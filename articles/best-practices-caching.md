<properties
   pageTitle="Välimuistin ohjeet | Microsoft Azure"
   description="Ohjeita suorituskyky ja skaalattavuus parantamiseksi."
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
   ms.date="07/14/2016"
   ms.author="masashin"/>


# <a name="caching-guidance"></a>Välimuistin ohjeet

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

Välimuistin on yleisiä tapaa, joilla voit parantaa suorituskyky ja skaalattavuus järjestelmän pyritään. Se tekee tämän kopioimalla tilapäisesti usein käytettyjen tietojen nopea tallennustilaa, joka sijaitsee Sulje sovellus. Jos tämä nopea tietosäilö sijaitsee lähemmäksi kuin alkuperäinen lähde-sovellukseen, sitten välimuistiin voi merkittävästi parantaa vastauksen kertaa asiakassovellukset käsittelevä tiedot nopeasti.

Välimuistiin on mahdollisimman tehokasta kun asiakkaan esiintymän lukee toistuvasti samat tiedot etenkin silloin, kun alkuperäinen tietovaraston koskevat kaikki seuraavat ehdot:
- Pysyy suhteellisen staattinen.
- Se on hidas, välimuistin nopeuden verrattuna.
- On ristiriita korkean tason veloittaa.
- Se on kaukana verkon latenssin voivat aiheuttaa pääsy hidastua.

## <a name="caching-in-distributed-applications"></a>Välimuistiin tallentamisen jaetut sovellukset

Jaetut sovellukset Toteuta yleensä toisen tai molemmat seuraavista strategioita kun välimuistin tiedot:

- Voit käyttää yksityinen välimuisti-kohtaa, johon tiedot säilytetään paikallisesti tietokoneeseen, jossa on käytössä sovelluksen tai palvelun esiintymä.
- Voit käyttää jaettua välimuisti-että tarjoillaan yleisiä tietolähteeksi, joita voit käyttää useita prosesseja ja/tai koneet.

Kummassakin tapauksessa välimuistiin voi käyttää asiakkaan ja/tai palvelinpuolen. Asiakaspuolen välimuisti tehdään prosessi, joka sisältää käyttöliittymän järjestelmän, kuten selaimen tai työpöytäsovellus.
Palvelinpuolen välimuistiin tehdään prosessi, joka tarjoaa etäyhteyden käynnissä olevat business-palveluita.

### <a name="private-caching"></a>Yksityinen välimuistiin tallentaminen

Välimuistin yleisin tyyppi on ladatun-kaupasta. Se on yhden prosessin osoitetilan säilytetään ja käyttämien koodi, jota käytetään prosessin. Tällaista välimuistin on lyhyt käyttämään. Erittäin tehokas tapa myös antaa projektiin vähän määriä staattinen tietoja, koska välimuistin kokoa on yleensä rajoitettu prosessin isännöivän tietokoneen käytettävissä olevan muistin määrän mukaan.

Jos haluat lisätietoja kuin on mahdollista fyysisesti muistiin välimuistiin, voit kirjoittaa paikallisen tiedostojärjestelmän välimuistiin tallennetut tiedot. Tämä on heikompi käyttämään tietoja, joita pidetään ladatun kuin, mutta pitäisi olla vielä nopeampi ja luotettava kuin tietojen noutaminen verkon kautta.

Jos sinulla on käynnissä samanaikaisesti mallia käyttävä sovellus useita kertoja, sovelluksen jokaiselle esiintymälle on oma riippumaton välimuisti pitämällä oma kopio.

Ajattele välimuistin tilannevedoksen alkuperäisiä tietoja joskus menneisyydessä. Jos näitä tietoja ei ole staattinen, on todennäköistä, että eri Palvelusovellusten esiintymät pidä niiden tallentaa tiedot eri versioita. Vuoksi maksettavan korvauksen nämä esiintymät saman kyselyn voi palauttaa erilaiset tulokset, kuten kuvassa 1.

![Ladatun-välimuistin eri esiintymissä sovelluksen avulla](media/best-practices-caching/Figure1.png)

_Kuva 1: Käyttämällä ladatun-välimuistin sovelluksen eri esiintymissä_

### <a name="shared-caching"></a>Jaettu välimuistiin tallentaminen

Jaetun välimuistin avulla voit lievittämään koskee, että tiedot voi erilaisia kunkin välimuistin, jonka voi ilmetä ladatun välimuistiin. Jaetun välimuistiin varmistaa, että eri Palvelusovellusten esiintymät nähdä välimuistiin tallennetut tiedot samassa näkymässä. Se tekee tämän etsiminen välimuistin eri sijainnissa, yleensä isännöidään erillisen palvelun osana 2 kuvassa esitetyllä tavalla.

![Jaetun välimuistin käyttäminen](media/best-practices-caching/Figure2.png)

_Kuva 2: Käyttämällä jaetun välimuisti_

Jaetun välimuistiin lähestymistapa tärkeitä etuna on se sisältää skaalattavuus. Monia jaetun välimuisti-palveluita käyttämällä klusterin palvelinten toteutettu ja hyödyntää ohjelmisto, joka jakaa tietoja klusterin avoimella tavalla. Sovelluksen esiintymä lähettää pyynnön yksinkertaisesti välimuisti-palvelun.
Pohjana oleva infrastruktuuri on vastuussa välimuistiin tallennetut tiedot klusterin sijainnin määrittäminen. Voit helposti skaalata välimuistin lisäämällä palvelimien.

On jaettu välimuistiin lähestymistapa tärkeimmät haitat:
- Välimuisti on hidas käyttää, koska se ei enää pidetään paikallisesti kunkin sovelluksen esiintymään.
- Erillinen välimuisti-palvelun toteuttamisesta vaatimus ehkä lisätä monimutkaisuus ratkaisu.

## <a name="considerations-for-using-caching"></a>Huomioitavaa välimuistiin tallentaminen

Seuraavissa kohdissa kuvataan tarkemmin suunnittelusta ja käytöstä välimuistin Huomioitavaa.

### <a name="decide-when-to-cache-data"></a>Päättää, milloin kannattaa tallentaa tiedot

Välimuistin voidaan parantaa huomattavasti suorituskyky ja skaalattavuus käytettävyyttä. Mitä enemmän tietoja, jotka sinulla ja käyttäjät, joilla on pystyttävä käyttämään tiedoista suurempi edut välimuistiin muuttuvat mitä suurempi luku. Tämä johtuu välimuistiin pienentää viive ja ristiriita, joka liittyy käsittely samanaikaiset pyynnöt alkuperäisen tietovaraston suurista tietomääristä.

Tietokannan voi esimerkiksi tue samanaikaisia yhteyksiä rajoitettu määrä. Tietojen noutaminen jaetun välimuistin kuitenkin asemesta pohjana olevan tietokannan ansiosta asiakassovellus käyttää näitä tietoja, vaikka käytettävissä yhteyksien määrä on tällä hetkellä lopussa. Lisäksi, jos tietokanta ei ole käytettävissä, asiakassovellukset ehkä Jatka käyttämällä välimuistin tiedot, joita pidetään.

Harkitse välimuistin tiedot, jotka on lukea usein, mutta muokattu harvoin (esimerkiksi tietoja, jotka sisältävät suurempi osuus luettujen kuin kirjoitus-toimintoja). Ei, suosittelemme käyttämään välimuistin tärkeitä tietoja tärkeimpien kaupan. Varmista sen sijaan, että sovelluksesi varmuuskopioida kaikki muutokset tallentuvat aina pysyvän tietosäilö. Tämä tarkoittaa, että jos välimuisti ei ole käytettävissä, sovelluksen voit jatkaa toimimaan käyttämällä tietovaraston ja et menetä tärkeitä tietoja.

### <a name="determine-how-to-cache-data-effectively"></a>Määrittää, kuinka välimuistin tietoja tehokkaasti

Käyttämisessä välimuistin tehokkaasti sijaitsee välimuistin sopivimman tietojen määrittämiseen ja välimuistiin määritettynä ajankohtana. Tietoja voidaan lisätä tarvittaessa ensimmäistä kertaa, se on noudettu sovelluksen välimuisti. Tämä tarkoittaa, että sovellus on hakea tietoja vain kerran tietovaraston ja kyseisen käyttöä voidaan täyty välimuistin avulla.

Voit myös välimuisti kokonaan tai osittain määritetään tietojen etukäteen, yleensä kun sovellus käynnistyy (toimintatavan, jota kutsutaan valuuttamuunnosten). Se ehkä kuitenkin suositeltavaa toteuttamisesta valuuttamuunnosten suuri välimuistin, koska tätä tapaa voit asettaa alkuperäisen tietovaraston äkillinen, suuri kuormitus, kun sovellus käynnistyy.

Usein käyttötavat analyysin avulla voit päättää, kokonaan tai osittain valmiiksi välimuistin ja valita välimuistin tiedot. Esimerkiksi voi olla hyödyllistä Määritä välimuistin staattisen käyttäjäprofiilin tiedot asiakkaat, jotka käyttävät sovelluksen säännöllisesti (esimerkiksi päivittäin), mutta ei niiden asiakkaiden kohdalla, jotka käyttävät sovellusta vain kerran viikossa.

Välimuistin yleensä toimii hyvin tietoja, jotka on muuttumaton tai harvoin muutokset. Esimerkkejä ovat viittaus tietoja, kuten tuotteen ja hintatiedot e commerce-sovelluksen tai staattinen resursseja, jotka ovat kallista muodostamiseen. Jotkin tai kaikki nämä tiedot voidaan ladata välimuistiin sovellusten käynnistyksen Pienennä demand resursseja ja suorituskyvyn parantamiseksi. Myös ehkä on tausta-prosessi, joka päivittää säännöllisesti viittaus välimuistin sen varmistamiseksi, että tiedot ovat ajan tasalla tai, joka päivittää välimuistin kun viittauksen tietojen muutokset.

Välimuistiin on hyötyä vähemmän dynaamisia tietoja, vaikka siellä poikkeuksiin tätä huomiota (katso Välimuistin hyvin dynaamisen varten tämän artikkelin Lisätietoja-osion). Kun alkuperäiset tiedot muuttuvat säännöllisesti, välimuistiin tallennetut tiedot on todella nopeasti vanhentuneiden tai välimuistin synkronoimista alkuperäisen tietovaraston katseltavan vähentää tehokkuutta välimuistiin tallentamisen.

Huomaa, että välimuistia ei ole täydellinen tiedot kohteen. Esimerkiksi jos tieto-osan moniarvoisen objektin, kuten pankin asiakkaan nimi, osoite ja tilin saldo, joitakin näitä elementtejä voi säilyvät staattinen (kuten nimi ja osoite), kun toiset (esimerkiksi tilin saldo) voivat olla dynaamisemmaksi. Näissä tilanteissa voi olla hyödyllistä välimuistin tiedot staattinen osiin ja hakeminen (tai laskea) vain jäljellä olevat tiedot, kun se on tarpeen.

Suosittelemme, että Suorita suorituskyvyn testaus ja käyttö analyysi, onko edeltävä populaation tai tarvittaessa lataaminen välimuistin tai molempia, sopiva. Päätös olisi perusteella haihtuvuus ja tietojen käyttö kuvio. Välimuistin käyttö ja suorituskyvyn analysointi on erityisen tärkeää sovelluksissa, jotka ilmenee paksu latautuu ja täytyy olla erittäin skaalattava. Esimerkiksi erittäin skaalattava tilanteissa se saattaa järkevää Määritä välimuistin vähentää tietovaraston kuormitus aikaan.

Välimuistiin, myös voidaan välttää toistuvan funktiolauseita, kun sovellus on käynnissä. Jos toiminto muuntaa tietoja tai monimutkaisia laskutoimituksen, se tallennetaan välimuistin toiminnon tulokset. Jos sama laskutoimitus vaaditaan jälkeenpäin, sovelluksen voit hakea tulokset ainoastaan välimuistista.

Sovelluksen muokata tietoja, joita pidetään välimuistiin. Suosittelemme kuitenkin ajattelua välimuistin kuin lyhytkestoisia tietosäilö, joka saattaa kadota milloin tahansa. Älä tallenna tärkeiden tietojen välimuistin. Varmista, että myös alkuperäisen tietovaraston tietojen ylläpitämiseen. Tämä tarkoittaa, että jos välimuisti ei ole käytettävissä, voit pienentää mahdollisuutta menetä.

### <a name="cache-highly-dynamic-data"></a>Välimuistin erittäin dynaamiset tiedot

Kun tallennat nopeasti muuttaminen tiedot pysyvät tietosäilö, se asettaa katseltavan järjestelmään. Esimerkkinä laitteella, joka ilmoittaa jatkuvasti tila tai joissakin mitta. Jos jokin sovellus ei ole välimuistin tiedoista, että välimuistiin tallennetut tiedot on melkein aina vanhentuneiden välein, sama vastikkeen voi olla tosi, jos tallentaminen ja hakeminen tiedot tiedot-kaupasta. Valitse Tallenna ja noutaa tiedot kuluvaa aikaa se voi ovat muuttuneet.

Esimerkiksi tilanteessa harkitse edut tallentaminen dynaamiset tiedot suoraan välimuistin pysyvä tietovaraston sijaan. Tiedot on vähäinen ja ei edellytä valvonta, valitse se ei ole väliä Jos ajoittaiset muutokset menetetään.

### <a name="manage-data-expiration-in-a-cache"></a>Hallitse tietojen voimassaolon välimuistiin

Useimmissa tapauksissa tiedot, joita pidetään välimuistiin on tietoja, joita pidetään alkuperäisen tietovaraston kopio. Alkuperäinen tietovaraston tietoja voi muuttaa sen jälkeen, kun se on välimuistiin, aiheuttaa välimuistiin tallennetut tiedot muuttuvat vanhentuneiden. Voit määrittää tietojen voimassa ja vähentää ajan, jossa tietoja voi olla vanhentunut välimuistin monta välimuistiin järjestelmien avulla.

Kun välimuistiin tallennetut tiedot vanhenee, se on poistettu välimuistista ja sovellus on tietojen hakemiseen alkuperäiset tiedot-kaupasta (se voi takaisin juuri hakemisen tiedot välimuistiin). Voit määrittää oletusarvon vanhenemiskäytännön määrittäessäsi välimuistissa. Yksittäisten objektien voimassaoloajan monta välimuisti-palveluja, voit säätää myös kun tallennat ne ohjelmallisesti välimuistin.
Jotkin tallentaa avulla voit määrittää voimassaoloajan absoluuttinen arvo tai liukuva arvo, joka aiheuttaa kohde poistetaan välimuistista, jos se ei käsitellä määritetyn ajan kuluessa. Tämä asetus ohittaa kaikki välimuistin laajuinen vanhenemiskäytäntöä, mutta vain määritetyt objektit.

> [AZURE.NOTE] Harkitse voimassaoloajan välimuistin ja joka sisältää huolellisesti objekteja varten. Jos teet liian lyhyt, objektien päättyy liian nopeasti ja vähentää välimuistin eduista. Jos teet liian pitkä ajan, riskien muuttumisesta vanhentuneita tietoja.

On myös mahdollista, että välimuistin ehkä Täytä ylös, jos tiedot ovat käytettävissä olevaa tietoa kauan on oikeus. Tässä tapauksessa pyyntöihin uusien kohteiden lisääminen välimuistiin voi aiheuttaa jotkin kohteet poistetaan asiakasistuntojen eli eviction. Välimuistin services Poista yleensä tietojen tarkistamiseen (LRU) pienimmän viimeksi käytettyjen, mutta yleensä voit ohittaa tämän käytännön ja estää parhaillaan poistaa kohteita. Jos tätä tapaa kannattaa antaa voit kuitenkin riskin yli välimuistin käytettävissä oleva muisti. Sovellus, joka yritetään lisätä kohteen välimuistiin epäonnistuu poikkeuksen vuoksi.

Jotkin välimuistiin käyttöotot antaa muita eviction käytännöt. On useita erilaisia eviction käytännöt. Näitä ovat:
- Viimeksi käytetyt käytännön (olettaen että niitä ei vaadita uudelleen).
- Ensimmäinen-in-first-out käytäntö (vanhimmat tiedot on poistaa ensin).
- Eksplisiittinen Poistamiskäytäntö käynnistettävät tapahtuma (esimerkiksi tietoja muokataan) perusteella.

### <a name="invalidate-data-in-a-client-side-cache"></a>Vahingoittaa asiakkaan välimuistin tiedot

Tiedot, jotka säilytetään asiakkaan välimuistissa pidetään yleensä on alkuperäiset tiedot asiakkaan palvelun yhteyteen ulkopuolella. Asiakas voi lisätä tai poistaa asiakkaan välimuistin tietoja ei voi suoraan pakottaa palvelu.

Tämä tarkoittaa, että se on mahdollista, voit edelleen käyttää vanhentunutta tietoa huonosti määritetty välimuistin käyttävä asiakas. Esimerkiksi jos välimuistin vanhenemiskäytännöistä eivät ole oikein, asiakas voi käyttää vanhentunutta tietoa, joka on paikallisesti, kun alkuperäisen tietolähteen tiedot ovat muuttuneet.

Jos olet muodostamassa web-sovelluksen, joka on tietoja HTTP-yhteyden kautta, web-asiakasohjelma (kuten selaimessa tai web-välityspalvelimen) voit pakottaa implisiittisesti hakeaksesi päivitetyt tiedot. Voit tehdä tämän Jos resurssin päivitetään resurssin URI muutoksesta. Verkkosovellukset käytetään yleensä resurssin URI asiakkaan välimuistin avaimeksi, jolloin URI muuttuessa web-asiakasohjelman ohittaa jokin aiemmin välimuistiin tallennetut versiot resurssin ja hakee uuden version sijaan.

## <a name="managing-concurrency-in-a-cache"></a>Samanaikainen välimuistiin hallinta

Tallentaa käsitellään usein jaettava sovelluksen useita kertoja. Sovelluksen jokaiselle esiintymälle voit lukea ja muokata tietoja välimuistin. Näin ollen saman samanaikainen ongelmat, jotka kaikki jaetut tiedot kaupan syntyä koskevat myös välimuistin. Tilanne, jossa sovellus on muokattava tiedot, joita pidetään välimuistin voit joutua muuttamaan varmistaa, että esiintymän sovelluksen tekemät päivitykset eivät korvaa toinen tekemät muutokset.

Tietojen laatu ja rajuille todennäköisyys mukaan antaa kaksi tavoilla voit samanaikainen:

- __Optimistinen.__ Välittömästi ennen päivitystä tiedot, sovellus tarkistaa, onko välimuistin tiedot on muuttunut, vaikka se on noudettu. Jos tiedot ovat silti sama, voit tehdä muutoksen. Muussa tapauksessa sovellus on päivittäminen sitä. (Liiketoimintalogiikan, joka ohjaa päätökseen on sovelluksen kielikohtaiset.) Tämä menetelmä sopii tilanteissa missä päivitykset ovat epäsäännölliset tai jos rajuille on epätodennäköistä.
- __Pessimistinen.__ Kun se hakee tietoja, sovelluksen lukitsee välimuistin estämään toinen muuttamista. Tämä prosessi varmistaa, että näin tapahtuu ei onnistu, mutta ne estää myös muut esiintymät, jotka on käyttänyt samat tiedot. Pessimistisen voivat vaikuttaa ratkaista skaalattavuus ja suositellaan vain lyhytkestoinen toimintoja. Tätä tapaa kannattaa ehkä sovellu tilanteissa, joissa rajuille on todennäköisesti, etenkin silloin, kun sovellus päivittää useita kohteita välimuistin ja varmista, että nämä muutokset ovat johdonmukaisesti.

### <a name="implement-high-availability-and-scalability-and-improve-performance"></a>Suuri käytettävyys ja skaalattavuus ja suorituskyvyn parantaminen

Vältä välimuistin kuin ensisijaisen säilön tietoja. Tämä on alkuperäinen tietosäilö, josta välimuistin lisätään rooli. Alkuperäinen tietosäilö on vastuussa tiedot pysyvyys.

Varmista, ettet mitataan kriittinen riippuvuuksien jaetun välimuisti-palvelun käytettävyydestä ratkaisujen. Sovelluksen pitäisi jatkaa toimi, jos palvelun, joka tarjoaa jaetun välimuistin ei ole käytettävissä. Sovellus ei olisi jumittua tai epäonnistua, kun haluat jatkaa välimuisti-palvelun odotettaessa.

Tämän vuoksi sovellus on valmisteltava välimuisti-palvelun saatavuuden tunnistaminen ja kuuluvat takaisin alkuperäiseen tietojen Store Jos välimuisti ei ole käytettävissä. [Katkaisin kaava](http://msdn.microsoft.com/library/dn589784.aspx) on hyötyä käsittely Tämä skenaario. Palvelun, joka tarjoaa välimuistin voidaan palauttaa ja kun se on käytettävissä, välimuistin uudelleen, kun tiedot on luku-lomakkeen Alkuperäinen tietovaraston seuraavan strategia, kuten [välimuistin kesantoala kuvio](http://msdn.microsoft.com/library/dn589799.aspx).

Voi kuitenkin skaalattavuus vaikutus järjestelmän Jos sovellus takaisin alkuperäiseen tietojen Store kun välimuisti on tilapäisesti poissa käytöstä.
Kun tietovaraston hyödynnetään, alkuperäinen tietovaraston saattaa swamped on tietoja, tuloksena on aikakatkaisu pyyntöjä ja yhteydet epäonnistui.

Harkitse paikallisen, yksityinen välimuistin sovelluksen, kaikki sovelluksen esiintymät käyttää jaettua välimuistin ja esiintymissä. Kun sovellus hakee kohteen, se voi tarkistaa ensin sen paikallisen välimuistin ja valitse jaetun välimuistin ja lopuksi, alkuperäiset tiedot säilytetään. Paikallisen välimuistin kaikille voi käyttää tietoja joko jaetun välimuistin tai tietokannan Jos jaetun välimuistia ei ole käytettävissä.

Tämän menetelmän edellyttää varovainen kokoonpanon tulossa liian vanhentunut, jotka koskevat jaetun välimuistin paikallisen välimuistin estäminen. Kuitenkin paikallisen välimuistin toimii puskurin Jos jaetun välimuistia ei ole käytettävissä. Kuva 3 näyttää tätä rakennetta.

![Paikallisen, yksityinen välimuistin käyttäminen jaetun välimuistin](media/best-practices-caching/Caching3.png)
_Kuva 3: paikallisen, yksityinen välimuistin käyttäminen jaetun välimuistin_

Jotkin välimuisti-palvelut on tukemaan suuria tallentaa, joka sisältää suhteellisen pitkäkestoisia tietoja suuren käytettävyyden-vaihtoehto, joka sisältää automaattinen automaattisesti, jos välimuisti ei ole käytettävissä. Tämän menetelmän liittyy yleensä replikoiminen välimuistiin tallennetut tiedot, jotka on tallennettu ensisijainen välimuistin palvelimessa toissijainen välimuisti-palvelimeen ja siirrytään toissijaisen palvelimen, jos ensisijainen palvelin epäonnistuu tai yhteys menetetään.

Viive, johon on liitetty useita kohteita kirjoittaminen pienentämiseksi toissijainen palvelimeen replikoinnin mahdollisesti esiintyviä asynkronisesti tiedot kirjoitetaan välimuistin ensisijainen palvelimessa. Tämän menetelmän ohjaa mahdollisuutta välimuistissa olevia tietoja voidaan menettää Jos, mutta näitä tietoja osuus pieni pitäisi olla verrattuna välimuistin kokoa.

Jos jaettu välimuistin on suuri, voi olla hyödyllisiin osion välimuistiin tallennetut tiedot solmujen Pienennä ristiriita ja kehittää skaalattavuus yli. Monta jaetun tukevat voi lisätä dynaamisesti (ja poistaa) solmujen ja saattaa jälleen tasapainoon tiedot osioiden välillä. Tämän menetelmän voi liittyä klusterointi, jossa solmut sivustokokoelman esitetään asiakassovelluksiin saumattomasti, yksi välimuistin. Sisäisesti mutta tiedot on keskiarvosta seuraavan ennalta määritetyn jakauman strategia, joka jakaa kuormituksen tasaisesti solmujen välillä. Microsoft-sivuston [tietojen osioinnin ohjeet asiakirjan](http://msdn.microsoft.com/library/dn589795.aspx) on lisätietoja mahdollista osioinnin strategioita.

Klusterointi kasvattaa välimuistin käytettävyyttä. Jos solmu epäonnistuu, välimuistin loppuosa on edelleen käytettävissä.
Klusterointi käytetään usein yhdessä replikointi ja automaattisesti. Kukin solmu voi replikoida, ja se voi nopeasti online-tilaan Jos solmu epäonnistuu.

Monet lukeminen ja kirjoittaminen toimintojen todennäköisesti yhtä arvoja tai objekteja. Kuitenkin joskus voi olla tarpeen tallentaa tai noutaa suurista tietomääristä nopeasti.
Esimerkiksi valuuttamuunnosten välimuistin voi liittyä kirjoittaminen välimuistin satoja tai tuhansia kohteita. Sovellus on ehkä myös hakea liittyvät kohteet on runsaasti välimuistista pyynnössä osana.

Monta suurissa tallentaa on näiltä erä-toimintoja. Tämä ottaa käyttöön asiakassovellus päivittäin erittäin paljon kohteita yhden pyynnön määrittäminen pakkaaminen ja vähentää katseltavan, joka liittyy suorittamiseen pieniä pyyntöjä suuri määrä.

## <a name="caching-and-eventual-consistency"></a>Välimuistin ja potentiaalisen yhdenmukaisuuden

Välimuistin kesantoala kuvion toimimaan sovellus, joka täyttää välimuistin esiintymän on oltava käytettävissä eniten uusimpia ja yhtenäinen versio tiedot. Järjestelmässä, joka sisältää potentiaalisen yhdenmukaisuuden (kuten replikoitua tietosäilö) tämä eivät välttämättä ole kirjainkokoa.

Esiintymän sovelluksen voi muokata tieto-osa ja poistaa kohteen välimuistiin tallennettu versio. Esiintymässä sovellus yrittää lukea kohteen välimuistista, joka aiheuttaa-epäonnistumisten, niin se lukee tiedot tietovaraston ja lisää se välimuistiin. Jos tietovaraston ei ole täysin synkronoitu muiden replikoiden kanssa, sovelluksen esiintymää voi lukea ja täytä välimuistin vanha arvolla.

Lisätietoja tietojen yhdenmukaisuuden käsittely näy Microsoft-sivuston [tietojen kelpoisuus askeleet](http://msdn.microsoft.com/library/dn589800.aspx) -sivu.

### <a name="protect-cached-data"></a>Suojaa välimuistiin tallennetut tiedot

Riippumatta välimuisti-palvelun avulla, harkitse, säilytetään välimuistissa luvattomalta käytöltä tietojen suojaaminen. On kaksi tärkeimmät osat:

- Tietojen välimuistin yksityisyyttä
- Tietojen sellaisena kuin se yksityisyyttä jatkuu välimuistin ja sovellus, jossa on käytössä välimuistin välillä

Tietojen välimuistin suojaamiseksi välimuisti-palvelun voi toteuttaa todennus-järjestelmä, joka edellyttää, että sovellusten Määritä seuraavat:
- Mitä käyttäjätietojen voi käyttää välimuistin tietoja.
- Mitkä toiminnot (luku- ja kirjoitusoikeudet), jotka nämä käyttäjätietojen voivat suorittaa.

Voit pienentää yleiskustannus, joka liittyy lukemiseen ja kirjoittamiseen tiedot, kun jäsenyyden on myönnetty kirjoitus-ja/tai lukuoikeudet välimuistiin, että käyttäjätietojen käyttää mitään tietoja välimuistin.

Jos haluat rajoittaa käyttöä alijoukot välimuistiin tallennetut tiedot, tee jompikumpi seuraavista:

- Jaa välimuistin osioihin (käyttämällä eri välimuisti-palvelimet) ja vain käyttöoikeuden käyttäjätiedot osioita, jotka he voi käyttää.
- Salaa tietojen alijoukko käyttämällä eri näppäimet ja anna vain ohjelman käyttäjätietoja, kannattaa käyttää alijoukko salausavaimet. Asiakassovellus edelleen voi hakea kaikkia välimuistin tietoja, mutta se vain voi purkaa, johon se on näppäimet tietoja.

On myös suojaa tiedot, kuten välimuistin ja uloskirjautuminen jatkuu. Voit tehdä tämän määräytyvät verkkoinfrastruktuuria, asiakassovellukset avulla voit muodostaa välimuistin myöntämä suojaustoiminnot. Jos välimuisti on samassa organisaatiossa paikalla palvelimen käyttäminen isännöivä asiakassovellusten, sitten itse verkko eristystaso eivät ehkä tarvitse tehdä lisätoimia. Jos välimuisti sijaitsee etäyhteyden vaatii TCP- tai HTTP-yhteyden yleisen verkon (esimerkiksi Internet), voit käyttöönoton SSL.

## <a name="considerations-for-implementing-caching-with-microsoft-azure"></a>Huomioitavaa käyttöönoton välimuistiin Microsoft Azure kanssa

Azure on Azure Redis välimuistin. Tämä on toteutus Avaa lähde Redis.txt välimuistin, joka suoritetaan palveluna Azure palvelinkeskukseen. Se sisältää välimuistiin palvelu, jota voi käyttää missä tahansa Azure-sovelluksessa, onko sovellus on toteutettu pilvipalveluun, verkkosivuston tai Azure virtuaalikoneen sisällä. Tallentaa voidaan jakaa asiakassovellukset, joilla on asianmukaiset käyttöoikeudet-avain.

Azure Redis välimuisti on tehokas välimuistiin ratkaisu, joka sisältää käytettävyyden, skaalattavuus ja suojaus. Se suoritetaan yleensä palveluna levitä vähintään yksi erillinen tietokoneissa. Se yrittää tallentaa mahdollisimman paljon tietoja kuin mahdollista muistiin, jotta nopeasti. Tämä arkkitehtuuri tarkoituksena on annettava pieni viive ja suuri siirtonopeuden vähentämällä hidas i/o-toimintojen suorittaminen edellyttää.

 Azure Redis välimuistin on yhteensopiva monia eri API, joita käytetään asiakkaan sovellukset. Jos sinulla on nykyinen sovellus käyttää jo käynnissä paikallisen Azure Redis välimuistin, välimuistin Azure Redis on nopea siirtymispolku pilveen tallentamisesta välimuistiin.

> [AZURE.NOTE] Azure sekä hallita välimuisti-palvelun. Tämä palvelu perustuu Azure palvelun kangasta välimuisti-ohjelma. Sen avulla voit luoda hajautetun välimuistin, joka voidaan jakaa löyhästi sovellukset. Välimuistin isännöidään Azure joten tehokas-palvelimiin.
Kuitenkin tämä asetus ei enää ole suositeltavaa ja on saatavana vain tukemaan nykyisiä sovelluksia, jotka on muodostettu, voit käyttää sitä. Kaikki uudet kehittämiseen Käytä Azure Redis välimuistin.
>
> Lisäksi Azure tukee rooli-välimuistin. Tämän ominaisuuden avulla voit luoda välimuistin, joka liittyy pilvipalveluun.
Välimuistin esiintymät, verkossa tai työntekijän rooli nykyisessä ja niitä voi käyttää vain rooleihin, jotka toimivat saman cloud palvelun käyttöönoton yksikön osana. (Käyttöönoton yksikkö on roolin esiintymiä, jotka on otettu kuin pilvipalvelussa tiettyyn alueeseen.) Välimuistin on liitetty ja saman välimuistin klusterin osaksi käyttöönoton yksikkönä, joka isännöi välimuistin roolia kaikki esiintymät. Kuitenkin tämä asetus ei enää ole suositeltavaa ja on saatavana vain tukemaan nykyisiä sovelluksia, jotka on muodostettu, voit käyttää sitä. Kaikki uudet kehittämiseen Käytä Azure Redis välimuistin.
>
> Azure hallitun välimuisti-palvelun ja Azure-roolin välimuistin tällä hetkellä tarkoitus 16 marraskuun-2016-käytöstä poistaminen.
On suositeltavaa siirtää Azure Redis välimuistin, valmistelussa tämän käytöstä poistaminen. Katso lisätietoja seuraavasta sivun   [Azure Redis välimuistin tarjoaa ja koon kannattaa käyttää?](redis-cache/cache-faq.md#what-redis-cache-offering-and-size-should-i-use) Microsoft-sivustossa.


### <a name="features-of-redis"></a>Redis.txt ominaisuudet

 Redis.txt on enemmän kuin yksinkertainen välityspalvelin. Laaja komentoja, joka tukee monia yleisiä tilanteita, joissa tarjoaa hajautettu ladatun tietokanta. Nämä on artikkelissa jäljempänä tässä asiakirjassa-osan käyttäminen Redis välimuistiin. Tässä osassa on yhteenveto Redis.txt parantavia ominaisuuksia.

### <a name="redis-as-an-in-memory-database"></a>Ladatun tietokantana Redis

Redis.txt tukee sekä lukeminen ja kirjoittaminen toimintoja. Valitse Redis.txt kirjoituksia voidaan suojata järjestelmävirheen joko tallentaminen säännöllisesti paikallisen tilannevedoksen tiedoston tai vain liittäminen lokitiedoston. Tämä ei ole useita tallentaa (jotka tulisi ottaa huomioon näyttöominaisuusvaihtoehdon Microsoft Data) kirjainkokoa.

 Kaikki kirjoituksia ovat asynkroninen ja estä asiakkaiden lukeminen ja kirjoittaminen. Kun Redis käynnissä käynnistyy, se lukee tiedot tilannevedoksen tai log-tiedostosta ja käyttää sitä ladatun välimuistin muodostaminen. Lisätietoja on artikkelissa Redis.txt-sivustossa [Redis pysyvyys](http://redis.io/topics/persistence) .

> [AZURE.NOTE] Redis.txt takaa, että kaikki kirjoituksia tallennetaan Jos kriittinen virhe, mutta samalla huonoin voi hävitä vain muutaman sekunnin enemmän tietoja. Muista, että välimuistia ei ole tarkoitus toimia tärkeimpien tietolähteeksi ja se on vastuussa käyttävät välimuistin varmistaaksesi, että tärkeät tiedot on tallennettu onnistuneesti tarvittavat tiedot-kaupan sovellukset. Lisätietoja on artikkelissa [välimuistin kesantoala mallia](http://msdn.microsoft.com/library/dn589799.aspx).

#### <a name="redis-data-types"></a>Redis tietotyypit

Redis.txt on avain-arvo-säilö, jossa arvot voi olla yksinkertainen tiedostotyypit ja monimutkaisten tietojen rakenteet hajautusarvot, luettelot, kuten ja määrittää. Se tukee seuraavia tietotyyppejä atomisia toimenpiteet. Näppäimet voi olla pysyvä tai merkittynä--elinaika, jolloin avain ja sen vastaavan arvon poistetaan automaattisesti välimuistin rajoitetun kanssa. Lisätietoja Redis.txt avaimet ja arvot käy sivulla [Johdanto tietotyyppeihin ja vedenotto Redis](http://redis.io/topics/data-types-intro) Redis.txt-sivustossa.

#### <a name="redis-replication-and-clustering"></a>Redis.txt replikointi ja klusterointi

Redis.txt tukee perustyyli/alaisen replikointia avulla varmistetaan ja ylläpitää siirtonopeuden. Kirjoita Redis.txt perusmuodon solmu toimintojen, replikoida vähintään yksi alisteiset solmujen. Luku-toimintoja voidaan served perusmuodon tai alaiset.

Jos verkko-osion alaiset jatkaa yhteyshenkilönä tiedot ja sitten läpinäkyvä synkronointi uudelleen perusmuodon kanssa kun yhteys muodostetaan. Lisätietoja käy [replikoinnin](http://redis.io/topics/replication) sivulla Redis.txt-sivustossa.

Redis.txt sisältää myös klusterointi, avulla voit läpinäkyvä osion tietojen tuominen shards palvelinten ja kerro Lataa. Tämä ominaisuus parantaa skaalattavuus, koska uudet Redis.txt palvelimet voidaan lisätä samanaikaisesti ja kasvattaa osioidaan koon, välimuistin tiedot.

Lisäksi kunkin palvelimen klusterin voi replikoida perustyyli/alaisen replikoinnin avulla. Näin varmistat käytettävyys klusterin kunkin solmun yli. Lisätietoja klusterointi ja sharding käy [Redis klusterin opetusohjelman sivun](http://redis.io/topics/cluster-tutorial) Redis.txt-sivustossa.

### <a name="redis-memory-use"></a>Redis muistin käyttö

Redis.txt välimuistin on rajallinen koko, joka määräytyy isäntätietokoneen resurssit. Kun määrität Redis.txt palvelimeen, voit määrittää muistin, se voi käyttää enimmäismäärän. Voit myös määrittää näppäintä Redis.txt välimuistiin on vanheneminen-ajan, minkä jälkeen ne poistetaan automaattisesti välimuistista. Tämä ominaisuus voi auttaa estämään ladatun välimuistin täyttämisen vanha tai vanhentuneita tietoja.

Kun muistin täyttyy, Redis.txt voit automaattisesti Poista näppäimet ja niiden arvojen määrän käytäntöjä seuraamalla. Oletusarvo on LRU (vähintään viimeksi käytetyt), mutta voit myös valita muita käytäntöjä, kuten evicting näppäimet satunnaisesti tai poistaminen käytöstä eviction kokonaan (, joka palvelupyynnön yrittää kohteiden lisääminen välimuistin epäonnistuu, jos se on täynnä). [Käyttämällä Redis kuin LRU välimuistin](http://redis.io/topics/lru-cache) sivulla on lisätietoja.

### <a name="redis-transactions-and-batches"></a>Redis tapahtumat ja erissä

Redis.txt mahdollistaa asiakassovellus lähettää toiminnoista, joita lukeminen ja kirjoittaminen tietojen välimuistin atomisia tapahtumana sarjaa. Kaikki komennot tapahtuman on varmasti suorittaa peräkkäin ja komentoja ei ollut myöntänyt muut samanaikaiset asiakkaat punottu niiden välille.

Näitä ei kuitenkaan tosi tapahtumat relaatiotietokannasta suorittaa ne. Tapahtumien käsittely kuuluu kaksi vaihetta--ensimmäinen on komennot ovat jonossa ja toinen on komentoja suoritettaessa. Komennon queuing vaiheessa komennot, jotka muodostavat tapahtuman lähetetään asiakas. Tarkoitus virheen ilmetessä tässä vaiheessa (esimerkiksi Syntaksivirhe tai virheellinen määrä parametreja) sitten Redis.txt ei käsitellä koko tapahtuman ja hylkää se.

Suorita vaiheessa Redis.txt suorittaa kunkin jonossa komennon järjestyksessä. Jos komento epäonnistuu tässä vaiheessa, Redis.txt jatkuu jonossa seuraava komento ja palauttaminen ei ole komennot, jotka olet jo suorittanut tehosteita. Tämän yksinkertaistettu lomakkeen tapahtuman auttaa suorituskyky ja välttää suorituskykyongelmat, jotka aiheutuvat ristiriita.

Redis.txt Toteuta Optimistinen lukitusta auttamaan ylläpito yhdenmukaisuuden lomakkeen. Lisätietoja tapahtumien ja lukitseminen ja Redis.txt käy [tapahtumat-sivulla](http://redis.io/topics/transactions) Redis.txt-sivustossa.

Redis.txt tukee myös muiden tapahtumien jonottaminen pyyntöihin. Redis.txt protokolla, asiakkaiden avulla voit lähettää komentoja Redis.txt palvelimen avulla asiakas voi lähettää toimintojen sarjan pyynnössä osana. Tämä voi auttaa vähentämään paketin pirstoutuminen verkossa. Kun erän käsitellään, jokaisen komennon suoritetaan. Jos jokin näistä komennoista on virheellinen, ne hylätään (joka ei tapahdu, tapahtuma), mutta muut komennot suoritetaan. On myös ei tietoja siinä järjestyksessä, jossa erän komennot käsitellään takaa.

### <a name="redis-security"></a>Redis suojaus

Redis.txt tarkoituksena laskettuna on nopea tietojen käytön ja on suunniteltu toimimaan luotettujen ympäristössä, joka voi käyttää vain luotettavien asiakkaiden sisällä. Redis.txt tukee rajoitettu suojausmalli salasanan todennusta perusteella. (Se on mahdollista käyttöoikeuksien poistaminen kokonaan, vaikka tämä ei ole suositeltavaa.)

Kaikki todennetut asiakkaat jakaa saman yleinen salasanan ja käyttää samoja resursseja. Jos tarvitset monipuolisemman kirjautumisen suojaus, käyttöön oman suojauksen kerroksen Redis.txt palvelimen eteen ja kaikki olisi kulkevat tämän kerroksen. Redis.txt olisi ei voi suoraan luovuttaa epäluotettavista tai Todentamattomille asiakkaille.

Voit rajoittaa komentoja, access poistaa ne käytöstä tai nimeämällä (ja antamalla vain sellaisten asiakkaiden uudet nimet).

Redis.txt ei tue suoraan tai salauksen, jotta kaikki koodaus on suoritettava asiakassovelluksiin. Lisäksi Redis.txt ei tarjoa tai transport suojaus. Jos haluat suojata tiedot, kun se kulkee verkossa, on suositeltavaa käyttöönoton SSL-välityspalvelimen.

Lisätietoja on lisätietoja [Redis suojaus](http://redis.io/topics/security) -sivulla Redis.txt-sivustossa.

> [AZURE.NOTE] Azure Redis välimuistin on oma suojaus kerroksen, jonka kautta asiakastietokoneet muodostavat yhteyden. Yleisen verkon eikä niitä julkaista pohjana Redis.txt-palvelimiin.

### <a name="using-the-azure-redis-cache"></a>Azure Redis välimuistin käyttäminen

Välimuistin Azure Redis pääsee Redis.txt-palvelimessa on osoitteessa Azure palvelinkeskuksen; tietueita palvelimissa se toimii ulkoseinä, joka sisältää käyttöoikeuksien hallinta ja suojaus. Voit valmistella välimuistin Azure hallinta-portaalin avulla. Portaalissa on useita esimääritettyjä määritykset, vakiovalikoimasta suoritetaan erillinen palvelu, joka tukee SSL viestintää (privacy) ja perustyyli/alaisen replikointia SLA 99,9 % saatavuuden alaspäin 250 Megatavun välimuistin ilman toistoa (ei käytettävissä oikeudet) jaetun laitteiston käytössä 53 Gigatavua välimuistia.

Käytä Azure hallinta-portaalissa, voit myös välimuistin eviction-käytännön määrittäminen ja hallita välimuistiin lisäämällä kyseiset käyttäjät annettu; roolit Osallistuja, omistaja Reader. Rooli määrittää toiminnoista, joita jäsenet voivat suorittaa. Esimerkiksi omistaja-roolin jäsenillä on täydet oikeudet (mukaan lukien suojaus) välimuistin ja sen sisällön, osallistujan roolin jäsenet voivat lukea ja kirjoittaa tiedot välimuistin ja Lukija-roolin vain voit hakea tietoja välimuistista.

Useimmat järjestelmänvalvojan tehtävät suoritetaan palvelun Azure hallinta-portaalissa ja tästä syystä monet käytettävissä Redis.txt perusversiota järjestelmänvalvojan komennot ovat ei ole käytettävissä, mukaan lukien pätevyys muokata kokoonpanoa ohjelmallisesti Sammuta Redis.txt-palvelimeen, Määritä muita slaves tai asiakasistuntojen Tallenna tiedot levylle.

Azure hallinta-portaalissa on kätevä graafisen, jonka avulla voit valvoa suorituskykyä välimuistin. Esimerkiksi voit tarkastella tehdään yhteyksien määrä, suorittaa pyyntöjen määrä lukee ja kirjoittaa, äänenvoimakkuuden ja välimuistin määrän käynnit välimuistin epäonnistumisten ja. Näiden tietojen, voit määrittää välimuistin tehokkuuden ja jos tarvittaessa siirtyä eri määritysten tai muuta eviction käytännön avulla. Lisäksi voit luoda ilmoituksia, jotka lähettävät sähköpostiviestejä järjestelmänvalvoja, jos vähintään yksi kriittinen arvot odotetun alueen ulkopuolella. Esimerkiksi jos välimuistin epäonnistumisten määrä ylittää määritetyn arvon edellisen tunnin aikana, järjestelmänvalvoja voi saada ilmoituksen, kun välimuisti voi olla pieni tai tiedot voivat olla käytössä poistaa liian nopeasti.

Voit valvoa suorittimen ja muistin verkon käyttö välimuistin.

Käy lisätietojen ja esittää, kuinka voit luoda ja määrittää Azure Redis välimuistin esimerkkejä sivun [ympäri Azure Redis välimuistin lantio](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) Azure blogista.

## <a name="caching-session-state-and-html-output"></a>Välimuistiin istunnon tila ja HTML-tiedot

Jos olet rakentaminen ASP.NET web-sovellusten, suorittaa käyttämällä Azure web roolit, voit tallentaa istunnon tilatietojen ja HTML-tulostuksen Azure Redis välimuistissa. Azure Redis välimuistin istunnon tila-palvelun avulla voit jakaa istunnon tietoja ASP.NET web-sovelluksen eri esiintymien välillä ja on erittäin hyödyllinen web klusterin tilanteissa, jossa asiakkaan ja palvelimen affiniteetti ei ole käytettävissä ja välimuistiin istunnon tietoja muistissa eivät ehkä sovellu.

Käyttäminen istunnon tila-palveluntarjoajan Azure Redis välimuistin toimittaa useita hyötyjä, mukaan lukien:

- Se jakaa istunnon tila on runsaasti ASP.NET web-sovelluksen, skaalattavuus, jotka tarjoavat esiintymät toiseen
- Se tukee useita lukijat ja yhden kirjoittajan hallitun, samanaikainen saman istunnon tila-tietojen käytön ja
- Se säästää muistia ja parantaa verkon suorituskykyyn pakkauksen avulla.

Lisätietoja käy [ASP.NET-istunnon tila palvelu Azure Redis välimuisti](redis-cache/cache-aspnet-session-state-provider.md) -sivulla Microsoft-sivustossa.

> [AZURE.NOTE] Älä käytä istunnon tila-palveluntarjoajan Azure Redis välimuistin ASP.NET-sovelluksissa, jotka ulkopuolella Azure ympäristölle Suorita. Viive käyttäminen ulkopuolella Azure-välimuistin esiinny suorituskyvyn edut tiedot välimuistiin.

Vastaavasti Azure Redis välimuistin tulosteen välimuisti-palvelun avulla voit tallentaa ASP.NET web-sovelluksen luoma HTTP-vastaukset. Azure Redis välimuistin kanssa tulosteen välimuisti-palvelun avulla voit parantaa vastauksen ajat sovellusten, joista hahmonnetaan monimutkaisia HTML-tiedot; Palvelusovellusten esiintymät luotaessa samalla vastaukset tehdä käyttö jaetun tulosteen osat välimuistin sen sijaan, että luodaan tämä HTML tulosteen uudelleen.  Lisätietoja käy [ASP.NET-tulostus välimuistin palvelu Azure Redis välimuistin](redis-cache/cache-aspnet-output-cache-provider.md) -sivulla Microsoft-sivustossa.

### <a name="azure-redis-cache"></a>Azure Redis.txt välimuisti

Azure Redis välimuistin tarjoaa, joita isännöidään osoitteessa Azure palvelinkeskuksen Redis.txt-palvelimiin. Se toimii ulkoseinä, joka sisältää käyttöoikeuksien hallinta ja suojaus. Voit valmistella välimuistin mukaan Azure-portaalissa.

Portaalissa on useita esimääritettyjä määrityksiä. Nämä vaihtelevat yksinkertaisista käynnissä erillinen palveluna 53 Gigatavua välimuistia, joka tukee SSL viestintää (privacy) ja perustyyli/alaisen replikointia SLA 99,9 % saatavuuden alaspäin 25 0 Mt-välimuistin, ilman toistoa (ei käytettävissä oikeudet) jaetun laitteisto-käyttöjärjestelmässä.

Käytä Azure portaalin, voit myös välimuistin eviction-käytännön määrittäminen ja hallita välimuistiin lisäämällä kyseiset käyttäjät annettu roolit.  Näistä toiminnoista, joita jäsenet voivat suorittaa määrittävät roolit ovat omistaja, avustaja ja Reader. Esimerkiksi omistaja-roolin jäsenillä on täydet oikeudet (mukaan lukien suojaus) välimuistin ja sen sisällön, osallistujan roolin jäsenet voivat lukea ja kirjoittaa tiedot välimuistin ja Lukija-roolin vain voit hakea tietoja välimuistista.

Useimmat järjestelmänvalvojan tehtävät suoritetaan palvelun Azure-portaalissa. Tästä syystä monia järjestelmänvalvojan Redis.txt perusversiota käytettävissä olevat komennot eivät ole käytettävissä, mukaan lukien pätevyys muokata kokoonpanoa ohjelmallisesti, Sammuta Redis.txt palvelimeen, Lisää alaiset määrittäminen ja Tallenna asiakasistuntojen levylle tiedot.

Azure-portaalissa on kätevä graafisen, jonka avulla voit valvoa suorituskykyä välimuistin. Voit esimerkiksi tarkastella tehdään tehdä pyyntöjen määrä, lukee ja kirjoittaa, äänenvoimakkuuden yhteyksien määrä ja välimuistin epäonnistumisten ja osumien määrä. Näiden tietojen avulla voit määrittää välimuistin tehokkuuden ja tarvittaessa siirtyä eri määrittäminen tai muuttaminen eviction käytännön.

Lisäksi voit luoda ilmoituksia, jotka lähettävät sähköpostiviestejä järjestelmänvalvoja, jos vähintään yksi kriittinen arvot odotetun alueen ulkopuolella. Voit esimerkiksi haluta ilmoittamaan järjestelmänvalvojan Jos välimuistin epäonnistumisten määrä ylittää määritetyn arvon edellisen tunnin, koska se tarkoittaa sitä, välimuistin voi olla pieni tai tietoja voi olla käytössä poistaa liian nopeasti.

Voit valvoa suorittimen, muistin ja verkon käyttö välimuistin.

Käy lisätietojen ja esittää, kuinka voit luoda ja määrittää Azure Redis välimuistin esimerkkejä sivun [ympäri Azure Redis välimuistin lantio](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) Azure blogista.

## <a name="caching-session-state-and-html-output"></a>Välimuistiin istunnon tila ja HTML-tiedot

Jos luot ASP.NET web-sovellusten, suorittaa käyttämällä Azure web roolit, voit tallentaa istunnon ilmoitettava tiedot ja Azure Redis välimuistin HTML-tiedot. Azure Redis välimuistin istunnon tila-palvelun avulla voit jakaa istunnon tietoja ASP.NET web-sovelluksen eri esiintymien välillä ja on erittäin hyödyllinen web klusterin tilanteissa, jossa asiakkaan ja palvelimen affiniteetti ei ole käytettävissä ja välimuistiin istunnon tietoja muistissa eivät ehkä sovellu.

Käyttäminen istunnon tila-palveluntarjoajan Azure Redis välimuistin toimittaa useita hyötyjä, mukaan lukien:

- Jakaminen on runsaasti ASP.NET web-sovellusten esiintymiä istunnon tila.
- Tarjoaa skaalattavuus.
- Tukemaan useita lukijat ja yhden kirjoittajan valvottu samanaikainen käyttää samoja istunnon tilan tietoja.
- Pakkaaminen säästää muistia ja parantaa verkon suorituskykyyn.

Lisätietoja on lisätietoja [ASP.NET-istunnon tilan palvelu Azure Redis välimuistin](redis-cache/cache-aspnet-session-state-provider.md) -sivulla Microsoft-sivustossa.

> [AZURE.NOTE] Älä käytä istunnon tila-palveluntarjoajan Azure Redis välimuistin ASP.NET-sovellukset, jotka ulkopuolella Azure ympäristölle Suorita. Viive käyttäminen ulkopuolella Azure-välimuistin esiinny suorituskyvyn edut tiedot välimuistiin.

Vastaavasti Azure Redis välimuistin tulosteen välimuisti-palvelun avulla voit tallentaa ASP.NET web-sovelluksen luoma HTTP-vastaukset. Tulosteen välimuisti-palvelun avulla Azure Redis välimuistin ja parantaa vastauksen ajat sovellusten, joista hahmonnetaan monimutkaisia HTML-tiedot. Palvelusovellusten esiintymät, joka luo samanlainen vastaukset tehdä käyttäminen jaetun tulosteen osat välimuistin sen sijaan, että luodaan tämä HTML tulosteen uudelleen. Lisätietoja on lisätietoja Microsoft-sivuston [ASP.NET-tulostus välimuistin palvelu Azure Redis välimuistin](redis-cache/cache-aspnet-output-cache-provider.md) -sivulla.

## <a name="building-a-custom-redis-cache"></a>Mukautetun Redis.txt välimuistin muodostaminen

Azure Redis välimuistin toimii ulkoseinä pohjana Redis.txt-palvelimiin. Tällä hetkellä se tukee kiinteä määritykset, mutta ei tarjoa Redis.txt klusterointi. Jos asetat lisämäärityksiä, joka ei kuulu Azure Redis välimuistin (kuten välimuistin on suurempi kuin 53 gt) voit luoda ja isännöidä Redis.txt-palvelimia käyttämällä Azuren näennäiskoneiden.

Tämä on mahdollisesti monimutkainen prosessi, koska joudut ehkä luoda useita VMs edustajana pää- ja alitehtäviä solmujen, jos haluat ottaa replikoinnin. Lisäksi jos haluat luoda klusterin, sitten on useita perustyylejä ja alitehtäviä palvelimiin. Mahdollisimman vähän liitetty replikoinnin topologian, joka sisältää hyvin aste saatavuudesta ja skaalattavuus käsittää vähintään kuusi VMs järjestetään kolme paria perustyyli/alaisen servers (klusteriin on oltava vähintään kolme perusmuodon solmujen).

Kunkin perustyyli/alaisen pari olisi sijaitsevat lähellä toisiaan Pienennä viive. Kukin ehtojoukko paria voit kuitenkin käynnissä-eri Azure palvelinkeskusten sijaitsevat eri alueilla, jos haluat etsiä lähellä sovellukset, jotka todennäköisesti sitä käytetään välimuistiin tallennetut tiedot. Microsoft-sivuston sivulle [Käynnissä Redis-CentOS Linux-AM Azure-tietokannassa](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) käydään läpi esimerkki, joka näyttää, miten voit luoda ja määrittää Redis.txt solmu Azure-AM käynnissä.

[AZURE.NOTE] Huomaa, että jos otat Redis.txt-välimuistin tällä tavalla, olet vastuussa valvonta, hallinta ja suojaaminen palvelu.

## <a name="partitioning-a-redis-cache"></a>Jakaminen Redis.txt välimuisti

Jakaminen välimuistin liittyy välimuistin jakaminen useiden tietokoneiden välillä. Tämä rakenne käyttöösi useita etuja yksittäisen välimuisti-palvelimen käyttäminen mukaan lukien:

- Luominen välimuistiin, on paljon suurempi kuin voidaan tallentaa yhteen palvelimeen.
- Tietojen jakaminen palvelinten, käytettävyyden parantaminen. Jos yksi palvelin epäonnistuu, tai se ei voi käyttää, tiedot, jotka se pitää ei ole käytettävissä, mutta tiedot jäljellä palvelimissa, jotka yhä voi käyttää. Välimuisti-näin ei ole tärkeä koska välimuistiin tallennetut tiedot ovat vain tiedot, joita pidetään tietokannan lyhytkestoisia kopio. Palvelimessa, jota ei voi käyttää välimuistiin tallennettuja tietoja voidaan välimuistin eri palvelimessa sen sijaan.
- Lataa levittäminen palvelinten, siten parantaa suorituskyky ja skaalattavuus.
- Geolocating tietojen Sulje käyttäjille, jotka käyttää sitä, mikä pienentää viive.

Välimuistiin yleisin jakaminen on sharding. Tämä strategia kunkin osion (tai shard) on oma Redis.txt välimuistin. Tietoja ohjataan tiettyyn osioon käyttämällä sharding logiikan, jonka voit jakaa tietoja monin eri tavoin. [Sharding kuvio](http://msdn.microsoft.com/library/dn589797.aspx) on lisätietoja käyttöönoton sharding.

Toteuttamisesta jakaminen Redis.txt välimuistiin, voit tehdä jonkin seuraavista tavoista:

- _Palvelinpuolen kyselyn reititys._ Tällä menetelmällä asiakassovellus lähettää pyynnön mihin tahansa Redis.txt palvelimissa, jotka muodostavat välimuistin (todennäköisesti lähin palvelin). Kunkin Redis.txt palvelimen metatiedot, joka kuvaa, että se sisältää ja on myös tietoa siitä, mitkä osiot sijaitsevat muilla palvelimilla osio. Redis.txt palvelimen käy läpi pyyntö. Jos se voi ratkaista paikallisesti, se suorittaa toimintoa. Muussa tapauksessa se välittää pyynnön voin haluamasi palvelin. Tämän mallin Redis.txt klusterointi täytäntöön ja on kuvattu tarkemmin [Redis klusterin opetusohjelma](http://redis.io/topics/cluster-tutorial) Redis.txt-sivusto-sivulla. Redis.txt klusterointi on läpinäkyvä asiakassovelluksiin ja muut Redis.txt-palvelimet voidaan lisätä klusterin (ja tiedot uudelleen osioitu) tarvitsematta asiakkaat määrittäminen uudelleen.

- _Asiakkaan jakaminen._ Tämän mallin asiakassovellus sisältää funktioiden (mahdollisesti kirjaston lomakkeen), joka reitittää pyynnöt tarvittavat Redis.txt palvelimeen. Tämän menetelmän voidaan käyttää Azure Redis välimuistin. Luo useita Azure Redis tallentaa (yksi vuoden jokaiselle tiedot-osio) ja toteuttaa asiakkaan logiikan, joka reitittää pyynnöt oikea välimuistiin. Jos osiointimallin muutoksia (Jos muita Azure Redis tallentaa on luotu esimerkiksi), asiakassovellukset on ehkä määritettävä uudelleen.

- _Välityspalvelimen keskuksen kautta jakaminen._ Tämä malli asiakkaan sovellusten lähettäminen pyytää välittävät välityspalvelu joka ymmärtää, kuinka tiedot on osioitu ja reitittää pyynnön sopivan Redis palvelimeen. Tämän menetelmän voidaan käyttää myös Azure Redis välimuistin; välityspalvelimen palvelun voidaan toteuttaa kuin Azure pilvipalvelussa. Tämän menetelmän edellyttää monimutkaisuus Toteuta palvelua Lisää taso ja pyynnöt saattaa kestää kauemmin kuin käyttää asiakkaan jakaminen suorittamiseen.

Sivun [osioimisen: Voit jakaa tietoja eri Redis.txt esiintymissä kesken](http://redis.io/topics/partitioning) Redis.txt-sivustossa on lisätietoja käyttöönoton jakaminen Redis.txt kanssa.

### <a name="implement-redis-cache-client-applications"></a>Toteuta Redis.txt välimuistin asiakassovellukset

Redis tukee asiakassovelluksissa kirjoitettu monia ohjelmoinnin kielillä. Jos kokoat uusien sovellusten käyttämällä .NET Framework-suositellaan on StackExchange.Redis asiakas-kirjasto. Tämä kirjasto sisältää .NET Framework-objektimallia, joka käsittelee tietoja yhteyden muodostamisesta Redis.txt palvelimeen, komentoja lähettäminen ja vastaanottaminen vastaukset. Se on käytettävissä Visual Studiossa NuGet-pakettina. Voit käyttää samaa kirjaston Azure Redis välimuistin tai mukautetun Redis.txt-välimuistin isännöimät AM.

Redis.txt palvelimeen käytät staattisen `Connect` menetelmää `ConnectionMultiplexer` luokka. Tämä menetelmä luo haluamasi yhteys on suunniteltu, jota käytetään koko asiakassovellus elinkaaren ja samaa yhteyttä voidaan käyttää useita samanaikaisia viestiketjuissa siirtyminen. Älä yhdistä ja Katkaise yhteys aina, kun teet Redis.txt toimen, koska tämä heikentää suorituskykyä.

Voit määrittää yhteyden parametreja, kuten osoite Redis.txt isännän ja salasana. Jos käytössäsi on Azure Redis välimuistin, salasana on joko ensisijaisen tai toissijaisen avainta, joka on luotu käyttämällä Azure hallinta-portaalin Azure Redis välimuistin.

Kun olet yhdistänyt Redis.txt-palvelimeen, voit hankkia Redis.txt-tietokantaan, joka toimii välimuistin kahvaa. Redis.txt-yhteyden avulla `GetDatabase` menetelmä toiminto. Voit hakea kohteita välimuistista ja tietojen tallentaminen välimuistiin avulla `StringGet` ja `StringSet` tavoista. Näistä tavoista odotetusti näppäintä parametrina ja palauttaa kohteen joko välimuistin, jossa on vastaava arvo (`StringGet`) tai kohteen lisääminen välimuistiin avaimeen (`StringSet`).

Redis.txt palvelimen sijainnin mukaan monien toimintojen voi syntyä joitakin viive aikana palvelimeen lähetetään pyynnön ja vastauksen palautetaan asiakkaalle. StackExchange kirjasto sisältää monia menetelmiä, jotka se paljastaa avulla asiakassovellukset pysyvät vastaa asynkroninen versiot. Näistä tavoista tue [Tehtävien asynkroninen kuvion](http://msdn.microsoft.com/library/hh873175.aspx) .NET Framework.

Seuraavat koodikatkelman näkyy nimeltä menetelmän `RetrieveItem`. Se on kuvattu välimuistin kesantoala kuvion perusteella Redis.txt ja StackExchange kirjasto toteutuksen. Menetelmä avaimen merkkijonoarvo ja yrittää hakea vastaavaan kohteeseen Redis.txt välimuistista soittamalla `StringGetAsync` menetelmä (asynkroninen version `StringGet`).

Jos kohde ei löydy, se haeta pohjana olevat tiedot-lähde-käyttämisen `GetItemFromDataSourceAsync` menetelmä (joka on paikallinen menetelmä ja ei-osa StackExchange kirjaston). Se lisätään välimuistin avulla `StringSetAsync` menetelmä, jotta se voi hakea nopeasti seuraavan kerran.

```csharp
// Connect to the Azure Redis cache
ConfigurationOptions config = new ConfigurationOptions();
config.EndPoints.Add("<your DNS name>.redis.cache.windows.net");
config.Password = "<Redis cache key from management portal>";
ConnectionMultiplexer redisHostConnection = ConnectionMultiplexer.Connect(config);
IDatabase cache = redisHostConnection.GetDatabase();
...
private async Task<string> RetrieveItem(string itemKey)
{
    // Attempt to retrieve the item from the Redis cache
    string itemValue = await cache.StringGetAsync(itemKey);

    // If the value returned is null, the item was not found in the cache
    // So retrieve the item from the data source and add it to the cache
    if (itemValue == null)
    {
        itemValue = await GetItemFromDataSourceAsync(itemKey);
        await cache.StringSetAsync(itemKey, itemValue);
    }

    // Return the item
    return itemValue;
}
```

`StringGet` Ja `StringSet` menetelmät eivät ole rajoitettu haetaan tai tallentaminen merkkijonoarvoa. Ne voivat käyttää kohteessa, joka on muuntaa sarjaksi matriisina tavujen määrä. Jos haluat tallentaa .NET-objektin, voit muuntaa sarjaksi kuin DBCS-muodossa ja käyttää `StringSet` tavan kirjoittaa välimuistiin.

Vastaavasti voit lukea objektin välimuistista käyttämällä `StringGet` menetelmä ja poistettaessa .NET-objekteina. Seuraava koodi näkyy tunniste menetelmiä IDatabase-liittymän joukko ( `GetDatabase` Redis.txt yhteyden menetelmä palauttaa `IDatabase` objekti), ja jotkin näistä tavoista lukemiseen ja kirjoittamiseen käyttävän mallikoodin `BlogPost` objektin välimuistiin:

```csharp
public static class RedisCacheExtensions
{
    public static async Task<T> GetAsync<T>(this IDatabase cache, string key)
    {
        return Deserialize<T>(await cache.StringGetAsync(key));
    }

    public static async Task<object> GetAsync(this IDatabase cache, string key)
    {
        return Deserialize<object>(await cache.StringGetAsync(key));
    }

    public static async Task SetAsync(this IDatabase cache, string key, object value)
    {
        await cache.StringSetAsync(key, Serialize(value));
    }

    static byte[] Serialize(object o)
    {
        byte[] objectDataAsStream = null;

        if (o != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {
                binaryFormatter.Serialize(memoryStream, o);
                objectDataAsStream = memoryStream.ToArray();
            }
        }

        return objectDataAsStream;
    }

    static T Deserialize<T>(byte[] stream)
    {
        T result = default(T);

        if (stream != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                result = (T)binaryFormatter.Deserialize(memoryStream);
            }
        }

        return result;
    }
}
```

Seuraava koodi on kuvattu nimeltä menetelmän `RetrieveBlogPost` , joka käyttää tunniste näistä tavoista lukemiseen ja kirjoittamiseen voi sarjoittaa `BlogPost` objektin välimuistiin seuraavan välimuistin kesantoala kaavaa:

```csharp
// The BlogPost type
[Serializable]
private class BlogPost
{
    private HashSet<string> tags = new HashSet<string>();

    public BlogPost(int id, string title, int score, IEnumerable<string> tags)
    {
        this.Id = id;
        this.Title = title;
        this.Score = score;
        this.tags = new HashSet<string>(tags);
    }

    public int Id { get; set; }
    public string Title { get; set; }
    public int Score { get; set; }
    public ICollection<string> Tags { get { return this.tags; } }
}
...
private async Task<BlogPost> RetrieveBlogPost(string blogPostKey)
{
    BlogPost blogPost = await cache.GetAsync<BlogPost>(blogPostKey);
    if (blogPost == null)
    {
        blogPost = await GetBlogPostFromDataSourceAsync(blogPostKey);
        await cache.SetAsync(blogPostKey, blogPost);
    }

    return blogPost;
}
```

Redis tukee komento pipelining Jos asiakassovellus lähettää useita asynkroninen pyynnöt. Redis.txt voit multiplex pyynnöt samaa yhteyttä sijaan vastaanottaminen ja niihin vastaaminen komennot tarkka järjestyksessä.

Tämän menetelmän auttaa vähentämään viive tekemällä verkon tehokkaampaa avulla. Seuraavat koodikatkelman näyttää esimerkin, joka hakee tietoja kaksi asiakkaiden samanaikaisesti. Koodin lähettää kaksi pyynnöt ja suorittaa sitten joitakin muita käsittely (ei näy) ennen vastaanottaa tulokset. `Wait` Välimuistin objektin menetelmää muistuttaa .NET Framework `Task.Wait` menetelmää:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
var task1 = cache.StringGetAsync("customer:1");
var task2 = cache.StringGetAsync("customer:2");
...
var customer1 = cache.Wait(task1);
var customer2 = cache.Wait(task2);
```

Microsoft-sivuston sivulle [Azure Redis välimuistin ohjeissa](https://azure.microsoft.com/documentation/services/cache/) on lisätietoja viestin kirjoittaminen Azure Redis välimuistin käyttävät asiakassovellukset. Lisätietoja on käytettävissä [peruskäyttö sivun](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) StackExchange.Redis-sivustossa.

Sivu samaa sivustoon [putkistot ja multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) on lisätietoja asynkronisten toimintojen ja pipelining Redis.txt ja StackExchange-kirjaston kanssa.  Tämän artikkelin avulla Redis tallentamisesta välimuistiin, on seuraavassa osassa on esimerkkejä joistain monimutkaisemman tekniikoita, joiden avulla voit käyttää tietoja, jotka säilytetään Redis.txt välimuistissa.

## <a name="using-redis-caching"></a>Käyttämällä Redis.txt välimuistiin tallentaminen

Redis.txt helpoin käyttäminen koskee välimuistiin tallentaminen on avain-arvo-pareina, joiden arvo on pituudeltaan binaarinen tietoja sisältävät tulkitsemattoman merkkijonona. (Se on käytettävä matriisin, joka voidaan käsitellä merkkijonona tavua). Tässä skenaariossa on kuvattu osassa Ota käyttöön Redis välimuisti-asiakassovellusten tämän artikkelin.

Huomaa, että avaimet sisältävät tulkitsemattoman tiedot myös niin voit käyttää mitä tahansa tiedot avaimeksi. Pidempi avain on kuitenkin enemmän tilaa kestää tallentaa ja sitä kauemmin haku toimintojen suorittaminen kestää. Käytettävyyttä ja vaivatonta huollosta että keyspace suunnitella huolellisesti ja kuvaava (mutta ei yksityiskohtainen)-näppäimillä.

Esimerkiksi edustavat avaimen tunnus 100 yksinkertaisesti "100" sijaan asiakkaan rakenteellisia näppäimet, kuten "asiakkaan: 100" avulla. Tämän mallin avulla voit helposti erottaa toisistaan, joihin voidaan tallentaa eri tietotyyppien arvoja. Esimerkiksi voi myös käyttää avain-tilaukset: 100"edustavan tilaus, jonka tunnus 100 näppäintä.

Yksiulotteinen binaarinen merkkijonoja, lukuun ottamatta Redis.txt avain-arvo-pari arvon myös mahtuu enemmän jäsennettyjä tietoja, mukaan lukien luettelot, määrittää (lajiteltu ja lajittelematon) ja tekstikopioon. Redis.txt on täydellinen komentoja, jotka voivat käsitellä tällaisia ja monia näitä komentoja voi käyttää .NET Framework-sovellukset, kuten StackExchange asiakkaan kirjaston kautta. Sivulla [Johdanto tietotyyppeihin ja vedenotto Redis](http://redis.io/topics/data-types-intro) Redis.txt-sivustossa on lisätietoja yleistä tällaisia ja komennot, joiden avulla voit käsitellä niitä.

Tässä osassa on koottu yleisiä Käytä joskus näiden tietotyyppien ja komennot.

### <a name="perform-atomic-and-batch-operations"></a>Suorita atomisia ja erä toiminnot

Redis.txt tukee atomisia get-ja set toimenpiteet merkkijonoarvoa sarjaa. Näitä toimintoja poistaminen saattaa kadota, kun käytät eri mahdollista kilpa vaarat `GET` ja `SET` komennot. Toiminnot, jotka ovat käytettävissä ovat seuraavat:

- `INCR`, `INCRBY`, `DECR`, ja `DECRBY`, joka suorittaa atomisia kokonaisluku numeeristen arvojen lisäys ja vähennys toimintoja. StackExchange kirjasto sisältää ylikuormitettu versiot `IDatabase.StringIncrementAsync` ja `IDatabase.StringDecrementAsync` suorittaa nämä toiminnot ja palauttaa tuloksen, joka on tallennettu välimuistin menetelmiä. Seuraavat koodikatkelman kuvataan, miten seuraavilla tavoilla:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  await cache.StringSetAsync("data:counter", 99);
  ...
  long oldValue = await cache.StringIncrementAsync("data:counter");
  // Increment by 1 (the default)
  // oldValue should be 100

  long newValue = await cache.StringDecrementAsync("data:counter", 50);
  // Decrement by 50
  // newValue should be 50
  ```

- `GETSET`, joka hakee arvon, joka liittyy avaimen ja muutoksista uusi arvo. StackExchange kirjasto on tämän toiminnon kautta `IDatabase.StringGetSetAsync` menetelmää. Alla olevassa koodikatkelman Esimerkki tätä menetelmää. Koodi palauttaa nykyisen arvon, joka liittyy avain "tietojen: laskuri" edellisessä esimerkissä käytetty mallitietokanta. Valitse se palauttaa avaimeen arvo nolla, kaikki osana samaa toimintoa:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  string oldValue = await cache.StringGetSetAsync("data:counter", 0);
  ```

- `MGET`ja `MSET`, jotka palauttavat tai muuttaa merkkijonoarvot joukko kuin yhdellä kertaa. `IDatabase.StringGetAsync` Ja `IDatabase.StringSetAsync` menetelmiä ylikuormitettu tukevat tätä toimintoa, kuten seuraavassa esimerkissä:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  // Create a list of key-value pairs
  var keysAndValues =
      new List<KeyValuePair<RedisKey, RedisValue>>()
      {
          new KeyValuePair<RedisKey, RedisValue>("data:key1", "value1"),
          new KeyValuePair<RedisKey, RedisValue>("data:key99", "value2"),
          new KeyValuePair<RedisKey, RedisValue>("data:key322", "value3")
      };

  // Store the list of key-value pairs in the cache
  cache.StringSet(keysAndValues.ToArray());
  ...
  // Find all values that match a list of keys
  RedisKey[] keys = { "data:key1", "data:key99", "data:key322"};
  RedisValue[] values = null;
  values = cache.StringGet(keys);
  // values should contain { "value1", "value2", "value3" }
  ```

Voit myös yhdistää useita toimintoja yhdeksi Redis.txt tapahtumaksi Redis.txt tapahtumia ja erissä osan tämän artikkelin ohjeiden mukaisesti. StackExchange kirjaston tukee tapahtumien kautta `ITransaction` käyttöliittymän.

Voit luoda `ITransaction` objektin avulla `IDatabase.CreateTransaction` menetelmää. Suoritat komennot tapahtuman myöntämä tavoilla `ITransaction` objekti.

`ITransaction` Käyttöliittymän pääsee, joka on kaltaisia käyttämien menetelmiä joukko `IDatabase` liittymän lukuun ottamatta sitä, että kaikki menetelmät ovat asynkroninen. Tämä tarkoittaa, että ne ovat vain suorittaa toiminnon `ITransaction.Execute` menetelmä käynnistetään. Arvo, joka palautetaan `ITransaction.Execute` menetelmä osoittaa tapahtuma on luotu (tosi) tai jos se ei onnistunut (EPÄTOSI).

Seuraavat koodikatkelman näyttää esimerkin kyseisen kerrallaan ja vähentää kahden laskureita saman tapahtuman osana:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
ITransaction transaction = cache.CreateTransaction();
var tx1 = transaction.StringIncrementAsync("data:counter1");
var tx2 = transaction.StringDecrementAsync("data:counter2");
bool result = transaction.Execute();
Console.WriteLine("Transaction {0}", result ? "succeeded" : "failed");
Console.WriteLine("Result of increment: {0}", tx1.Result);
Console.WriteLine("Result of decrement: {0}", tx2.Result);
```

Muista, että Redis.txt tapahtumat ovat toisin kuin relaatiotietokannasta tapahtumat. `Execute` Menetelmä jonot riittää, että kaikki komennot, jotka muodostavat tapahtuman suoritettavien ja jos halutut arvosarjat on Vääränlainen on pysäytetty tapahtuma. Jos kaikki komennot on onnistuneesti jonossa, jokaisen komennon suoritetaan asynkronisesti.

Jos minkä tahansa komennon epäonnistuu, muihin sarakkeisiin jatkaa edelleen käsittely. Jos haluat varmistaa, että komento on suoritettu onnistuneesti, on noutaa komennon tulokset vastaavat tehtävän **tuloksen** -ominaisuuden avulla, kuten edellä olevassa esimerkissä. Luettaessa **tulos** -ominaisuuden estää puheluja viestiketjun, ennen kuin tehtävä on valmis.

Lisätietoja [Redis.txt tapahtumat](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) -sivulla StackExchange.Redis-sivustossa.

Kun suorittamiseen erä toiminnot, voit käyttää `IBatch` käyttöliittymän StackExchange kirjaston. Tämän liittymän pääsee menetelmiä kaltaisia käyttämien joukko `IDatabase` liittymän lukuun ottamatta sitä, että kaikki menetelmät ovat asynkroninen.

Voit luoda `IBatch` objektin avulla `IDatabase.CreateBatch` menetelmä ja suorita erän avulla `IBatch.Execute` menetelmä, kuten seuraavassa esimerkissä. Tämä koodi yksinkertaisesti määrittää merkkijonoarvo, kerrallaan ja vähentää edellisessä esimerkissä käytetty saman laskureita, ja näyttää tulokset:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
IBatch batch = cache.CreateBatch();
batch.StringSetAsync("data:key1", 11);
var t1 = batch.StringIncrementAsync("data:counter1");
var t2 = batch.StringDecrementAsync("data:counter2");
batch.Execute();
Console.WriteLine("{0}", t1.Result);
Console.WriteLine("{0}", t2.Result);
```

On tärkeää ymmärtää, että toisin kuin tapahtuman, jos erän komennon epäonnistuu, koska se on virheellinen, muut komennot voivat silti suorittaa. `IBatch.Execute` Menetelmä ei palauta maininta onnistumisesta tai epäonnistumisesta.

### <a name="perform-fire-and-forget-cache-operations"></a>Suorittaa fire ja unohda välimuistin toiminnot

Redis tukee fire ja unohda toimintojen komento lippujen avulla. Tässä tilanteessa asiakkaan vain käynnistää toiminnon, mutta tulos ei ole etua ja odottaa tehtävä-komennon. Alla olevassa esimerkissä fire kuin INCR-komennon suorittaminen ja unohdat toiminto:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
await cache.StringSetAsync("data:key1", 99);
...
cache.StringIncrement("data:key1", flags: CommandFlags.FireAndForget);
```

### <a name="specify-automatically-expiring-keys"></a>Määritä automaattisesti vanhentuvien näppäimet

Kun tallennat kohteen Redis.txt välimuistiin, voit määrittää aikakatkaisu, minkä jälkeen kohde poistetaan automaattisesti välimuistista. Voit tehdä myös kyselyn, miten paljon aikaa näppäintä on ennen kuin se vanhentuu avulla `TTL` komento. Tämä komento on käytettävissä StackExchange-sovellusten avulla `IDatabase.KeyTimeToLive` menetelmää.

Seuraavat koodikatkelman näytetään, miten voit määrittää 20 sekunnin ajan vanheneminen-näppäintä ja kyselyn avain jäljellä elinkaaren:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration time of 20 seconds
await cache.StringSetAsync("data:key1", 99, TimeSpan.FromSeconds(20));
...
// Query how much time a key has left to live
// If the key has already expired, the KeyTimeToLive function returns a null
TimeSpan? expiry = cache.KeyTimeToLive("data:key1");
```

Voit myös määrittää päättymisaika tietyn päivämäärän ja kellonajan käyttämällä VANHENTUMISPÄIVÄMÄÄRÄ-komento, joka on käytettävissä StackExchange kirjastossa `KeyExpireAsync` menetelmää:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration date of midnight on 1st January 2015
await cache.StringSetAsync("data:key1", 99);
await cache.KeyExpireAsync("data:key1",
    new DateTime(2015, 1, 1, 0, 0, 0, DateTimeKind.Utc));
...
```

> _Vihje:_ Voit poistaa kohteen välimuistin manuaalisesti käyttämällä DEL-komento, joka on saatavana StackExchange kirjastossa `IDatabase.KeyDeleteAsync` menetelmää.

### <a name="use-tags-to-cross-correlate-cached-items"></a>Tunnisteiden käyttäminen rajat-korreloinnin välimuistiin tallennetut kohteet

Redis.txt joukko on kokoelma useita kohteita, jotka jakavat yksittäistä avainta. Voit luoda joukon SADD-komennon avulla. Voit hakea joukon kohteiden SMEMBERS-komennon avulla. StackExchange kirjaston toteuttaa SADD-komento, jossa on `IDatabase.SetAddAsync` menetelmä ja SMEMBERS komennon kanssa `IDatabase.SetMembersAsync` menetelmää.

Voit myös yhdistää nykyisten tietojoukkojen voit luoda uuden joukot SDIFF (Aseta ero), SINTTERI (Aseta leikkaus) ja SUNION (Aseta yhdiste)-komentoja. StackExchange kirjaston yhdistää yhtenäiseksi ohjeistukseksi nämä toiminnot- `IDatabase.SetCombineAsync` menetelmää. Tämän menetelmän ensimmäinen parametri ilmaisee määrittäminen toiminto suoritetaan.

Seuraava koodikatkelmat näyttää, miten joukkoja voi olla hyötyä nopeasti tallentaminen ja hakeminen muotokokoelmia, joiden liittyvät kohteet. Käyttää tätä koodia `BlogPost` tyyppi, joka on kuvattu tämän artikkelin osassa toteuttaminen Redis välimuisti-asiakassovelluksia.

A `BlogPost` objekti sisältää neljä kenttää – tunnus, otsikko, luokittelu tulos ja tunnisteet-apuohjelmista. Ensimmäinen alla koodikatkelman näkyy mallitietoja, jota käytetään täyttää C# luettelo `BlogPost` objekteja:

```csharp
List<string[]> tags = new List<string[]>()
{
    new string[] { "iot","csharp" },
    new string[] { "iot","azure","csharp" },
    new string[] { "csharp","git","big data" },
    new string[] { "iot","git","database" },
    new string[] { "database","git" },
    new string[] { "csharp","database" },
    new string[] { "iot" },
    new string[] { "iot","database","git" },
    new string[] { "azure","database","big data","git","csharp" },
    new string[] { "azure" }
};

List<BlogPost> posts = new List<BlogPost>();
int blogKey = 0;
int blogPostId = 0;
int numberOfPosts = 20;
Random random = new Random();
for (int i = 0; i < numberOfPosts; i++)
{
    blogPostId = blogKey++;
    posts.Add(new BlogPost(
        blogPostId,               // Blog post ID
        string.Format(CultureInfo.InvariantCulture, "Blog Post #{0}",
            blogPostId),          // Blog post title
        random.Next(100, 10000),  // Ranking score
        tags[i % tags.Count]));   // Tags--assigned from a collection
                                  // in the tags list
}
```

Voit tallentaa tunnisteet kunkin `BlogPost` objekti joukkona Redis.txt välimuistin ja kukin ehtojoukko liittäminen tunnus `BlogPost`. Näin voit etsiä nopeasti kaikki tunnisteet, jotka kuuluvat tietyn blogimerkinnän sovellus. Hakeminen päin ottaminen käyttöön ja Etsi kaikki, joilla on tietyn tunnisteen blogimerkintöjen, voit luoda toisen joukon, joka sisältää blogimerkinnät viittaava Tunnistetunnus-näppäintä:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Tags are easily represented as Redis Sets
foreach (BlogPost post in posts)
{
    string redisKey = string.Format(CultureInfo.InvariantCulture,
        "blog:posts:{0}:tags", post.Id);
    // Add tags to the blog post in Redis
    await cache.SetAddAsync(
        redisKey, post.Tags.Select(s => (RedisValue)s).ToArray());

    // Now do the inverse so we can figure how which blog posts have a given tag
    foreach (var tag in post.Tags)
    {
        await cache.SetAddAsync(string.Format(CultureInfo.InvariantCulture,
            "tag:{0}:blog:posts", tag), post.Id);
    }
}
```

Näiden rakenteiden avulla voit suorittaa useita yleisiä kyselyjen erittäin tehokkaasti. Voit etsiä ja näyttää kaikki tunnisteet blogimerkinnän 1 tältä:

```csharp
// Show the tags for blog post #1
foreach (var value in await cache.SetMembersAsync("blog:posts:1:tags"))
{
    Console.WriteLine(value);
}
```

Voit tarkistaa kaikki tunnisteet, jotka ovat yleisiä blogiin kirjaa 1 ja blogin kirjaa 2 suorittamalla määrittäminen leikkaus-toimintoa seuraavalla tavalla:

```csharp
// Show the tags in common for blog posts #1 and #2
foreach (var value in await cache.SetCombineAsync(SetOperation.Intersect, new RedisKey[]
    { "blog:posts:1:tags", "blog:posts:2:tags" }))
{
    Console.WriteLine(value);
}
```

Ja löydät, jotka sisältävät tietyn tunnisteen blogimerkintöjen:

```csharp
// Show the ids of the blog posts that have the tag "iot".
foreach (var value in await cache.SetMembersAsync("tag:iot:blog:posts"))
{
    Console.WriteLine(value);
}
```

### <a name="find-recently-accessed-items"></a>Etsi viimeksi käytetyt kohteet

Yleisiä tehtävän vaadittavat useita sovelluksia on löytää useimmin viimeksi käytetyt kohteet. Esimerkiksi sivuston blogin hyvä näyttää viimeksi luetun blogimerkintöjen tietoja.

Voit ottaa tämän toiminnon Redis.txt luettelon avulla. Redis.txt-luettelo sisältää useita kohteita, jotka käyttävät samaa avainta. Luettelon toimii kaksipäiseksi jonossa. Voit siirtää kohteita joko luettelon loppuun LPUSH (vasen painallus) ja RPUSH (oikea painallus) komentojen avulla. Kohteiden voit noutaa joko luettelon loppuun LPOP ja RPOP-komennoilla. Voit palata joukko elementtejä myös käyttämällä LRANGE ja Järjestä-komentoja.

Koodikatkelmat alla Näytä siitä, miten voit suorittaa nämä toiminnot käyttämällä StackExchange-kirjasto. Käyttää tätä koodia `BlogPost` edellisessä kohdassa käytettyä tyyppi. Kun blogikirjoitus on vain luku-käyttäjä, `IDatabase.ListLeftPushAsync` menetelmä Vie luettelo, joka liittyy avainta "blog:recent_posts" Redis.txt välimuistin sivulle blogimerkinnän otsikko.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:recent_posts";
BlogPost blogPost = ...; // Reference to the blog post that has just been read
await cache.ListLeftPushAsync(
    redisKey, blogPost.Title); // Push the blog post onto the list
```

Lisää blogimerkintöjen luetaan, kun niiden otsikoita siirretty sivulle samassa luettelossa. Luettelossa on tilattu sarja, johon on lisätty otsikot mukaan. Viimeksi luetun blogimerkintöjä on luettelon vasemmassa reunassa kohti. (Jos saman blogimerkinnän luetaan useammin kuin kerran, se on useita kohteita luettelosta.)

Voit näyttää viimeksi luetun viestien otsikot `IDatabase.ListRange` menetelmää. Tämä menetelmä kestää avainta, joka sisältää luettelon, pohjana ja Viimeinen piste. Seuraava koodi hakee 10 blogimerkintöjen (kohteiden 0-9) luettelon vasemmanpuoleisimmasta lopussa otsikot:

```csharp
// Show latest ten posts
foreach (string postTitle in await cache.ListRangeAsync(redisKey, 0, 9))
{
    Console.WriteLine(postTitle);
}
```

Huomaa, että `ListRangeAsync` menetelmä ei poista kohteita luettelosta. Voit tehdä tämän käyttämällä `IDatabase.ListLeftPopAsync` ja `IDatabase.ListRightPopAsync` tavoista.

Jos haluat estää luettelon kasvava toistaiseksi, voit teuraaksi säännöllisesti kohteet mukaan rajaaminen luettelossa. Alla olevassa koodikatkelman näytetään, miten voit poistaa kaikki mutta viiden vasemmanpuoleisimmasta kohteita luettelosta:

```csharp
await cache.ListTrimAsync(redisKey, 0, 5);
```

### <a name="implement-a-leader-board"></a>Toteuta Täytemerkki-taulusta

Oletusarvon mukaan joukon kohteiden järjestetään ole missään tietyssä järjestyksessä. Voit luoda järjestetyn joukon ZADD-komennolla ( `IDatabase.SortedSetAdd` menetelmä StackExchange kirjastossa). Kohteiden järjestetään numeerinen arvo, joka on nimeltään tulos, joka on annettu parametrina-komennon avulla.

Seuraavat koodikatkelman Lisää järjestetyn luettelon blogimerkinnän otsikko. Tässä esimerkissä kunkin blogimerkintä on tulos-kenttä, joka sisältää luvun sijoituksen blogimerkinnän.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:post_rankings";
BlogPost blogPost = ...; // Reference to a blog post that has just been rated
await cache.SortedSetAddAsync(redisKey, blogPost.Title, blogpost.Score);
```

Blogin viestin otsikot ja tulosten pistemäärän nousevasti avulla voit hakea `IDatabase.SortedSetRangeByRankWithScores` menetelmää:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(redisKey))
{
    Console.WriteLine(post);
}
```

> [AZURE.NOTE] StackExchange kirjasto sisältää myös `IDatabase.SortedSetRangeByRankAsync` menetelmä, joka palauttaa tuloksen tilauksen tiedot, mutta ei palauta tulokset.

Voit myös hakea kohteita laskevassa järjestyksessä tulosten ja muut parametrit antamalla palautettavien kohteiden määrän rajoittaminen `IDatabase.SortedSetRangeByRankWithScoresAsync` menetelmää. Seuraavassa esimerkissä näkyy otsikoita ja Ylin 10 luokiteltujen blogimerkintöjen tulosten:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(
                               redisKey, 0, 9, Order.Descending))
{
    Console.WriteLine(post);
}
```

Seuraavassa esimerkissä `IDatabase.SortedSetRangeByScoreWithScoresAsync` menetelmä, jonka avulla voit rajoittaa niihin, jotka kuuluvat määritetyn pistemäärän palautettavien kohteiden alue:

```csharp
// Blog posts with scores between 5000 and 100000
foreach (var post in await cache.SortedSetRangeByScoreWithScoresAsync(
                               redisKey, 5000, 100000))
{
    Console.WriteLine(post);
}
```

### <a name="message-by-using-channels"></a>Viestin kanavien avulla

Lukuun ottamatta visuaalisessa muodossa tietovälimuistin, Redis.txt-palvelin antaa messaging tehokas publisher/tilaajan järjestelmä, jonka kautta. Asiakassovellukset tilata kanavan ja muita sovelluksia ja palveluja julkaista viestejä kanava. Tilaamisen sovellukset sitten saavat viestit ja käsitellä niitä.

Redis.txt on TILAA-komennon avulla voit tilata kanavien asiakassovellukset. Tämä komento odottaa vähintään yksi kanavat, joina sovelluksen hyväksymään viestejä nimi. StackExchange kirjasto sisältää `ISubscription` -käyttöliittymä, joka mahdollistaa tilaa ja julkaise kanavien .NET Framework-sovellus.

Voit luoda `ISubscription` objektin avulla `GetSubscriber` yhteys palvelimeen Redis.txt-menetelmää. Sitten voit kuunnella viestien kanavan avulla `SubscribeAsync` objekti-menetelmää. Seuraava koodi-esimerkissä nimeltä "viestien: blogPosts" kanavan tilaaminen:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
await subscriber.SubscribeAsync("messages:blogPosts", (channel, message) =>
{
    Console.WriteLine("Title is: {0}", message);
});
```

Ensimmäinen parametri `Subscribe` tapa on kanavan nimi. Tämä nimi noudattaa samoja sääntöjä, jotka käyttävät välimuistin avaimet. Nimessä voi olla mikä tahansa binaaritietoja, on suositeltavaa käyttää suhteellisen lyhyt ja merkityksellinen merkkijonot varmistavat hyvän suorituskyvyn ja kunnossapidettävyys mutta.

Huomaa myös, että nimitilan, jota kanavien on erillinen näppäimet käyttämää. Tämä tarkoittaa voi olla kanavat ja avaimet, joilla on sama nimi, vaikka tämä saattaa vaikeuttaa sovelluksen koodin pitämään.

Toinen parametri on toiminto edustajalle. Tämä edustaja suoritetaan asynkronisesti aina, kun uusi viesti näkyy kanava. Tässä esimerkissä ainoastaan näyttää sanoman konsolissa (viesti sisältää blogimerkinnän otsikko).

Julkaiseminen kanava-sovelluksen komennolla Redis JULKAISE. StackExchange kirjasto sisältää `IServer.PublishAsync` menetelmä tehtävän suorittamiseen. Seuraava koodikatkelman näytetään, miten voit julkaista viestin "viestien: blogPosts" kanavan:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
BlogPost blogpost = ...;
subscriber.PublishAsync("messages:blogPosts", blogPost.Title);
```

On useita pitäisi ymmärtää Julkaise ja tilaa-järjestelmä tietoja:

- Useita tilaajille tilata samaa kanavaa, ja kaikki saavat viestit, jotka on julkaistu kyseisen kanavan.
- Vain tilaajille vastaanottaa viestejä, jotka on julkaistu, kun ne on tilattu. Kanavien ei ellei ja kun viesti on julkaistu, Redis.txt infrastruktuuri Vie viestin kunkin tilaajan ja poistaa sen.
- Viestit ovat oletusarvoisesti vastaanottanut siinä järjestyksessä, jossa ne lähetetään tilaajille. Suuri määrä viestejä sekä monia tilaajille ja julkaisijoista erittäin aktiivinen järjestelmässä viestien peräkkäisiä taattua voi hidastaa järjestelmän suorituskyky. Jos viesti on erillinen ja tilaus ei ole merkitystä, voit ottaa samanaikaisen käsittelyn Redis.txt järjestelmä, jossa voit parantaa vastausajan. Voit tehostaa tätä StackExchange-asiakasohjelmassa määrittämällä FALSE tilaajan käyttämä yhteys PreserveAsyncOrder:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
redisHostConnection.PreserveAsyncOrder = false;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
```

## <a name="related-patterns-and-guidance"></a>Aiheeseen liittyvät kuvioiden ja ohjeita

Seuraavaa kaavaa myös voi olla merkitystä käyttämässäsi skenaariossa välimuistiin tallentamisen sovellusten käyttöönoton yhteydessä:

- [Välimuistin kesantoala mallia](http://msdn.microsoft.com/library/dn589799.aspx): tätä mallia kerrotaan, miten voit ladata tietoja pyydettäessä välimuistin tiedot-kaupasta. Tämän mallin avulla pystyy säilyttämään yhtenäisyyden tiedot, joita pidetään välimuistin ja alkuperäinen tietovaraston tietojen välillä.
- [Sharding kuvio](http://msdn.microsoft.com/library/dn589797.aspx) on tietoja käyttöönoton vaakasuora jakaminen parantaa skaalattavuus tallentamiseen ja käyttämiseen suurista tietomääristä.

## <a name="more-information"></a>Lisätietoja

- Microsoft-sivuston [MemoryCache luokka](http://msdn.microsoft.com/library/system.runtime.caching.memorycache.aspx) -sivulla
- Microsoft-sivuston [Azure Redis välimuistin asiakirjat](https://azure.microsoft.com/documentation/services/cache/) -sivu
- Microsoft-sivuston [Azure Redis välimuistin usein kysytyt kysymykset](redis-cache/cache-faq.md) -sivulla
- Microsoft-sivuston [mallin määrittäminen](http://msdn.microsoft.com/library/windowsazure/hh914149.aspx) -sivulla
- Microsoft-sivuston [Tehtävien asynkroninen kuvio](http://msdn.microsoft.com/library/hh873175.aspx) -sivulla
- StackExchange.Redis GitHub repo [putkistot ja multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) -sivulla
- [Redis pysyvyys](http://redis.io/topics/persistence) sivun Redis.txt-sivustossa
- [Replikoinnin sivun](http://redis.io/topics/replication) Redis.txt-sivustossa
- Redis.txt-sivuston [Redis klusterin opetusohjelma](http://redis.io/topics/cluster-tutorial) -sivulle
- [Osioimisen: Voit jakaa tietoja eri Redis.txt esiintymissä kesken](http://redis.io/topics/partitioning) Redis.txt sivuston sivulle
- Redis.txt-sivuston [Käyttäminen Redis kuin LRU välimuisti](http://redis.io/topics/lru-cache) -sivulle
- [Tapahtumat](http://redis.io/topics/transactions) -sivulla Redis.txt-sivustossa
- Redis.txt-sivuston [Redis suojaus](http://redis.io/topics/security) -sivulle
- Azure blogin [lantio ympärille Azure Redis välimuisti](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) -sivulla
- Microsoft-sivuston [Käynnissä Redis CentOS Linux-AM Azure-tietokannassa-](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) sivulla
- Microsoft-sivuston [ASP.NET-istunnon tilan palvelu Azure Redis välimuisti](redis-cache/cache-aspnet-session-state-provider.md) -sivulla
- Microsoft-sivuston [ASP.NET-tulostus välimuistin palvelu Azure Redis välimuisti](redis-cache/cache-aspnet-output-cache-provider.md) -sivulla
- [Johdanto tietotyyppeihin ja vedenotto Redis](http://redis.io/topics/data-types-intro) sivun Redis.txt-sivustossa
- [Peruskäyttö](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) sivun StackExchange.Redis-sivustossa
- StackExchange.Redis repo [Redis.txt tapahtumat](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) -sivulla
- Microsoft-sivuston [tietojen osioinnin opas](http://msdn.microsoft.com/library/dn589795.aspx)
