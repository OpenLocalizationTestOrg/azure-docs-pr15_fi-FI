<properties
    pageTitle="Azure-portaalissa Azure haun indeksin luominen | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Luo indeksi Azure-portaalissa."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Azure-portaalissa Azure haun indeksin luominen
> [AZURE.SELECTOR]
- [Yleiskatsaus](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [MUILLE KÄYTTÄJILLE](search-create-index-rest-api.md)

Tässä artikkelissa opastaa luominen Azure haun [indeksi](search-what-is-an-index.md) Azure-portaalissa.

Ennen tämän oppaan seuraavan ja indeksin, sinulla on oltava jo [luotu Azure-hakupalvelun](search-create-service-portal.md).


## <a name="i-go-to-your-azure-search-blade"></a>Voin. Siirry Azure haku-sivu
1. Valitse vasemmalla puolella [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) -valikossa "Kaikkien resurssien"
2. Valitse Azure-hakupalvelu

## <a name="ii-add-and-name-your-index"></a>II. Lisää ja nimeä indeksi
1. Napsauta "Lisää hakemisto"-painiketta
2. Nimeä Azure-hakuindeksin. Koska luodaan indeksin Etsi hotellit tässä oppaassa, emme ovat nimeltään indeksi "Hotellit".
  * Indeksinimi on alettava kirjaimella ja sisältää vain pieniä kirjaimia, numeroita tai katkoviivat ("-").
  * Kaltaisilta palvelunimi Indeksinimi, voit valita myös osallistuvat päätepisteen URL-osoite Jos lähetät HTTP-pyyntöjen Azure haun API
3. Valitse Avaa uusi sivu-kenttien"-tapahtuma

![](./media/search-create-index-portal/add-index.png)


## <a name="iii-create-and-define-the-fields-of-your-index"></a>III. Luo ja määritä indeksi kentät
1. "Kentät" tapahtuma valitsemalla Uusi sivu avautuu lomake ja anna indeksimääritys.
2. Kenttien lisääminen lomakkeeseen indeksi.

  * Tyypin Edm.String *avain* -kenttä on pakollinen jokaisen Azure hakuindeksin. Tämä kenttä luodaan oletusarvoisesti kentän nimi "tunnus". Olemme muuttanut sen "hotelId", indeksi.
  * Indeksi-rakenteen tiettyjä ominaisuuksia voidaan lisätä vain kerran, eikä sitä voi päivittää myöhemmin. Tästä syystä rakenteen päivityksiä, jotka vaativat uudelleenindeksointi kuten kenttätyypit eivät ole tällä hetkellä mahdollista alkuperäinen määritysten jälkeen.
  * Microsoft on valinnut huolellisesti kunkin kentän mukaan, miten on mielestäsi niitä käytetään sovelluksen ominaisuusarvoihin. Etsi käyttäjä kokemus ja business tarpeitasi pitää mielessä, kun suunnittelet indeksi, kuten kunkin kentän täytyy olla vastaanotto [haluamasi ominaisuudet](https://msdn.microsoft.com/library/azure/dn798941.aspx). Kentät, jotka koskevat nämä ominaisuudet määrittää, mitkä Etsi ominaisuuksia (suodatus, faceting, lajittelu, teksti-haun, jne.). Esimerkiksi on todennäköistä, että hotellit hakeminen henkilöt kiinnostunut "kuvaus"-kenttään vastaa avainsanaa niin on "Searchable"-ominaisuuden teksti-kenttään hakeminen käyttöön.
    * Voit myös määrittää [kielen analyzer](https://msdn.microsoft.com/en-us/library/azure/dn879793.aspx) kunkin kentän napsauttamalla "Analyzer"-välilehden yläreunaan sivu. Näet alla, että olemme olet valinnut Ranskan analyzer kentän indeksi tarkoitettu Ranskan teksti.

3. Valitse **OK** ja vahvista kenttien määritykset "Kentät"-sivu
4. Valitse Tallenna ja luo vain määrittämäsi indeksi "Lisää hakemisto"-sivu valitsemalla **OK** .

Alla näyttökuvat näet miten syy on nimetty ja määritetty kentät "Hotellit" indeksi.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next"></a>Seuraava
Kun olet luonut Azure-hakuindeksin, voi haluat [ladata sisältöä hakemiston kyselyjä](search-what-is-data-import.md) , voit ryhtyä tietojen etsimistä.
