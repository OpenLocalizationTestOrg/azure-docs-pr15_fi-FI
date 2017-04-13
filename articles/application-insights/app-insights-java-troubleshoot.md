<properties 
    pageTitle="Vianmääritys sovelluksen havainnollistamisen Java-web-projekti" 
    description="Vianmääritysoppaan – seuranta live Java-sovellusten kanssa hakemuksen tiedot." 
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
    ms.date="03/01/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Vianmääritys ja kysymysten ja vastausten for Javan sovelluksen tietoa

Kysymyksiä tai ongelmia [Visual Studio hakemuksen tiedot Java][java]? Seuraavassa on muutamia vinkkejä.


## <a name="build-errors"></a>Muodosta virheet

*Saan Pimennys, kun lisäät sovelluksen havainnollistamisen SDK maven-testi tai Gradle, muodosta tai tarkistussumma tarkistusvirheitä.*

* Jos riippuvuuden <version> elementti on käytössä kuvion yleismerkkien kanssa (esimerkiksi (maven-testi) `<version>[1.0,)</version>` tai (Gradle) `version:'1.0.+'`), yritä määrittää tietyn version sijaan kuten `1.0.2`. Katso [julkaisutiedot](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) uusimman version.

## <a name="no-data"></a>Ei tietoja 

*Voin lisättiin hakemuksen tiedot ja suoritettiin sovelluksen, mutta avaamani koskaan tietojen portaalissa.*

* Odota hetki ja valitse sitten Päivitä. Kaavioiden päivittää itse säännöllisesti, mutta voit myös päivittää manuaalisesti. Päivitysvälin määräytyy kaavion aikaväli.
* Tarkista, että sinulla on määritetty ApplicationInsights.xml-tiedostossa (-projektin resurssit-kansiossa) instrumentation-näppäin
* Varmista, että on ei ole `<DisableTelemetry>true</DisableTelemetry>` solmu xml-tiedoston.
* Palomuurin voit joutua avaa TCP-portit 80 ja 443 lähtevän tietoliikenteen dc.services.visualstudio.com avulla. Katso [Täysi luettelo palomuuripoikkeukset](app-insights-ip-addresses.md)
* Kirjoita Microsoft Azure tauluun, tarkista palvelun tila kartan. Jos luettelossa on joitakin ilmoitusten merkintöjä, odota, kunnes ne on palautettu OK ja valitse Sulje ja avaa se uudelleen sovelluksen tiedot-sovellusta-sivu.
* Kirjaamisen ottaminen käyttöön IDE console-ikkuna, lisäämällä `<SDKLogger />` pääkansion solmun ApplicationInsights.xml tiedoston (projektin resurssit-kansio) ja tapahtumat etuliitteenä on [virhe] Tarkista elementti.
* Varmista, että oikea ApplicationInsights.xml-tiedosto on onnistuneesti ladattu Java SDK-paketissa, tarkistamalla konsolin tulosteen viestien "kokoonpanotiedosto on onnistuneesti havaittu"-lauseen.
* Jos config-tiedostoa ei löydy, tarkista nähdäksesi, jossa määritystiedoston on haettava tulostus-viestit ja varmista, ApplicationInsights.xml sijaitsee johonkin näistä sijainneista. Säännön määrä-napin kuin voit siirtää määritystiedoston lähellä hakemuksen tiedot SDK JARs. Esimerkki: Tomcat, valitse se tarkoittaa WEB-INF/lib-kansio.



#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Voin käyttää tiedot, mutta se on lopetettu

