<properties
   pageTitle="Virhe tilassa analyysi | Microsoft Azure"
   description="Ohjeet virhe-tilassa analysointia cloud ratkaisuja Azure perusteella."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="mwasson"/>

# <a name="azure-resiliency-guidance-failure-mode-analysis"></a>Azure vikasietoisuudelle ohjeet: Virhe tilassa analyysi

Virhe tilassa analysointi (FMA) on prosessin etsimisen vikasietoisuudelle järjestelmään, tunnistamalla mahdollista virheen pisteiden järjestelmässä. FMA pitäisi olla osa arkkitehtuuri ja rakenne-vaiheiden, niin, että voit luoda virheen palautus järjestelmään alusta.

Yleistä prosessin Järjestä FMA näin: 

1. Määritä kaikkien osien järjestelmässä. Lisää ulkoisia riippuvuudet, kuten tunnistetietojen toimittajat ja kolmannen osapuolen palvelujen.   

2. Kullekin osalle tunnistaa mahdolliset virheet, joita voi ilmetä. Yksittäinen komponentti voi olla useita virhe-tilassa. Voit esimerkiksi harkitse luku virheet, ja kirjoittaa virheet erikseen, koska vaikutus ja mahdollista ongelman pienentämistavat on erilainen.

3. Arvioi virheen moodin sen yleinen riski mukaan. Ota huomioon seuraavat seikat:  

    - Mikä on todennäköistä, että virheen. On suhteellisen yleisiä? Extrememly harvinaisten? Sinun ei tarvitse tarkka numeroita; tarkoitus on auttaa Järjestä prioriteetti.

    - Mikä on sovelluksen saatavuus, tietojen menettämisen, raha kustannusten ja business häiriöt vaikutus? 

4. Virhe kunkin tilan määrittävät, kuinka sovellus vastata ja palauttaminen. Harkitse kompromissien kustannus- ja monimutkaisuus.   

Tässä artikkelissa on mahdollisen virheen tilasta luetteloon ja niiden ongelman pienentämistavat FMA prosessin-kohdan. Luettelo on järjestetty tekniikka tai Azure-palvelu ja Yleiset-luokka sovellustason rakenne. Luettelo ei ole täydellinen, mutta kattaa monet core Azure palvelut. 


## <a name="app-service"></a>Sovelluksen-palvelu

### <a name="app-service-app-shuts-down"></a>Sovelluksen palvelun sovellus suljetaan.

**Tunnistus**. Mahdollisia syitä:

- Odotettu sulkeminen
    
    - Sovelluksen ja sulkee operaattori Voit käyttää esimerkiksi Azure portaalin.

    - Sovellus on lataus, koska se on ollut käyttämättömänä. (Vain, jos `Always On` asetus on poistettu käytöstä.)

- Odottamattoman sulkemisen jälkeen

    - Sovellus kaatuu.

    - Sovelluksen palvelun AM esiintymä ei ole käytettävissä.


Application_End kirjaaminen sieppaa app toimialueen Sammuta (Pehmeä prosessin kaatua), ja se on ainoa tapa suodattaa sovelluksen toimialueen sammutukset.

**Palautus**

- Jos pitäisi olla sulkeminen, sulje tilanteen sovelluksen sulkeminen tapahtuman avulla. Esimerkiksi ASP.NET, valitse Käytä `Application_End` menetelmällä.

- Jos sovellus ei lataus, kun käyttämättömänä, se käynnistetään automaattisesti seuraavan pyynnön. Voit kuitenkin selvittää "kylmän Käynnistä" kustannus. 

- Voit estää sovelluksen lataamista poistetaan samalla, kun se on ollut käyttämättömänä, ota käyttöön `Always On` määrittäminen web App-sovelluksessa. Katso [Azure App palvelun Configure web App-sovellusten][app-service-configure].

- Jos haluat estää operaattori sovellus suljetaan, määritä resurssin lukko kanssa `ReadOnly` taso. [Lukitse]resursseissa Azure resurssien hallinnan[rm-locks].

- Jos sovellus kaatuu tai se on sovelluksen palvelun AM ei ole käytettävissä, sovelluksen palvelu käynnistyy automaattisesti sovellus. 


**Diagnostiikka**. Sovelluksen lokit ja web-palvelimen lokit. Katso [Ota lokiin kirjaaminen web Apps-sovellukset Azure App palvelun diagnostiikka][app-service-logging].


### <a name="a-particular-user-repeatedly-makes-bad-requests-or-overloads-the-system"></a>Tietyn käyttäjän on virheellinen pyynnöt tai ylikuormittaminen järjestelmän toistuvasti. 

**Tunnistus**. Käyttäjien todentamiseen ja Sisällytä Käyttäjätunnus sovelluksen lokit.

**Palautus**

- [Azure API] hallinnalla[ api-management] rajoituksen pyyntöihin käyttäjältä. Katso [Lisäasetukset pyynnön Azure API hallinnan rajoittaminen][api-management-throttling]

- Estä käyttäjää.

**Diagnostiikka**. Kirjaudu todennus pyynnöt.


### <a name="a-bad-update-was-deployed"></a>Virheelliset päivitys on otettu käyttöön.

**Tunnistus**. Sovelluksen tarkkailla palvelun Azure-portaalissa (katso [näytön Azure web app suorituskyvyn][app-insights-web-apps]) tai toteuta [kunto päätepisteen seurantaa kuvio][health-endpoint-monitoring-pattern].

