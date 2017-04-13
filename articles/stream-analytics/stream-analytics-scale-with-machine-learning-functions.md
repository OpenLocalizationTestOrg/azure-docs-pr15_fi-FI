<properties
    pageTitle="Skaalaa Stream Analytics työtäsi Azure koneen Learning funktioiden | Microsoft Azure"
    description="Opettele skaalata oikein Stream Analytics töitä (jakaminen, SU määrä ja muut) kun Azure koneen Learning funktioiden avulla."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Skaalaa Stream Analytics työtäsi Azure koneen Learning-toiminnoilla

Usein on melko helppoa Stream Analytics-projektin määrittäminen ja suorita mallitietoja kulkee. Mikä on pitäisi tehdä, kun annettava suorittaa saman työn suurempi data-asema? Us Stream Analytics-projektin määrittäminen niin, että se skaalautuvat edellyttää. Tämän asiakirjan keskitytään määräten näkökohdat skaalaus Stream Analytics työt, joilla kone Learning funktiot. Saat lisätietoja skaalata Stream Analytics työt yleensä on artikkelissa [Skaalaus työt](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Mikä on Stream Analytics Azure koneen Learning-funktiossa?

Virta Analytics koneen Learning-funktiota voidaan käyttää kuten säännöllisesti funktiokutsun Stream Analytics-kyselykielen. Takana näkymän, funktiokutsut on kuitenkin todella Azure koneen Learning verkkopalvelun pyynnöt. Koneen Learning verkkopalvelut tukevat "jonottaminen" useita rivejä, joita kutsutaan pienen erä saman web service API puhelun aikana, voit parantaa yleinen. Seuraavissa artikkeleissa lisätietoja; [Azure koneen Learning Funktiot Stream Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) ja [Azure koneen Learning Web Services](machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Määritä Stream Analytics työn koneen Learning Funktiot

Koneen Learning funktion Stream Analytics työn määritettäessä on kaksi parametria ottaa huomioon, erän koko tietokoneen Learning funktiokutsut ja Streaming yksiköt (SUs) Stream Analytics työn valmistelun yhteydessä. Määrittämään tarvittavat arvot näiden ensin päätös on annettava välinen viive ja siirtonopeuden, eli Stream Analytics-projekti ja kunkin SU siirtonopeuden viive. SUs aina voidaan lisätä projektin niin, että siirtonopeuden hyvin osioitua Stream Analytics kyselyn, vaikka muut SUs suurenee tehdyn työn kustannukset.

Tämän vuoksi on tärkeää selvittää viive Stream Analytics suoritetaan *poikkeama* . Lisää viive toimimasta Azure koneen Learning palvelupyyntöjä luonnollisesti suurenee erän kokoa, joka yhdistelmä Stream Analytics-työ viive. Toisaalta, erän koon suurentaminen sallii käsittelemään Stream Analytics-työ *tapahtumien kanssa *sama määrä *, tietokoneen Learning ‑palveluun WWW. Koneen Learning web palvelun viive kasvu on usein osa lineaarinen erän koko kasvaa, joten on tärkeä ottaa huomioon kustannukset-tehokkaasti erän koko, tietokoneen Learning WWW-palvelun minkä tahansa määritetyn tilanteessa. Web-palvelupyyntöjä erän oletuskoko 1 000, ja niitä voidaan muuttaa joko käyttämällä [Stream Analytics REST API](https://msdn.microsoft.com/library/mt653706.aspx "Stream Analytics REST API") tai [PowerShell asiakkaan Stream Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md "PowerShell-asiakasohjelman Stream analysoinnissa").

Kun erän koko on määritetty, Streaming yksikköjen (SUs) summa, voidaan määrittää-funktio on tapahtumien määrä perusteella Käsittele sekunnissa. Lisätietoja Streaming yksiköt kuulla on artikkelissa [Stream Analytics asteikko työt](stream-analytics-scale-jobs.md#configuring-streaming-units).

Yleensä on 20 samanaikaisia yhteyksiä jokaisen 6 SUs koneen Learning web-palvelu, sillä erotuksella, että 1 SU työt ja 3 SU töiden saavat 20 samanaikaisia yhteyksiä myös.  Jos syöttötiedot korko on 200,000 tapahtumat sekunnissa ja erän koko on vasemmalta oletusarvo on 1 000 tuloksena web-palvelun viive 1000 tapahtumien pienen erän on 200 MS. Tämä tarkoittaa jokaista yhteyttä tehdä 5 pyynnöt koneen Learning WWW-palvelun toisessa. 20 yhteyksiä Stream Analytics-työ voit käsitellä 200 MS 20 000 tapahtumat ja näin ollen 100 000 tapahtumia toisessa. Niin käsittelemään 200,000 tapahtumat sekunnissa Stream Analytics-työ on 40 samanaikaisia yhteyksiä, joka sisältää 12 SUS. Seuraavassa kuvassa on kuvattu pyynnöt Stream Analytics-työstä koneen Learning web palvelun päätepisteelle – 6 jokaisen SUs on 20 samanaikainen yhteydet koneen Learning WWW-palvelun max-palvelussa.

![Skaalaa Stream Analytics ja tietokoneen Learning Funktiot 2 työn Esimerkki] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Skaalaa Stream Analytics ja tietokoneen Learning Funktiot 2 työn Esimerkki")

Yleensä erän koko, ***L*** web palvelun viive millisekunteina koossa erä B ***B*** Stream Analytics-työn ***N*** SUs kanssa on:

![Skaalaa Stream analyysin ja Konepohjaisten Oppimistekniikoiden Funktiot-kaava] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Skaalaa Stream analyysin ja Konepohjaisten Oppimistekniikoiden Funktiot-kaava")

Muita huomioon voi olla 'samanaikainen max puhelut' koneen Learning web service-puolella, on suositeltavaa asetuksena suurimman arvon (tällä hetkellä 200).

Lisätietoja tämän asetuksen please Tarkista [tietokoneen Learning verkkopalvelut skaalaus-artikkelissa](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Esimerkki – Markkinatunnelma analyysi

Seuraavassa esimerkissä on Stream Analytics työn markkinatunnelma-analyysin koneen Learning-funktio, kuvatulla tavalla [Stream Analytics koneen Learning integrointi opetusohjelma](stream-analytics-machine-learning-integration-tutorial.md).

Kysely on yksinkertainen täysin osioitua kysely perään **markkinatunnelma** -funktio, alla kuvatulla tavalla:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Ota huomioon seuraavat skenaarion; ja siirtonopeuden on 10 000 tweets sekunnissa Stream Analytics-työ on luotava markkinatunnelma analysoinnissa, tweets (tapahtumien). Käytä 1 SU, voi Stream Analytics työn voi käsitellään liikenteen? Erän oletuskoko 1000 käyttämällä projektin pitäisi seurata syöte. Edelleen lisätty tietokoneen Learning-funktio tulee Luo enintään sekunnin viive, joka on yleinen oletusarvon viive markkinatunnelma analyysin koneen Learning WWW-palvelun (jossa 1000 erä oletuskoon). Virta Analytics-työ **Yleistä** tai lopusta loppuun viive yleensä olisi muutaman sekunnin. Katso yksityiskohtaisempia kyselyjä Stream Analytics työn, *erityisesti* Konepohjaisten Oppimistekniikoiden funktiokutsut. Erän koko on kuin 1 000, siirtonopeuden 10 000 tapahtumien vievät noin 10 pyynnöt WWW-palvelun. Silloinkin 1 SU on tarpeeksi tämän syötteen liikenne samanaikaisia yhteyksiä.

Mutta Entäpä jos syötteen tapahtuman korko suurentaa 100 x ja nyt Stream Analytics-työ on käyttänyt 1,000,000 tweets sekunnissa? On kaksi vaihtoehtoa:

1.  Siirtoerän, kokoa tai
2.  Osion syötteen virta käsittelemään rinnakkain tapahtumat

Ensimmäinen vaihtoehto kasvattaa työn **viive** .

Toinen vaihtoehto ja lisää SUs on valmisteltava ja luoda näin ollen Lisää samanaikainen koneen Learning web palvelupyyntöjä. Tämä tarkoittaa projektin **kustannukset** kasvaa.


Oletetaan markkinatunnelma analyysin koneen Learning WWW-palvelun viive on 200 MS 1000 tapahtuman erissä tai alla 5 000 tapahtuman erissä, 300ms 10 000 tapahtuman erissä tai 25 000 tapahtuman erissä 500ms 250ms.

1. Ensimmäinen vaihtoehto, (**ei** Lisää SUs valmistelu) erän koko voi olla kasvoi **25 000**. Tämä puolestaan mahdollistaa työ korvattavan 1,000,000 tapahtumien 20 samanaikaisia yhteyksiä koneen Learning web-palveluun (viive 500ms puhelun kohden). Niin vuoksi markkinatunnelma-funktion pyynnöt koneen Learning web palvelupyyntöjä Stream Analytics-työ muita viive on suurennettu **200 MS** **500ms**. Huomaa, että erän kokoa **et voi** kuitenkin olla parempi infinitely Machine Learning verkkopalvelut vaatii tietojen kooksi pyynnön on 4 Megatavua tai pienempi verkkopalvelun pyytää aikakatkaisu toiminnon 100 sekunnin kuluttua.
2. Toinen vaihtoehto erän koko jätetään kanssa 200 MS WWW-palvelun viive 1 000, joka 20 samanaikaisia yhteyksiä web-palveluun voi käsitellä 1000 *20* 5 tapahtumia = 100,000 sekunnissa. Niin käsittelemään 1,000,000 tapahtumat sekunnissa työ on 60 SUs. Verrattuna ensimmäinen vaihtoehto, Stream Analytics työn tehdä lisää erä ‑palveluun WWW-luodaan puolestaan parantavat kustannukset.

Alla on Stream Analytics-työ siirtonopeuden taulukon eri SUs ja erä koot (sekunnissa tapahtumien määrä).

| erän koko (ML viive)  | 500 (200 MS) | 1 000 (200 MS) | 5 000 (250ms) | 10 000 (300ms) | 25 000 (500ms) |
|--------|-------------------------|---------------|---------------|----------------|----------------|
| **1 SU** | 2 500 | 5 000 | 20 000 | 30 000 | 50 000 |
| **3 SUs** | 2 500 | 5 000 | 20 000 | 30 000 | 50 000 |
| **6 SUs** | 2 500 | 5 000 | 20 000 | 30 000 | 50 000 |
| **12 SUs** | 5 000 | 10 000 | 40 000 | 60,000 | 100,000 |
| **18 SUs** | 7,500 | 15 000 | 60,000 | 90,000 | 150,000 |
| **24 SUs** | 10 000 | 20 000 | 80,000 | 120 000 | 200 000 |
| **…** | … | … | … | … | … |
| **60 SUs** | 25 000 | 50 000 | 200 000 | 300 000 | 500 000 |

Nyt sinulla pitäisi olla jo ymmärtää koneen Learning Funktiot Stream Analytics toiminnasta. Todennäköisesti myös ymmärrät Stream Analytics työt "erotettu" tietolähteiden ja kunkin "erotettu" palauttaa tapahtumien Stream Analytics työn käsittelemään erän. Miten tämä salaus puretaan mallin vaikutus koneen Learning web palvelupyyntöjä?

Tavallisesti on määritetty Machine Learning toimintojen erän koko täsmälleen ei ole jaollinen Stream Analytics kunkin projektin "erotettu" palauttama tapahtumien määrä. Kun tämä tapahtuu tietokoneen Learning WWW-palvelun kutsuttava "osittaisen" erissä. Tämä tapahtuu maksamaan ei lisää työn viive katseltavan coalescing tapahtumia salaus puretaan erotettu.

## <a name="new-function-related-monitoring-metrics"></a>Uusi funktio liittyvät seurantaa arvot

Virta Analytics työn näyttö-alueella on lisätty kolme Lisää funktio liittyvät arvot. He ovat FUNKTION PYYNNÖT, FUNKTIO tapahtumien ja epäonnistui FUNKTION PYYNNÖT alla olevassa kuvassa esitetyllä tavalla.

![Skaalaa Stream analyysin ja Konepohjaisten Oppimistekniikoiden Funktiot arvot] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Skaalaa Stream analyysin ja Konepohjaisten Oppimistekniikoiden Funktiot arvot")

Oletko määritellään seuraavasti:

**FUNKTION PYYNNÖT**: funktion pyyntöjen määrä.

**FUNKTION tapahtumien**: funktion pyynnöt numero tapahtumat.

**FUNKTION PYYNNÖT epäonnistui**: epäonnistui funktion pyyntöjen määrä.

## <a name="key-takeaways"></a>Tärkeimmät Takeaways  

Yhteenveto pääkohdat, jos haluat skaalata Stream Analytics-työ koneen Learning toiminnoilla on otettava huomioon seuraavat seikat:

1.  Kirjoita tapahtuman korko
2.  Siedetty viive käynnissä Stream Analytics-työ (ja täten erän koko, tietokoneen Learning ‑palveluun WWW)
3.  Valmistellun Stream Analytics-SUs ja tietokoneen Learning web ‑palveluun (Lisää funktio liittyvät kustannukset) määrä

Täysin osioitua Stream Analytics kyselyn käytettiin esimerkkinä. Monimutkaisen kyselyn tarvittaessa [Azure Stream Analytics-keskustelupalstalla](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) on erinomainen tehostavia lisäohjeita Stream Analytics-ryhmältä.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Stream Analytics on seuraavissa artikkeleissa:

- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
