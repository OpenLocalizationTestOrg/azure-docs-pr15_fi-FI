<properties 
    pageTitle="Sovelluksen havainnollistamisen tehostaminen Pimennys Java käytön aloittaminen" 
    description="Laajennuksen Pimennys avulla voit lisätä suorituskyky ja käytön valvonta Java-sivustoon ja sovelluksen tiedot" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/02/2016" 
    ms.author="awills"/>
 
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a>Sovelluksen havainnollistamisen tehostaminen Pimennys Java käytön aloittaminen

Hakemuksen tiedot SDK lähettää telemetriatietojen Java-web-sovelluksen siten, että voit analysoida käyttöä ja suorituskykyä. Hakemuksen tiedot laajennus Pimennys asentaa SDK automaattisesti projektin niin, että voit siirtyä ruutuun telemetriatietojen sekä API, joiden avulla voit kirjoittaa mukautetun telemetriatietojen ulos.   


## <a name="prerequisites"></a>Edellytykset

Tällä hetkellä laajennuksen works maven-testi ja dynaaminen Pimennys-Web-projekteissa. ([Lisää sovellus havainnollistamisen muuntyyppisten Java projektin][java].)

Sinun on:

* Oracle JRE 1,6 tai uudempi versio
* [Microsoft Azure](https://azure.microsoft.com/)-tilausta. (Voi alkaa [maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).)
* [Pimennys IDE Java ss kehittäjille](http://www.eclipse.org/downloads/)Indigo tai uudempi versio.
* Windows 7 tai uudempi tai Windows Server 2008: n tai uudempi versio

## <a name="install-the-sdk-on-eclipse-one-time"></a>Asenna SDK Pimennys (kerran)

Sinun on vain tämän tietokoneen kohti kerran tehtävä. Tässä vaiheessa asentaa työkalut, jonka jälkeen voit lisätä SDK kunkin dynaamisen Web-projekti.

1. Valitse Pimennys, Ohje, Asenna uusi ohjelmisto.

    ![Ohje, uusi-ohjelmiston asentaminen](./media/app-insights-java-eclipse/0-plugin.png)

2. SDK on http://dl.windowsazure.com/eclipse Azure-Työkalut-kohdassa. 
3. Poista **Ota päivityksen sivustoille...**

    ![Hakemuksen tiedot SDK-paketissa Poista Kysy sivustoille päivitys](./media/app-insights-java-eclipse/1-plugin.png)

Noudattamalla annettuja ohjeita Java-hankkeen eri.

## <a name="create-an-application-insights-resource-in-azure"></a>Luo sovelluksen tiedot-resurssi Azure-tietokannassa

1. Kirjautuminen [Azure portal](https://portal.azure.com).
2. Luo uusi sovelluksen havainnollistamisen resurssi.  

    ![Valitse + ja valitse sovelluksen tiedot](./media/app-insights-java-eclipse/01-create.png)  
3. Määritä sovelluksen Java verkkosovellukseen.  

    ![Täytä nimi, valitse Java web app ja valitse Luo](./media/app-insights-java-eclipse/02-create.png)  
4. Etsi uusi resurssi instrumentation-näppäintä. Tarvitset Liitä tämä koodiprojektin pian.  

    ![Valitse ominaisuudet ja kopioi Instrumentation-näppäintä uusi Resurssikatsaus](./media/app-insights-java-eclipse/03-key.png)  


## <a name="add-application-insights-to-your-project"></a>Hakemuksen tiedot lisääminen projektiin

1. Lisää sovellus havainnollistamisen Java web projektin pikavalikosta.

    ![Valitse ominaisuudet ja kopioi Instrumentation-näppäintä uusi Resurssikatsaus](./media/app-insights-java-eclipse/02-context-menu.png)

2. Liitä instrumentation avainta, jota olet saanut Azure-portaalista.

    ![Valitse ominaisuudet ja kopioi Instrumentation-näppäintä uusi Resurssikatsaus](./media/app-insights-java-eclipse/03-ikey.png)


Avaimen lähetetään sekä jokaisen kohteen telemetriatietojen ja kertoo hakemuksen tiedot näkyvät resurssi.

## <a name="run-the-application-and-see-metrics"></a>Suorita sovellus ja katso arvot

Suorita sovellus.

Palaa sovelluksen tietoja: Microsoft Azure resurssi.

HTTP-pyynnöt tiedot näkyvät Yhteenveto-sivu. (Jos se ei löydy, odota hetki ja valitse sitten Päivitä.)

![Palvelimen vastaus, pyyntö määrät ja virheet ](./media/app-insights-java-eclipse/5-results.png)
 

Napsauttamalla mitä tahansa kaavion saat näkyviin lisää arvot. 

![Pyyntö laskee nimen mukaan](./media/app-insights-java-eclipse/6-barchart.png)


[Lue lisää arvot.][metrics]

 

Ja pyynnön ominaisuuksien tarkasteleminen, kun näet telemetriatietojen tapahtumia, kuten pyynnöt ja poikkeukset liittyy.
 
![Kaikki tapahtumat pyynnön](./media/app-insights-java-eclipse/7-instance.png)


## <a name="client-side-telemetry"></a>Asiakkaan telemetriatietojen

Valitse Hae koodi seurannassa web-sivujen pikaopas-sivu: 

![Valitse sovellus-yleiskatsaus sivu Pika-aloitus, Hae koodi seurannassa web-sivuja. Kopioi komentosarja.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

Lisää koodikatkelman HTML-tiedostojen otsikkoon.

#### <a name="view-client-side-data"></a>Asiakkaan tietojen tarkasteleminen

Avaa päivitetty verkkosivujen ja käyttää niitä. Odota hetki tai kahdessa, ja valitse palautettava sovellus havainnollistamisen ja avaa käyttö-sivu. (Valitse Yhteenveto-sivu alaspäin ja valitse käyttö.)

Sivun näkymän, käyttäjän ja istunnon arvot näkyvät käyttö-sivu:

![Istunnot, käyttäjät ja sivun näkymät.](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[Lisätietoja asiakkaan telemetriatietojen määrittäminen.][usage]

## <a name="publish-your-application"></a>Sovelluksen julkaiseminen

Sovelluksen julkaiseminen palvelimeen, ja katso telemetriatietojen näkyvät portaalissa käyttäjien käyttää nyt.

* Varmista, että palomuurin sovelluksen telemetriatietojen lähettäminen seuraavat portit:

 * DC.Services.visualstudio.com:443
 * DC.Services.visualstudio.com:80
 * F5.Services.visualstudio.com:443
 * F5.Services.visualstudio.com:80


* Asenna Windows-palvelimiin:

 * [Microsoft Visual C++ Redistributable](http://www.microsoft.com/download/details.aspx?id=40784)

    (Näin suorituskyvyn laskureita.)

## <a name="exceptions-and-request-failures"></a>Poikkeukset ja pyynnön virheet

Käsittelemättömän poikkeukset kerätään automaattisesti:

![](./media/app-insights-java-eclipse/21-exceptions.png)

Kerätä tietoja muiden poikkeukset, sinulla on kaksi vaihtoehtoa:

* [Lisää TrackException koodissa puhelut](app-insights-api-custom-events-metrics.md#track-exception). 
* [Asenna Java-agentti palvelimeen](app-insights-java-agent.md). Voit määrittää menetelmiä haluat seurata.


## <a name="monitor-method-calls-and-external-dependencies"></a>Seurata menetelmäkutsujen ja riippuvuussuhteet

Kirjaudu [asentaa Java-agentti](app-insights-java-agent.md) määritetty sisäinen menetelmistä ja soitettua puhelua kautta JDBC-ja aikatiedot.


## <a name="performance-counters"></a>Suorituskyvyn laskureita

Yhteenveto-sivu Vieritä alas ja valitse **palvelimet** -ruutu. Näet suorituskyvyn laskureita solualueen.


![Vieritä napsauttamalla palvelimet-ruutu](./media/app-insights-java-eclipse/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Suorituskyvyn laskuri sivustokokoelman mukauttaminen

Käytöstä sivustokokoelman suorituskyvyn laskureita ja, Lisää seuraava koodi pääkansion solmun ApplicationInsights.xml tiedoston:

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>

### <a name="collect-additional-performance-counters"></a>Kerää suorituskykylaskureita

Voit määrittää muita suorituskykylaskureita kerätään.

#### <a name="jmx-counters-exposed-by-the-java-virtual-machine"></a>JMX laskureita (tarjoamia mukaan Java virtuaalikoneen)

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>

*   `displayName`– Nimi näkyy sovelluksen tiedot-portaalissa.
*   `objectName`– JMX objektinimi.
*   `attribute`– JMX objektinimi hakeaksesi määrite
*   `type`(valinnainen) - määritteen JMX objektin tyyppi:
 *  Oletusarvo: Yksinkertainen tyyppi kuten kokonaisluku tai pitkä.
 *  `composite`: resurssilaskurin tietojen on "Attribute.Data"-muodossa
 *  `tabular`: resurssilaskurin tietojen on taulukkorivin muoto



#### <a name="windows-performance-counters"></a>Windowsin suorituskykylaskureita

Kunkin [Windowsin suorituskyky laskuri](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) on luokan jäsen (samalla tavalla, että kenttä on luokan jäsen). Luokat voivat olla yleinen, tai voit on numeroitu tai nimeltä esiintymät.

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>

*   Näyttönimi – sovelluksen tiedot-portaalissa näkyvä nimi.
*   LuokanNimi –, johon on liitetty suorituskyvyn laskuri suorituskyvyn laskuri luokan (objekti).
*   counterName – nimen suorituskyky laskuri.
*   EsiintymänNimi – nimen suorituskyky laskuri luokan esiintymän tai tyhjä merkkijono (""), jos luokka sisältää yksittäisen esiintymän. Jos ryhmän on prosessi ja haluat kerätä suorituskyvyn laskuri on nykyisen JVM prosessista sovelluksen suoritetaan, Määritä `"__SELF__"`.

Suorituskyvyn laskureita näkyvät mukautettua arvot [Arvot]Explorerissa[metrics].

![](./media/app-insights-java-eclipse/12-custom-perfs.png)


### <a name="unix-performance-counters"></a>UNIX suorituskyvyn laskureita

* Hae erilaisia järjestelmän ja verkon tietojen [collectd kanssa sovelluksen tiedot-laajennuksen asentaminen](app-insights-java-collectd.md) .

## <a name="availability-web-tests"></a>Käytettävyys web testit

Hakemuksen tiedot voit testata verkkosivuston, tarkista, että se on käytössä ja vastaanottamisen sekä säännöllisin väliajoin. [Voit määrittää][availability], valitse käytettävyys alaspäin.

![Vieritä alaspäin, valitse käytettävyys, valitse Lisää WWW-testi](./media/app-insights-java-eclipse/31-config-web-test.png)

Saat kaavioiden vastauksen kertaa sekä sähköposti-ilmoitukset, jos sivuston siirtyy.

![Web-testi-Esimerkki](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[Lisätietoja käytettävyys web testit.][availability] 

## <a name="diagnostic-logs"></a>Vianmäärityslokit

Jos käytät Logback tai Log4J (v1.2 tai v2.0) jäljitys, voit määrittää jäljityksen lokit lähetetään automaattisesti sovelluksen havainnollistamisen, jossa voit tarkastella ja etsiä niitä.

[Lisätietoja vianmäärityslokit][javalogs]

## <a name="custom-telemetry"></a>Mukautettu telemetriatietojen 

Lisää muutama koodin rivit Java verkkosovelluksen selvittää, mitä käyttäjät tekevät sitä tai selvittää ongelmia. 

Voit lisätä koodin sekä verkkosivun JavaScript palvelinpuolen Java.

[Lisätietoja mukautettujen telemetriatietojen][track]



## <a name="next-steps"></a>Seuraavat vaiheet

#### <a name="detect-and-diagnose-issues"></a>Haku- ja vianmääritys

* [Lisää WWW-asiakasohjelman telemetriatietojen] [ usage] suorituskyvyn telemetriatietojen käyttämistä web-asiakasohjelmassa.
* [Määritä web testit] [ availability] , varmista, että sovelluksesi pysyy suorien ja vastaa.
* [Etsi tapahtuma- ja lokit] [ diagnostic] voi selvittää ongelmia.
* [Sieppaa Log4J tai Logback jäljittää][javalogs]

#### <a name="track-usage"></a>Seurata käyttöä

* [Lisää WWW-asiakasohjelman telemetriatietojen] [ usage] näytön sivun näkymiä ja käyttäjän arvot.
* [Mukautetut tapahtumat ja arvot seuranta] [ track] lisätietoja siitä, miten sovelluksesi käytetään, sekä asiakkaan ja palvelimen.


<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 