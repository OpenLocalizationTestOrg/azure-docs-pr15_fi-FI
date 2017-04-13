<properties
    pageTitle="Kirjaudu Analytics näkymän suunnittelu | Microsoft Azure"
    description="Näkymän suunnittelu Log Analytics avulla voit luoda mukautettuja näkymiä, jotka sisältävät erilaisia visualisointeja tietojen OMS säilössä OMS konsolissa. Tässä artikkelissa on viittauksen asetukset ovat käytettävissä mukautettuja näkymiä visualisoinnin osat."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-visualization-part-reference"></a>Kirjaudu Analytics näkymän suunnittelu visualisoinnin osan viittaus
Näkymän suunnittelu Log Analytics avulla voit luoda mukautettuja näkymiä, jotka sisältävät erilaisia visualisointeja tietojen OMS säilöstä OMS konsolissa. Tässä artikkelissa on viittauksen asetukset ovat käytettävissä mukautettuja näkymiä visualisoinnin osat.

Muita artikkeleita näkymän suunnittelu käytettävissä ovat seuraavat:

- [Näkymän suunnittelu](log-analytics-view-designer.md) - näkymän suunnittelu ja luonnin ja muokata mukautettuja näkymiä yleiskatsaus.
- [Ruutu-viittaus](log-analytics-view-designer-tiles.md) - asetukset ovat käytettävissä mukautettuja näkymiä ruutujen viittaus. 

Seuraavassa taulukossa kuvataan erityyppiset ruudut, jotka ovat käytettävissä näkymän suunnittelu.  Seuraavissa osissa kuvataan kunkin ruututyyppi tiedot ja niiden ominaisuudet.

