<properties
    pageTitle="Azure hakuindeksin kyselyn | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Muodosta Azure hakutoiminnossa hakukyselyn ja Etsi parametreilla, suodattaminen ja lajitteleminen hakutulokset."
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

# <a name="query-your-azure-search-index"></a>Azure-hakuindeksin kyselyn
> [AZURE.SELECTOR]
- [Yleiskatsaus](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [MUILLE KÄYTTÄJILLE](search-query-rest-api.md)

Search-pyyntöjen Azure Etsi lähetettäessä on määrä parametreja, jotka voidaan todellinen sanat, jotka olet kirjoittanut hakuruutuun sovelluksesi rinnalla. Nämä kyselyparametrit mahdollistavat joitakin tarkempaa hallinnan teksti-hakuja saavuttamiseksi.

Alla on luettelo, jossa kerrotaan lyhyesti kyselyparametrit Azure hakutoiminnossa Yleiset käyttötavat. Koko soveltamisala kyselyparametrit ja toiminnat [REST API](https://msdn.microsoft.com/library/azure/dn798927.aspx) ja [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.searchparameters_properties.aspx)on yksityiskohtaiset sivut.

## <a name="types-of-queries"></a>Kyselytyyppejä

Azure haku on useita vaihtoehtoja hyvin tehokas kyselyjen luominen. Kysely, jota käytät kaksi päätyyppiä ovat `search` ja `filter`. A `search` kyselyn yksi tai useampi ehto etsii _kaikki hakukenttiä, indeksi-_ ja toimii tuttuun tapaan hakukone, kuten Google tai Bing toimimaan haluamallasi tavalla. A `filter` kyselyn arvioi totuusarvolauseke kaikista _suodatettavia_ indeksin kenttien. Toisin kuin `search` kyselyt- `filter` kyselyjen vastaavat kentän, mikä tarkoittaa, että ne kirjainkoko on merkitsevä merkkijonon kenttien tarkka sisältöä.

Voit käyttää haut ja suodattimet yhdessä tai erikseen. Jos käytät ne yhteen, suodatinta käytetään ensin koko indeksissä ja sitten haku suoritetaan suodattimen tuloksiin. Suodattimet vuoksi voi olla hyötyä menetelmällä parantaa kyselyn suorituskykyä, sillä ne vähentää Asiakirjajoukon hakukyselyn on käyttänyt.

Suodatinlausekkeiden syntaksi on alijoukkoa [OData suodattimen kieli](https://msdn.microsoft.com/library/azure/dn798921.aspx). Hakukyselyjen voit [yksinkertaistettu syntaksi](https://msdn.microsoft.com/library/azure/dn798920.aspx) tai [Lucene kyselyn syntaksi](https://msdn.microsoft.com/library/azure/mt589323.aspx) , jossa käsitellään alapuolella.

### <a name="simple-query-syntax"></a>Yksinkertaisen kyselyn syntaksi
[Yksinkertaisen kyselyn syntaksi](https://msdn.microsoft.com/library/azure/dn798920.aspx) on Azure haussa käytettävät oletuskielen kyselyn. Yksinkertaisen kyselyn syntaksi tukee useita yleisiä hakuoperaattorit ja-, mukaan lukien VAI ei, lause, jälkiliite ja operaattorien järjestys.

### <a name="lucene-query-syntax"></a>Lucene kyselyn syntaksi
[Lucene kyselysyntaksia](https://msdn.microsoft.com/library/azure/mt589323.aspx) avulla voit käyttää laajasti hyväksytään ja ilmeikäs kyselykielen kehittänyt [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)osana.

Tämä kyselysyntaksia avulla voit helposti saavuttamiseksi seuraavia ominaisuuksia: [kentän Kohdistettu kyselyt](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fields), [epäselvältä haku](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fuzzy), [lähialue haun](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_proximity), [termi kehittämällä](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_termboost), [säännöllisen lausekkeen haun](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_regex), [yleismerkkien haku](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_wildcard), [syntaksi fundamentals](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_syntax)ja [kyselyjen avulla Totuusarvo-operaattorit](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_boolean).



## <a name="ordering-results"></a>Tulosten lajittelu
Vastaanotettaessa haun kyselyn tuloksia, voit pyytää Azure haku on tuloksia tietyn kentän arvojen mukaan. Oletusarvon mukaan Azure haun tilaukset kunkin asiakirjan haun tulos, joka johdetaan [TF IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)tietojoukon perusteella hakutulokset.

Jos haluat, että Azure haku palauttamaan tulokset tilaamista arvon kuin haun tulokset, voit käyttää `orderby` Etsi parametrin. Voit määrittää arvon `orderby` parametrin sisällytettävien kenttien nimet ja puhelut [ `geo.distance()` funktion](https://msdn.microsoft.com/library/azure/dn798921.aspx) geospatiaalisia arvot. Kunkin lausekkeen perään `asc` osoittamaan tulokset pyydetään nousevassa järjestyksessä ja `desc` osoittamaan, että tulokset pyydetään laskevaan järjestykseen. Oletusarvoinen luokituksen nousevassa järjestyksessä.

## <a name="paging"></a>Sivutuksen
Azure haku on helppo toteuttamisesta sivutus hakutulokset. Käyttämällä `top` ja `skip` parametreja, voit sujuvasti myöntää search-pyyntöjen, jotta voit vastaanottaa yhteensä hakutulosten joukkoon helpommin hallittaviin, tilatut alijoukot, jotka mahdollistavat helposti hyvä haun Käyttöliittymän käytännöt. Nämä pienempi alijoukot tulosten vastaanotettaessa voit vastaanottaa asiakirjojen määrää haun tuloksia yhteensä määrittäminen.

Saat lisätietoja tietoja sivutus on artikkelissa [miten sivun hakutuloksia Azure hakutoiminnossa](search-pagination-page-layout.md)hakutulokset.


## <a name="hit-highlighting"></a>Osumien korostaminen
Azure hakutoiminnossa korostavat hakutuloksia, jotka vastaavat hakukyselyn tarkka osa on helppo avulla `highlight`, `highlightPreTag`, ja `highlightPostTag` parametrit. Voit määrittää _minkä hakukenttiä_ olisi on niiden parilliset tekstin korostuksen sekä määrittäminen tarkan merkkijonon tunnisteet liittäminen alussa ja lopussa yhdistettävää tekstiä, että Azure haku palauttaa.
