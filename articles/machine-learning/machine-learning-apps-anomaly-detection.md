<properties 
    pageTitle="Koneen Learning app: poikkeavuuksista tunnistus palvelun | Microsoft Azure" 
    description="Poikkeavuuksista tunnistus Ohjelmointirajapinnan on luotu Microsoft Azure koneen Learning, joka tunnistaa poikkeamia sarjan aikatietoja numeeriset arvot, jotka ovat yhdenmukaisesti peräkkäin samanaikaisesti." 
    services="machine-learning" 
    documentationCenter="" 
    authors="alokkirpal" 
    manager="jhubbard"
    editor="cgronlun" /> 

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="multiple" 
    ms.date="10/11/2016" 
    ms.author="alokkirpal"/>


# <a name="machine-learning-anomaly-detection-service"></a>Koneen Learning poikkeavuuksista tunnistus-palvelu#

##<a name="overview"></a>Yleiskatsaus

[Poikkeavuuksista tunnistus API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) on luotu Azure koneen Learning, joka tunnistaa poikkeamia sarjan aikatietoja numeeriset arvot, jotka ovat yhdenmukaisesti peräkkäin samanaikaisesti. 

Tämä API tunnistaa erheellisiin kuviot seuraavanlaisia aikatietoja sarja:

* **Positiivisia ja negatiivisia trendejä**: esimerkiksi kun seuranta muistinkäytön tietojenkäsittely ylöspäin trendi saattaa olla merkitystä mahdollisesti indikatiivisten, muistivuotoa,

* **Dynaamisen alueen arvojen muutokset**: esimerkiksi kun seuranta poikkeukset pilvipalveluun, dynaamisen alueen arvojen muutokset saattaa johtua kiintolevyvirheet palvelun kunto ja

* **Piikkejä ja liuoksia**: esimerkiksi kun seuranta-palveluun kirjautuminen epäonnistuneiden yritysten määrä tai hylkääminen numero e-commerce-sivustossa, piikkarit tai tapahtuneen saattaa johtua epätavallisia toiminta.

Nämä koneen learning ilmaisimet seurata näiden arvojen muutokset aika- ja niiden arvojen jatkuvaa muutokset poikkeavuuksista tuloksina. Ne eivät edellytä tietokoneiden välinen kynnysarvo säätäminen ja niiden tulosten avulla voidaan ohjata EPÄTOSI positiivinen maksu. API on kätevä vaihtoehto eri tavoin jäljitys suorituskykyilmaisimia ajan myötä valvoo palvelun poikkeavuuksista tunnistus käyttö seuranta – arvot, kuten hakujen määrä, numerot napsautuksella, suorituskyvyn seurantaa kautta laskureita muistin suorittimen, kuten tiedoston lukee jne ajan kuluessa.

Poikkeavuuksista tunnistus tarjoaa sisältää hyödyllisiä työkaluja, joiden avulla pääset alkuun. 

