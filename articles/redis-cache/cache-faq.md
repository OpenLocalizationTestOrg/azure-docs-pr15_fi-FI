<properties 
    pageTitle="Azure Redis.txt välimuistin usein kysytyt kysymykset | Microsoft Azure" 
    description="Lue vastauksia yleisiin kysymyksiin, kuvioiden ja parhaita käytäntöjä Azure Redis välimuistin" 
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
    ms.date="10/18/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-faq"></a>Azure Redis.txt välimuistin usein kysytyt kysymykset

Lue vastauksia yleisiin kysymyksiin, kuvioiden ja parhaita käytäntöjä Azure Redis välimuistin. 


## <a name="what-if-my-question-isnt-answered-here"></a>Entä jos kysymykseeni ei ole vastattu tähän?

Jos kysymys ei ole mainittu tässä, kerro meille, ja Microsoft auttaa sinua löydä vastausta.

-   Voit lähettää kysymyksen [Disqus viestiketjun](#comments) jäljempänä tässä usein kysytyt kysymykset ja osallistuminen Azure välimuisti-ryhmän ja muut yhteisön jäsenet tietoja tässä artikkelissa.
-   Saavuttaa laajan yleisön, Lähetä kysymys- [Azure välimuistin MSDN-keskustelupalsta](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) ja osallistuminen Azure välimuisti-ryhmän ja muut yhteisön jäsenten kanssa.
-   Jos haluat esittää ominaisuus, voit lähettää pyyntöjä ja [Azure Redis välimuistin käyttäjän Voice](https://feedback.azure.com/forums/169382-cache)ideoita.
-   Voit lähettää sähköpostiviestin US osoitteessa [Azure välimuistin ulkoisen palautetta](mailto:azurecache@microsoft.com).

## <a name="azure-redis-cache-basics"></a>Azure Redis välimuistin perusteet

Usein kysyttyjä kysymyksiä sisältö käsittelemme joitakin Azure Redis välimuistin perusteet.

-    [Mikä on Azure Redis välimuistin?](#what-is-azure-redis-cache)
-    [Miten voi voin aloittaa Azure Redis välimuistin?](#how-can-i-get-started-with-azure-redis-cache)

Seuraavat usein kysyttyjä kysymyksiä peruskäsitteet ja Azure Redis välimuistin koskeviin kysymyksiin ja usein kysytyt kysymykset-Kohdista vastataan.

-   [Mitä Redis välimuistin tarjoaa ja koko kannattaa käyttää?](#what-redis-cache-offering-and-size-should-i-use)
-   [Mitä Redis.txt välimuistin asiakkaat voivat käyttää?](#what-redis-cache-clients-can-i-use)
-   [Onko Azure Redis välimuistin paikallisen emulointikone?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Miten kunto ja oma välimuistin suorituskykyä valvoa?](#how-do-i-monitor-the-health-and-performance-of-my-cache)



## <a name="planning-faqs"></a>Usein kysyttyjä kysymyksiä suunnittelu

-   [Mitä Redis välimuistin tarjoaa ja koko kannattaa käyttää?](#what-redis-cache-offering-and-size-should-i-use)
-   [Azure Redis välimuistin suorituskyky](#azure-redis-cache-performance)
-   [Mitä alueen oma välimuistin olisi Etsi?](#in-what-region-should-i-locate-my-cache)
-   [Miten Azure Redis välimuistin 'M veloittaa?](#how-am-i-billed-for-azure-redis-cache)
-   [Azure Redis välimuistin käyttää Azure Government Cloud tai Azure Kiinan pilveen](#can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud)



## <a name="development-faqs"></a>Kehittäminen usein kysyttyjä kysymyksiä

-   [Mitä StackExchange.Redis kokoonpanoasetusten määrittäminen tehdä?](#what-do-the-stackexchangeredis-configuration-options-do)
-   [Mitä Redis.txt välimuistin asiakkaat voivat käyttää?](#what-redis-cache-clients-can-i-use)
-   [Onko Azure Redis välimuistin paikallisen emulointikone?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Miten voit suorittaa Redis.txt komentoja?](#how-can-i-run-redis-commands)
-   [Miksi Azure Redis välimuistia ei ole on MSDN luokan kirjaston viittaus kuten joitakin Azure muiden?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
-   [Azure Redis välimuistin käyttää PHP istunnon välimuistin nimellä](#can-i-use-azure-redis-cache-as-a-php-session-cache)


## <a name="security-faqs"></a>Suojauksen usein kysyttyjä kysymyksiä

-   [Kun-SSL yhteyden muodostamisesta Redis.txt portti tulee käyttöön?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)


## <a name="production-faqs"></a>Tuotannon usein kysyttyjä kysymyksiä

-   [Mitkä ovat tuotannon parhaista käytännöistä?](#what-are-some-production-best-practices)
-   [Mitkä ovat seikat yleisiä Redis.txt komentoja käytettäessä?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
-   [Miten voin ensisijainen ja testaa Omat välimuistin suorituskykyä?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Tärkeitä tietoja Säievarannon kasvu](#important-details-about-threadpool-growth)
-   [Ota käyttöön palvelimen yleisen luettelon avulla opit Lisää siirtonopeuden asiakkaan StackExchange.Redis käytettäessä](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)


## <a name="monitoring-and-troubleshooting-faqs"></a>Valvonta ja vianmääritys usein kysyttyjä kysymyksiä

Usein kysyttyjä kysymyksiä sisältö kansilehden yleisiä seuranta ja vianmääritys kysymyksiä. Saat lisätietoja valvonta ja vianmääritys Azure Redis välimuistin esiintymät [seuraamisesta Azure Redis välimuistin](cache-how-to-monitor.md) ja [Azure Redis välimuistin vianmääritys](cache-how-to-troubleshoot.md).

-   [Miten kunto ja oma välimuistin suorituskykyä valvoa?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
-   [Välimuistin diagnostiikka tallennustilan tilin asetusten muuttunut, mitä on tapahtunut?](#my-cache-diagnostics-storage-account-settings-changed-what-happened)
-   [Miksi diagnostiikan on käytössä joitakin uuden tallentaa, mutta ei muiden?](#why-is-diagnostics-enabled-for-some-new-caches-but-not-others)
-   [Miksi aikakatkaisu?](#why-am-i-seeing-timeouts)
-   [Miksi asiakkaan katkesi välimuistin?](#why-was-my-client-disconnected-from-the-cache)


## <a name="prior-cache-offering-faqs"></a>Aiemman välimuistin tarjoaa usein kysyttyjä kysymyksiä

-   [Mitä Azure välimuistin tarjoaa sopii minulle parhaiten?](#which-azure-cache-offering-is-right-for-me)


### <a name="what-is-azure-redis-cache"></a>Mikä on Azure Redis välimuistin?

Avaa-tietolähteen Suositut [Redis välimuistin](http://redis.io)perustuu Azure Redis välimuistin. Sen kautta pääset käsiksi suojattua ja erillinen Redis.txt välimuistiin, hallitun Microsoft ja käytettävissä mistä tahansa sovelluksesta Azure kuluessa. Lisätietoja yleisiä näy Azure.com [Azure Redis välimuistin](https://azure.microsoft.com/services/cache/) product-sivu.


### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Miten voi voin aloittaa Azure Redis välimuistin?

Voit aloittaa Azure Redis välimuistin kanssa seuraavilla tavoilla:

-    Voit tarkistaa jokin käytettävissä Microsoftin opetusohjelmat [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md)ja [Python](cache-python-get-started.md). 
-    Voit katsoa [luominen hyvin suorituskyvyn sovellukset käyttämällä Microsoft Azure Redis välimuistin poistamisesta](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
-    Voit tarkistaa asiakkaat, jotka vastaavat kehittäminen kielen ja katso, miten voit käyttää Redis.txt asiakkaan ohjeissa. On monia Redis.txt ohjelmat, joita voidaan käyttää Azure Redis välimuistin. Redis.txt-ohjelmien luettelo on artikkelissa [http://redis.io/clients](http://redis.io/clients).


Jos sinulla ei ole vielä Azure-tili, voit:

-    [Avaa Azure-tili maksutta](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Saat hyvitykset, jonka avulla voit kokeilla maksettu Azure services. Vaikka hyvitysten on käytetty, voit pitää tili ja Azure palveluiden ja -toimintojen käytön.
-    [Aktivoi Visual Studio tilaajan etuja](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). MSDN-tilauksen tutustutaan hyvitykset joka kuukausi, jonka maksettu Azure-palveluiden avulla.


<a name="cache-size"></a>
### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Mitä Redis välimuistin tarjoaa ja koko kannattaa käyttää?
Kunkin Azure Redis välimuistin tarjoaa on eri tasoilla **koon**, **kaistanleveyden**, **suuri käytettävyys**ja **SLA** asetukset.

Seuraavassa on Huomioitavaa valitseminen Välimuistin tarjoaa.

-   **Muisti**: perus- ja standardin tasoa tarjota 250 Mt – 53 Gigatavua. Premium-taso on enintään 530 Gigatavua helppokäyttöisyyden [pyynnön](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase)kanssa. Lisätietoja on artikkelissa [Azure Redis välimuistin hinnat](https://azure.microsoft.com/pricing/details/cache/).
-   **Verkon suorituskykyyn**: Jos työmäärän, joka edellyttää hyvin siirtonopeuden Premium taso on enemmän standardiksi tai Basic verrattuna. Myös kunkin tason suurempi koot tallentaa on enemmän pohjana AM, joka isännöi välimuistin vuoksi. Katso lisätietoja [seuraavasta taulukosta](#cache-performance) .
-   **Siirtonopeuden**: Premium taso on suurin käytettävissä siirtonopeuden. Jos välimuistin palvelimeen tai asiakkaan saavuttaa kaistanleveyden rajoituksia, saat asiakaspuolen aikakatkaisu. Lisätietoja on artikkelissa seuraavassa taulukossa.
-   **Suuri käytettävyys/SLA**: Azure Redis välimuistin takaa Standard/Premium-välimuistin olevan käytettävissä vähintään 99,9 % ajan. Lisätietoja Microsoftin SLA on artikkelissa [Azure Redis välimuistin hinnat](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). SLA kattaa vain välimuistin päätepisteet yhteys. SLA eivät kuulu tietojen menettämistä jatkossa suojaus. On suositeltavaa Redis.txt tietojen pysyvyyttä-toiminnon käyttäminen Premium tason niin, että vikasietoisuudelle tietojen katoamiselta.
-   **Redis tietojen pysyvyyttä**: Premium taso sallii jatkuvat Azure-tallennustilan tilin välimuistin tiedot. Basic/Standard välimuistiin kaikki tiedot on tallennettu vain muistissa. Jos pohjana oleva infrastruktuuri ongelmien voi olla tietoja ei menetetä. On suositeltavaa Redis.txt tietojen pysyvyyttä-toiminnon käyttäminen Premium tason niin, että vikasietoisuudelle tietojen katoamiselta. Azure Redis välimuistin vaihtoehdot RDB ja AOF (tulossa)-Redis.txt pysyvyys. Lisätietoja on artikkelissa [Premium Azure Redis välimuistin pysyvyyden määrittämisestä](cache-how-to-premium-persistence.md).
-   **Redis klusterin**: Luo välimuistiin 53 megatavun tai useita Redis.txt solmujen shard tietojen, voit käyttää Redis.txt klusterointi, joka on käytettävissä Premium taso. Kukin solmu koostuu ensisijainen/replikan välimuistin pari suuren. Lisätietoja on artikkelissa [käyttövarmuuden lisäämiseksi Premium Azure Redis välimuistin määrittämisestä](cache-how-to-premium-clustering.md).
-   **Parannettu suojaus ja verkon eristystaso**: Azure Virtual verkon (VNET) käyttöönoton on parannettu suojaus ja eristystaso oman Azure Redis välimuistin, sekä aliverkosta, access hallita käytäntöjä ja muita ominaisuuksia, jotka edelleen rajoittaa käyttöä. Lisätietoja on artikkelissa [Premium Azure Redis välimuistin VPN-tuki määrittämisestä](cache-how-to-premium-vnet.md).
-   **Määritä Redis**: Standard- ja Premium tasoa, voit määrittää Redis.txt Keyspace ilmoituksia.
-   **Asiakasyhteydet enimmäismäärä**: Premium taso on asiakkaat, jotka voit muodostaa yhteyden Redis.txt isommalla numerolla suurempi samankokoista tallentaa yhteyksien enimmäismäärä. [Kirjoita viittaa hinnoittelu sivulla, jotta tiedot](https://azure.microsoft.com/pricing/details/cache/).
-   **Erillinen Core Redis palvelimen**: N Premium-tason välimuistin koosta riippumatta on erillinen core Redis.txt. Basic/standardin tasoa C1 koko ja edellä on erillinen core Redis.txt palvelimen.
-   **Redis.txt on yksi threaded** niin on enemmän kuin kaksi sydämiä ei tarjoa myös hyötyä päälle ottaa kahdella sydämiä, mutta suurempien AM käyttäminen on yleensä koko on pienempi kuin enemmän. Jos välimuistin palvelimeen tai asiakkaan saavuttaa kaistanleveyden rajoituksia, näyttöön tulee asiakaspuolen aikakatkaisu.
-   **Suorituskykyä on parannettu**: Premium tason tallentaa on otettu käyttöön laite, jonka on nopeampaa suorittimien ja antaa parempi suorituskyky verrattuna Basic tai Standard-taso. Premium taso tallentaa on suurempi siirtonopeuden ja pienempi viiveitä.

<a name="cache-performance"></a>
### <a name="azure-redis-cache-performance"></a>Azure Redis välimuistin suorituskyky

Seuraavassa taulukossa on suurin kaistanleveys arvo mittaa testin erikokoisista standardin aikana ja Premium välimuistiin käyttämällä `redis-benchmark.exe` -Iaas-AM vastaan Azure Redis välimuistin päätepiste. Huomaa, että näitä arvoja ei oteta ei ole SLA näiden numerot, vaikka sen pitäisi olla Normaali. Sinun on ladattava testata oman sovelluksen määrittämään sovelluksen oikean välimuistin kokoa.

Tässä taulukossa on piirtää seuraavat päätelmien.

-   Tallentaa, joka on samankokoinen siirtonopeuden on suurempi Premium tason verrattuna vakio taso. Esimerkiksi 6 Gigatavua välimuistia P1 siirtonopeuden on 140K RPS 49 K C3 verrattuna.
-   Redis.txt klusterointi, jossa siirtonopeuden kasvaa lineaarisesti Lisää shards (solmujen) klusterin määrää. Esimerkiksi jos luot 10 shards P4 klusterin, valitse käytettävissä olevat siirtonopeuden on 250K * 10 = 2,5 miljoonaa RPS.
-   Suurentaa avaimen koosta siirtonopeuden on suurempi-standardin taso verrattuna Premium-tason.

| Hinnat taso             | Kokoa   | Suorittimen sydämiä | Kaistanleveyttä                                    | 1 kt avaimen koko                            |
|--------------------------|--------|-----------|--------------------------------------------------------|------------------------------------------|
| **Vakio välimuistin kokoja** |        |           | **Sek (Mt/s) jotka / megatavua (Mt/s) sekunnissa** | **Pyynnöt sekunnissa (RPS)**            |
| C0                       | 250 MT | Jaettu    | 5 / 0.625                                              | 600                                      |
| C1                       | VÄHINTÄÄN 1 GT   | 1         | 100 / 12,5                                             | 12200                                    |
| C2                       | 2,5 GIGATAVUA | 2         | 200 / 25                                               | 24000                                    |
| C3                       | 6 GIGATAVUA   | 4         | 400 / 50                                               | 49000                                    |
| C4                       | 13 GT  | 2         | 500 / 62.5                                             | 61000                                    |
| C5                       | 26 GT  | 4         | 1000 / 125                                             | 115000                                   |
| C6                       | 53 GT  | 8         | 2000 / 250                                             | 150000                                   |
| **Premium välimuistin kokoja**  |        | **Suorittimen sydämiä shard kohden**  |                                         | **Pyynnöt sekunnissa (RPS) shard kohti** |
| P1                       | 6 GIGATAVUA   | 2         | 1000 / 125                                             | 140000                                   |
| P2                       | 13 GT  | 4         | 2000 / 250                                             | 220000                                   |
| P3                       | 26 GT  | 4         | 2000 / 250                                             | 220000                                   |
| P4                       | 53 GT  | 8         | 4000 / 500                                             | 250000                                   |


Ohjeet, kuten lataamisen Redis.txt-Työkalut `redis-benchmark.exe`, katso [miten Redis.txt komentoja voi suorittaa?](#cache-commands) osa.

<a name="cache-region"></a>
### <a name="in-what-region-should-i-locate-my-cache"></a>Mitä alueen oma välimuistin olisi Etsi?

Parhaan suorituskyvyn ja alimman viive Etsi Azure Redis välimuistin sama kuin välimuistin asiakassovellus alue.

<a name="cache-billing"></a>
### <a name="how-am-i-billed-for-azure-redis-cache"></a>Miten Azure Redis välimuistin 'M veloittaa?

Azure Redis välimuistin hinnat on [tähän](https://azure.microsoft.com/pricing/details/cache/). Sivun hinnoittelutiedot luettelo hinnat tuntihinta nimellä. Tallentaa ovat laskuttaa minuutissa välein aika siitä välimuistin luodaan saakka, välimuistin poistetaan. Ei ole mahdollisuutta käyttää pysäyttäminen tai keskeyttäminen välimuistin laskutus.


## <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud"></a>Azure Redis välimuistin käyttää Azure Government Cloud tai Azure Kiinan pilveen

Kyllä, Azure Redis välimuisti on käytettävissä Azure Government Cloud ja Azure kiina pilveen. Huomaa, että URL-osoitteet käyttämisen ja Azure Redis välimuistin hallinnan ovat eri Azure Government Cloud ja Azure Kiinan Cloud verrattuna Azure julkisen pilveen. Lisätietoja Azure Redis välimuistin käyttäminen Azure Government Cloud ja Azure kiina Cloud seikkoja on artikkelissa [Azure Government tietokannat - Azure Redis välimuistin](../azure-government/documentation-government-services-database.md#azure-redis-cache) ja [Azure kiina Cloud - Azure Redis välimuistin](https://www.azure.cn/documentation/services/redis-cache/).

Lisätietoja Azure Redis välimuistin käyttämisestä PowerShellin Azure Government Cloud ja Azure Kiinan Cloud on artikkelissa [Azure Government Cloud tai Azure Kiinan pilvestä yhdistämisestä](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


<a name="cache-configuration"></a>
### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Mitä StackExchange.Redis kokoonpanoasetusten määrittäminen tehdä?

StackExchange.Redis on useita vaihtoehtoja. Tässä osassa puheen joistain Yleiset asetukset. Katso tarkempia tietoja StackExchange.Redis asetukset [StackExchange.Redis määritys](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md).

ConfigurationOptions|Kuvaus|Suositus
---|---|---
AbortOnConnectFail|Kun asetus on TOSI, yhteys ei muodostaa verkon virheen jälkeen.|Arvo on EPÄTOSI ja kerro StackExchange.Redis yhdistää automaattisesti.
ConnectRetry|Kuinka monta kertaa toista yhteyksien aikana ensimmäinen muodosta.| Katso seuraavat ohjeet. |
ConnectTimeout|Ms varten aikakatkaisu Yhdistä toimintoja.| Katso seuraavat ohjeet. |

Useimmissa tapauksissa asiakkaan oletusarvot on riittävä. Voit viimeistellä havainnollistamiseen perusteella asetukset.

-   **Uudelleenyritykset**
    -   ConnectRetry ja ConnectTimeout yleisohjeita voi epäonnistua nopea ja yritä uudelleen. Tämä perustuu havainnollistamiseen ja kuinka paljon aikaa keskimääräinen Redis.txt-komennon ja saa vastausta asiakkaan kestää.
    -   Anna StackExchange.Redis automaattisesti muodostaa uudelleen yhteyden tilan tarkistaminen ja muodostaminen uudelleen itsellesi sijaan. **Vältä ConnectionMultiplexer.IsConnected-ominaisuuden avulla**.
    -   Snowballing - Joskus voi tulla ongelmana on yritetään ja tämä snowballs ja aina palauttaa. Tässä tapauksessa kannattaa harkita käyttämällä eksponentiaalisen backoff uudelleen algoritmin ohjeiden mukaisesti, [Yritä yleisohjeita](best-practices-retry-general.md) julkaisema Microsoft Patterns ja käytännöt-ryhmässä.
-   **Aikakatkaisuarvot**
    -   Harkitse havainnollistamiseen ja määritä arvot vastaavasti. Jos tallennat suuriin arvoihin, Määritä aikakatkaisun suuremmat arvot.
    -   Määritä `AbortOnConnectFail` EPÄTOSI ja anna StackExchange.Redis yhdistää puolestasi.
    -   Käytä yksittäistä ConnectionMultiplexer esiintymää sovelluksen. Voit käyttää LazyConnection Luo yhteys-ominaisuutta, jonka palauttaa yksittäisen esiintymän [ConnectionMultiplexer-luokan avulla välimuistin muodostaminen](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache)esitetyllä tavalla.
    -   Määritä `ConnectionMultiplexer.ClientName` -ominaisuuden arvoksi sovelluksen esiintymää yksilöllinen nimi vianmääritystä varten.
    -   Käytä useita `ConnectionMultiplexer` mukautetun työmääriä esiintymät.
    -   Tämän mallin voit seurata, jos sinulla on erilaisten Lataa sovellus. Esimerkki:
    -   Käytössä voi olla jokin multiplekseri käsittely suuri nuolinäppäimillä.
    -   Voit määrittää yhden multiplekseri käsittely pieni nuolinäppäimillä.
    -   Voit määrittää yhteyden aikakatkaisu eri arvot ja yritä uudelleen logiikan kunkin ConnectionMultiplexer, jota käytetään.
    -   Määritä `ClientName` ominaisuuden kunkin multiplekseri vianmäärityksen avulla.
    -   Johtaa Lisää tehostettua viive kohti `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Mitä Redis.txt välimuistin asiakkaat voivat käyttää?

Hyvien toimea Redis.txt tietoja, on monet asiakkaat tukevat useita eri kehittäminen kieliä. Asiakkaiden nykyinen luettelo-kohdassa [Redis asiakkaille](http://redis.io/clients). Katso, [miten voit käyttää Azure Redis välimuistin](cache-dotnet-how-to-use-azure-redis-cache.md) opetusohjelmia, jotka kattavat useita eri kielillä ja asiakkaiden, ja valitse haluamasi kieli Valitse kieli-Vaihtajan yläreunaan artikkelin.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>
### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Onko Azure Redis välimuistin paikallisen emulointikone?

Ei ole paikallisen emulointikone Azure Redis välimuistin, mutta voit suorittaa Redis.txt server.exe MSOpenTech version [Redis komentorivin työkalujen](https://github.com/MSOpenTech/redis/releases/) paikallisessa tietokoneessa ja muodostaa siihen samanlaisia ongelmia voi kuitenkin ilmetä pääset paikallisen välimuistin-emulaattorin seuraavan esimerkin mukaisesti.


    private static Lazy<ConnectionMultiplexer>
        lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Voit halutessasi määrittää [redis.conf](http://redis.io/topics/config) tiedoston samat tarkemmin [välimuistin oletusasetukset](cache-configure.md#default-redis-server-configuration) online Azure Redis välimuistin tarvittaessa.

<a name="cache-commands"></a>
### <a name="how-can-i-run-redis-commands"></a>Miten voit suorittaa Redis.txt komentoja?

Voit käyttää mitä tahansa komentoa luettelossa osoitteessa [Redis komennot](http://redis.io/commands#) lukuun ottamatta osoitteessa [Redis komentoja, ei tueta Azure Redis välimuistin](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache)mainitut komennot. Suorita Redis.txt komennot on useita vaihtoehtoja.

-   Jos sinulla on vakio- tai Premium välimuistin, voit suorittaa Redis.txt komennot [Redis konsolin](cache-configure.md#redis-console)avulla. Tämä on suojattu voi suorittaa Redis.txt komentoja Azure-portaalissa.
-   Voit käyttää myös Redis.txt komentorivi-työkalut. Jos haluat käyttää niitä, suorita seuraavat vaiheet.
-   Lataa [Redis komentoriviltä Työkalut](https://github.com/MSOpenTech/redis/releases/).
-   Yhteyden muodostaminen välimuistin käyttämällä `redis-cli.exe`. Välitä välimuistin-päätepisteen-avulla, -h vaihtaminen ja avaimen avulla-seuraavan esimerkin mukaisesti.
-   `redis-cli -h <your cache="" name="">
.redis.cache.windows.net -a <key>
  `
  - Huomautus Redis.txt komentoriviltä työkalut eivät toimi SSL-portti, mutta voit käyttää apuohjelman kuten `stunnel` muodostaa työkaluja suojatusti seuraamalla [Julkaisu ASP.NET istunnon tilan Provider for Redis Preview-versio](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) -blogimerkinnässä SSL-portti.

<a name="cache-reference"></a>
### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Miksi Azure Redis välimuistia ei ole on MSDN luokan kirjaston viittaus kuten joitakin Azure muiden?

Microsoft Azure Redis välimuistin perustuu Suositut Avaa lähde Redis välimuistin ja niitä voi käyttää erilaisia [Redis asiakkaat](http://redis.io/clients) , jotka ovat saatavilla monia ohjelmoinnin kielet. Kukin asiakas on oma API, joka tekee kutsuja [Redis komentoja](http://redis.io/commands)käyttämällä Redis.txt välimuistin esiintymään.

Kukin asiakas on erilainen, koska MSDN; ei ole yksi keskitetystä luokan viite sen sijaan kunkin asiakkaan ylläpitää omassa oppaat. Lisäksi oppaat-sovelluksessa on useita opetusohjelmat esittää, kuinka eri kielillä ja välimuistin asiakkaiden Azure Redis välimuistin käytön aloittaminen. Näissä Opetusohjelmissa, katso, [miten voit käyttää Azure Redis välimuistin](cache-dotnet-how-to-use-azure-redis-cache.md) ja valitse haluamasi kieli Valitse kieli-Vaihtajan yläreunaan artikkelin.


### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Azure Redis välimuistin käyttää PHP istunnon välimuistin nimellä

Määritä Pystyn käyttämään Azure Redis välimuistin PHP istunnon välimuistin yhteysmerkkijonoa Azure Redis välimuisti-esiintymässä `session.save_path`.

>[AZURE.IMPORTANT] Kun Käytä Azure Redis välimuistin PHP istunnon välimuistin, sinun täytyy URL-Osoitteen koodata käytetään muodostaa välimuistin, kuten seuraavassa esimerkissä suojaus-näppäintä.
>
>`session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
>Jos avain ei ole koodattu URL-osoite, näyttöön voi tulla poikkeuksen seuraavankaltaiselta:`Failed to parse session.save_path`

Saat lisätietoja käyttäen Redis välimuistin PHP istunnon välimuistin PhpRedis avulla [PHP istunnon käsittelijä](https://github.com/phpredis/phpredis#php-session-handler).



<a name="cache-ssl"></a>
### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Kun-SSL yhteyden muodostamisesta Redis.txt portti tulee käyttöön?

Redis palvelin ei tue SSL ruutuun ulos, mutta Azure Redis välimuistia ei. Jos olet muodostamassa yhteyttä Azure Redis välimuistin ja asiakkaan tukee SSL, kuten StackExchange.Redis, kannattaa käyttää SSL-Suojausta.

Huomaa, että-SSL portti ei ole käytössä oletusarvoisesti uudet Azure Redis välimuistin esiintymät. Jos asiakas ei tue SSL-sinun on otettava-SSL portin [määrittäminen välimuistin Azure Redis välimuistin](cache-configure.md) on artikkelissa [Access-portit](cache-configure.md#access-ports) -kohdan ohjeiden mukaisesti.

Redis työkaluja, kuten `redis-cli` eivät toimi SSL-portti, mutta voit käyttää apuohjelman kuten `stunnel` muodostaa työkaluja suojatusti seuraamalla [Julkaisu ASP.NET istunnon tilan Provider for Redis Preview-versio](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) -blogimerkinnässä SSL-portti.

Katso ohjeet lataamisesta Redis.txt-Työkalut [miten Redis.txt komentoja voi suorittaa?](#cache-commands) osan.



### <a name="what-are-some-production-best-practices"></a>Mitkä ovat tuotannon parhaista käytännöistä?

-   [StackExchange.Redis parhaat käytännöt](#stackexchangeredis-best-practices)
-   [Määritys ja käsitteitä](#configuration-and-concepts)
-   [Suorituskyvyn testaaminen](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>StackExchange.Redis parhaat käytännöt

-   Määritä `AbortConnect` FALSE, anna yhdistää automaattisesti ConnectionMultiplexer. [Lisätietoja on täällä](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
-   Uudelleen ConnectionMultiplexer – ei luoda uuden sivupyynnön. `Lazy<ConnectionMultiplexer>` Kuvio [kuvassa](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) on erittäin suositeltavaa.
-   Redis toimii parhaiten pienemmät arvot, harkitse niin silppuamisen isommaksi tietojen tuominen useita näppäimiä. [Tämä Redis keskustelun](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ)100 kt pidetään "suuri". Lue [tämän artikkelin](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) Esimerkki-ongelman, joka voi johtua suuriin arvoihin.
-   Määritä [Säievarannon asetukset](#important-details-about-threadpool-growth) aikakatkaisu välttämiseksi.
-   Käytä vähintään viiden sekunnin oletusarvo-connectTimeout. Tämä helmi StackExchange.Redis riittävästi aikaa muodostamaan yhteyden, jos kyseessä on verkko-blip uudelleen.
-   Käytössäsi on eri toiminnot liittyvät suorituskyvyn kustannukset huomioon. Esimerkiksi `KEYS` -komento on O(n) toiminto ja voidaan välttää. [Redis.io sivusto](http://redis.io/commands/) on tiedot kutakin toimintoa, joka tukee aika-monimutkaisuuden ympärille. Valitse jokaisen komennon voit tarkastella kutakin toimintoa monimutkaisuuden.

#### <a name="configuration-and-concepts"></a>Määritys ja käsitteitä

-   Käytä tuotannon Systemsin vakio- tai Premium taso. Tavallinen taso on yksi solmu järjestelmä tietoja ei ole sallittuja ja ei ole SLA. Myös käyttää vähintään C1-välimuistin. Yksinkertainen keskihajonta/koe skenaariot todella tarkoitettu C0 tallentaa.
-   Muista, että Redis.txt **Ladatun** -tietovarasto. Lue [tästä artikkelista](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) , niin, että voidaan ottaa huomioon tietojen menettämisen esiintymiskohdan voit skenaarioita.
-   Kehittää järjestelmän siten, että se voidaan käsitellä yhteyden blips [korjataan ja automaattisesti vuoksi](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Suorituskyvyn testaaminen

-   Käynnistä käyttämällä `redis-benchmark.exe` Kokeile mahdollista siirtonopeuden, ennen kuin kirjoitat omia perf Testaa. Huomautus Redis.txt-ensisijainen ei tue SSL-, jotta on [palvelun Azure-portaalissa ei SSL-portin ottaminen käyttöön](cache-configure.md#access-ports) , ennen kuin voit suorittaa testi. Katso esimerkkejä [Miten voin ensisijainen ja testaa Omat välimuistin suorituskykyä?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   Asiakastietokoneen AM käytettäviä testaaminen on oltava samalla alueella kuin Redis.txt välimuisti-esiintymä.
-   On suositeltavaa käyttää Dv2 AM sarjan asiakkaan, kun ne on parantaminen laitteiston ja antaa parhaat tulokset.
-   Varmista, että asiakkaan AM valitset on vähintään yhtä paljon tietojenkäsittely ja testaat kaistanleveyden ominaisuuksien välimuistin nimellä. 
-   Ota käyttöön VRSS asiakaskoneeseen, jos käytät Windows. [Lisätietoja on täällä](https://technet.microsoft.com/library/dn383582.aspx).
-   Premium taso Redis.txt esiintymät on paremmin verkon viive ja siirtonopeuden koska niitä parantaminen laitteiston suoritetaan suorittimen ja verkon.

<a name="cache-redis-commands"></a>
### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Mitkä ovat seikat yleisiä Redis.txt komentoja käytettäessä?

-   Älä suorita tiettyjä Redis.txt komentoja, joka kestää kauan suorittamiseen ilman tietoja vaikutus näitä komentoja.
    -   Esimerkiksi eivät toimi [NÄPPÄIMET](http://redis.io/commands/keys) -komennon tuotannon voi kestää kauan palauttaa näppäinten määrän mukaan. Redis.txt single threaded palvelin ja käsittelee komennot yksi kerrallaan. Jos sinulla on muita komentoja jälkeen NÄPPÄIMET, niitä ei käsitellä, kunnes Redis.txt käsittelee NÄPPÄIMET-komentoa. [Redis.io sivusto](http://redis.io/commands/) on tiedot kutakin toimintoa, joka tukee aika-monimutkaisuuden ympärille. Valitse jokaisen komennon voit tarkastella kutakin toimintoa monimutkaisuutta.
-   Avaimen koot - kannattaa käyttää pieni avaimen/arvot tai suuri avaimen/arvot? Yleinen odotusaika riippuu skenaariota. Jos käyttämässäsi skenaariossa vaatii suurempi sitten voit muuttaa ConnectionTimeout ja yritä arvot ja säätäminen uudelleen logiikan. Redis.txt palvelimen näkökulmasta pienemmät arvot havaitaan on suorituskyvyn parantamiseksi.
-   Tämä tarkoittaa, että et voi tallentaa suuremmat arvot Redis.txt; Sinun on otettava huomioon seuraavat seikat. Viiveet suurempia tulee korkeampi. Jos sinulla on yksi tietojoukolle, joka on suurempi ja sellainen, joka on pienempi, voit käyttää useita ConnectionMultiplexer esiintymät, jokainen määritetty on erilaisia arvoja aikakatkaisu ja yritä uudelleen, [mitä StackExchange.Redis kokoonpanoasetusten määrittäminen tehdä](#cache-configuration) edellisessä kohdassa kuvatulla tavalla.



<a name="cache-benchmarking"></a>
### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Miten voin ensisijainen ja testaa Omat välimuistin suorituskykyä?

-   [Käyttöön välimuistin diagnostiikka](cache-how-to-monitor.md#enable-cache-diagnostics) , niin voit [näytön](cache-how-to-monitor.md) välimuistin kunto. Voit tarkastella Azure portal arvot ja voit myös [ladata ja tarkastella](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) niitä valittua työkaluilla.
-   Voit käyttää Redis.txt benchmark.exe lataamaan testi Redis.txt palvelimellesi.
-   Varmistaa, että testaaminen asiakkaan ja Redis.txt välimuistin kuormituksen samalla alueella.
-   Käytä Redis.txt cli.exe ja seurata välimuistin tiedot-komennon avulla.
-   Jos että latautuvat aiheuttaa hyvin muistin pirstoutuminen sitten voit olisi skaalata välimuistin suurentaminen.
-   Katso ohjeet lataamisesta Redis.txt-Työkalut [miten Redis.txt komentoja voi suorittaa?](#cache-commands) osan.

Seuraavassa on esimerkki Redis.txt benchmark.exe käytöstä. Tarkkoja tuloksia, suorita tämä komento AM välimuistin kanssa samalla alueella.

-   Testaa Pipelined määrittäminen pyytää 1 k-paketti

    Redis.txt benchmark.exe - h **yourcache**. redis.cache.windows.net- **yourAccesskey** -t määrittäminen - n 1000000 -d 1 024 - P 50
    
-   Testaa Pipelined HAE pyytää käyttämällä 1 k-paketti. 
    Huomautus: Suorita edellä esitetyllä ensin täyttämiseen välimuistin testata joukko
    
    Redis.txt benchmark.exe - h **yourcache**. redis.cache.windows.net- **yourAccesskey** -t HAE -n 1000000 - d 1 024 - P 50

<a name="threadpool"></a>
### <a name="important-details-about-threadpool-growth"></a>Tärkeitä tietoja Säievarannon kasvu

CLR-Säievarannon on kaksi viestiketjuissa siirtyminen - "Työntekijä" ja "I/o valmistuminen portti" (eli IOCP) viestiketjuissa siirtyminen. 

-   Työntekijän viestiketjuissa siirtyminen käytetään, kun kohteita, kuten käsittely `Task.Run(…)` tai `ThreadPool.QueueUserWorkItem(…)` tavoista. Nämä viestiketjuissa siirtyminen käytetään myös eri osien CLR-, kun työmäärän sille on tehtävä, valitse tausta viestiketjun.
-   IOCP viestiketjuissa siirtyminen käytetään, kun asynkroninen IO tapahtuu (kuten lukeminen verkosta). 

Viestiketjun varanto on uuden työntekijän viestiketjuissa siirtyminen tai i/o valmistumisen viestiketjuissa siirtyminen pyydettäessä (ilman mitään rajoittimen) kunnes ohjausobjekti mistäkin säikeen "Pienin"-asetus. Oletusarvon mukaan viestiketjuissa siirtyminen vähimmäismäärä on määritetty järjestelmässä suorittimien määrän. 

Kun määrän nykyisen (varattu) viestiketjut "" vähimmäismäärä viestiketjuissa siirtyminen, Säievarannon on osumien rajoituksen nopeus, jolla on injects uuden viestiketjuissa siirtyminen, yksi viestiketjun kohti 500 millisekuntia. Tämä tarkoittaa, että jos järjestelmässä saa tehdyn työn hänen nimeään tarvitse IOCP viestiketjun purske, se voi käsitellä tukevat nopeasti. Työn burst on suurempi kuin määritetty "Pienin"-asetus, jos kuitenkin jonkin aikaa käsittelyn osa tiedoista Säievarannon odottaa jommankumman seuraavista ilmenee, kun.

1. Olemassa olevan viestiketjun tulee vapaa käsittelemään työmäärä.
1. Ei ole olemassa viestiketjun ei ole ilmaiseksi 500ms, jotta uusi viestiketjun luodaan.

Lähinnä se tarkoittaa sitä, että varattu viestiketjuissa siirtyminen määrä on suurempi kuin Min viestiketjuissa siirtyminen, kun olet todennäköisesti maksuvälineenä 500ms viive ennen kuin sovellus käsittele verkkoliikennettä. Lisäksi on tärkeää Huomaa, että kun aiemmin viestiketjun pysyy käyttämättömänä yli 15 sekunnin kuluttua (perustuu voin muista), se poistetaan ja kasvun ja Kutistuminen jakson toista.

Jos Odotamme on esimerkki virhesanomasta-StackExchange.Redis (1.0.450 luominen tai uudempi), näet, että se nyt tulostaa Säievarannon tilastotiedot (Katso IOCP ja työntekijän tietoja alla).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive, 
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0, 
    IOCP: (Busy=6,Free=994,Min=4,Max=1000), 
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

Edellä olevassa esimerkissä näet, että IOCP viestiketjun on 6 varattu viestiketjuissa siirtyminen ja järjestelmä on määritetty sallimaan 4 pienin viestiketjuissa siirtyminen. Tässä tapauksessa asiakkaan todennäköisesti tullut kaksi 500 ms viiveitä koska 6 > 4.

Huomaa, että StackExchange.Redis osumien aikakatkaisu Jos IOCP tai työntekijä viestiketjuissa siirtyminen kasvua saa rajoittanut.

### <a name="recommendation"></a>Suositus

Annetut tiedot, on suositeltavaa, että asiakkaat asettaminen IOCP ja työntekijän viestiketjuissa siirtyminen pienin määritysten arvo suurempi kuin oletusarvon järjestelmässä. Emme voi antaa one-size-fits-all ohjeita siitä, mitä tämän arvon pitäisi olla koska sovelluksen oikea arvo on liian suureksi tai pieneksi toisesta sovelluksesta. Tämä asetus myös voivat vaikuttaa muihin osiin monimutkaisia sovellusten suorituskykyä, joten voit hienosäätää niiden erityistarpeita tämä asetus on oltava kunkin asiakkaan. Hyvä aloitus on 200 tai 300, Testaa ja säätää tehosteasetuksilla tarpeen mukaan.

Miten määritetään asetus:

-   Käyttää ASP.NET-kohdassa ["minIoThreads" hakumäärityksen asetus][] - `<processModel>` web.config osan määritys. Jos käytössäsi on sisältyy Azure-sivustoja, tämä asetus ei ole näkyvät määritysten avulla. Kuitenkin yhä pitäisi voit määrittää ohjelmallisesti (Katso alla) Application_Start-menetelmä-global.asax.cs.

> **Tärkeä huomautus:** määritys-osaan arvo on *- core* asetus. Esimerkiksi jos sinulla on 4 core machine ja haluat minIOThreads asetuksen 200 suorituksen aikana, voit käyttää `<processModel minIoThreads="50"/>`.

-   ASP.NET-ulkopuolella käyttää [ThreadPool.SetMinThreads(...)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) API.

<a name="server-gc"></a>
### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Ota käyttöön palvelimen yleisen luettelon avulla opit Lisää siirtonopeuden asiakkaan StackExchange.Redis käytettäessä

Palvelimen yleisen luettelon ottaminen käyttöön voi optimoida asiakkaan ja anna parantaa suorituskykyä ja siirtonopeuden StackExchange.Redis käytettäessä. Lisätietoja palvelimen yleisen luettelon ja kuinka se otetaan käyttöön on seuraavissa artikkeleissa.

-   [Jos haluat ottaa käyttöön palvelimen yleisen luettelon](https://msdn.microsoft.com/library/ms229357.aspx)
-   [Muistista perusteet](https://msdn.microsoft.com/library/ee787088.aspx)
-   [Muistista ja suorituskyky](https://msdn.microsoft.com/library/ee851764.aspx)







<a name="cache-monitor"></a>
### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Miten kunto ja oma välimuistin suorituskykyä valvoa?

Microsoft Azure Redis välimuistin esiintymät voidaan valvoa [Azure portal](https://portal.azure.com). Voit Näytä arvot, kiinnittäminen arvot kaaviot Startboard, päivämäärä ja kellonaika alueen seuranta kaavioiden mukauttaminen, lisääminen ja arvot poistaminen kaaviot ja määrittämään ilmoituksia, kun tietyt ehdot täyttyvät. Lisätietoja on artikkelissa [Näytön Azure Redis välimuistin](cache-how-to-monitor.md).

**Tuki + vianmääritys** osan Redis välimuistin **asetukset** -sivu sisältää myös useita työkaluja seuranta ja että tallentaa vianmäärityksessä. 

-   **Vianmääritys** on yleisiä ongelmia ja strategioita ratkaisemista koskevia tietoja.
-   **Valvontalokien** tietoja välimuistin-toimintoja. Voit käyttää suodatusta Laajenna tämä näkymä sisältää muita resursseja.
-   **Resurssin kunto** seuraa resurssi ja ilmoittaa, jos se toimii odotetulla tavalla. Saat lisätietoja Azure resurssin kunto-palvelun [Azure-kunto Resurssikatsaus](../resource-health/resource-health-overview.md).
-   **Uusi tue pyyntöä** vaihtoehtoja Avaa välimuistin tukipyynnön.

Kyseisten työkalujen avulla voit tarkkailla Azure Redis välimuistin esiintymät ja välimuistiin sovellusten hallinnan avulla. Lisätietoja on artikkelissa [tuki- ja asetusten vianmääritys](cache-configure.md#support-amp-troubleshooting-settings).

### <a name="my-cache-diagnostics-storage-account-settings-changed-what-happened"></a>Välimuistin diagnostiikka tallennustilan tilin asetusten muuttunut, mitä on tapahtunut?

Tallentaa saman alue-ja tilauksen yhteiset diagnostiikka tallennustilan asetukset, ja jos määritys on muutettu (diagnostiikka käytössä/poissa käytöstä tai muuttamalla tallennustilan-tili) se koskee kaikki tilauksen tallentaa, jotka ovat alueen. Jos välimuistin diagnostiikan asetuksia ovat muuttuneet, tarkista, jos samassa tilaus ja alueen toisen välimuistin diagnostiikan asetuksia ovat muuttuneet. Tarkista yksi tapa on varten välimuistin valvontalokien tarkasteleminen `Write DiagnosticSettings` tapahtuma. Lisätietoja valvontalokien käsittelemisestä on artikkelissa [tapahtumien tarkasteleminen ja valvontalokit](../monitoring-and-diagnostics/insights-debugging-with-events.md) ja [valvonta toimintojen resurssien hallinta](../resource-group-audit.md). Lisätietoja Azure Redis välimuistin tapahtumien seuranta-kohdassa [Toiminnot ja ilmoitukset](cache-how-to-monitor.md#operations-and-alerts).

### <a name="why-is-diagnostics-enabled-for-some-new-caches-but-not-others"></a>Miksi Diagnostiikka on käytössä joitakin uuden tallentaa, mutta ei muiden?

Tallentaa saman alue-ja tilauksen jakaminen diagnostiikka tallennustilan samoja asetuksia. Jos luot uuden välimuistin saman alue ja toinen välimuistin, jossa on käytössä diagnostiikka-tilauksen, diagnostiikka otettu käyttöön uusi välimuisti käyttämällä samoja asetuksia.


<a name="cache-timeouts"></a>
### <a name="why-am-i-seeing-timeouts"></a>Miksi aikakatkaisu?

Aikakatkaisu tapahtua, joiden avulla voit jutella Redis.txt-asiakassovelluksessa. Suurin osa Redis.txt palvelin ei ole kadota itsestään. Kun komennon lähetetään Redis.txt-palvelimeen, komento on jonossa ja Redis.txt palvelimen myöhemmin vastataan komento ja suorittaa sen. Kuitenkin asiakas voi kadota itsestään vaihdon ja jos se ei poikkeuksen korotetaan puheluja reunassa. Lisätietoja aikakatkaisu vianmäärityksestä on artikkelissa [asiakkaan puoli vianmääritys](cache-how-to-troubleshoot.md#client-side-troubleshooting) ja [StackExchange.Redis aikakatkaisu poikkeukset](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).


<a name="cache-disconnect"></a>
### <a name="why-was-my-client-disconnected-from-the-cache"></a>Miksi asiakkaan katkesi välimuistin?

Seuraavassa on joitakin yleisiä välimuistin Katkaise syy.

-   Asiakkaan syyt
    -   Asiakassovellus on otettava käyttöön uudelleen.
    -   Asiakassovellus suorittanut skaalauksen toiminnon.
        -   Cloud Services-tai verkkosovelluksissa Tämä saattaa johtua Automaattinen skaalaus.
    -   Asiakaspuolen verkko kerroksen muutettu.
    -   Lyhytkestoisia virheitä asiakas- tai Verkkosolmut asiakkaan ja palvelimen välillä.
    -   Kaistanleveyden raja-arvojen on saavutettu.
    -   Suorittimen sidottu toimintojen noudatit liian kauan.
-   Palvelinpuolen syitä
    -   Vakio välimuistin ojentamassa Azure Redis välimuisti-palvelun Omat korvaava-ensisijainen solmu toissijaisen solmun.
    -   Azure on korjataan esiintymä, jos välimuisti on otettu käyttöön
        -   Tämä voi olla Redis.txt palvelimen päivityksiä tai Yleiset AM ylläpito.







### <a name="which-azure-cache-offering-is-right-for-me"></a>Mitä Azure välimuistin tarjoaa sopii minulle parhaiten?

>[AZURE.IMPORTANT]Edellinen vuosi- [ilmoitus](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/), kuten Azure hallitun välimuisti-palvelun ja Azure-roolin välimuisti-palvelun poistua-2016 30 marraskuun. Tutustu Suosituksena on [Azure Redis välimuistin](https://azure.microsoft.com/services/cache/). Lisätietoja siirtämisestä on artikkelissa [hallitut välimuisti-palvelun Azure Redis välimuistin artikkelista](cache-migrate-to-redis.md).

### <a name="azure-redis-cache"></a>Azure Redis.txt välimuisti
Azure Redis välimuisti on yleensä käytettävissä koko on enintään 53 gt ja saatavuus SLA 99,9 %. Uusi [premium taso](cache-premium-tier-intro.md) on koot enintään 530 Gigatavua ja klusterointi, VNET ja pysyvyyttä 99,9 % SLA ja tuki.

Azure Redis välimuistin antaa asiakkaille voi käyttää suojattuun, erillinen Redis.txt välimuistiin, hallinnassa on käytössä Microsoft. Tämän tarjouksen saat monipuoliset ominaisuudet ja ekosysteemiin Redis.txt, ja luotettava isännöinnin ja Microsoftin seuranta.

Toisin kuin perinteisen tallentaa joka käsitellä avaimen arvon tietoparin Redis.txt on suosittu sen erittäin performant tietotyyppien. Redis myös tukee käynnissä atominen toimenpiteet näitä tiedostotyyppejä, kuten liitettäväksi merkkijonon; kasvavat hash; arvo luettelo, valitseminen tietojenkäsittely määrittäminen leikkaus, unionin ja ero; tai käytön jäsenen kanssa korkein luokittelu lajiteltu määrittäminen. Muita ominaisuuksia ovat tapahtumia, kirja ja sub, Lua komentosarjat-näppäimiä rajoitetun--elinaika- ja tehdä Redis.txt tavoin perinteinen välimuistin asetukset tuki.

Toisen tarjoama Redis.txt menestyksen salaisuus on kunnossa, eloisia Avaa lähde ekosysteemissä sen ympärille. Tämä näkyy Redis.txt asiakkaiden käytettävissä monipuolisen joukko useilla kielillä. Näin sitä voidaan käyttää lähes minkä tahansa luot sisältyy Azure työmäärää. 

Katso lisätietoja Azure Redis välimuistin käytön aloittamisesta, [Voit käyttää Azure Redis välimuistin](cache-dotnet-how-to-use-azure-redis-cache.md) ja [Azure Redis välimuistin asiakirjat](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="managed-cache-service"></a>Hallittujen välimuisti-palvelu
[Hallitun välimuistin palvelu on määritetty poistua 2016 30 marraskuun.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

### <a name="in-role-cache"></a>Roolin-välimuisti
[Rooli-välimuistin asetetaan poistua 2016 30 marraskuun.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)





["minIoThreads" hakumäärityksen asetus]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