* Valitse [tila-blogi](http://blogs.msdn.com/b/applicationinsights-status/).
* On osumien kuukauden kiintiön arvopisteiden? Avaa asetukset/kiintiön ja hinnoittelu selvittää. Jos näin on, voit päivittää palvelupaketin tai muita kapasiteetin maksaa. Katso [hinnat värimallin](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-the-data-im-expecting"></a>En näe kaikkia I 'M puuttuu tietoja

* Avaa kiintiöiden ja hinnat sivu ja tarkista, onko [Esimerkkejä](app-insights-sampling.md) on toiminnassa. (100 %: n siirto tarkoittaa, että kun otos, jota ei ole toiminnassa.) Sovelluksen tiedot-palvelua voi määrittää hyväksymään vain murto-osa, joka saapuu sovelluksestasi telemetriatietojen. Näin voit pitää kuukausittain kiintiö on telemetriatietojen. 

## <a name="no-usage-data"></a>Ei käyttötietoja

*Tietoja pyynnöt ja kertaa vastausta, mutta ei sivunäkymän, selaimen tai käyttäjätiedot näkyy.*

Onnistuneesti määrität sovelluksen lähettämään telemetriatietojen palvelimesta. Nyt seuraava vaihe on [lähettää telemetriatietojen selaimella verkkosivujen määrittäminen][usage].

Voit myös onko asiakkaan sovelluksen [Puhelin tai muu laite][platforms], voit lähettää telemetriatietojen sieltä. 

Määrittää asiakkaan ja palvelimen telemetriatietojen saman instrumentation avaimen avulla. Tiedot näkyvät saman hakemuksen tiedot resurssin ja voit tarvittaessa tapahtumien asiakkaan ja palvelimen yhdistäminen.



## <a name="disabling-telemetry"></a>Telemetriatietojen poistaminen käytöstä

*Miten voin vaimentaa telemetriatietojen sivustokokoelman?*

Valitse koodi:

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);


**Tai** 

Päivitä ApplicationInsights.xml (-projektin resurssit-kansio). Lisää pääkansio solmun seuraavasti:

    <DisableTelemetry>true</DisableTelemetry>

XML-menetelmällä on Käynnistä sovellus uudelleen, kun arvo muuttuu.

## <a name="changing-the-target"></a>Kohteen muuttaminen

*Miten voin muuttaa mitä Azure resurssin projektin lähettää tietoja?*

* [Pyydä uusi resurssi instrumentation-näppäintä.][java]
* Jos valitset hakemuksen tiedot lisätään Azure-työkalujen käyttäminen Pimennys, projektin oikealle web-projekti, valitse **Azure**, **Määritä sovellus tiedot**, ja muuta avain.
* Muussa tapauksessa päivitä avain ApplicationInsights.xml projektin resurssit-kansiossa.


## <a name="the-azure-start-screen"></a>Azure aloitusnäyttö

*Etsimääni [Azure-portaalissa](https://portal.azure.com)osoitteessa. Kartan kertoo jotain sovelluksen?*

Ei, se näyttää Azure-palvelimien kuntotietojen eri puolilla maailmaa.

*FROM Azure Käynnistä board (aloitusnäytön) Mistä löydän tiedot sovelluksen tietoja?*

Olettaen [että sovelluksen tiedot-sovelluksen määrittäminen][java], valitsemalla Selaa, valitse sovelluksen tiedot ja valitse luomasi, kun sovellus app resurssi. Saat siellä nopeammin tulevaisuudessa, voit kiinnittää tauluun Käynnistä sovellus.

## <a name="intranet-servers"></a>Intranet-palvelimet

*Palvelimen valvoa Omat intranetissä*

Kyllä, antaa palvelimesi lähettää telemetriatietojen sovelluksen tiedot-portaaliin julkisen Internetin kautta. 

Palomuurin voit joutua avaa TCP-portit 80 ja 443 lähtevän tietoliikenteen dc.services.visualstudio.com ja f5.services.visualstudio.com.

## <a name="data-retention"></a>Tietojen säilytys 

*Kuinka kauan tietoja säilytetään portaalissa? On suojattu?*

Katso, [tietojen säilytys- ja tietosuoja][data].

## <a name="next-steps"></a>Seuraavat vaiheet

*Hakemuksen tiedot määritetään for Javan palvelimen sovelluksen. Mitä muuta voin tehdä?*

* [Valvoa verkkosivujen saatavuus][availability]
* [Näytön verkkosivun käyttö][usage]
* [Seurata käyttöä ja laitteen sovelluksia vianmääritys][platforms]
* [Voit seurata sovelluksen käyttö koodin kirjoittaminen][track]
* [Sieppaa vianmäärityslokit][javalogs]


## <a name="get-help"></a>Ohjeiden saaminen

* [Pinon ylivuoto](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 