* [Web-sovelluksen](http://anomalydetection-aml.azurewebsites.net/) avulla voit arvioida ja esittää tiedot tunnistaminen poikkeavuuksista API tulokset. 

* [Esimerkki koodi](http://adresultparser.codeplex.com/) näkyy ohjelmallisesti Ohjelmointirajapinnan käyttäminen ja jäsentää C# tulokset.

>[AZURE.NOTE]
>Kokeile [Tämän API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) tarjoaa **IT poikkeavuuksista havainnollistamisen ratkaisu**
>
>Saat tämän lopusta loppuun-ratkaisun käyttöön Azure tilauksen <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **, Aloita tästä >**</a>


##<a name="api-definition"></a>API määritys

Palvelun tarjoaa REST-pohjainen API päälle, joka on käytetty eri tavalla, mukaan lukien verkossa tai R, Python, Excel-mobiilisovelluksen HTTPS jne.  Sarjan aikatietoja lähettäminen tämän palvelun kautta REST API-kutsu, ja se suoritetaan yhdistelmän kolme poikkeavuuksista kuvattuun tyyppiin. Palvelun suorittaa Azure koneen Learning-ympäristössä, jossa Skaalaa yrityksesi tarpeisiin saumattomasti sekä annetaan palvelutasosopimuksia.

###<a name="headers"></a>Otsikot
Varmista oikea otsikot sisällyttäminen pyyntö, jonka pitäisi näyttää seuraavalta:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Löydät tiliavain [Azure Data Market](https://datamarket.azure.com/account/keys)-tililtä. 

###<a name="score-api"></a>Tulos Ohjelmointirajapinta

Tulos Ohjelmointirajapinnan käytetään tunnistamisen poikkeavuuksista-kausiluonteisista aikatietoja sarja. Ohjelmointirajapinnan suoritetaan poikkeavuuksista ilmaisimet useita tiedot ja palauttaa niiden poikkeavuuksista tulosten. Alla olevassa kuvassa on esimerkki poikkeamia, pisteet Ohjelmointirajapinnan tunnistaa. Tämä aikasarjalle on 2 eri tason muutokset ja 3 piikkarit. Punaiset pisteet Näyttää ajan, jossa tason muutos havaitaan, kun taas mustista pisteistä näytetään olemassa piikkarit.

![Tulos Ohjelmointirajapinta][1]
    
**URL-OSOITE**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/Score 

**Esimerkki pyynnön tekstissä**

Alla pyynnön lähetämme aikasarjan (kuten katkaistut) arvopisteiden 3/1/2016 – 3/10/2016 ja piikin ilmaisimet parametrit ohjelmointirajapinnan tunnistamiseen, jos jokin näistä arvopisteitä on erheellisiin. 

    {
      "data": [
        [ "9/21/2014 11:05:00 AM", "3" ],
        [ "9/21/2014 11:10:00 AM", "9.09" ],
        [ "9/21/2014 11:15:00 AM", "0" ]
      ],
      "params": {
        "tspikedetector.sensitivity": "3",
        "zspikedetector.sensitivity": "3",
        "trenddetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "postprocess.tailRows": "2"
      }
    }

Tämä vastaus on: 

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
      "ADOutput":"{
        "ColumnNames":["Time","Data","TSpike","ZSpike","rpscore","rpalert","tscore","talert"],
        "ColumnTypes":["DateTime","Double","Double","Double","Double","Int32","Double","Int32"],
        "Values":[
          ["9/21/2014 11:10:00 AM","9.09","0","0","-1.07030497733224","0","-0.884548154298423","0"],
          ["9/21/2014 11:15:00 AM","0","0","0","-1.05186237440962","0","-1.173800281031","0"]
        ]
      }"
    }

###<a name="scorewithseasonality-api"></a>ScoreWithSeasonality API

ScoreWithSeasonality Ohjelmointirajapinnan käytetään tunnistamisen poikkeavuuksista aikasarjalle, joissa on kausiluonteisista kuviot. Tämä API on hyötyä esiintyvien poikkeamien kausiluonteisista kuviot.  

Seuraavassa kuvassa on esimerkki poikkeamia havaita kausiluonteisista aikasarjalle. Aikasarjan on yksi piikin (1st musta piste), kaksi tapahtuneen (2. musta piste ja toinen lopussa) ja yhden tason muutoksen (punainen piste). Huomaa, että molemmat dip keskellä aikasarjan ja tason muutos ovat vain discernable sarjasta kausiluonteisista osien poistamisen jälkeen.

![Kausivaihtelu API][2]

**URL-OSOITE**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/ScoreWithSeasonality 

**Esimerkki pyynnön tekstissä**

Alla olevassa pyynnön lähetämme aikasarjan (kuten katkaistut) arvopisteiden 3/1/2016 – 3/10/2016 ja parametrit ohjelmointirajapinnan tunnistamiseen, jos jokin näistä arvopisteitä on erheellisiin. 

    {
      "data": [
        [ "9/21/2014 11:10:00 AM", "9" ],
        [ "9/21/2014 11:15:00 AM", "12" ],
        [ "9/21/2014 11:20:00 AM", "10" ]
      ],
      "params": {
        "preprocess.aggregationInterval": "0",
        "preprocess.aggregationFunc": "mean",
        "preprocess.replaceMissing": "lkv",
        "postprocess.tailRows": "1",
        "zspikedetector.sensitivity": "3",
        "tspikedetector.sensitivity": "3",
        "upleveldetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "trenddetector.sensitivity": "3.25",
        "detectors.historywindow": "500",
        "seasonality.enable": "true",
        "seasonality.transform": "deseason",
        "seasonality.numSeasonality": "1"
      }
    }

Tämä vastaus on: 

    {
        "odata.metadata": "https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
        "ADOutput": "{
            "ColumnNames":["Time","OriginalData","ProcessedData","TSpike","ZSpike","PScore","PAlert","RPScore","RPAlert","TScore","TAlert"],
            "ColumnTypes":["DateTime","Double","Double","Double","Double","Double","Int32","Double","Int32","Double","Int32"],
            "Values":[
                ["9/21/201411: 20: 00AM","10","10","0","0","-1.30229513613974","0","-1.30229513613974","0","-1.173800281031","0"]
            ]
        }"
    }

