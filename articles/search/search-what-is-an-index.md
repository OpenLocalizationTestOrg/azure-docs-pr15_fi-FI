<properties
    pageTitle="Luo Azure-hakuindeksin | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Mikä on hakemiston Azure haun ja kuinka sitä käytetään?"
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index"></a>Azure-hakuindeksin luominen
> [AZURE.SELECTOR]
- [Yleiskatsaus](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [MUILLE KÄYTTÄJILLE](search-create-index-rest-api.md)

## <a name="what-is-an-index"></a>Indeksin ominaisuudet

*Indeksi* on jatkuva säilö *asiakirjojen* ja muiden rakenteita Azure-hakupalvelun käyttämä. Asiakirja on yksikkönä indeksi haettavissa olevia tietoja. Esimerkiksi e commerce jälleenmyyjältä voi olla kunkin kohteen ne myy asiakirjan, uutisia organisaatio voi olla asiakirjan jokaisen artikkelissa, jne. Yhdistäminen käsitteiden tutun tietokannan vastineet: *indeksi* on käsitteellisesti samanlaiset *taulukon*ja *asiakirjojen* vastaavat suurin piirtein taulukon *rivejä* .

Kun lisääminen ja lataa tiedostot ja hakukyselyjä Azure Etsi, voit lähettää tietyn indeksin pyyntöjen search-palvelun.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Kenttien lajit ja Azure-hakuindeksin määritteet

Kun määrität rakenteen, on määritettävä nimi, tyyppi ja määritteitä kunkin kentän indeksi. Kenttätyyppi luokittelee tietoihin, jotka on tallennettu kyseiseen kenttään. Määritteet määritetään yksittäisiä kenttiä voit määrittää, miten kenttää käytetään. Seuraavissa taulukoissa Luetteloi tyypit ja voit määrittää määritteet.


### <a name="field-types"></a>Kenttien lajit
|Tyyppi|Kuvaus|
|------------|-----------|
|*Edm.String*|Teksti, joka voi myös tokenized teksti-haun (word uusinta, jäsenyydestä ja niin edelleen).|
|*Collection(EDM.String)*|Merkkijonoja, joita voit halutessasi tokenized koko tekstin haun luettelo. Ei ole teoreettinen yläraja kokoelman kohteiden määrän, mutta tietojen kooksi yläraja 16 Mt koskee sivustokokoelmat.|
|*Edm.Boolean*|On TOSI tai EPÄTOSI.|
|*Edm.Int32*|32-bittisen kokonaisluvun arvot.|
|*Edm.Int64*|64-bittisen kokonaisluvun arvot.|
|*Edm.Double*|Kaksinkertainen tarkkuus numeerisia tietoja.|
|*Edm.DateTimeOffset*|Päivämäärä, aika-arvot esitetään OData V4-muodossa (esimerkiksi `yyyy-MM-ddTHH:mm:ss.fffZ` tai `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`).|
|*Edm.GeographyPoint*|Edustava maantieteellisen sijainnin maapalloa piste.|

Löydät Azure hakua [tuetut tietotyypit MSDN-sivuston](https://msdn.microsoft.com/library/azure/dn798938.aspx)tarkempia tietoja.



### <a name="field-attributes"></a>Kentän määritteet
|Määrite|Kuvaus|
|------------|-----------|
|*Avain*|Merkkijono, joka sisältää kunkin asiakirjan, asiakirjan Etsi käytettävä yksilöllinen tunnus. Jokaisen indeksi on oltava yhtä näppäintä. Vain yhden kentän voi olla avain ja sen tyyppi on määritettävä Edm.String.|
|*Noudatettavan*|Määrittää, onko kentän voidaan palauttaa hakutulos.|
|*Suodatettavia*|Kentän käytettävän suodattimen kyselyiden avulla.|
|*Lajiteltava*|Käytät tätä kenttää hakutulosten lajitteleminen kyselyn avulla.|
|*Facetable*|Sallii kentän käytettävä [kohdistettua](search-faceted-navigation.md) siirtymisrakenteen käyttäjän itse ohjattu suodattamista varten. Yleensä kentät, joissa toistuvien arvot, joiden avulla voit ryhmitellä useita asiakirjoja yhteen (esimerkiksi useita tiedostoja, jotka kuuluvat yksittäinen merkki tai palveluluokan) toimivat parhaiten rakenteita.|
|*Haettavat*|Merkitsee teksti-kenttään etsittävän.|

Löydät lisätietoja Azure haun [Indeksimääritteet MSDN-sivuston](https://msdn.microsoft.com/library/azure/dn798941.aspx).



## <a name="guidance-for-defining-an-index-schema"></a>Lisäohjeita määrittäminen indeksi-rakenne

Kun suunnittelet indeksi, tutustu huomioon otettavia läpi kunkin päätös suunnitteluvaiheessa. On tärkeää, että pidät Etsi käyttäjä kokemus ja business tarpeitasi mielessä suunniteltaessa indeksi kuin kunkin kentän on määritetty [oikea määritteet](https://msdn.microsoft.com/library/azure/dn798941.aspx). Indeksin muuttaminen, kun se on otettu käyttöön tehdään, joiden ja tiedot uudelleen.


Jos tietojen tallennuksen vaatimukset muuttuvat ajan kuluessa, voit suurentaa tai pienentää kapasiteetin lisäämällä tai poistamalla osiot. Lisätietoja on artikkelissa [Hallitse Search-palvelun Azure-tietokannassa](search-manage.md) tai [Palvelun rajoitukset](search-limits-quotas-capacity.md).
