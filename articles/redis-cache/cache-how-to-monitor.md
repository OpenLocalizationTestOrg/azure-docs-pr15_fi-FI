<properties 
    pageTitle="Voit valvoa Azure Redis välimuistin | Microsoft Azure" 
    description="Lue, miten voit valvoa terveys- ja suorituskyvyn Azure Redis välimuistin esiintymiä" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-monitor-azure-redis-cache"></a>Voit valvoa Azure Redis välimuisti

Azure Redis välimuistin on useita asetuksia seurannasta välimuistiesiintymät. Voit Näytä arvot, kiinnittäminen arvot kaaviot Startboard, mukauttaa päivämäärän ja kellonajan seuranta kaavioiden solualue, lisääminen ja arvot poistaminen kaaviot ja määrittämään ilmoituksia, kun tietyt ehdot täyttyvät. Kyseisten työkalujen avulla voit tarkkailla Azure Redis välimuistin esiintymät ja välimuistiin sovellusten hallinnan avulla.

Kun välimuistin diagnostiikka ovat käytössä, Azure Redis välimuistin esiintymät mittaukset kerääminen noin 30 sekuntia ja tallennetaan, jotta ne näkyvät arvot-kaavioiden ja tulkinnut ilmoitusten säännöt.

Välimuistin arvot kerätään käyttämällä Redis.txt [INFO](http://redis.io/commands/info) -komentoa. Lisätietoja eri tiedot kunkin käytettävät arvot välimuistin arvo on artikkelissa [käytettävissä olevat arvot ja raportoinnin väliajoin](#available-metrics-and-reporting-intervals).

Voit tarkastella välimuistin mittarit, [Selaa](cache-configure.md#configure-redis-cache-settings) välimuisti-esiintymän [Azure portal](https://portal.azure.com). Azure Redis välimuistin esiintymät mittaukset on käyttää **Redis arvot** -sivu.

![Redis arvot][redis-cache-redis-metrics-blade]

>[AZURE.IMPORTANT] Jos seuraava sanoma näkyy **Redis arvot** -sivu, [Ota käyttöön välimuistin diagnostiikka](#enable-cache-diagnostics) -osan, jotta välimuistin diagnostiikka ohjeiden mukaisesti.
>
>`Monitoring may not be enabled. Click here to turn on Diagnostics.`

**Redis arvot** -sivu on **Seuranta** -kaavioita, jotka esittävät välimuistin arvot. Kunkin kaavion voi mukauttaa lisäämällä tai poistamalla arvot ja muuttamalla raportoinnin aikaväli. Tarkasteleminen ja määrittäminen toiminnot ja ilmoitukset- **Redis välimuisti** -sivu on **Toiminnot** -osa, joka sisältää **tapahtumia** välimuistista ja **ilmoitusten säännöt**.

## <a name="enable-cache-diagnostics"></a>Ota käyttöön välimuistin diagnostiikka

Azure Redis välimuistin on diagnostiikka-tiedot, jotka on tallennettu tallennustilan tilillä, jotta voit käyttää mitä tahansa työkaluja, jota haluat käyttää ja käsitellä tietoja suoraan. Jotta välimuistin diagnostiikka kerätään tallennettu, ja näkyviin Azure-portaalissa tallennustilan tilin on oltava määritettynä. Tallentaa saman alue-ja tilauksen jakaminen saman diagnostiikka-tallennustilan tilin ja kun määritykset on muutettu toiminto kaikki tilauksen tallentaa, jotka ovat alueen.

Ottaa käyttöön ja määrittää välimuistin Diagnostiikka, siirry välimuistin esiintymän **Redis välimuisti** -sivu. Jos Diagnostiikka eivät ole vielä käytössä, viesti näkyy sijaan vianmäärityksen kaavioon.

![Ota käyttöön välimuistin diagnostiikka][redis-cache-enable-diagnostics]

Napsauta viestiä **metrijärjestelmä** sivu ja valitse **Diagnostiikan asetusten** ottaminen käyttöön ja välimuisti-palvelun esiintymän diagnostiikan asetusten määrittäminen.

![Diagnostiikan asetusten][redis-cache-diagnostic-settings]

![Diagnostiikan asetusten määrittäminen][redis-cache-configure-diagnostics]

**Valitse** -painiketta käyttöön välimuistin diagnostiikka ja diagnostiikka-määritys näyttää.

Valitse Valitse tallennustilan-tilin säilyttämään diagnostiikan tietojen **Tallennustilan** tilin oikealla puolella olevaa nuolta. Saat parhaan suorituskyvyn Valitse tallennustilan tilin sama kuin välimuistin alue.

Kun diagnostiikan asetukset on määritetty, valitse Tallenna määritykset **Tallenna** . Huomaa, että voi kestää hetken, jotta muutokset tulevat voimaan.

>[AZURE.IMPORTANT] Tallentaa saman alue-ja tilauksen yhteiset diagnostiikka tallennustilan asetukset, ja kun määritykset on muutettu (diagnostiikka käytössä/poissa käytöstä tai muuttamalla tallennustilan-tili) se koskee kaikki tilauksen tallentaa, jotka ovat alueen.

Tallennetun arvot katselemista tutkia tallennustilan tilin nimet, jotka alkavat taulukot `WADMetrics`. Lisätietoja Azure portaalin ulkopuolella tallennetut arvot käyttämisestä on artikkelissa [Access Redis välimuistin seuranta](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) näyte.

>[AZURE.NOTE] Azure-portaalissa näkyvät vain ne arvot, jotka on tallennettu valitun tallennustilan tilin. Jos muutat tallennustilan tilit, aiemmin määritetyn tallennustilan tilin tiedot pysyvät ladattavissa, mutta se ei ole näkyvissä Azure-portaalissa.  

## <a name="available-metrics-and-reporting-intervals"></a>Käytettävissä olevat arvot ja aikavälit raportointi

Välimuistin arvot raportoidaan käyttämällä useita raportoinnin välein, esimerkiksi **kuluneen tunnin** **tänään**, **edellisen viikon**ja **Mukautettu**. Arvot kunkin kaavion **metrijärjestelmä** -sivu näyttää kunkin metrijärjestelmän keskiarvo, pienimpien ja suurimpien arvojen kaaviossa ja jotkin arvot näyttää summan raportoinnin aikavälin. 

Kunkin arvo sisältää kaksi versiota. Yksi arvo mittaa koko välimuistin ja tallentaa, jotka käyttävät [klusterointi](cache-how-to-premium-clustering.md), arvo, joka sisältää toisen version suorituskyky `(Shard 0-9)` nimi mitat suorituskyvyn yksittäisen shard välimuistiin varten. Esimerkiksi jos välimuisti on 4 shards `Cache Hits` on koko välimuistin osumien kokonaismäärä ja `Cache Hits (Shard 3)` on vain kyseisen shard välimuistin osumia.

>[AZURE.NOTE] Vaikka välimuistin olisi käyttämättömänä yhdistetyn active asiakassovellusten kanssa, näkyviin voi tulla joitakin välimuistin toimintojen, kuten asiakkaiden, muistinkäytön ja toiminnoista. Tämä toiminto on Normaali Azure Redis välimuistin esiintymän toiminnon aikana.

| Arvo            | Kuvaus                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Osumia        | Onnistuneiden avaimen hakujen raportoinnin määritetyn aikavälin aikana määrä. Tämä yhdistää `keyspace_hits` Redis.txt [tiedot](http://redis.io/commands/info) -komento.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Välimuistin epäonnistumisia      | Epäonnistuneiden avaimen haut raportoinnin määritetyn aikavälin aikana määrä. Tämä yhdistää `keyspace_misses` Redis tiedot-komento. Välimuistin epäonnistumisia välttämättä merkitse on välimuistin ongelmasta. Esimerkiksi käytettäessä välimuistin kesantoala ohjelmoinnin kuvio-sovellus näyttää ensimmäisen kohteen välimuistin. Jos kohde ei ole (välimuistin puuttuva), kohde on haettu tietokannasta ja lisätään välimuistista seuraavan kerran. Välimuistin epäonnistumisia ovat normaalia välimuistin kesantoala ohjelmoinnin kuvion. Jos välimuistin epäonnistumisten määrä on suurempi kuin oikein, tarkistamalla, joka täyttää ja lukee välimuistista sovelluksen logiikkaa. Jos kohteet ovat parhaillaan poistaa muistinkäyttöön vuoksi välimuistissa sitten ehkä joitakin välimuistin epäonnistumisten, mutta voit valvoa muistinkäyttöön paremmin metrijärjestelmä olisi `Used Memory` tai `Evicted Keys`. |
| Asiakkaiden | Välimuistin raportoinnin määritetyn aikavälin aikana asiakasyhteydet määrä. Tämä yhdistää `connected_clients` Redis tiedot-komento. Kun [yhteyksien enimmäismäärä](cache-configure.md#default-redis-server-configuration) on saavutettu yhteyttä yritykset välimuistiin epäonnistuu. Huomaa, että vaikka ei ole aktiivinen asiakassovellus, voi edelleen asiakkaiden sisäiset prosessit ja yhteyksien vuoksi muutaman esiintymät.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Poistaa näppäimet      | Poistaa välimuistista vastaanottaja raportoinnin määritetyn aikavälin aikana kohteiden määrää `maxmemory` rajoitus. Tämä yhdistää `evicted_keys` Redis tiedot-komento.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Vanhentuneen näppäimet      | Kohteiden määrän vanhentunut välimuistista raportoinnin määritetyn aikavälin aikana. Tämän arvon yhdistää `expired_keys` Redis tiedot-komento.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Saa              | Hae toimintojen välimuistista raportoinnin määritetyn aikavälin aikana määrä. Tämä arvo on summa seuraavat arvot Redis tiedot kaikki: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, ja `cmdstat_getrange`, ja vastaa osumien ja epäonnistumisten raportoinnin aikavälin aikana.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Redis lataaminen palvelimeen | Prosenttiosuus, jossa Redis.txt palvelin on varattu käsitteleminen ja odottaa ei käytetä viestien jaksot. Jos laskuri saavuttaa 100, se tarkoittaa sitä, Redis.txt palvelin on osumien suorituskyvyn enimmäismäärän ja Suoritin ei voi käsitellä toimi jokin nopeammin. Jos näe hyvin Redis palvelimen kuormituksen kellokuvakkeesta saa näkyviin aikakatkaisu poikkeukset-asiakassovelluksessa. Tässä tapauksessa kannattaa harkita skaalaus- tai tietojen jakaminen useiden tallentaa kyselyjä.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Joukot              | Määritä toimintojen välimuistiin raportoinnin määritetyn aikavälin aikana määrä. Tämä arvo on summa seuraavat arvot Redis tiedot kaikki: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange`, ja `cmdstat_setnx`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Toimintojen summa  | Välimuistin palvelin käsittelee raportoinnin määritetyn aikavälin aikana komennot määrä. Tämän arvon yhdistää `total_commands_processed` Redis tiedot-komento. Huomaa, että kun Azure Redis välimuistin käytetään laskettuna kirja ja sub ei ole mittaukset `Cache Hits`, `Cache Misses`, `Gets`, tai `Sets`, mutta ole `Total Operations` arvot, jotka vastaavat välimuistin käytön kirja ja sub-toimintoja.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Käytetty muistin       | Välimuistin käytettyä avain/arvo-pareina megatavuina välimuistin raportoinnin määritetyn aikavälin määrä. Tämän arvon yhdistää `used_memory` Redis tiedot-komento. Ei ole metatiedot tai pirstoutuminen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Käytetty muistin RSS   | Raportoinnin tietyn aikavälin, mukaan lukien pirstoutuminen ja metatietojen megatavuina käytettyä välimuisti määrä. Tämän arvon yhdistää `used_memory_rss` Redis tiedot-komento.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| SUORITIN               | Suorittimen käyttö Azure Redis välimuistin palvelimen prosentteina raportoinnin määritetyn aikavälin aikana. Tämän arvon yhdistää käyttöjärjestelmän `\Processor(_Total)\% Processor Time` suorituskyvyn laskuri.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Välimuistin luku        | Tietojen määrää lukea välimuistista megatavuina sekunnissa (Mt/s) raportoinnin määritetyn aikavälin aikana. Tämän arvon johdetaan verkko-liittymän kortit, jotka tukevat virtuaalikoneen, joka isännöi välimuistin ja ei ole Redis.txt tietyn. **Tämä arvo vastaa välimuistin käyttämä kaistanleveys. Voit halutessasi puoli verkon kaistanleveyden kokorajoituksia ilmoitusten määrittäminen luo sitten sen tämän `Cache Read` laskuri. Lisätietoja eri tasojen ja koot välimuistin havaittujen kaistanleveyden rajat [Tässä taulukossa](cache-faq.md#cache-performance) .**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Välimuistin kirjoittaminen       | Kirjoittaa välimuistiin megatavuina (Mt/s) sekunnissa aikana määritetyn tietomäärän raportointi aikaväli. Tämän arvon johdetaan verkko-liittymän kortit, jotka tukevat virtuaalikoneen, joka isännöi välimuistin ja ei ole Redis.txt tietyn. Tämä arvo vastaa kaistanleveys lähetettyjä asiakaskoneesta välimuistin tietoja.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |


## <a name="how-to-view-metrics-and-customize-charts"></a>Voit tarkastella arvot ja kaavioiden mukauttaminen

Voit tarkastella yleiskatsaus määritetty välimuistin- **Redis arvot** -sivu. Voit käyttää **Redis arvot** sivu valitsemalla **kaikki asetukset** > **Redis arvot**.

![Redis arvot][redis-cache-redis-metrics]


**Redis arvot** -sivu sisältää seuraavat kaavioita.

| Redis arvot kaavio | Näytetyt arvot     |
|------------------|-------------------|
| Välimuistin luku- ja välimuistin kirjoittaminen | Välimuistin luku |
|                            | Välimuistin kirjoittaminen |
| Asiakkaiden      | Asiakkaiden |
| Osumien ja epäonnistumisten  | Osumia        |
|                  | Välimuistin epäonnistumisia      |
| Yhteensä komennot   | Toimintojen summa  |
| Hakee ja joukot    | Saa              |
|                  | Joukot              |
| Suorittimen käyttö         | SUORITIN           |
| Muistinkäyttö      | Käytetty muistin   |
|                   | Käytetty muistin RSS |
| Redis lataaminen palvelimeen | Lataa palvelimeen   |
| Avaimen määrä | Yhteensä näppäimet |
|           | Poistaa näppäimet |
|           | Vanhentuneen näppäimet |


Jos haluat tarkempia nähdä määritetty kaaviossa ja voit mukauttaa kaaviota, valitsemalla haluamasi kaavion näyttämään **metrijärjestelmä** sivu kaavion **Redis arvot** -sivu.

![Metrijärjestelmän sivu][redis-cache-metric-blade]

Ilmoitukset, jotka on kaavio näyttää arvojen mukaisesti näkyvät alareunassa olevan kaavion **metrijärjestelmä** -sivu.

Voit lisätä tai poistaa arvot tai muuttaa raportoinnin aikaväli, valitse **Muokkaa kaavion**.

Voit lisätä tai poistaa arvot kaaviosta, valitsemalla lisätiedot nimen vieressä oleva valintaruutu. Voit muuttaa raportoinnin aikaväli, valitsemalla haluamasi väli. **Kaaviolajin**muuttaminen, valitse haluamasi tyyli. Kun haluamasi muutokset on tehty, valitse **Tallenna**. 

![Kaavion muokkaaminen][redis-cache-edit-chart]

Kun valitset **Tallenna** tekemäsi muutokset on käytössä, kunnes jätät **metrijärjestelmä** -sivu. Kun palaat myöhemmin, näet alkuperäisen metriyksiköt ja aikavälin uudelleen. Lisätietoja kaavioiden mukauttamisesta on artikkelissa [näytön palvelun arvot](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Jos haluat tarkastella määritetty tietyn ajanjakson ajaksi kaaviossa, viemällä hiiren osoittimen tiettyihin palkkeihin tai kaavion, joka vastaa haluamaasi aikaa ja aikaväliä mittaukset näytetään.

![Kaavion tietojen tarkasteleminen][redis-cache-view-chart-details]

## <a name="how-to-monitor-a-premium-cache-with-clustering"></a>Voit valvoa premium-välimuistin asentaminen

Premium tallentaa, joiden [klusterointi](cache-how-to-premium-clustering.md) on käytössä voi olla enintään 10 shards. Kunkin shard on oma arvot ja antamaan arvot välimuistin kokonaiskoko koostetaan nämä arvot. Kunkin arvo sisältää kaksi versiota. Yksi arvo mittaa koko välimuistin ja arvo, joka sisältää toisen version suorituskyky `(Shard 0-9)` nimi mitat suorituskyvyn yksittäisen shard välimuistiin varten. Esimerkiksi jos välimuisti on 3 shards `Cache Hits` on koko välimuistin osumien kokonaismäärä ja `Cache Hits (Shard 2)` on vain kyseisen shard välimuistin osumia.

Kunkin seurantaa kaavio näyttää sekä kunkin välimuistin shard mittaukset välimuistin ylimmän tason mittaukset.

![Valvonta][redis-cache-premium-monitor]

Kohdassa tietoja viemällä hiiren osoitin päälle arvopisteiden Näyttää ajan. 

![Valvonta][redis-cache-premium-point-summary]

Suuremmat arvot ovat yleensä välimuistin koostearvoja, vaikka pienemmät arvot ovat shard yksittäisiä mittaukset. Huomaa, että tässä esimerkissä on kolme shards osumien tasaisesti jakautuu shards.

![Valvonta][redis-cache-premium-point-shard]

Voit tarkastella tarkemmin, napsauta kaaviota ja tarkastella laajennettu näyttämisen **metrijärjestelmä** -sivu.

![Valvonta][redis-cache-premium-chart-detail]

Kunkin kaavion näkyy oletusarvoisesti ylimmän tason välimuistin suorituskyvyn laskuri sekä yksittäisten shards suorituskyvyn laskureita. Voit mukauttaa näitä **Muokkaa kaavio** -sivu.

![Valvonta][redis-cache-premium-edit]

Lisätietoja käytettävissä suorituskyvyn laskureita on artikkelissa [käytettävissä olevat arvot ja aikavälit raportointi](#available-metrics-and-reporting-intervals).


## <a name="operations-and-alerts"></a>Toimintojen ja ilmoitukset

**Toiminnot** -osio **Redis välimuisti** -sivu on **tapahtumien** ja **ilmoitusten säännöt** osaa.

![Oeprations][redis-cache-operations-events]

Saat uusimmat välimuistin toimintojen luettelo valitsemalla **tapahtumien** kaavio näyttää **tapahtumat** -sivu. Esimerkkejä toiminnot ovat hakeminen ja uudelleen pikanäppäinten, ja aktivointi- ja tarkkuuden ilmoitusten säännöt. Lisätietoja kuhunkin tapahtumaan napsauttamalla tapahtumaa **tapahtumat** -sivu.

Lisätietoja tapahtumista on artikkelissa [tapahtumien tarkasteleminen ja valvontalokien](../monitoring-and-diagnostics/insights-debugging-with-events.md).

**Ilmoitusten säännöt** -kohdassa näkyy välimuistin esiintymän ilmoitusten määrää. Ilmoitusten säännön avulla voit valvoa välimuistin esiintymän ja saa sähköpostiviestiä aina, kun tiettyjä kustannusarvo saavuttaa määritetty säännön raja-arvon. 

Ilmoitusten säännöt käsitellään noin viisi minuuttia ja ilmoitusten sääntö on otettu käyttöön, kun kaikki määritetyn ilmoitukset lähetetään. Hälytyksen aktivointia ja ilmoituksia ei käsitellä välittömästi; mahdollinen viive useiden minuuttia, ennen kuin ilmoitusten sääntö on aktivoitu ja ilmoitukset lähetetään.

Ilmoitusten säännöt voi tarkastella ja määrittää tietyn seurantaa kaavioon **metrijärjestelmä** -sivu tai **ilmoitusten säännöt** -sivu.

Ilmoitusten säännöt ovat seuraavat ominaisuudet.

| Hälytyksen-ominaisuus                 | Kuvaus                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Resurssi                            | Arvioi hälytyksen resurssi. Luotaessa ilmoitusten säännön Redis.txt välimuistista välimuisti on resurssi.                                                                                                                                                                                                                                                                                                                                                  |
| Nimi                                | Nimi, joka yksilöi nykyisen esiintymän välimuistiin ilmoitusten säännön.                                                                                                                                                                                                                                                                                                                                                                                       |
| Kuvaus                         | Hälytyksen valinnainen kuvaus.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Arvo                              | Metrijärjestelmän seurattava ilmoitusten säännön perusteella. Välimuistin arvot luettelo-kohdassa käytettävissä olevat arvot ja raportointi väliajoin.                                                                                                                                                                                                                                                                                                                                             |
| Ehto                           | Hälytyksen ehto-operaattori. Mahdollisia vaihtoehtoja: suurempi kuin, suurempi tai yhtä suuri kuin, pienempi kuin, pienempi tai yhtä suuri kuin                                                                                                                                                                                                                                                                                                                             |
| Raja-arvo                           | Arvo, jota käytetään ehto-ominaisuudessa määritetyn operaattorilla metrijärjestelmä vertailu. Sen mukaan, arvo, tätä arvoa voi olla tavua/toisen tavua, % tai määrä.                                                                                                                                                                                                                                                                                     |
| Piste                              | Määrittää ajan tulosjoukko, jolle lisätiedot keskiarvo käytetään hälytyksen vertailua varten. Esimerkiksi jos edellisen tunnin ajanjakso, edellisen tunnin ajalta metrijärjestelmä keskiarvo käytetään vertailua varten. Jos haluat saada ilmoituksen, kun kynnysarvo täyttyy vuoksi Piikkiin toimintaa, lyhempi on sopiva. Saat ilmoituksen, kun kyseessä on kestävä tehtävän kynnysarvo, käytä pidempi. |
| Sähköpostipalvelu ja apuyhteyshenkilöiden | Kun arvo on TOSI, palvelun järjestelmänvalvoja- ja muiden järjestelmänvalvoja on sähköpostitse, kun ilmoituksen on aktivoitu.                                                                                                                                                                                                                                                                                                                                                                    |
| Lisätietoja järjestelmänvalvojan sähköposti      | Lisää järjestelmänvalvoja voi saada ilmoituksen, kun ilmoituksen aktivoidaan valinnainen sähköpostiosoite.                                                                                                                                                                                                                                                                                                                                                                    |

Vain yksi ilmoitus lähetetään hälytyksen aktivointi kohden. Kun sääntö raja-arvo on ylitetty ja ilmoitus lähetetään, sääntö ei uudelleen arvioidaan, kunnes lisätiedot laskee raja-arvon alapuolelle. Jos myöhemmin lisätiedot suurempi kuin kynnysarvo, ilmoituksen aktivoidaan ja uusi ilmoitus lähetetään.

Voit tarkastella kaikkia ilmoitusten sääntöjä välimuistin esiintymän valitsemalla **ilmoitusten säännöt** -osan **Redis välimuisti** -sivu. Jos haluat tarkastella vain tietyn metrijärjestelmä käyttävät ilmoitusten säännöt, siirry kaavion, joka sisältää kyseisen metrijärjestelmä **metrijärjestelmä** -sivu.

![Ilmoitusten säännöt][redis-cache-alert-rules]

Voit lisätä ilmoitusten sääntö valitsemalla **Lisää ilmoitus** **metrijärjestelmä** sivu tai **ilmoitusten säännöt** -sivu. 

Kirjoita haluamasi säännön ehtoja **ilmoituksen Lisää** sääntö-sivu ja valitse **OK**. 

![Ilmoitusten säännön lisääminen][redis-cache-add-alert]

>[AZURE.NOTE] Kun ilmoitusten sääntö on luotu valitsemalla **Lisää ilmoitus** **metrijärjestelmä** -sivu, vain kyseisen sivu kaaviossa näkyviä arvot näkyvät **metrijärjestelmä** avattavasta. Kun ilmoitusten sääntö on luotu valitsemalla **Lisää ilmoitus** **ilmoitusten säännöt** -sivu, kaikki välimuistin arvot ovat käytettävissä **metrijärjestelmä** avattavasta.

Kun ilmoitusten sääntö tallentuu, se näkyy **ilmoitus säännöt** -sivu ja, kaaviot, joissa käytetään metrijärjestelmän näyttäminen hälytyksen **metrijärjestelmä** -sivu. Voit muokata ilmoitusten säännön valitsemalla **Muokkaa sääntöä** -sivu näyttää ilmoituksen säännön nimi. **Muokkaa sääntöä** -sivu-säännön ominaisuuksien muokkaaminen, poistaminen tai hälytyksen poistaminen käytöstä tai ottaa säännön käyttöön uudelleen, jos se on aiemmin poistettu käytöstä.

>[AZURE.NOTE] Säännön ominaisuuksiin tekemäsi muutokset voi kestää hetken, ennen kuin ne näkyvät **ilmoitusten säännöt** -sivu tai **metrijärjestelmän** -sivu.

Kun ilmoitusten sääntö on aktivoitu, lähettää sähköpostiviestin hälytyksen määritysten mukaan ja ilmoitus-kuvake näkyy **ilmoitus säännöt** -osassa **Redis välimuisti** -sivu.

Ilmoitusten säännön pidetään ratkaistava, kun ilmoitusten ehto on tosi enää. Kun ilmoitusten säännön ehtoa ratkeaa, ilmoitus-kuvake on korvattu valintamerkki. Lisätietoja ilmoitusten aktivointia ja ratkaisut Valitse **tapahtumat** -osassa **Redis välimuisti** -sivu, jos haluat tarkastella tapahtumat **tapahtumat** -sivu.

Saat lisätietoja Azure ilmoitukset [Vastaanota ilmoitukset](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="metrics-on-the-redis-cache-blade"></a>Arvot-Redis välimuisti-sivu

**Redis välimuisti** -sivu näyttää seuraavanlaisia arvot.

-   [Kaavioiden seuranta](#monitoring-charts)
-   [Käyttö-kaaviot](#usage-charts)

### <a name="monitoring-charts"></a>Kaavioiden seuranta

**Seuranta** -osassa on **käynnit ja löytymättömien** **hakee ja määrittää**, **yhteydet**ja **Yhteensä komennot** kaavioita.

![Kaavioiden seuranta][redis-cache-monitoring-part]

**Seuranta** -kaavioissa seuraavat arvot.

| Kaavion seuranta | Välimuistin arvot     |
|------------------|-------------------|
| Osumien ja epäonnistumisten  | Osumia        |
|                  | Välimuistin epäonnistumisia      |
| Hakee ja joukot    | Saa              |
|                  | Joukot              |
| Yhteydet      | Asiakkaiden |
| Yhteensä komennot   | Toimintojen summa  |

Tietojen tarkasteleminen arvot ja mukauttamisesta yksittäisille kaavioille tämän osion kohdassa [tarkastelemisesta arvot ja arvot kaavioiden mukauttaminen](#how-to-view-metrics-and-customize-charts) .

### <a name="usage-charts"></a>Käyttö-kaaviot

**Käyttö** on **Redis kuormitus**, **Muistinkäytön**, **Verkon huippua**ja **Suorittimen käyttö** kaavioita ja näyttää myös välimuistin esiintymän **hinnoittelu taso** .

![Käyttö-kaaviot][redis-cache-usage-part]

**Hinnoittelu taso** näyttää välimuistin hinnat taso ja niitä voi käyttää [näytöstä](cache-how-to-scale.md) eri hinnoittelutiedot taso välimuisti.

**Käyttö** -kaavioissa seuraavat arvot.

| Käyttökaavio       | Välimuistin arvot |
|-------------------|---------------|
| Redis lataaminen palvelimeen | Lataa palvelimeen   |
| Muistinkäyttö      | Käytetty muistin   |
| Kaistanleveys | Välimuistin kirjoittaminen   |
| Suorittimen käyttö         | SUORITIN           |

Tietojen tarkasteleminen arvot ja mukauttamisesta yksittäisille kaavioille tämän osion kohdassa [tarkastelemisesta arvot ja arvot kaavioiden mukauttaminen](#how-to-view-metrics-and-customize-charts) .
  
<!-- IMAGES -->

[redis-cache-enable-diagnostics]: ./media/cache-how-to-monitor/redis-cache-enable-diagnostics.png

[redis-cache-diagnostic-settings]: ./media/cache-how-to-monitor/redis-cache-diagnostic-settings.png

[redis-cache-configure-diagnostics]: ./media/cache-how-to-monitor/redis-cache-configure-diagnostics.png

[redis-cache-monitoring-part]: ./media/cache-how-to-monitor/redis-cache-monitoring-part.png

[redis-cache-usage-part]: ./media/cache-how-to-monitor/redis-cache-usage-part.png

[redis-cache-metric-blade]: ./media/cache-how-to-monitor/redis-cache-metric-blade.png

[redis-cache-edit-chart]: ./media/cache-how-to-monitor/redis-cache-edit-chart.png

[redis-cache-view-chart-details]: ./media/cache-how-to-monitor/redis-cache-view-chart-details.png

[redis-cache-operations-events]: ./media/cache-how-to-monitor/redis-cache-operations-events.png

[redis-cache-alert-rules]: ./media/cache-how-to-monitor/redis-cache-alert-rules.png

[redis-cache-add-alert]: ./media/cache-how-to-monitor/redis-cache-add-alert.png

[redis-cache-premium-monitor]: ./media/cache-how-to-monitor/redis-cache-premium-monitor.png

[redis-cache-premium-edit]: ./media/cache-how-to-monitor/redis-cache-premium-edit.png

[redis-cache-premium-chart-detail]: ./media/cache-how-to-monitor/redis-cache-premium-chart-detail.png

[redis-cache-premium-point-summary]: ./media/cache-how-to-monitor/redis-cache-premium-point-summary.png

[redis-cache-premium-point-shard]: ./media/cache-how-to-monitor/redis-cache-premium-point-shard.png

[redis-cache-redis-metrics]: ./media/cache-how-to-monitor/redis-cache-redis-metrics.png

[redis-cache-redis-metrics-blade]: ./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png



