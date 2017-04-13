<properties
    pageTitle="Kirjaudu Analytics näkymän suunnittelutyökalun ruutu viittaus | Microsoft Azure"
    description="Näkymän suunnittelu Log Analytics avulla voit luoda mukautettuja näkymiä, jotka sisältävät erilaisia visualisointeja tietojen OMS säilössä OMS konsolissa. Tässä artikkelissa on viittauksen asetukset ovat käytettävissä mukautettuja näkymiä ruudut."
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
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-tile-reference"></a>Lokitiedoston Analytics näkymän suunnittelutyökalun ruutu viittaus
Näkymän suunnittelu Log Analytics avulla voit luoda mukautettuja näkymiä, jotka sisältävät erilaisia visualisointeja tietojen OMS säilössä OMS konsolissa. Tässä artikkelissa on viittauksen asetukset ovat käytettävissä mukautettuja näkymiä ruudut.

Muita artikkeleita näkymän suunnittelu käytettävissä ovat seuraavat:

- [Näkymän suunnittelu](log-analytics-view-designer.md) - näkymän suunnittelu ja luonnin ja muokata mukautettuja näkymiä yleiskatsaus.
- [Visualisoinnin osan viittaus](log-analytics-view-designer-parts.md) - asetukset ovat käytettävissä mukautettuja näkymiä ruudut viittaus. 


Seuraavassa taulukossa on erityyppisiä ruudut, jotka ovat käytettävissä näkymän suunnittelu.  Seuraavissa osissa kuvataan kunkin ruututyyppi tiedot ja niiden ominaisuudet.

