<properties
    pageTitle="Aloittaminen Azure haun NodeJS | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Käy läpi isännöityä pilvipalveluun haun Etsi ohjelman pohjalta Azure käyttäen NodeJS oman ohjelmointikieli."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-nodejs"></a>Azure haun NodeJS käytön aloittaminen
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Opettele Luo mukautettu NodeJS Etsintäsovellus, joka käyttää Azure etsiminen sen hakukokemusta. Tässä opetusohjelmassa käyttää [Azure haun palvelun REST API](https://msdn.microsoft.com/library/dn798935.aspx) muodostaa objektit ja käyttää tässä toimintoja.

On käytetty [NodeJS](https://nodejs.org) ja NPM, [Sublime tekstin 3](http://www.sublimetext.com/3)ja Windows PowerShellin Windows 8. 1 kehittämään ja testaa koodi.

Jos haluat suorittaa tämän mallin on oltava Azure Search-palvelun, joka Kirjaudu käyttäjäksi [Azure-portaalissa](https://portal.azure.com). Katso vaiheittaiset ohjeet [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

## <a name="about-the-data"></a>Tietoja

Esimerkkisovellus käyttää tietoja [Yhdysvallat geologiset Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), valitse-tilaan, Rhode saari tietojoukko koon pienentämiseksi suodatettujen. Microsoft search-sovellus, joka palauttaa maamerkki rakennuksia, kuten sairaaloita ja koulut sekä geologiset ominaisuuksia, kuten virtaa, järviä ja huippukokouksissa luominen näitä tietoja käytetään.

Tämän sovelluksen **DataIndexer** ohjelma muodostaa ja lataa indeksin avulla [indeksointitoiminto](https://msdn.microsoft.com/library/azure/dn798918.aspx) -rakennetta, suodatettu USGS tietojoukko noutaminen julkisen Azure SQL-tietokantaan. Tunnistetiedot ja yhteys online-tietolähteen tiedot on annettu ohjelmakoodi. Ei edellytä mitään muita määrityksiä.

> [AZURE.NOTE] Suodatin on käytössä tämän tietojoukko yhteydenpitoon vapaa hinnoittelutiedot taso 10 000 asiakirjan raja-kohdassa. Jos käytät vakio taso, tämä rajoitus ei koske. Lisätietoja hinnoittelutiedot kunkin tason kapasiteetti on artikkelissa [palvelun hakurajoista](search-limits-quotas-capacity.md).


<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Etsi palvelunimi ja Azure-hakupalvelun ohjelmointirajapinnan avain

Kun olet luonut palvelu, palaa portaaliin tarkistamaan URL-osoite tai `api-key`. Yhteyden muodostaminen Search-palvelun vaatii, että molemmat URL-osoite ja `api-key` puhelun tarkistamiseen.

1. Kirjautuminen [Azure Portal](https://portal.azure.com).
2. Napsauta tekstin-palkin **Hakupalvelun** luettelon kaikista valmisteltu tilauksen Azure-Etsi-palveluita.
3. Valitse palvelu, jota haluat käyttää.
4. Palvelun koontinäytössä näet keskeiset tiedot ruudut ja Avainkuvake käyttämiseen järjestelmänvalvoja-näppäimiä.

    ![][3]

5. Kopioi URL-osoite, järjestelmänvalvoja-näppäintä ja kysely-näppäintä. Sinun on kaikki kolme myöhemmin, kun lisäät ne config.js-tiedostoon.

## <a name="download-the-sample-files"></a>Tiedostojen lataaminen

Ladata malli joko jokin seuraavista toimintatavoista avulla.

1. Siirry [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodeJSIndexerDemo).
2. Valitse **Lataa ZIP-**, Tallenna .zip-tiedosto ja Pura se sisältää kaikki tiedostot.

Kaikki myöhemmin tiedoston muutoksia ja suorita lauseet tehdään vastaan tämän kansion tiedostot.


## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a>Päivitä config.js. Search-palvelun URL-osoite ja ohjelmointirajapinnan avain

Määritystiedostossa URL-osoite ja api-näppäimistö, joka on kopioitu aiempaa versiota, Määritä URL-osoite, järjestelmänvalvoja-näppäintä painettuna ja kysely-näppäintä.

Järjestelmänvalvojan näppäimet myöntää palvelutoimintoja, kuten luominen tai indeksin poistaminen ja tiedostojen lataaminen täydet oikeudet. Sen sijaan kyselyn näppäimet ovat vain luku-toimintoja, yleensä käyttämien asiakassovelluksissa, jotka muodostavat yhteyden Azure haku.

Tässä esimerkissä on sisältää kyselyn avain vahvistaminen käyttäminen kyselyn avain asiakassovelluksissa parhaiden käytäntöjen avulla.

Seuraavassa näyttökuvassa näkyy **config.js** Avaa tekstiksi editorin asianmukaisiin tapahtumiin rajaamina, jotta näet, mihin tiedosto Päivitä arvot, jotka ovat kelvollisia search-palvelun kanssa.

![][5]


## <a name="host-a-runtime-environment-for-the-sample"></a>Host otosten runtime käyttöympäristön

Otosten edellyttää HTTP-palvelin, johon voit asentaa käyttämällä yleisesti npm.

Käyttää seuraavia komentoja PowerShell-ikkunaa.

1. Siirry **package.json** tiedoston sisältävä kansio.
2. Kirjoita `npm install`.
2. Kirjoita `npm install -g http-server`.

## <a name="build-the-index-and-run-the-application"></a>Luo sitten varsinainen hakemisto ja suorita sovellus

1. Kirjoita `npm run indexDocuments`.
2. Kirjoita `npm run build`.
3. Kirjoita `npm run start_server`.
4. Suora selaimella osoitteessa`http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>Etsi USGS tietoihin

USGS tietojoukon sisältää tietueet, jotka liittyvät, tila on Rhode saari. Jos valitset **Etsi** tyhjä hakuruutuun, näet yläreunan 50 merkinnät-mikä on oletusarvo.

Kirjoittamalla hakusana avulla hakukonetta jotain Siirry. Kirjoita alueellisen nimi. "Pekka Williams" on Rhode saari ensimmäisen säädin. Monia puistojen, rakennuksia ja koulut ovat nimeltään hänelle jälkeen.

![][9]

Voi myös kokeilla jotakin näistä ehdoista:

- Pawtucket
- Pembroke
- hanhi + Kap


## <a name="next-steps"></a>Seuraavat vaiheet

Tämä on ensimmäinen Azure haun opetusohjelman NodeJS ja USGS tietojoukko perusteella. Ajan myötä on laajentaa Tässä opetusohjelmassa osoittamaan mukautettujen ratkaisujen saatat haluta käyttää muita hakuominaisuudet.

Jos sinulla on jo joitakin taustan Azure hakutoiminnossa, sinun springboard tässä esimerkissä käytetään kokeilujakson suggesters (täydentävä tai automaattinen täydennys-kyselyitä), suodattimien ja kohdistettua siirtymistä varten. Voit parantaa yhteydessä hakutulossivulla lisäämällä määrät ja jonottaminen asiakirjoja, jotta käyttäjät voivat selata tulokset.

Uusi Azure haun? On suositeltavaa kokeilujakson muiden opetusohjelmat kehittää ymmärtämisen voit luoda. Lisätietoja saat lisätietoja Microsoftin [asiakirjat-sivu](https://azure.microsoft.com/documentation/services/search/) . Voit tarkastella linkkejä myös sekä [videon ja sovelluksen luettelon](search-video-demo-tutorial-list.md) enemmän tietoja.

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png