###<a name="detectors"></a>Ilmaisimet

Poikkeavuuksista tunnistus API tukee ilmaisimet 3 luokkaan. Tietyn syöteparametrit ja kunkin ilmaisimen tulostaa tiedot löytyvät seuraavassa taulukossa.

|Ilmaisimen luokka|Ilmaisimen|Kuvaus|Syöteparametrit|Tulostus
|---|---|---|---|---|
|Piikin ilmaisimet|TSpike tunnistus|Tunnista piikkarit ja tapahtuneen kaukana arvojen perusteella ovat peräisin ensimmäisen ja kolmannen quartiles|*tspikedetector.sensitivity:* on kokonaisluku väliltä 1 – 10 oletusarvo: 3; Suurten arvojen sieppaa näin henkilöistä vähemmän luottamuksellisia erittäin arvoja|TSpike: binaarinen arvot – '1' Jos piikin/dip havaitaan, "0" muuten|
||ZSpike tunnistus|Tunnistaa piikkarit ja tapahtuneen perusteella, kuinka kauas arvopisteet on keskiarvon.|*zspikedetector.sensitivity:* kestää kokonaisluku väliltä 1 – 10 oletusarvo: 3; Suurten arvojen sieppaa tekeminen vähemmän luottamuksellisia erittäin arvoja|ZSpike: binaarinen arvot – '1' Jos piikin/dip havaitaan, "0" muuten|
|Hidas trendi tunnistus|Hidas trendi tunnistus|Tunnista hidas positiivinen trendi määrittäminen luottamuksellisuus mukaan|*trenddetector.sensitivity:* raja-ilmaisimen tulos (oletus: 3,25, 3,25 – 5 on suositeltavaa kerralla solualue, jos haluat valita tämän; Mitä suurempi pienempi merkitsevä)|TScore: kelluvat numero edustava poikkeavuuksista tulos-trendi|
|Tason muutos ilmaisimet|Yksisuuntainen tason muutos tunnistus|Tunnista ylöspäin tason mukaan määrittäminen luottamuksellisuus muuttaminen|*upleveldetector.sensitivity:* raja-ilmaisimen tulos (oletus: 3,25, 3,25 – 5 on suositeltavaa kerralla solualue, jos haluat valita tämän; Mitä suurempi pienempi merkitsevä)|PScore: kelluvat edustava poikkeavuuksista tulos ylöspäin luku tason muuttaminen|
||Kaksisuuntainen tason muuttaminen tunnistus|Tunnista määrittäminen luottamuksellisuus mukaan sekä ylöspäin ja alaspäin tason muuttaminen|*bileveldetector.sensitivity:* raja-ilmaisimen tulos (oletus: 3,25, 3,25 – 5 on suositeltavaa kerralla solualue, jos haluat valita tämän; Mitä suurempi pienempi merkitsevä)|RPScore: kelluvat luku kuvaavia poikkeavuuksista tulos-ylöspäin ja alaspäin taso muuttaminen

###<a name="parameters"></a>Parametrit

Alla olevassa taulukossa näkyy nämä syöteparametrit tarkempia tietoja:

