Tietyt rajoitukset arvot ja sovellusta kohden tapahtumien määrä on (eli kohti instrumentation avain). 

Rajoitukset määräytyvät sen mukaan, [hinnat taso](https://azure.microsoft.com/pricing/details/application-insights/) , voit valita haluamasi.

**Resurssi** | **Oletusarvoinen enimmäismäärä** | **Enimmäismäärä**
-------- | ------------- | -------------
Istunnon arvopisteet<sup>1, 2</sup> kuukautta kohti | rajoittamaton tallennus | 
Yhteensä arvopisteiden kuukaudessa pyynnön, tapahtuma, riippuvuuden, seuranta, poikkeuksen ja sivunäkymän | 5 miljoonaa | 50 miljoonaa<sup>3</sup>
[Jäljitä ja kirjaudu](../articles/application-insights/app-insights-search-diagnostic-logs.md) tietojen määrä | 200 dp/s | 500 dp/s
[Poikkeuksen](../articles/application-insights/app-insights-asp-net-exceptions.md) tiedot korko | 50 dp/s | 50 dp/s
Tietojen laskeminen yhteen korko pyynnön, tapahtuma, riippuvuuden ja sivun näkymän telemetriatietojen | 200 dp/s | 500 dp/s
[Etsi](../articles/application-insights/app-insights-diagnostic-search.md) ja [Analytics](../articles/application-insights/app-insights-analytics.md) raaka tietojen säilytys | 7 päivää
Koostetun tietojen säilytys [arvot](../articles/application-insights/app-insights-metrics-explorer.md) Explorerin | 90 päivää
[Ominaisuuden](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) nimi määrä | 100 |
Ominaisuuden nimen pituus | 150 | 
Ominaisuuden arvo pituus | 8192 | 
Jäljitys- ja poikkeuksen viestin pituus | 10000 |
[Metrijärjestelmän](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) nimi määrä | 100 |
Metrijärjestelmän nimen pituus |  150 | 
[Käytettävyys testit](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> arvopisteitä on yksittäisiä Kustannusarvo tai tapahtuman liitetty ominaisuudet ja mitat.

<sup>2</sup> A istunnon arvopiste kirjautuu alussa tai lopussa istunnon ja kirjaa käyttäjätiedot.

<sup>3</sup> voit ostaa lisää kapasiteetti yli 50 miljoonaa.
 
[Hinnat ja sovelluksen tiedot-kiintiön](../articles/application-insights/app-insights-pricing.md)