| Ruutu | Kuvaus |
|:--|:--|
| [Numero](#number-tile) | Kyselyn tietueiden määrän, jossa näkyy yksi numero. |
| [Kahden luvun](#two-numbers-tile) | Kahden yksittäisen luvun näkyy kaksi eri kyselyjen tietueiden määrät. |
| [Rengas](#donut-tile) | Rengas kaavio kysely, jossa on yhteenveto keskelle arvon perusteella. |
| [Viivakaavio ja kuvateksti](#line-chart-amp-callout-tile) | Viivakaavio kyselyn ja kuvatekstin yhteenveto arvon perusteella. |
| [Viivakaavio](#line-chart-tile) | Viivakaavio kyselyn perusteella. |
| [Kaksi aikajanat](#two-timelines-tile) | Pylväskaavio, jossa on kaksi erillistä kyselyn perusteella kunkin sarjaa. |



## <a name="number-tile"></a>Numero-ruutu

**Numero** -ruutu näyttää yhden numeron näyttäminen loki kyselyn ja selitteen tietueiden määrän.

![Numero-ruutu](media/log-analytics-view-designer/tile-number.png)

| Asetus | Kuvaus |
|:--|:--|
| Nimi        | Ruudun yläreunassa näkyvä teksti. |
| Kuvaus | Ruutu-apuohjelmassa näytettävä teksti.    |
| **Ruutu** |
| Selite | Valitse arvo näytettävä teksti. |
| Kyselyn | Jos haluat suorittaa kyselyn.  Kysely palauttaa tietueiden määrän laskeminen näkyvät. |
| **Lisäasetukset** |  **> Tietoja tiedonkulun todentaminen** |
| Käytössä | Valitse, jos Tietovuo vahvistus on otettu ruutu.  Tämä on vaihtoehtoinen viestin, jos tiedot eivät ole käytettävissä ruutu.  Tätä käytetään yleensä antamaan viestin tilapäinen aikana, kun näkymä on asennettu ja tiedot tulevat käytettävissä. |
| Kyselyn | Voit tarkistaa, jos tiedot ovat käytettävissä näkymässä suorittaa kyselyn.  Kysely palauttaa tuloksia, jos sanoma näkyy sen sijaan, että tärkeimmät kyselyn olevaa arvoa. |
| Viestin | Viesti, jos Tietovuo vahvistus-kysely palauttaa mitään tietoja.  Jos annat viestiä, *Suorittamiseen arviointi* näytetään. |
| **Aikavälin** |
| Kesto | Käytettävä aikaväli kyselyn kuluvasta päivästä kesto.  Esimerkiksi jos **7 päivää** on määritetty, valitse kysely on rajoitettu tietueiden 7 päivää aiemmin luotu kuluvaan päivämäärään mennessä. |
| Lopeta tietojen poikkeama | Valinnainen siirtymä nykyinen tietojen käytettävät tärkeimmät kyselyn aikaväli.  Esimerkiksi jos käytössä on **-1 päivä** , **Lopeta päivämäärä siirtymää** ja käytettävien **kesto** **7 päivää** , valitse kysely on rajoitettu tietueet, jotka on luotu 8 päivää, eilen. |

## <a name="two-numbers-tile"></a>Kahden luvun-ruutu

**Kaksi numero** -ruutu näyttää kaksi lukua, jossa lokitiedostojen kyselyt ja selitteen tietueiden määrän kunkin.

![Kahden luvun-ruutu](media/log-analytics-view-designer/tile-two-numbers.png)

| Asetus | Kuvaus |
|:--|:--|
| Nimi        | Ruudun yläreunassa näkyvä teksti. |
| Kuvaus | Ruutu-apuohjelmassa näytettävä teksti.    |
| **Ensimmäinen ruutu** |
| Selite | Valitse arvo näytettävä teksti. |
| Kyselyn | Jos haluat suorittaa kyselyn.  Kysely palauttaa tietueiden määrän laskeminen näkyvät. |
| **Toinen ruutu** |
| Selite | Valitse arvo näytettävä teksti. |
| Kyselyn | Jos haluat suorittaa kyselyn.  Kysely palauttaa tietueiden määrän laskeminen näkyvät. |
| **Lisäasetukset** | **> Tietoja tiedonkulun todentaminen** |
| Käytössä | Valitse, jos Tietovuo vahvistus on otettu ruutu.  Tämä on vaihtoehtoinen viestin, jos tiedot eivät ole käytettävissä ruutu.  Tätä käytetään yleensä antamaan viestin tilapäinen aikana, kun näkymä on asennettu ja tiedot tulevat käytettävissä. |
| Kyselyn | Voit tarkistaa, jos tiedot ovat käytettävissä näkymässä suorittaa kyselyn.  Kysely palauttaa tuloksia, jos sanoma näkyy sen sijaan, että tärkeimmät kyselyn olevaa arvoa. |
| Viestin | Viesti, jos Tietovuo vahvistus-kysely palauttaa mitään tietoja.  Jos annat viestiä, *Suorittamiseen arviointi* näytetään. |
| **Aikavälin** |
| Kesto | Käytettävä aikaväli kyselyn kuluvasta päivästä kesto.  Esimerkiksi jos **7 päivää** on määritetty, valitse kysely on rajoitettu tietueiden 7 päivää aiemmin luotu kuluvaan päivämäärään mennessä. |
| Lopeta tietojen poikkeama | Valinnainen siirtymä käytettävä aikaväli tärkeimmät kyselyn nykyisen tiedoista.  Esimerkiksi jos käytössä on **-1 päivä** , **Lopeta päivämäärä siirtymää** ja käytettävien **kesto** **7 päivää** , valitse kysely on rajoitettu tietueet, jotka on luotu 8 päivää, eilen. |

## <a name="donut-tile"></a>Rengas-ruutu

**Rengas** -ruutu näyttää yhteenveto loki kyselyn arvo sarakkeesta yksittäisen luvun.  Rengas graafisesti yläreunan kolme tietuetta tulokset.

![Rengas-ruutu](media/log-analytics-view-designer/tile-donut.png)

| Asetus | Kuvaus |
|:--|:--|
| Nimi        | Ruudun yläreunassa näkyvä teksti. |
| Kuvaus | Ruutu-apuohjelmassa näytettävä teksti.    |
| **Rengas** |
| Kyselyn | Kysely, joka suoritetaan rengas.  Ensimmäisen ominaisuuden pitäisi olla tekstiarvon ja toinen ominaisuuden numeerisen arvon.  Tämä on yleensä kysely, joka käyttää **Mitta** -avainsanan yhteenveto tulokset. |
| **Rengas** | **> Center** |
| Teksti | Valitse arvo rengas sisällä näkyvä teksti. |
| Toiminto | Tehdä yhteenvedon yksittäisen arvon arvo-ominaisuuden suoritettava toiminto.<br><br>-Summa: Lisää kaikki tietueet arvojen ominaisuuden arvon.<br>-Prosentteina: Ominaisuuden arvo verrattuna kaikki tietueet yhteen arvot yhteen tietueiden arvot prosenttiosuus. |
| Keskitä-toimintoa käytetään tulosarvojen | Valitse vähintään yksi arvot plus-merkkiä.  Kyselyn tulosten rajoittuvat tietueet, joissa voit määrittää ominaisuusarvoihin.  Jos mitään arvoja on lisätty, kuin kaikki tietueet, jotka sisältyvät kyselyn. |
| **Rengas** | **> Lisäasetukset** |
| Värit | Näytä kunkin kolme yläreunan ominaisuudet väri.  Jos haluat määrittää vaihtoehtoiset värit ominaisuuden arvot, käytä Advanced väri yhdistäminen. |
| Kehittyneet värin yhdistäminen | Näyttää tietyn ominaisuusarvoihin väri.  Jos arvolla on kolmen ylimmän, Vaihtoehtoinen väri näkyy vakio värin sijaan.  Jos ominaisuus ei ole kolmen ylimmän, väriä ei ole näkyvissä. |
| **Lisäasetukset** | **> Tietoja tiedonkulun todentaminen** |
| Käytössä | Valitse, jos Tietovuo vahvistus on otettu ruutu.  Tämä on vaihtoehtoinen viestin, jos tiedot eivät ole käytettävissä ruutu.  Tätä käytetään yleensä antamaan viestin tilapäinen aikana, kun näkymä on asennettu ja tiedot tulevat käytettävissä. |
| Kyselyn | Voit tarkistaa, jos tiedot ovat käytettävissä näkymässä suorittaa kyselyn.  Kysely palauttaa tuloksia, jos sanoma näkyy sen sijaan, että tärkeimmät kyselyn olevaa arvoa. |
| Viestin | Viesti, jos Tietovuo vahvistus-kysely palauttaa mitään tietoja.  Jos annat viestiä, *Suorittamiseen arviointi* näytetään. |
| **Aikavälin** |
| Kesto | Käytettävä aikaväli kyselyn kuluvasta päivästä kesto.  Esimerkiksi jos **7 päivää** on määritetty, valitse kysely on rajoitettu tietueiden 7 päivää aiemmin luotu kuluvaan päivämäärään mennessä. |
| Lopeta tietojen poikkeama | Valinnainen siirtymä käytettävä aikaväli tärkeimmät kyselyn nykyisen tiedoista.  Esimerkiksi jos käytössä on **-1 päivä** , **Lopeta päivämäärä siirtymää** ja käytettävien **kesto** **7 päivää** , valitse kysely on rajoitettu tietueet, jotka on luotu 8 päivää, eilen. |

## <a name="line-chart-tile"></a>Viivakaavioruutu

**Viivakaavio** -ruudussa näkyy viivakaaviota, jossa on useita arvosarjoja log kyselystä ajan kuluessa.  

![Viivakaavio ja kuvateksti-ruutu](media/log-analytics-view-designer/tile-line-chart.png)

| Asetus | Kuvaus |
|:--|:--|
| Nimi        | Ruudun yläreunassa näkyvä teksti. |
| Kuvaus | Ruutu-apuohjelmassa näytettävä teksti.    |
| **Viivakaavio** |  
| Kyselyn | Kyselyn suorittaminen viivakaavio.  Ensimmäisen ominaisuuden pitäisi olla tekstiarvon ja toinen ominaisuuden numeerisen arvon.  Tämä on yleensä kysely, joka käyttää **Mitta** -avainsanan yhteenveto tulokset.  Jos kysely käyttää **väli** -avainsanan kaavion akselin käytetään tähän aikaväli.  Jos kyselyä ei ole **väli** -avainsanan tunnin välein käytetään x-akselin. |
| **Viivakaavio** | **> Y-akseli** |
| Käytä Logaritminen asteikko | Valitse tämä vaihtoehto, jos haluat käyttää y-akselin asteikon logaritmiseksi. |
| Yksiköt | Määritä kysely palauttaa arvot yksiköt.  Näytä otsikot kaavion, joka ilmaisee arvon tyypeistä ja halutessasi muuntamisen arvot käyttävät näitä tietoja.  **Yksikön laji** määrittää luokan yksikön ja määrittää **Nykyisen tyyppi** -arvot, jotka ovat käytettävissä.  Jos valitset arvon **muuntaminen** numeeriset arvot muunnetaan **Nykyinen mittayksikkö** tyypistä **muuntaminen** tyyppi. |
| Mukautetun tarran | Y-akselin otsikko yksikkötyyppi-kohdan vieressä näkyvä teksti.  Jos ei nimeä ei määritetä, näkyy vain yksikkötyyppi. |
| **Lisäasetukset** | **> Tietoja tiedonkulun todentaminen** |
| Käytössä | Valitse, jos Tietovuo vahvistus on otettu ruutu.  Tämä on vaihtoehtoinen viestin, jos tiedot eivät ole käytettävissä ruutu.  Tätä käytetään yleensä antamaan viestin tilapäinen aikana, kun näkymä on asennettu ja tiedot tulevat käytettävissä. |
| Kyselyn | Voit tarkistaa, jos tiedot ovat käytettävissä näkymässä suorittaa kyselyn.  Kysely palauttaa tuloksia, jos sanoma näkyy sen sijaan, että tärkeimmät kyselyn olevaa arvoa. |
| Viestin | Viesti, jos Tietovuo vahvistus-kysely palauttaa mitään tietoja.  Jos annat viestiä, *Suorittamiseen arviointi* näytetään. |
| **Aikavälin** |
| Kesto | Käytettävä aikaväli kyselyn kuluvasta päivästä kesto.  Esimerkiksi jos **7 päivää** on määritetty, valitse kysely on rajoitettu tietueiden 7 päivää aiemmin luotu kuluvaan päivämäärään mennessä. |
| Lopeta tietojen poikkeama | Valinnainen siirtymä käytettävä aikaväli tärkeimmät kyselyn nykyisen tiedoista.  Esimerkiksi jos käytössä on **-1 päivä** , **Lopeta päivämäärä siirtymää** ja käytettävien **kesto** **7 päivää** , valitse kysely on rajoitettu tietueet, jotka on luotu 8 päivää, eilen. |


## <a name="line-chart--callout-tile"></a>Rivi kaavion & kuvateksti-ruutu

**Viivakaavio ja kuvateksti** -ruudussa näkyy viivakaaviota, jossa on useita arvosarjoja log kyselystä ajan ja kuvatekstin tiivistetty arvolla.  

![Viivakaavio ja kuvateksti-ruutu](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Asetus | Kuvaus |
|:--|:--|
| Nimi        | Ruudun yläreunassa näkyvä teksti. |
| Kuvaus | Ruutu-apuohjelmassa näytettävä teksti.    |
| **Viivakaavio** |  
| Kyselyn | Kyselyn suorittaminen viivakaavio.  Ensimmäisen ominaisuuden pitäisi olla tekstiarvon ja toinen ominaisuuden numeerisen arvon.  Tämä on yleensä kysely, joka käyttää **Mitta** -avainsanan yhteenveto tulokset.  Jos kysely käyttää **väli** -avainsanan kaavion akselin käytetään tähän aikaväli.  Jos kyselyä ei ole **väli** -avainsanan tunnin välein käytetään x-akselin. |
| **Viivakaavio** | **> Kuvaselite** |
| Kuvateksti | Otsikon tekstin yläpuolella kuvateksti-arvon näyttämiseen. |
| Sarjan nimi | Ominaisuuden arvo sarjojen käytettävä kuvateksti-arvo.  Jos ei sarjoja ei anneta, kaikki tietueet kyselystä käytetään. |
| Toiminto | Tehdä yhteenvedon kuvatekstin yksittäisen arvon arvo-ominaisuuden suoritettava toiminto.<br>-Keskiarvo: Kaikki tietueet arvot tuottojen keskiarvo.<br><br>-Määrä: Kysely palauttaa kaikki tietueet määrä.<br>-Viimeisen malli: Kaavion viimeinen aikavälin arvo.<br>-Maks: Kaavion sisältämää välit suurin sallittu arvo.<br>-Min: Kaavion sisältämää välit pienin arvo.<br>-Summa: Summa kaikki tietueet olevaa arvoa. |
| **Viivakaavio** | **> Y-akseli** |
| Käytä Logaritminen asteikko | Valitse tämä vaihtoehto, jos haluat käyttää y-akselin asteikon logaritmiseksi. |
| Yksiköt | Määritä kysely palauttaa arvot yksiköt.  Näytä otsikot kaavion, joka ilmaisee arvon tyypeistä ja halutessasi muuntamisen arvot käyttävät näitä tietoja.  **Yksikön laji** määrittää luokan yksikön ja määrittää **Nykyisen tyyppi** -arvot, jotka ovat käytettävissä.  Jos valitset arvon **muuntaminen** numeeriset arvot muunnetaan **Nykyinen mittayksikkö** tyypistä **muuntaminen** tyyppi. |
| Mukautetun tarran | Y-akselin otsikko yksikkötyyppi-kohdan vieressä näkyvä teksti.  Jos ei nimeä ei määritetä, näkyy vain yksikkötyyppi. |
| **Lisäasetukset** | **> Tietoja tiedonkulun todentaminen** |
| Käytössä | Valitse, jos Tietovuo vahvistus on otettu ruutu.  Tämä on vaihtoehtoinen viestin, jos tiedot eivät ole käytettävissä ruutu.  Tätä käytetään yleensä antamaan viestin tilapäinen aikana, kun näkymä on asennettu ja tiedot tulevat käytettävissä. |
| Kyselyn | Voit tarkistaa, jos tiedot ovat käytettävissä näkymässä suorittaa kyselyn.  Kysely palauttaa tuloksia, jos sanoma näkyy sen sijaan, että tärkeimmät kyselyn olevaa arvoa. |
| Viestin | Viesti, jos Tietovuo vahvistus-kysely palauttaa mitään tietoja.  Jos annat viestiä, *Suorittamiseen arviointi* näytetään. |
| **Aikavälin** |
| Kesto | Käytettävä aikaväli kyselyn kuluvasta päivästä kesto.  Esimerkiksi jos **7 päivää** on määritetty, valitse kysely on rajoitettu tietueiden 7 päivää aiemmin luotu kuluvaan päivämäärään mennessä. |
| Lopeta tietojen poikkeama | Valinnainen siirtymä käytettävä aikaväli tärkeimmät kyselyn nykyisen tiedoista.  Esimerkiksi jos käytössä on **-1 päivä** , **Lopeta päivämäärä siirtymää** ja käytettävien **kesto** **7 päivää** , valitse kysely on rajoitettu tietueet, jotka on luotu 8 päivää, eilen. |

## <a name="two-timelines-tile"></a>Kaksi aikajanoja-ruutu

**Kaksi aikajanoja** -ruutu näyttää tulosten kaksi log pylväskaaviot aikaan.  Kuvatekstin näkyy kussakin sarjassa.  

![Kaksi aikajanoja-ruutu](media/log-analytics-view-designer/tile-two-timelines.png)

| Asetus | Kuvaus |
|:--|:--|
| Nimi        | Ruudun yläreunassa näkyvä teksti. |
| Kuvaus | Ruutu-apuohjelmassa näytettävä teksti.    |
| Ensimmäisessä kaaviossa   
| Selite | Valitse ensimmäisen sarjan kuvaselitteessä näytettävä teksti.
| Väri | Väri, jota käytetään ensimmäisen sarjan sarakkeet.
| Kaavio Queryn | Kyselyn suorittaminen ensimmäisen sarjan.  Kullakin aikavälillä päälle tietueiden määrän laskeminen edustaa kaavion sarakkeet.
| Toiminto | Tehdä yhteenvedon kuvatekstin yksittäisen arvon arvo-ominaisuuden suoritettava toiminto.<br><br>-Keskiarvo: Kaikki tietueet arvot tuottojen keskiarvo.<br>-Määrä: Kysely palauttaa kaikki tietueet määrä.<br>-Viimeisen malli: Kaavion viimeinen aikavälin arvo.<br>-Maks: Kaavion sisältämää välit suurin sallittu arvo.
| **Toisessa kaaviossa** |
| Selite | Valitse toisen sarjan kuvaselitteessä näytettävä teksti.
| Väri | Toisen sarjan sarakkeet kuvakkeiden väri.
| Kaavio Queryn | Kyselyn suorittaminen toisen sarjan.  Kullakin aikavälillä päälle tietueiden määrän laskeminen edustaa kaavion sarakkeet.
| Toiminto | Tehdä yhteenvedon kuvatekstin yksittäisen arvon arvo-ominaisuuden suoritettava toiminto.<br><br>-Keskiarvo: Kaikki tietueet arvot tuottojen keskiarvo.<br>-Määrä: Kysely palauttaa kaikki tietueet määrä.<br>-Viimeisen malli: Kaavion viimeinen aikavälin arvo.<br>-Maks: Kaavion sisältämää välit suurin sallittu arvo. |
| **Lisäasetukset** | **> Tietoja tiedonkulun todentaminen** |
| Käytössä | Valitse, jos Tietovuo vahvistus on otettu ruutu.  Tämä on vaihtoehtoinen viestin, jos tiedot eivät ole käytettävissä ruutu.  Tätä käytetään yleensä antamaan viestin tilapäinen aikana, kun näkymä on asennettu ja tiedot tulevat käytettävissä. |
| Kyselyn | Voit tarkistaa, jos tiedot ovat käytettävissä näkymässä suorittaa kyselyn.  Kysely palauttaa tuloksia, jos sanoma näkyy sen sijaan, että tärkeimmät kyselyn olevaa arvoa. |
| Viestin | Viesti, jos Tietovuo vahvistus-kysely palauttaa mitään tietoja.  Jos annat viestiä, *Suorittamiseen arviointi* näytetään. |
| **Aikavälin** |
| Kesto | Käytettävä aikaväli kyselyn kuluvasta päivästä kesto.  Esimerkiksi jos **7 päivää** on määritetty, valitse kysely on rajoitettu tietueiden 7 päivää aiemmin luotu kuluvaan päivämäärään mennessä. |
| Lopeta tietojen poikkeama | Valinnainen siirtymä käytettävä aikaväli tärkeimmät kyselyn nykyisen tiedoista.  Esimerkiksi jos käytössä on **-1 päivä** , **Lopeta päivämäärä siirtymää** ja käytettävien **kesto** **7 päivää** , valitse kysely on rajoitettu tietueet, jotka on luotu 8 päivää, eilen. |

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [lokin haut](log-analytics-log-searches.md) tukemaan kyselyt ruudut.
- Lisää mukautettu näkymä [Visualisoinnin osat](log-analytics-view-designer-parts.md) .