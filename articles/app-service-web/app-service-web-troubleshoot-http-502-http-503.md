<properties
    pageTitle="502 – virheellinen yhdyskäytävä aiheuttamista 503 palvelu ei ole käytettävissä virheitä | Microsoft Azure"
    description="502 – virheellinen yhdyskäytävä ja HTTP 503 – palvelu ei ole käytettävissä virheitä ylläpidettävä Azure App Service-sovellukseen liittyviä ongelmia."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="502 – virheellinen yhdyskäytävä 503 service ei ole käytettävissä, 503 virhe, virhe 502"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>HTTP-vianmääritys "502 – virheellinen yhdyskäytävä" ja "http 503 – palvelu ei ole käytettävissä" Azure web Apps-sovelluksissa

"502 – virheellinen yhdyskäytävä" ja "http 503 – palvelu ei ole käytettävissä" ovat yleisiä virheitä ylläpidettävä [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)-sovellukseen. Tämän artikkelin avulla voit tehdä näitä vianmäärityksen.

Jos tarvitset apua milloin tahansa tämän artikkelin, ota Azure asiantuntijoille [MSDN-Azure](https://azure.microsoft.com/support/forums/)ja Pinon ylivuoto-keskustelupalstoilla. Voit vaihtoehtoisesti myös jättää Azure tuki tapahtuma. Siirry [Azure tukisivustossa](https://azure.microsoft.com/support/options/) ja valitse sitten **Hae tuki**.

## <a name="symptom"></a>Oire

Kun selaat web App-sovellukseen, se palauttaa HTTP "502 – virheellinen yhdyskäytävä"-Virhe tai HTTP "http 503 – palvelu ei ole käytettävissä"-virhe.

## <a name="cause"></a>Syy

Tämä ongelma usein aiheuttaa sovellusten tason ongelmia, kuten:

-   pyynnöt kestää kauan
-   sovelluksen hyvin muistin/Suoritin
-   Sovellus kaatuu poikkeuksen vuoksi.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Vianmääritysohjeita "502 – virheellinen yhdyskäytävä" ja "http 503 – palvelu ei ole käytettävissä"-virheet

Vianmääritys voidaan jakaa kolmeen eri tehtävät tässä järjestyksessä:

1.  [Noudata ja sovelluksen toiminnan valvominen](#observe)
2.  [Tietojen kerääminen](#collect)
3.  [Ongelman vähentäminen](#mitigate)

[Palvelun Web sovellukset](/services/app-service/web/) asetuksilla voit eri kussakin vaiheessa.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. Noudata ja sovelluksen toimintaa seuranta

####    <a name="track-service-health"></a>Seuraa palvelun kuntoa

Microsoft Azure publicizes aina, kun on palvelun keskeytymisen tai suorituskyvyn heikkeneminen. Voit seurata palvelun kuntoa [Azure-portaalissa](https://portal.azure.com/). Lisätietoja on artikkelissa [seuraa palvelun kuntoa](../monitoring-and-diagnostics/insights-service-health.md).

####    <a name="monitor-your-web-app"></a>Valvoa web Appissa

Tämän asetuksen avulla voit selvittää, jos sovellus on ongelmia. Valitse web-sovelluksen sivu **pyynnöt ja virheet** -ruutu. **Arvo** -sivu näyttää kaikki arvot, voit lisätä.

Jotkin tietoja, joista haluat ehkä valvoa web App

-   Keskimääräinen muistin toimi määrittäminen
-   Keskimääräinen vastausajan
-   Suorittimen ajan
-   Muistin Työsarja
-   Palvelupyynnöt

![näytön web Appin kohti 502 – virheellinen yhdyskäytävä ja HTTP 503 – palvelu ei ole käytettävissä HTTP-virheiden ratkaiseminen](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Lisätietoja on artikkelissa:

-   [Web-sovellusten valvonta Azure sovelluksen-palvelussa](web-sites-monitor.md)
-   [Ilmoitusten vastaanottaminen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />
### <a name="2-collect-data"></a>2. kerää tietoja

####    <a name="use-the-azure-app-service-support-portal"></a>Azure App palvelun tuki-portaalin käyttäminen

Web Apps on katsomalla HTTP lokit, tapahtumalokien prosessin Vedostaa tai lisää web App-sovellukseen liittyviä ongelmia voidaan ratkaista. Voit avata tämän sekä tuki-portaalissa osoitteessa tiedot **http://&lt;sovelluksen nimi >.scm.azurewebsites.net/Support**

Azure sovellus palvelun tukee-portaalissa on kolme eri välilehdissä tukemaan käytetty vianmäärityksen vaihtoehto kolme vaihetta:

1.  Noudata nykyisen toiminta
2.  Analysoi diagnostiikka tietojen kerääminen ja suorittamalla valmiin analyzers
3.  Vähentäminen

Jos ongelma tapahtuu juuri nyt, valitse **Analysoi** > **Diagnostiikka** > **Vianmäärityksen nyt** diagnostiikan istunnon luominen, joka kerää HTTP kirjaa-katseluohjelman tapahtumalokit, muistin Vedostaa, PHP virhelokeja ja PHP raportin käsittely.

Kun tiedot on kerätty, se analysoi tietoja ja antaa sinulle HTML-raportti.

Siltä varalta, että haluat ladata tiedot oletusarvoisesti, se tallennettu D:\home\data\DaaS-kansiossa.

Lisätietoja Azure sovelluksen tuki-portaalissa on artikkelissa [Azure sivustojen sivuston Tukilaajennus uudet päivitykset](/blog/new-updates-to-support-site-extension-for-azure-websites).

####    <a name="use-the-kudu-debug-console"></a>Käytä Kudu virheenkorjaus-konsolin

Web Apps sisältyy virheenkorjaus-konsolin, joiden avulla voit virheenkorjaus, tutustuminen, ladata tiedostoja sekä JSON päätepisteet tietojen tarkasteleminen ympäristön. Tätä kutsutaan _Kudu konsolissa_ tai _Palvelujen ohjauksen hallinta Raporttinäkymät-ikkunan_ web App-ohjelman.

Voit käyttää tätä Raporttinäkymät-ikkunan valitsemalla linkki **https://&lt;sovelluksen nimi >.scm.azurewebsites.net/**.

Muutamia esimerkkejä, joka sisältää Kudu ovat seuraavat:

-   sovelluksen ympäristöasetuksia
-   log-muodossa
-   Diagnostiikan dump
-   Virheenkorjaus konsolin, jossa voit suorittaa PowerShellin cmdlet-komennot ja DOS peruskomennot.


Toisen Kudu hyödyllinen ominaisuus on, että siltä varalta, että sovelluksesi on yliheittävä ensimmäisen kokeilemaan poikkeukset, voit käyttää Kudu ja SysInternals-työkalun luominen muistin Procdump kirjoittaa. Nämä muistivedokset ovat tilannevedoksia prosessin ja usein auttaa sinua web-sovelluksen monimutkaisempia ongelmien vianmääritykseen.

Lisätietoja Kudu ominaisuuksista on artikkelissa [Azure sivustojen verkkotyökalut tärkeitä tietoja](/blog/windows-azure-websites-online-tools-you-should-know-about/).

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

 ![Käynnistä sovellus 502 – virheellinen yhdyskäytävä ja HTTP 503 – palvelu ei ole käytettävissä HTTP virheiden ratkaiseminen](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Voit hallita PowerShellin Azure koodiin. Lisätietoja on artikkelissa [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md).
