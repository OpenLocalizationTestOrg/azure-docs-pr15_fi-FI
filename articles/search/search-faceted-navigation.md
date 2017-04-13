<properties 
    pageTitle="Ottamisesta käyttöön kohdistettua siirtymistä Azure hakutoiminnossa | Microsoft Azure | haun siirtymisen luokat" 
    description="Lisää sovelluksia, jotka Azure haun cloud isännöidään search-palvelun Microsoft Azure-integrointi kohdistettua siirtymistä varten." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

#<a name="how-to-implement-faceted-navigation-in-azure-search"></a>Ottamisesta käyttöön kohdistettua siirtymistä Azure hakutoiminnossa

Kohdistettua siirtymistä on suodatus toimintoa, joka sisältää itse ohjattu tehostat siirtymistä haku-sovelluksissa. Kun termi 'kohdistettua siirtymistä' saattaa olla tuntemattomia, se on melkein koska, että olet käyttänyt eteen. Kuin seuraavassa esimerkissä esitetään, kohdistettua siirtymistä ei ole enää mitään muuta kuin Suodata hakutulokset käytetyt luokat.

## <a name="what-it-looks-like"></a>Miltä se näyttää

 ![][1]
  
Muita avulla löydät, mitä etsit, ja varmistaa, että, et saa nolla tulokset. Kehittäjän-rakenteita avulla voit näyttää eniten hyötyä hakuehdot Hae corpus siirtymiseen. Verkkokauppa sovelluksissa kohdistettua siirtymistä muodostanut usein merkkien, osastot (lapset 's kengät), koko, hinta, suosion ja luokitusten päälle. 

Kohdistettua siirtymistä eroaa haun tekniikoita ja voivat olla hyvin monimutkaisia. Azure hakutoiminnossa kohdistettua siirtymistä muodostanut kyselyn milloin aiemmin määritetty rakenteen arvioituun kenttien avulla. Kyselyn Lähetä kyselyissä, jota käytetään sovelluksen, jotta he saavat käytettävissä voi olla suodatinarvoja, tulos tiedostojoukon *pinta kyselyparametrit* . Voit leikata todella asiakirjan tulos on sovellus on otettava käyttöön `$filter` lauseke.

Sovelluksen kehitys kirjoitat koodia, joka muodostaa kyselyt on usean työn. Monia sovelluksen toimintoja, jotka haluat kohdistettua siirtymistä varten on annettu palvelu, mukaan lukien alueita määrittämisestä ja laskee käytön pinta tulosten tukee. Palvelun myös järkevää oletusarvot, joiden avulla voit välttää suureksi siirtyminen rakenteet. 

Tämä artikkeli sisältää seuraavat osat:

- [Miten voit luoda sen](#howtobuildit)
- [Esityksen kerroksen luominen](#presentationlayer)
- [Indeksin luominen](#buildindex)
- [Tietojen laadun tarkistaminen](#checkdata)
- [Kyselyn muodostaminen](#buildquery)
- [Vihjeitä siitä, miten voit hallita kohdistettua siirtymistä varten](#tips)
- [Kohdistettua siirtymistä varten alueen arvojen perusteella](#rangefacets)
- [Kohdistettua siirtymistä GeoPoints perusteella](#geofacets)
- [Kokeile](#tryitout)

##<a name="why-use-it"></a>Mitä hyötyä
Mahdollisimman tehokasta hakusovellukset on useita vuorovaikutus-mallien lisäksi hakukenttä. Kohdistettua siirtymistä varten on vaihtoehtoisia aloituskohtaa haun ojentamassa kätevä vaihtoehto monimutkaisia hakuja lausekkeiden kirjoittaminen käsin.

##<a name="know-the-fundamentals"></a>Perustiedot tutustuminen

Jos ole aiemmin käyttänyt Etsi kehitystä, kohdistettua siirtymistä ajatella paras tapa on, että se näkyy mahdollisuudet itse Ohjattu haku. Se on alirakenteen hakuja, ennalta määritetyt suodattimet, tarkentaa nopeasti alaspäin hakutulosten kohdassa ja valitse toimintoja käytetään perusteella. 

**Vuorovaikutuksen malli**

Hakuja käyttöön kohdistettua siirtymistä on toistuva, niin japanin ratkaisuun tietoja kyselyitä, jotka käyttäjän vastauksen liukuvat sarjana.

Aloitusarvo on sovellus-sivulle, jossa on kohdistettua siirtymistä, yleensä sijoitettu pinnalla. Kohdistettua siirtymistä on usein puun rakenteen, valintaruutuja kunkin arvon ja napsauttaa tekstiä. 

1.  Azure Etsi lähetettävä kyselyn määrittää kohdistettua siirtymisrakenteen pinta-kyselyparametrit kautta. Esimerkiksi kyselyn saattavat sisältää `facet=Rating`, samasta `:values` tai `:sort` vaihtoehto, jos haluat tarkentaa hakua esitys.
2.  Esityksen kerros hahmontaa, joka sisältää kohdistettua siirtymistä, käyttämällä määritetty pyynnön rakenteita haun sivulle.
3.  Valita kohdistettua siirtymistä-rakennetta, joka sisältää luokitus, käyttäjä napsauttaa "4" osoittamassa, että vain tuotteet, joiden luokitus on 4 tai uudempi mukana. 
4.  Sovelluksen lähettää vastauksen, kysely, joka sisältää`$filter=Rating ge 4` 
5.  Esityksen kerros päivittää sivun kohta rajoitetun tulosjoukon, joka sisältää vain ne kohteet, jotka täyttävät uusi (Tässä tapauksessa tuotteiden luokitellut 4 tai uudempi).

Yksinkertainen ulkoasu on kyselyparametri, mutta ei sekoittaa kyselyn syöte. Sitä käytetään aina kyselyn valintaehdot. Sen sijaan ajatella pinta kyselyparametrit syötteiden siirtymisrakenteeseen, joka on peräisin vastauksessa. Kunkin pinta kyselyn parametri antaisit Azure haun arvioi montako tiedostot ovat osittaisia tuloksia, pinta jokaisen arvon.

Ilmoitus `$filter` vaiheessa 4. Tämä on tärkeää näkökohdat kohdistettua siirtymistä varten. Vaikka rakenteita ja suodattimet ovat itsenäisten Ohjelmointirajapinnan, sinun on sekä toimittamaan aiot kokemus. 

**Rakenne-kuviot**

Sovelluksen koodissa kuvio on pinta kyselyparametrit avulla voit palauttaa kohdistettua siirtymisrakenteen sekä pinta tuloksia sekä $filter-lauseke, joka käsittelee Napsautettaessa-tapahtuma. Jaettu muiden käyttäjien `$filter` lausekkeen koodi takana todellinen rajaamisen hakutulosten palauttaa esityksen kerrokseen. Annetut värit pinta, valitsemalla värin punainen suoritetaan kautta `$filter` lauseke, joka valitsee vain kohteista, jotka on punaista väriä. 

**Azure haun kyselyn perusteet**

Azure hakutoiminnossa pyyntö on määritetty vähintään yksi kyselyparametrit (katso [Etsi asiakirjoista](http://msdn.microsoft.com/library/azure/dn798927.aspx) kullekin kuvauksen) kautta. Ei mitään kyselyparametrit tarvitaan, mutta sinulla on oltava vähintään yksi, jotta kysely, joka on voimassa.

Tarkkuus, yleensä viittauksina mahdollisuus ole merkitystä osumia suodattamaan saavutetaan jompikumpi tai molemmat lausekkeet kautta:

- **Etsi =**<br/>
Parametrin arvo on haku-lauseke. Sinun on ehkä yhtenäiseksi tekstiä tai monimutkaisen hakulausekkeen, joka sisältää useita ehtoja ja operaattorit. Palvelimessa Etsi-lauseketta käytetään teksti-haussa kyselyt hakukenttiä indeksissä valitsemiseen ehdot, tuota tuloksia arvon.mukaan järjestyksessä. Jos määrität `search` Null-kyselyn suorituksen ylittäneen koko indeksi (eli `search=*`). Tässä tapauksessa muiden osien kyselyn, kuten `$filter` vai näkyvissä pistemäärä profiili, näkyvätkö asiakirjat, jotka vaikuttavat ensisijainen tekijät palautetaan `($filter`) ja missä järjestyksessä (`scoringProfile` tai `$orderb`y).

- **$filter =**<br/>
Suodattimen on tehokas tapa kokorajoitukset hakutuloksia tiettyyn tiedostoon määritteiden arvojen perusteella. A `$filter` ensimmäisinä, ja sen faceting logiikka, joka luo käytettävissä olevat arvot ja vastaavan laskee jokaisen arvon jälkeen

Monimutkaisia hakuja lausekkeiden vähenee kyselyn suorituskykyä. Jos mahdollista, käyttämiseen hyvin rakennettava suodatinlausekkeiden tarkkuutta ja parantaa kyselyn suorituskykyä.

Lisätietoja siitä, miten suodattimen Lisää tarkkaan, vertaa monimutkaisen hakulausekkeen sellaiseksi, joka sisältää suodatin-lausekkeen:

- `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`

- `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Vaikka molemmat kyselyt eivät ole kelvollisia, toinen on ylempi, jos etsit kanssa Seattle Pysäköintialueet ei motellien. Ensimmäinen kysely on riippuvainen näitä sanoja on mainittu tai merkkijonokentät, kuten nimi, kuvaus ja muut haettavat tiedot sisältävä kenttä ei ole mainittu. Toisen kyselyn tarkkoja vastineita Jäsennettyjen tietojen tämännimistä ja todennäköisesti paljon tarkempi.

Sovelluksissa, jotka sisältävät kohdistettua siirtymistä haluat ja varmista, että kunkin käyttäjän toimi päälle kohdistettua siirtymisrakenteen mukana tarkentaa hakutulosten saavuttaa suodatin-lausekkeen avulla.

<a name="howtobuildit"></a>
##<a name="how-to-build-it"></a>Miten voit luoda sen

Azure hakutoiminnossa kohdistettua siirtymistä on toteutettu sovelluksen koodisi muodostaa pyynnön, mutta se on riippuvainen valmiin rakenteen osat.

Ennalta määritettyjen hakuun indeksi on `Facetable [true|false]` index-määritteen valittujen kenttien käyttöön ottaminen käyttöön kohdistettua siirtymisrakenteen määrittäminen. Ilman `"Facetable" = true`-kenttää ei voi käyttää pinta siirtyminen.

Kyselyn milloin sovelluksen koodin Luo pyynnön, joka sisältää `facet=[string]`-pyynnön parametri, joka sisältää kentän pinta mukaan. Kyselyn voi olla useita rakenteita, kuten `&facet=color&facet=category&facet=rating`, jokaiselle erotettu operaattoria (&) merkki.

Sovelluksen koodi on myös muodostaa `$filter` lausekkeen käsittelemään kohdistettua siirtymistä varten napsauttamalla-tapahtumat. A `$filter` vähentää hakutuloksia, käytä kuin arvo suodatusehdot.

Azure haku palauttaa hakutuloksissa käyttäjän sekä päivitykset kohdistettua siirtymisrakenteeseen hakusanat kohden. Azure hakutoiminnossa kohdistettua siirtymistä on yksitasoinen rakennetta, pinta-arvoilla ja laskee, kuinka monta tulosten löytyy kullekin.

Koodin esityksen kerros sisältää käyttäjäkokemuksen. Se on luettelo kohdistettua siirtymistä, kuten otsikon, arvot, valintaruutuja ja määrä rakenteellisia osia. Azure haun REST-Ohjelmointirajapinta on platform ympäristöstä riippumattomalla tavalla ja siten jostakin kieli- ja haluat ympäristössä. Tärkeintä on lisätä Käyttöliittymän osat, jotka tukevat lisäävän päivityksen päivitetyn Käyttöliittymän ja kunkin muita pinta on valittuna. 

Seuraavissa osissa Käymme läpi muodostaminen jokaisen osan tarkemmin esityksen kerros alkaen.

<a name="presentationlayer"></a>
##<a name="build-the-presentation-layer"></a>Esityksen kerroksen luominen

Takaisin-esitys-tason voit havaita vaatimukset, jotka saattavat vastattu muussa ja ymmärrät, mitkä ominaisuudet ovat välttämättömiä hakuja.

Kannalta kohdistettua siirtymistä verkossa tai sovelluksen sivusi näyttää kohdistettua siirtymisrakenteen, havaitsee syöttötapa-sivulla ja lisää muutetuista elementeistä. 

Web-sovellusten AJAX käytetään tavallisesti esityksen tason koska sen avulla voit päivittää vaiheittainen muutokset. Voit myös käyttää ASP.NET MVC tai mitä tahansa muita visualisoinnin käyttöympäristö, voit muodostaa yhteyden Azure-hakupalvelun HTTP. Otoksen koko tässä artikkelissa-- **AdventureWorks luettelon** – viitataan sovelluksen tapahtuu pidetään ASP.NET MVC.

Seuraavassa esimerkissä ottaa sovelluksen malli **index.cshtml** -tiedostosta muodostaa dynaamisen HTML-rakenne ja näyttämään kohdistettua siirtymistä etsinnän tulokset-sivulla. Otoksen kohdistettua siirtymistä hakutulossivulla sisäinen ja tulee näkyviin, kun käyttäjä on lähettänyt hakusana.

Huomaa, että kunkin pinta on otsikko (värejä, luokat-hinnat), on sidonta kohdistettua kenttään (väri, LuokanNimi, listPrice), mutta `.count` parametri palauttaa löydetty pinta tuloksen kohteiden määrän.

  ![][2]
 

> [AZURE.TIP] Hakutulossivulla suunnitellessasi muista lisätä valinnan rakenteita tarkoitetut. Jos käytät valintaruutujen valinta, käyttäjät voivat helposti intuit tyhjentäminen suodattimet. Voit joutua linkkipolun kuvion tai toisen creative lähestymistavan, muut asetteluissa. Esimerkiksi AdventureWorks luettelon malli-sovelluksessa, voit valita otsikko AdventureWorks luettelon vaihtamaan haun sivulle.

<a name="buildindex"></a>
##<a name="build-the-index"></a>Indeksin luominen

Faceting on käytössä kenttä kentän välein indeksissä, indeksi-määritteen kautta: `"Facetable": true`.  
Kaikki kenttätyypit, jotka voidaan mahdollisesti kohdistettua siirtymistä on `Facetable` oletusarvoisesti. Näiden kenttätyyppien sisällyttää `Edm.String`, `Edm.DateTimeOffset`, ja kaikki numeeriset kenttätyyppejä (, kaikki Kenttätyypit ovat facetable lukuun ottamatta `Edm.GeographyPoint`, joka ei voi käyttää kohdistettua siirtymistä varten). 

Indeksin luotaessa erikseen käytöstä faceting kentät, jotka on käytettävä koskaan pinta on suositeltavaa käyttöön kohdistettua siirtymistä varten.  Erityisesti merkkijonokentät Yksiarvoinen arvoista, kuten tunnus tai tuotteen nimi on määritettävä `"Facetable": false` kohdistettua siirtymistä niiden vahingossa (ja kelvoton) käytön estämiseksi.

Seuraavassa on rakenteen AdventureWorks luettelon otoksen sovelluksen (rajattu joitakin määritteitä, voit pienentää kokoa):

 ![][3]
 
Huomaa, että `Facetable` merkkijonokentät, joita ei kannata käyttää rakenteita, kuten nimi tai tunnus on poistettu käytöstä. Ottaminen pois käytöstä, jos et tarvitse sitä faceting pitää kokoa hakemiston pieni ja parantaa suorituskykyä, yleensä.

> [AZURE.TIP] Paras käytäntö sisältyy koko kunkin kentän indeksimääritteet. Vaikka `Facetable` on oletusarvoisesti käytössä lähes kaikissa kentissä, tarkoituksellisesti sivun reunaan määrittäminen kunkin määritteen avulla voit ajatella läpi kunkin rakenteen päätös vaikutukset. 

<a name="checkdata"></a>
##<a name="check-for-data-quality"></a>Tietojen laadun tarkistaminen 

Kun kehittäminen minkä tahansa tietokeskeisiä sovelluksen tietojen valmisteleminen on usein työn isommaksi osat. Etsi-sovellukset ovat poikkeusta ei ole. Tietojen laadun on suora vaikutus, onko kohdistettua siirtymisrakenteen toteutuu haluamallasi tavalla ja että sen tehokkuus auttavat käyttää suodattimia, joka vähentää tulos työntekijöiden määrittäminen.

Azure hakutoiminnossa haun corpus on muotoiltu tiedostoista, jotka täyttävät indeksin. Asiakirjojen sisältävät arvot, joita käytetään rakenteita laskemiseen. Jos haluat pinta brändiä tai hinta, kunkin asiakirjan on sisällettävä arvot *BrandName* ja *ProductPrice* , jotka ovat kelvollisia, johdonmukaisesti ja tehokkaasti suodatin-asetus.

Muutama muistutukset, mitä scrub varten on lueteltu alla:

- Kaikkien kenttien, jonka haluat mukaan pinta Mieti, onko se sisältää arvot, jotka sopivat suodattimina itse Ohjattu haku. Arvojen on oltava lyhyt, kuvaava ja riittävän erottuva Tyhjennä antavat kilpailevien vaihtoehtojen välillä.
- Kirjoitusvirheiden tai lähes toisiaan vastaavat arvot. Jos olet pinta väri ja kenttien arvot ovat oranssi ja Ornage (väärin), pinta, väri-kentän perusteella jatkaa molemmat.
- Erilaiset kirjoitusmuotoista tekstiä voi myös wreak kohdistettua siirtymistä oranssi ja näkyvät tällöin kaksi eri arvoja oranssi havoc. 
- Yksittäisten ja monikollista versioita saman arvon voi aiheuttaa erillisessä pinta kunkin.

Kuten arvata saattaa, kassaan-tietojen valmisteleminen on olennainen osa on tehokas kohdistettua siirtymistä varten.

<a name="buildquery"></a>
##<a name="build-the-query"></a>Kyselyn muodostaminen

Tunnus, jolla voit kirjoittaa kyselyjen luomisesta on määritettävä kelvollinen kysely, mukaan lukien haun lausekkeiden rakenteita, suodattimia, näkyvissä pistemäärä profiilit – mitään käytetään muodostetaan pyyntö kaikista osista. Tässä osassa tutustutaan missä rakenteita sopii yhteen kyselyn ja siitä, miten suodattimia käytetään rakenteita rajoitetun tulosjoukon aikana.

Voit aloittaa on usein esimerkiksi. Seuraavassa esimerkissä tehdyt **CatalogSearch.cs** tiedostosta muodostaa pyynnön, joka luo pinta siirtyminen värejä, luokan tai hinnan perusteella. 

Huomaa, että rakenteita on olennainen malli-sovelluksessa. Hakuja AdventureWorks luettelossa on suunniteltu kohdistettua siirtymistä varten ja suodattimet. Tämä on ilmeistä kohdistettua siirtymistä sivulla ääniä sijaintia. Sovelluksen malli sisältää rakenteita (väriä, luokka, hinnat) parametrit URI-ominaisuuksien yhdistäminen haku-menetelmän (kuten rakenteeltaan sovelluksen malli).

  ![][4]
 
Pinta kyselyparametri on määritetty kentän ja sen mukaan, tietotyyppi, voi olla edelleen parametrina pilkuin erotettu luettelo, joka sisältää `count:<integer>`, `sort:<>`, `intervals:<integer>`, ja `values:<list>`. Arvojen luettelon tuetaan numeerisia tietoja alueiden määritettäessä. Lisätietoja [Haun asiakirjat (Azure haun API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) käyttö.

Sekä muita muotoilla sovelluksen mukaan pyyntö olisi luominen suodattimet rajataksesi candidate asiakirja voi olla arvo valinnan perusteella. Polkupyörien-kaupan kohdistettua siirtymistä on vihjeitä kysymyksiin, kuten "mitä värejä, valmistajat ja bikes tyypit ovat käytettävissä", kun suodatus vastauksia kysymyksiin, kuten "mitä tarkka bikes on punainen, mountain bikes tämän hinta alue".

Kun käyttäjä napsauttaa "Punainen" osoittamassa, että vain punainen tuotteet mukana, näkyy tällöin sovelluksen lähettää seuraavan kyselyn `$filter=Color eq ‘Red’`.

## <a name="best-practices-for-faceted-navigation"></a>Parhaat käytännöt kohdistettua siirtymistä varten

Seuraavassa luettelossa on yhteenveto joitakin parhaita käytäntöjä.

- **Tarkkuus**<br/>
Suodattimilla. Jos haluat vain haun rajoituksista yksin perusmuodon aiheuttaa asiakirjaan, joka palautetaan, joka ei ole kuin tarkka arvo kaikista kentistä. 

- **Kohdekentät**<br/>
Kohdistettua alirakenteen yleensä haluat sisällyttää vain asiakirjoja, joissa on kuin arvo (kohdistettua) tietyn kentän mitä tahansa ei yli kaikki hakukenttiä. Suodattimen lisäämistä vahvistaa kohdekentän ohjaamalla palvelun haluat etsiä vain kohdistettua on vastaava arvo-kenttään.

- **Indeksi tehokkuutta**<br/>
Jos sovellus käyttää kohdistettua siirtymistä yksinomaan (eli ei hakuruutuun) voit merkitä kenttä `searchable=false`, `facetable=true` tuottamaan tiiviimmän indeksi. Lisäksi indeksoinnin ilmenee vain koko pinta-arvoja, word-break tai usean sanan arvon osien indeksoinnin.

- **Suorituskyky**<br/>
Suodattimet tarkentaa hakua varten candidate Asiakirjajoukon ja pois tulosten luokittelua. Jos sinulla on suuri asiakirjajoukon, käyttämällä pakolliset pinta Poraudu alaspäin usein avulla voit merkittävästi parantaa suorituskykyä.


<a name="tips"></a> 
##<a name="tips-on-how-to-control-faceted-navigation"></a>Vihjeitä siitä, miten voit hallita kohdistettua siirtymistä varten

Alla on vinkki-taulukko, jossa ohjeet koskevat ongelmat.

**Lisää kunkin kentän otsikot pinta siirtyminen**

Otsikoiden määritetään yleensä HTML- tai lomakekirjaston (**index.cshtml** -sovelluksen malli). Ei ole API Azure Hae pinta siirtyminen otsikot tai metatietojen muita kuvia.

**Määrittää kentät, jotka voidaan käyttää pinta**

Et voi peruuttaa hakemiston rakenteen määrittää kentät, jotka ovat käytettävissä pinta käyttää. Jos kenttä on facetable, kysely määrittää kenttien pinta mukaan. Kenttä, jonka olet faceting sisältää arvot, jotka näkyvät-otsikon alapuolella. 

Jokaisen tarran-kohdassa näkyvät arvot haetaan pois indeksistä. Jos pinta-kenttä on *väri*, muita suodattamista varten käytettävissä olevat arvot voidaan kentän arvot näkyvät (punainen, musta ja niin edelleen).

Numeerista tai päivämäärä ja aika-arvoja vain, voit määrittää arvot pinta-kenttään (esimerkiksi `facet=Rating,values:1|2|3|4|5`). Arvojen luettelon sallitaan näiden kenttätyyppien yksinkertaistaa pinta tulokset erottelua kyselyjä yhtenäiset alueet (alueet joko numeerisia arvoja tai ajanjaksoja). 

**Rajaa pinta tulokset**

Pinta tulokset ovat löytyvät tulos, joka vastaa pinta termin asiakirjat. Seuraavassa esimerkissä *cloud tietojenkäsittely*-hakutuloksissa 254 myös kohteilla *sisäinen määritys* sisältötyyppinä. Kohteet eivät ole toisensa poissulkevia. Jos kohteen ehdot sekä suodattimet, lasketaan yksitellen. Tämä on mahdollista, kun faceting- `Collection(Edm.String)` kentät, jotka käytetään usein toteuttamisesta asiakirjan tunnisteita.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Jos huomaat, että pinta tulokset ovat liian suuria pysyvästi, on suositeltavaa, että voit lisätä useita suodattimia aiemmat osat antaa sovelluksen käyttäjille tarkentaa hakua Lisää asetuksia-artikkelissa kuvatulla tavalla.

**Rajoita kohteiden pinta-siirtyminen**

Kunkin siirtymisrakenteessa kohdistettua kentän on oletusrajoitus 10 arvoista. Tämä oletusarvo on järkevää siirtyminen rakenteiden, koska se säilyttää arvoluettelot hallittavissa. Voit ohittaa oletusarvo määrittämällä arvon laskeminen.

- `&facet=city,count:5`määrittää, että vain luokitellut tulokset yläreunassa olevia ensin 5 kaupunkien palautetaan voi olla tuloksena. Annettu hakusana "airport" ja 32 vastineita, jos kysely määrittää `&facet=city,count:5`, vain ensimmäiset viisi yksilöllinen kaupunkien eniten tiedostojen hakutuloksissa sisältyvät pinta tulokset.

Ilmoitus voi olla tulokset ja etsinnän tulokset välinen ero. Etsinnän tulokset ovat kaikki tiedostot, jotka vastaavat kyselyä. Pinta tulokset ovat pinta kullekin arvolle vastineita. Esimerkissä hakutulokset sisältävät Kaupunki nimiä, jotka eivät ole pinta luokitus luettelossa (5 tässä esimerkissä). Tulokset, jotka on suodatettu kautta kohdistettua siirtymistä muuttuvat näkyviksi käyttäjälle, kun hän poistaa muita tai valitsee muiden rakenteita lisäksi kaupunki. 

> [AZURE.NOTE] Keskustella `count` kun kyseessä on useampi kuin yksi voi olla sekava. Seuraavassa taulukossa on lyhyt yhteenveto termi käyttötavan Azure haun Ohjelmointirajapinta, otoksen koodi ja ohjeissa. 

- `@colorFacet.count`<br/>
Esityksen koodissa näkyy Laske parametrin pinta käytetään esittämään pinta tulosten määrää. Pinta tulosten määrä viittaa asiakirjoja, jotka vastaavat pinta termin tai alue.

- `&facet=City,count:12`<br/>
Pinta kyselyn voit määrittää Laske arvon.  Oletusarvo on 10, mutta voit määrittää sen ylemmäs tai alemmas. Määrittäminen `count:12` saa yläreunassa 12 vastaa pinta tuloksissa tiedostojen määrän mukaan.

- "`@odata.count`"<br/>
Kysely, vastaus arvo viittaa hakuehtoja vastaavat kohteiden määrä. Keskimäärin on suurempi kuin kaikkien pinta tuloksen yhdistetyn vuoksi kohteet, jotka vastaavat hakusanoja, tavoitettavuuden summa, mutta ei voi olla arvo hakutuloksia on.


**Tasojen kohdistettua siirtymistä varten** 

Kuten on kerrottu, ei ole suoraa tukea sisäkkäisiä rakenteita hierarkiassa. Poissa-ruutuun kohdistettua siirtymistä tukee vain yhden tason verran suodattimet. Kuitenkin olemassa menetelmää. Voit salata pinta hierarkkinen rakenne `Collection(Edm.String)` määritelmää osoita hierarkiaa kohden. Käyttöönoton tämä vaihtoehtoinen toimintatapa on tämän artikkelin laajemmin, mutta on lisätietoja [OData esimerkin mukaan](http://msdn.microsoft.com/library/ff478141.aspx)sivustokokoelmat. 

**Vahvista kentät**

Jos luot rakenteita dynaamisesti luotettu käyttäjän syötteiden perusteella luettelo-olisi joko vahvistat, että kohdistettua kenttien nimet ovat kelvollisia tai pääse nimiä luotaessa URL-osoitteet, jossa on joko `Uri.EscapeDataString()` .NET tai vastaa omaa ympäristöäsi valinta kohdassa.

**Laskee pinta tulokset**

Kun lisäät suodattimen kohdistettua kyselyn, voit säilyttää pinta-lause (esimerkiksi `facet=Rating&$filter=Rating ge 4`). Tarkalleen ottaen, voi olla = luokitus ei tarvita, mutta päivittäminen palauttaa luokitusten arvoja voi olla arvo 4 tai uudempi versio. Esimerkiksi Jos kyselyssä on suurempi tai yhtä suuri "4" suodattimen käyttäjä napsauttaa "4", laskee palautetaan kutakin luokitusta, joka on 4 ja määrittäminen.  

**Valitse pinta laskee sharding vaikutukset**

Kaikissa tilanteissa saatat huomata, että pinta-määrät eivät vastaa tulosjoukkoja, (katso [kohdistettua siirtymistä Azure-haku (vastaukseksi)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Pinta laskee voi olla virheellisiä sharding-arkkitehtuuri vuoksi. Jokaisen hakuindeksin on useita shards ja jokaisessa raportteja ylimmät N rakenteita tiedostojen määrän, joka on sitten yhdistää yhden tuloksen mukaan. Jos jotkin shards on paljon vastaavat arvot, kun muut käyttävät pienempi, voit etsiä joitakin pinta-arvoja puuttuu tai valitse laskettu tuloksissa.

Vaikka tämä ongelma voi muuttaa milloin tahansa, jos tämä ongelma ilmenee tänään, voit tutustua sen ympärille inflating keinotekoisesti määrä:<number> erittäin suuri luvuksi voidaan valvoa kunkin shard koko raportit. Jos Laske arvo: on suurempi tai yhtä suuri kuin-kentän yksilöllisten arvojen määrän, sinun on tarkkoja tuloksia. Kun asiakirjan arvo on erittäin suuri, suorituskyky viivästyy, niin käyttänyt tätä asetusta harkiten.

<a name="rangefacets"></a>
##<a name="facet-navigation-based-on-a-range-values"></a>Alueen arvojen perusteella pinta siirtyminen

Faceting alueiden kautta on yleinen haku sovelluksen vaatimus. Alueiden tuetaan numeerisia tietoja ja päivämäärä ja aika-arvot. Voit lukea lisää kunkin [Search-tiedostot (Azure haun API)](http://msdn.microsoft.com/library/azure/dn798927.aspx)lähestymistavan tietoja.

Azure haun yksinkertaistaa alueen rakentaminen tarjoamalla kaksi tavoista tietojenkäsittely alueen. Molemmat tavoista Azure haun Luo annettu määritetty syötteiden haluamasi alueet. Esimerkiksi jos määrität alueen arvot 10 | 20 | 30, se luo automaattisesti solualueiden 0-10 10 – 20, 20 – 30. Sovelluksen malli poistaa välein, jotka ovat tyhjiä. 

**Vaihtoehto 1: Käytä väli-parametri**<br/>
Voit määrittää hinta rakenteita 10 kerrallaan, määrität:`&facet=price,interval:10`


**Menetelmä 2: Arvojen luettelon käyttäminen**<br/>
Numeerisia tietoja voit käyttää arvojen luettelon.  Harkitse pinta alueen listPrice, hahmontaa seuraavasti:

  ![][5]

Alue on määritetty **CatalogSearch.cs** tiedoston arvojen luettelon avulla:

    facet=listPrice,values:10|25|100|500|1000|2500

Kunkin alueen on luotu käyttämällä lähtökohtana-luettelossa tilassa seinän päätepiste arvon 0 ja sen jälkeen leikata edellisen alueen luoda erilliset väliajoin. Azure haun tehdä osaksi kohdistettua siirtymistä varten. Ei tarvitse kirjoittaa jäsentämiseen kussakin koodia.

### <a name="build-a-filter-for-facet-ranges"></a>Pinta alueiden suodattimen luominen ###

Voit suodattaa käyttäjän valitsemat alueen perustuvat asiakirjat, käyttämällä `"ge"` ja `"lt"` suodattaa kaksiosainen-lausekkeen, joka määrittää alueen päätepisteet operaattoreita. Esimerkiksi jos käyttäjä valitsee alueen 10 – 25, suodattimen olisi `$filter=listPrice ge 10 and listPrice lt 25`.

Valitse malli-sovelluksessa suodatin-lausekkeen käyttää **priceFrom** ja **priceTo** parametrit päätepisteet määrittämiseen. **CatalogSearch.cs** **BuildFilter** menetelmä sisältää suodatin-lausekkeen, joka antaa alueella asiakirjat.

  ![][6]

<a name="geofacets"></a> 
##<a name="filtered-navigation-based-on-geopoints"></a>GeoPoints perusteella suodatettu siirtyminen

Se on yhteinen nähdäksesi suodattimet, jotka auttavat, voit valita säilö, ravintolaan tai kohde sen lähellä nykyisen sijainnin perusteella. Tämän tyyppisen suodattimen esimerkin kohdistettua siirtymistä varten, mutta on todella vain suodattimen. Olemme mainitse se tässä käyttäjille, että sinä, jotka etsimäsi erityisesti käyttöönoton neuvoja tietyn rakenne tämän ongelman.

On kaksi Azure haun, **geo.distance** ja **geo.intersects**Geospatiaalisia-funktiota.

- **Geo.distance** -funktio palauttaa etäisyys kilometreiksi kahden pisteen välisen, yksi kenttä ja toisaalta vakio välitettävä suodattimen osana. 

- **Geo.intersects** -funktio palauttaa arvon TOSI, jos tiettynä annetun monikulmion, jossa kohta on kentän ja monikulmio on määritetty osana suodattimen koordinaatit vakion luettelo.

Löydät suodattimen esimerkkejä [OData Lausekkeiden syntaksin (Azure haku)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>
##<a name="try-it-out"></a>Kokeile

Azure haun Adventure Worksin esittelyn codeplexissä on viitattu tämän artikkelin esimerkkejä. Kun käsittelet hakutulokset, katso kyselyn rakenteeseen muutokset URL-osoite. Tämän sovelluksen tapahtuu liittäminen rakenteita URI valitessasi yksitellen.

1.  Määritä URL-osoite ja-ohjelmointirajapinnan avain sovelluksen malli. 

    Huomaa rakenne, joka on määritetty CatalogIndexer projektin Program.cs-tiedostossa. Määrittää värin, listPrice, kokoa, leveyttä, LuokanNimi ja modelName facetable kentät.  Vain joidenkin nämä (väri, listPrice, LuokanNimi) todella otettu käyttöön kohdistettua siirtymistä varten.

3.  Suorita sovellus. 

    Ensimmäinen näkyy vain Etsi-ruutuun. Valitse hakupainike heti, jos haluat saada kaikki tulokset, tai kirjoita hakuehto.

    ![][7]
 
4.  Kirjoita hakusana, kuten polkupyörien, ja valitse sitten **Hae**. Kysely suoritetaan nopeasti.
 
    Kohdistettua siirtymisrakenteen palautetaan myös hakutulokset.  URL-Osoitteen muita värejä, luokat ja hinnat ovat tyhjiä. Hakutulossivulle valitsemalla kohdistettua siirtymisrakenteen sisältää laskee kunkin pinta-Tulosta varten.

     ![][8]
 
5.  Valitse väri, luokka ja hinta-alue. Rakenteita ei null alkuperäinen haun, mutta ne ottaa arvot, kuten hakutulokset rajataan kohteet, jotka eivät enää vastaa. Huomaa, että URI vastataan muutoksia kyselyyn.

    ![][9]
 
6.  Voit tyhjentää kohdistettua kyselyn niin, että voit kokeilla erilaisia kyselyn toiminnan valitsemalla sivun yläreunassa **AdventureWorks luettelon** .

    ![][10]
 
<a name="nextstep"></a>
##<a name="next-step"></a>Seuraava vaihe

Testaa tietosi, voit lisätä *modelName*pinta-kenttään. Indeksi on jo määritetty tämä pinta, jotta indeksiin muutoksia ei tarvita. Mutta sinun on muokattava lisätä uusia pinta-mallien ja pinta-kenttä lisätään kyselyyn konstruktoria HTML.

Lisää tiedot käyttöön kohdistettua siirtymistä periaatteet rakenne Suosittelemme on seuraavissa linkeissä:

- [Kohdistettua haun suunnitteleminen](http://www.uie.com/articles/faceted_search/)
- [Rakenne-kuvioita: Kohdistettua siirtymistä varten](http://alistapart.com/article/design-patterns-faceted-navigation)

Voit myös katsoa [Azure haun perinpohjaisesti käsittelevään artikkeliin](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). 45:25-osoitteessa ei esittely rakenteita ottamisesta käyttöön.

<!--Anchors-->
[How to build it]: #howtobuildit
[Build the presentation layer]: #presentationlayer
[Build the index]: #buildindex
[Check for data quality]: #checkdata
[Build the query]: #buildquery
[Tips on how to control faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/Facet-1-slide.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

 