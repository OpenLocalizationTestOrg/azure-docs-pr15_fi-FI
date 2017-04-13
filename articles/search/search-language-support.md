<properties
   pageTitle="Luo indeksi asiakirjojen Azure haun useilla kielillä | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
   description=" Azure haun tukee 56 kielet hyödyntäminen kielen analyzers Lucene ja luonnollisen kielen käsittelyn tekniikan Microsoftilta."
   services="search"
   documentationCenter=""
   authors="yahnoosh"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="07/14/2016"
   ms.author="jlembicz"/>

# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Luo indeksi asiakirjojen useilla kielillä Azure haku
> [AZURE.SELECTOR]
- [Portal](search-language-support.md)
- [MUILLE KÄYTTÄJILLE](https://msdn.microsoft.com/library/azure/dn879793.aspx)
- [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)

Unleashing kielen analyzers power on yhtä helppoa kuin yksi ominaisuus etsittävän kentän indeksin määrityksessä. Nyt voit tehdä tämän vaiheen portaalissa.

Seuraavassa on Azure Portal näiden näyttökuvat Azure hakuja, jotka käyttäjät voivat määrittää hakemiston rakenne. Tämä sivu-käyttäjät voivat luoda kaikki kentät ja analyzer ominaisuuden kullekin niistä.

> [AZURE.IMPORTANT] Voit määrittää vain kielen analyzer aikana kentän määrittäminen kuin luodessasi uuden indeksin alkava ylös tai kun lisäät uuden kentän aiemmin luotua indeksiä. Varmista, että määrität täysin attribuutit analysoiminen, mukaan lukien kentän luomisen aikana. Et voi muokata määritteet tai analyzer-tyypin muuttaminen, kun olet tallentanut muutokset.

## <a name="define-a-new-field-definition"></a>Määritä uusi kenttämääritys

1. Kirjautuminen [Azure-portaalin](https://portal.azure.com) ja avaa search-palvelun service-sivu.
2. Valitsemalla **Lisää hakemisto** Aloita uusi indeksi palvelun Raporttinäkymät-ikkunan yläreunassa olevia painikkeita tai avaa aiemmin luotua indeksiä Määritä analyzer uusia kenttiä, jotka olet lisäämässä aiemmin luotua indeksiä.
3. Kentät-sivu tulee näkyviin, jossa sisällytettävien indeksin, rakenne Analyzer-välilehden myös käyttää valitsemalla kielen analyzer.
4. Kentät Aloita kentän määrittäminen nimen antaminen, valitsemalla tietotyyppi ja määritteitä merkitseminen kentän koko tekstin etsittävän, noudatettavan hakutuloksissa käytettävä pinta siirtyminen rakenteet lajiteltavan ja niin edelleen. 
5. Ennen kuin siirryt seuraavaan kenttään, Avaa **Analyzer** -välilehti. 

   
![][1]
*Jos haluat valita analyzer, valitse kentät-sivu Analyzer-välilehti*

## <a name="choose-an-analyzer"></a>Valitse analysoiminen

6. Siirry Etsi määrität kenttä. 
7. Jos et vielä merkitty etsittävän kenttä, valitse nyt, jos haluat merkitä **Searchable**valintaruudun valinta.
8. Napsauta Analyzer Näytä käytettävissä analyzers luettelo.
9. Valitse analyzer, jota haluat käyttää.

![][2]
*Valitse jokin tuettu analyzers kunkin kentän*

Kaikki hakukenttiä käytetään oletusarvon mukaan [Vakio Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) on kielen ympäristöstä riippumattomalla tavalla. Tuetut analyzers täydellinen luettelo on artikkelissa [Azure haun kielten tuesta](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Kun analyzer kieli on valittu kentälle, sitä käytetään indeksoinnin ja Etsi sivupyynnön kentälle. Kun kysely on annettu vastaan käyttämällä eri analyzers useita kenttiä, kysely käsitellään itsenäisesti oikean analyzers kunkin kentän mukaan.

Monta web ja mobile-sovellukset toimivat eri kielillä maapallo ympäri. Se on mahdollista määrittää hakemiston skenaarion tältä luomalla kullekin kielelle tuettu kentän.

![][3]
*Kullekin kielelle tuettu kuvaus-kenttään indeksi määritelmä*

Jos tiedossa on myöntänyt kyselyn agentti kielen, hakupyyntöä on määritetty käyttämällä **searchFields** kyselyn parametri tiettyyn kenttään. Seuraava kysely myönnetään vain vastaan Puola kuvaus:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2015-02-28`

Voit tehdä kyselyn indeksi-portaalista, Liitä samalla tavalla kuin edellä kuvatun kyselyn **haun Resurssienhallinnan** avulla. Erikoishaku on saatavana komentopalkin-palvelu-sivu. Katso lisätietoja [kyselyn Azure haun indeksi-portaalissa](search-explorer.md) .

Joskus varmenteiden kyselyn agentti kieltä ei tiedetä, jolloin kyselyn voi myöntää vastaan kaikki kentät samanaikaisesti. Tarvittaessa tuloksia tietyn kielen asetus voidaan määrittää käyttämällä [näkyvissä pistemäärä profiilit](https://msdn.microsoft.com/library/azure/dn798928.aspx). Seuraavassa esimerkissä löydetty kuvaus-englanniksi vastaavat Tulosta suurempi suhteessa vastineita puola ja Ranska:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2015-02-28`

Jos olet kehittäjä .NET, Huomaa, että voit määrittää kielen analyzers käyttämällä [Azure haun .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search). Uusin versio sisältää myös Microsoft-kielen analyzers tuen.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
