<properties 
   pageTitle="Lokitiedoston Analytics ilmoitukset | Microsoft Azure"
   description="Log Analytics ilmoitukset tunnistaa OMS säilössä tärkeitä tietoja ja voit itse ilmoittaa ongelmista tai kutsua toiminnot yrittää korjata ne.  Tässä artikkelissa käsitellään luoda ilmoituksen sääntö ja tiedot ne voivat käyttää erilaisia toimintoja."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="bwren" />

# <a name="alerts-in-log-analytics"></a>Lokitiedoston Analytics ilmoitukset

Lokitiedoston Analytics ilmoitukset tunnistaa OMS säilössä tärkeitä tietoja.  Ilmoitusten säännöt suorittaa log hakuja aikataulun mukaan ja luoda ilmoituksen tietueeseen, jos tuloksia tietyn ehtoja vastaavat automaattisesti.  Säännön automaattisesti suorittaa yhden tai useamman toiminnot, kun muutoksista ilmoittaa sinulle ilmoituksen tai kutsua toisen prosessin käytössä.   

![Kirjaudu Analytics-ilmoitukset](media/log-analytics-alerts/overview.png)

## <a name="creating-an-alert-rule"></a>Ilmoitusten säännön luominen
Jos haluat luoda ilmoituksen sääntö, aloita luomalla tietueet, jotka tulisi käynnistää ilmoituksen lokitiedoston etsiminen.  **Ilmoitus** -painike on käytettävissä, jotta voit luoda ja määrittää hälytyksen.

1.  Valitse **Log Etsi**OMS yleiskatsaus-sivulla.
2.  Luo uusi loki hakukyselyn tai tallennettu loki haku. 
3.  Valitse **ilmoitusten** sivun yläreunassa Avaa **Ilmoitusten säännön lisääminen** -ruutu.
4. Lisätietoja on taulukoiden tiedot valitsemalla asetukset-ilmoituksen määrittäminen.
5. Kun olet kirjoittanut hälytyksen aika-ikkuna, aiemmin kyseisen aikaikkunan hakuehtoja vastaavat tietueet numero näytetään.  Tämän avulla voit määrittää, miten usein avulla voit odotat tulosten määrä.
4.  Valitse Viimeistele hälytyksen **Tallenna** .  Se käynnistyy heti käytössä.


![Ilmoitusten säännön lisääminen](media/log-analytics-alerts/add-alert-rule.png)

| Ominaisuus | Kuvaus |
|:--|:--|
| **Ilmoituksen tietoja** | |
| Nimi |  Hälytyksen yksilöllinen nimi. |
| Vakavuus | Ilmoitus, joka on luotu säännöllä vakavuus. |
| Hakukyselyn | Valitse **Käytä nykyistä hakukyselyn** nykyisen kyselyn tai valitse luettelosta tallennettua hakua.  Kyselysyntaksia annetaan tekstiruutu, jossa voit muokata sitä tarvittaessa.  |
| Aika-ikkuna | Määrittää kyselyn aikaväli.  Kysely palauttaa tietueet, jotka on luotu tällä hetkellä välillä.  Tämä voi olla mikä tahansa arvo 5 minuuttia-24 tuntia.  Se on oltava suurempi tai yhtä suuri ilmoitusten aikaväli.  <br><br> Esimerkiksi jos aika-ikkuna on määritetty 60 minuuttia ja kysely suoritetaan, 1:15 PM, vain ne tietueet, jotka on luotu 12:15 PM väliltä 1:15 PM palautetaan. |
| **Aikataulu** |     
| Raja-arvo | Ilmoituksen luominen koskevat ehdot.  Ilmoituksen luodaan, jos kysely palauttaa tietueiden määrän vastaa näitä ehtoja. |
| Ilmoitusten aikaväli | Määrittää, kuinka usein suorittaa kyselyn.  Voi olla mikä tahansa arvo 5 minuuttia-24 tuntia.  On oltava yhtä suuri kuin tai pienempi kuin aika-ikkuna. |
| Poista ilmoitukset käytöstä | Kun otat hälytyksen vaimennusta, säännön toiminnot on poistettu käytöstä määritetyn pituutta uusi ilmoitus luomisen jälkeen.  Sääntö on käynnissä ja luo ilmoitusten tietueet, jos ehto täyttyy.  Tämä on, jotta voit korjata ongelman suorittamatta kaksoiskappaleiden toiminnot aikaa. |
| **Toiminnot** | |
| Sähköposti-ilmoituksen | Määritä **Kyllä** , jos haluat, että sähköposti lähetetään, kun hälytyksen. |
| Aihe    | Aihe sähköpostien.  Et voi muokata viestin tekstiosaan. |
| Vastaanottajat | Sähköpostin vastaanottajien osoitteet.  Jos määrität asiakkaalle useita osoitteita, erota osoitteet toisistaan puolipisteellä (;). |
| Webhook | Määritä **Kyllä** , jos haluat soittaa webhook, kun hälytyksen. |
| Webhook URL-osoite | Webhook URL-osoite. |
| Sisällyttää mukautetut JSON-paketti | Valitse tämä asetus, jos haluat korvata mukautetun paketti oletusarvo-paketti. |
| Kirjoita mukautetun JSON-paketti | Webhook mukautetut tiedot.  Katso edellisessä osassa. |
| Runbookin | Määritä **Kyllä** , jos haluat aloittaa Azure automaatio-runbookin, kun hälytyksen. |
| Valitse runbookin | Valitse runbookin käynnistäminen runbooks määritetty automaatio-ratkaisussa automaatio-tilille. |
| Suorita | Valitse **Azure** suorittamiseen: n runbookin Azure pilveen.  Valitse **Hybrid työntekijä** voidaan suorittaa: n runbookin [Hybrid Runbookin työntekijä](..\automation\automation-hybrid-runbook-worker.md) paikallisen ympäristön. |


