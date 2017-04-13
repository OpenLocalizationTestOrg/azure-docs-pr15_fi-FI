<properties
    pageTitle="Azure CDN Advanced HTTP raporttien | Microsoft Azure"
    description="Microsoft Azure CDN HTTP-raportit Lisäasetukset. Näiden raporttien Anna CDN tehtävän yksityiskohtaiset tiedot."
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

# <a name="advanced-http-reports-in-microsoft-azure-cdn"></a>Microsoft Azure CDN Lisäasetukset HTTP-raportit

## <a name="overview"></a>Yleiskatsaus

Tässä asiakirjassa kerrotaan Lisäasetukset HTTP raportoinnin Microsoft Azure CDN. Näiden raporttien Anna CDN tehtävän yksityiskohtaiset tiedot.

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Lisäasetukset HTTP-raporttien käyttäminen

1. Valitse **hallinta** -painike CDN profiili-sivu.

    ![CDN profiilin sivu hallinta-painike](./media/cdn-advanced-http-reports/cdn-manage-btn.png)

    CDN hallinta-portaalin avautuu.

2. Vie hiiren osoitin **Analytics** -välilehti ja valitse Vie hiiren osoitin **Advanced HTTP-raportit** -valikon avauspainike.  Valitse **suuri HTTP**-ympäristössä.

    ![CDN hallinta-portaalin - Advanced Raportit-valikko](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)

    Raportin asetukset ovat näkyvissä.

## <a name="geography-reports-map-based"></a>Geography-raportit (määritykseen perustuvien)

On viisi raportteja, jotka hyödyntävät kartan ilmaisemaan, jonka sisältöä haetaan alueet. Näiden raporttien ovat Maailmankartta, USA: ssa kartan, Kanada kartan, Europe kartta ja Aasian ja Tyynenmeren alueen kartta.

Kunkin määritykseen perustuvien raportin olettaa maantieteelliset kohteiden (kuten maat, tilan ja provinsseissa), joka on peräisin osumien prosenttiosuus mukaan alueen. Lisäksi kartan annetaan avulla voit visualisoida sijainnit, joista sisältöä haetaan. Se pystyy tekemään niin värikoodaus kunkin alueen demand kokeneilta alueella olevat määrän mukaan. Työntöproomua Varjostettu alueiden osoittavat alemman demand sisällölle, kun tummempi alueiden osoittaa tarvittaessa sisällölle korkeammat tasot.

Yksityiskohtainen liikenteen ja kaistanleveyden tiedot kunkin alueen toimitetaan suoraan kartan alapuolella. Näin voit tarkastella osumaa, osumien prosenttiosuus, tietojen määrä kokonaismäärän siirretään (gigatavuina), ja tietojen prosenttiosuus siirretään kunkin alueen. Tarkastele nämä arvot kuvaus. Lopuksi, kun viet osoittimen alueen (eli maan, osavaltion tai provinssin), nimi ja alueella on tapahtunut osumien prosenttiosuus näkyvät työkaluvihjeenä.

Lyhyt kuvaus on jäljempänä mistäkin määritykseen perustuvien geography-raportti.

Raporttinimi | Kuvaus
------------|------------
Maailmankartta | Tämän raportin avulla voit tarkastella maailmanlaajuisesti demand CDN sisällölle. Kunkin maan on värikoodattu Maailmankartta ilmaisemaan, joka on peräisin alueen osumien prosenttiosuus.
Yhdysvallat kartta | Tämän raportin avulla voit tarkastella CDN sisällölle demand Yhdysvalloissa. Kukin tila on väreillä merkityt tämän kartalla ilmaisemaan, joka on peräisin alueen osumien prosenttiosuus.
Kanada kartta | Tämän raportin avulla voit tarkastella CDN sisällölle demand Kanadassa. Kunkin provinssi on väreillä merkityt tämän kartalla ilmaisemaan, joka on peräisin alueen osumien prosenttiosuus.
Europe kartta | Tämän raportin avulla voit tarkastella CDN sisällölle tarvittaessa Euroopan. Kunkin maan on väreillä merkityt tämän kartalla ilmaisemaan, joka on peräisin alueen osumien prosenttiosuus.
Aasian Tyynenmeren kartta | Tämän raportin avulla voit tarkastella CDN sisällölle demand Aasiassa. Kunkin maan on väreillä merkityt tämän kartalla ilmaisemaan, joka on peräisin alueen osumien prosenttiosuus.

