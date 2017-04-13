<properties
    pageTitle="Reunan analysoiminen Azure CDN | Microsoft Azure"
    description="Analysoi Microsoft Azure CDN reunan solmu suorituskykyä. Reunan suorituskyvyn Analytics tarjoaa hajautetun tiedot liikenteen ja kaistanleveyden käytön CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Microsoft Azure CDN reunan solmu suorituskyvyn analysoiminen

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Yleiskatsaus
Reunan suorituskyvyn analytics tarjoaa hajautetun tiedot liikenteen ja kaistanleveyden käytön CDN. Nämä tiedot voidaan sitten tykkäysten tilastojen, jonka voit saada tietoja siitä, miten oman varat kierretään välimuistin ja asiakkaiden toimitettu. Näin voit lomakkeen strategia siitä, miten voit optimoida sisällön toimittamisen puolestaan ja voit selvittää, mitä ongelmia pitäisi olla toteuttaa paremmin suorituskykykertoimen, CDN. Tämän vuoksi paitsi voit hakea tietoja toimituksen suorituskyvyn parantamiseksi, mutta myös voi pienentää CDN kustannuksia.

> [AZURE.NOTE] Kaikkien raporttien Käytä UTC-ajan/GMT merkintätapaa määritettäessä päivämäärä ja kellonaika.

## <a name="reports-and-log-collection"></a>Raporttien ja log sivustokokoelman

CDN tehtävätietojen täytyy kerätä reunan suorituskyvyn Analytics-moduulin, ennen kuin se voit luoda raportteja sitä. Sivustokokoelman yhteydessä ilmenee, kun päivän ja sitä käsitellään tehtävä, jota on tapahtunut edellisen päivän aikana. Tämä tarkoittaa, että raportin tilastotiedot esittää päivän tilastojen otoksen milloin se käsiteltiin ja eivät välttämättä ole sisältävät kuluvan päivän tietojen sarja. Näiden raporttien ensisijainen toiminto on arvioida suorituskykyä. Niitä ei voi käyttää laskutuksen tarkoituksiin tai tarkka numeerinen tilastotiedot.

> [AZURE.NOTE] Raaka tiedot, joista reunan suorituskyvyn analyyttisten raportteja luodaan on vähintään 90 päivää.

## <a name="dashboard"></a>Raporttinäkymät-ikkunan

Reunan suorituskyvyn Analytics Raporttinäkymät-ikkunan seuraa nykyisten ja historiallisten CDN liikenne kaavion ja tilastot. Tunnistaa uusimpia ja pitkään trendejä CDN liikenne tilissäsi suorituskykyyn tämän Raporttinäkymät-ikkunan avulla.

Tämä koontinäyttö sisältää seuraavat tiedot:

* Vuorovaikutteisen kaavion, joka sallii avaimen arvot ja trendien visualisoinnin.
* Aikajana, joka sisältää pitkällä aikavälillä kuviot tarkoitetaan avaimen arvot ja trendien.
* Avaimen arvot ja tilastollisia lisätietoja siitä, miten Microsoftin CDN verkon parantaa sivuston liikenteen yleistä suorituskykyä, käyttö ja tehokkuutta mitattuna.

### <a name="accessing-the-edge-performance-dashboard"></a>Reunan suorituskyvyn Raporttinäkymät-ikkunan käyttäminen

1. Valitse **hallinta** -painike CDN profiili-sivu.

    ![CDN profiilin sivu hallinta-painike](./media/cdn-edge-performance/cdn-manage-btn.png)

    CDN hallinta-portaalin avautuu.

2. Vie hiiren osoitin **Analytics** -välilehti ja valitse Vie hiiren osoitin **Reunan Perfomance Analytics** -valikon avauspainike.  Valitse **koontinäytössä**.

    Näkyviin tulee reunan solmu analysoinnin koontinäytössä.

### <a name="chart"></a>Kaavio

Koontinäytön sisältää kaavion, joka seuraa mittarin aikajanan suoraan sen alapuolella näkyvää valitulle ajanjaksolle päälle.  Kaavioiden ylös viimeisen kahden vuoden aikana CDN toiminnan aikajanan näkyy suoraan kaavion alapuolella.

#### <a name="using-the-chart"></a>Kaavion käyttäminen