## <a name="manage-alert-rules"></a>Ilmoitusten sääntöjen hallinta
Saat kaikki ilmoituksen säännöt luettelo **ilmoitukset** -valikossa Log Analytics- **asetukset**.  

![Ilmoitusten hallinta](./media/log-analytics-alerts/configure.png)

1. Valitse OMS-konsolin **asetukset** -ruutu.
2. Valitse **ilmoitukset**.

Voit suorittaa useita toimintoja tästä näkymästä.

- Säännön käytöstä valitsemalla sen vieressä **käytöstä** .
- Muokkaa ilmoitusten sääntöä valitsemalla sen vieressä olevaa kynäkuvaketta.
- Poistaa ilmoituksen sääntöä valitsemalla sen vieressä olevaa **X** -kuvaketta. 


## <a name="setting-time-windows"></a>Windowsin määrittäminen 

### <a name="event-alerts"></a>Tapahtuma-ilmoitukset

Tapahtumat ovat tietolähteistä, kuten Windowsin tapahtumalokien, Syslog, ja mukautetut kirjautuu.  Haluat ehkä luoda ilmoituksen, kun tietyn error-tapahtuma luodaan tai kun useita virheitä tapahtumat luodaan suorittaa tietyn ajanjakson.

Ilmoitus yhden tapahtuman, määrittää tulosten määrä suurempi kuin 0 sekä korkojakso ja aikaikkunan 5 minuuttia.  Joka suorittaa kyselyä 5 minuutin välein ja tarkista yhden tapahtuman, joka on luotu kysely suoritettiin edellisen jälkeen.  Pidempi korkojakso viivästää kerätty tapahtuma- ja luomisen ilmoituksen välisen ajan.

Jotkin sovellukset voivat kirjata ajoittaiset virhe, joka ei välttämättä ole Nosta ilmoituksen.  Sovellus voi esimerkiksi prosessi, joka on luotu error-tapahtuma uudelleen ja onnistuu seuraavan kerran.  Sinulla ei ole tässä tapauksessa kannattaa luoda ilmoituksen, ellei useita tapahtumat luodaan suorittaa tietyn ajanjakson.  

Haluat ehkä luoda ilmoituksen tapahtuman puuttuessa.  Esimerkiksi prosessi voi kirjautua säännöllisesti tapahtumien osoittamassa, että se toimii oikein.  Jos se ei kirjaa suorittaa tietyn ajanjakson näitä tapahtumia, luodaanko ilmoituksen.  Määritä tässä tapauksessa kynnysarvo *pienempi kuin*1.

### <a name="performance-alerts"></a>Suorituskyvyn ilmoitukset

[Suorituskyvyn tiedot](log-analytics-data-sources-performance-counters.md) tallennetaan tietueiksi OMS säilöön, samalla tapahtumat.  Kunkin tietueen arvo on keskiarvon mitattu edellisen 30 minuuttia.  Jos haluat ilmoittaa, kun suorituskyvyn laskuri tietyn kynnysarvo, raja sisällytetään kyselyn.

Esimerkiksi, jos haluat määrittää ilmoittamaan suorittimen suoritettaessa yli 90 % 30 minuuttia, kuten kyselyn vakiomittoja *tyyppi = Perf nimi = suoritin CounterName = "% suoritinaika" CounterValue > 90* ja *suurempi kuin 0*ilmoitusten säännön raja-arvon.  

 Koska [suorituskyvyn tietueet](log-analytics-data-sources-performance-counters.md) yhdistetään 30 minuutin välein, riippumatta siitä, korkojakso kerää kunkin laskuri-pienempi kuin 30 minuutin ajan ikkunan voi palauttaa tietueita ei ole.  Aika-ikkuna asettaminen 30 minuuttia varmistat vastaavan tietueen hankkiminen kunkin yhdistetyn tietolähde, jota edustaa keskiarvon, jaettuina.