## <a name="geography-reports-bar-charts"></a>Geography-raportit (palkkikaaviot)

On kaksi lisäraportteja tilastollinen mukaan geography-tietoja, joista on ylhäällä kaupunkien ja Top maat. Näiden raporttien arvioi kaupunkien ja maissa, vastaavasti osumaa, joka on peräisin alueilla määrän mukaan. Palkkikaavion lisääminen osoittaa yhteydessä luodaan tällaista raportin, Ylin 10 kaupunkien tai maat, jotka pyydetty sisällön tiettyyn ympäristö päälle. Tämä palkkikaavion avulla voit arvioida nopeasti alueet, jotka luovat pyynnöt sisällölle suurinta numeroa.

Kaavio (y) vasemmalla puolella osoittaa, kuinka monta tapahtui määritetyllä alueella. Kaavion (x), alla olevassa etsii selitteen kunkin Ylin 10 alueet.

### <a name="using-the-bar-charts"></a>Palkki-kaavioiden avulla

* Jos asetat palkin päälle, nimi ja osumien alueella on tapahtunut kokonaismäärä näytetään työkaluvihjeenä.
* Ensimmäiset kaupunkien raportin työkaluvihje tunnistaa kaupungin nimi, osavaltion/provinssin ja maan lyhenne.
* Jos kaupungin tai alueen (eli, osavaltion/provinssin), josta pyyntö on peräisin ei onnistunut, valitse se ilmaisee, että ne ovat tuntematon. Jos maa on tuntematon, ja sitten kaksi kysymysmerkki (eli?), näytetään.
* Raportin voi sisältyä mittaukset "Europe" tai "Aasian ja Tyynenmeren alue". Kohteiden ei ole tarkoitettu antamaan tilastotiedot kaikki IP-osoitteiden alueilla. Sen sijaan niitä koskevat vain Europe tai Aasian ja Tyynenmeren sijaan ovat levittäminen tietyn kaupunki tai maa IP-osoitteiden peräisin olevien pyyntöjen.

Tiedot, joita on käytetty luomiseen palkkikaavion voi tarkastella sen alapuolella. Löydät osumaa, osumien prosenttiosuus, tietomäärän kokonaismäärän siirretään (gigatavuina) ja tietojen prosenttiosuus siirretty ylös 250 alueiden. Tarkastele nämä arvot kuvaus.

Kummankintyyppisten alla raportteja varten on annettu lyhyt kuvaus.

Raporttinimi | Kuvaus
------------|------------
Ensimmäiset kaupunkien | Tämä raportti olettaa kaupunkien osumaa, joka on peräisin määrän mukaan alueen.
Ylimmät maat | Tämä raportti olettaa maat, jotka ovat peräisin osumien määrä mukaan alueen.

## <a name="daily-summary"></a>Päivittäinen yhteenveto

Päivittäinen yhteenveto-raportin avulla voit tarkastella osumien ja päivittäin yli tietyn käyttötason siirrettyjä kokonaismäärä. Nämä tiedot voidaan discern nopeasti CDN tehtävän kuviot. Esimerkiksi Tämä raportti auttaa sinua tunnistamaan päivät kokeneilta suurempi tai pienempi kuin odotettu liikenne.

