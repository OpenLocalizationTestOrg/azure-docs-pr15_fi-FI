<properties 
    pageTitle="Azure Redis välimuistin vianmääritys | Microsoft Azure" 
    description="Lue, miten voit ratkaista yleisiä ongelmia Azure Redis välimuistin." 
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
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-troubleshoot-azure-redis-cache"></a>Azure Redis välimuistin vianmääritys

Tässä artikkelissa on ohjeita seuraavanlaisia Azure Redis välimuistin ongelmien vianmääritystä varten.

-   [Asiakkaan puoli vianmääritys](#client-side-troubleshooting) – tässä osassa on ohjeita tunnistaminen ja ratkaiseminen Azure Redis välimuistin muodostaminen sovelluksen aiheuttamia ongelmia.
-   [Palvelimen puoli vianmääritys](#server-side-troubleshooting) – tässä osassa on ohjeita tunnistaminen ja ratkaiseminen-Azure Redis välimuistin palvelinpuolen aiheuttaa ongelmia.
-   [StackExchange.Redis aikakatkaisu poikkeukset](#stackexchangeredis-timeout-exceptions) - tässä osassa on tietoja vianmääritys, kun StackExchange.Redis-asiakas.


>[AZURE.NOTE] Monet tässä oppaassa vianmääritysohjeita sisältävät ohjeet Redis.txt komennot ja seurata eri suorituskyvyn mittarit. Saat lisätietoja ja ohjeita [Lisätietoja](#additional-information) -osion artikkelit.

## <a name="client-side-troubleshooting"></a>Asiakkaan puoli vianmääritys


Tässä osassa käsitellään vianmäärityksen-asiakassovellus ehdon ryhmäkäytännössä ilmenevät ongelmat.

-   [Asiakkaan muistinkäyttöön](#memory-pressure-on-the-client)
-   [Purskeen liikenteen](#burst-of-traffic)
-   [Suuri asiakkaan suorittimen käyttö](#high-client-cpu-usage)
-   [Asiakkaan puoli kaistanleveyden ylitetty](#client-side-bandwidth-exceeded)
-   [Suuri pyynnön ja vastauksen koko](#large-requestresponse-size)
-   [Mitä tapahtui Redis.txt Omat tiedot?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Asiakkaan muistinkäyttöön

#### <a name="problem"></a>Ongelma

Muistinkäyttöön asiakaskoneeseen johtaa kaikenlaisten suorituskykyongelmia, joka voi viivästyä, jotka on lähettänyt Redis.txt esiintymän välittömästi tietojen käsittely. Kun muistinkäyttöön käynnit, järjestelmä on yleensä sivun tietoihin-levyn eli näennäismuistin fyysistä muistia. Tämän *sivun Virhesovellus* aiheuttaa järjestelmän hidasta.

#### <a name="measurement"></a>Mitta 

1.  Seurata muistinkäyttö tietokoneessa, varmista, että se ylitä käytettävissä oleva muisti. 
2.  Valvonta `Page Faults/Sec` suorituskyvyn laskuri. Useimmat järjestelmät tulevat sivun virheitä on myös normaalin käytön aikana, niin kuuntelemaan sivun virheitä suorituskyvyn laskuri piikkarit, jotka vastaavat aikakatkaisu.

#### <a name="resolution"></a>Tarkkuus

Asiakkaan ksi suurempi asiakkaan muistia AM kokoa tai tarkastella muistin käyttö-kuvioiden avulla ja vähentää muistin consuption.


### <a name="burst-of-traffic"></a>Purskeen liikenteen

#### <a name="problem"></a>Ongelma

Heikko yhdistettynä bursts liikenteen `ThreadPool` asetuksia voi johtaa viipeitä jo lähettämä Redis palvelimeen, mutta ei ole vielä käytetty asiakaspuolen tietojen käsittelemistä.

#### <a name="measurement"></a>Mitta 

Näytön siitä, miten oman `ThreadPool` tilastotiedot muuttua vaiheittain pidemmän ajan kuluessa koodin [tältä](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Voit myös tarkastella `TimeoutException` StackExchange.Redis viesti. Tässä on esimerkki:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

Yllä oleva viesti on joitakin ongelmia, jotka kiinnostavat:

 1. Huomaa, että `IOCP` osan ja `WORKER` osassa, sinulla `Busy` arvo on suurempi kuin `Min` arvo. Tämä tarkoittaa, että `ThreadPool` asetuksia haluat muuttaa.
 2. Voit myös tarkastella `in: 64221`. Tämä ilmaisee, että 64211 tavua ydin socket layer vastaanottanut, mutta vielä ole luettu sovellus (esimerkiksi StackExchange.Redis). Tämä yleensä tarkoittaa, että sovelluksesi ei ole tietojen lukeminen verkosta nopeasti palvelin on lähettäminen sinulle.

#### <a name="resolution"></a>Tarkkuus

Määritä [Säievarannon asetukset](https://gist.github.com/JonCole/e65411214030f0d823cb) ja varmista, että viestiketjun resurssivarantoon skaalautuvat nopeasti purskeen tilanteissa.


### <a name="high-client-cpu-usage"></a>Suuri asiakkaan suorittimen käyttö

#### <a name="problem"></a>Ongelma

Suuri asiakkaan suorittimen käyttö on ilmoitus, että järjestelmä ei voi seurata työmäärän, joka on pyydetty suorittamiseen. Tämä tarkoittaa, että asiakas voi epäonnistua käsittelemään Redis.txt vastausta ajoissa tavalla, vaikka Redis.txt lähettää vastauksen nopeasti.

#### <a name="measurement"></a>Mitta

Valvonta järjestelmän leveä suorittimen käyttö on Azure-portaalin tai liittyvät suorituskyvyn laskuri kautta. Varmista, ettet valvoa *prosessin* Suoritin, koska yksi prosessi voi olla suorittimen käytön samanaikaisesti yleistä järjestelmän suorittimen voivat olla hyvin. Katso suorittimen käyttö piikkarit, jotka vastaavat aikakatkaisu varten. Tuloksena suuri suorittimen, voit nähdä myös suuri `in: XXX` arvot `TimeoutException` , näyttöön tulee virhesanomia [purskeen liikenteen](#burst-of-traffic) -kohdassa kuvatulla tavalla.

>[AZURE.NOTE] StackExchange.Redis 1.1.603 ja myöhemmin `local-cpu` metrisillä- `TimeoutException` virhesanomia. Varmista käyttämällä [StackExchange.Redis NuGet paketin](https://www.nuget.org/packages/StackExchange.Redis/)uusimman version. On korjaamista jatkuvasti parhaillaan koodi, minkä ansiosta tehokkaamman aikakatkaisu, joten on tärkeää ottaa uusimpaan versioon.

#### <a name="resolution"></a>Tarkkuus

Päivitä AM suorittimen tehokkaampaan suurentaminen tai tutkia, mikä aiheuttaa suorittimen piikkarit. 



### <a name="client-side-bandwidth-exceeded"></a>Asiakkaan puoli kaistanleveyden ylitetty

#### <a name="problem"></a>Ongelma

Eri samankokoista asiakaskoneiden on rajoitukset, kuinka paljon kaistanleveyttä heillä on käytettävissä. Jos asiakkaan ylittää kaistanleveyttä, valitse tietoja ei käsitellä asiakaspuolen nopeasti palvelin lähettää sen. Tämä voi aiheuttaa aikakatkaisu.

#### <a name="measurement"></a>Mitta

Seurata, kuinka kaistanleveyden käytön muuttuvat ajan kuluessa koodin [tältä](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs)avulla. Huomaa, että tämä koodi ei ehkä toimi onnistuneesti joissakin ympäristöissä käyttöoikeuksiltaan rajoitettuina (kuten Azure sivustoja).

#### <a name="resolution"></a>Tarkkuus 

Asiakkaan AM suurentaa tai pienentää verkon kaistanleveyden käyttöä.


### <a name="large-requestresponse-size"></a>Suuri pyynnön ja vastauksen koko

#### <a name="problem"></a>Ongelma

Suuri pyynnön ja vastauksen heikentää aikakatkaisu. Esimerkkinä oletetaan epäonnistua aikakatkaisuarvo on 1 sekunti. Sovelluksen pyytää kahta näppäintä (esimerkiksi "A" ja "B") (joko samalla fyysistä verkkoyhteyttä) samanaikaisesti. Useimmat asiakkaat tue "Pipelining" pyyntöjen, siten, että molemmat pyynnöt "A" ja "B" on lähetetty palvelimeen yhden koneisiin peräkkäin odottamatta vastaukset. Palvelin Lähetä vastaukset takaisin samassa järjestyksessä. Jos vastaus "A" on suuri tarpeeksi se syö myöhemmät aikakatkaisun useimmat ylöspäin. 

Seuraavassa esimerkissä on kuvattu Tämä skenaario. Tässä skenaariossa lähetetään pyyntö "A" ja "B" nopeasti-palvelin käynnistyy "A" ja "B" vastaukset lähetetään nopeasti, mutta tietojen siirtoajat, koska "B" Hae jumissa takana pyynnön ja aikakatkaistaan, vaikka palvelimen vastaus nopeasti.

  	|-------- 1 Second Timeout (A)----------|
  	|-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Mitta

Tämä on vaikeaa yksi mitata. Sinun on lähinnä soittimen asiakas-koodin jäljittämiseksi suuri pyynnöt ja vastaukset. 

#### <a name="resolution"></a>Tarkkuus

1.  Redis.txt on tarkoitettu suuri määrä pienet arvot muutaman suuren arvon sijaan. Ensisijainen ratkaisu on poistettava liittyvät pienemmät arvot yhdeksi tietojen määrittäminen. Lisätietoja [Redis.txt ihanteellinen arvoalue kokoa on? 100 kt on liian suuri?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) Lisätietoja ympärille miksi pienemmät arvot on suositeltavaa viestiin.
2.  Suurentaa oman AM (for asiakas- ja Redis välimuistin), saat suurempi kaistanleveys-ominaisuuksia, vähentää tietojen siirtoajat suurempi vastaukset. Huomaa, että käytön enemmän vain palvelimessa tai vain tämän asiakkaan eivät välttämättä ole tarpeeksi. Mittaa kaistanleveyden käytön ja vertaa sitä AM asennetun koon ominaisuuksia.
3.  Lisää `ConnectionMultiplexer` objektien voit käyttää ja pyöreän pöydän pyynnöt eri yhteyksillä.


### <a name="what-happened-to-my-data-in-redis"></a>Mitä tapahtui Redis.txt Omat tiedot?

#### <a name="problem"></a>Ongelma

Odotuksia tiettyjen tietojen Azure Redis välimuisti-esiintymän, mutta se ei vaikuta ollutta.

##### <a name="resolution"></a>Tarkkuus

Katso [mitä on tapahtunut Omat tiedot Redis.txt?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) mahdollisia syitä ja ratkaisut.


## <a name="server-side-troubleshooting"></a>Palvelimen puoli vianmääritys

Tässä osassa käsitellään vianmäärityksen välimuistin palvelimessa ehdon ryhmäkäytännössä ilmenevät ongelmat.

-   [Muistinkäyttöön palvelimessa](#memory-pressure-on-the-server)
-   [Suuren suorittimen käytön / lataaminen palvelimeen](#high-cpu-usage-server-load)
-   [Palvelimen puoli kaistanleveyden ylitetty](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Muistinkäyttöön palvelimessa

#### <a name="problem"></a>Ongelma

Muistinkäyttöön palvelimella johtaa kaikenlaisten suorituskykyongelmia, joka voi viivästyä pyyntöjen käsittelyn. Kun muistinkäyttöön käynnit, järjestelmä on yleensä sivun tietoihin-levyn eli näennäismuistin fyysistä muistia. Tämän *sivun Virhesovellus* aiheuttaa järjestelmän hidasta. On useita, tämä muistinkäyttöön mahdollisia syitä: 

1.  Olet syöttänyt välimuistin koko kapasiteettiin tiedoilla. 
2.  Redis.txt näkee hyvin muistin pirstoutuminen - aiheutuvat useimmin tallentaminen suurista (Redis.txt on tarkoitettu pieni objektit - kohdassa [mitä Redis.txt ihanteellinen arvoalue kokoa? 100 kt on liian suuri?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) Lisätietoja Kirjaa). 

#### <a name="measurement"></a>Mitta

Redis.txt paljastaa kaksi mittarit, joka auttaa sinua huomaamaan ongelman. Ensimmäinen on `used_memory` ja toinen `used_memory_rss`. [Nämä arvot](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) ovat käytettävissä Azure-portaalin tai [Redis INFO](http://redis.io/commands/info) -komentoa.

#### <a name="resolution"></a>Tarkkuus

On useita voi parantaa muistinkäytön kunnossa tekemäsi muutokset:

1. [Määritä muistin käytännön](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) ja määrittää avaimien vanheneminen kertaa. Huomaa, että ei välttämättä ole tarpeeksi, jos sinulla on pirstoutuminen.
2. [Määritä maxmemory varattu arvo](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) , joka on riittävän suuri muistin tarkistettavat hyvittää.
3. Jakaa suuria välimuistiin tallennettujen objektien pienempi liittyvien objektien päälle.
4. Välimuistin suurentaminen [asteikko](cache-how-to-scale.md) .
5. Jos käytössäsi on [premium välimuistin Redis.txt klusterin käytössä kanssa](cache-how-to-premium-clustering.md) voit [lisätä shards määrää](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Suuren suorittimen käytön / lataaminen palvelimeen

#### <a name="problem"></a>Ongelma

Suuri suorittimen käyttö voi tarkoittaa, että asiakaspuolen voi epäonnistua käsittelemään Redis.txt vastausta ajoissa tavalla, vaikka Redis.txt lähettää vastauksen nopeasti.

#### <a name="measurement"></a>Mitta

Valvonta järjestelmän leveä suorittimen käyttö on Azure-portaalin tai liittyvät suorituskyvyn laskuri kautta. Varmista, ettet valvoa *prosessin* Suoritin, koska yksi prosessi voi olla suorittimen käytön samanaikaisesti yleistä järjestelmän suorittimen voivat olla hyvin. Katso suorittimen käyttö piikkarit, jotka vastaavat aikakatkaisu varten.

#### <a name="resolution"></a>Tarkkuus

Suurempi välimuistin [asteikko](cache-how-to-scale.md) taso suorittimen tehokkaampaan tai tutkia, mikä aiheuttaa suorittimen piikkarit. 

### <a name="server-side-bandwidth-exceeded"></a>Palvelimen puoli kaistanleveyden ylitetty

#### <a name="problem"></a>Ongelma

Eri samankokoista välimuistiesiintymät on rajoitukset, kuinka paljon kaistanleveyttä heillä on käytettävissä. Jos palvelin on suurempi kuin kaistanleveyttä, valitse tietoja ei lähetetä nopeasti asiakkaalle. Tämä voi aiheuttaa aikakatkaisu.

#### <a name="measurement"></a>Mitta

Voit valvoa `Cache Read` arvo, joka on tietomäärän lukea välimuistista megatavuina sekunnissa (Mt/s) raportoinnin määritetyn aikavälin aikana. Tämä arvo vastaa välimuistin käyttämä kaistanleveys. Jos haluat puoli verkon kaistanleveyden kokorajoituksia ilmoitusten määrittäminen, voit luoda ne käyttävät tätä `Cache Read` laskuri. Vertaa oman lukemat eri tasojen ja koot välimuistin havaittujen kaistanleveyden rajat [tämän taulukon](cache-faq.md#cache-performance) arvoihin.

#### <a name="resolution"></a>Tarkkuus

Jos olet johdonmukaisesti lähellä havaittujen suurin kaistanleveys hinnoittelu tason ja välimuistin kokoa, harkitse hinnoittelu taso tai koko, joka on suurempi kaistanleveys, käyttämällä arvoja [Tässä](cache-faq.md#cache-performance) taulukossa ohjeena [Skaalaus](cache-how-to-scale.md) .


## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis aikakatkaisu poikkeukset

StackExchange.Redis käyttää kokoonpanon määrittäminen nimetyn `synctimeout` synkronisen toimille, jonka oletusarvo on 1 000 ms. Jos synkronisen puhelun ei täytä josta kellonajan, StackExchange.Redis asiakkaan ilmoittaa samalla tavalla kuin seuraavassa esimerkissä aikakatkaisu-virheen.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Tämä virhesanoma sisältää arvot, jotka auttavat kohtaa ongelman syy ja mahdollinen ratkaisu. Seuraavassa taulukossa on tietoja virhe viestin arvot.

| Virhe viestin metrijärjestelmä | Tiedot                                                                                                                                                                                                                                          |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inst       | Valitse viimeisen aikajakso: 0 komennot on myönnetty                                                                                                                                                                                              |
| muuttaa        | Socket hallinta toimii `socket.select` OS osoittamaan socket, jossa on jotain tekemään, mikä tarkoittaa sitä pyytää lähinnä: Lukija ei aktiivisesti lukee verkosta koska ei mielestäsi on tekemistä |
| jonossa      | On käynnissä-toimintojen 73 summa                                                                                                                                                                                                        |
| qu         | 6 käynnissä olevat toiminnot ovat lähettämättömien jonossa ja ei vielä ole kirjoitettu lähtevä verkkoon                                                                                                                                                           |
| Qs         | hän käynnissä olevien toimintojen 67 on lähetetty palvelimeen, mutta vastausta ei ole vielä käytettävissä. Vastaus voi olla `Not yet sent by the server` tai`sent by the server but not yet processed by the client.`                                                   |
| QC         | 0 keskeneräiset-toimintoa tuliko vastaukset, mutta se ei ole vielä merkitty valmiiksi vuoksi Odotetaan valmistumista tapahtuu                                                                                                                                      |
| wR         | On aktiivinen (eli 6 lähettämättömien pyynnöt ohitetaan) writer tavua/activewriters                                                                                                                                                   |
| Valitse         | Ei ole aktiivinen lukijat ja nolla tavua ovat käytettävissä, joilla voi lukea NIC tavua/activereaders                                                                                                                                               |


### <a name="steps-to-investigate"></a>Jos haluat tutkia vaiheet

1. Kun parhaita käytäntöjä Varmista, että käytät seuraavaa kaavaa muodostetaan, kun StackExchange.Redis-asiakas.


        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


    Lisätietoja on artikkelissa [etäyhteyden muodostaminen käyttämällä StackExchange.Redis välimuistin](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

2. Varmista, että Azure Redis välimuistin ja asiakassovellus ovat samalla alueella Azure-tietokannassa. Esimerkiksi voit saattaa olla käytön aikakatkaisu kun välimuistin ovat Yhdysvaltojen Itä, mutta asiakas on Länsi US ja pyyntö ei täytä sisällä `synctimeout` välin tai saattaa olla näyttöön aikakatkaisu korjattava paikallista kehittämistä tietokoneesta. 

    On suositeltavaa on välimuistin ja samalla Azure alueella-asiakassovelluksessa. Jos sinulla on tilanne, jossa on cross alueen puhelut, sinun kannattaa asettaa `synctimeout` välin arvo, joka on suurempi kuin oletusarvoinen 1000 ms aikavälin mukaan lukien `synctimeout` yhteysmerkkijono-ominaisuutta. Seuraavassa esimerkissä StackExchange.Redis välimuistin yhteyden merkkijonon koodikatkelman kanssa `synctimeout` 2000 MS.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...

3. Varmista käyttämällä [StackExchange.Redis NuGet paketin](https://www.nuget.org/packages/StackExchange.Redis/)uusimman version. On korjaamista jatkuvasti parhaillaan koodi, minkä ansiosta tehokkaamman aikakatkaisu, joten on tärkeää ottaa uusimpaan versioon.

4. Jos määritettynä on pyynnöt, jotka ovat käytön sido palvelimeen tai asiakkaan kaistanleveyden rajoituksia, kestää niiden loppuun ja siten aiheuttaa aikakatkaisu. Jos yhteyttä aikakatkaisu on palvelimessa kaistanleveys on kohdassa [palvelimen puoli Kaistanleveys ylitetty](#server-side-bandwidth-exceeded). Jos yhteyttä aikakatkaisu on asiakkaan kaistanleveys on ohjeartikkelissa [asiakkaan puoli kaistanleveys on ylitetty](#client-side-bandwidth-exceeded).

6. Sidottujen voit suorittimen käytön palvelimessa tai asiakkaan?
    -   Tarkista, jos sinulla on käytön sidottu suoritinta asiakkaalle, joka saattaa aiheuttaa pyyntö ei käsitellä sisällä `synctimeout` aikaväli, aiheuttaa näin aikakatkaisu. Asiakkaan suurentaminen siirtäminen tai Lataa jakaminen avulla voit määrittää tämän. 
    -   Valintaruutu, jos ilmenee suorittimen sidottu palvelimessa seuraamalla `CPU` [välimuistin suorituskyvyn mittarin](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Redis.txt ollessa suorittimen sidottu tulevat pyynnöt voivat aiheuttaa näiden aikakatkaisu pyynnöt. Sähköpostiosoite, tämä kuormituksen jakaa useita shards premium välimuistiin tai päivittää suurentaminen tai hinnoittelu taso. Lisätietoja on kohdassa [Palvelimen puoli Kaistanleveys ylitetty](#server-side-bandwidth-exceeded).

7. Onko komennot kestää kauan käsittelemään palvelimessa? Pitkään suoritetut komennot, jotka kestää kauan Redis.txt-palvelimen aikakatkaisu voi aiheuttaa. Esimerkkejä pitkään käynnissä komennot ovat `mget` on paljon näppäimet- `keys *` tai huonosti lua komentosarjoja. Voit muodostaa oman Azure Redis välimuistin esiintymän Redis.txt cli käyttävä tai käyttää [Redis konsolin](cache-configure.md#redis-console) ja suorittamalla [SlowLog](http://redis.io/commands/slowlog) -komento ole kestää odotettua pyynnöt. Redis.txt-palvelimen nimen ja StackExchange.Redis on optimoitu monta pieniä pyyntöjä vähemmän suuri pyynnöt sijaan. Tietojen jakaminen pienempi joukkojen voi parantaa seuraavat asiat. 

    Lisätietoja yhteyden muodostamisesta Azure Redis välimuistin SSL-päätepisteen Redis.txt cli ja stunnel on blogimerkinnässä [Julkaisu ASP.NET istunnon tilan Provider for Redis Preview-versio](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) . Lisätietoja on artikkelissa [SlowLog](http://redis.io/commands/slowlog).

8. Suuri Redis.txt kuormitus heikentää aikakatkaisu. Voit valvoa palvelimen kuormituksen valvonta `Redis Server Load` [välimuistin suorituskyvyn mittarin](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Kuormitus 100 (enimmäisarvo) ilmaisee, että Redis.txt palvelin on varattu, jossa ei ole joutoaika käsittely. Jos kaikki server-ominaisuuksien määrittäminen kestää tiettyjä pyynnöt näkyviin Suorita SlowLog-komento, edellisessä kohdassa kuvatulla. Lisätietoja on artikkelissa [suuri suorittimen käyttö / palvelimen ladata](#high-cpu-usage-server-load).

9. Oli asiakaspuolen, joka saattaa aiheuttaa verkon blip muun tapahtuman? Tarkista asiakkaan (web, Työntekijä roolin tai Iaas AM), jos oli tapahtuman, kuten skaalaus asiakkaan kerrat, ylös tai alas tai otat käyttöön uuden version asiakkaan tai Automaattinen skaalaus on käytössä? Tutustu testauksessa on löytänyt Automaattinen skaalaus tai skaalauksen ylös/alas heikentää lähtevä verkkoyhteyden katoavat muutaman sekunnin ajan. StackExchange.Redis koodi on joustavat tällaisten tapahtumien ja muodostaa. Kyseisenä ajanjaksona uudelleen yhteyttä pyyntöihin jonossa voi kadota itsestään.

10. On olemassa useita pieniä pyyntöjä edeltävän Redis välimuistiin, aikakatkaisu suuri pyyntö? Parametrin `qs` kuvattu virhe viesti kertoo, kuinka monta pyynnöt lähetettiin asiakaskoneesta palvelimeen, mutta ei ole vielä käsitelty vastausta. Tämän arvon pitää kasvava koska StackExchange.Redis käyttää yhden TCP-yhteyden ja lukea vain yhden vastauksen kerrallaan. Vaikka ensimmäisen toiminnon aikakatkaistu, se ei lopeta on lähetetty / palvelimesta tiedot ja muut pyynnöt estetään, kunnes tämä on valmis, aiheuttaa aika käyttäminen. Yksi ratkaisu on Pienennä aikakatkaisu mahdollisuutta varmistetaan, että välimuistin on riittävän suuri, havainnollistamiseen ja jakaminen suuriin arvoihin pienempi näkyvissä. Toinen mahdollinen ratkaisu on käyttää resurssivarantoon `ConnectionMultiplexer` asiakkaan objektit ja sitten vähintään ladattu `ConnectionMultiplexer` lähetettäessä uuden kokouspyynnön. Tämä olisi estää yhden aikakatkaisu aiheuttaa myös aikakatkaisu muiden pyynnöt.

11. Jos käytössäsi on `RedisSessionStateprovider`, varmista uudelleen aikakatkaisu on määritetty oikein. `retrytimeoutInMilliseconds`on oltava suurempi kuin `operationTimeoutinMilliseonds`, muuten ei uudelleenyritykset tapahtuu. Seuraavassa esimerkissä `retrytimeoutInMilliseconds` on määritetty 3000. Lisätietoja on artikkelissa [ASP.NET-istunnon tilan palvelu Azure Redis välimuistin](cache-aspnet-session-state-provider.md) ja [Opi käyttämään istunnon tilan ja tulosteen välimuisti-palvelun määrittäminen parametreja](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).


    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


12. Tarkista muistinkäytön Azure Redis välimuistin palvelimessa [seuraamalla](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` ja `Used Memory`. Jos eviction-käytäntö on määritetty, Redis.txt käynnistyy evicting nuolinäppäimiä milloin `Used_Memory` saavuttaa välimuistin kokoa. Ihannetapauksessa `Used Memory RSS` pitäisi olla vain hieman suurempi kuin `Used memory`. Suuri ero tarkoittaa on muistin pirstoutuminen (sisäinen tai ulkoinen. Kun `Used Memory RSS` on pienempi kuin `Used Memory`, se tarkoittaa sitä, välimuistin osa vaihtaa on paikkaa käyttöjärjestelmä. Jos näin tapahtuu voit odottaa joitakin merkittäviä viiveet suurempia. Koska Redis.txt ei ole päättää, kuinka sen kohdistukset yhdistetään muistisivuja, suuri `Used Memory RSS` on usein tulos muistinkäytön Piikkiin. Kun Redis.txt vapauttaa muistia, muistin annetaan takaisin varauksen ja varauksen voi tai ei saa muistin takaisin järjestelmään. Ehkä eroavat toisistaan `Used Memory` arvo ja muistin kulutus ilmoittaa käyttöjärjestelmä. Lähettää sen vuoksi muistin on käytetty ja julkaissut Redis.txt, mutta ei ole määritetty järjestelmän takaisin. Muistin ongelmat voidaan minimoida voit suorittaa seuraavat vaiheet.
    -   Päivitä välimuistin suurentaminen niin, että käytössäsi ei ole työpöytäversioihin muistin rajoitukset järjestelmään.
    -   Määrittää vanhentumispäivä kertaa näppäimet niin, että vanhat arvot ovat poistaa itse.
    -   Valvonta `used_memory_rss` välimuistin arvo. Kun tämä arvo lähestyy niiden välimuistin kokoa, olet todennäköisesti Aloita näe suorituskykyongelmia. Jakaa useita shards tiedot, jos olet premium-välimuistin avulla, tai päivittää välimuistin suurentaminen.

    Lisätietoja on artikkelissa [Muistinkäyttöön palvelimessa](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Lisätietoja

-   [Mitä Redis välimuistin tarjoaa ja koko kannattaa käyttää?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
-   [Miten voin ensisijainen ja testaa Omat välimuistin suorituskykyä?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Miten voit suorittaa Redis.txt komentoja?](cache-faq.md#how-can-i-run-redis-commands)
-   [Voit valvoa Azure Redis välimuisti](cache-how-to-monitor.md)