## <a name="alert-actions"></a>Ilmoitusten toiminnot

Lisäksi ilmoitusten tietueen luomisessa, voit määrittää ilmoitusten sääntö voidaan suorittaa seuraavia toimia automaattisesti.  Kun muutoksista toiminnot voit ilmoittaa sinulle ilmoituksen tai joitakin prosessi, jossa yrittää korjata ongelman, joka on havaittu ongelma.  Seuraavissa osioissa on kuvattu toimet, jotka ovat tällä hetkellä käytettävissä.

### <a name="email-actions"></a>Sähköpostin toiminnot
Sähköpostin toiminnot lähettää ilmoituksen sisältävän sähköpostiviestin yhdelle tai usealle vastaanottajalle.  Voit määrittää viestin aihe, mutta sen sisältö on vakiomuoto Log Analytics valmistama.  Se sisältää yhteenvetotietoja, kuten nimi ilmoituksen lisäksi loki haun palauttamat enintään kymmenen tietueiden tietoja.  Se on myös lokitiedoston, joka palauttaa koko tietuejoukon kyseisen kyselystä Analytics log haun linkki.   Viestin lähettäjä on *Microsoft toimintojen hallinnan Suite työryhmän &lt; noreply@oms.microsoft.com *. 


### <a name="webhook-actions"></a>Webhook toiminnot

Webhook toimintojen avulla voit käynnistää ulkoinen toiminto läpi yksi HTTP POST pyyntö.  Kutsutaan palvelun olisi tukevat webhooks ja määrittää, miten se käyttää mitä tahansa paketti se saa.  Voi myös soittaa REST-Ohjelmointirajapinta, erityisesti ei tue webhooks, kunhan pyyntö on muodossa, joka Ohjelmointirajapinnan ymmärtää.  Esimerkkejä käyttämisestä webhook ilmoituksen saatuaan ilmoituksen luominen tapahtuma-palvelun, kuten [PagerDuty](http://pagerduty.com/)tai tietoja sisältävän viestin lähettäminen palvelun kuten [liukuma](http://slack.com) avulla.  

Vaiheittainen ilmoitusten säännön luominen webhook soittaminen näyte-palvelun kanssa on saatavilla osoitteessa [Log Analytics-ilmoitukset Webhooks](log-analytics-alerts-webhooks.md).

Webhooks ovat URL-osoite ja paketti, joka on muotoiltu JSON ulkoiseen palveluun lähetetyt tiedot, jotka.  Oletusarvon mukaan paketti sisältää arvot seuraavasta taulukosta.  Voit korvata mukautetun jokin tämän paketin.  Tässä tapauksessa voit muuttujat taulukon kunkin parametrit sisällytettävien niiden arvo mukautetut tiedot.


| Parametri | Muuttujan | Kuvaus |
|:--|:--|:--|
| AlertRuleName | #alertrulename | Ilmoitusten säännön nimi. |
| AlertThresholdOperator | #thresholdoperator | Hälytyksen raja-operaattoria.  *Suurempi* tai *pienempi kuin*. |
| AlertThresholdValue | #thresholdvalue | Hälytyksen raja-arvo. |
| LinkToSearchResults | #linktosearchresults | Linkki Log Analytics log haun, joka palauttaa tietueet, jotka luoda ilmoituksen kyselystä. |
| ResultCount  | #searchresultcount | Hakutulosten tietueiden määrän. |
| SearchIntervalEndtimeUtc  | #searchintervalendtimeutc | Päättymisaika kyselyn UTC-muodossa. |
| SearchIntervalInSeconds | #searchinterval | Hälytyksen aikaikkunan. |
| SearchIntervalStartTimeUtc  | #searchintervalstarttimeutc | Kyselyn UTC-muodossa aloitusaika. |
| SearchQuery | #searchquery | Log-hakukyselyn hälytyksen käyttämä. |
| Hakutulokset | Noudata seuraavia ohjeita | JSON-muodossa kysely palauttaa tietueet.  Rajoitettu ensin 5 000 tietuetta. |
| WorkspaceID | #workspaceid | OMS työtilan tunnus. |


Voit määrittää esimerkiksi seuraavat mukautetun paketti, joka sisältää yksittäisen parametrin nimeltään *teksti*.  Palvelu, jota tässä webhook soittaa odotetaan parametriä.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Tässä esimerkissä tiedot ratkaista seuraavanlaiselta webhook lähetetään.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Sisällytettävät mukautetun sisällön hakutulokset Lisää seuraava rivi ylimmän tason ominaisuuden json-paketti.  

    "IncludeSearchResults":true

Esimerkiksi jos haluat luoda mukautetun paketti, joka sisältää vain ilmoituksen nimen ja etsinnän tulokset, voit käyttää seuraavia. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Voit käydä läpi täydellinen esimerkki ilmoituksen säännön luominen webhook Aloita ulkoiseen palveluun osoitteessa [Log Analytics Ilmoita webhook otoksen](log-analytics-alerts-webhooks.md)kanssa.

### <a name="runbook-actions"></a>Runbookin toiminnot

Runbookin toiminnot Aloita Azure automaatio runbookin.  Jotta voit käyttää tämäntyyppistä toiminnon on oltava [automaatio-ratkaisun](log-analytics-add-solutions.md) , asentanut ja määrittänyt OMS työtilassa.  Jos sinulla ei ole sitä asennettuna, kun luot uuden ilmoituksen säännön, näkyviin tulee linkin sen asennuksen.  Voit valita runbooks automaatio-tili, jonka määritit automaatio-ratkaisussa.

Runbookin toiminnot Aloita käyttäminen [webhook](../automation/automation-webhooks.md)runbookin.  Kun luot hälytyksen, se luo uusi webhook: n runbookin varten automaattisesti **OMS ilmoitus korjaus** perään GUID-tunnus-nimisen.  

Et voi täyttää suoraan parametreja: n runbookin, mutta [$WebhookData parametrin](../automation/automation-webhooks.md) sisällytetään ilmoitusta, mukaan lukien, se on luotu loki etsinnän tulokset tietoja.  N runbookin täytyy määrittää **$WebhookData** parametrina sen ominaisuuksia ilmoituksen.  Ilmoitusten tiedot ovat käytettävissä **$WebhookData** **RequestBody** -ominaisuudessa **hakutulokset** yhden ominaisuuden json-muodossa.  Tämä on ominaisuuksien seuraavassa taulukossa.


| Solmu | Kuvaus |
|:--|:--|
| tunnus         | Polun ja GUID-tunnus haku. |
| __metadata | Ilmoitus, kuten tietueiden määrän ja tulos tilasta tietoja. |
|  arvo     |  Erillinen merkintä kunkin tietueen hakutuloksista.  Tapahtuman tiedot vastaavat ominaisuudet ja tietueen arvot.   |

Esimerkiksi seuraavat runbookin purkaa log haun palauttamat tietueet ja määritä eri ominaisuuksien perusteella kunkin tietueen tyyppi.  Huomaa, että: n runbookin alkaa muuntamalla **RequestBody** json niin, että se voi olla käsitellyt PowerShell objektina.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value
    
    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer
        
        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }
        
        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="alert-records"></a>Ilmoitusten tietueet