Yhteydessä luodaan tällaista raportin palkkikaavio päivitystavasta kuvallinen siten, että käyttöjärjestelmäkohtaiset demand kokeneilta määrä päivittäin raportin kattaman ajan kuluessa. Se tehdä näyttämällä palkin päivittäinen raportin. Esimerkiksi valitsemalla ajanjakson nimeltä "Viime viikolla" Luo palkkikaavio seitsemän palkkeina. Kukin palkki tarkoittaa kokeneilta samana päivänä osumien kokonaismäärä.

Kaavio (y) vasemmalla puolella näkyy, kuinka monta on tapahtunut määritetyn päivämäärän. Kaavion (x), alla olevassa löydät otsikkoa, joka osoittaa päivämäärän (muodossa: YYYY-MM-DD) päivittäinen raporttiin.

> [AZURE.TIP] Jos asetat palkin päälle, osumien tiettynä päivänä on tapahtunut kokonaismäärä näkyy työkaluvihjeenä.

Tiedot, joita on käytetty luomiseen palkkikaavion voi tarkastella sen alapuolella. Olemassa Etsi osumien kokonaismäärä ja siirrettyjen (gigatavuina) tietojen määrän päivittäinen raportin kattaman.

## <a name="by-hour"></a>Tunnin mukaan

Tunti mukaan-raportin avulla voit tarkastella kokonaismäärän osumien ja yli tietyn käyttötason siirrettyjä tunnissa. Nämä tiedot voidaan discern nopeasti CDN tehtävän kuviot. Esimerkiksi Tämä raportti auttaa sinua tunnistamaan aikajaksot päivän aikana, ilmenee suurempi tai pienempi kuin odotettu liikenne.

Yhteydessä luodaan tällaista raportin palkkikaavio antaa kuvallinen siten, että käyttöjärjestelmäkohtaiset demand listan tunnissa raportin kattama ajanjaksolla määrää. Se ohjelmiston palkin näyttäminen raportin kattama tunnin välein. Esimerkiksi valitsemalla 24 tunnin ajanjakso Luo palkkikaavio kahdenkymmenen neljä on esitetty palkkeina. Kukin palkki tarkoittaa kokeneilta kyseisen tunnin aikana osumien kokonaismäärä.

Kaavio (y) vasemmalla puolella osoittaa, kuinka monta on tapahtunut määritetyn tunti. Kaavion (x), alla olevassa löydät otsikkoa, joka ilmaisee, päivämäärä ja kellonaika (muodossa: YYYY-MM-DD hh: mm) kunkin tunnin raporttiin. Aika ilmoitetaan 24 tunnin muodossa ja se on määritetty käyttämällä UTC-ajan/GMT aikavyöhyke.

> [AZURE.TIP] Jos asetat palkin päälle, kyseisen tunnin aikana on tapahtunut osumien kokonaismäärä näkyy työkaluvihjeenä.

Tiedot, joita on käytetty luomiseen palkkikaavion voi tarkastella sen alapuolella. Sinun on löytää osumien kokonaismäärä ja siirrettyjen (gigatavuina) tietojen määrän raportin kattaman tunnin välein varten.

## <a name="by-file"></a>Tiedosto

Mukaan-raportin avulla voit tarkastella demand ja kertyneet päälle eniten pyydetty varat tietyn ympäristön liikenteen määrää. Yhteydessä luodaan tällaista raportin palkkikaavio luodaan 10 eniten pyydetty varat määritetyn ajan kuluessa.

> [AZURE.NOTE] Tämän raportin tarkoitetaan reunan CNAME URL-osoitteet muunnetaan niiden vastaavat CDN URL-osoitteet. Näin tarkkoja yhdenmukaisuuden, riippumatta CDN tai reunan CNAME URL-Osoitetta käytetään pyytää niiden omaisuuden liittyvät osumien kokonaismäärä varten.

Kaavio (y) vasemmalla puolella osoittaa pyyntöjen kunkin kohteiden määrä määritetyn ajan kuluessa. Suoraan alla kaavio (x), löydät otsikkoa, joka ilmaisee tiedostonimen kunkin 10 pyydetty kohdalla.