* Oletusarvon mukaan piirrettävät välimuistin tehokkuuden arvolla ja viimeisten 30 päivän aikana.
* Tässä kaaviossa luodaan tiedoista, jotka on lajiteltu päivittäin.
* Kohdalla viivakaaviota päivässä osoittaa päivämäärän ja lisätiedot arvon tiettynä päivänä.
* Valitse Vaihda Vaalea harmaat pystysuora palkit, jotka edustavat viikonloput kaavioon ja Korosta viikonloput. Tällaista päällekkäin kannattaa yksilöimiseen liikenne kuviot viikonloput päälle.
* Valitse Näytä yksi vuosi sitten Vaihda ja edellisen vuoden tehtävän saman ajanjaksolla kaavioon. Vertailun tulokset on pitkällä aikavälillä CDN käyttötavat huomioon. Kaavion oikeassa yläkulmassa on selite, joka osoittaa kunkin rivin kaavion värikoodi.

#### <a name="updating-the-chart"></a>Kaavion päivittäminen

* Aikavälin: Tee jompikumpi seuraavista:
    * Valitse haluamasi alue aikajanan. Kaavio päivitetään tiedoilla, joka vastaa valitulle aikajaksolle.
    * Kaksoisnapsauta Näytä kaikkia käytettävissä olevia historiatietoja enintään kahden vuoden aikana kaaviota.
* Arvo: Haluamasi lisätiedot vieressä näkyvä Kaaviokuvaketta. Kaavion ja aikajanan päivitetään vastaavan metrijärjestelmän tiedoilla.


### <a name="key-metrics-and-statistics"></a>Avaimen arvot ja tilastot

#### <a name="efficiency-metrics"></a>Tehokkuuden arvot

Nämä arvot tarkoituksena on kohdassa, onko välimuistin tehokkuutta voidaan parantaa. Johdettu välimuistin tehokkuuden tärkeimmät etuja ovat:

* Mikä voi johtaa origin palvelimen kuormitus vähenee:
    * Web-palvelimen suorituskyvyn parantamiseksi.
    * Rajoitetun toiminnan kustannukset.
* Parannettu tietojen toimituksen acceleration, koska enemmän pyyntöjä served suoraan CDN.

Kenttä | Kuvaus
------|------------
Välimuistin tehokkuutta | Osoittaa siirtää tietoja, jotka on served välimuistista prosentteina. Tämä metrisillä toimenpiteitä, kun pyydetty sisällön välimuistiin tallennettu versio on served suoraan CDN (reunapalvelimet) epämuodollisista (esimerkiksi web-selaimessa)
Osumien korko | Ilmaisee, jotka olivat served välimuistista pyynnöt prosentteina. Tämä metrisillä toimenpiteitä, kun pyydetty sisällön välimuistiin tallennettu versio on served suoraan CDN (reunapalvelimet) epämuodollisista (esimerkiksi web-selaimessa).
Remote tavua - välimuistin ei Config % | Osoittaa liikenteestä, joka on served origin palvelimien CDN (reunapalvelimet), joka ei ole tallennetaan välimuistiin tuloksena ohituksen välimuisti-ominaisuuden (HTTP säännöt ohjelma) prosentteina.
prosenttia Remote tavua - vanhentunut välimuisti | Osoittaa liikenteestä, joka on served origin palvelimien CDN (reunapalvelimet) prosentteina vanhentuneiden sisällön uudelleenvahvistusta tuloksena.

#### <a name="usage-metrics"></a>Käyttö-arvot

Nämä arvot tarkoituksena on selventäminen kustannukset leikkaaminen seuraavat toimenpiteet:

* Pienentäminen toiminnallisia kustannukset CDN kautta.
* Vähentää CDN menot välimuistin tehokkuuden ja pakkaamisen kautta.

> [AZURE.NOTE] Liikenne aseman numerot vastaavat liikenteestä, joka on käytetty laskentaan suhteiden ja prosenttien ja paljon asiakkaiden vain saattaa näyttää osan yhteensä liikenne.