**Palauttaminen**. Käytä useita [käyttöönoton paikkojen] [ app-service-slots] ja palata viimeksi toiminut käyttöönotto. Lisätietoja on artikkelissa [Basic web application viittaus arkkitehtuuri][ra-web-apps-basic].


## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="openid-connect-oidc-authentication-fails"></a>OpenID yhteyden (OIDC)-todennus epäonnistuu.

**Tunnistus**. Mahdolliset virheen tilat ovat seuraavat:

1. Azure AD ei ole käytettävissä tai ei voi siirtyä vuoksi verkko-ongelma. Uudelleenohjaus päätepisteelle todennus epäonnistuu, ja OIDC middleware ilmoittaa poikkeuksen.
2. Azure AD-vuokraajan ei ole. Todennus-päätepisteen uudelleenohjaus palauttaa HTTP-virhekoodin ja OIDC middleware ilmoittaa poikkeuksen.
3. Käyttäjä ei voi todentaa. Tunnistus ei ole strategia on tarpeen, Azure AD käsittelee kirjautuminen epäonnistuu.

**Palautus**

1. Todellinen middleware käsittelemättömän poikkeuksia.
2. Käsittele `AuthenticationFailed` tapahtumat.
3. Siirtää käyttäjän virhesivun.
4. Käyttäjän uudelleenyritykset.


## <a name="azure-search"></a>Azure haku

### <a name="writing-data-to-azure-search-fails"></a>Azure haun tietojen kirjoittaminen epäonnistuu.

**Tunnistus**. Todellisen `Microsoft.Rest.Azure.CloudException` virheitä.

**Palautus**

[Etsi .NET SDK] [ search-sdk] yrittää automaattisesti suorittaa lyhytkestoisia virheen jälkeen uudelleen. Asiakkaan SDK minkä tahansa poikkeukset käsitellään lyhytaikainen virheet.

Yritä oletuskäytäntö käyttää eksponentiaalisen Edellinen pois päältä. Jos haluat käyttää eri uudelleen käytännön, soita `SetRetryPolicy` - `SearchIndexClient` tai `SearchServiceClient` luokka. Lisätietoja on artikkelissa [Automaattinen yrittää suorittaa uudelleen][auto-rest-client-retry].

**Diagnostiikka**. Käytä [Etsi liikenne Analytics][search-analytics].


### <a name="reading-data-from-azure-search-fails"></a>Tietojen lukeminen Azure haun epäonnistuu.

**Tunnistus**. Todellisen `Microsoft.Rest.Azure.CloudException` virheitä.

**Palautus**

[Etsi .NET SDK] [ search-sdk] yrittää automaattisesti suorittaa lyhytkestoisia virheen jälkeen uudelleen. Asiakkaan SDK minkä tahansa poikkeukset käsitellään lyhytaikainen virheet.

Yritä oletuskäytäntö käyttää eksponentiaalisen Edellinen pois päältä. Jos haluat käyttää eri uudelleen käytännön, soita `SetRetryPolicy` - `SearchIndexClient` tai `SearchServiceClient` luokka. Lisätietoja on artikkelissa [Automaattinen yrittää suorittaa uudelleen][auto-rest-client-retry].

**Diagnostiikka**. Käytä [Etsi liikenne Analytics][search-analytics].



## <a name="cassandra"></a>Cassandra


### <a name="reading-or-writing-to-a-node-fails"></a>Luettavaksi tai muut solmu epäonnistuu.

