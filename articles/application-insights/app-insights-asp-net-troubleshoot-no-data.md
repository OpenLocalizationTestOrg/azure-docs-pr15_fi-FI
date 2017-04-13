<properties 
    pageTitle="Tietoja - sovelluksen tietoa .NET vianmääritys" 
    description="Näe tietojen havainnollistamisen Visual Studio-sovelluksen? Kokeile tätä." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Tietoja - sovelluksen tietoa .NET vianmääritys

## <a name="some-of-my-telemetry-is-missing"></a>Jotkin Oma telemetriatietojen puuttuu

*Sovelluksen tiedot-näy vain tapahtumat, jotka ovat luomaa sovelluksen murtoluku.*

* Jos näe johdonmukaisesti saman nimittäjä, syynä on todennäköisesti mukautuvat [Esimerkkejä](app-insights-sampling.md). Voit vahvistaa tämän avata haun (yhteenveto-sivu) ja katsomalla esiintymän pyyntö tai muu tapahtuma. Valitse "..." koko ominaisuuden tietoja ominaisuudet-osan alareunassa. Jos pyytää Laske > 1 ja valitse esimerkkejä on toiminnassa. 
* Muussa tapauksessa on mahdollista, että sinulla on pallolla [tietojen määrä rajoittaa](app-insights-pricing.md#limits-summary) hinnoittelu-palvelupakettiisi. Nämä raja-arvot käytetään minuutissa.

## <a name="no-data-from-my-server"></a>Palvelin ei ole tietoja

*Sovelluksen asennuksen web-palvelimelle ja kaikki telemetriatietojen sen näkyvistä. Se toimi OK keskihajonta omassa tietokoneessa.*

* Todennäköisesti palomuurin ongelma. [Määritä palomuuripoikkeukset sovelluksen tietoja Lähetä tietoa](app-insights-ip-addresses.md).

*Voin [tilan valvonta asennettuna](app-insights-monitor-performance-live-website-now.md) Omat web-palvelin aiemmin sovellusten valvonta. En näe tuloksia.*

* Katso [vianmääritys tilan valvonta](app-insights-monitor-performance-live-website-now.md#troubleshooting). 


## <a name="q01"></a>Visual Studio ei "Lisää sovellus tiedot"-vaihtoehto

*Kun uuden projektin luominen Visual Studiossa tai kun voin aiemmin luotu projekti on ratkaisunhallinnassa hiiren kakkospainikkeella, en näe sovelluksen tiedot-valinnat.*

+ Kaikki tyypit .NET project tukee työkaluja. Web- ja WCF projektit ovat tuettuja. Muut projektityypit, kuten työpöytä- tai sovellukset voit silti [lisätä sovelluksen tiedot-SDK projektiin manuaalisesti](app-insights-windows-desktop.md).
+ Varmista, että sinulla on [Visual Studio 2013-päivitys 3 tai uudempi](http://go.microsoft.com/fwlink/?LinkId=397827). Se esiasennukseen kuuluu sovelluksen tiedot-työkalut.
+ Valitse **Työkalut**- **laajennukset ja päivitykset** ja tarkista **Hakemuksen tiedot Työkalut** on asennettu ja otettu käyttöön. Jos näin on, valitse **päivitykset** , onko päivitys käytettävissä.
+ Avaa uusi projekti-valintaikkunassa ja valitse ASP.NET Web-sovelluksen. Jos sovelluksen tiedot-vaihtoehto on näkyvissä, työkalut on asennettu. Jos et, yritä poistaminen ja asentaa sovelluksen tiedot-Työkalut uudelleen.


## <a name="q02"></a>Ei voitu lisätä hakemuksen tiedot

*Virheilmoitus uusi web-projekti luodessasi tai kun yritän hakemuksen tiedot lisätään aiemmin luodusta projektista*

Todennäköisiä syitä:

* Viestintä sovelluksen tiedot-portaalissa epäonnistui. tai
* Jotkin ongelma Azure tilisi;
* Sinulla on vain [lukuoikeus-tilauksen tai ryhmä, johon yritit luoda uusi resurssi](app-insights-resources-roles-access-control.md).

Korjaus:

+ Tarkista kirjautumisen tunnistetiedot säädetty oikealle Azure-tili. 
+ Selaimessa Tarkista, että access [Azure portal](https://portal.azure.com). Avaa asetukset ja tarkista, onko mitään rajoituksia.
+ [Lisää sovellus havainnollistamisen luotu projekti](app-insights-asp-net.md): Valitse ratkaisunhallinnassa hiiren kakkospainikkeella projektin ja valitse "Lisää sovellus-tiedot".
+ Jos näppäimistö ei edelleenkään toimi, toimi [Manuaalinen toimintosarjan](app-insights-windows-services.md) SDK lisääminen projektiin ja Lisää resurssi-portaalissa. 

## <a name="emptykey"></a>Saan virhesanoman "Instrumentation avain ei voi olla tyhjä"

Näyttää siltä, että järjestelmässä tapahtui virhe, kun asennetaan hakemuksen tiedot tai ehkä kirjaaminen sovittimen.

Napsauta ratkaisunhallinnassa, napsauta hiiren kakkospainikkeella `ApplicationInsights.config` ja valitse sitten **Määritä sovelluksen tiedot**. Näyttöön tulee valintaikkuna, joka kutsuu voit kirjautua Azure ja Luo sovelluksen tiedot-resurssi, tai Käytä aiemmin luotua uudelleen.


##<a name="NuGetBuild"></a>"NuGet paketit ovat puuttuu" Muodosta-palvelimessa

*Kaikki muodostaa OK, kun I 'M virheenkorjaus kehittäminen omassa tietokoneessa, mutta näyttöön tulee NuGet virhe muodosta-palvelimeen.*

Katso [NuGet paketin Palauta](http://docs.nuget.org/Consume/Package-Restore) ja [Automaattinen paketin Palauta](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Puuttuvat valikkokomento Avaa sovelluksen havainnollistamisen Visual Studio

*Kun voin projektin ratkaisunhallinnassa hiiren kakkospainikkeella minkä tahansa sovelluksen tiedot-komennot eivät näy tai avaa sovelluksen tiedot-komento ei näy.*

Todennäköisiä syitä:

* Jos loit sovelluksen tiedot resurssin manuaalisesti tai jos projektin tyyppi, joka ei tue sovelluksen tiedot-työkalut.
* Visual Studio sovelluksen tiedot-työkalut eivät ole käytettävissä.
* Visual Studio on vanhempi kuin 2013-päivitys 3.

Korjaus:

* Varmista, että Visual Studio-versio on 2013-päivitys 3 tai uudempi.
* Valitse **Työkalut**- **laajennukset ja päivitykset** ja tarkista **Hakemuksen tiedot Työkalut** on asennettu ja otettu käyttöön. Jos näin on, valitse **päivitykset** , onko päivitys käytettävissä.
* Napsauta hiiren kakkospainikkeella, napsauta ratkaisunhallinnassa projektin. Jos **Määrittäminen sovelluksen tiedot**-komento on näkyvissä, voit käyttää sitä projektin muodostaa resurssin sovelluksen tiedot-palvelussa.


Muussa tapauksessa projektityyppi ei tue suoraan sovelluksen tiedot-työkalut. Voit tarkastella telemetriatietojen-kirjautuminen [Azure portal](https://portal.azure.com), valitse vasemmanpuoleisesta siirtymispalkista sovelluksen tiedot ja valitse sovellus.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>"Käyttö estetty"-sovelluksen tiedot avaaminen Visual Studio

*"Avaa sovelluksen tiedot-komento on minulle Azure-portaaliin, mutta saan käyttö estetty-virhesanoman.*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

Microsoft-kirjautuminen, jota käytit viimeksi oletusselaimessa ei ole [, joka luotiin, kun sovellus tiedot on lisätty sovelluksen resurssin](app-insights-asp-net.md)käyttöoikeutta. On todennäköistä kahdesta syystä: 

* Sinulla on useita Microsoft-tili - ehkä työ- ja henkilökohtainen Microsoft-tili? Kirjaudu sisään, jota käytit viimeksi oletusselaimeksi on eri tilin kuin näkymän, jolla on pääsy [lisätä hakemuksen tiedot projektiin](app-insights-asp-net.md). 

 * Korjaus: Valitse nimesi etsiminen ensimmäiset selaimen ikkunan oikealla puolella ja kirjaudu ulos. Kirjaudu sisään tilille, jolla on käyttöoikeudet. Valitse vasemman reunan siirtymispalkissa, valitse sovelluksen tiedot ja valitse sovelluksen.

* Toinen käyttäjä on lisännyt sovelluksen havainnollistamisen projektiin ja ne unohtanut avulla voit [käyttää resurssiryhmän](app-insights-resources-roles-access-control.md) , jossa se on luotu. 

 * Korjaus: Jos ne on organisaatiotili, he voivat lisätä voit ryhmän; tai he voivat myöntää yksittäisiä oikeudet resurssiryhmä.



## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>'Resurssi ei löydy"sovelluksen havainnollistamisen avaaminen Visual Studio

*"Avaa sovelluksen tiedot-komento on minulle Azure-portaaliin, mutta saan virheilmoituksen"resurssi ei löydy".*

Todennäköisiä syitä:

* Sovelluksen sovelluksen tiedot-resurssi on poistettu; tai
* Instrumentation-näppäintä on määritetty tai uudistettu ApplicationInsights.config muokkaamalla sitä suoraan päivittämättä projektitiedoston. 

ApplicationInsights.config ohjausobjektit, jossa telemetriatietojen lähetetään instrumentation avain. Projektitiedoston rivin ohjausobjektit, mikä resurssi avautuu, kun käytät komentoa Visual Studiossa. 

Korjaus:

* Napsauta ratkaisunhallinnassa projektin hiiren kakkospainikkeella ja valitse sovelluksen tiedot, määrittää sovelluksen tiedot. Valitse valintaikkunan joko voit lähettää telemetriatietojen aiemmin luotu resurssi tai luoda uuden. Tai:
* Avaa resurssi suoraan. Kirjaudu [Azure-portaaliin](https://portal.azure.com), valitse vasemmanpuoleisesta siirtymispalkista sovelluksen tiedot ja valitse sitten sovellus.



## <a name="where-do-i-find-my-telemetry"></a>Missä oma telemetriatietojen?

*Voin kirjautunut [Microsoft Azure-portaalin](https://portal.azure.com)ja Azure koti Raporttinäkymät-ikkunan katsoo. Niin missä tietoni hakemuksen tiedot?*

* Valitse vasemman reunan siirtymispalkissa sovellusten tiedot ja valitse sovelluksen nimi. Jos sinulla ei ole projekteja siellä, joudut [lisääminen tai määrittäminen sovelluksen web projektin tiedot](app-insights-asp-net.md).

    Näet on joitakin yhteenvetokaaviot. Voit valita avulla ne yksityiskohdat.

* Visual Studio samalla, kun olet virheenkorjaus sovelluksen, valitse sovelluksen tiedot-painiketta.


## <a name="q03"></a>Palvelin ei ole tietoja (tai ei lainkaan tietoja)

*Voin suoritettiin sovellukseni ja avataan sitten Microsoft Azure-sovelluksen tiedot-palvelu, mutta kaikki kaaviot näkyvät "Lue, miten voit kerätä..." tai "Ei ole määritetty."* Vaihtoehtoisesti *vain sivunäkymän ja käyttäjän tiedot, mutta ei palvelintietojen.*

+ Suorita sovellus virheenkorjaus tilassa Visual Studiossa (F5). Sovelluksen siten, että jotkin telemetriatietojen Luo avulla. Tarkista, että näet tapahtumien Visual Studio kohde-ikkunassa. 

    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)

+ Avaa [Diagnostiikan haku](app-insights-diagnostic-search.md)sovelluksen tiedot-portaalissa. Tietoja yleensä tässä näkyy ensin.
+ Napsauta Päivitä-painiketta. Sivu päivittää itse säännöllisesti, mutta voit myös tehdä sen manuaalisesti. Päivitysvälin on pidempi suurempi aikavälin.
+ Tarkista instrumentation näppäimet vastaavat. Katsomalla tärkeimmät sivu, valitse sovelluksen tiedot-portaalissa **Essentials** avattavasta, kun sovellus **Instrumentation-näppäintä**. Valitse projektin Visual Studiossa, Avaa ApplicationInsights.config ja Etsi `<instrumentationkey>`. Tarkista, että avaimet ovat yhtä. Jos et:
 + Portaalissa sovelluksen tiedot ja Etsi sovellus resurssin oikean avaimella; tai
 + Visual Studio ratkaisunhallinnassa projektin hiiren kakkospainikkeella ja valitse sovelluksen tiedot, Määritä. Palauta telemetriatietojen lähettäminen oikean resurssin sovellus.
 + Jos et löydä vastaava näppäimet, tarkista, että käytät samoja kirjautumisen tunnistetietoja kuin Visual Studiossa portaaliin.

    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
    
+ [Microsoft Azure Raporttinäkymät-ikkunan aloitus](https://portal.azure.com)Tarkista palvelun kunto-kartta. Jos luettelossa on joitakin ilmoitusten merkintöjä, odota, kunnes ne on palautettu OK ja valitse Sulje ja avaa se uudelleen sovelluksen tiedot-sovellusta-sivu.
+ Tarkista myös [tila-blogi](http://blogs.msdn.com/b/applicationinsights-status/).
+ Tiesitkö kirjoittaa koodia varten [palvelinpuolen SDK-paketissa](app-insights-api-custom-events-metrics.md) , joka saattaa muuttua instrumentation avain `TelemetryClient` esiintymiä tai `TelemetryContext`? Tiesitkö kirjoittaminen [suodattimen tai esimerkkejä](app-insights-api-filtering-sampling.md) , jotka voivat suodattaa, liikaa?
+ Jos olet muokannut ApplicationInsights.config, Tarkista huolellisesti [TelemetryInitializers](app-insights-api-filtering-sampling.md)ja TelemetryProcessors määritykset. Virheellisesti nimetty tyyppi tai parametria voi aiheuttaa SDK: ssa voit lähettää tietoja.

## <a name="q04"></a>Selaimet, Page Views käyttö ei ole tietoja

*Näen palvelimen vastausajan ja palvelimen pyynnöt tiedot, mutta ei tietoja sivun näkymän latausajasta tai selaimen tai käyttö näiden.*

Tiedot tulevat verkkosivujen komentosarjoja. 

+ Jos olet lisännyt sovelluksen tiedot aiemmin luotuun web projektiin, [sinun on lisättävä komentosarjat käsin](app-insights-javascript.md).
+ Varmista, että Internet Explorer ei näy sivuston yhteensopivuustilassa.
+ Selaimen virheenkorjaus-toiminnon (Joissain selaimissa F12 Valitse verkon sitten) avulla voit varmistaa, että tiedot lähetetään `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Riippuvuus tai poikkeuksen tietoja

Katso [riippuvuuden telemetriatietojen](app-insights-asp-net-dependencies.md) ja [poikkeuksen telemetriatietojen](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Ei ole käytettävissä

Suorituskykytietoja (suorittimen, IO korko ja niin edelleen) on käytettävissä [Java-verkkopalvelut](app-insights-java-collectd.md), [Windows-työpöytäsovellusten](app-insights-windows-desktop.md), [IIS-WWW-sovelluksia ja palveluja, jos asennat tilan valvonta](app-insights-monitor-performance-live-website-now.md)ja [Azure pilvipalveluihin](app-insights-azure.md). löydät sen asetukset-palvelimiin.

Ei ole käytettävissä Azure sivustot.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>(Palvelin) ei ole tietoja, koska palvelin on julkaistu sovellus

+ Tarkista, että kopioimasi todella on Microsoft. ApplicationInsights dll-palvelimeen, ja Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
+ Palomuurin voit joutua avaamaan [joitakin TCP-portit](app-insights-ip-addresses.md#data-access-api).
+ Jos sinun on käytettävä välityspalvelimen lähettämään ulos yrityksen verkossa, määrittää [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) Web.config
+ Windows Server 2008: Varmista, että olet asentanut seuraavat päivitykset: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523) [KB2600217](https://support.microsoft.com/kb/2600217).


## <a name="i-used-to-see-data-but-it-has-stopped"></a>Voin käyttää tiedot, mutta se on lopetettu

* Valitse [tila-blogi](http://blogs.msdn.com/b/applicationinsights-status/).
* On osumien kuukauden kiintiön arvopisteiden? Avaa asetukset/kiintiön ja hinnoittelu kieliominaisuuksien. Jos näin on, voit päivittää palvelupaketin tai muita kapasiteetin maksaa. Katso [hinnoittelua värimallin](https://azure.microsoft.com/pricing/details/application-insights/).


## <a name="i-dont-see-all-the-data-im-expecting"></a>En näe kaikkia I 'M puuttuu tietoja

Jos sovelluksesi lähettää tietojen tarkastelun ja käytät sovelluksen tiedot-SDK ASP.NET-version 2.0.0-beta3 tai uudempi versio, [mukautuvat esimerkkejä](app-insights-sampling.md) -ominaisuus saattaa toimia ja Lähetä vain oman telemetriatietojen prosentteina. 

Voit poistaa sen käytöstä, mutta se ei ole suositeltavaa. Esimerkkejä on suunniteltu siten, että liittyvät telemetriatietojen oikein siirretään, vianmääritystä varten. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Väärä maantieteellisten tietojen käyttäjän telemetriatietojen

Kaupunki, alue ja maa-mitat johdettuja IP-osoitteet ja ei aina tarkkoja.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Poikkeus "menetelmä ei löydy" suorittamisesta Azure Cloud Services-palveluissa

Luot .NET 4.6 varten? 4.6 ei tueta automaattisesti Azure pilvipalveluihin roolit. [Asenna 4.6 kunkin roolin](../cloud-services/cloud-services-dotnet-install-dotnet.md) ennen sovelluksen suorittamista.

## <a name="still-not-working"></a>Edelleenkään onnistu...

* [Sovelluksen tiedot-keskustelupalsta](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

