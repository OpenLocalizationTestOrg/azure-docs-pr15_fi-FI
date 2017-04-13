<properties
    pageTitle="Näkyvissä (Azure haun REST API-versio 2015-02-28-esikatselu)-profiileista pistemäärä | Microsoft Azure | Azure Search-esikatselu Ohjelmointirajapinta"
    description="Azure haku on isännöityä cloud search-palvelun, joka tukee säätö luokitellut tulokset käyttäjän määrittämä tulosmalli profiilien perusteella."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.author="heidist"
    ms.date="08/29/2016" />

# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Näkyvissä (Azure haun REST API-versio 2015-02-28-esikatselu)-profiileista pistemäärä

> [AZURE.NOTE] Tässä artikkelissa kuvataan tulosmalli profiilit [2015-02-28-esikatselu](search-api-2015-02-28-preview.md). Tällä hetkellä ei ole eroa `2015-02-28` [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) -versioon ja `2015-02-28-Preview` versio kuvatut, mutta Tarjoamme tämän asiakirjan silti jotta asiakirjan tarkistustyökalua koko Ohjelmointirajapinnan kautta.

## <a name="overview"></a>Yleiskatsaus

Etsi pistemäärän jokaisen kohteen hakutulosten määrää laskenta näkyvissä pistemäärä viittaa. Pisteet on ilmaisin kohteen asiayhteyden nykyisen hakutoiminto kontekstissa. Mitä suurempi pistemäärä, Lisää haluamasi kohteen. Hakutuloksissa kohteet ovat järjestys vyöhykkeen suuresta laskee kunkin kohteen haku kirjainarvosanan heikko, perusteella.

Azure haku käyttää oletusarvoisesti näkyvissä pistemäärä Laske alkuperäinen tulos, mutta voit mukauttaa laskutoimituksen tulosmalli profiilin kautta. Tulosmalli profiilit antaa sinulle kohteiden luokittelu tarkemmin hakutuloksissa. Haluat ehkä edistää tuotto ne saattavat perusteella, ylemmälle uusimmat kohteet tai edistää ehkä kohteet, joiden on liian pitkä varastossa.

Tulosmalli profiili on osa indeksin määrityksen, koostuva kentät, toiminnot ja parametrit.

Seuraavassa esimerkissä esitetään, avulla voit verrata tulosmalli profiilin näyttää seuraavalta, yksinkertainen profiilia "geo". Tämän saman osa osat, jotka eivät hakusanoja `hotelName` kentän. Tiedostossa käytetään myös `distance` Web-sivuston yhteensopivaksi kohteet, jotka ovat nykyisen sijainnin kymmenen kilometreiksi-funktiota. Jos joku etsii termi 'inn' ja 'inn' tapahtuu on hotellista nimeen, tiedostot, jotka sisältävät hotellit 'inn' kanssa näkyvät suurempi hakutuloksista.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Jos haluat käyttää tulosmalli profiilia, kyselyn on muotoiltu siten Määritä profiilin kyselymerkkijonon. Huomaa alla kyselyn kyselyn parametri, `scoringProfile=geo` pyynnön.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Tämä kysely etsii termi 'inn' ja siirtää kohdistimen kohtaan. Huomaa, että tämä kysely muita parametreja, kuten `scoringParameter`. Kyselyparametrit on artikkelissa [Haku-tiedostot (Azure haun API)](search-api-2015-02-28-preview.md#SearchDocs).

Valitse tarkistettava yksityiskohtaisempia Esimerkki tulosmalli profiilin [Esimerkki](#example) .

## <a name="what-is-default-scoring"></a>Mikä on oletusarvoisesti näkyvissä pistemäärä?

Näkyvissä pistemäärä laskee kunkin kohteen arvon.mukaan järjestetyn tulosjoukon haun tulos. Etsi tulosjoukon jokaisen kohteen määritetty haun tulokset ja valitse luokitellut korkein pienimpään. Kohteet, joiden suurempi tulosten palautetaan sovellukseen. Oletusarvon mukaan ylimmän 50 palautetaan, mutta voit käyttää `$top` parametri palauttaa kohteet (enintään 1 000 yksittäisen vastaus) pienentää tai suurentaa määrän.

Oletusarvon mukaan haun tulokset lasketaan mukaan tilastollisia ominaisuuksia seuraavien tietojen ja kysely. Azure hakutoiminto löytää tiedostot, jotka sisältävät hakuehdot kyselymerkkijonon (jotkin tai kaikki sen mukaan, `searchMode`), bittikarttaa, jotta saadaan asiakirjoja, jotka sisältävät useita esiintymiä hakusanoja. Etsi kirjainarvosanan siirtyy-myös suurempi, jos termi on harvinaisissa tietojen corpus, mutta yleisiä asiakirjassa. Tämän menetelmän tietojenkäsittely asiayhteyteen perusta kutsutaan TF IDF tai (termin korkojakso käänteisen asiakirjan taajuus).

Jos ei ole mukautettu lajittelu, tulosten sitten luokittelu haun tuloksen mukaan ennen kuin ne palautetaan puheluja-sovellukseen. Jos `$top` ei määritetä, ottaa korkein haun tulokset palautetaan 50 kohteet.

Voit toistaa haun tulokset arvot koko tulosjoukon. Esimerkiksi 10 tietueita, joiden pistemäärän 1.2 on ehkä 20 tietueita, joiden pistemäärän 1.0 ja 20 tietueita, joiden pistemäärän 0,5. Kun useita osumia on sama haku pisteet, sama tulos kohteiden järjestystä ei ole määritetty ja se ei ole. Suorita kysely uudelleen, ja saatat nähdä kohteiden VAIHTO sijainti. Valita kaksi tietueita, joiden samat tulokset, ei ei takaa, mikä näkyy ensimmäisenä.

## <a name="when-to-use-custom-scoring"></a>Milloin kannattaa käyttää mukautettuja näkyvissä pistemäärä

Sinun on luotava vähintään yksi tulosmalli profiilit, kun luokitus toiminta oletusarvo ei siirry äärimmäisenä tarpeeksi tekstimuodossa kokouksen liiketoiminnan tavoitteiden. Voit esimerkiksi päättää, että haun asiayhteyden olisi Web-sivuston yhteensopivaksi lisätyt kohteet. Vastaavasti voit joutua, kenttä, joka sisältää käyttökate tai jonkin muun kentän, joka ilmoittaa, mahdollinen tuotto. Kehittämällä osumaa, jotka tuovat edut yrityksesi voi olla tärkeä tekijä päätät käyttää tulosmalli profiileja.

Suunnittelet perustuva järjestys myös suoritetaan kautta näkyvissä pistemäärä profiilit. Harkitse, joiden avulla voit lajitella hinta, päivämäärä, luokitus tai asiayhteyden aiemmin käyttänyt hakutulossivuja. Azure hakutoiminnossa tulosmalli profiilit vaikuttavat "asiayhteyden"-vaihtoehto. Ohjaa asiayhteyden määritelmän voit predicated business tavoitteet ja hakuja, jotka haluat pitää tyyppi.

<a name="example"></a>
## <a name="example"></a>Esimerkki

Mukautetun näkyvissä pistemäärä suoritetaan kautta näkyvissä pistemäärä indeksi-rakenne määritellään profiilit, kuten.

Tässä esimerkissä indeksi on kaksi tulosmalli-profiileista rakenteen (`boostGenre`, `newAndHighlyRated`). Kaikki tämän indeksin, joka sisältää joko profiilin kyselyparametri käyttämällä profiilia sija tulosjoukko kysely.

[Kokeile tässä esimerkissä](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Työnkulun

Toteuttamisesta tulosmalli mukautettuja toimintoja, Lisää tulosmalli profiili rakenne, joka määrittää hakemiston. Voit määrittää hakemiston sisällä tulosmalli profiileja, mutta voit määrittää vain yhtä profiilia milloin tahansa tietyn kyselyn.

[Mallin](#bkmk_template) kuin tämän ohjeaiheen alussa.

Anna nimi. Tulosmalli-profiileista on valinnainen, mutta jos lisäät jonkin, nimi on pakollinen. Noudata kenttien nimeämiskäytännöt (alkaa kirjaimella, vältetään erikoismerkeistä ja varatut sanat). Lisätietoja on kohdassa [Nimeäminen säännöt](http://msdn.microsoft.com/library/azure/dn857353.aspx) .

Tulosmalli profiilin leipätekstiin muodostetaan painotettu kentistä ja funktioita.

### <a name="weights"></a>Leveydet ###

`weights` Tulosmalli profiilin ominaisuus määrittää, joka määrittää suhteellisen leveyden kentän nimi-arvo-parit. [Esimerkki](#example)albumTitle, tyylilaji ja artistName kentät ovat tehosti 1,5, 5 ja 2 tarpeen mukaan. Miksi on Tyylilaji tehosti paljon suurempi kuin muiden? Jos haku suoritetaan tiedot, jotka ovat hieman yhtenäisinä päälle (on 'Tyylilaji' kirjoitusmuotoon `musicstoreindex`), sinun on ehkä suurempi varianssi suhteelliset leveydet. Esimerkiksi `musicstoreindex`, 'rock' näkyy sekä tyylilaji ja samannimisen phrased Tyylilaji kuvaukset. Tyylilaji korvaamaan Tyylilaji kuvaus halutessasi Tyylilaji-kentässä on paljon suurempi suhteellinen paino.

### <a name="functions"></a>Funktiot ###

Funktioita käytetään, kun tietyn kontekstit varten tarvittavat muita laskutoimituksia. Kelvollinen funktion ovat `freshness`, `magnitude`, `distance` ja `tag`. Kukin funktio on parametreja, jotka ovat yksilöllisiä siihen.

  - `freshness`käytetään, kun haluat edistää mukaan, miten uusi tai vanha kohteen. Tätä funktiota voidaan käyttää vain datetime-kenttien kanssa (`Edm.DataTimeOffset`). Huomautus `boostingDuration` määrite käytetään vain tuoreus-funktion kanssa.
  - `magnitude`on käytettävä, kun haluat edistää perusteella kuinka suuri tai pieni on numeerinen arvo. Tilanteita, joissa tämä funktio Soita Sisällytä kehittämällä käyttökate, ylin hinta, alin hinta tai lataukset määrä. Voit peruuttaa alueen hyvin pieni, voit halutessasi käänteisen kuvio (esimerkiksi kohteisiin tehon lisäys alempihintaista enemmän kuin suurempi hinnoiteltu kohteet). Valita eri hintoja 100 $1, Määritä `boostingRangeStart` 100 ja `boostingRangeEnd` 1 edistää alempihintaista kohteet. Tätä funktiota voi käyttää vain Kaksoistarkkuus ja kokonaisluku-kenttien kanssa.
  - `distance`on käytettävä, kun haluat edistää lähialue tai maantieteellisen sijainnin mukaan. Tätä funktiota voidaan käyttää vain `Edm.GeographyPoint` kentät.
  - `tag`on käytettävä, kun haluat edistää tunnisteilla yhteiset tiedostot ja hakukyselyt välillä. Tätä funktiota voidaan käyttää vain `Edm.String` ja `Collection(Edm.String)` kentät.
  
#### <a name="rules-for-using-functions"></a>Säännöt funktioilla ####

  - Funktion tyyppi (tuoreus, sitä, etäisyyden, tunniste) on oltava pieniksi kirjaimiksi.
  - Funktioita ei voi sisältää null tai tyhjiä arvoja. Erityisesti jos sisällytät kentän nimi, on määritetty jokin.
  - Toimintoja voi käyttää vain suodatettavia kenttiin. Saat lisätietoja suodatettavia kentistä [Indeksin luominen](search-api-2015-02-28-preview.md#CreateIndex) .
  - Toimintoja voi käyttää vain kenttiä, jotka on määritelty indeksin kentät-sivustokokoelman.

Kun olet määrittänyt indeksi-kootaan hakemisto lataamalla indeksi-malli, Seuratut asiakirjat. Saat ohjeet näistä toiminnoista [Indeksin luominen](search-api-2015-02-28-preview.md#CreateIndex) ja [Lisää tai päivitä asiakirjoja](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) . Kun indeksi muodostanut, on oltava toimintojen tulosmalli profiilia, joka toimii haun tiedot.

<a name="bkmk_template"></a>
## <a name="template"></a>Malli
Tässä osassa esitellään syntaksista ja näkyvissä pistemäärä profiilit-malli. Lisätietoja [indeksin määrite viittaus](#bkmk_indexref) seuraavan osion kuvaukset määritteet.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
            ...
          }
        },
        "functions": (optional) [
          {
            "type": "magnitude | freshness | distance | tag",
            "boost": # (positive number used as multiplier for raw score != 1),
            "fieldName": "...",
            "interpolation": "constant | linear (default) | quadratic | logarithmic",

            "magnitude": {
              "boostingRangeStart": #,
              "boostingRangeEnd": #,
              "constantBoostBeyondRange": true | false (default)
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## <a name="scoring-profile-property-reference"></a>Profiilin ominaisuuksien ohje näkyvissä pistemäärä

**Huomautus** Tulosmalli funktiota voi käyttää vain kentät, jotka ovat suodatettavia.

| Ominaisuus | Kuvaus |
|----------|-------------|
| `name`   | Pakollinen. Tämä on tulosmalli profiilin nimi. Seuraa kentän samaa nimeämiskäytäntöä. Se on alettava kirjaimella, ei voi olla pistettä, kaksoispisteet tai @ symbolien ja ei voi alkaa lause "azureSearch" (kirjainkoko on merkitsevä). |
| `text` | Sisältää leveydet-ominaisuutta. |
| `weights` | Valinnainen. Nimi-arvo-pari, joka määrittää kenttänimi- ja suhteellinen leveys. Suhteellinen leveys on oltava positiivinen kokonaisluku tai liukuluku. Voit määrittää kenttänimi ilman vastaavaa leveys. Leveydet käytetään tärkeysasteen yhden kentän suhteessa toiseen. |
| `functions` | Valinnainen. Huomaa, että tulosmalli funktiota voi käyttää vain kenttiä, jotka ovat suodatettavia. |
| `type` | Pakollinen näkyvissä pistemäärä funktiot. Osoittaa ryhmän tyypin toiminto, jota käytetään. Kelvolliset arvot `magnitude`, `freshness`, `distance` ja `tag`. Voit lisätä useamman kuin yhden funktion jokaiseen tulosmalli profiiliin. Funktionimi on oltava pieniksi kirjaimiksi. |
| `boost` | Pakollinen näkyvissä pistemäärä funktiot. Positiivinen luku, joka käyttää kerroin raaka tulos. Ei voi olla yhtä suuri kuin 1. |
| `fieldName` | Pakollinen näkyvissä pistemäärä funktiot. Tulosmalli funktiota voi käyttää vain kenttiä, jotka sisältyvät indeksin kenttä-sivustokokoelman, ja ne suodatettavia. Lisäksi jokaisen funktion tyyppi esitellään lisärajoituksin (tuoreus käytetään kentillä, sitä kokonaisluku tai lisää kenttiä, etäisyyden sijainti-kenttien kanssa ja tunniste, jossa on merkkijono tai merkkijono sivustokokoelman kentät datetime). Voit määrittää vain yhden kentän funktion määritelmä kohden. Esimerkiksi jos haluat käyttää sitä kahdesti sama, haluat lisätä kaksi määritykset sitä, yhteen kunkin kentän. |
| `interpolation` | Pakollinen näkyvissä pistemäärä funktiot. Määrittää kulmakerroin, jonka kehittämällä kasvaa alueen alusta loppuun alueen kirjainarvosanan. Kelvolliset arvot `linear` (oletus) `constant`, `quadratic`, ja `logarithmic`. Katso lisätietoja [määrittäminen interpolations](#bkmk_interpolation) . |
| `magnitude` | Funktion näkyvissä pistemäärä sitä käytetään muuta luokittelut numeerisen kentän arvojen alueen perusteella. Joitakin yleisimmät käyttöesimerkkejä nämä ovat seuraavat:<ul><li>Luokitusten tähti: Muuta näkyvissä pistemäärä "Tähti arviointi"-kentän arvon perusteella. Kaksi kohdetta liittyvät, kun kohde, jonka korkeampi luokitus näkyvät ensin.</li><li>Reunuksen: Kun kahden asiakirjan liittyvät, jälleenmyyjälle halutessasi edistää asiakirjoja, joissa on suurempi reunukset ensin.</li><li>Valitse arvo: sovellukset, jotka seurata napsauttamalla toimintoja tuotteiden tai sivuja, voit käyttää sitä tehon lisäys kohteet, jotka yleensä Hae eniten liikennettä.</li><li>Lataa laskee: sovellusten Jäljitä lataukset-suuruus-funktion avulla voit lisätä kohteita, jotka ovat eniten ladattavissa.</li></ul> |
| `magnitude:boostingRangeStart` | Määrittää alueen, jonka kautta sitä tulos aloitusarvo. Arvon on oltava kokonaisluku tai liukuluku. 1 – 4 tähden luokitukset tämä olisi 1. Yli 50 % reunusasetukset olisi 50. |
| `magnitude:boostingRangeEnd` | Määrittää alueen, jonka kautta sitä tulos loppuarvo. Arvon on oltava kokonaisluku tai liukuluku. 1 – 4 tähden luokitukset tämä olisi 4. |
| `magnitude:constantBoostBeyondRange` | Kelvollisia arvot TOSI tai EPÄTOSI (oletus). Kun arvo on TOSI, koko tehon lisäys säilyvät tiedostoja, joissa on arvo, kohde-kentän, joka on suurempi kuin alueen ylemmässä loppuun. Jos arvo on EPÄTOSI, tämän funktion tehon lisäys ei voi käyttää arvoa, joka on välin ulkopuolella alueen kohdekentän asiakirjoja. |
| `freshness` | Tuoreus näkyvissä pistemäärä funktiota käytetään muuttavat tulosten luokittelua tulosten kohteiden DateTimeOffset-arvoa kenttien arvojen perusteella. Kohteen uudempaan päivämäärän, voit esimerkiksi luokitellut suurempi kuin vanhoja kohteita. (Huomaa, että se on myös mahdollista arvon.mukaan kohteita, kuten kalenteritapahtumiin kanssa tuleville päivämäärille siten, että kohteet lähemmäksi päiväksi voit asetettavien suurempi kuin kohteet edelleen tulevaisuudessa.) Palvelun nykyinen versio alueen toinen pää korjataan nykyisen kellonajan. Toinen pää on menneisyydessä, perusteella `boostingDuration`. Jos edistää joskus tulevaisuudessa solualueen Käytä negatiivisia arvoja `boostingDuration`. Korko, jolla kehittämällä muuttuu enintään ja alueen pienin määräytyy interpoloimalla käytetty tulosmalli profiilin (Katso alla olevassa kuvassa). Käännä lisäyksen kerroin, jota käytetään, valitse tehon lisäys kerroin on pienempi kuin 1. |
| `freshness:boostingDuration` | Määrittää voimassaoloajan jälkeen mitä kehittämällä ne eivät enää tietyn tiedoston. Katso [määrittäminen boostingDuration] [#bkmk_boostdur] seuraavassa osassa syntaksista ja esimerkkejä. |
| `distance` | Tulosmalli-funktiota käytetään vaikuttavat etäisyys miten perustuvat asiakirjat kirjainarvosanan Sulje tai paljon ne ovat viittaus maantieteellisen sijainnin suhteessa. Viittaus sijainti on määritetty osana parametri kyselyn (käyttämällä `scoringParameter` kyselyn parametri) kuin pitkä lat-argumentti. |
| `distance:referencePointParameter` | Parametrin välittämisen viittaus sijainniksi käyttäminen kyselyissä. scoringParameter on kyselyparametri. [Etsi asiakirjoista](search-api-2015-02-28-preview.md#SearchDocs) saat kyselyparametrit kuvauksia. |
| `distance:boostingDistance` | Luku, joka ilmaisee etäisyys kilometreiksi lisäyksen alueen päättymiskohtaa viittaus sijainnista. |
| `tag` | Funktion näkyvissä pistemäärä tunnisteen käytetään vaikuttavat tunnisteet asiakirjoissa ja hakukyselyt perustuvat asiakirjat kirjainarvosanan. Asiakirjoja, joissa on yhteisiä hakukyselyn tunnisteet tehosti. Hakukyselyn tunnisteet on annettu tulosmalli parametrina kunkin hakupyyntöä (käyttämällä `scoringParameter` kyselyn parametri). |
| `tag:tagsParameter` | Parametrin välittämisen kyselyissä, voit määrittää tietyn pyynnön tunnisteet. `scoringParameter`on kyselyparametri. [Etsi asiakirjoista](search-api-2015-02-28-preview.md#SearchDocs) saat kyselyparametrit kuvauksia. |
| `functionAggregation` | Valinnainen. Koskee vain, kun funktiot on määritetty. Kelvolliset arvot: `sum` (oletus) `average`, `minimum`, `maximum`, ja `firstMatching`. Etsi-tulos on yksittäinen arvo, joka on laskettu-useita muuttujia, mukaan lukien usean funktion. Tämä määritteet ilmaisee, miten kaikki funktiot Suunnittelujärjestelmäyrityksen yhdistetään yhteen kooste-tehon-lisäys, jota käytetään sitten perusasiakirja kirjainarvosanan. Perus kirjainarvosanan perustuu luvut asiakirjan ja hakukyselyn tf idf-arvo. |
| `defaultScoringProfile` | Suoritettaessa hakupyyntöä, jos tulosmalli profiilia ei ole määritetty, valitse oletusarvoisesti näkyvissä pistemäärä on käytetty (tf-idf vain). Oletusarvoisesti näkyvissä pistemäärä profiilinimi voidaan määrittää, aiheuttaa Azure haun ja käyttää profiilin, kun muuta profiilia ei määritetä hakupyyntöä. |

<a name="bkmk_interpolation"></a>
## <a name="set-interpolations"></a>Määritä interpolations

Interpolations avulla voit määrittää kulmakerroin, jonka kehittämällä kasvaa alueen alusta loppuun alueen kirjainarvosanan. Voit käyttää seuraavia interpolations:

  - `Linear`: Viittaamaan kohteisiin, joita ovat suurin ja pienin arvo tehon lisäys käytetty kohteeseen tehdään jatkuvasti määrää. Lineaarinen on oletusarvoinen Interpolaatio tulosmalli profiili.
  - `Constant`: Viittaamaan kohteisiin, joita ovat alkamis- ja alue-vakion tehon lisäys käytetään arvon.mukaan tuloksiin.
  - `Quadratic`:-Verrattuna lineaarisen interpoloinnin, jossa on jatkuvasti laskevan tehon lisäys-toisen asteen yhtälön vähenee alun perin pienempi tahdissa ja sitten Lopeta alueen lähestyessä se se pienenee paljon suurempi aikaväli. Lomitettujen-asetus ei sallita tunnisteen näkyvissä pistemäärä funktiot.
  - `Logarithmic`:-Verrattuna lineaarisen interpoloinnin, jossa on jatkuvasti laskevan tehon lisäys-Logaritminen vähenee alun perin suurempi tahdissa ja sitten Lopeta alueen lähestyessä se se pienenee paljon pienempi aikaväli. Lomitettujen-asetus ei sallita tunnisteen näkyvissä pistemäärä funktiot.

<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>
## <a name="set-boostingduration"></a>Määritä boostingDuration

`boostingDuration`on määrite tuoreus-funktiota. Sen avulla voit määrittää vanheneminen piste jälkeen mitä kehittämällä ne eivät enää tiettyä asiakirjaa. Haluat edistää tuotteen viiva tai merkki 10 päivää markkinointikampanja ajaksi, Määritä 10 päivää määräajan nimellä "P10D" tiedostot. Tai edistää tulevista tapahtumista seuraavalla viikolla määrittää "-P7D".

`boostingDuration`on muotoiltava XSD "dayTimeDuration" arvo (rajoitettu alijoukko on ISO 8601 kesto-arvo). Tämä kaava on: `[-]P[nD][T[nH][nM][nS]]`.

Seuraavassa taulukossa on useita esimerkkejä.

| Kesto | boostingDuration |
|----------|------------------|
| 1 päivä | "P1D" |
| 2 päivää ja 12 tuntia | "P2DT12H" |
| 15 minuutin | "PT15M" |
| 30 päivää, 5 tuntia, 10 minuutin, ja 6.334 sekuntia | "P30DT5H10M6.334S" |

Katso Lisää esimerkkejä [XML-rakenteen: tietotyypit (W3.org sivuston)](http://www.w3.org/TR/xmlschema11-2/).

**Katso myös**
MSDN-sivuston[Azure Search-palvelun REST-Ohjelmointirajapinnalla](http://msdn.microsoft.com/library/azure/dn798935.aspx) <br/>
MSDN-sivuston [Luo indeksi (Azure haku API)](http://msdn.microsoft.com/library/azure/dn798941.aspx)<br/>
[Lisää tulosmalli profiilia hakuindeksin](http://msdn.microsoft.com/library/azure/dn798928.aspx) MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
