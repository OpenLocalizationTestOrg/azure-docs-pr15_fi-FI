<properties
    pageTitle="Azure App palvelussa sovellusten valvonta"
    description="Lisätietoja Azure-sovelluksen palvelun sovellusten valvonta mukaan Azure-portaalissa."
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="byvinyal"/>

# <a name="how-to-monitor-apps-in-azure-app-service"></a>Toimintaohje: Azure App palvelun sovellusten valvonta

[Sovelluksen-palvelu](http://go.microsoft.com/fwlink/?LinkId=529714) sisältää rakennettu seurantaa toimintoja [Azure-portaalissa](https://portal.azure.com).
Tämä on mahdollista Tarkista **kiintiön** ja **arvot** sovelluksen sekä sovelluksen Service-palvelupaketti, määrittämällä **ilmoitukset** ja jopa **Skaalaus** nämä arvot automaattisesti perusteella.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Tietoja kiintiöiden ja arvot

### <a name="quotas"></a>Kiintiön

Sovelluksen palvelun ylläpidettävä sovellukset ovat resurssit, he voivat käyttää tietyt *rajoitukset* . Rajat on määritetty liittyvä sovellus **App palvelusopimus** .

Jos sovellus nykyisessä **vapaa** tai **jaettu** suunnitelmassa, sovellus käyttää resurssien rajoitusten on määritetty **kiintiön**mukaan.

Jos sovelluksen isännöidään **Basic**, **Vakio** - tai **Premium** -palvelupaketin sitten rajoituksia he voivat käyttää resursseja määritetään **koon** (pieni, Normaali, suuri) ja **esiintymän määrä** (1, 2, 3,...) **sovelluksen palvelusopimus**.

**Vapaa** - tai **Jaetut** sovellukset **kiintiöt** ovat seuraavat:

* **CPU(short)**
   * Tämän sovelluksen 3 minuutin aikavälin sallittu suorittimen määrä. Tämän kiintiön määrittää uudelleen 3 minuutin välein.
* **CPU(Day)**
   * Tämän sovelluksen päivän sallittu suorittimen määrä. Tämän kiintiön määrittää uudelleen 24 tunnin välein keskiyön UTC-palvelussa.
* **Muisti**
   * Tämän sovelluksen sallittu muistin määrän.
* **Kaistanleveyden**
   * Lähtevän postin kaistanleveyden sallittu tämän sovelluksen päivän määrä.
   Tämän kiintiön määrittää uudelleen 24 tunnin välein keskiyön UTC-palvelussa.
* **Tiedostojärjestelmässä**
   * Tallennustilan määrä.

Sovellusten isännöimät **Basic**, **Vakio** ja **Premium** -palvelupaketin koskevat vain kiintiö on **tiedostojärjestelmässä**.

Lisätietoja tietyn kiintiön, rajoitukset ja eri App Service-tuotteissa käytettävissä olevia toimintoja löytyy tähän: [Azure tilauksen palvelun rajoitukset](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Kiintiön käyttäminen

Jos sovelluksen sen käyttö ylittää **suorittimen (lyhyt)**, **Suoritin (päivä)**tai **kaistanleveyden** kiintiö sitten sovellus lopetetaan ennen kuin kiintiön määrittää uudelleen. Tänä aikana kaikkia pyynnöt johtaa **HTTP 403**.
![][http403]

Jos sovelluksen **muistiin** kiintiö on ylitetty-sovellus on aloitettu uudelleen.

Jos **tiedostojärjestelmän** kiintiö on ylitetty, valitse jokin kirjoittaa epäonnistuu, mukaan lukien lokit kirjoittaminen.

Kiintiön voit korottaa tai poistaa sovellus napsauttamalla sovelluksen palvelusopimus päivitetään.

### <a name="metrics"></a>Arvot

**Arvot** on tietoja sovelluksen tai sovelluksen palvelun suunnitelman toiminta.

**Sovelluksen**käytettävissä olevat arvot ovat:

* **Keskimääräinen vastausajan**
   * Käsittele pyyntöjä ms-sovelluksen keskimäärin kuluva aika.
* **Keskimääräinen muistin toimi määrittäminen**
   * Sovelluksen käyttämät MiBs muistin keskimääräinen määrä.
* **Suorittimen ajan**
   * Suorittimen ja sekunteina sovelluksessa Kulutettu määrä. Lisätietoja tästä metrisillä on kohdassa: [suorittimen aika ja suorittimen prosentteina](#cpu-time-vs-cpu-percentage)
* **Tietoja**
   * Saapuvan postin MiBs sovelluksen kaistanleveyttä määrä.
* **Tietojen**
   * Lähtevän MiBs sovelluksen kaistanleveyttä määrä.
* **HTTP-2xx**
   * Tuloksena on http-tilakoodin pyyntöjä > = 200, mutta < 300.
* **HTTP-3xx**
   * Tuloksena on http-tilakoodin pyyntöjä > = 300, mutta < 400.
* **HTTP 401**
   * Tuloksena on HTTP 401 tilakoodin pyyntöjen määrä.
* **HTTP 403**
   * Tuloksena on HTTP 403 tilakoodin pyyntöjen määrä.
* **HTTP 404**
   * Tuloksena on HTTP 404 tilakoodin pyyntöjen määrä.
* **HTTP 406**
   * Tuloksena on HTTP 406 tilakoodin pyyntöjen määrä.
* **HTTP-4xx**
   * Tuloksena on http-tilakoodin pyyntöjä > = 400, mutta < 500.
* **HTTP-palvelin-virheet**
   * Tuloksena on http-tilakoodin pyyntöjä > = 500, mutta < 600.
* **Muistin Työsarja**
   * Sovelluksen MiBs käyttämän muistin määrän.
* **Palvelupyynnöt**
   * Riippumatta siitä, niiden tuloksena oleva HTTP-tilakoodin pyyntöjen kokonaismäärä.

**Sovelluksen palvelusopimus**käytettävissä olevat arvot ovat:

>[AZURE.NOTE] Sovelluksen palvelun suunnitelman arvot ovat käytettävissä vain Palvelupaketit **Basic**, **Vakio** ja **Premium** tuote.

* **Suorittimen prosentti**
   * Suunnitelman kaikki esiintymät käytettävä suorittimen keskiarvo.
* **Muistin prosentti**
   * Keskimääräinen muistia käytettävä suunnitelman kaikki esiintymät.
* **Tietoja**
   * Keskimääräinen saapuvien kaistanleveys käytettävä suunnitelman kaikki esiintymät.
* **Tietojen**
   * Lähtevien suunnitelman kaikki esiintymät koko kaistanleveyden keskiarvon.
* **Keskiarvo**
   * Määrän keskiarvo sekä lukeminen ja kirjoittaminen jonossa olleet pyynnöt-tallennustilan. Suuri keskiarvo on maininta sovellus, joka voi hidastaa liiallinen levyn i/o vuoksi.
* **HTTP-jono**
   * HTTP-pyyntöjen, joka oli ennen täyty jonossa istut keskimääräinen määrä. Suuri tai kasvava HTTP jonon pituuden on Oire kuormitettu suunnitelma.

### <a name="cpu-time-vs-cpu-percentage"></a>Suorittimen ja suorittimen prosenttiosuuden
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

On 2 arvot, jotka vastaavat suorittimen käyttö. **Suorittimen ajan** ja **suorittimen prosentti**

**Suorittimen ajan** on hyötyä ylläpidettävä **vapaa** tai **jaetun** suunnitelmia, koska jokin niiden kiintiöiden on määritetty sovellus käyttää suorittimen minuutteina sovellukset.

Toisaalta **suorittimen prosentti** on hyötyä ylläpidettävä **basic**, **Vakio** ja **premium** -palvelupaketin jälkeen voi skaalata ja tämä arvo on hyvä maininta yleinen käyttö kaikki sovellukset.

##<a name="metrics-granularity-and-retention-policy"></a>Arvot rakeisuuden ja säilytyskäytäntö

Sovelluksen ja sovelluksen palvelusopimus mittaukset kirjautunut ja -palvelu sisältää seuraavat tarkkuudet ja säilytyskäytännöt koostetun:

 * **Minuutti** rakeisuuden arvot säilyvät **48** tuntia
 * **Tunti** rakeisuuden arvot säilyvät **30** päivän
 * **Päivä** rakeisuuden arvot säilyvät **90** päivää

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Seuranta kiintiöiden ja arvot Azure-portaalissa.

Voit tarkastella eri **kiintiön** ja **arvot** vaikuttavia [Azure Portal](https://portal.azure.com)-sovellus.

![][quotas]
**Kiintiön** löytyy, valitse Asetukset >**kiintiön**. UX avulla voit tarkastella: kiintiön (1) nimi, (2) sen Palauta aikaväli, (3) sen nykyisen ja (4) nykyisen arvon.

![][metrics]
**Arvot** voidaan käyttää suoraan resurssi-sivu. Voit mukauttaa kaavion mukaan: (1) **napsauttamalla** sitä ja valitse (2) **Muokkaa kaavion**.
Tässä voit muuttaa **aikavälin**(3), (4) **Kaaviolaji**ja näyttämiseen (5)- **arvot** .  

Voit lukea lisää seuraavat arvot: [näytön palvelun arvot](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Ilmoitusten ja Automaattinen skaalaus
Arvot, sovelluksen tai sovelluksen palvelupaketissasi taustapuolen enintään ilmoitukset, Lisätietoja on artikkelissa [Ilmoitusten vastaanottaminen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Ylläpidettävä basic, Normaali tai premium App palvelun suunnitelmien App Service-sovellukset tukevat **Automaattinen skaalaus**. Näin voit määrittää sääntöjä, jotka valvoa App palvelun suunnitelman arvot ja voit suurentaa tai pienentää lisäresursseja tarjoaa tarvittaessa esiintymän määrä tai tallentaminen rahaa, kun sovellus on liiallinen säännöstä. Voit lukea lisää tässä Automaattinen skaalaus: [kuinka asteikko](../monitoring-and-diagnostics/insights-how-to-scale.md) ja tähän [Azure näytön automaattisen skaalauksen poistaminen parhaat käytännöt](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="whats-changed"></a>Mikä on muuttunut
* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