Kenttä | Kuvaus
------|------------
Tallenna tavua ulos | Osoittaa sivupyynnön served CDN (reunapalvelimet)-pyytäjä (esimerkiksi web-selaimessa) siirrettyjen tavujen määrän keskiarvo.
Ei ole välimuistin Config tavu korko | Osoittaa liikenne mainos pyytäjä (esimerkiksi web-selaimessa), joka tallennetaan välimuistiin ei vuoksi ohituksen välimuistin-ominaisuus-CDN (reunapalvelimet) prosentteina.
Pakattu tavu korko | Osoittaa liikenne lähettää CDN (reunapalvelimet) pyytäjät (esimerkiksi web-selaimessa) prosentteina pakattu.
Tavua ulos | Osoittaa tietojen tavuina, joka on toimitettu CDN (reunapalvelimet), pyytäjä (esimerkiksi web-selaimessa) määrää.  
Tavua | Tietojen tavuina määrää lähettämät pyytäjät (esimerkiksi web-selaimessa) ilmaisee CDN (reunapalvelimet).
Tavua etäyhteyksien | Määrittää tietojen lähetettyjen CDN ja asiakkaan origin palvelimien CDN (reunapalvelimet) tavujen määrän.

#### <a name="performance-metrics"></a>Suorituskyvyn mittarit

Nämä arvot tarkoituksena on seurata tietoliikenteestä CDN suorituskykyyn.

Kenttä | Kuvaus
------|------------
Tiedonsiirto | Ilmaisee, jossa sisältö on siirretty kohteesta CDN pyytäjä keskimääräinen kurssi.
Kesto | Ilmaisee keskimääräinen aika millisekunteina kului pitää sijoituksen pyytäjä (esimerkiksi web-selaimessa).
Pakattu pyynnön korko | Osoittaa osumaa, joka on toimitettu CDN (reunapalvelimet), pyytäjä (esimerkiksi web-selaimessa) prosentteina pakattu.
4xx virhe korko | Ilmaisee, joka on luotu 4xx tilakoodin osumien prosenttiosuus.
5xx virhe korko | Ilmaisee, joka on luotu 5xx tilakoodin osumien prosenttiosuus.
Osumia | Osoittaa pyyntöjen CDN sisällön määrä.

#### <a name="secure-traffic-metrics"></a>Suojattu tietoliikenne arvot

Nämä arvot tarkoituksena on seurata CDN suorituskyvyn HTTPS-liikenne.

Kenttä | Kuvaus
------|------------
Suojatun välimuistin tehokkuutta | Osoittaa tiedonsiirron HTTPS-pyyntöihin, jotka olivat served välimuistista prosentteina. Tämä metrisillä toimenpiteitä, kun pyydetty sisällön välimuistiin tallennettu versio on served suoraan CDN (reunapalvelimet) epämuodollisista (esimerkiksi web-selaimessa) HTTPS-protokollan välityksellä.
Suojatun tiedonsiirto | Ilmaisee, jossa sisältö on siirretty CDN (reunapalvelimet) pyytäjät (esimerkiksi web-palvelimet) keskimääräinen kurssi HTTPS-protokollan välityksellä.
Keskimääräinen suojatun kesto | Ilmaisee keskimääräinen aika millisekunteina kului pitää sijoituksen pyytäjä (esimerkiksi web-selaimessa) HTTPS-protokollan välityksellä.
Suojatun osumia | Ilmaisee HTTPS-pyyntöjä CDN sisällön määrän.
Suojatun tavua ulos | Osoittaa HTTPS-liikennettä tavuina, joka on toimitettu CDN (reunapalvelimet), pyytäjä (esimerkiksi web-selaimessa).

## <a name="reports"></a>Raportit

Kunkin raportin moduuli sisältää kaavio ja kaistanleveys ja liikenteen käyttö erityyppisten arvot tilastotietoja (esimerkiksi HTTP-tilakoodin välimuistin tilakoodin pyynnön URL-osoite, jne.). Nämä tiedot voidaan käyttää delve tarkempaa kyselyjä siitä, miten sisältö on parhaillaan served asiakkaat ja hienosäätää CDN toiminta tietojen toimituksen suorituskyvyn parantamiseksi.

### <a name="accessing-the-edge-performance-reports"></a>Reunan suorituskyky-raporttien käyttäminen

1. Valitse **hallinta** -painike CDN profiili-sivu.

    ![CDN profiilin sivu hallinta-painike](./media/cdn-edge-performance/cdn-manage-btn.png)

    CDN hallinta-portaalin avautuu.

2. Vie hiiren osoitin **Analytics** -välilehti ja valitse Vie hiiren osoitin **Reunan Perfomance Analytics** -valikon avauspainike.  Valitse **HTTP objekti**.

    Reunan solmu analytics-raporttien näyttö tulee näkyviin.

