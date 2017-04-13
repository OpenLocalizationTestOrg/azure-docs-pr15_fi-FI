<properties 
    pageTitle="Vianmääritys ja kysymyksiisi hakemuksen tiedot" 
    description="Valitse jokin Visual Studio hakemuksen tiedot epäselvä tai ei toimi? Kokeile tätä." 
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
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="questions---application-insights-for-aspnet"></a>Kysymykset - sovelluksen tietoa ASP.NET

## <a name="configuration-problems"></a>Määritysongelmia

*Minulla on ongelmia asetus Omat:*

* [.NET-sovellus](app-insights-asp-net-troubleshoot-no-data.md)
* [Sovelluksen jo käytön valvonta](app-insights-monitor-performance-live-website-now.md#troubleshooting)
* [Azure diagnostiikka](app-insights-azure-diagnostics.md)
* [Java web Appissa](app-insights-java-troubleshoot.md)
* [Muiden ympäristöjen](app-insights-platforms.md)

*Ei ole tietojen noutaminen palvelin*

* [Määritä palomuuripoikkeukset](app-insights-ip-addresses.md)
* [ASP.NET-palvelimen määrittäminen](app-insights-monitor-performance-live-website-now.md)
* [Java-palvelimen määrittäminen](app-insights-java-agent.md)


## <a name="can-i-use-application-insights-with-"></a>Voit käyttää sovelluksen havainnollistamisen tehostaminen...?

[Katso Ympäristöt][platforms]


## <a name="is-it-free"></a>Onko se maksuton?

* Kyllä, jos valitset vapaa [hinnoittelua taso](app-insights-pricing.md). Saat suurin osa toiminnoista ja tietojen generous kiintiön. 
* Sinun on annettava luottokortin tiedot rekisteröityä Microsoft Azure, mutta ei veloituksia tehdään, paitsi jos käytät toisen maksettu varten Azure palvelun tai erikseen päivitettävä maksetuksi taso.
* Jos sovelluksen lähettää enemmän tietoja kuukauden kiintiön vapaa tason, se pysäytetään lokiin. Jos näin tapahtuu, voit joko valitsemalla Aloita maksaa, tai odota, kunnes kiintiön palautetaan kuukauden lopussa.
* Käyttö- ja istunnon perustiedot eivät koske kiintiön:
* On myös maksuton 30 päivän kokeiluversio, aikana saat maksutta maksettu ominaisuuksia.
* Sovelluksen kullekin resurssille on erillinen kiintiön, ja voit määrittää sen hinnoittelu taso ulkopuolisista muihin palveluihin.

#### <a name="what-do-i-get-if-i-pay"></a>Mitä saan, jos voin maksaa?

* Suurempi [kuukauden kiintiön tiedot](https://azure.microsoft.com/pricing/details/application-insights/).
* Mahdollisuus maksaa 'overage' Jatka tietojen keräämisen kuukauden kiintiön päälle. Jos tietojen esitellään kiintiön, olet Peri Mt kohden.
* [Jatkuva Vie](app-insights-export-telemetry.md).


## <a name="q14"></a>Mitä sovelluksen havainnollistamisen muokata projektin?

Tiedot määräytyvät projektin tyyppi. Web-sovelluksen:


+ Lisää nämä tiedostot projektin:

 + ApplicationInsights.config. 
 + Ai.js


+ Asentaa NuGet pakkauksissa:

 -  *Hakemuksen tiedot API* - core Ohjelmointirajapinta

 -  *Sovelluksen tiedot-Ohjelmointirajapinnan verkkosovellusten* - lähettämiseen telemetriatietojen palvelimesta

 -  *Sovelluksen tiedot-Ohjelmointirajapinnan JavaScript-sovellusten* - käytettävä telemetriatietojen lähettäminen asiakkaalle

    Paketit ovat näistä kokoonpanoista:

 - Microsoft.ApplicationInsights

 - Microsoft.ApplicationInsights.Platform

+ Lisää kohteita:

 - Web.config

 - Packages.config

+ (Uusi projektit - vain, jos voit [lisätä sovelluksen tietoja aiemmin luotu projekti][start], sinun on tehtävä manuaalisesti.) Lisää katkelmat asiakkaan ja palvelimen-koodiksi valmistelu ne sovelluksen tiedot resurssin tunnus. Esimerkiksi MVC-sovelluksen koodi on lisätty perustyylisivun Views/Shared/_Layout.cshtml


## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Miten SDK aiempi päivittäminen?

Saat haluamasi sovellus tyypin SDK [julkaisutiedot](app-insights-release-notes.md) . 



## <a name="update"></a>Miten voin muuttaa mitä Azure resurssin projektin lähettää tietoja?

Napsauta ratkaisunhallinnassa, napsauta hiiren kakkospainikkeella `ApplicationInsights.config` ja valitse **Päivitä sovelluksen tiedot**. Voit lähettää tiedot olemassa oleva vai uusi resurssi Azure-tietokannassa. Ohjattu päivitys muuttuu ApplicationInsights.config, joka määrittää, missä SDK-palvelin lähettää tietoja instrumentation-näppäintä. Voit poistaa valinnan "Päivitä kaikki", ellei se muuttuu avain, jossa se näkyy verkkosivujen.


#### <a name="data"></a>Kuinka kauan tietoja säilytetään portaalissa? On suojattu?

Tutustu [tietojen säilytys- ja tietosuoja][data].

## <a name="logging"></a>Lokiin kirjaaminen

#### <a name="post"></a>Miten voin tarkistaa, Kirjaa tietoja diagnostiikan hakutoiminnossa?

Olemme kirjautumatta kirjaa tiedot automaattisesti, mutta voi käyttää TrackTrace puhelun: sijoittaa tiedot message-parametria. Pidempi enimmäiskoko kuin merkkijonon ominaisuudet-rajoituksia on mutta sitä ei voi suodattaa. 

## <a name="security"></a>Tietoturva

#### <a name="is-my-data-secure-in-the-portal-how-long-is-it-retained"></a>Ovatko tietoni suojatun portaalissa? Kuinka kauan se säilyttää?

Katso [tietojen säilytys- ja tietosuoja][data].


## <a name="q17"></a>Ottanut, voin kaikki hakemuksen tiedot ovat?

<table border="1">
<tr><th>Mitä pitäisi näkyä</th><th>Hankkiminen</th><th>Miksi haluat</th></tr>
<tr><td>Käytettävyys kaaviot</td><td><a href="../app-insights-monitor-web-app-availability/">Web-testiä</a></td><td>Tiedät web app on käytössä</td></tr>
<tr><td>Palvelimen sovelluksen perf: kertaa vastausta...
</td><td><a href="../app-insights-asp-net/">Hakemuksen tiedot lisääminen projektiin</a><br/>tai <br/><a href="../app-insights-monitor-performance-live-website-now/">Asenna AI tilan valvonta palvelimeen</a> (tai oman koodin kirjoittaminen jäljittämiseksi <a href="../app-insights-api-custom-events-metrics/#track-dependency">riippuvuudet</a>)</td><td>Perf virheiden havaitsemista</td></tr>
<tr><td>Riippuvuus telemetriatietojen</td><td><a href="../app-insights-monitor-performance-live-website-now/">Asenna AI tilan valvonta-palvelimeen</a></td><td>Tietokantojen tai muiden ulkoisten osien vianmääritys</td></tr>
<tr><td>Pyydä pinon jäljittää poikkeukset</td><td><a href="../app-insights-search-diagnostic-logs/#exceptions">Lisää TrackException puhelut koodissa</a> (mutta jotkin raportoidaan automaattisesti)</td><td>Haku- ja poikkeukset vianmääritys</td></tr>
<tr><td>Etsi lokin jäljittää</td><td><a href="../app-insights-search-diagnostic-logs/">Lokiin kirjaaminen-sovittimen lisääminen</a></td><td>Poikkeukset-perf ongelmien vianmääritys</td></tr>
<tr><td>Asiakkaan käytön perusteet: sivun näkymät, istunnot,...</td><td><a href="../app-insights-javascript/">Verkkosivujen JavaScript alustaja</a></td><td>Käyttöanalyysin</td></tr>
<tr><td>Asiakkaan mukautettua arvot</td><td><a href="../app-insights-api-custom-events-metrics/">Verkkosivujen tallenteita seuranta</a></td><td>Parantaa käyttökokemusta</td></tr>
<tr><td>Palvelimen mukautettua arvot</td><td><a href="../app-insights-api-custom-events-metrics/">Palvelimen koodin puhelut seuranta</a></td><td>Liiketoimintatiedot</td></tr>
</table>


## <a name="automation"></a>Automaatio

Voit [kirjoittaa PowerShell-komentosarjojen](app-insights-powershell.md) luominen ja Päivitä tiedot sovelluksen resurssit.

## <a name="more-answers"></a>Lisätietoja on artikkelissa

* [Sovelluksen tiedot-keskustelupalsta](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)


<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 