<properties
    pageTitle="Hidastaa web app suorituskyky-sovelluksen palvelun | Microsoft Azure"
    description="Tämän artikkelin avulla voit vianmääritys hidas web app suorituskykyongelmia Azure sovelluksen-palvelussa."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="Web app suorituskyvyn, hidas app-sovellus on hidas"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Azure App palvelun hidas web app suorituskyky-ongelmien vianmääritys

Tämän artikkelin avulla voit vianmääritys hidas web app suorituskykyongelmia [Azure sovelluksen](http://go.microsoft.com/fwlink/?LinkId=529714)-palvelussa.

Jos tarvitset apua milloin tahansa tämän artikkelin, ota Azure asiantuntijoille [MSDN-Azure](https://azure.microsoft.com/support/forums/)ja Pinon ylivuoto-keskustelupalstoilla. Voit vaihtoehtoisesti myös jättää Azure tuki tapahtuma. Siirry [Azure tukisivustossa](https://azure.microsoft.com/support/options/) ja valitse sitten **Hae tuki**.

## <a name="symptom"></a>Oire

Kun selaat web app-sivut latautuvat hitaasti ja joskus aikakatkaisu.

## <a name="cause"></a>Syy

Tämä ongelma usein aiheuttaa sovellusten tason ongelmia, kuten:

-   pyynnöt kestää kauan
-   sovelluksen hyvin muistin/Suoritin
-   Sovellus kaatuu poikkeuksen vuoksi.

## <a name="troubleshooting-steps"></a>Vianmääritysohjeita

Vianmääritys voidaan jakaa kolmeen eri tehtävät tässä järjestyksessä:

1.  [Noudata ja sovelluksen toiminnan valvominen](#observe)
2.  [Tietojen kerääminen](#collect)
3.  [Ongelman vähentäminen](#mitigate)

[Palvelun Web sovellukset](/services/app-service/web/) asetuksilla voit eri kussakin vaiheessa.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. Noudata ja sovelluksen toimintaa seuranta

#### <a name="track-service-health"></a>Seuraa palvelun kuntoa

Microsoft Azure publicizes aina, kun on palvelun keskeytymisen tai suorituskyvyn heikkeneminen. Voit seurata palvelun kuntoa [Azure-portaalissa](https://portal.azure.com/). Lisätietoja on artikkelissa [seuraa palvelun kuntoa](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Valvoa web Appissa

Tämän asetuksen avulla voit selvittää, jos sovellus on ongelmia. Valitse web-sovelluksen sivu **pyynnöt ja virheet** -ruutu. **Arvo** -sivu näyttää kaikki arvot, voit lisätä.

Jotkin tietoja, joista haluat ehkä valvoa web App

-   Keskimääräinen muistin toimi määrittäminen
-   Keskimääräinen vastausajan
-   Suorittimen ajan
-   Muistin Työsarja
-   Palvelupyynnöt

![web-sovelluksen suorituskyvyn seuranta](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Lisätietoja on artikkelissa:

-   [Web-sovellusten valvonta Azure sovelluksen-palvelussa](web-sites-monitor.md)
-   [Ilmoitusten vastaanottaminen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Näytön web päätepisteen tila

Jos käytössäsi on web app **standardin** hinnat taso, Web Apps-sovellusten avulla voit valvoa 2-3 Maantieteellisten sijaintien päätepisteet.

Päätepisteen seuranta määrittää web testien geo distributed sijainneista, joka testaa vastausajan ja käytettävyyttä web-URL-osoitteet. Testi suorittaa HTTP GET toiminnon web-URL-osoitetta määrittävät vastausajan ja käytettävyyttä kunkin sijainnista. Jokainen määritetty sijainti suorittaa testin viiden minuutin välein.

Toiminta-aika on seurattava HTTP vastauksen merkkikoodeja käyttämällä ja vastausajan mitataan millisekunteina. Seurannan testi epäonnistuu, jos HTTP-vastauksen koodi on suurempi tai yhtä suuri 400 tai vastauksen kestää yli 30 sekuntia. Päätepisteen pidetään käytettävissä, jos sen seurantaa testien onnistu tiettyihin sijainteihin.

Määritä se kyseisessä kohdassa [miten: web päätepisteen tilaa](web-sites-monitor.md#webendpointstatus).

Katso myös [pitäminen Azure Web sivustojen määrittäminen plus päätepisteen seurantaa - Teemun tilanne Schackow](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) video päätepisteen seuranta.

#### <a name="application-performance-monitoring-using-extensions"></a>Sovelluksen suorituskyvyn seurantaa tunnisteesta käyttäminen

Voit myös valvoa sovelluksen suorituskyvyn hyödyntäminen _sivuston tunnisteet_.

Kunkin sovelluksen palvelun web Appissa on extensible hallinta loppupisteen merkki, jonka avulla voit hyödyntää joukon tehokkaita työkaluja kuin sivuston laajennukset käyttöön. Nämä työkalut väliltä lähde koodin editors [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) kuten yhdistetyn resurssit, kuten MySQL-tietokantaan yhdistetty verkkosovellukseen hallintatyökalut.

[Azure-sovelluksen tiedot](/services/application-insights/) ja [Uuden Relic](/marketplace/partners/newrelic/newrelic/) on kaksi suorituskyvyn sivuston laajennukset, jotka ovat käytettävissä. Voit käyttää uuden Relic asentamalla agentti suorituksen aikana. Käyttämään Azure sovelluksen tiedot SDK koodin uudelleen ja voit myös asentaa tunniste, joka sisältää tiedot. SDK voit kirjoittaa koodia seurannassa käyttö- ja suorituskyvyn sovelluksesi tarkemmin.

Hakemuksen tiedot on artikkelissa [verkkosovellusten suorituskyvyn seuranta](../application-insights/app-insights-web-monitor-performance.md).

Uusi Relic on artikkelissa [Uuden Relic sovellusten suorituskykyä hallinta-Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />
### <a name="2-collect-data"></a>2. kerää tietoja

####    <a name="enable-diagnostics-logging-for-your-web-app"></a>Ota käyttöön vianmäärityksen kirjaus käyttöön web Appissa

Online-ympäristö on diagnostiikan lokitiedot verkkopalvelin ja web-sovelluksen ominaisuudet. Nämä ovat loogisesti jaettu web server diagnostiikka- ja sovelluksen vianmääritys.

##### <a name="web-server-diagnostics"></a>Web-palvelimen diagnostiikka

Voit ottaa käyttöön tai poistaminen käytöstä lokit seuraavanlaisia:

-   **Yksityiskohtainen virhelokit** - yksityiskohtaisia virhetietoja HTTP tilakoodit, jotka ilmaisevat epäonnistui (tilakoodi 400 tai uudempi). Tämä saattaa sisältää tietoja, voit selvittää, miksi palvelin palautti virhekoodi.
-   **Epäonnistui pyynnön jäljitys** - epäonnistuneiden pyyntöjen, mukaan lukien jäljityksen IIS-osien avulla pyynnön ja aika, jokaisen osan yksityiskohtaiset tiedot. Tämä voi olla hyödyllinen, jos yrität web app suorituskyvyssä tai eristää mikä aiheuttaa tietyn HTTP-virhe.
-   **Web-palvelimen lokiin kirjaaminen** - W3C laajennettu log-tiedostomuotoa käytettäessä HTTP-tapahtumien tiedot. Tästä on hyötyä määritettäessä yleistä web app arvot, kuten pyynnöt käsitellään tai kuinka monta pyynnöt ovat IP-osoite.

##### <a name="application-diagnostics"></a>Sovelluksen vianmääritys

Sovelluksen Diagnostiikan avulla voit siepata web-sovelluksen tuottamat tiedot. ASP.NET-sovellukset voivat käyttää `System.Diagnostics.Trace` luokan lokitiedot sovelluksen diagnostiikka lokiin.

Lisätietoja kirjaamisen hakemukseen määrittämisestä on artikkelissa [Ota lokiin kirjaaminen web Apps-sovellukset Azure App palvelun diagnostiikka](web-sites-enable-diagnostic-log.md).

#### <a name="use-remote-profiling"></a>Käytä Remote profilointi

Azure sovelluksen käytössä Web Apps-Ohjelmointirajapinnan sovellukset ja WebJobs voi olla etäyhteyden sopivat. Jos prosessin toimii oikein, hitaammin tai pyyntöjen viive on suurempi kuin tavallinen ja prosessin suorittimen käyttö on myös suuri, voit etäyhteyden profiilin prosessin ja saada suorittimen esimerkkejä puhelun pinoa voit analysoida tehtävään ja koodin kuuma polkuja.

Saat lisätietoja, [Azure App palvelun Remote profilointi tuki](/blog/remote-profiling-support-in-azure-app-service).


#### <a name="use-the-azure-app-service-support-portal"></a>Azure App palvelun tuki-portaalin käyttäminen

Web Apps on katsomalla HTTP lokit, tapahtumalokien prosessin Vedostaa tai lisää web App-sovellukseen liittyviä ongelmia voidaan ratkaista. Voit avata tämän sekä tuki-portaalissa osoitteessa tiedot **http://&lt;sovelluksen nimi >.scm.azurewebsites.net/Support**

Azure sovellus palvelun tukee-portaalissa on kolme eri välilehdissä tukemaan käytetty vianmäärityksen vaihtoehto kolme vaihetta:

1.  Noudata nykyisen toiminta
2.  Analysoi diagnostiikka tietojen kerääminen ja suorittamalla valmiin analyzers
3.  Vähentäminen

Jos ongelma tapahtuu juuri nyt, valitse **Analysoi** > **Diagnostiikka** > **Vianmäärityksen nyt** diagnostiikan istunnon luominen, joka kerää HTTP kirjaa-katseluohjelman tapahtumalokit, muistin Vedostaa, PHP virhelokeja ja PHP raportin käsittely.

Kun tiedot on kerätty, se analysoi tietoja ja antaa sinulle HTML-raportti.

Siltä varalta, että haluat ladata tiedot oletusarvoisesti, se tallennettu D:\home\data\DaaS-kansiossa.

Lisätietoja Azure sovelluksen tuki-portaalissa on artikkelissa [Azure sivustojen sivuston Tukilaajennus uudet päivitykset](/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Käytä Kudu virheenkorjaus-konsolin

Web Apps sisältyy virheenkorjaus-konsolin, joiden avulla voit virheenkorjaus, tutustuminen, ladata tiedostoja sekä JSON päätepisteet tietojen tarkasteleminen ympäristön. Tätä kutsutaan _Kudu konsolissa_ tai _Palvelujen ohjauksen hallinta Raporttinäkymät-ikkunan_ web App-ohjelman.

Voit käyttää tätä Raporttinäkymät-ikkunan valitsemalla linkki **https://&lt;sovelluksen nimi >.scm.azurewebsites.net/**.

Muutamia esimerkkejä, joka sisältää Kudu ovat seuraavat:

-   sovelluksen ympäristöasetuksia
-   log-muodossa
-   Diagnostiikan dump
-   Virheenkorjaus konsolin, jossa voit suorittaa PowerShellin cmdlet-komennot ja DOS peruskomennot.


Toisen Kudu hyödyllinen ominaisuus on, että siltä varalta, että sovelluksesi on yliheittävä ensimmäisen kokeilemaan poikkeukset, voit käyttää Kudu ja SysInternals-työkalun luominen muistin Procdump kirjoittaa. Nämä muistivedokset ovat tilannevedoksia prosessin ja usein auttaa sinua web-sovelluksen monimutkaisempia ongelmien vianmääritykseen.

Lisätietoja Kudu ominaisuuksista on artikkelissa [Azure sivustojen Team Services-Työkalut tärkeitä tietoja](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3. ongelman vähentäminen

####    <a name="scale-the-web-app"></a>Skaalaa web Appissa

Azure App palvelun suorituskyky ja siirtonopeuden, voit säätää mittakaava, jossa sovellus on käynnissä. Skaalaus web Appin kuuluu kaksi niihin liittyvien toimintojen: muuttaminen App palvelusopimus suurempi hinta taso ja tiettyjen asetusten määrittäminen kun olet vaihtanut suurempi hinta taso.

Lisätietoja Skaalaus-kohdassa [asteikko Azure-sovelluksen palvelun verkkosovellukseen](web-sites-scale.md).

Lisäksi voit suorittaa sovelluksen useamman kuin yhden esiintymän. Tämä paitsi Lisää käsittely-ominaisuus löytyy minkä lisäksi saat myös joitakin vikasietoa määrä. Jos prosessin siirtyy yhden esiintymän, toinen jatkaa edelleen käsittelevä pyynnöt.

Voit määrittää skaalaus Manuaalinen tai automaattinen.

####    <a name="use-autoheal"></a>Käytä AutoHeal

AutoHeal kierrättää työntekijä-prosessi, kun sovellus asetukset (kuten määritysmuutoksia, ominaisuuksia, muistin perustuva rajoitukset tai pyynnön suorittamiseen tarvittava aika) perusteella. Yleensä-Roskakorin prosessi on nopein tapa palauttaa virheen. Vaikka voit käynnistää uudelleen aina web-sovelluksen suoraan Azure-portaalissa, AutoHeal valita automaattisesti puolestasi. Kaikki sinun on suoritettava on joitakin käynnistimien lisääminen web-sovelluksen pääkansion web.config. Huomaa, että nämä asetukset toimivat samalla tavalla, vaikka sovellus ei ole .net jokin.

Lisätietoja on artikkelissa [Automaattinen kenties Azure-sivustoja](/blog/auto-healing-windows-azure-web-sites/).

####    <a name="restart-the-web-app"></a>Web-sovelluksen käynnistäminen uudelleen

Tämä on usein helpoin tapa palauttaminen erikseen ongelmat. [Azure Portal](https://portal.azure.com/)-web-sovelluksen sivu sinulla Pysäytä tai Käynnistä sovellus uudelleen.

 ![Käynnistä Ratkaise suorituskykyongelmia web Appissa](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Voit hallita PowerShellin Azure koodiin. Lisätietoja on artikkelissa [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md).