Raportti | Kuvaus
-------|------------
Päivittäinen yhteenveto | Voit tarkastella päivittäinen liikenne trendejä määritetyn ajan kuluessa. Tässä kaaviossa kukin palkki edustaa tietyn päivämäärän. Palkin kokoa osoittaa tiettynä päivänä on tapahtunut osumien kokonaismäärä.
Tunnin välein yhteenveto | Voit tarkastella kerran tunnissa liikenne trendejä määritetyn ajan kuluessa. Tässä kaaviossa kukin palkki edustaa yhden tunnin tiettynä päivänä. Palkin kokoa ilmaisee, että tunnin aikana on tapahtunut osumien kokonaismäärä.
Protokollat | Näyttää jakautuminen liikenne HTTP ja HTTPS-protokollia välillä. Rengas-kaavio osoittaa mistäkin protokolla on tapahtunut osumien prosenttiosuus.
HTTP-menetelmiä | Voit diaesitystilassa mitä HTTP menetelmiä käytetään pyytää tietoja. Yleensä yleisimmät HTTP-pyyntö menetelmiä ovat GET, PÄÄTÄ ja JULKAISE. Rengas-kaavio osoittaa mistäkin HTTP pyyntömenetelmä on tapahtunut osumien prosenttiosuus.
URL-osoitteet | Sisältää kaavion, joka näyttää 10 tarvittavat osoitteet. Palkin näkyy kunkin URL-osoitteen. Palkin korkeus osoittaa, kuinka monta osumaa, erityisesti URL-osoite on luonut päälle loppupäivät raportin kattaman. Ensimmäiset 100 tilastotiedot pyydetty URL-osoitteet näkyvät alapuolella tämä kaavio.
CNAMEs | Sisältää kaavion, joka näyttää 10 CNAMEs avulla voidaan pyytää varat ajan kattavat raportin yläreunassa. Ensimmäiset 100 tilastotiedot pyytänyt CNAMEs näkyvät alapuolella tämä kaavio.
Alkuperistä | Sisältää kaavion, joka näyttää Ylin 10 CDN tai asiakkaan origin palvelimissa, joista varat pyydettiin määritetyn ajan kuluessa. Ensimmäiset 100 tilastotiedot pyytänyt CDN tai asiakkaan origin palvelimet näkyvät alapuolella tämä kaavio. Asiakkaan origin palvelinten tunnistetaan kansionimi-kohdassa määritetyn nimen.
GEO ponnahtaa | Näyttää, kuinka paljon tietoliikenteestä reititetään erityisesti piste-,-tavoitettavuustietojen avulla (POP). Kolme kirjain lyhennetty muoto edustaa POP Microsoftin CDN verkossa.
Asiakkaat | Sisältää kaavion, joka näyttää Ylin 10 asiakkaat, joka pyytää varat määritetyn ajan kuluessa. Tämän raportin tarkoitetaan kaikki pyynnöt, jotka ovat peräisin sama IP-osoite pidetään saman asiakasohjelmassa. Ylimmät 100 asiakasta tilastotiedot näkyvät alapuolella tämä kaavio. Tämä raportti on hyötyä määrittämiseksi Lataa tehtävän kuviot yläreunan asiakkaille.
Välimuistin tilat | Antaa erittelyn välimuistin ongelman, joka saattaa paljastaa tavoista yleinen käyttäjäkokemuksen parantamiseksi. Koska nopein suorituskyky peräisin osumia, voit optimoida tietojen toimituksen nopeuksia pienentäminen välimuistin epäonnistumisten ja vanhentuneet osumia.
Ei mitään tietoja | Sisältää kaavion, joka näyttää resurssit, jonka välimuistin sisällön tuoreus on tarkisteta määritetyn ajan kuluessa Ylin 10 URL-osoitetta. Ylimmät 100 URL-osoitteet, tämäntyyppisille kalusto tilasto näkyvät alapuolella tämä kaavio.
CONFIG_NOCACHE tiedot | Sisältää kaavion, joka näyttää kalusteet, joiden vuoksi asiakkaan CDN määritys ei välimuistiin top 10 URL-osoitteet. Resurssien seuraavanlaisiin on served suoraan alkuperäisestä palvelimesta. Ylimmät 100 URL-osoitteet, tämäntyyppisille kalusto tilasto näkyvät alapuolella tämä kaavio.
UNCACHEABLE tiedot | Sisältää kaavion, joka näyttää kalusteet, joiden ei voitu välimuistiin pyynnön otsikon tiedot Ylin 10 URL-osoitetta. Ylimmät 100 URL-osoitteet, tämäntyyppisille kalusto tilasto näkyvät alapuolella tämä kaavio.
TCP_HIT tiedot | Sisältää kaavion, joka näyttää kalusteet, joiden saapuvat heti välimuistin Ylin 10 URL-osoitetta. Ylimmät 100 URL-osoitteet, tämäntyyppisille kalusto tilasto näkyvät alapuolella tämä kaavio.
TCP_MISS tiedot | Sisältää kaavion, joka näyttää kalusteet, joiden välimuistin tila on TCP_MISS Ylin 10 URL-osoitetta. Ylimmät 100 URL-osoitteet, tämäntyyppisille kalusto tilasto näkyvät alapuolella tämä kaavio.
TCP_EXPIRED_HIT tiedot | Sisältää kaavion, joka näyttää vanhentuneiden kalusteet, joiden on served suoraan POP Ylin 10 URL-osoitetta. Ylimmät 100 URL-osoitteet, tämäntyyppisille kalusto tilasto näkyvät alapuolella tämä kaavio.
TCP_EXPIRED_MISS tiedot | Sisältää kaavion, joka näyttää vanhentuneiden kalusto, jonka uusi versio oli noudetaan alkuperäisestä palvelimesta Ylin 10 URL-osoitetta. Ylimmät 100 URL-osoitteet, tämäntyyppisille kalusto tilasto näkyvät alapuolella tämä kaavio.
TCP_CLIENT_REFRESH_MISS tiedot | Sisältää palkkikaavion, jossa näyttää top 10 URL-osoitteet varat on haettu origin-palvelin ei välimuistin pyynnön asiakaskoneesta vuoksi. Ylimmät 100 URL-osoitteet, pyynnöt tämäntyyppisille tilastotiedot näkyvät alapuolella Tässä kaaviossa.
Pyyntö Asiakastyyppien | Osoittaa ryhmän tyypin pyynnöt tekemät muutokset HTTP-asiakkaat (esimerkiksi selaimet). Tämä raportti sisältää rengas kaavioihin, joissa on järkevää siitä, miten pyynnöt käsitellään. Pyyntö mistäkin kaistanleveys ja liikenteen tiedot näkyvät kaavion alapuolella.
Käyttäjäagentti | Sisältää näyttäminen yläreunan pyytää sisältöä sekä CDN – 10 käyttäjäagenttien pylväskaavio. Käyttäjäagentti on yleensä selaimessa, media Player-ohjelmassa tai matkapuhelimen selaimen. Ylimmät 100 käyttäjäagenttien tilastotiedot näkyvät alapuolella Tässä kaaviossa.
Viittaajat | Sisältää näyttäminen 10 viittaajat kautta Microsoftin CDN sisällön pylväskaavio. Yleensä referrer on verkkosivu tai resurssi, joka toimii linkkinä sisällön URL-osoite. Ensimmäiset 100-viittaajat on tarkoitettu yksityiskohtaiset tiedot kaavion alapuolelle.
Pakkaamisen tyypit | Sisältää rengas kaavioihin, joissa erittelee pyydetty resurssien mukaan, onko ne on pakattu meidän reunapalvelimet. Pakattu varat prosentteina on eritelty pakkaamisen käytetään tyypin mukaan. Yksityiskohtaiset tiedot on tarkoitettu kaavion alla olevassa kunkin pakkaamisen tyyppi ja tila.
Tiedostotyypit | Sisältää pylväskaavio, jossa näkyy Ylin 10 tiedostotyypit, jotka on pyydetty Microsoftin CDN-tilin kautta. Tämän raportin tarkoitetaan tiedostotyyppi määrittämän kohteen tiedostonimen tunniste ja Internet-mediatyyppi (esimerkiksi .html \[teksti/html\], .htm \[teksti/html\], .aspx \[teksti/html\]jne.). Yksityiskohtaiset tiedot on tarkoitettu kaavion alla olevassa ylimmät 100 tiedostotyypit.
Yksilöivä tiedostot | Sisältää kaavion, joka esittää kokonaismäärän yksilöllinen kalusteet, joiden pyydettiin tiettynä päivänä määritetyn ajanjakson aikana.
Suojaustunnuksen Auth yhteenveto | Sisältää ympyräkaavio, jossa kuvataan lyhyesti, onko pyydetty varat on suojattu tunnuksen lomakepohjaisen todennuksen määrittäminen. Suojatun resurssien näkyvät kaavion tulokset niiden yritettiin käyttöoikeuksien mukaan.
Suojaustunnuksen Auth estä tiedot | Sisältää pylväskaavio, jonka avulla voit tarkastella Ylin 10 pyyntöjä, joka on estetty, koska tunnuksen lomakepohjaisen todennuksen määrittäminen.
HTTP-vastauksen koodit | Sisältää HTTP-tilakoodin jakautuminen (esimerkiksi 200 OK, 403 Käyttö estetty-404 ei löydy, jne.), joka on toimitettu HTTP-asiakkaille sekä reunapalvelimet. Ympyräkaavion voit arvioida nopeasti, miten oman varat on served. Yksityiskohtainen tilastollisten tietojen annetaan kunkin vastauksen koodin kaavion alapuolelle.
404 virheet | Sisältää pylväskaavio, jonka avulla voit tarkastella Ylin 10 pyyntöjä, jotka ovat 404 ei löydy vastausta koodin.
403 virheet | Sisältää pylväskaavio, jonka avulla voit tarkastella Ylin 10 pyyntöjä, jotka ovat 403 Käyttö estetty-vastauksen koodi. 403 Käyttö estetty-vastauksen koodin tapahtuu, kun pyyntö estetty asiakkaan origin palvelimeen tai tutustu POP reuna-palvelimelle.
4xx virheet | Sisältää pylväskaavio, jonka avulla voit tarkastella Ylin 10 pyyntöjä, jotka ovat vastauksen koodin 400 alueen. Tämän raportin ulkopuolelle ovat 403 ei löydy ja 404 kielletty vastauksen koodit. Yleensä 4xx vastauksen koodi tapahtuu, kun pyyntö estetty tuloksena asiakasvirhe.
504 virheet | Sisältää pylväskaavio, jonka avulla voit tarkastella Ylin 10 pyyntöjä, jotka ovat 504 yhdyskäytävän aikakatkaisu vastauksen koodin. 504 yhdyskäytävän aikakatkaisu vastauksen koodi tapahtuu, kun aikakatkaisu tapahtuu, kun HTTP-välityspalvelin yrittää kommunikoida toisen palvelimen kanssa. Tutustu CDN kyseessä 504 yhdyskäytävän aikakatkaisu vastauksen koodi yleensä ilmenee, kun edge server ei voi muodostaa yhteyksissä asiakkaan origin palvelimeen.
502 virheet | Sisältää pylväskaavio, jonka avulla voit tarkastella Ylin 10 pyyntöjä, jotka ovat 502 – virheellinen yhdyskäytävä vastauksen koodin. 502 – virheellinen yhdyskäytävä vastauksen koodi tapahtuu, kun HTTP-protokolla-virhe ilmenee palvelimen ja HTTP-välityspalvelin. Tutustu CDN kyseessä 502 – virheellinen yhdyskäytävä vastauksen koodi yleensä ilmenee, kun asiakkaan origin palvelimen palauttaa virheellisen vastauksen reuna-palvelimeen. Vastaus on virheellinen, jos sitä ei voi jäsentää tai jos se ei ole täydellinen.
5xx virheet | Sisältää pylväskaavio, jonka avulla voit tarkastella Ylin 10 pyyntöjä, jotka ovat 500 alueen vastauksen koodin.  Tämän raportin ulkopuolelle ovat 502 – virheellinen yhdyskäytävä ja 504 yhdyskäytävän aikakatkaisu vastauksen koodit.

## <a name="see-also"></a>Katso myös
* [Azure CDN yleiskatsaus](cdn-overview.md)
* [Reaaliaikainen tilasto-Microsoft Azure CDN](cdn-real-time-stats.md)
* [Säännöt-ohjelma käyttää HTTP-oletustoiminnan ohittaminen](cdn-rules-engine.md)
* [Lisäasetukset HTTP-raportit](cdn-advanced-http-reports.md)