Ilmoitusten ilmoitusten sääntöjen loki Analytics luomat tietueet on **tyyppi** **-Ilmoituksen** ja **SourceSystem** **OMS**.  Heillä on ominaisuuksien seuraavassa taulukossa.

| Ominaisuus | Kuvaus |
|:--|:--|
| Tyyppi          | *Ilmoitus* |
| SourceSystem  | *OMS* |
| AlertSeverity | Ilmoituksen vakavuustaso. |
| AlertName     | Ilmoituksen nimeä. |
| Kyselyn         | Kysely, joka on suoritettu teksti.  |
| QueryExecutionEndTime   | Kyselyn aikavälin lopussa. |
| QueryExecutionStartTime | Kyselyn aikavälin alkuun.  |
| TimeGenerated | Päivämäärän ja kellonajan ilmoituksen on luotu. |

On erilaisia luodut [ilmoitus Management-ratkaisun](log-analytics-solution-alert-management.md) ja [Power BI: Vie](log-analytics-powerbi.md)ilmoitusten tietueet.  Nämä kaikilla on **tyyppi** , **ilmoitus** , mutta niiden **SourceSystem**mukaan erottaa.




## <a name="next-steps"></a>Seuraavat vaiheet

- Voit analysoida luotu loki Analytics kerääminen järjestelmän Center Operations Manager (SCOM)-ilmoitukset ja hälytykset [ilmoitusten Management-ratkaisun](log-analytics-solution-alert-management.md) asentaminen.
- Lisätietoja [lokin hakuja](log-analytics-log-searches.md) , jotka voivat luoda ilmoituksia.
- Vaiheiden ilmoitusten säännöllä [määrittämisestä webook](log-analytics-alerts-webhooks.md) .  
- Opettele kirjoittamaan [Azure automaatio runbooks](https://azure.microsoft.com/documentation/services/automation) , riskien parantaminen ongelmia merkittyä ilmoitukset.