Tiedot, joita on käytetty luomiseen palkkikaavion voi tarkastella sen alapuolella. Löydät kunkin 250 pyydetty kohdalla seuraavat tiedot: suhteellinen polku, osumaa, osumien prosenttiosuus, tietomäärän kokonaismäärän siirretään (gigatavuina), ja tietojen prosenttiosuus siirretään.

## <a name="by-file-detail"></a>Mukaan-tiedoston tiedot

Tiedoston tiedot raportin avulla voit tarkastella demand ja kertyneet päälle, tietyn annetaan tietyn käyttötason liikenteen määrää. Tämän raportin ylimpänä on tiedoston tiedot varten-vaihtoehto. Tämä vaihtoehto sisältää eniten pyydetty resurssien luettelon valitun ympäristössä. Tarvitset raportti tiedoston tiedot, jotta voit valita haluamasi kohteen tiedoston tiedot varten-vaihtoehto. Minkä jälkeen palkkikaavio kertovat päivittäin vaatia, että se luo määritetyn ajan kuluessa määrää.

Vasemmalla puolella kaavio (y) ilmaisee, että sijoituksen listan tiettynä päivänä pyyntöjen kokonaismäärä. Kaavion (x), alla olevassa löydät otsikkoa, joka osoittaa päivämäärän (muodossa: YYYY-MM-DD), mitkä CDN kohteen tarve ilmoitettiin.

Tiedot, joita on käytetty luomiseen palkkikaavion voi tarkastella sen alapuolella. Olemassa Etsi osumien kokonaismäärä ja siirrettyjen (gigatavuina) tietojen määrän päivittäinen raportin kattaman.

## <a name="by-file-type"></a>Tiedostotyypin mukaan

Tiedostotyypin mukaan-raportin avulla voit tarkastella demand ja kertyneet tiedostotyypin mukaan liikenteen määrää. Rengas-kaavio osoittaa yhteydessä luodaan tällaista raportin, Ylin 10 tiedostotyypit luomien osumien prosenttiosuus.

> [AZURE.TIP] Jos asetat rengas-kaavion sektoria päälle, Internet-media tyyppi on tiedostotyyppi näytetään työkaluvihjeenä.

Tiedot, joita on käytetty rengas-kaavion luonnissa voi tarkastella sen alapuolella. Saat näkyviin tiedoston nimi tunniste tai Internet-mediatyyppi, osumien kokonaismäärä, osumien prosenttiosuus tietomäärän siirretään (gigatavuina), ja tietojen prosenttiosuus siirretään kunkin sivun 250 tiedostotyypit.

## <a name="by-directory"></a>Hakemistossa

Mukaan Directory raportin avulla voit tarkastella demand ja kertyneet päälle tietyn käyttötason sisällön tiettyyn hakemistosta liikenteen määrää. Palkkikaavio osoittaa yhteydessä luodaan tällaista raportin luoma Ylin 10 kansioiden sisällön osumien kokonaismäärä.

### <a name="using-the-bar-chart"></a>Palkkikaavion avulla

* Voit tarkastella suhteellinen polku vastaavan hakemiston palkin päälle.
* Kansion alikansio tallennetun sisällön Laske laskettaessa demand hakemistossa. Laskenta luottaa pyyntöjen luotu tallennetun todelliset kansion sisällön määrä.
* Tämän raportin tarkoitetaan reunan CNAME URL-osoitteet muunnetaan niiden vastaavat CDN URL-osoitteet. Näin tarkkoja yhdenmukaisuuden, kaikki tilastoja liittyvät omaisuuden riippumatta CDN tai reunan CNAME URL-Osoitteen avulla voidaan pyytää sitä varten.

Kaavio (y) vasemmalla puolella osoittaa tarjouspyyntöjen top 10-kansioissa tallennetun sisällön määrä. Kaavion kukin palkki edustaa hakemisto. Koodien mallin avulla voit verrata palkki ensimmäiset 250 koko hakemistoja-osassa hakemistoon.