|Syöteparametrit|Kuvaus|Oletusasetusten määrittäminen|Tyyppi|Kelvollinen arvoalue|Ehdotettu alue|
|---|---|---|---|---|---|
|preprocess.aggregationInterval|Kooste väli (sekunteina) yhdistäminen syötteen aikasarjalle|0 (ei yhdistämistä ei suoriteta)|kokonaisluku|0: Ohita kooste-> 0 muuten|5 minuuttia 1 päivä-aikasarjalle riippuvainen
|preprocess.aggregationFunc|Funktio käyttää tietojen yhdistäminen yhdeksi määritetyn AggregationInterval|keskiarvo|luetteloitu|keskiarvo-summa-, pituus|PUUTTUU|
|preprocess.replaceMissing|Puuttuvien tietojen hankintapäivän sijaan käytettävät arvot|lkv (tunnetaan viimeisen arvon)|luetteloitu|nolla, lkv, keskiarvon.|PUUTTUU|
|detectors.historyWindow|Historia-(# arvopisteiden) käytetään poikkeavuuksista pistemäärän laskenta|500|kokonaisluku|10 – 2000|Aikasarjan riippuvainen|
|upleveldetector.sensitivity|Ylöspäin osoittava tason luottamuksellisuustason muuttaminen tunnistus. |3,25|Kaksinkertainen|Ei mitään|3,25 5(Lesser values mean more sensitive)|
|bileveldetector.sensitivity|Kaksisuuntainen tason luottamuksellisuustason muuttaminen tunnistus. |3,25|Kaksinkertainen|Ei mitään|3,25 5(Lesser values mean more sensitive)|
|trenddetector.sensitivity|Positiivinen trendi tunnistus luottamuksellisuustason. |3,25|Kaksinkertainen|Ei mitään|3,25 5(Lesser values mean more sensitive)|
|tspikedetector.sensitivity |Luottamuksellisuustason TSpike tunnistus|3|kokonaisluku|1 – 10|3-5(Lesser values mean more sensitive)|
|zspikedetector.sensitivity |Luottamuksellisuustason ZSpike tunnistus|3|kokonaisluku|1 – 10 |3-5(Lesser values mean more sensitive)|
|seasonality.Enable|Onko kausivaihtelu analyysi on suoritettava|TOSI|Totuusarvo|arvo on TOSI, EPÄTOSI|Aikasarjan riippuvainen|
|seasonality.numSeasonality |Enimmäismäärä säännöllisiä jaksot tunnistaminen|1|kokonaisluku|1, 2|1-2|
|seasonality.Transform |Onko kausiluonteisista (ja) trendi osat on poistettava ennen käyttöä poikkeavuuksista tunnistus|deseason|luetteloitu|ei mitään, deseason, deseasontrend|PUUTTUU|
|postprocess.tailRows |Uusimmat tiedot voidaan säilyttää tulos tulokset arvopisteiden määrä|0|kokonaisluku|0 (pitää kaikkiin arvopisteisiin), tai määrittää tulosten huomioitavat pistemäärän verran|PUUTTUU|

###<a name="output"></a>Tulos
Ohjelmointirajapinnan suoritetaan kaikki ilmaisimet sarjan aikatietoja ja palauttaa poikkeavuuksista tulosten ja binaarinen piikin ilmaisimet kullekin aika. Seuraavassa taulukossa on lueteltu tulostaa olevasta Ohjelmointirajapinnan. 

|Tulostus|Kuvaus|
|---|---|
|Aika|Tietoja tai koostetun (ja/tai) selkeämmin tietojen aikaleimat Jos Kooste (ja/tai) puuttuu tietoja huomioon ottaminen käyttöön|
|OriginalData|Tietoja tai koostetun (ja/tai) selkeämmin tietojen arvoja, jos Kooste (ja/tai) puuttuu tietoja huomioon ottaminen käyttöön|
|ProcessedData|Jompikumpi seuraavista toimista: <ul><li>Jos merkitsevän kausivaihtelu havaittu ja deseason on valittuna; kausitasoitettuina aikasarjalle</li><li>kausittain säätää ja Trenditön aikasarjalle Jos merkittäviä kausivaihtelu havaitsi ja deseasontrend on valittuna</li><li>Muussa tapauksessa tämä on sama kuin OriginalData</li>|
|TSpike|Binaarinen ilmaisin osoittaa, onko Piikkiin havaitaan TSpike tunnistus|
|ZSpike|Binaarinen ilmaisin osoittaa, onko Piikkiin havaitaan ZSpike tunnistus|
|Pscore|Irrallinen poikkeavuuksista pistemäärän ylöspäin tasolla edustava luku muuttaminen|
|Palert|1 tai 0-arvon, joka ilmaisee ylöspäin taso ei muuta poikkeavuuksista syötteen luottamuksellisuus perusteella|
|RPScore|Irrallinen luku kuvaavia poikkeavuuksista sija kaksisuuntainen tason muutosten|
|RPAlert|1 tai 0-arvon, joka ilmaisee kaksisuuntainen taso ei muuta poikkeavuuksista syötteen luottamuksellisuus perusteella|
|TScore|Irrallinen luku kuvaavia poikkeavuuksista sija-positiivinen trendi|
|TAlert|Ilmaisee, että siellä 1 tai 0-arvo on positiivinen trendi-poikkeavuuksista syötteen luottamuksellisuus perusteella|


Tämä tulos voi jäsentää [Yksinkertainen jäsentimen](https://adresultparser.codeplex.com/) avulla – se on esimerkki koodi, joka muodostaa Ohjelmointirajapinnan ja jäsentää tulos. Havaittu poikkeamia voidaan näyttää koontinäyttöön ja/tai välitetään korjaavat toimenpiteet ihmisten asiantuntijoiden tai integroitu liput systems.


[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-seasonal.png

 

 