**Tunnistus**. Todellinen poikkeuksen. .NET-asiakkaat, tämä yleensä on `System.Web.HttpException`. Toisen asiakkaan ehkä muuntyyppisten poikkeuksen.  Lisätietoja on artikkelissa [Cassandra Virheenkäsittely valmis oikealle](http://www.datastax.com/dev/blog/cassandra-error-handling-done-right).

**Palautus**

- Kunkin [asiakkaan Cassandra](https://wiki.apache.org/cassandra/ClientOptions) on oma uudelleen käytännöt ja ominaisuuksia. Lisätietoja on artikkelissa [Cassandra Virheenkäsittely valmis oikealle][cassandra-error-handling].
- Teline-aware käyttöönoton käyttäminen tietojen solmujen vika toimialueilla jaetaan.
- Käyttöönotto paikallisen quorum yhdenmukaisuuden alueille. Jos muu kuin lyhytaikainen virhe ilmenee, epäonnistua toisen alueen yli.

**Diagnostiikka**. Sovelluksen lokit


## <a name="cloud-service"></a>Pilvipalvelussa

### <a name="web-or-worker-roles-are-unexpectedlybeing-shut-down"></a>Web- tai työntekijä roolit ovat odottamatta suljetaan.

**Tunnistus**. [RoleEnvironment.Stopping] [ RoleEnvironment.Stopping] tapahtuma käynnistyy.

**Palauttaminen**. Ohita [RoleEntryPoint.OnStop] [ RoleEntryPoint.OnStop] tavan tilanteen Puhdista. Lisätietoja on artikkelissa [Oikealle tapa käsitellä Azure OnStop tapahtumien] [ onstop-events] (blogi).


## <a name="documentdb"></a>DocumentDB

### <a name="reading-data-from-documentdb-fails"></a>Tietojen lukeminen DocumentDB epäonnistuu.

**Tunnistus**. Todellisen `System.Net.Http.HttpRequestException` tai `Microsoft.Azure.Documents.DocumentClientException`. 

**Palautus**

- SDK uudelleenyrityksiä automaattisesti epäonnistuneen yrityksen jälkeen. Asettaaksesi uudelleenyritykset ja suurin odotusaika määrän määrittäminen `ConnectionPolicy.RetryOptions`. Poikkeuksia, jotka asiakas aiheuttaa ovat joko lisäksi uudelleen-käytäntö tai eivät ole lyhytkestoisia virheitä. 

- Jos DocumentDB throttles asiakas, se palauttaa virheen HTTP 429. Tilakoodin Kuittaa sisään `DocumentClientException`. Jos ilmenee virhe 429 jatkuvasti, harkitse DocumentDB-sivustokokoelman siirtonopeuden arvo.

- Replikoida DocumentDB tietokannan kahden tai useamman alueen yli. Kaikki replikat ovat luettavissa. Käytä asiakkaan SDK: T, Määritä `PreferredLocations` parametri. Tämä on järjestetty luettelo Azure alueiden. Kaikki lukee lähetetään ensimmäinen käytettävissä alue-luettelossa. Jos kutsu epäonnistuu, asiakkaan Kokeile muiden alueiden luettelossa tilaus. Lisätietoja on artikkelissa [kehitysmaiden monille DocumentDB tilien][docdb-multi-region].

**Diagnostiikka**. Kirjaudu asiakaspuolen kaikki virheet.


### <a name="writing-data-to-documentdb-fails"></a>Kirjoittaminen DocumentDB epäonnistuu.

**Tunnistus**. Todellisen `System.Net.Http.HttpRequestException` tai `Microsoft.Azure.Documents.DocumentClientException`. 

**Palautus**

- SDK uudelleenyrityksiä automaattisesti epäonnistuneen yrityksen jälkeen. Voit määrittää uudelleenyritykset ja suurin odotusaika määrä, Määritä `ConnectionPolicy.RetryOptions`. Poikkeuksia, jotka asiakas aiheuttaa ovat joko lisäksi uudelleen-käytäntö tai eivät ole lyhytkestoisia virheitä. 

- Jos DocumentDB throttles asiakas, se palauttaa virheen HTTP 429. Kuittaa sisään tilakoodin `DocumentClientException`. Jos ilmenee virhe 429 jatkuvasti, harkitse DocumentDB sivustokokoelman siirtonopeuden arvo.

- Replikoida DocumentDB tietokannan kahden tai useamman alueen yli. Jos ensisijainen alueen epäonnistuu, kirjoittaa voi muuntaa toisen alueen. Voit käynnistää automaattisesti myös manuaalisesti. SDK ei automaattinen etsintä- ja reititys, jotta sovelluksen koodin työkirja toimii jälkeen automaattisesti. Automaattisesti ajan (yleensä minuuttia) kirjoitustoiminnot on suurempi viive kuin SDK löytää uusi kirjoitus-alue. Lisätietoja on artikkelissa [kehitysmaiden monille DocumentDB tilien][docdb-multi-region].

- Varmistuksena jatkuvat asiakirjan varmuuskopion jonossa ja käsitellä jonossa myöhemmin.

**Diagnostiikka**. Kirjaudu asiakaspuolen kaikki virheet.


## <a name="elasticsearch"></a>Elasticsearch

### <a name="reading-data-from-elasticsearch-fails"></a>Tietojen lukeminen Elasticsearch epäonnistuu.

**Tunnistus**. Suodattaa tietyn [Elasticsearch asiakkaan] tarvittavat poikkeuksen[ elasticsearch-client] käytössä. 

**Palautus**

- Käytä uudelleen järjestelmä. Kukin asiakas on oma uudelleen käytännöt. 

- Ottaa käyttöön useita Elasticsearch solmujen ja käyttää replikoinnin suuren käytettävyyden.

Lisätietoja on artikkelissa [Käynnissä Elasticsearch Azure-][elasticsearch-azure].

**Diagnostiikka**. Voit käyttää valvontatyökaluja Elasticsearch tai Kirjaudu asiakaspuolen kaikki virheet paketti. "Seuranta-kohdassa [Käynnissä Elasticsearch Azure-][elasticsearch-azure].


### <a name="writing-data-to-elasticsearch-fails"></a>Kirjoittaminen Elasticsearch epäonnistuu.

**Tunnistus**. Suodattaa tietyn [Elasticsearch asiakkaan] tarvittavat poikkeuksen[ elasticsearch-client] käytössä.  

**Palautus**

- Käytä uudelleen järjestelmä. Kukin asiakas on oma uudelleen käytännöt. 
 
- Jos sovellusta hyväksyt rajoitetun yhdenmukaisuuden taso, harkitse kirjoittaminen kanssa `write_consistency` asettaminen `quorum`.

Lisätietoja on artikkelissa [Käynnissä Elasticsearch Azure-][elasticsearch-azure].

**Diagnostiikka**. Voit käyttää valvontatyökaluja Elasticsearch tai Kirjaudu asiakaspuolen kaikki virheet paketti. "Seuranta-kohdassa [Käynnissä Elasticsearch Azure-][elasticsearch-azure].


## <a name="queue-storage"></a>Jonon tallennustila

### <a name="writing-a-message-to-azure-queue-storage-fails-consistently"></a>Kirjoittaminen viestin johdonmukaisesti Azure jonon tallennustilan epäonnistuu.

**Tunnistus**. Kun *N* uudelleen yritykset, kirjoitus epäonnistuu silti.

**Palautus**

- Tiedot tallennetaan paikallisen välimuistin ja välittää säilöön kirjoituksia myöhemmin, kun palvelu on käytettävissä.
- Luo toissijainen jonossa ja kirjoittaa kyseisen jonon, jos ensisijainen jonossa ei ole käytettävissä.

**Diagnostiikka**. [Tallennustilan arvot]käyttää[storage-metrics].


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Sovellus ei voi käsitellä jonossa tietty sähköpostiviesti. 

**Tunnistus**. Sovelluksen tietyn. Esimerkiksi viesti sisältää virheellisiä tietoja tai liiketoimintalogiikan epäonnistuu jostain syystä. 

**Palautus**

Siirrä viesti erillisessä jonossa. Suorita erillinen prosessi tarkistaa, että jonossa olevat viestit.

Harkitse Azure Bus Messaging olevien, joka sisältää [jonon Perille toimittamattomat] [ sb-dead-letter-queue] tähän tarkoitukseen toimintoja.

Huomautus: Jos käytössäsi on tallennustilan olevien WebJobs, WebJobs SDK sisältää valmiita torjuttujen viestin käsittely. Opettele [käyttämään Azure jonon tallennustilan kanssa WebJobs SDK][sb-poison-message].

**Diagnostiikka**. Käytä sovelluksen kirjaaminen.


## <a name="redis-cache"></a>Redis välimuisti

### <a name="reading-from-the-cache-fails"></a>Lukuruudun välimuistista epäonnistuu.

**Tunnistus**. Todellisen `StackExchange.Redis.RedisConnectionException`.

**Palautus**

1. Valitse lyhytkestoisia virheet uudelleen. Azure Redis.txt välimuistin tukee valmiin uudelleen – Katso [Redis välimuistin uudelleen ohjeet][redis-retry].
2. Käsittele lyhytaikainen virheet epäonnistumisten ja kuuluvat takaisin alkuperäiseen tietolähteeseen.

**Diagnostiikka**. Käytä [Redis välimuistin diagnostiikka][redis-monitor].


### <a name="writing-to-the-cache-fails"></a>Välimuistin kirjoittaminen epäonnistuu.

**Tunnistus**. Todellisen `StackExchange.Redis.RedisConnectionException`.

**Palautus**

1. Valitse lyhytkestoisia virheet uudelleen. Azure Redis.txt välimuistin tukee valmiin uudelleen – Katso [Redis välimuistin uudelleen ohjeet][redis-retry].
2. Jos virhe on muu kuin lyhytaikainen, ohita se ja antaa muiden tapahtumien kirjoittaa välimuistiin myöhemmin.

**Diagnostiikka**. Käytä [Redis välimuistin diagnostiikka][redis-monitor].


## <a name="sql-database"></a>SQL-tietokantaan

### <a name="cannot-connect-to-the-database-in-the-primary-region"></a>Ensisijainen alueen tietokantaan ei voi muodostaa yhteyttä.

**Tunnistus**. Yhteyden muodostaminen epäonnistuu.

**Palautus**

Edellytyksenä: Tietokanta on määritettävä aktiivinen geo-replikoinnin. Katso [SQL, tietokanta, aktiivinen Geo-replikoinnin][sql-db-replication].

- Kyselyjen Lue toissijainen replikasta. 
- Lisää ja päivitykset manuaalisesti ei onnistu päälle toissijainen replikan. [Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokannan]artikkelissa[sql-db-failover].

Se käyttää eri yhteysmerkkijonon, joten sinun on päivitettävä yhteysmerkkijono-sovelluksessa.


### <a name="client-runs-out-of-connections-in-the-connection-pool"></a>Asiakkaan loppuu yhteysvarannon yhteyksiä.

**Tunnistus**. Todellisen `System.InvalidOperationException` virheitä. 

**Palautus**

- Yritä uudelleen.
- Kuin lieventämissuunnitelma eristää yhteyden jakavat käyttötapaus-, niin, että käyttötapauksen ei johtoryhmän kaikki yhteydet.
- Suurentaa yhteyden jakavat.

**Diagnostiikka**. Sovelluksen lokit.


### <a name="database-connection-limit-is-reached"></a>Tietokannan yhteyksien enimmäismäärä on saavutettu. 

**Tunnistus**. Azure SQL-tietokantaan rajoittaa samanaikainen työntekijöiden, kirjautumiset ja istuntoja. Raja-arvot vaihtelevat palvelutaso. Lisätietoja on artikkelissa [Azure SQL-tietokanta resurssien rajoitukset][sql-db-limits].

Näiden virheiden korjaaminen vianmäärityksen avulla todellisen `System.Data.SqlClient.SqlException` ja tarkista arvon `SqlException.Number` SQL-virhekoodin. Asianmukaisten virhekoodit luettelo on artikkelissa [SQL-virhekoodit SQL-tietokantaan asiakassovellukset: tietokannan yhteysvirhe ja muiden ongelmien][sql-db-errors].

**Palauttaminen**. Nämä virheet pidetään lyhytkestoisia, joten yritetään voi ratkaista ongelman. Jos valitset johdonmukaisesti virheet, harkitse skaalaus tietokanta.

**Diagnostiikka**. - [Sys.event_log] [ sys.event_log] kysely palauttaa onnistuneen tietokantayhteyksiä epäonnistuneita ja lukkiutuu.

- Luo [hälytyksen] [ azure-alerts] varten epäonnistui yhteydet.

- [SQL-tietokantaan] valvoa[ sql-db-audit] ja tarkista, onko epäonnistui kirjautumiset.


## <a name="service-bus-messaging"></a>Palvelun Bus viestintä

### <a name="reading-a-message-from-a-service-bus-queue-fails"></a>Viestin lukeminen palvelun Bus jonon epäonnistuu.

**Tunnistus**. Todellinen asiakkaan SDK poikkeukset. Palvelun Bus poikkeuksia perus-luokka on [MessagingException][sb-messagingexception-class]. Jos virhe on lyhytkestoisia, `IsTransient` ominaisuuden arvo on TOSI. 

Lisätietoja on artikkelissa [palvelun Bus messaging poikkeukset][sb-messaging-exceptions].

**Palautus**

1. Valitse lyhytkestoisia virheet uudelleen. Katso [Palvelun Bus uudelleen ohjeita][sb-retry].

2. Viestit, joissa ei voi toimittaa minkä tahansa vastaanottaja sijoitetaan *Perille toimittamattomat jonossa*. Tämä jono avulla voit nähdä, mitkä viestejä ei vastaanoteta. Ei ole automaattinen uudelleenjärjestäminen Perille toimittamattomat jonon. Viestit ovat olemassa, kunnes erikseen noudat. Katso [yleiskuvaus palvelun Bus Perille toimittamattomat olevien][sb-dead-letter-queue].


### <a name="writing-a-message-to-a-service-bus-queue-fails"></a>Viestin kirjoittamisen palvelun Bus jonon epäonnistuu.

**Tunnistus**. Todellinen asiakkaan SDK poikkeuksia. Palvelun Bus poikkeuksia perus-luokka on [MessagingException][sb-messagingexception-class]. Jos virhe on lyhytkestoisia, `IsTransient` ominaisuuden arvo on TOSI. 

Lisätietoja on artikkelissa [palvelun Bus messaging poikkeukset][sb-messaging-exceptions].

**Palautus**

1. Palvelun Bus asiakas-yrittää suorittaa automaattisesti uudelleen lyhytkestoisia virheiden jälkeen. Oletusarvon mukaan se käyttää eksponentiaalisen Edellinen pois päältä. Suurin uudelleen Laske tai suurin aikakatkaisuajan jälkeen asiakkaan ilmoittaa poikkeuksen. Lisätietoja [Palvelun Bus uudelleen ohjeet][sb-retry].

2. Jos jonon kiintiö on ylitetty-asiakas ilmoittaa [QuotaExceededException][QuotaExceededException]. Poikkeuksen sanoma antaa enemmän tietoja. Tyhjennä joitakin sähköpostiviestejä jonossa ennen kuin yrität uudelleen ja kannattaa käyttää katkaisin kuvion välttämiseksi jatkuvan uudelleenyritykset, kun kiintiö on ylitetty. Varmista myös, että [BrokeredMessage.TimeToLive] -ominaisuutta ei ole määritetty liian suuri. 

3. Alueella, vikasietoisuudelle voidaan parantaa [osioitua olevien]tai aiheet[sb-partition]. Osittamattoman jonossa tai aiheen määritetään yksi tekstiviesti-kaupasta. Jos tekstiviesti myymälän ei ole käytettävissä, kaikki toiminnot jonossa tai aiheen epäonnistuu. Osioitu jonossa tai aiheen on osioitu yli useita tekstiviesti stores. 

4. Lisää vikasietoisuudelle Luo kaksi palvelun Bus nimitilan eri alueilla ja toistaa viestit. Voit käyttää active replikoinnin tai passiivisen replikoinnin.

    - Aktiivinen replikoinnin: asiakas lähettää kaikkiin lähetettäviin sekä olevien. Vastaanottajan seuraa sekä olevien. Tunnisteen viestit, joissa on yksilöllinen, joten asiakas, voit hylätä viesteistä.

    - Passiivinen replikoinnin: asiakkaan valitaan, viesti lähetetään yksi jono. Jos virheitä ilmenee, asiakkaan kuuluu takaisin muiden jonossa. Vastaanottajan seuraa sekä olevien. Tätä tapaa vähentää kaksoiskappaleiden lähetetyt viestit. Vastaanottaja on kuitenkin yhä käsiteltävä viesteistä.

    Lisätietoja on artikkelissa [GeoReplication otoksen] [ sb-georeplication-sample] ja [parhaita käytäntöjä turvallisuustarkoituksiin sovellusten vastaan palvelun Bus katkokset ja katastrofit][sb-outages].


### <a name="duplicate-message"></a>Monista viesti.

**Tunnistus**. Tarkistaa `MessageId` ja `DeliveryCount` viestin ominaisuudet.

**Palautus**

- Jos mahdollista suunnitella on idempotent käsittelyt viesti. Muussa tapauksessa tallentaa viestit, joissa käsitellään jo ja tunnus tarkistaminen ennen viestin käsittelyn viestin tunnukset.

- Ota käyttöön kaksoiskappaleiden, luomalla jonossa kanssa `RequiresDuplicateDetection` arvo on TOSI. Kun tämä asetus, palvelun Bus poistaa automaattisesti samojen lähetettävän viestin `MessageId` edellisen viestin.  Ota seuraavat seikat huomioon:

    -  Tämän asetuksen voit estää viesteistä ne otetaan jonossa. Ei estä vastaanotin käsittelemästä saman viestin useita kertoja.

    - Kaksoiskappaleiden on aika-ikkuna. Jos kaksoiskappale lähetetään lisäksi tässä ikkunassa, sitä ei tunnisteta.

**Diagnostiikka**. Kirjaudu kaksoiskappaleet viestejä.


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Sovellus ei voi käsitellä jonossa tietty sähköpostiviesti. 

**Tunnistus**. Sovelluksen tietyn. Esimerkiksi viesti sisältää virheellisiä tietoja tai liiketoimintalogiikan epäonnistuu jostain syystä. 

**Palautus**

Harkitse virheen kahdessa tilassa. 

- Vastaanottajan havaitsee virheen. Tässä tapauksessa siirtää viestin perille toimittamattomat jonossa. Suorita myöhemmin erillinen prosessi tutkia Perille toimittamattomat jonossa olevat viestit.

- Vastaanottajan epäonnistuu keskellä viestin käsittelyn &mdash; esimerkiksi käsittelemättömän poikkeuksen vuoksi. Tässä tapauksessa käsittelemään käyttää `PeekLock` tilassa. Tässä tilassa, jos lukitus vanhenee, viesti ei ole käytettävissä muita vastaanottajia. Jos viesti on suurempi kuin suurin toimitus-määrä tai--elinaika-viesti siirretään automaattisesti Perille toimittamattomat jonossa.

Lisätietoja on artikkelissa [Yleistä palvelun Bus Perille toimittamattomat olevien][sb-dead-letter-queue].

**Diagnostiikka**. Aina, kun sovellus siirtyy viestin perille toimittamattomat jonossa, kirjoittaa tapahtuman sovelluksen lokit.


## <a name="service-fabric"></a>Palvelun kangasta

### <a name="a-request-to-a-service-fails"></a>Pyyntö palveluun epäonnistuu.

**Tunnistus**. Palvelu palauttaa virheen.

**Palautus**

- Etsi välityspalvelimen uudelleen (`ServiceProxy` tai `ActorProxy`) ja palvelun/toimija-menetelmän kutsu uudelleen. 

- **Tilallisten palvelu**. Rivittää tapahtuman luotettava sivustokokoelmat-toimintoja. Jos virheitä ilmenee, tapahtuman peruutetaan. Pyynnön, jos kerääminen jono, käsitellään uudelleen. 
 
- **Tilattomien palvelu**. Jos palvelun jatkuu tietojen ulkoisen säilöön, kaikki toiminnot on oltava idempotent.


**Diagnostiikka**. Sovelluksen lokista

### <a name="service-fabric-node-is-shut-down"></a>Palvelun kangasta solmun suljetaan.

**Tunnistus**. Peruutus-tunnuksen välitetään palvelun `RunAsync` menetelmää. Palvelun kangasta peruuttaa tehtävän ennen solmu suljetaan.

**Palauttaminen**. Peruutus-tunnuksen avulla voit tunnistaa Sammuta. Kun palvelun kangasta pyytää peruutus, valmis työsi ja sulje `RunAsync` mahdollisimman pian.

**Diagnostiikka**. Sovelluksen lokit


## <a name="storage"></a>Tallennustilan

### <a name="writing-data-to-azure-storage-fails"></a>Azuren tallennustilaan epäonnistuu tietojen kirjoittaminen

**Tunnistus**. Asiakastietokone on vastaanottanut virheitä, kun kirjoitat.

**Palautus**

1. Yritä, Palauta lyhytkestoisia virheet. [Yritä käytännön] [ Storage.RetryPolicies] työasemaohjelmassa SDK käsittelee tämän automaattisesti.
2. Ota käyttöön katkaisin kuvion hankalaa tallennustilan välttämiseksi.
3. Jos N uudelleenyritykset epäonnistuu, suorita kaksivaiheista varmistusta. Esimerkki:

    - Tiedot tallennetaan paikallisen välimuistin ja välittää säilöön kirjoituksia myöhemmin, kun palvelu on käytettävissä.
    - Jos kirjoitus-toiminto on tapahtumien alueen, hyvittää tapahtuma.

**Diagnostiikka**. [Tallennustilan arvot]käyttää[storage-metrics].


### <a name="reading-data-from-azure-storage-fails"></a>Tietojen lukeminen Azuren tallennustilaan epäonnistuu.

**Tunnistus**. Asiakastietokone on vastaanottanut virheitä luettaessa.

**Palautus**

1. Yritä, Palauta lyhytkestoisia virheet. [Yritä käytännön] [ Storage.RetryPolicies] työasemaohjelmassa SDK käsittelee tämän automaattisesti.
2. RA GRS tallennustilaa, jos ensisijainen päätepisteen luettaessa ei onnistu, kokeile toissijainen päätepisteen luettaessa. Asiakkaan SDK voit käsitellä tämä automaattisesti. Katso [Azuren tallennustilaan replikoinnin][storage-replication].
3. Jos *N* uudelleenyritykset epäonnistuu, tutustu varakyselyjen toiminto, joka vähentää toimintoja sujuvasti sellaisia. Jos tuotteen kuvan ei voi hakea tallennustilaa, Näytä yleinen paikkamerkkikuvan.

**Diagnostiikka**. [Tallennustilan arvot]käyttää[storage-metrics].


## <a name="virtual-machine"></a>Virtuaalikoneen

### <a name="connection-to-a-backend-vm-fails"></a>Taustassa AM yhteys epäonnistuu.

**Tunnistus**. Verkon yhteyden virheitä.

**Palautus**

- Ota käyttöön vähintään kaksi Taustajärjestelmä VMs käytettävyys määrittäminen, kuormituksen tasauspalvelun takana.

- Jos yhteysvirhe on lyhytkestoisia, joskus TCP onnistuneesti yrittää viestin. 

- Ota käyttöön uudelleen käytännön-sovelluksessa. 

- Jatkuva tai muu kuin lyhytaikainen virheet Toteuta [katkaisin] [ circuit-breaker] kuvio.

- Jos puheluja AM ylittyy verkon juniin, lähtevien jonossa täyttää. Jos lähtevien jonossa on johdonmukaisesti täynnä, harkitse laajentaminen. 

**Diagnostiikka**. Kirjaudu sisään osoitteessa palvelun rajat tapahtumat.

### <a name="vm-instance-becomes-unavailable-or-unhealthy"></a>AM esiintymän tulee perusasemassa tai pois käytöstä.

**Tunnistus**. Määrittää kuormituksen [kunto näytteenottimen] [ lb-probe] , joka ilmoittaa, onko AM esiintymää on kunnossa. Näytteenottimen kannattaa tarkistaa, onko tärkeitä toimintoja vastaa oikein. 

**Palauttaminen**. Sovelluksen kunkin tason eri AM esiintymissä laittaa käytettävyys samat ja sijoita kuormituksen VMs eteen. Jos kunto näytteenottimen epäonnistuu, kuormituksen lopettaa uusia yhteyksiä lähettäminen perusasemassa esiintymä.

**Diagnostiikka**. – Käytä kuormituksen [log analytics][lb-monitor].
- Määritä valvonta järjestelmän valvoa kaikkia kunnon valvonta päätepisteet.


### <a name="operator-accidentally-shuts-down-a-vm"></a>Operaattori sulkee vahingossa AM.

**Tunnistus**. PUUTTUU

**Palauttaminen**. Määritä resurssin lukko kanssa `ReadOnly` taso. [Lukitse]resursseissa Azure resurssien hallinnan[rm-locks].

**Diagnostiikka**. Käytä [Azure tehtävän kirjautuu][azure-activity-logs].


## <a name="webjobs"></a>WebJobs

### <a name="continuous-job-stops-running-when-the-scm-host-is-idle"></a>Jatkuva työn suoritus keskeytyy, kun palvelujen ohjauksen hallinta isäntä ei käytetä.

**Tunnistus**. Välittää peruutus-tunnuksen WebJob-funktiolle. Lisätietoja on artikkelissa [kaksivaiheista Sammuta][web-jobs-shutdown]. 

**Palauttaminen**. Ota käyttöön `Always On` määrittäminen web App-sovelluksessa. Lisätietoja on artikkelissa [WebJobs tehtävien suorittaminen taustalla][web-jobs].


## <a name="application-design"></a>Sovelluksen suunnittelu

### <a name="application-cant-handle-a-spike-in-incoming-requests"></a>Sovellus ei Käsittele pyynnöt Piikkiin.

**Tunnistus**. Määräytyy sovelluksen. Tyypillinen ongelmista:

- Sivuston alkaa palauttaa HTTP 5xx virhekoodit.
- Riippuvaiset palvelut, kuten tietokannasta tai tallennusta, Käynnistä rajoita pyynnöt. Etsi HTTP-virheet, kuten 429 HTTP (liian monta pyyntöjen)-palvelun mukaan.
- HTTP-jono kasvaa.

**Palautus**

- Skaalaa ulos käsittelemisestä suojaavat aiempaa kuormituksen. 

- Ehkäistä virheet Vältä CSS häiritä koko sovelluksen virheet. Rajoituksen strategiat ovat seuraavat:

    - Toteuta [Rajoittimen kuvio] [ throttling-pattern] hankalaa Taustajärjestelmä järjestelmien välttämiseksi.
    - Käytä [jonon perustuva kuormituksen tasaamisen] [ queue-based-load-leveling] puskurin pyynnöt ja käsitellä niitä tarvittavat tahdissa.
    - Priorisoida tiettyjä asiakkaille. Esimerkiksi jos sovellus on maksuttomia ja maksullisia tasoa, rajoita asiakkaiden vapaa ylätasolla, mutta ei ole maksettu asiakkaille. Katso [prioriteetti jonon kuvion][priority-queue-pattern].

**Diagnostiikka**. Käytä [sovelluksen palvelun Diagnostiikan kirjaus][app-service-logging]. Palvelun, kuten [Azure Log Analytics][azure-log-analytics]- [Sovelluksen tiedot][app-insights], tai [Uusi Relic] [ new-relic] tietoja siitä, vianmäärityslokit.


### <a name="one-of-the-operations-in-a-workflow-or-distributed-transaction-fails"></a>Yksi työnkulun tai hajautettu tapahtuman epäonnistuu.

**Tunnistus**. Kun *N* uudelleen yritykset, epäonnistuu.

**Palautus**

- Kuin lieventämissuunnitelma, Toteuta [Ajoituksen agentti valvojan] [ scheduler-agent-supervisor] voit hallita koko työnkulun kuvio. 
- Älä yritä uudelleen aikakatkaisu. On pieni success korvaus tämän virheen. 
- Jonon toimivat, jotta voit yrittää myöhemmin uudelleen.

**Diagnostiikka**. Kirjaudu kaikki toiminnot (onnistuneen ja epäonnistui), mukaan lukien jalostettujen toiminnot. Käytä korrelaatio tunnukset, jotta voit seurata saman tapahtuman kaikki toiminnot.


### <a name="a-call-to-a-remote-service-fails"></a>Remote-palvelun kutsu epäonnistuu.

**Tunnistus**. HTTP-virhekoodin.

**Palautus**

1. Valitse lyhytkestoisia virheet uudelleen. 
2. Jos kutsu epäonnistuu jälkeen *N* yrittää, varakyselyjen toimia. (Sovellus tietyn.)
3. Toteuta [katkaisin kuvion] [ circuit-breaker] CSS virheiden välttämiseksi. 

**Diagnostiikka**. Kirjaudu kaikki remote kutsu epäonnistuu.


## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja FMA prosessin [toimintakykyyn pilvipalveluihin rakenteeltaan] [ resilience-by-design-pdf] (Lataa PDF).

<!-- links -->

[api-management]: https://azure.microsoft.com/documentation/services/api-management/
[api-management-throttling]: ../api-management/api-management-sample-flexible-throttling.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-web-apps]: ../application-insights/app-insights-azure-web-apps.md
[app-service-configure]: ../app-service-web/web-sites-configure.md
[app-service-logging]: ../app-service-web/web-sites-enable-diagnostic-log.md
[app-service-slots]: ../app-service-web/web-sites-staged-publishing.md
[auto-rest-client-retry]: https://github.com/Azure/autorest/blob/master/Documentation/clients-retry.md
[azure-activity-logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[azure-alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md
[azure-log-analytics]: ../log-analytics/log-analytics-overview.md
[BrokeredMessage.TimeToLive]: https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
[cassandra-error-handling]: http://www.datastax.com/dev/blog/cassandra-error-handling-done-right
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[docdb-multi-region]: ../documentdb/documentdb-developing-with-multiple-regions.md
[elasticsearch-azure]: guidance-elasticsearch-running-on-azure.md
[elasticsearch-client]: https://www.elastic.co/guide/en/elasticsearch/client/index.html
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[onstop-events]: https://azure.microsoft.com/blog/the-right-way-to-handle-azure-onstop-events/
[lb-monitor]: ../load-balancer/load-balancer-monitor-log.md
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md#learn-about-the-types-of-probes
[new-relic]: https://newrelic.com/
[priority-queue-pattern]: https://msdn.microsoft.com/library/dn589794.aspx
[queue-based-load-leveling]: https://msdn.microsoft.com/library/dn589783.aspx
[QuotaExceededException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx
[ra-web-apps-basic]: guidance-web-apps-basic.md
[redis-monitor]: ../redis-cache/cache-how-to-monitor.md
[redis-retry]: ../best-practices-retry-service-specific.md#cache-redis-retry-guidelines
[resilience-by-design-pdf]: http://download.microsoft.com/download/D/8/C/D8C599A4-4E8A-49BF-80EE-FE35F49B914D/Resilience_by_Design_for_Cloud_Services_White_Paper.pdf
[RoleEntryPoint.OnStop]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[RoleEnvironment.Stopping]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx
[rm-locks]: ../resource-group-lock-resources.md
[sb-dead-letter-queue]: ../service-bus-messaging/service-bus-dead-letter-queues.md
[sb-georeplication-sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/GeoReplication
[sb-messagingexception-class]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
[sb-messaging-exceptions]: ../service-bus-messaging/service-bus-messaging-exceptions.md
[sb-outages]: ../service-bus/service-bus-outages-disasters.md#protecting-queues-and-topics-against-datacenter-outages-or-disasters
[sb-partition]: ../service-bus-messaging/service-bus-partitioning.md
[sb-poison-message]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#poison
[sb-retry]: ../best-practices-retry-service-specific.md#service-bus-retry-guidelines
[search-sdk]: https://msdn.microsoft.com/library/dn951165.aspx
[scheduler-agent-supervisor]: https://msdn.microsoft.com/library/dn589780.aspx
[search-analytics]: ../search/search-traffic-analytics.md
[sql-db-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-db-errors]: ../sql-database/sql-database-develop-error-messages.md#resource-governanance-errors
[sql-db-failover]: ../sql-database/sql-database-geo-replication-failover-portal.md
[sql-db-limits]: ../sql-database/sql-database-resource-limits.md
[sql-db-replication]: ../sql-database/sql-database-geo-replication-overview.md
[storage-metrics]: https://msdn.microsoft.com/library/dn782843.aspx
[storage-replication]: ../storage/storage-redundancy.md
[Storage.RetryPolicies]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.aspx
[sys.event_log]: https://msdn.microsoft.com/library/dn270018.aspx
[throttling-pattern]: https://msdn.microsoft.com/library/dn589798.aspx
[web-jobs]: ../app-service-web/web-sites-create-web-jobs.md
[web-jobs-shutdown]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful

