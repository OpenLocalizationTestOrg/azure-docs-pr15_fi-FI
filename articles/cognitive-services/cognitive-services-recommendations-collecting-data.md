<properties
    pageTitle="Tietoja kouluttaminen mallin kerätään: koneen Koulujen suosituksia Ohjelmointirajapinta | Microsoft Azure"
    description="Azure koneen Koulujen suositukset - kouluttaminen mallin avulla tapahtuvan tiedonkeräyksen"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="luisca"/>

#  <a name="collecting-data-to-train-your-model"></a>Tietoja kouluttaminen mallin kerätään #

Suositukset-Ohjelmointirajapinnan oppii-Etsi kohteita olisi edellisten tapahtumien suositeltavia tietyn käyttäjän.

Kun olet luonut mallin, sinun on antaa kaksi tieto, ennen kuin voit tehdä minkä tahansa koulutus: luettelotiedoston ja käyttötiedot.

>   Jos et ole vielä niin, on suositeltavaa suorittamiseen [pikaopas](cognitive-services-recommendations-quick-start.md).


## <a name="catalog-data"></a>Luettelotiedot ##

### <a name="catalog-file-format"></a>Luettelo-tiedostomuoto ###

Luettelotiedosto sisältää tietoja tarjoat asiakkaalle.
Luettelon tietojen pitäisi seurata seuraavanlainen:

- Ominaisuudet - ilman`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Ominaisuudet - kanssa`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

#### <a name="sample-rows-in-a-catalog-file"></a>Esimerkki rivien luettelotiedosto

Ilman ominaisuuksia:

    AAA04294,Office Language Pack Online DwnLd,Office
    AAA04303,Minecraft Download Game,Games
    C9F00168,Kiruna Flip Cover,Accessories

Ominaisuuksia:

    AAA04294,Office Language Pack Online DwnLd,Office,, softwaretype=productivity, compatibility=Windows
    BAB04303,Minecraft DwnLd,Games, softwaretype=gaming,, compatibility=iOS, agegroup=all
    C9F00168,Kiruna Flip Cover,Accessories, compatibility=lumia,, hardwaretype=mobile

#### <a name="format-details"></a>Muotoile tiedot

| Nimi | Pakollinen | Tyyppi |  Kuvaus |
|:---|:---|:---|:---|
| Kohteen tunnus |Kyllä | [A-ö], [-ö], [0-9], [_] & #40; Alaviiva & #41; [-] & #40; viiva & #41;<br> Enimmäispituus: 50 | Yksilöllinen tunnus kohteen. |
| Kohteen nimi | Kyllä | Aakkosnumeerisia merkkejä<br> Enimmäispituus: 255 | Kohteen nimi. |
| Kohteen luokka | Kyllä | Aakkosnumeerisia merkkejä <br> Enimmäispituus: 255 | Luokka, johon kohdetta kuuluu (esimerkiksi ruoanlaitto kirjat, näytelmiä...); voi olla tyhjä. |
| Kuvaus | Ei, ellet ominaisuudet ovat esittää (mutta voi olla tyhjä) | Aakkosnumeerisia merkkejä <br> Enimmäispituus: 4000 | Kohteen kuvaus. |
| Ominaisuuksien luettelo | Ei | Aakkosnumeerisia merkkejä <br> Enimmäispituus: 4000; Ominaisuudet: 20 enimmäismäärä | Ominaisuuden nimi CSV-luettelon = ominaisuuden arvo, jonka avulla voidaan parantaa mallin suositus.|

#### <a name="uploading-a-catalog-file"></a>Luettelotiedoston lataaminen

Tarkista luettelotiedoston lataamisessa [API viittaus](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1) .  

Huomaa, että luettelotiedoston sisällön välitetty pyyntö tekstinä.

Jos useita luettelotiedostojen lataaminen useita soitettaessa samaan tietomalliin, emme Lisää vain uudet luettelokohteet. Muiden kohteiden säilyy alkuperäisessä arvoilla. Luettelotiedot voi päivittää tällä menetelmällä.

>   Huomautus: Suurin sallittu tiedostokoko on 200 Megatavua.
>   Tuetut luettelon kohteiden enimmäismäärä on 100 000 kohteet.


## <a name="why-add-features-to-the-catalog"></a>Toimintojen lisääminen luettelo miksi?

Suositukset-ohjelma luo tilastollinen mallin, joka ilmoittaa, tykännyt tai ostaa asiakkaan todennäköisesti kohteet. Kun sinulla on uusi tuote, joka on aina projektitietoja sitä ei voida luoda muiden esiintymien mallin yksin. Oletetaan, että aloitat uuden ojentamassa "lasten violin" säilöstä, koska olet koskaan myynyt kyseisen violin ennen ei tiedä, mitä muut kohteet suositeltavaa, että violin kanssa.

Joka said, jos moduulin tietää, että violin tietoja (eli musical instrument on lasten iät 7 – 10, se ei ole kallista violin, jne.), valitse moduulin voit oppia muiden tuotteiden kanssa vastaavia toimintoja. Esimerkiksi olet myynyt violin's aiemman ja yleensä henkilöt, jotka ostavat viulut yleensä ostaa klassinen musiikkia CD- ja taulukon musiikkia Metsikköjen.  Järjestelmä etsiä näiden ominaisuuksien väliset yhteydet ja antaa suosituksia ominaisuuksien perusteella samalla, kun uusi violin on pieni käyttö.

Osana luettelotiedot on tuotu ominaisuudet ja sitten niiden järjestys (tai mallin ominaisuus tärkeyttä) on yhdistetty kun arvon.mukaan muodosta on valmis. Ominaisuuden arvon muuttaa mallia käyttötiedot ja kohteiden lajin mukaan. Mutta yhdenmukainen käyttö/kohteet järjestyksen pitäisi olla vain pienen vaihteluista. Ominaisuuksien tietojoukon on muu kuin negatiivinen luku. Numero 0 tarkoittaa, että toiminto luokitellut ei (tapahtuu, jos suoritat ensimmäisen arvon.mukaan muodosta loppuun ennen tämän API). Päivämäärä, johon järjestyksen tehtäväksi kutsutaan pistemäärän tuoreus.


###<a name="features-are-categorical"></a>Ominaisuudet ovat Categorical

Tämä tarkoittaa, että sinun on luotava ominaisuuksia, jotka muistuttavat luokka. Esimerkiksi hinta = 9.34 ei ole categorical ominaisuutta. Toisaalta-ominaisuutta, kuten priceRange = Under5Dollars categorical toimintoa. Toinen yleinen virhe on käyttää ominaisuutena kohteen nimi. Tämä aiheuttaa kohteen on yksilöllinen, jotta ne eivät kuvaa luokan nimi. Varmista, että ominaisuudet vastaavat kohteet luokkiin.


###<a name="how-manywhich-features-should-i-use"></a>Kuinka monta ja jolla ominaisuudet kannattaa käyttää?


Kädessä suositukset luominen tukee rakentamiseen enintään 20 ominaisuuksilla. Voit määrittää enintään 20 ominaisuuksia kohteiden luettelon, mutta on tarkoitus tehdä luokittelu muodosta ja valitse vain ominaisuuksia, jotka kuuluvat ryhmään suuri. (Tietojoukon 2.0 tai Lisää ominaisuus on hyvä ominaisuus käyttää!). 


###<a name="when-are-features-actually-used"></a>Kun ominaisuuksia todella käytetään?

Ominaisuudet käyttävät mallia ei ole tarpeeksi tapahtumatietojen antaa suosituksia tapahtumatietoja yksinään. Toiminnot saa niin mahdollisimman hyvin "kylmän kohteiden" – kohteiden muutamia tapahtumien kanssa. Jos kaikki kohteet on riittävän tapahtumatietoja, joudut ehkä ole täydentää mallin ominaisuuksia.


###<a name="using-product-features"></a>Tuote-ominaisuuksilla

Toimintojen käytön osana oman muodosta, sinun on:

1. Varmista, että luettelo sisältää toimintoja, kun lataat sen.

2. Käynnistää luokittelu muodosta. Tämä tällöin toimi ominaisuuksia tärkeys/tietojoukon analyysi.

3. Käynnistää suosituksia muodosta, seuraavan luominen parametrit: useFeaturesInModel arvoksi TOSI, TOSI, allowColdItemPlacement ja modelingFeatureList asettaminen pilkuilla erotettu luettelo toiminnoista, jotka haluat parantaa mallin avulla. Lisätietoja on kohdassa [suosituksia luominen tyyppi Parametrit](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0) .





## <a name="usage-data"></a>Käyttötietojen ##
Käyttö-tiedosto sisältää tietoja siitä, miten kohteiden käytetään ja yrityksesi tapahtumat.

#### <a name="usage-format-details"></a>Muotoile käyttötiedot
Käyttö-tiedosto on (Luetteloerottimella erotetut arvot) CSV-tiedoston missä kullekin riville käyttö tiedoston kuvaa vuorovaikutuksen käyttäjän ja kohteen välillä. Kullakin rivillä on muotoiltu seuraavasti:<br>
`<User Id>,<Item Id>,<Time>,[<Event>]`



| Nimi  | Pakollinen | Tyyppi | Kuvaus
|-------|------------|------|---------------
|Käyttäjätunnus|         Kyllä|[A-ö], [-ö], [0-9], [_] & #40; Alaviiva ja #41; [-] & #40; viiva ja #41;<br> Enimmäispituus: 255 |Käyttäjän yksilöllinen.
|Kohteen tunnus|Kyllä|[A-ö], [-ö], [0-9], [& #95;] & #40; Alaviiva & #41; [-] & #40; viiva & #41;<br> Enimmäispituus: 50|Yksilöllinen tunnus kohteen.
|Aika|Kyllä|Päivämäärä-muodossa: VVVV/KK/ppTHH (kuten 2013/06/20T10:00:00)|Tietoja aika.
|Tapahtuman|Ei | Jokin seuraavista toimista:<br>• Napsauttamalla<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Hankkiminen| Tapahtuman tyyppi. |

#### <a name="sample-rows-in-a-usage-file"></a>Esimerkki rivien käyttö-tiedosto

    00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase
    0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase
    0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase
    00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase
    0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase
    00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

#### <a name="uploading-a-usage-file"></a>Käyttö-tiedoston lataaminen

Tarkista käyttö tiedostojen lataamisesta [API viittaus](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2) .
Huomaa, että haluat välittää käyttö-tiedoston sisällön HTTP-puhelun tekstinä.

>  Huomautus:

>  Tiedoston enimmäiskoko: 200 Megatavua. Useita käyttö-tiedostoja voi ladata.

>  Sinun on ladattava luettelotiedoston, ennen kuin aloitat käyttötietojen lisääminen malliin. Vain luettelotiedoston kohteita käytetään koulutus vaiheen aikana. Kaikki muut kohteet ohitetaan.

## <a name="how-much-data-do-you-need"></a>Kuinka paljon tietoja on?

Mallin äänenlaatu on raskaasti riippuvainen laatu ja tietojen määrä.
Järjestelmä oppii, kun käyttäjä ostaa eri kohteille (olemme kutsua tämän muiden esiintymien). Se on myös tärkeää tietää, mitkä kohteet ostetaan samat tapahtumat muuttaa FBT-Maksuerien versiot. 

Hyvä peruslähtökohta on useimpien kohteiden 20 tapahtumia tai enemmän, joten Suosittelemme, että sinulla on vähintään 20 kertaa määrä tapahtumien tai noin 200,000 tapahtumat Jos haluat käyttää luettelon 10 000 kohdetta,. Uudelleen Tämä on hyvä peruslähtökohta. Haluat kokeilla tiedot.

Kun olet luonut mallin, voit suorittaa [offline-tilassa arvioinnin](cognitive-services-recommendations-buildtypes.md) voit tarkistaa, kuinka hyvin mallin todennäköisesti suorittamiseen.
