<properties
    pageTitle="Sovelluksen tietoa JavaScript-web-sovellusten | Microsoft Azure"
    description="Hae sivun näkymän ja istunnon laskee-Web-sivuston asiakkaan tiedot, ja seuraa käyttötavat. Tunnista JavaScript-web-sivujen poikkeukset ja suorituskykyongelmia."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="awills"/>

# <a name="application-insights-for-web-pages"></a>Verkkosivujen sovelluksen tietoa


[AZURE.INCLUDE [app-insights-selector-get-started-dotnet](../../includes/app-insights-selector-get-started-dotnet.md)]

Tutustu suorituskyvyn ja käyttömäärän verkkosivu tai sovelluksen. Jos lisäät Visual Studio hakemuksen tiedot sivun komentosarjan, saat ajoitukset sivujen latausaikaa sekunneilla AJAX puhelut, määrät ja selaimen poikkeukset ja AJAX virheet, sekä käyttäjien ja istunnon laskee tiedot. Kaikki näihin voit Segmentoitu sivun, asiakkaan käyttöjärjestelmän ja selaimen versio, geo sijainti ja muiden dimensioiden. Voit myös ilmoitusten määrittäminen epäonnistui laskee tai hidastaa sivun lataamista.

Voit käyttää sovelluksen havainnollistamisen web-sivuilla – voit lisätä lyhyt JavaScript-osan. Web-palvelu on [Java](app-insights-java-get-started.md) tai [ASP.NET](app-insights-asp-net.md), voit integroida Serveristä ja asiakkaiden telemetriatietojen.

