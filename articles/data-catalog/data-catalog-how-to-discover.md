<properties
   pageTitle="Miten löydät tietolähteitä | Microsoft Azure"
   description="Toimintaohjeet artikkelissa korostaminen voit löytää Azure tietoluettelon, mukaan lukien etsiminen ja suodattaminen ja korostaminen ominaisuuksia Azure tietoluettelon portaalin osumien käyttämällä resurssien rekisteröidyt tiedot."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="how-to-discover-data-sources"></a>Miten löydät tietolähteitä

## <a name="introduction"></a>Johdanto
**Microsoft Azure tietoluettelon** on täysin hallitun pilvipalvelussa, joka on järjestelmän rekisteröinnin ja järjestelmä on enterprise tietolähteiden etsiminen. Toisin sanoen **Azure tietoluettelon** 's all about toimintamallia löytää, toimintaperiaatteet ja käyttäminen tietolähteiden ja auttaa organisaatioita tulee niiden annetuista tiedoista. Kun tietolähde on rekisteröity **Azure tietoluettelon**, sen metatietoja on indeksoitu-palvelu, niin, että käyttäjät voivat helposti etsiä tuttuihin tarvitsemansa tiedot.

## <a name="searching-and-filtering"></a>Hakemisesta ja suodattamisesta

**Azure tietoluettelon** etsiminen käyttää kaksi ensisijainen järjestelmiä: hakemisesta ja suodattamisesta.

Haku on suunniteltu intuitiivinen ja tehokas – oletusarvoisesti, hakusanat, jotka vastaavat vastaan minkä tahansa luettelon, kuten käyttäjän antamaa huomautukset-ominaisuus.

Suodatus on suunniteltu täydentämään haku. Käyttäjät voivat valita tiettyjä ominaisuuksia, esimerkiksi asiantuntijoiden, tietolähdetyyppi, Objektilaji ja tunnisteita, voit tarkastella vain täsmäävät tiedot varat ja haluat rajoittaa haun tulokset vastaavat varat sekä.

Hakemisesta ja suodattamisesta yhdistelmän avulla käyttäjät voivat siirtyä nopeasti tietolähteitä, joka on rekisteröity **Azure tietoluettelon** löydät tietolähteitä, he tarvitsevat.

## <a name="search-syntax"></a>Haun syntaksi

Oletusarvoinen tekstimuotoinen haku on yksinkertainen ja intuitiivinen, mutta käyttäjät voivat käyttää **Azure tietoluettelon**haun syntaksi myös suurempi päättää hakutulokset. **Azure tietoluettelon** haun tukee seuraavia tekniikoita:

| Tekniikka                 | Käytä                                                                                                                                     | Esimerkki                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Perushaun avulla              | Yhden tai useamman hakusanojen käyttämällä perushaun avulla. Tulokset ovat kalusteet, joiden vastaa ominaisuuksista yhden tai useamman: n ehtojen kanssa. | myyntitiedot                                                |
| Ominaisuuden vaikutusalueen määrittäminen          | Palauttaa vain tietolähteitä, jossa hakusanoja verrataan määritetty ominaisuus                                                   | nimi: talousmallit                                              |
| Totuusarvo-operaattorit         | Laajenna tai Tarkenna haun totuusarvo toiminnot                                                                                     | talous ei ole yrityksen                                     |
| Ryhmittelyjen kaarisulje | Käytä sulkeita ryhmän osiin kyselyn saavuttamiseksi looginen eristystaso, erityisesti yhdessä Totuusarvo-operaattorit              | nimi: talous ja (tunnisteet: Q1 tai tunnisteet: Q2) |
| Vertailuoperaattorit      | Käytä vertailuja kuin tasa ominaisuuksia, joilla on numeerista tai päivämäärä-tietotyypit                                                | modifiedTime > "11/05/2014"                                 |

Saat lisätietoja **Azure tietoluettelon** haun [https://msdn.microsoft.com/library/azure/mt267594.aspx](https://msdn.microsoft.com/library/azure/mt267594.aspx).

## <a name="hit-highlighting"></a>Osumien korostaminen
Kun tarkastelet hakutuloksia, näkyviä ominaisuuksia, jotka vastaavat määritettyjä hakuehtoja – tietojen resurssi nimi, kuvaus ja tunnisteita – kuten korostetaan tunnistaa annettu data resurssi on tietyn haun palauttamat on helpompaa.

> [AZURE.NOTE] Käyttäjien ottaa osumien korostamisen käytöstä halutessasi ohjatulla "Korosta" **Azure tietoluettelon** -portaalissa.

Kun tarkastelet hakutuloksia, se ei aina ehkä ilmeisimmät miksi tietojen resurssi on sisältää, vaikka, jossa on Osumien korostaminen on käytössä. Koska kaikki ominaisuudet etsitään oletusarvoisesti, tietojen resurssi voi palauttaa vuoksi vastinetta sarakkeen tason ominaisuudesta. Ja usea käyttäjä voi lisätä huomautuksia rekisteröidyt tiedot resurssien omien tunnisteiden ja kuvaukset, koska kaikki metatietojen voi näyttää hakutulokset-luettelossa.

Oletusarvo-ruutu Näytä, kunkin ruudun hakutulokset näkyvät sisällytetään "Näytä hakusana vastaa"-kuvaketta, joka sallii käyttäjän voit nopeasti tarkastella hakutuloksia ja niiden sijainnin ja voit siirtyä niitä tarvittaessa.

 ![Osumien korostaminen ja Etsi vastaa Azure tietoluettelon-portaalissa](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Yhteenveto
Kyseisen tietolähteen tietolähteen rekisteröimistä **Azure tietoluettelon** on helpompi löytää ja ymmärtää kopioimalla rakenteellisia ja kuvaava metatietojen tietolähteen luettelo-palvelun. Tietolähde on rekisteröity, kun käyttäjät löytävät sen suodattaminen ja haun **Azure tietoluettelon** portaalissa.

## <a name="see-also"></a>Katso myös
- [Azure tietoluettelon käytön aloittaminen](data-catalog-get-started.md) -opetusohjelma vaiheittaiset Lisätietoja löydät tietolähteitä.
