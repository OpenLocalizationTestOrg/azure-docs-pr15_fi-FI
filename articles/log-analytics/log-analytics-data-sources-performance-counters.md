<properties 
   pageTitle="Windows- ja Linux resurssilaskurit Log Analytics | Microsoft Azure"
   description="Suorituskyvyn laskureita kerätään Log Analytics analysointiin Windows ja Linux suorituskyvyn mukaan.  Tässä artikkelissa kerrotaan, miten määrittäminen sivustokokoelman suorituskyvyn laskureita sekä Windows ja Linux tekijöiden tietoja siitä, että ne on tallennettu OMS tietovaraston ja kohdasta analysoiminen OMS-portaalissa."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows- ja Linux suorituskyvyn tietolähteiden Log Analytics 

Windows ja Linux laskureita suorituskyvyn selventäminen suorituskyvyn laitteistosta, käyttöjärjestelmiä ja sovelluksia.  Lokitiedoston Analytics voi kerätä suorituskyvyn laskureita toistuvin väliajoin lisäksi yhdistäminen suorituskykytietoja pidempi termin analyysia varten ja raportointi analysointiin lähellä Real Time (NRT).

![Suorituskyvyn laskureita](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Suorituskyvyn laskureita määrittäminen

Määritä suorituskyvyn laskureita [Lokiasetukset Analytics-tiedot-valikko](log-analytics-data-sources.md#configuring-data-sources).

Kun määrität uuden OMS työtilan Windows- tai Linux suorituskyvyn laskureita, voit luoda nopeasti useita yleisiä laskureita on annettu.  Ne on lueteltu jokaisen vieressä oleva valintaruutu.  Varmista, että mitään laskureita alun perin luotavan ovat valittuina, ja valitse sitten **Lisää valitun suorituskyvyn laskureita**.

![Windowsin suorituskyky laskureita määrittäminen](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Ohjeita noudattamalla voit lisätä uuden Windowsin suorituskyky laskuri voi kerätä.

1. Kirjoita Muotoile *objekti (esiintymän) \counter*tekstikentässä laskuri nimi.  Kun alat kirjoittaa, näyttöön tulee Yleiset laskureita vastaavia luettelo.  Voit valita laskuri joko luettelosta tai kirjoita kuvaksi.  Voit myös palauttaa kaikki esiintymät tietyn laskuri määrittämällä *object\counter*. 
2. Valitse **+** tai painamalla **Enter** Lisää laskuri-luetteloon.
3. Kun lisäät laskuri, se käyttää 10 sekuntia oletusarvon sen **Ajan**.  Voit muuttaa tätä suurempi arvo, enintään 1 800 sekuntia (30 minuuttia), jos haluat pienentää kerätä suorituskykytietoja tallennustilan vaatimuksia.
4. Lopuksi lisääminen laskureita valitsemalla Tallenna määritykset näytön yläreunassa **Tallenna** -painiketta.

![Määritä Linux suorituskyvyn laskureita](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Ohjeita noudattamalla voit lisätä uuden Linux suorituskyvyn laskuri voi kerätä.

1. Oletusarvon mukaan kaikki määritysmuutoksia automaattisesti siirretty, kaikki agenttien vuoksi.  Linux agenttien vuoksi määritystiedoston lähetetään Fluentd tietojen kerääminen.  Jos haluat muokata tämän tiedoston manuaalisesti jokaisen Linux-agentti, poista sitten valinta *Käytä alla määritysten Linux-tietokoneissa*.
2. Kirjoita Muotoile *objekti (esiintymän) \counter*tekstikentässä laskuri nimi.  Kun alat kirjoittaa, näyttöön tulee Yleiset laskureita vastaavia luettelo.  Voit valita laskuri joko luettelosta tai kirjoita kuvaksi.  
2. Valitse **+** tai paina **Enter** laskuri lisääminen muita laskureita objektin luettelo.
3. Objektin kaikkien laskureita käyttää samaa **Ajan**.  Oletusarvo on 10 sekuntia.  Voit muuttaa tämä suurempi arvo, enintään 1 800 sekuntia (30 minuuttia), jos haluat pienentää kerätä suorituskykytietoja tallennustilan vaatimukset.
4. Lopuksi lisääminen laskureita valitsemalla Tallenna määritykset näytön yläreunassa **Tallenna** -painiketta.

## <a name="data-collection"></a>Tietojen kerääminen

Kirjaudu Analytics kerää kaikki määritetyn suorituskyvyn laskureita kaikki agenttien vuoksi, joissa on asennettuna laskuri määritetyn otoksen niiden väliajoin.  Tiedot yhdistetään ei ja perustietoja on käytettävissä kaikissa log haun näkymissä OMS tilauksen määrittämää ajaksi.


## <a name="performance-record-properties"></a>Suorituskyvyn tietueen ominaisuudet-valintaikkunassa

Suorituskyvyn tietueiden tyyppi on **Perf** ja sen ominaisuudet ovat seuraavassa taulukossa.

| Ominaisuus | Kuvaus |
|:--|:--|
| Tietokoneen         | Tietokone, jossa tapahtuma on kerätty. |
| CounterName      | Nimen suorituskyky laskuri |
| CounterPath      | Laskuri-lomakkeessa koko polku \\ \\ \<tietokone >\\object(instance)\\laskuri. |
| CounterValue     | Numeerinen arvo laskuri.  |
| EsiintymänNimi     | Tapahtuma-esiintymän nimi.  Jos esiintymä ei ole tyhjä. |
| Objektin nimi       | Suorituskyvyn objektin nimi |
| SourceSystem  | Tiedot on kerätty agentti tyyppi. <br> Yhdistä OpsManager – Windows-agentti, joko suoraan tai SCOM <br> Linux – kaikki Linux agenttien vuoksi  <br> AzureStorage – Azure diagnostiikka |
| TimeGenerated       | Päivämäärä ja kellonaika, tiedot on näyte. |


## <a name="sizing-estimates"></a>Arvioi koon muuttaminen

 Arvio 10 sekunnin välein, erityisesti laskuri-kokoelman on noin 1 Megatavu esiintymän päivässä kohti.  Voit arvioida tietyn laskuri kaava on seuraava tallennustilan vaatimuksia.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Lokitiedoston haut suorituskyvyn tietueisiin

Seuraavassa taulukossa on eri esimerkkejä log hakuja, jotka hakevat suorituskyvyn tietueita.

| Kyselyn | Kuvaus |
|:--|:--|
| Tyyppi = Perf | Kaikki suorituskykytietoja |
| Tyyppi = Perf tietokoneen = "Oma tietokone" | Kaikki suorituskykytietoja tietokoneesta |
| Tyyppi = Perf CounterName = "Nykyinen levyn-jono" | Tietyn laskuri kaikki suorituskykytietoja |
| Tyyppi = Perf (objektin nimi = suoritin) CounterName = "% suoritinaika" InstanceName = _Kokonais & #124; mittaa Avg(Average) kuin AVGCPU tietokoneella | Keskimääräinen suorittimen käyttö tietokoneissa |
| Tyyppi = Perf (CounterName = "% suoritinaika") & #124;  mittaa max(Max) tietokoneella | Suurin suorittimen käyttö tietokoneissa |
| Tyyppi = Perf nimi = looginen levy CounterName = "Nykyinen levyn jono" tietokoneen = "MyComputerName" & #124; mittaa Avg(Average) EsiintymänNimi mukaan | Keskimääräinen Nykyinen levyn jono määritetyn tietokoneen kaikki esiintymät koko |
| Tyyppi = Perf CounterName = "DiskTransfers/sec" & #124; mittaa percentile95(Average) tietokoneella | 95. prosenttipiste, levyn siirrot/Sec tietokoneissa |
| Tyyppi = Perf CounterName = "% suoritinaika" InstanceName = "_Kokonais" & #124; mittaa avg(CounterValue) tietokoneen aikavälillä 1 tunti | Tunnin välein keskiarvon suorittimen käyttö tietokoneissa |
| Tyyppi = Perf tietokoneen = "Oma tietokone-CounterName = % * InstanceName = _Kokonais & #124; mittaa percentile70(CounterValue) CounterName aikavälillä 1 tunti | Tunnin välein 70 prosenttipisteen jokaisen % tietyn tietokoneen prosentti laskuri. |
| Tyyppi = Perf CounterName = "% suoritinaika" InstanceName = "_Kokonais" (tietokoneen = "Oma tietokone") & #124; mittaa min(CounterValue) avg(CounterValue), percentile75(CounterValue), max(CounterValue) tietokoneen aikavälillä 1 tunti | Tunnin välein keskiarvo, vähintään, enintään ja 75 prosenttipiste suorittimen käyttö tietyn tietokoneen |

## <a name="viewing-performance-data"></a>Suorituskyvyn tietojen tarkasteleminen

Kun suoritat suorituskykytietoja lokitiedoston etsiminen, Näytä **loki** näkyy oletusarvoisesti.  Voit tarkastella tietoja graafisessa muodossa, valitse **arvot**.  Saat yksityiskohtaiset graafinen **+** laskuri-kohdan vieressä.  

![Kutistettu arvot-näkymä](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Jos olet valinnut aikavälin on 6 tuntia tai pienempi, kaavio päivitetään muutaman sekunnin välein.  Reaaliaikaiset tiedot näkyvät Vaaleansininen kaavio oikealla puolella.

![Arvot näkymä laajennettu ajantasaisten tietojen kanssa](media/log-analytics-data-sources-performance-counters/metricsexpanded.png)

Koota suorituskykytietoja log haussa on artikkelissa [tarvittaessa metrisillä kooste ja visualisoinnin OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [lokin haut](log-analytics-log-searches.md) tietolähteet ja ratkaisujen kerättyjen tietojen analysointia varten.  
- Kerättyjen tietojen vieminen [Power BI](log-analytics-powerbi.md) muita visualisointeja ja analysointia varten.