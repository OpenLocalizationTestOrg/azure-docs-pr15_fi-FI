<properties
    pageTitle="Parhaiden käytäntöjen ja vianmäärityksen oppaan solmu sovellusten Azure Web Apps-sovelluksista"
    description="Tutustu parhaisiin käytäntöihin ja Azure Web Apps-sovellusten solmu vianmääritysohjeita."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="ranjithr"
    manager="wadeh"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="06/06/2016"
    ms.author="ranjithr;wadeh"/>
    
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Parhaiden käytäntöjen ja vianmäärityksen oppaan solmu sovellusten Azure Web Apps-sovelluksista

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Tässä artikkelissa kerrotaan parhaita käytäntöjä ja vianmääritysohjeita [solmu sovellusten](app-service-web-nodejs-get-started.md) käytössä Azure Webapps ( [iisnode](https://github.com/azure/iisnode)).

>[AZURE.WARNING] Ole varovainen, kun käyttämällä vianmääritysohjeita tuotannon sivustossa. Suositus on vianmääritys sovelluksen tuotannon määritys, kuten väliaikaisen paikka ja kun ongelma on kiinteä, Vaihda oman väliaikaisen paikka tuotannon paikka kanssa.

## <a name="iisnode-configuration"></a>IISNODE määritys

[Mallitiedoston](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) näyttää iisnode voidaan määrittää haluamasi asetukset. Joitakin asetuksia, jotka on kuitenkin hyötyä sovelluksen ovat seuraavat:

* nodeProcessCountPerApplication

    Tällä asetuksella kohti IIS-sovelluksen käynnistää solmu prosessien määrän. Oletusarvo on 1. Voit käynnistää niin monta node.exe AM core oman määränä määrittämällä tämä 0. Suositeltu arvo on 0, useimmat sovelluksen niin asiakas käyttää kaikki sydämiä käyttämääsi laitteeseen. Node.exe on yksi threaded niin yhden node.exe käyttää enintään 1 core ja saat parhaan mahdollisen suorituskyvyn, haluat ehkä käyttää kaikkien sydämiä solmu sovelluksen ulos.

* nodeProcessCommandLine

    Tällä asetuksella node.exe polku. Voit määrittää tämän arvon osoittamaan node.exe-versio.

* maxConcurrentRequestsPerProcess

    Tällä asetuksella samanaikaiset pyynnöt lähettämä iisnode kunkin node.exe enimmäismäärä. Azure webapps Tämä oletusarvo on jatkuva. Sinun ei tarvitse huolehtia tämän asetuksen. Azure webapps ulkopuolella oletusarvo on 1 024. Voit määrittää tämän sen mukaan, kuinka monta pyytää sovelluksen saa ja kuinka nopeasti sovelluksen käsittelee sivupyynnön.

* maxNamedPipeConnectionRetry

    Tällä asetuksella kuinka monta kertaa iisnode yrittää kaikki tarvittavat yhteyden nimettyjen putkien lähettää pyynnön node.exe päälle. Yhdessä namedPipeConnectionRetryDelay tämä asetus määrittää sivupyynnön sisällä iisnode yhteensä aikakatkaisu. Oletusarvo on Azure Webapps 200. Yhteensä aikakatkaisu sekunteina = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1 000

* namedPipeConnectionRetryDelay

    Tämä asetus määrittää ajan (millisekunteina) iisnode määrä odottaa välillä kunkin yritä lähettää pyynnön node.exe nimettyjen putkien päälle. Oletusarvo on 250ms.
    Yhteensä aikakatkaisu sekunteina = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1 000

    Valitse azure webapps iisnode yhteensä aikakatkaisu on oletusarvon mukaan 200 \* 250ms = 50 sekuntia.

* logDirectory

    Tällä asetuksella kansio, johon iisnode Kirjaudu stdout/stderr. Oletusarvo on iisnode, joka on suhteessa tärkeimmät komentosarja-kansio (kansio missä tärkeimmät server.js esitä)

* debuggerExtensionDll

    Tällä asetuksella minkä version solmu tarkastaminen iisnode käytetään, kun virheenkorjaus solmu-sovellus. Iisnode tarkastaminen-0.7.3.dll ja iisnode inspector.dll ovat tällä hetkellä vain 2 kelvollisia arvoja tämä asetus. Oletusarvo on iisnode tarkastaminen-0.7.3.dll. iisnode tarkastaminen-0.7.3.dll versio käyttää solmu-tarkastaminen-0.7.3 ja käyttää websockets, joten täytyy ottaa käyttöön websockets Web tämä oman azure App-sovelluksen käyttöön. Katso lisätietoja <http://www.ranjithr.com/?p=98> määrittäminen iisnode uusi solmu-tarkastaminen-toiminnolla.

* flushResponse

    IIS oletusasetuksista on, että se puskuroi vastauksen tietojen määrittäminen 4 Mt ennen materiaalinottoon tai vastauksen, loppuun saakka kumpi on ensin. iisnode tarjoaa määritys-asetusta, voit ohittaa tämän: tyhjentämään osa vastauksen kohteen tekstiin, kun iisnode vastaanottaa sen node.exe on määritettävä iisnode/@flushResponse web.config "true"-määrite:
    
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```

    Ottaminen käyttöön jokaisen osan vastauksen kohteen tekstiin materiaalinottoon Lisää suorituskyvyn katseltavan, joka vähentää järjestelmän siirtonopeuden noin 5 % (vuodesta v0.1.13), niin kannattaa laajuus asetus vain päätepisteet, jotka edellyttävät vastauksen streaming (kuten avulla <location> osan web.config)

    Saat streaming-sovellukset, tarvitset myös iisnode käsittelijä responseBufferLimit arvoksi 0.
    
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```

* watchedFiles

    Tämä on erotettu puolipiste luettelo tiedostoista, jotka seurattujen muutosten. Tiedoston muuttaminen aiheuttaa sovelluksen Roskakori. Tekstien koostuu valinnainen kansionimen plus tarvittavaa tiedostonimi, joka on suhteessa hakemiston ensisijaista sovellusta aloituskohtaa sijainti. Yleismerkit sallitaan tiedoston nimi-vain-osaan. Oletusarvo on "\*. js;web.config"

* recycleSignalEnabled

    Oletusarvo on EPÄTOSI. Jos käytössä-solmu-sovelluksen voit muodostaa yhteyden nimettyjen putkien (ympäristömuuttuja IISNODE\_OHJAUSOBJEKTIN\_PYSTYVIIVAA) ja lähettää "Roskakorin"-viesti. Tällöin voit Roskakorin tilanteen w3wp.

* idlePageOutTimePeriod

    Oletusarvo on 0, mikä tarkoittaa, että tämä ominaisuus on poistettu käytöstä. Jos määritetty arvoon suurempi kuin 0, iisnode sivun, kaikki sen aliprosesseja jokaisen 'idlePageOutTimePeriod' millisekuntia. Selvittääksesi, mikä tarkoittaa, sivu, katso [ohjeet](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Tämä asetus on hyötyä sovelluksissa, jotka kuluttavat paljon muistia ja haluat pageout muistia levylle toisinaan vapautumista joitakin RAM-Muistia.

>[AZURE.WARNING] Ole varovainen, kun otat käyttöön seuraavien kokoonpanoasetusten tuotannon-sovelluksiin. Suositus on ne käyttöön ei ole suoraa tuotannon sovellukset.

* debugHeaderEnabled

    Oletusarvo on EPÄTOSI. Jos arvo on TOSI, iisnode lisätään HTTP vastauksen otsikon iisnode-virheenkorjaus jokainen HTTP-vastaus lähettää iisnode virheenkorjaus otsikon arvo on URL-osoite. Vianmääritystiedot yksittäisiä laitteita voidaan asianmukaisine katsomalla URL-Osoitteen osa, mutta paljon paremmin visualisoinnin saavutetaan avaamalla URL-Osoitteen selaimeen.

* loggingEnabled

    Tällä asetuksella stdout ja stderr kirjaaminen iisnode mukaan. Iisnode kerätä stdout/stderr solmu prosessien se käynnistyy ja kirjoittaa määritettyyn hakemistoon "logDirectory"-asetus. Kun kyseessä on ottaminen käyttöön, sovelluksen olla lokit tiedostojärjestelmän ja kirjoittaminen kirjaus tehty sovellus määrän mukaan, tämä saattaa johtua suorituskyvyn vaikutukset.

* devErrorsEnabled

    Oletusarvo on EPÄTOSI. Kun arvo on TOSI, iisnode näkyy HTTP-tilakoodin ja Win32-virhekoodi sovellukseen selaimessa. Win32-koodi on hyötyä virheenkorjaus tietyntyyppisiä ongelmat.
    
* debuggingEnabled (Älä Ota reaaliaikainen tuotannon sivustossa)

    Tällä asetuksella muistin ominaisuus. Iisnode on integroitu solmu tarkastaminen. Otat tämän asetuksen käyttöön, voit ottaa käyttöön virheenkorjaus solmu-sovelluksen. Kun tämä asetus on käytössä, iisnode tulee asettelun ensimmäisen virheenkorjaus pyydettäessä solmu sovelluksen 'debuggerVirtualDir' hakemiston tarvittavat solmu-tarkastaminen tiedostot. Voit ladata solmu-tarkastaminen lähettämällä pyynnön http://yoursite/server.js/debug. Voit hallita virheenkorjaus URL-osiossa "debuggerPathSegment"-asetus. DebuggerPathSegment oletusarvon mukaan = virheenkorjaus". Voit määrittää tämän GUID-tunnus, esimerkiksi siten, että se on vaikea havaitaan muut.

    Katso lisätietoja virheenkorjaus tätä [linkkiä](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) .

## <a name="scenarios-and-recommendationstroubleshooting"></a>Skenaariot ja suositukset ja vianmääritys

### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Solmu-sovellus on tekeminen liikaa lähtevät puhelut.

Useita sovelluksia haluat tehdä Lähtevät yhteydet niiden säännöllinen toiminta osana. Esimerkiksi pyyntö tullessa solmu sovelluksen haluat Ota REST-Ohjelmointirajapinnalla muualla ja tietoja pyynnön. Haluat käyttää Säilytä toiminnassa agentti, http tai https soittamista. Esimerkiksi voit käyttää agentkeepalive moduulin Säilytä toiminnassa edustajaksi tehtäessä lähtevät puhelut. Näin varmistat, että sockets käytetään uudelleen oman azure Web App-sovelluksen AM ja luoda uuden sockets jokaisen lähtevän pyynnön katseltavan vähentämiseksi. Lisäksi Näin varmistat, että käytössäsi on vähemmän sockets tehdä monta lähtevä pyynnöt ja näin ollen ei saa ylittää maxSockets, joka on kohdistettu kohti AM. Valitse Azure Webapps suositus olisi agentKeepAlive maxSockets-arvo arvoksi korkeintaan 160 sockets AM kohden. Tämä tarkoittaa, että jos sinulla on käytössä AM 4 node.exe, haluat määrittää agentKeepAlive maxSockets 40 node.exe 160 Yhteensä per AM eli kohden.

Esimerkki agentKeepALive määritykset:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

Tässä esimerkissä oletetaan, että 4 node.exe oman AM käytössä. Jos sinulla on käytössä AM node.exe eri määrän, sinun on Muokkaa maxSockets määrittäminen vastaavasti.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Solmu-sovellus on käyttö liikaa suorittimen.

Näkyviin tulee todennäköisesti suositus Azure Webapps-portaalissa suuri suorittimen kulutus tietoja. Voit määrittää myös näyttöjen odottaminen tietyt [arvot](web-sites-monitor.md). Suorittimen käyttö [Azure Portal Raporttinäkymät-ikkunan](../application-insights/app-insights-web-monitor-performance.md)tarkistettaessa Tarkista MAX-arvot suorittimen niin, Huippu-arvot eivät jää huomaamatta.
Jos uskot sovelluksesi on muissa liikaa suorittimen ja et voi kerrotaan, miksi tapauksissa sinun on profiilin solmu-sovellus.

### 

#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Solmu-ohjelman kanssa V8 Profilerin azure webapps profilointi

Esimerkiksi Voit vaikkapa Hei maailma-sovellus, jonka haluat profiilin alla kuvatulla tavalla:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Siirry palvelujen ohjauksen hallinta-sivuston https://yoursite.scm.azurewebsites.net/DebugConsole

Komentorivi-ikkuna tulee näkyviin, alla kuvatulla tavalla. Siirry sivuston/wwwroot-kansioon

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

"Npm asentaa v8 Profilerin" komennon suorittaminen

Tämä on asennettava v8-kuvaajan solmun\_moduulit-kansio ja kaikki sen tarvitsemat.
Muokkaa-kohdassa server.js profiilin sovelluksen.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Yllä muutokset profiilin WriteConsoleLog-funktio ja kirjoita 'profile.cpuprofile'-tiedostoon, valitse sivusto-wwwroot profiilin tulos. Lähetä pyyntö sovelluksen. Näkyviin tulee sivuston wwwroot luotuihin 'profile.cpuprofile' tiedoston.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Lataa tämä tiedosto, ja sinun on tämän tiedoston avaaminen Chrome F12-työkaluja. Painamalla F12 chrome-ja valitse sitten-profiileista välilehden". Napsauta "Lataa"-painiketta. Valitse juuri lataamasi profile.cpuprofile-tiedosto. Valitse juuri lataamasi profiilissa.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Näet, että WriteConsoleLog-funktio on käytetty aika 95 % alla kuvatulla tavalla. Tämä näyttää myös voit tarkka rivinumerot ja lähdetiedostot, jotka aiheuttavat ongelman.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Solmu-sovellus on käyttö liikaa muistia.

Näkyviin tulee todennäköisesti suositus Azure Webapps-portaalissa hyvin muistinkulutus tietoja. Voit määrittää myös näyttöjen odottaminen tietyt [arvot](web-sites-monitor.md). Muistinkäytön [Azure Portal Raporttinäkymät-ikkunan](../application-insights/app-insights-web-monitor-performance.md)tarkistettaessa Tarkista muistin Maks arvot niin, Huippu-arvot eivät jää huomaamatta.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Paljastaa tunnistaminen ja keon Diffing node.js varten 

[Solmun memwatch](https://github.com/lloyd/node-memwatch) avulla voi auttaa sinua tunnistamaan muistivuotoja.
Voit asentaa memwatch tavoin v8 Profilerin ja muokata koodisi sieppaus ja eroavuus heaps tunnistavan muistin vuodot-sovelluksessa.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Oma node.exe käytön lopetetaan satunnaisesti 

Muutamia syitä, miksi tämä saattaa olla näistä:

1.  Sovellus on yliheittävä aiheutetaan poikkeukset – Määritä valintaruudun d:\\home\\publish LogFiles\\sovelluksen\\kirjaaminen errors.txt tiedoston lisätietoja poikkeus. Tämä tiedosto on pinon jäljitys, joten voit korjata tämän perusteella sovelluksen.

2.  Sovellus on käyttö liikaa muistia, joka on vaikuttamatta muihin prosessit käytön aloittaminen. Jos koko AM muisti on lähellä 100 %, että node.exe voidaan lopettaa prosessin hallinnan kertoaksesi muita prosesseja, voit tehdä joitakin työt. Voit korjata ongelman, varmista, että sovelluksesi ei muistivuoto tai jos sovelluksen on todella paljon muistia käyttämään, valitse Skaalaa useampia RAM-Muistia jossa suuremmaksi AM.

### <a name="my-node-application-does-not-start"></a>Solmu-sovellus ei käynnisty

Jos sovelluksen palauttaa käynnistyksen 500 virheitä, voi olla useita syitä:

1.  Node.exe ei ole oikein sijainnissa. Tarkista nodeProcessCommandLine-asetus.

2.  Tärkeimmät komentosarjatiedosto ei ole oikein sijainnissa. Tarkista web.config ja varmista, että tärkeimmät komentosarjatiedosto käsittelytoimintoja-osan nimi vastaa tärkeimmät komentosarjatiedosto.

3.  Web.config määritys ei ole oikein – Tarkista nimet ja arvot-asetukset.

4.  Kylmän Käynnistä – sovelluksen kestää liian kauan käynnistys. Jos sovelluksen kestää kauemmin (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1 000 sekuntia iisnode palauttaa 500-virheen. Suurentaa näitä asetuksia sovelluksen aloitusaika estää iisnode aikakatkaistu ja 500 virhe, joka palauttaa arvot.

### <a name="my-node-application-crashed"></a>Solmun sovelluksessa kaatui

Sovellus on yliheittävä aiheutetaan poikkeukset – Määritä valintaruudun d:\\home\\publish LogFiles\\sovelluksen\\kirjaaminen errors.txt tiedoston lisätietoja poikkeus. Tämä tiedosto on pinon jäljitys, joten voit korjata tämän perusteella sovelluksen.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>Solmu-sovelluksen kestää liian kauan käynnistys (kylmän aloittaminen)

Yleisin syy tähän on, että sovellus on paljon tiedostoja solmun\_moduulit ja sovellus yrittää ladata nämä tiedostot yleensä käynnistyksen yhteydessä. Oletusarvoisesti, koska tiedostot sijaitsevat Azure Webapps verkkoresurssiin niin paljon tiedostojen lataaminen voi kestää jonkin aikaa.
Jotkin ratkaisut saavat tämän nopeammin ovat seuraavat:

1.  Varmista, että npm3 avulla voit asentaa oman moduulit on tasainen riippuvuus-rakenne ja riippuvuuksia ei kaksoiskappaleita.

2.  Kuitata yritä ladata oman solmu\_moduulit ja ladata kaikki moduulit käynnistyksen yhteydessä. Tämä tarkoittaa, että require('module') kutsu tulee tehdä, kun tarvitse sitä todella yrität käyttää moduulin funktion sisällä.

3.  Azure Webapps on nimeltään paikallisen välimuistin ominaisuus. Tämä toiminto kopioi sisältöä verkkoresurssista paikalliseen levyasemaan AM käyttöön. Koska tiedostot ovat paikallisia, solmun latausajasta\_moduulit on paljon aiempaa nopeammin. -Näissä [ohjeissa](../app-service/app-service-local-cache.md) kerrotaan, miten voit käyttää paikallisen välimuistin tarkemmin.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http-tila ja alitila

[Lähdetiedosto](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) on lueteltu kaikki mahdolliset tila/alitila yhdistelmä iisnode voi palauttaa virheen sattuessa.

Ottaa käyttöön sovelluksen Nähdäksesi win32-virhekoodin FREB (Varmista, että otat FREB vain tuotannon sivustoissa nopeuttamiseksi).

| Http-tila | HTTP-alitila | Mahdollinen syy?                                                                                                                                                                                            
|-------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
| 500         | 1 000           | Oli joitakin ongelman lähettämisestä IISNODE pyynnön – tarkistaminen Jos node.exe on. Node.exe voi olla kaatui käynnistyksen yhteydessä. Tarkista web.config kokoonpano virheet.                                                                                                                                                                                                                                                                                     |
| 500         | 1001           | -Win32Error 0x2 - sovellus ei vastaa URL-osoite. Tarkista URL-Osoitteen suorittamaan sääntöjä tai jos pika-sovelluksessa on määritetty oikein tiet. -Win32Error 0x6d – nimettyjen putkien on varattu – Node.exe ei hyväksy pyyntöjä, koska putkien on varattu. Tarkista suuren suorittimen käytön. -Muut virheet – Tarkista node.exe kaatui.
| 500         | 1002           | Node.exe kaatui – Tarkista d:\\home\\publish LogFiles\\kirjaaminen-errors.txt ja pinon seurantatiedot.                                                                                                                                                                                                                                                                                                                                                                                        |
| 500         | 1003           | Pipe määritysten ongelman – koskaan näkyviin tulee seuraava mutta tällöin nimettyjen putkien määritys on virheellinen.                                                                                                                                                                                                                                                                                                                                                          |
| 500         | 1004-1018      | Lähetettäessä pyynnön tai käsittelyn vastausta / node.exe on joitakin virhe. Tarkista, onko node.exe kaatui. Tarkista d:\\home\\publish LogFiles\\kirjaaminen-errors.txt ja pinon seurantatiedot.                                                                                                                                                                                                                                                                                    |
| 503         | 1 000           | Muisti ei riitä varata lisää nimetty putkien yhteyksiä. Miksi sovelluksen muissa paljon muistia valintaruutu. Tarkista maxConcurrentRequestsPerProcess arvon. Jos sen ei jatkuva ja pyynnöt on useita, Suurenna tätä arvoa voit estää tämän virheen.                                                                                                                                                                                                                                                                                                                  |
| 503         | 1001           | Pyyntö ei saa lähettää node.exe, koska sovelluksen kierrättäminen. Kun sovellus on kierrätetään, olisi served pyynnöt normaalisti.                                                                                                                                                                                                                                                                                                               |
| 503         | 1002           | Tarkista win32-virhekoodi todellinen syystä – pyyntö ei saa lähettää node.exe.                                                                                                                                                                                                                                                                                                                                                                               |
| 503         | 1003           | Nimettyjen putkien on varattu – Tarkista Jos solmu muissa suorittimen paljon                                                                                                                                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
Ei sisällä NODE.exe solmu nimeltä asetus\_odottaa\_PUTKIEN\_ESIINTYMÄT. Tämä arvo on oletusarvoisesti ulkopuolella azure webapps 4. Tämä tarkoittaa, että node.exe voi hyväksyä 4 pyynnöt vain nimettyjen putkien kerrallaan. Azure Webapps-arvoksi on määritetty 5000 ja tämän arvon pitäisi olla tarpeeksi hyvä useimmissa azure webapps solmu sovellusten. Sinun pitäisi näe 503.1003 azure webapps koska solmun on suuri arvo\_odottaa\_PUTKIEN\_ESIINTYMÄT.  |

## <a name="more-resources"></a>Lisää resursseja

Näistä linkeistä saat lisätietoja node.js sovellukset App Azure-palvelusta.

* [Azure-sovelluksen palvelun Node.js web App-sovellusten käytön aloittaminen](app-service-web-nodejs-get-started.md)
* [Voit korjata Node.js verkkosovellukseen Azure sovelluksen-palvelussa](web-sites-nodejs-debug.md)
* [Käyttämällä Node.js moduulit Azure-sovellusten kanssa](../nodejs-use-node-modules-azure-apps.md)
* [Azure App palvelun verkkosovelluksissa: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Developer Center](../nodejs-use-node-modules-azure-apps.md)
* [Huipputehokas salainen Kudu virheenkorjaus-konsolin tutustuminen](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)