| Näkymälaji | Kuvaus |
|:--|:--|
| [Kyselyjen luetteloa](#list-of-queries-part) | Näyttää luettelon log hakukyselyt.  Käyttäjä voi napsauttaa kussakin kyselyssä sen tulosten esittämistä varten.  |
| [Numero & luettelo](#number-amp-list-part) | Ylätunniste on lokin hakukyselyn tietueiden yhden numeron näyttäminen laskeminen.  Luettelossa näkyy kyselyn kymmenen ylimmän tuloksia kaavion, joka ilmaisee suhteellinen arvo numerosarake tai sen muuttaminen ajan kuluessa. |
| [Kahden numerot & luettelo](#two-numbers-amp-list-part) | Ylätunniste on kaksi lukua, jossa näkyy erillisessä log hakukyselyt tietueista.  Luettelossa näkyy kyselyn kymmenen ylimmän tuloksia kaavion, joka ilmaisee suhteellinen arvo numerosarake tai sen muuttaminen ajan kuluessa. |
| [Rengas & luettelo](#donut-amp-list-part) | Otsikon Näyttää Yhteenveto loki kyselyn arvo sarakkeesta yksittäisen luvun.  Rengas graafisesti yläreunan kolme tietuetta tulokset. |
| [Kaksi aikajanojen & luettelo](#two-timelines-amp-list-part) | Otsikon näyttää tulosten kaksi log pylväskaaviot aikaan näyttäminen yhteenveto loki kyselyn arvo sarakkeesta yksittäisen luvun selite.  Luettelossa näkyy kyselyn kymmenen ylimmän tuloksia kaavion, joka ilmaisee suhteellinen arvo numerosarake tai sen muuttaminen ajan kuluessa. |   
| [Tietoja](#information-part) | Otsikko näkyy staattiseksi tekstiksi ja valinnainen linkki.  Luettelo sisältää vähintään yhden kohteen staattiseksi tekstiksi ja otsikko. |
| [Viivakaavio ja kuvatekstin sekä luettelo](#line-chart-callout-amp-list-part) | Otsikon Näyttää ajan ja kuvatekstin tiivistetty arvona viivakaaviota, jossa on useita arvosarjoja log kyselystä.  Luettelossa näkyy kyselyn kymmenen ylimmän tuloksia kaavion, joka ilmaisee suhteellinen arvo numerosarake tai sen muuttaminen ajan kuluessa. |
| [Viivakaavio ja luettelo](#line-chart-amp-list-part) | Otsikko näkyy viivakaaviota, jossa on useita arvosarjoja log kyselystä ajan kuluessa.  Luettelossa näkyy kyselyn kymmenen ylimmän tuloksia kaavion, joka ilmaisee suhteellinen arvo numerosarake tai sen muuttaminen ajan kuluessa. |
| [Pinotut rivin kaaviot-osa](#stack-of-line-charts-part) | Näyttää kolme erillisenä rivinä-kaavioita, joissa on useita arvosarjoja log kyselystä ajan kuluessa. |


## <a name="list-of-queries-part"></a>Kyselyjen osa luettelo

Näyttää luettelon log hakukyselyt.  Käyttäjä voi napsauttaa kussakin kyselyssä sen tulosten esittämistä varten.  Näkymä sisältää yksittäisen kyselyn oletusarvoisesti ja voit napsauttaa **+ kyselyn** Lisää uusia kyselyjä.

![Kyselyjen Näytä luettelo](media/log-analytics-view-designer/view-list-queries.png)

| Asetus | Kuvaus |
|:--|:--|
| **Yleiset** |
| Otsikko | Näkymän yläreunassa näkyvä teksti. |
| Uusi ryhmä | Valitse tämä vaihtoehto, jos haluat luoda uuden ryhmän aloittaen nykyinen näkymä-näkymässä. |
| Valmiiksi valittujen suodattimet | Pilkuilla erotettu luettelo sisältämään vasemman suodatinruudussa, kun käyttäjä valitsee kyselyn ominaisuudet. |
| Muunna tila | Näkymää näkyviin, kun kysely on valittu.  Käyttäjä voi valita kaikki käytettävissä olevat näkymät kyselyn avaamisen jälkeen. |
| **Kyselyt** |
| Hakukyselyn | Jos haluat suorittaa kyselyn. |
| Kutsumanimi | Kyselyn, joka näyttää käyttäjälle kuvaava nimi. |


## <a name="number--list-part"></a>Numero ja luettelo-osa

Ylätunniste on lokin hakukyselyn tietueiden yhden numeron näyttäminen laskeminen.  Luettelossa näkyy kyselyn kymmenen ylimmän tuloksia kaavion, joka ilmaisee suhteellinen arvo numerosarake tai sen muuttaminen ajan kuluessa.


![Kyselyjen Näytä luettelo](media/log-analytics-view-designer/view-number-list.png)

| Asetus | Kuvaus |
|:--|:--|
| **Yleiset** |
| Ryhmäotsikko | Näkymän yläreunassa näkyvä teksti. |
| Uusi ryhmä | Valitse tämä vaihtoehto, jos haluat luoda uuden ryhmän aloittaen nykyinen näkymä-näkymässä. |
| Kuvake | Kuvatiedoston näyttämään tulos otsikon vieressä.
| Kuvake | Valitse kuvake näkyy. |
| **Otsikko** |
| Selite | Otsikon yläreunassa näkyvä teksti. |
| Kyselyn | Kysely suoritetaan otsikkona.  Kysely palauttaa tietueiden määrän laskeminen näkyvät. |
| **Luettelo** |
| Kyselyn | Kysely suoritetaan luettelon.  Kaksi ensimmäistä ominaisuudet ensin kymmenen tietueiden tulokset näkyvät.  Ensimmäisen ominaisuuden pitäisi olla tekstiarvon ja toinen ominaisuuden numeerisen arvon.  Palkit luodaan automaattisesti numeeristen sarakkeiden suhteellinen arvon perusteella.<br><br>Kyselyn Lajittele-komennon avulla voit lajitella luettelon tietueet.  Käyttäjä voi valitsemalla Näytä kaikki ja suorita kysely palauttaa kaikki tietueet. |
| Piilota graph | Valitse oikealla puolella numerosarake graph käytöstä. |
| Ota käyttöön sparkline-kaaviot | Valitse Näytä sparkline-kaavion vaakapalkkia sijaan.  Katso lisätietoja [Yleiset asetukset](#sparklines) . |
| Väri | Palkkien tai sparkline-kaavion väri. |
| Nimen ja arvon erotin | Yksittäinen merkki erotin, jos haluat jäsentää useita arvoja text-ominaisuuteen.  Katso lisätietoja [Yleiset asetukset](#name-value-separator) . |
| Siirtyminen kyselyn | Kysely, joka suoritetaan, kun käyttäjä valitsee kohteen luettelosta.  Katso lisätietoja [Yleiset asetukset](#navigation-query) . |
| **Luettelo** | **> Sarakeotsikot** |
| Nimi | Luettelon ensimmäisen sarakkeen yläreunassa näkyvä teksti. |
| Arvo | Toisen sarakkeen luettelon yläreunassa näkyvä teksti. |
| **Luettelo** | **> Raja-arvot** |
| Ota käyttöön raja-arvot | Valitse tämä vaihtoehto, jos haluat ottaa käyttöön raja-arvot.  Katso lisätietoja [Yleiset asetukset](#thresholds) . |


## <a name="two-numbers--list-part"></a>Kahden luvun & luettelon osa

Ylätunniste on kaksi lukua, jossa näkyy erillisessä log hakukyselyt tietueista.  Luettelossa näkyy kyselyn kymmenen ylimmän tuloksia kaavion, joka ilmaisee suhteellinen arvo numerosarake tai sen muuttaminen ajan kuluessa.

![Kahden luvun ja Luettelonäkymä](media/log-analytics-view-designer/view-two-numbers-list.png)

| Asetus | Kuvaus |
|:--|:--|
| **Yleiset** |
| Ryhmäotsikko | Näkymän yläreunassa näkyvä teksti. |
| Uusi ryhmä | Valitse tämä vaihtoehto, jos haluat luoda uuden ryhmän aloittaen nykyinen näkymä-näkymässä. |
| Kuvake | Kuvatiedoston näyttämään tulos otsikon vieressä.
| Kuvake | Valitse kuvake näkyy. |
| **Otsikko** |
| Selite | Otsikon yläreunassa näkyvä teksti. |
| Kyselyn | Kysely suoritetaan otsikkona.  Kysely palauttaa tietueiden määrän laskeminen näkyvät. |
| **Luettelo** |
| Kyselyn | Kysely suoritetaan luettelon.  Kaksi ensimmäistä ominaisuudet ensin kymmenen tietueiden tulokset näkyvät.  Ensimmäisen ominaisuuden pitäisi olla tekstiarvon ja toinen ominaisuuden numeerisen arvon.  Palkit luodaan automaattisesti numeeristen sarakkeiden suhteellinen arvon perusteella.<br><br>Kyselyn Lajittele-komennon avulla voit lajitella luettelon tietueet.  Käyttäjä voi valitsemalla Näytä kaikki ja suorita kysely palauttaa kaikki tietueet. |
| Piilota graph | Valitse oikealla puolella numerosarake graph käytöstä. |
| Ota käyttöön sparkline-kaaviot | Valitse Näytä sparkline-kaavion vaakapalkkia sijaan.  Katso lisätietoja [Yleiset asetukset](#sparklines) . |
| Väri | Palkkien tai sparkline-kaavion väri. |
| Toiminto | Voit suorittaa sparkline toiminto.  Katso lisätietoja [Yleiset asetukset](#sparklines) . |
| Nimen ja arvon erotin | Yksittäinen merkki erotin, jos haluat jäsentää useita arvoja text-ominaisuuteen.  Katso lisätietoja [Yleiset asetukset](#name-value-separator) . |
| Siirtyminen kyselyn | Kysely, joka suoritetaan, kun käyttäjä valitsee kohteen luettelosta.  Katso lisätietoja [Yleiset asetukset](#navigation-query) . |
| **Luettelo** | **> Sarakeotsikot** |
| Nimi | Luettelon ensimmäisen sarakkeen yläreunassa näkyvä teksti. |
| Arvo | Luettelon toisen sarakkeen yläreunassa näkyvä teksti. |
| **Luettelo** | **> Raja-arvot** |
| Ota käyttöön raja-arvot | Valitse tämä vaihtoehto, jos haluat ottaa käyttöön raja-arvot.  Katso lisätietoja [Yleiset asetukset](#thresholds) . |

## <a name="donut--list-part"></a>Rengas- ja luettelo-osa

Otsikon Näyttää Yhteenveto loki kyselyn arvo sarakkeesta yksittäisen luvun.  Rengas graafisesti yläreunan kolme tietuetta tulokset.

![Rengas- ja luettelo-näkymä](media/log-analytics-view-designer/view-donut-list.png)

| Asetus | Kuvaus |
|:--|:--|
| **Yleiset** |
| Ryhmäotsikko | Ruudun yläreunassa näkyvä teksti. |
| Uusi ryhmä | Valitse tämä vaihtoehto, jos haluat luoda uuden ryhmän aloittaen nykyinen näkymä-näkymässä. |
| Kuvake | Kuvatiedoston näyttämään tulos otsikon vieressä. |
| Kuvake | Valitse kuvake näkyy. |
| **Ylätunniste** |
| Otsikko | Otsikon yläreunassa näkyvä teksti.
| Alaotsikko | Teksti, joka näytetään otsikon yläosassa otsikon alle.
| **Rengas** |
| Kyselyn | Kysely, joka suoritetaan rengas.  Ensimmäisen ominaisuuden pitäisi olla tekstiarvon ja toinen ominaisuuden numeerisen arvon. |
| **Rengas** |  **> Center** |
| Teksti | Valitse arvo rengas sisällä näkyvä teksti. |
| Toiminto | Tehdä yhteenvedon yksittäisen arvon arvo-ominaisuuden suoritettava toiminto.<br><br>-Summa: Lisää kaikki tietueet.<br>-Prosentteina: Prosenttiosuus **center-toimintoa käytetään tulosarvojen** arvoilla yhteensä tietueisiin kyselyssä palautetut tietueet. |
| Keskitä-toimintoa käytetään tulosarvojen | Valitse vähintään yksi arvot plus-merkkiä.  Kyselyn tulosten rajoittuvat tietueet, joissa voit määrittää ominaisuusarvoihin.  Jos mitään arvoja on lisätty, kaikki tietueet sisältyvät kyselyn. |
| **Lisäasetukset** | **> Värit** |
| Väri 1<br>Väri 2<br>Väri 3 | Valitse arvojen näyttötapa-rengas kunkin väri. |
| **Lisäasetukset** | **> Värien lisäominaisuudet yhdistäminen** |
| Kenttäarvo | Kirjoita Näytä eri väriksi, jos se on mukana rengas-kentän nimi. |
| Väri | Valitse väri yksilöllinen kentälle. |
| **Luettelo** |
| Kyselyn | Kysely suoritetaan luettelon.  Kysely palauttaa tietueiden määrän laskeminen näkyvät. |
| Piilota graph | Valitse oikealla puolella numerosarake graph käytöstä. |
| Ota käyttöön sparkline-kaaviot | Valitse Näytä sparkline-kaavion vaakapalkkia sijaan.  Katso lisätietoja [Yleiset asetukset](#sparklines) . |
| Väri | Palkkien tai sparkline-kaavion väri. |
| Toiminto | Voit suorittaa sparkline toiminto.  Katso lisätietoja [Yleiset asetukset](#sparklines) . |
| Nimen ja arvon erotin | Yksittäinen merkki erotin, jos haluat jäsentää useita arvoja text-ominaisuuteen.  Katso lisätietoja [Yleiset asetukset](#name-value-separator) . |
| Siirtyminen kyselyn | Kysely, joka suoritetaan, kun käyttäjä valitsee kohteen luettelosta.  Katso lisätietoja [Yleiset asetukset](#navigation-query) . |
| **Luettelo** | **> Sarakeotsikot** |
| Nimi | Luettelon ensimmäisen sarakkeen yläreunassa näkyvä teksti. |
| Arvo | Toisen sarakkeen luettelon yläreunassa näkyvä teksti. |
| **Luettelo** | **> Raja-arvot** |
| Ota käyttöön raja-arvot | Valitse tämä vaihtoehto, jos haluat ottaa käyttöön raja-arvot.  Katso lisätietoja [Yleiset asetukset](#thresholds) . |

## <a name="two-timelines--list-part"></a>Kaksi aikajanojen ja luettelo-osa

Otsikon näyttää tulosten kaksi log pylväskaaviot aikaan näyttäminen yhteenveto loki kyselyn arvo sarakkeesta yksittäisen luvun selite.  Luettelossa näkyy kyselyn kymmenen ylimmän tuloksia kaavion, joka ilmaisee suhteellinen arvo numerosarake tai sen muuttaminen ajan kuluessa.

![Kaksi aikajanojen ja luettelon tarkasteleminen](media/log-analytics-view-designer/view-two-timelines-list.png)

| Asetus | Kuvaus |
|:--|:--|
| **Yleiset** |
| Ryhmäotsikko | Ruudun yläreunassa näkyvä teksti. |
| Uusi ryhmä | Valitse tämä vaihtoehto, jos haluat luoda uuden ryhmän aloittaen nykyinen näkymä-näkymässä. |
| Kuvake | Kuvatiedoston näyttämään tulos otsikon vieressä. |
| Kuvake | Valitse kuvake näkyy. |
| **Kaavion ensin<br>toisen kaavion** |
| Selite | Valitse ensimmäisen sarjan kuvaselitteessä näytettävä teksti. |
| Väri | Väri, jota käytetään sarakkeiden sarjan. |
| Kyselyn | Kyselyn suorittaminen ensimmäisen sarjan.  Kullakin aikavälillä päälle tietueiden määrän laskeminen edustaa kaavion sarakkeet. |
| Toiminto | Tehdä yhteenvedon kuvatekstin yksittäisen arvon arvo-ominaisuuden suoritettava toiminto.<br><br>-Summa: Summa kaikki tietueet olevaa arvoa.<br>-Keskiarvo: Kaikki tietueet arvot tuottojen keskiarvo.<br>-Viimeisen malli: Kaavion viimeinen aikavälin arvo.<br>-Ensimmäisen malli: Kaavion aikavälin ensimmäinen arvo.<br>-Määrä: Kysely palauttaa kaikki tietueet määrä.|
| **Luettelo** |
| Kyselyn | Kysely suoritetaan luettelon.  Kysely palauttaa tietueiden määrän laskeminen näkyvät. |
| Piilota graph | Valitse oikealla puolella numerosarake graph käytöstä. |
| Ota käyttöön sparkline-kaaviot | Valitse Näytä sparkline-kaavion vaakapalkkia sijaan.  Katso lisätietoja [Yleiset asetukset](#sparklines) . |
| Väri | Palkkien tai sparkline-kaavion väri. |
| Toiminto | Voit suorittaa sparkline toiminto.  Katso lisätietoja [Yleiset asetukset](#sparklines) . |
| Siirtyminen kyselyn | Kysely, joka suoritetaan, kun käyttäjä valitsee kohteen luettelosta.  Katso lisätietoja [Yleiset asetukset](#navigation-query) . |
| **Luettelo** | **> Sarakeotsikot** |
| Nimi | Luettelon ensimmäisen sarakkeen yläreunassa näkyvä teksti. |
| Arvo | Toisen sarakkeen luettelon yläreunassa näkyvä teksti. |
| **Luettelo** | **> Raja-arvot** |
| Ota käyttöön raja-arvot | Valitse tämä vaihtoehto, jos haluat ottaa käyttöön raja-arvot.  Katso lisätietoja [Yleiset asetukset](#thresholds) . |

## <a name="information-part"></a>Tieto-osan

Otsikko näkyy staattiseksi tekstiksi ja valinnainen linkki.  Luettelo sisältää vähintään yhden kohteen staattiseksi tekstiksi ja otsikko.

![Tietonäkymä](media/log-analytics-view-designer/view-information.png)

| Asetus | Kuvaus |
|:--|:--|
| **Yleiset** |
| Ryhmäotsikko | Ruudun yläreunassa näkyvä teksti. |
| Uusi ryhmä | Valitse tämä vaihtoehto, jos haluat luoda uuden ryhmän aloittaen nykyinen näkymä-näkymässä. |
| Väri | Otsikon taustaväri. |
| **Ylätunniste** |
| Kuva | Kuvatiedoston näyttämään otsikko. |
| Otsikko | Valitse ylätunnisteessa näytettävä teksti. |
| **Ylätunniste** | **> Linkki** |
| Otsikko | Linkkiteksti. |
| URL-osoite | Linkin URL-osoite. |
| **Tieto-osista** |
| Otsikko | Kunkin kohteen otsikkona näytettävä teksti. |
| Sisältö | Kunkin kohteen näytettävä teksti. |


## <a name="line-chart-callout--list-part"></a>Viivakaavio, kuvatekstin & luettelon osa

Otsikon näyttää viivakaaviota, jossa on useita arvosarjoja log kyselystä ajan ja kuvatekstin tiivistetty arvolla.  Luettelossa näkyy kyselyn kymmenen ylimmän tuloksia kaavion, joka ilmaisee suhteellinen arvo numerosarake tai sen muuttaminen ajan kuluessa.

![Viivakaavio, kuvatekstin ja Luettelonäkymä](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Asetus | Kuvaus |
|:--|:--|
| **Yleiset** |
| Ryhmäotsikko | Ruudun yläreunassa näkyvä teksti. |
| Uusi ryhmä | Valitse tämä vaihtoehto, jos haluat luoda uuden ryhmän aloittaen nykyinen näkymä-näkymässä. |
| Kuvake | Kuvatiedoston näyttämään tulos otsikon vieressä. |
| Kuvake | Valitse kuvake näkyy. |
| **Ylätunniste** |
| Otsikko | Otsikon yläreunassa näkyvä teksti. |
| Alaotsikko | Teksti, joka näytetään otsikon yläosassa otsikon alle. |
| **Viivakaavio** |
| Kyselyn | Kyselyn suorittaminen viivakaavio.  Ensimmäisen ominaisuuden pitäisi olla tekstiarvon ja toinen ominaisuuden numeerisen arvon.  Tämä on yleensä kysely, joka käyttää **Mitta** -avainsanan yhteenveto tulokset.  Jos kysely käyttää **väli** -avainsanan kaavion akselin käytetään tähän aikaväli.  Jos kyselyä ei ole **väli** -avainsanan tunnin välein käytetään x-akselin. |
| **Viivakaavio** | **> Kuvaselite** |
| Kuvatekstin otsikko | Kuvateksti-arvon yläpuolella näkyvä teksti. |
| Sarjan nimi | Ominaisuuden arvo sarjojen käytettävä kuvateksti-arvo.  Jos ei sarjoja ei anneta, kaikki tietueet kyselystä käytetään. |
| Toiminto | Tehdä yhteenvedon kuvatekstin yksittäisen arvon arvo-ominaisuuden suoritettava toiminto.<br><br>-Keskiarvo: Kaikki tietueet arvot tuottojen keskiarvo.<br>-Laske kysely palauttaa kaikki tietueet määrä.<br>-Viimeisen malli: Kaavion viimeinen aikavälin arvo.<br>-Maks: Kaavion sisältämää välit suurin sallittu arvo.<br>-Min: Kaavion sisältämää välit pienin arvo.<br>-Summa: Summa kaikki tietueet olevaa arvoa. |
| **Viivakaavio** | **> Y-akseli** |
| Käytä Logaritminen asteikko | Valitse tämä vaihtoehto, jos haluat käyttää y-akselin asteikon logaritmiseksi. |
| Yksiköt | Määritä kysely palauttaa arvot yksiköt.  Näytä otsikot kaavion, joka ilmaisee arvon tyypeistä ja halutessasi muuntamisen arvot käyttävät näitä tietoja.  Yksikkö määrittää luokan yksikön ja määrittää nykyisen tyyppi-arvot, jotka ovat käytettävissä.  Jos valitset arvon muuntaminen sitten numeeriset arvot muunnetaan Muunna Kirjoita nykyisen yksikön tyyppi. |
| Mukautetun tarran | Y-akselin otsikko yksikkötyyppi-kohdan vieressä näkyvä teksti.  Jos ei nimeä ei määritetä, näkyy vain yksikkötyyppi. |
| **Luettelo** |
| Kyselyn | Kysely suoritetaan luettelon.  Kysely palauttaa tietueiden määrän laskeminen näkyvät. |
| Piilota graph | Valitse oikealla puolella numerosarake graph käytöstä. |
| Ota käyttöön sparkline-kaaviot | Valitse Näytä sparkline-kaavion vaakapalkkia sijaan.  Katso lisätietoja [Yleiset asetukset](#sparklines) . |
| Väri | Palkkien tai sparkline-kaavion väri. |
| Toiminto | Voit suorittaa sparkline toiminto.  Katso lisätietoja [Yleiset asetukset](#sparklines) . |
| Nimen ja arvon erotin | Yksittäinen merkki erotin, jos haluat jäsentää useita arvoja text-ominaisuuteen.  Katso lisätietoja [Yleiset asetukset](#name-value-separator) . |
| Siirtyminen kyselyn | Kysely, joka suoritetaan, kun käyttäjä valitsee kohteen luettelosta.  Katso lisätietoja [Yleiset asetukset](#navigation-query) . |
| **Luettelo** | **> Sarakeotsikot** |
| Nimi | Luettelon ensimmäisen sarakkeen yläreunassa näkyvä teksti. |
| Arvo | Toisen sarakkeen luettelon yläreunassa näkyvä teksti. |
| **Luettelo** | **> Raja-arvot** |
| Ota käyttöön raja-arvot | Valitse tämä vaihtoehto, jos haluat ottaa käyttöön raja-arvot.  Katso lisätietoja [Yleiset asetukset](#thresholds) . |

## <a name="line-chart--list-part"></a>Rivin luettelon & kaavion osa

Otsikko näkyy viivakaaviota, jossa on useita arvosarjoja log kyselystä ajan kuluessa.  Luettelossa näkyy kyselyn kymmenen ylimmän tuloksia kaavion, joka ilmaisee suhteellinen arvo numerosarake tai sen muuttaminen ajan kuluessa.

![Kaavion & luettelon näkymä](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Asetus | Kuvaus |
|:--|:--|
| **Yleiset** |
| Ryhmäotsikko | Ruudun yläreunassa näkyvä teksti. |
| Uusi ryhmä | Valitse tämä vaihtoehto, jos haluat luoda uuden ryhmän aloittaen nykyinen näkymä-näkymässä. |
| Kuvake | Kuvatiedoston näyttämään tulos otsikon vieressä. |
| Kuvake | Valitse kuvake näkyy. |
| **Ylätunniste** |
| Otsikko | Otsikon yläreunassa näkyvä teksti. |
| Alaotsikko | Teksti, joka näytetään otsikon yläosassa otsikon alle. |
| **Viivakaavio** |
| Kyselyn | Kyselyn suorittaminen viivakaavio.  Ensimmäisen ominaisuuden pitäisi olla tekstiarvon ja toinen ominaisuuden numeerisen arvon.  Tämä on yleensä kysely, joka käyttää **Mitta** -avainsanan yhteenveto tulokset.  Jos kysely käyttää **väli** -avainsanan kaavion akselin käytetään tähän aikaväli.  Jos kyselyä ei ole **väli** -avainsanan tunnin välein käytetään x-akselin. |
| **Viivakaavio** | **> Y-akseli** |
| Käytä Logaritminen asteikko | Valitse tämä vaihtoehto, jos haluat käyttää y-akselin asteikon logaritmiseksi. |
| Yksiköt | Määritä kysely palauttaa arvot yksiköt.  Näytä otsikot kaavion, joka ilmaisee arvon tyypeistä ja halutessasi muuntamisen arvot käyttävät näitä tietoja.  Yksikkö määrittää luokan yksikön ja määrittää nykyisen tyyppi-arvot, jotka ovat käytettävissä.  Jos valitset arvon muuntaminen sitten numeeriset arvot muunnetaan Muunna Kirjoita nykyisen yksikön tyyppi. |
| Mukautetun tarran | Y-akselin otsikko yksikkötyyppi-kohdan vieressä näkyvä teksti.  Jos ei nimeä ei määritetä, näkyy vain yksikkötyyppi. |
| **Luettelo** |
| Kyselyn | Kysely suoritetaan luettelon.  Kysely palauttaa tietueiden määrän laskeminen näkyvät. |
| Piilota graph | Valitse oikealla puolella numerosarake graph käytöstä. |
| Ota käyttöön sparkline-kaaviot | Valitse Näytä sparkline-kaavion vaakapalkkia sijaan.  Katso lisätietoja [Yleiset asetukset](#sparklines) . |
| Väri | Palkkien tai sparkline-kaavion väri. |
| Toiminto | Voit suorittaa sparkline toiminto.  Katso lisätietoja [Yleiset asetukset](#sparklines) . |
| Nimen ja arvon erotin | Yksittäinen merkki erotin, jos haluat jäsentää useita arvoja text-ominaisuuteen.  Katso lisätietoja [Yleiset asetukset](#name-value-separator) . |
| Siirtyminen kyselyn | Kysely, joka suoritetaan, kun käyttäjä valitsee kohteen luettelosta.  Katso lisätietoja [Yleiset asetukset](#navigation-query) . |
| **Luettelo** | **> Sarakeotsikot** |
| Nimi | Luettelon ensimmäisen sarakkeen yläreunassa näkyvä teksti. |
| Arvo | Toisen sarakkeen luettelon yläreunassa näkyvä teksti. |
| **Luettelo** | **> Raja-arvot** |
| Ota käyttöön raja-arvot | Valitse tämä vaihtoehto, jos haluat ottaa käyttöön raja-arvot.  Katso lisätietoja [Yleiset asetukset](#thresholds) . |

## <a name="stack-of-line-charts-part"></a>Pinotut rivin kaaviot-osa

Näyttää kolme erillisenä rivinä-kaavioita, joissa on useita arvosarjoja log kyselystä ajan kuluessa.

![Pinotut viivakaaviot.](media/log-analytics-view-designer/view-stack-line-charts.png)

| Asetus | Kuvaus |
|:--|:--|
| **Yleiset** |
| Ryhmäotsikko | Ruudun yläreunassa näkyvä teksti. |
| Uusi ryhmä | Valitse tämä vaihtoehto, jos haluat luoda uuden ryhmän aloittaen nykyinen näkymä-näkymässä. |
| Kuvake | Kuvatiedoston näyttämään tulos otsikon vieressä. |
| **Kaavion 1<br>kaavion 2<br>kaavion 3** | **> Otsikko** |
| Otsikko | Kaavion yläpuolella näkyvä teksti. |
| Alaotsikko | Teksti yläreunaan kaavion otsikon alle. |
| **Kaavion 1<br>kaavion 2<br>kaavion 3** | **Viivakaavio** |
| Kyselyn | Kyselyn suorittaminen viivakaavio.  Ensimmäisen ominaisuuden pitäisi olla tekstiarvon ja toinen ominaisuuden numeerisen arvon.  Tämä on yleensä kysely, joka käyttää **Mitta** -avainsanan yhteenveto tulokset.  Jos kysely käyttää **väli** -avainsanan kaavion akselin käytetään tähän aikaväli.  Jos kyselyä ei ole **väli** -avainsanan tunnin välein käytetään x-akselin. |
| **Kaavio** | **> Y-akseli** |
| Käytä Logaritminen asteikko | Valitse tämä vaihtoehto, jos haluat käyttää y-akselin asteikon logaritmiseksi. |
| Yksiköt | Määritä kysely palauttaa arvot yksiköt.  Näytä otsikot kaavion, joka ilmaisee arvon tyypeistä ja halutessasi muuntamisen arvot käyttävät näitä tietoja.  Yksikkö määrittää luokan yksikön ja määrittää nykyisen tyyppi-arvot, jotka ovat käytettävissä.  Jos valitset arvon muuntaminen sitten numeeriset arvot muunnetaan Muunna Kirjoita nykyisen yksikön tyyppi. |
| Mukautetun tarran | Y-akselin otsikko yksikkötyyppi-kohdan vieressä näkyvä teksti.  Jos ei nimeä ei määritetä, näkyy vain yksikkötyyppi. |

## <a name="common-settings"></a>Yleiset asetukset
Seuraavissa osissa on useita visualisoinnin osia yhteiset asetukset.

### <a name="name-value-separator">Nimen ja arvon erotin</a>
Yksittäinen merkki erotin, jos haluat jäsentää useiden arvojen luettelon kyselystä text-ominaisuuteen.  Jos määrität erottimen, voit kirjoittaa kunkin kentän nimi-ruutuun saman erottimen erotettu nimet.

Esimerkkinä *sijainti* , joka sisällyttää arvot, kuten *Redmond rakennuksen 41* ja *Bellevue Building12*-ominaisuuden.  Voit määrittää – nimen ja arvon erotin ja *Kaupunki rakennuksen* nimi.  Tämä jäsentää kaksi ominaisuutta kutsutaan *Kaupunki* - ja *Rakennuksen*jokaisen arvon. 

### <a name="navigation-query">Siirtyminen kyselyn</a>
Kysely, joka suoritetaan, kun käyttäjä valitsee kohteen luettelosta.  *{Valitun kohteen}* avulla voit liittää syntaksi käyttäjälle valinnut kohteen osalta.

Esimerkiksi Jos kyselyssä on sarake nimeltä *tietokoneen* ja siirtyminen kysely on *{valitun kohteen}*-kyselyn kuten *tietokoneen = "Oma tietokone-* suoritetaan, kun käyttäjä valittuna tietokoneeseen.  Jos siirtyminen kysely on *tyyppi = {valitun kohteen} tapahtuman* sitten kysely *tyyppi = tapahtuman tietokoneen = "Oma tietokone-* suorittaa.

### <a name="sparklines">Sparkline-kaaviot</a>
Sparkline-kaaviossa on pieni viivakaavio, joka havainnollistaa luetteloon-merkinnän arvo ajan kuluessa.  Visualisoinnin luettelon osia voit valita, näytetäänkö vaakasuuntaisen palkki tarkoittaa numerosarake tai sparkline-kaavion, joka ilmaisee arvon ajan kuluessa suhteellinen arvon.

Seuraavassa taulukossa on kuvattu sparkline-kaavion asetukset.

| Asetus | Kuvaus |
|:--|:--|
| Ota käyttöön sparkline-kaaviot | Valitse Näytä sparkline-kaavion vaakapalkkia sijaan. |
| Toiminto | Jos sparkline-kaaviot ovat käytössä, tämä on ominaisuuksista laskea sparkline-luettelosta suoritettava toiminto.<br><br>-Viimeisen malli: Viimeinen arvo sarjojen päälle aikaväli.<br>-Maks: Suurin-arvo sarjojen päälle aikaväli.<br>-Min: Pienin arvo sarjojen päälle aikaväli.<br>-Summa: Sarjan päälle aikaväli arvot summa.<br>-Yhteenveto: Sama mitta-komentoa käyttämällä otsikossa kyselynä. |

### <a name="thresholds">Raja-arvot</a>
Raja-arvot avulla voit näyttää kunkin kohteen vieressä värillinen kuvakkeen luettelona, jossa voit nopeasti visuaalisia ilmaisin kohteista, jotka ovat tietyn arvon suurempi tai pienempi tietyn alueen sisällä.  Voit esimerkiksi näyttää kohteiden kelvollinen arvo, keltainen, jos arvo on alueella, joka ilmaisee varoitus ja punainen vihreä kuvake jos se ylittää virheellistä arvoa.

Kun otat raja-osa, sinun on määritettävä vähintään yksi raja-arvot.  Jos kohteen arvo on suurempi kuin kynnysarvo ja pienempi kuin seuraavan raja-arvo, väriä käytetään.  Jos kohde on suurempi kuin sitten korkein raja-arvo, on määritetty väri.   

Kunkin raja-arvo on yksi raja-arvo on **Oletusarvo**.  Tämä on väri, jos muut arvot on ylitetty.  Voit lisätä tai poistaa raja-arvot valitsemalla **+** tai **x** -painiketta.

Seuraavassa taulukossa on kuvattu omistysyhteyksien asetuksia.

| Asetus | Kuvaus |
|:--|:--|
| Ota käyttöön raja-arvot | Valitse väri-kuvake näyttää kunkin arvon, joka ilmaisee määritetyn raja-arvot suhteessa sen kunto vasemmalle. |
| Nimi | Raja-arvo nimi. |
| Raja-arvo | Raja-arvon.  Jokaisen luettelon kohdan kunto väri määritetään värin suurimman kohteen arvo ylittää raja-arvo.  On yksi, joka on väriä, jos ei ole raja-arvot ovat ylittänyt oletusarvo raja-arvon. |
| Väri | Värin raja-arvo. |


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [lokin haut](log-analytics-log-searches.md) tukemaan kyselyt visualisoinnin osat.