Tiedot, joita on käytetty luomiseen palkkikaavion voi tarkastella sen alapuolella. Löydät kunkin sivun 250 kansioiden seuraavat tiedot: suhteellinen polku, osumaa, osumien prosenttiosuus, tietomäärän kokonaismäärän siirretään (gigatavuina), ja tietojen prosenttiosuus siirretään.

## <a name="by-browser"></a>Selain

Selaimen mukaan-raportin avulla voit tarkastella mitä selaimia käytettiin pyytää sisältöä. Ympyräkaavion osoittaa yhteydessä luodaan tällaista raportin, Ylin 10-selaimet käsittelemien pyyntöjen prosenttiosuus.

### <a name="using-the-pie-chart"></a>Käyttämällä Ympyräkaavio

* Voit tarkastella selaimen nimi ja versio ympyräkaavion sektoria hiiren osoitinta.
* Tämä raportti yksilöllinen selainversion yhdistelmien pidetään eri selainta.
* Nimeltä "Muut" sektoria ilmoitetaan muut selaimet ja versiot käsittelemien pyyntöjen prosentti.

Tiedot, joita on käytetty Luo ympyräkaavion voidaan tarkastella sen alapuolella. Olemassa Etsi selaimen tyyppi-/ versionumero, osumien kokonaismäärä ja osumien prosenttiosuus kunkin sivun 250 selaimet.

## <a name="by-referrer"></a>Referrer mukaan

Mukaan Referrer raportin avulla voit tarkastella viittaajat sisällön valitun ympäristössä. Referrer osoittaa isäntänimi, josta pyyntö on luotu. Yhteydessä luodaan tällaista raportin palkkikaavio ilmoittaa vaatimus (eli osumia) 10 viittaajat luoma määrää.

Vasemmalla puolella kaavio (y) ilmaisee, että sijoituksen listan varten kunkin referrer pyyntöjen kokonaismäärä. Kaavion kukin palkki edustaa referrer. Koodien mallin avulla voit verrata palkki referrer, ylhäältä 250 Referrer-osassa.

Tiedot, joita on käytetty luomiseen palkkikaavion voi tarkastella sen alapuolella. Löydät URL-osoite, osumien kokonaismäärä ja luoda kaikkien 250 viittaajat osumien prosenttiosuus.

## <a name="by-download"></a>Lataamalla

Valitsemalla Lataa-raportin avulla voit analysoida Lataa kuviot eniten pyydetty sisällölle. Raportin yläreunassa sisältää vertaa yritti lataukset valmiit lataamisen 10 pyydetty varat ja palkkikaavio. Kukin palkki on värikoodattu, onko yritettiin lataaminen (sininen) tai valmiin Lataa (vihreä) mukaan.

> [AZURE.NOTE] Tämän raportin tarkoitetaan reunan CNAME URL-osoitteet muunnetaan niiden vastaavat CDN URL-osoitteet. Näin tarkkoja yhdenmukaisuuden, kaikki tilastotietoja liittyvän omaisuuden riippumatta CDN tai reunan CNAME URL-Osoitteen avulla voidaan pyytää sitä.

Kaavio (y) vasemmalla puolella näkyy kunkin 10 pyydetty kohdalla tiedostonimi. Kaavion (x), alla olevassa etsii otsikot, jotka ilmaisevat yritetään suorittaa loppuun lataukset kokonaismäärän.

Suoraan alla palkkikaaviota, seuraavat tiedot näkyvät 250 pyydetty resurssien osalta: suhteellinen polku (mukaan lukien tiedostonimi), kuinka monta kertaa, se on ladattu työn kesto päivinä, kuinka monta kertaa, se on pyydetty ja pyynnöt, jotka ovat valmiina lataaminen prosentteina.

