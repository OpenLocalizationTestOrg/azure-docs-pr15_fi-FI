<properties
   pageTitle="Skaalattavissa verkkosovelluksen | Azure viittaus arkkitehtuuri | Microsoft Azure"
   description="Parantaminen skaalattavuus käynnissä Microsoft Azure-web-sovelluksessa."
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/12/2016"
   ms.author="mwasson"/>


# <a name="improving-scalability-in-a-web-application"></a>Web-sovelluksen skaalattavuus parantaminen 

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa kerrotaan suositellut arkkitehtuuri skaalattavuus ja käynnissä olevat Microsoft Azure verkkosovelluksen suorituskyvyn parantamiseen. Arkkitehtuuri perustuu [Azure viittaus arkkitehtuuri: Basic web-sovelluksen][basic-web-app]. Suosituksia ja huomioon otettavia seikkoja, artikkelista käyttää tätä arkkitehtuuri.

>[AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: Resurssienhallinta ja perinteinen. Tässä artikkelissa käytetään Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

![[0]][0]

Arkkitehtuuri on seuraavat osat:

- **Resurssiryhmä**. [Resurssiryhmä] [ resource-group] on looginen säilö Azure resurssit. 

- ** [Web Appin] [ app-service-web-app] ** ja ** [API sovelluksen][app-service-api-app]**. Tyypillinen Moderni sovelluksen voivat lisätä sivuston ja yhden tai useamman RESTful web API. Verkko-Ohjelmointirajapinnan ehkä kulutettu ‑ohjelmia AJAX kautta, alkuperäisen asiakassovelluksissa tai palvelinpuolen sovellukset. Huomioon otettavia seikkoja suunnittelemisesta verkossa API-kohdassa [API rakenne ohjeet][api-guidance].    

- **WebJob**. Käytä [Azure WebJobs] [ webjobs] pitkään suoritettavien tehtävien suorittaminen taustalla. WebJobs voit suorittaa aikataulussa, pitoisuuslukema, tai käynnistimen, kuten viestin sijoittaminen jonon saatuaan. WebJob suorittaa taustalla-prosessi App Service-sovelluksen yhteydessä. 

- **Jonossa**. Kuvassa arkkitehtuuri, valitse sovelluksen olevien taustatehtävät sijoittamalla viestin päälle [Azure jonon tallennustilan] [ queue-storage] jonossa. Viestin käynnistää WebJob-funktiota. 

    Vaihtoehtoisesti voit palvelun Bus olevien. Jos haluat vertailla artikkeleissa [Azure olevien ja palvelun Bus olevien - verrattuna ja asiaan][queues-compared].

- **Välimuistin**. Osittain staattinen tiedot tallennetaan [Azure Redis välimuistin][azure-redis].  

- **CDN**. Käytä [Azure sisällön toimituksen verkon] [ azure-cdn] (CDN) välimuistin yleisesti saatavilla sisältöön, pienet viive ja nopeuttaa toimitusta sisällön.

- **Tietojen tallentaminen.** Käytä [SQL Azure-tietokannan] [ sql-db] relaatiotietoja varten. Relaatio tietoa, harkitse NoSQL säilöön, kuten Azure-taulukkotallennus tai [DocumentDB][documentdb].

- **Azure Etsi**. [Azure] hakutoiminnolla[ azure-search] Lisää hakutoimintoa, esimerkiksi hakuehdotukset epäselvältä haun ja kielikohtaiset haku. Azure haun käytetään yleensä yhdessä tietosäilö, erityisesti, jos ensisijainen tietovaraston edellyttää tarkka yhdenmukaisuuden. Valitse tämä vaihtoehto kuin muut tietovaraston tärkeimpien tietojen ja hakuindeksin laittaa Azure haku. Azure haku voidaan myös yhdistää useita tietojen stores-yksittäisen hakuindeksin.  

- **Sähköpostin/SMS**. Jos haluat lähettää sähköpostin tai tekstiviestien sovelluksesi, käytä kolmannen osapuolen palvelun, kuten SendGrid tai Twilio sijaan toiminto muodostetaan suoraan sovellukseen.

## <a name="recommendations"></a>Suosituksia

### <a name="app-service-apps"></a>Sovelluksen Service-sovellukset 

On suositeltavaa luominen web-sovelluksen ja verkko-Ohjelmointirajapinnan erillisen palvelun sovelluksen sovellukset. Tämän rakenteen avulla voit näyttää niitä eri App palvelusopimusten vaihtoehdot, jolla puolestaan voi skaalata erikseen. Jos et tarvitse kyseisiä skaalattavuus ensimmäisen kerran, voit asentaa sovellukset vastaavan palvelupaketin, ja siirrä ne erillisessä suunnitelmien myöhemmin tarvittaessa. (Basic, Vakio ja Premium-palvelupaketti, sinun on laskuttaa AM esiintymien suunnitelmassa ei app kohden. Katso [sovelluksen palvelun hinnat][app-service-pricing])

Jos aiot käyttää *Helposti taulukoita* tai -palvelun mobiilisovellukset ominaisuudet *Helposti API* , erillisessä App palvelun sovelluksen luominen tähän tarkoitukseen.  Nämä ominaisuudet ovat riippuvaisia tietylle sovellukselle kehys, jotta ne.

### <a name="webjobs"></a>WebJobs

Jos WebJob on paljon resursseja, harkitse tyhjä App Service-sovelluksen sisällä eri App Service-palvelupaketti, säätää erillinen esiintymät WebJob voidaan ottaa käyttöön. Lisätietoja [taustan työt ohjeissa][webjobs-guidance].  

### <a name="cache"></a>Välimuistin

Voit parantaa suorituskyky ja skaalattavuus käyttämällä [Azure Redis välimuistin] [ azure-redis] välimuistiin tietoja. Harkitse Redis välimuistin:

- Osittain staattinen tapahtumatiedot.

- Istunnon tila.

- HTML-tiedot. Tämä voi olla hyötyä sovelluksissa, joista hahmonnetaan monimutkaisia HTML-tiedot. 

Tarkemmat ohjeet suunnittelemisesta välimuistiin strategia Katso [ohjeet välimuistiin][caching-guidance].

### <a name="cdn"></a>CDN 

Käytä [Azure CDN] [ azure-cdn] välimuistin staattiseksi sisällöksi. CDN tärkein etu on lyhentää viive käyttäjille, koska sisältö on tallennettu välimuistiin *edge Serverin* , joka on lähellä käyttäjän maantieteellisesti. CDN myös pienentää kuormitus sovellukseen, koska liikenne käsitellään ei sovellus.

- Jos sovelluksen koostuu lähinnä staattisia sivuja, kannattaa käyttää CDN välimuistiin koko sovellus. Tutustu [Azure CDN Azure App palvelussa][cdn-app-service].

- Muussa tapauksessa staattinen sisältöä, kuten kuvien, CSS- ja HTML tiedostoja, Azuren tallennustilaan ja käytä CDN välimuistiin kyseiset tiedostot. Katso [integroida tallennustilan-tilin kanssa CDN][cdn-storage-account].

> [AZURE.NOTE] Azure CDN ei voi käyttää sisältöä, joka edellyttää käyttöoikeuden tarkistusta.

Tarkemmat ohjeet lisätietoja [sisällön toimituksen (CDN) ohjeissa][cdn-guidance]. 

### <a name="storage"></a>Tallennustilan

Nykyaikainen sovellusten käsitellä usein suurista tietomääristä. Skaalata pilveen varten, on tärkeää valita oikean tallennustyyppi. Seuraavassa on joitakin perusaikataulun suosituksia.  Tarkemmat ohjeet, Lisätietoja on artikkelissa [Arvioidaan tietojen kaupan ominaisuudet Polyglot pysyvyys ratkaisuja][polyglot-storage].

Mitä haluat tallentaa | Esimerkki | Suositeltu tallennustila
--- | --- | ---
Tiedostot | Kuvat, asiakirjat, PDF-tiedostot | Azure-Blob-säiliö
Avaimen ja arvon tietoparin | Käyttäjän profiilitiedot etsitään käyttäjätunnus | Azure-taulukkotallennus
Lyhyitä viestejä, jotka on tarkoitettu lisäkäsittelyä käynnistäminen | Tilauksen pyynnöt | Azure jonon tallennustilan, palvelun Bus jonossa tai palvelun Bus aihe
Muut suhteelliset tiedot, joustava rakenne, vaativat basic kyselyt | Tuoteluettelon | Asiakirjan tietokantaan, kuten Azure DocumentDB, MongoDB tai Apache CouchDB
Relaatiotietoja edellyttävän tehokkaan kyselyn tuki, tarkka rakenne ja/tai vahva yhdenmukaisuuden | Tuotteen varaston | Azure SQL-tietokanta

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

### <a name="app-service-app"></a>Sovelluksen Service-sovellus

Jos ratkaisu on useita sovelluksen Service-sovellukset, harkitse käyttöönoton erottaa App palvelusopimusten vaihtoehdot. Tämän menetelmän avulla voit skaalata itsenäisesti, koska ne suoritetaan erillisessä esiintymät. Saat lisätietoja laajentaminen [skaalattavuus huomioon otettavia seikkoja] [ basic-web-app-scalability] [Basic web application arkkitehtuuri]-osassa[basic-web-app].

Harkitse WebJob liikkeelle omassa suunnitelman vastaavasti, niin, että taustaprosessit ei suorittaa saman esiintymät, jotka käsittelevät pyyntöjen.  

### <a name="sql-database"></a>SQL-tietokantaan

Suurentaa SQL-tietokantaan skaalattavuus *sharding* tietokannan &mdash; jakaminen tietokannan toisin sanoen vaakasuunnassa. Sharding avulla voit skaalata, tietokannan vaakasuunnassa, käyttämällä [joustavasti Tietokantatyökalut][sql-elastic]. Mahdollinen sharding etuja ovat:

- Parempi tapahtuman siirtonopeuden.

- Kyselyjen nopeuttaa tietojen alijoukkoa päälle. 

### <a name="azure-search"></a>Azure haku

Azure haun poistaa katseltavan suorittaa hakuja monimutkaisten tietojen ensisijaiseksi-kaupasta, ja se skaalata käsittelemään kuormituksen. Katso [asteikko resurssitasolla kysely ja Azure hakutoiminnossa indeksoinnin työmääriä][azure-search-scaling].

## <a name="security-considerations"></a>Suojausasiat

### <a name="cross-origin-resource-sharing-cors"></a>Usean Origin resurssien jakaminen (CORS)

Jos luot verkkosivusto- ja verkko-Ohjelmointirajapinnan kuin erillisessä sovellukset, sivuston voi tehdä Ohjelmointirajapinnan asiakaspuolen AJAX puhelut, ellet CORS Ota käyttöön. 

> [AZURE.NOTE] Selaimen suojaus estää verkkosivun tekemästä AJAX-pyyntöjen toisen toimialueen. Tämä rajoitus, jonka nimi on sama origin käytännön ja estää haittaohjelmien sivuston sentitive tietojen lukeminen toisesta sivustosta. CORS on W3C vakio, joka sallii Keskeytä saman origin käytännön ja sallia usean origin jotkin pyynnöt aikana hylkäämällä muiden palvelimeen.

Sovelluksen Services on tukee CORS, eikä hänen nimeään tarvitse sovelluksen koodia. Katso [Kulutettava sovelluksen Ohjelmointirajapinnan käyttäminen CORS JavaScript][cors]. Lisää sivuston Ohjelmointirajapinnan sallittujen alkuperistä luetteloon. 

### <a name="sql-database-encryption"></a>SQL-tietokannan salaus

Käytä [Läpinäkyvää salauksen] [ sql-encryption] Jos haluat salata tietokannan muut tiedot. Tämä ominaisuus suorittaa reaaliaikaisia salausta ja salauksen (mukaan lukien varmuuskopiot ja tapahtuman lokitiedostojen), koko tietokannan tarvitsematta sovelluksen muutokset. Salauksen Lisää joitakin viive, jotta se on hyvä erottaa tiedot, jotka on oltava suojatun omaan tietokantaan ja käyttöön vain tietokannan salauksen.  

## <a name="next-steps"></a>Seuraavat vaiheet

- Korkeampi käytettävyyden useamman kuin yhden alueen sovellus ja käytä [Azure liikenteen hallinta] [ tm] varten automaattisesti. Lisätietoja on artikkelissa [Azure viittaus arkkitehtuuri: WWW-sovellus ja suuri käytettävyys][web-app-multi-region].    

<!-- links -->

[api-guidance]: ../best-practices-api-design.md
[app-service-web-app]: ../app-service-web/app-service-web-overview.md
[app-service-api-app]: ../app-service-api/app-service-api-apps-why-best-platform.md
[app-service-pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[azure-cdn]: https://azure.microsoft.com/en-us/services/cdn/
[azure-redis]: https://azure.microsoft.com/en-us/services/cache/
[azure-search]: https://azure.microsoft.com/en-us/documentation/services/search/
[azure-search-scaling]: ../search/search-capacity-planning.md
[background-jobs]: ../best-practices-background-jobs.md
[basic-web-app]: guidance-web-apps-basic.md
[basic-web-app-scalability]: guidance-web-apps-basic.md#scalability-considerations 
[caching-guidance]: ../best-practices-caching.md
[cdn-app-service]: ../app-service-web/cdn-websites-with-cdn.md
[cdn-storage-account]: ../cdn/cdn-create-a-storage-account-with-cdn.md
[cdn-guidance]: ../best-practices-cdn.md
[cors]: ../app-service-api/app-service-api-cors-consume-javascript.md
[documentdb]: https://azure.microsoft.com/en-us/documentation/services/documentdb/
[polyglot-storage]: https://github.com/mspnp/azure-guidance/blob/master/Polyglot-Solutions.md
[queue-storage]: ../storage/storage-dotnet-how-to-use-queues.md
[queues-compared]: ../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md
[resource-group]: ../resource-group-overview.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-elastic]: ../sql-database/sql-database-elastic-scale-introduction.md
[sql-encryption]: https://msdn.microsoft.com/en-us/library/dn948096.aspx
[tm]: https://azure.microsoft.com/en-us/services/traffic-manager/
[web-app-multi-region]: ./guidance-web-apps-multi-region.md
[webjobs-guidance]: ../best-practices-background-jobs.md
[webjobs]: ../app-service/app-service-webjobs-readme.md
[0]: ./media/blueprints/paas-web-scalability.png "Web-sovelluksen Azure ja skaalattavuus"