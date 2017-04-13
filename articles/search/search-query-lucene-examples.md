<properties
    pageTitle="Azure-haun Lucene kyselyn esimerkkejä | Microsoft Azure-haku"
    description="Lucene kyselysyntaksia epäselvältä haun, lähialue haun, termin kehittämällä, säännöllisen lausekkeen haun ja yleismerkkien haku."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="Lucene query analyzer syntax"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="liamca"
/>

# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Lucene kyselyn syntaksiesimerkkejä Azure hakutoiminnossa kyselyjen luomisesta

Luodessaan kyselyt haussa Azure voit käyttää oletusarvon [yksinkertaisen kyselyn syntaksi](https://msdn.microsoft.com/library/azure/dn798920.aspx) tai vaihtoehtoinen [Lucene kyselyn jäsentimen Azure hakutoiminnolla](https://msdn.microsoft.com/library/azure/mt589323.aspx). Lucene kyselyn jäsennin tukee monimutkaisia kysely-rakenteita, kuten kentän Kohdistettu kyselyt, epäselvältä haun, lähialue haun, termin kehittämällä ja reqular lausekkeen haku.

Tässä artikkelissa vaihe – esimerkkejä, jotka näyttävät Lucene kyselysyntaksia ja tulokset vierekkäin. Esimerkkejä suorittamalla valmiiksi ladattua hakuindeksin [JSFiddle](https://jsfiddle.net/), online-koodin editoria komentosarja ja HTML. 

Napsauta hiiren kakkospainikkeella kyselyn Esimerkki URL-osoitteet JSFiddle avaaminen erillisessä selainikkunassa.

> [AZURE.NOTE] Seuraavissa esimerkeissä hyödyntää hakuindeksin, koostuva työt käytettävissä [New York OpenData Kaupunki](https://nycopendata.socrata.com/) aloite myöntämä tietojoukko perusteella. Näitä tietoja ei tule pitää nykyisen tai valmis. Indeksi on Microsoftin eristetyn-palvelusta. Sinun ei tarvitse Azure-tilauksen tai kokeile näitä kyselyjen haku Azure.

## <a name="viewing-the-examples-in-this-article"></a>Tämän artikkelin esimerkeissä tarkasteleminen

Kaikki tämän artikkelin esimerkkejä Määritä**queryType** haku-parametrin Lucene kysely-jäsennin. Kun käytät koodista Lucene kysely-jäsentimen, **queryType** määrittää jokaisen pyynnön.  Kelvolliset arvot ovat **yksinkertaiset**|**koko**, jonka **Yksinkertainen** Oletuslomake ja **koko** Lucene kyselyn jäsentimen. [Etsi asiakirjoista (Azure haun palvelun REST API)](https://msdn.microsoft.com/library/azure/dn798927.aspx) saat tietoja määrittämällä kyselyparametrit.

**Esimerkki 1** – seuraava kysely koodikatkelman Avaa selaimessa uudelle sivulle, lataa JSFiddle ja suorittaa kyselyä hiiren kakkospainikkeella:
- [& queryType = täysi ja Etsi = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Tämä kysely palauttaa tiedostoja työt indeksiin (ladattuja eristetyn palvelu)

Uudessa selainikkunassa näet JavaScript-lähde- ja HTML-tulostuksen rinnakkain. Komentosarja viittaa kysely, joka tarjoaa tämän artikkelin Esimerkki URL-osoitteet. Esimerkiksi **esimerkissä 1**perustana olevan kyselyn on seuraavanlainen:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Ilmoitus kyselyn käyttää ennalta määritettyjä Azure haku-indeksiä kutsutaan nycjobs. **SearchFields** parametri rajoittaa haun vain liiketoiminnan otsikko-kenttään. **QueryType** on määritetty **koko**, joka ohjaa Azure haun Lucene kyselyn jäsentimen käytettävä kysely.

### <a name="fielded-query-operation"></a>Fielded hakutoiminto

Voit muokata tämän artikkelin esimerkkejä **fieldname:searchterm** rakennetta, voit määrittää fielded kyselytoiminto, jossa kenttä on yksittäinen sana ja hakusanoja on myös yksittäisen sanan tai lauseen, voit myös Totuusarvo-operaattorit ja määrittämällä. Esimerkkejä seuraavasti:

- business_title:(senior NOT junior)
- tila: ("Helsinki" ja "Uusi Jersey")

Muista valitseminen useita lainausmerkeissä, jos haluat, että molemmat merkkijonojen voidaan laskea yhden kohteen, kuten tässä esimerkissä etsitään kaksi eri kaupunkien sijainti-kenttään. Varmista myös, operaattori on iso kirjain, kun näet ei ja AND- operaattorilla

**Fieldname:searchterm** määritetyn kentän on oltava etsittävän kenttä. Katso lisätietoja indeksimääritteet käyttämisestä kenttien määritykset [Create Index (Azure haun palvelun REST API)](https://msdn.microsoft.com/library/azure/dn798941.aspx) .

## <a name="fuzzy-search"></a>Epäselvältä haku

Epäselvältä Etsi löytää ehdot, joissa on samanlainen rakenne. Kohti [Lucene dokumentaatio](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)epäselvältä haut perustuvat [Damerau Levenshtein etäisyys](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Epäselvältä etsintä-Käytä tilde "~" symbol valinnaisten parametrien, arvo 0 – 2, joka määrittää Muokkaa etäisyys yksittäisen sanan lopussa. Esimerkiksi "sininen ~" tai "sininen noin 1" palauttavat sininen ja liimaa asetusta.

**Esimerkki 2** – napsauttamalla hiiren kakkospainiketta seuraavassa kyselyn koodikatkelman Kokeile. Tämä kysely etsii ne, mutta ei sisällä termin-vanhempi otsikoita business:

- [& queryType = täysi ja Etsi = business_title:senior ei sisällä](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="proximity-search"></a>Lähialue haku

Lähialue haut käytetään etsimiseen ehdot, jotka ovat lähellä toisiaan asiakirjassa. Lisää tilde "~" symboli lauseen lopussa perään sanamäärä, jotka luovat lähialue reunaa. Esimerkiksi hotellista airport"" noin 5 etsii asiakirjan ehdot hotellista ja lentokenttien enintään 5 sanan päässä toisistaan.

**Esimerkki 3** – kakkospainiketta seuraavassa kyselyn koodikatkelman. Tämä kysely etsii töiden (jos se on kirjoitettu väärin) termin Liitä:

- [& queryType = täysi ja Etsi = business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

**Esimerkki 4** – hiiren kakkospainikkeella kyselyä. Etsiä töitä termillä "vanhempi henkilön", jossa se on erotettu useamman kuin yhden sanan verran:

- [& queryType = täysi ja Etsi = business_title: "vanhempi henkilön" noin 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Esimerkki 5** – Kokeile poistaminen uudelleen sanat termi "vanhempi henkilön" välillä.

- [& queryType = täysi ja Etsi = business_title: "vanhempi henkilön" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting"></a>Kausi lisäyksen

Termin kehittämällä viittaa uudempi versio, jos se sisältää boosted termi, tiedostot, jotka eivät sisällä termi suhteessa tiedoston luokitus. Tämä eroaa näkyvissä profiilit pistemäärä siten, että tulosmalli profiilit edistää tiettyjen kenttien sijaan tietyin ehdoin. Seuraavassa esimerkissä auttaa kuvaavat erot.

Harkitse tulosmalli profiilia, joka on osa vastaa tiettyjä kenttää, kuten **Tyylilaji** musicstoreindex esimerkissä. Termin kehittämällä voidaan edelleen edistää tiettyjä haun ehdot suurempi kuin muiden. Esimerkiksi "rock ^ 2 sähköisen" parantaa asiakirjoja, jotka sisältävät hakuehdot suurempi kuin muiden hakukenttiä indeksissä **Tyylilaji** -kentässä. Tiedostot, jotka sisältävät hakusanoja "rock" lisäksi luokitellut suurempi kuin muita hakusanoja, "sähköisen" tuloksena termin tehon lisäys arvo (2).

Termin edistää, käytä sirkumfleksi, "^"-kuva, jossa etsit termi lopussa tehon lisäys-kerroin (numero). Mitä suurempi tehon lisäys kerroin, Lisää haluamasi termi on suhteessa muihin hakusanojen. Oletusarvon mukaan tehon lisäys kerroin on 1. Vaikka tehon lisäys kerroin on positiivinen, se voi olla pienempi kuin 1, (esimerkiksi 0,2).

**Esimerkki 6** – hiiren kakkospainikkeella kyselyä. Etsi työt termillä "tietokoneen henkilön", jossa näemme ei ole tuloksia sanat tietokoneen ja henkilön kanssa, mutta henkilön työt ovat tulokset yläreunassa.

- [& queryType = täysi ja Etsi = business_title:computer henkilön](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Esimerkki 7** – Kokeile uudelleen, aika-kehittämällä tulokset termin tietokoneen päälle termin määrityksen Jos molemmat sanoja ei ole.

- [& queryType = täysi ja Etsi = business_title:computer ^ 2 henkilön](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression"></a>Säännöllinen lauseke

Sisällön välillä vinoviiva "/", kuin kuvata [RegExp luokan](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html)perusteella vastine löytyy säännöllisen lausekkeen haun.

**Esimerkki 8** – hiiren kakkospainikkeella kyselyä. Etsi työt joko vanhempi tai varten Yläkoulu tai termi.

- `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Tässä esimerkissä URL-osoite, eivät toimi oikein sivulle. Voit kiertää tämän ongelman kopioi alla URL-osoite ja liitä se selaimen URL-osoite:    `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`


## <a name="wildcard-search"></a>Yleismerkkien haku

Voit käyttää useita yleisesti syntaksissa (\*) tai yhden merkin yleismerkkihakujen (?). Huomautus Lucene kyselyn jäsennin tukee seuraavia merkkejä yksittäisen termin ja ei lausetta.

**Esimerkki 9** – hiiren kakkospainikkeella kyselyä. Etsi työt, jotka sisältävät etuliite "ohjelman', jossa näkyy tällöin business otsikot ohjelmointi ehtoja ja ohjelmointi ei.

- [& queryType = & täydet $select = business_title ja Etsi = business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:prog*)

Et voi käyttää * tai? haun ensimmäisenä merkkinä merkki.


## <a name="next-steps"></a>Seuraavat vaiheet

Yritä määrittää Lucene kyselyn jäsennin koodisi. Seuraavissa linkeissä kerrotaan, kuinka hakukyselyt määritetään .NET ja REST-Ohjelmointirajapinnalla. Linkit käyttää oletusarvon yksinkertainen syntaksi, jolloin sinun on käytettävä tästä artikkelista, voit määrittää **queryType**asiat.

- [Käyttämällä .NET SDK oman Azure-hakuindeksin kyselyn](search-query-dotnet.md)
- [Kyselyn lisääminen Azure hakuindeksin REST-Ohjelmointirajapinnan käyttäminen](search-query-rest-api.md)


 