<properties
    pageTitle="Miten malli monimutkaisten tietotyyppien Azure hakutoiminnossa | Microsoft Azure-haku"
    description="Sisällä tai hierarkkisten tietojen rakenteet voidaan mallintaa Azure haun indeksissä litistetty Rivijoukko ja sivustokokoelmat-tietotyyppi."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="complex data types; compound data types; aggregate data types"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="09/07/2016"
    ms.author="liamca"
/>

# <a name="how-to-model-complex-data-types-in-azure-search"></a>Miten mallin monimutkaisten tietotyyppien Azure hakutoiminnossa

Täytä Azure-hakuindeksin joskus käyttää ulkoisia tietojoukkoja ovat hierarkkisia tai sisäkkäisiä alirakenteita, joka ei ole Jaa siististi taulukkomuotoinen Rivijoukko. Esimerkkejä kyseiset rakenteet saattavat sisältää useita sijainnit ja puhelinnumerot yhteen asiakas, useita värit ja koot for yhden yksittäisen kirjan useiden tekijöiden tuote ja niin edelleen. Mallinnus ehtoja, saatat nähdä tarkoitettu *monimutkaisten tietotyyppien*, *yhdistelmä tietotyyppejä*, *Rakenteiset tietotyypit*tai *koota tietotyyppejä*, muun muassa seuraavia rakenteet.

Monimutkaisten tietotyyppien ei grafiikkatiedostomuotoja tueta Azure haun, mutta osoitettu Vaihtoehtoinen menetelmä on kaksivaiheinen prosessi litistämistä rakenne ja jakomenetelmä sisäinen rakenteen **kokoelman** tietotyyppi avulla. Tässä artikkelissa kuvattuja tekniikka noudatetaan sisältöä haetaan, kohdistettua, suodattaa ja lajitella.

## <a name="example-of-a-complex-data-structure"></a>Esimerkki monimutkaisia tietorakenne

Poistettavaan tiedot sijaitsevat yleensä joukon JSON tai XML-tiedostojen tai kohteiden NoSQL säilöön, kuten DocumentDB. Rakenne-pääse useita alatason kohteita, jotka on etsitty ja suodatettu korostaa haasteellista.  Hyvin menetelmä pohjana tee seuraava JSON-asiakirja, jossa näkyvät yhteystiedot joukko esimerkkinä:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Kun kenttien nimet "tunnus", "nimi" ja "yrityksen" helposti voidaan määrittää Azure-hakuindeksin kenttiä kuin yksi-yhteen, "sijainnit"-kentässä on matriisin sijainnit, ottaa sekä joukon sijainti tunnukset sekä sijainti kuvaukset. Koska Azure haku ei ole tietotyyppiä, joka tukee tätä, annettava malliin tämä Azure haussa eri tavalla. 