Tarvitset [Microsoft Azure](https://azure.com)-tilausta. Jos ryhmä on organisaation tilauksen, pyydä omistajaa Microsoft Account lisääminen. Ei vapaa hinnoittelu taso, joten kehittäminen ja pieniä käyttö ei kustannusten mitään.


## <a name="set-up-application-insights-for-your-web-page"></a>Web-sivun tiedot sovelluksen määrittäminen

Ensin sinun on lisättävä verkkosivujen sovelluksen tiedot? Ehkä jo tehnyt niin. Jos valitsit Lisää sovellus havainnollistamisen web App-sovellukseen, uusi projekti-valintaikkunassa Visual Studiossa, komentosarja on lisätty sitten. Tässä tapauksessa ei tarvitse tehdä mitään muuta.

Muussa tapauksessa sinun on lisättävä koodi katkelma web-sivuille seuraavasti.

### <a name="open-an-application-insights-resource"></a>Avaa sovelluksen tiedot-resurssi

Hakemuksen tiedot resurssi on, jolloin sivun suorituskyvyn ja käyttömäärän koskevia tietoja näkyy. 

Kirjautua [Azure-portaaliin](https://portal.azure.com).

Jos määrität jo sovelluksesi palvelinpuolen seurantaa, sinulla on jo resurssi:

![Valitse Selaa, Developer palvelut-sovelluksen tiedot.](./media/app-insights-javascript/01-find.png)

Jos sinulla ei ole, luo se:

![Valitse uusi, Developer palvelut-sovelluksen tiedot.](./media/app-insights-javascript/01-create.png)


*Jo kysymyksiä?* [Lisätietoja resurssin luominen](app-insights-create-new-resource.md).


### <a name="add-the-sdk-script-to-your-app-or-web-pages"></a>Sovelluksen tai web-sivujen SDK-komentosarjan lisääminen

Pikaoppaassa hakeminen verkkosivujen komentosarja:

![Valitse sovellus-yleiskatsaus sivu Pika-aloitus, Hae koodi seurannassa web-sivuja. Kopioi komentosarja.](./media/app-insights-javascript/02-monitor-web-page.png)

Lisää juuri ennen komentosarjan `</head>` jokaisen sivun, jota haluat seurata tunnistetta. Jos sivuston perustyylisivun, voit sijoittaa komentosarja siellä. Esimerkki:

* ASP.NET-MVC projektin lisäät sen`View\Shared\_Layout.cshtml`
* Avaa SharePoint-sivustoon, valitse Ohjauspaneelin [sivustoasetuksiin / perustyylisivu](app-insights-sharepoint.md).

Komentosarja on instrumentation avain, joka ohjaa hakemuksen tiedot resurssin tiedot. 

([Tarkempaa selvitys siitä, käytössä](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))

*(Jos käytät tunnetun web-sivun kehys, Etsi hakemuksen tiedot sovittimia. Http://servername, on [AngularJS-moduuli](http://ngmodules.org/modules/angular-appinsights).)*


## <a name="detailed-configuration"></a>Määrityksistä

On useita [parametreja](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) voit määrittää, mutta useimmissa tapauksissa sinun ei kannata täytyy. Voit esimerkiksi poistaa käytöstä tai rajoittaa Ajax kutsuja raportoitu sivunäkymän kohden (Voit pienentää liikenne). Tai voit määrittää Virheenkorjaustila on siirtyä nopeasti putkijohto ilman, että erämuotoinen telemetriatietojen.

Voit määrittää nämä parametrit Etsi koodikatkelman tämän rivin ja lisätä sen jälkeen CSV-kohteita:

    })({
      instrumentationKey: "..."
      // Insert here
    });

[Käytettävissä olevat parametrit](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) ovat:

    // Send telemetry immediately without batching.
    // Remember to remove this when no longer required, as it
    // can affect browser performance.
    enableDebug: boolean,

    // Don't log browser exceptions.
    disableExceptionTracking: boolean,

    // Don't log ajax calls.
    disableAjaxTracking: boolean,

    // Limit number of Ajax calls logged, to reduce traffic.
    maxAjaxCallsPerView: 10, // default is 500

    // Time page load up to execution of first trackPageView().
    overridePageViewDuration: boolean,

    // Set these dynamically for an authenticated user.
    appUserId: string,
    accountId: string,



## <a name="run"></a>Suorita sovellus

Suorita web app-käyttää sitä Luo telemetriatietojen ja odota hetki jonkin aikaa. Voit suorittaa käyttämällä **F5** -näppäintä kehittäminen tietokoneeseen tai julkaista sen ja toistaa sitä käyttäjille.

Jos haluat tarkistaa, joka lähettää verkkosovellukseen sovelluksen havainnollistamisen telemetriatietojen, käytä selaimen vianmääritystyökaluista (**F12** -selaimissa). Tiedot lähetetään dc.services.visualstudio.com.

## <a name="explore-your-browser-performance-data"></a>Tietojen selaimen suorituskyky

Avaa näyttämään koostetun suorituskykytietoja käyttäjien selaimilla selaimet-sivu.

![Avaa sinua sovelluksen resurssin portal.azure.com, ja valitse asetukset-selaimessa](./media/app-insights-javascript/03.png)


*Ei ole vielä tietoja? Valitse * *Päivitä* * sivun yläreunassa. Edelleen mitään? Katso [vianmääritys](app-insights-troubleshoot-faq.md).*

Selaimet-sivu on [mittarit Explorer sivu](app-insights-metrics-explorer.md) ja valmiit suodattimet-kaavion valinnat. Voit muokata aikaväli, suodattimien ja kaavion määrittäminen Jos ja tallentaa tuloksen suosikkeihin. Valitse **Palauta oletusarvot** pääset takaisin alkuperäiseen sivu-määritys.

## <a name="page-load-performance"></a>Sivun kuormituksen suorituskyky

Ylimpänä on Segmentoitu kaavion sivujen lataamista. Kaavion yhteensä korkeutta edustaa keskimääräinen aika lataamisen ja näyttää sivujen sovelluksestasi käyttäjien selaimissa. Aika mitataan kun selain lähettää alkuperäisen HTTP-pyynnön ennen kuin kaikki synkronisen kuormituksen tapahtumien käsitelty, mukaan lukien asettelun ja komentosarjojen suorittaminen. Se ei sisällä asynkroninen tehtäviä, kuten ladataan verkko-osien AJAX puhelut.

Kaavion lohkoja kokonaissivumäärä latausajasta [Vakio ajoitukset W3C määrittämiä](http://www.w3.org/TR/navigation-timing/#processing-model)kyselyjä. 

![](./media/app-insights-javascript/08-client-split.png)

Huomaa, että *verkkoyhteyden* aika on usein pienempi kuin voisi olettaa, koska se on keskiarvoon kaikki palvelupyynnöt selaimesta palvelimen kautta. Useita yksittäisiä pyynnöt on muodostettaessa 0, koska on jo aktiivisen yhteyden palvelimeen.

### <a name="slow-loading"></a>Hidas lataaminen?

Hidas sivujen latausaikaa sekunneilla ovat tyytymättömyyden käyttäjien pää lähteen. Jos kaavio osoittaa hidas sivujen latausaikaa sekunneilla, on helppo tehdä joitakin diagnostiikan Oheistiedot.

Kaavio näyttää kaikkien sivujen latausaikaa sekunneilla keskiarvo-sovelluksen. Jos ongelma vain tietyt sivut näkyviin kohde alemmas sivu, ei ruudukon Segmentoitu sivun URL-osoitteen avulla:

![](./media/app-insights-javascript/09-page-perf.png)

Huomaa, että Näytä sivumäärä ja keskihajonta. Jos sivujen määrä on hyvin pieni, valitse ongelma ei ole vaikuttavia käyttäjien paljon. Suuri (vastaa itse keskiarvo) keskihajonta ilmaisee useita yksittäisiä mitat välillä.

**Suurentaa yksi URL-osoite ja yksi sivu-näkymässä.** Valitse minkä tahansa sivun nimi, jos haluat nähdä suodatettuna vain kyseisen URL-osoite selaimen kaaviot sivu ja sen jälkeen, valitse sivu-näkymä esiintymän.

![](./media/app-insights-javascript/35.png)

Valitse `...` kyseisen tapahtuman ominaisuudet täydellinen luettelo tai tarkastaa Ajax puhelut ja niihin liittyvät tapahtumat. Hidas Ajax puhelut vaikuttavat koko sivun latausajasta, jos ne on synkronoitu. Aiheeseen liittyvät tapahtumat sisältävät pyynnöt palvelimen URL-Osoitetta (Jos olet asentanut sovelluksen havainnollistamisen verkkosivustoon).


**Sivun suorituskyky ajan kuluessa.** Selaimet, sivu uudelleen milloin Muuta sivun näkymän Latausajasta ruudukon viivakaavio, ovatko oli päät tietyn aikoina:

![Valitse ruudukon muistissaan ja valitse uusi kaaviolaji](./media/app-insights-javascript/10-page-perf-area.png)

**Määritetään muiden dimensioiden mukaan.** Sivut ovat ehkä hitaammin lataaminen selaimeen, asiakkaan OS tai käyttäjän paikka? Lisää uusi kaavio ja kokeilla **Group by** -dimensio.

![](./media/app-insights-javascript/21.png)


## <a name="ajax-performance"></a>AJAX-suorituskyky

Varmista, että AJAX puhelujen web-sivujen toimivat hyvin. Niitä käytetään usein täyttää sivun osat asynkronisesti. Vaikka yleinen sivua voi ladata välittömästi, käyttäjien saattaa voidaan kehysnopeuteen staring on tyhjä verkko-osat, odotetaan, ne näkyvät tiedot.

Selaimet sivu näytetään web-sivulta AJAX puhelujen riippuvuudet nimellä.

Yhteenvetokaaviot yläosassa sivu on:

![](./media/app-insights-javascript/31.png)

ja yksityiskohtainen ruudukoiden alemman alaspäin:

![](./media/app-insights-javascript/33.png)

Valitse minkä tahansa rivin tiedot.


> [AZURE.NOTE] Jos poistat selaimet suodatusvaihtoehdot sivu, sekä palvelimessa oleva että AJAX riippuvuudet sisältyvät kaavioista. Valitsemalla Palauta oletusarvot määrittämään suodatin.

**Voit siirtyä epäonnistunut Ajax puhelut** riippuvuuden virheet ruudukon kohtaan ja valitse sitten haluat nähdä tietyn esiintymät.

![](./media/app-insights-javascript/37.png)

Valitse `...` koko telemetriatietojen Ajax puhelun varten.

### <a name="no-ajax-calls-reported"></a>Ajax-soittoja raportoitu?

AJAX-puhelut sisältävät kaikki HTTP-web-sivun komentosarjan puhelujen. Jos et näe niitä ilmoitettu, tarkista, että koodikatkelman ei `disableAjaxTracking` tai `maxAjaxCallsPerView` [Parametrit](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).

## <a name="browser-exceptions"></a>Selaimen poikkeukset

Selaimet, sivu ei yhteenveto poikkeukset-kaavio ja ruudukko tyyppisiä poikkeuksen edelleen alaspäin sivu.

![](./media/app-insights-javascript/39.png)

Jos et näe selaimen poikkeukset ilmoitetaan, tarkista, että koodikatkelman ei `disableExceptionTracking` [parametria](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).

## <a name="inspect-individual-page-view-events"></a>Tarkasta yksittäisen sivun tapahtumien tarkasteleminen

Yleensä sivun näkymän telemetriatietojen analysoidaan mukaan hakemuksen tiedot ja tuo näkyviin vain kumulatiivinen raportit kaikkien käyttäjien keskiarvona. Mutta vianmääritystä varten, voit myös tarkastella yksittäisen sivun tapahtumien tarkasteleminen.

Määritä suodattimia sivunäkymän diagnostiikan haku-sivu.

![](./media/app-insights-javascript/12-search-pages.png)

Valitse mikä tahansa tapahtuma yksityiskohdat. Valitse tiedot-sivulla "..." jopa yksityiskohdat.

> [AZURE.NOTE] Jos käytät [haun](app-insights-diagnostic-search.md), Huomaa, että sinulla on vastattava koko sanat: "Abou" ja "tietoja" eivät vastaa "Tietoja".

Voit käyttää tehokkaita [Analytics kyselyn kielen](app-insights-analytics-tour.md) etsimään sivun näkymät.

### <a name="page-view-properties"></a>Sivun ominaisuuksien tarkasteleminen

* **Sivun näkymän kesto** 

 * Oletusarvon mukaan-sivulla Lataa asiakkaan kuluvaa aikaa pyytää täyden kuormituksen (mukaan lukien Aux tiedostot mutta lukuun ottamatta asynkroninen tehtäviä, kuten Ajax soittaa). 
 * Jos määrität `overridePageViewDuration` [sivun määritysten](#detailed-configuration)asiakkaan väli pyytää suorittamisen ensimmäisen `trackPageView`. Jos olet siirtänyt trackPageView sen tavallista kohdasta komentosarja alustamisen jälkeen, se vaikuttavat eri arvon.
 * Jos `overridePageViewDuration` määrittäminen ja argumentti on säädetty kesto `trackPageView()` Soita-argumentin arvo on käyttää sen sijaan. 


## <a name="custom-page-counts"></a>Mukautetun sivun laskee

Sivujen määrä tapahtuu oletusarvon mukaan aina, kun uusi sivu ladataan selaimeen.  Mutta voit laskea muita sivun näkymät. Esimerkiksi sivun voivat näkyä sisällön välilehdet ja haluat laskea sivuun, kun käyttäjä vaihtaa välilehdet. Tai JavaScript-koodia sivulla voi ladata uutta sisältöä muuttamatta selaimen URL-osoite.

Lisää JavaScript puhelun tältä asiakas-koodin haluamasi kohtaan:

    appInsights.trackPageView(myPageName);

Sivun nimi voi olla samaa merkkiä URL-osoitteena, mutta mikä tahansa "#" tai "?" ohitetaan.



## <a name="usage-tracking"></a>Seurannan käyttö


Haluatko selvittää, mitä käyttäjien tehdä sovelluksen?

* [Lisätietoja käyttö seuranta](app-insights-web-track-usage.md)
* [Lue lisää mukautetut tapahtumat ja arvot API](app-insights-api-custom-events-metrics.md).


#### <a name="video"></a>Video: Käytön seuranta

> [AZURE.VIDEO tracking-usage-with-application-insights]

## <a name="next"></a>Seuraavat vaiheet

* [Seurata käyttöä](app-insights-web-track-usage.md)
* [Mukautetut tapahtumat ja arvot](app-insights-api-custom-events-metrics.md)
* [Mittayksikön muodosta lisätietoja](app-insights-overview-usage.md)