> [AZURE.TIP] Tutustu CDN ei saa ilmoituksen HTTP-asiakasohjelman (eli selain) kun sijoituksen ladattu kokonaan. Tuloksena on laskemiseen onko sijoituksen on ladattu kokonaan tilakoodin ja DBCS-alueen mukaan pyynnöt. Odotamme, kun teet tämän laskutoimituksen ensimmäinen vaihe on, onko pyynnön johtaa 200 OK tilakoodi. Jos näin on, että Katso DBCS-alueen pyynnöt varmistaaksesi, että ne kattavat koko resurssi. Lopuksi on Vertaile pyydetty kohteen koon siirretään tietojen määrää. Jos DBCS-alueen pyynnöt soveltuvat kyseinen resurssi siirretyt tiedot on yhtä suuri kuin tai suurempi kuin tiedostokokoa, osumien lasketaan valmis latauksena.
>
>Tämän raportin interpretive laatu vuoksi pidä mielessä seuraavat seikat, jotka voivat muuttaa yhdenmukaisuutta ja raportti tarkkuudella.
>
>* Liikenne kuvioita ei voida ennustaa tarkasti, kun käyttäjäagentit toimivat eri tavalla. Tämä voi tuottaa valmiit Lataa tuloksia, jotka ovat yli 100 %.
>* Resurssit, jotka hyödyntävät HTTP asteittain Lataa saattaa ei tarkasti edustaa raportti. Tämä on käyttäjien hakemista video eri paikkaan.

## <a name="by-404-errors"></a>404 virheistä

404 virheet-raportin avulla voit tunnistaa minkä tyyppistä sisältöä, joka luo 404 ei löydy tilakoodin eniten. Raportin yläreunassa sisältää top 10-resurssit, jonka 404 ei löydy tilakoodi palautettiin palkkikaavio. Tämä palkkikaavion vertaa pyyntöjen pyyntöjen, tuloksena 404 ei löydy tilakoodin näiden resurssien kokonaismäärä. Kukin palkki on värikoodattu. Osoittamassa, että pyyntö tuloksena 404 ei löydy tilakoodin käytetään keltaista palkkia. Punainen palkki käytetään osoittamaan kohteen tarjouspyyntöjen kokonaismäärän.

> [AZURE.NOTE] Tämän raportin yhdistämiskyselyssä Ota huomioon seuraavat:
>
>* Osuma edustaa riippumatta tilakoodin omaisuuden koskeviin pyyntöihin.
>* Reunan CNAME URL-osoitteet muunnetaan niiden vastaavat CDN URL-osoitteet. Näin tarkkoja yhdenmukaisuuden, kaikki tilastotietoja liittyvän omaisuuden riippumatta CDN tai reunan CNAME URL-Osoitteen avulla voidaan pyytää sitä.

Kaavio (y) vasemmalla puolella näkyy kunkin Ylin 10 pyydetty kohdalla, jotka ovat 404 ei löydy tilakoodin tiedostonimi. Kaavion (x), alla olevassa etsii otsikot, jotka ilmaisevat pyyntöjen kokonaismäärä ja tuloksena 404 ei löydy tilakoodin pyyntöjen määrä.

Suoraan alla palkkikaaviota, seuraavat tiedot näkyvät 250 pyydetty resurssien osalta: suhteellinen polku (mukaan lukien tiedostonimi), tuloksena 404 ei löydy tilakoodin pyyntöjen määrä, ajankohdat, jolloin kohteen pyydettiin kokonaismäärä ja pyynnöt, jotka ovat 404 ei löydy tilakoodin prosentteina.

## <a name="see-also"></a>Katso myös
* [Azure CDN yleiskatsaus](cdn-overview.md)
* [Reaaliaikainen tilasto-Microsoft Azure CDN](cdn-real-time-stats.md)
* [Säännöt-ohjelma käyttää HTTP-oletustoiminnan ohittaminen](cdn-rules-engine.md)
* [Reunan suorituskyvyn analysoiminen](cdn-edge-performance.md)