> [AZURE.NOTE] Tällä menetelmällä myös kuvaaman Kirk Evans blogimerkinnässä [Indeksoinnin DocumentDB Azure haulla](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), joka näkyy tekniikka nimeltä "litistämistä tiedot", jossa on nimisen kentän `locationsID` ja `locationsDescription` , jotka ovat sekä [sivustokokoelmia](https://msdn.microsoft.com/library/azure/dn798938.aspx) (tai matriisin merkkijonot).   

## <a name="part-1-flatten-the-array-into-individual-fields"></a>Osa 1: Litistä taulukon yhdeksi yksittäisiä kenttiä

Azure haun indeksin, joka vastaa tämän tietojoukko, luotava sisäkkäisiä substructure yksittäisiä kenttiä: `locationsID` ja `locationsDescription` tietotyypin [sivustokokoelmat](https://msdn.microsoft.com/library/azure/dn798938.aspx) (tai matriisin merkkijonot). Näiden kenttien arvot "1" ja "2" indeksoida kyselyjä `locationsID` John Smith ja arvot "3" ja "4" kentän arvoa `locationsID` kentän Jen Campbell varten.  

Tietojen sisällä Azure haun näyttää tältä: 

![Mallitietojen 2 riviä](./media/search-howto-complex-data-types/sample-data.png)


## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>Osa 2: Lisää sivustokokoelman kentän indeksimääritys

Kenttien määritykset voi näyttää seuraavanlaiselta tässä esimerkissä indeksi-mallissa.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Vahvista haun toiminnan ja voit myös laajentaa indeksi

Jos olet luonut indeksi ja ladata tiedot, voit testata nyt ratkaisun Tarkista haun kyselyn suorittaminen dataset vastaan. **Sivustokokoelman** kunkin kentän on oltava **haettavissa olevia**, **suodattaa** ja **facetable**. Sinun pitäisi tehdä kyselyjä, kuten:

* Etsi kaikki henkilöt, jotka työskentelevät Adventureworks osoite.
* Hae 'Aloitus Office' työskentelevien henkilöiden määrän laskeminen.  
* Henkilöt, jotka työskentelevät 'aloitus Office-Näytä mitä toimistojen määrä kussakin sijainnissa henkilöiden kanssa toimivat.  

Jos tällä menetelmällä kuuluu osiin ei, jos sinun täytyy suorittaa haun, joka yhdistää sijainnin tunnus sekä sijainnin kuvaus. Esimerkki:

* Etsi kaikki henkilöt, joihin heillä on aloitus Office ja on sijainti-tunnus on 4.  

Jos muista alkuperäinen sisältö Etsitään tältä:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Kuitenkin, että tiedot on jaettu eri kentissä, on tiedät, jos ei voi aloitus Office for Jen Campbell liittyvät `locationsID 3` tai `locationsID 4`.  

Tässä tapauksessa käsittelemään määrittää toisen kentän indeksin, joka yhdistää kaikki tiedot yhteen sivustokokoelman.  Tässä esimerkissä on soittavat kentän `locationsCombined` ja erotetaan sisällön `||` vaikka voit valita erotin, joka mielestäsi on yksilöllinen merkkiyhdistelmän sisällölle. Esimerkki: 

![Mallitietojen erottimella 2 riviä](./media/search-howto-complex-data-types/sample-data-2.png)

Käyttämällä tätä `locationsCombined` -kentässä on nyt mahtuu entistä enemmän kyselyjä, kuten:

* Näytä henkilöt, jotka työskentelevät "aloitus Officen" sijainnin tunnus "4" määrä.  
* Hakeminen henkilöt, jotka työskentelevät 'aloitus Office-tunnuksen "4" sijaintia. 

## <a name="limitations"></a>Rajoitukset

Tätä tapaa kannattaa käyttää skenaariot määrän, mutta se ei ole käytettävissä kaikissa tapauksissa.  Esimerkki:

1. Jos monimutkaisia tietotyyppi ei ole staattinen joukko kenttiä ja oli ei voi yhdistää kaikki vaihtoehdot yksittäiseen kenttään. 
2. Sisäkkäiset objektit päivittämiseen tarvitaan ylimääräisiä työskennellessäsi voit selvittää tarkalleen mikä on päivitettävä Azure hakuindeksin

## <a name="sample-code"></a>Esimerkki koodi

Esimerkki indeksoida monimutkaisia JSON tietojoukon Azure haun kyselyjä ja suorittaa tämän tietojoukko osoitteessa tämän [GitHub repo](https://github.com/liamca/AzureSearchComplexTypes)kyselyjen määrän.

## <a name="next-step"></a>Seuraava vaihe

[Monimutkaisten tietotyyppien tuki äänestä](https://feedback.azure.com/forums/263029-azure-search) Azure haun UserVoice-sivulle ja lisää käyttäjältä, jotka haluat us huomioitavia koskeva ominaisuuksien käyttöönoton. Voit myös pääset minulle suoraan Twitter- @liamca.


 