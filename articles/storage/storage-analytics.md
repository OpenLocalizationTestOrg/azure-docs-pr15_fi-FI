<properties
    pageTitle="Tallennustilan-analyysin avulla voit kerätä lokit ja arvot | Microsoft Azure"
    description="Tallennustilan analyysin avulla voit seurata arvot tietoja kaikissa tallennustilan-palveluissa ja Blob, jonossa ja taulukon tallennustilan lokien kerääminen."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="storage-analytics"></a>Tallennustilan Analytics

## <a name="overview"></a>Yleiskatsaus

Azure tallennustilan Analytics tekee kirjaaminen ja mahdollistaa tietojen arvot tallennustilan tilin. Voit jäljittää pyynnöt ja analysoida käyttö trendejä ongelmien vianmääritys tallennustilan-tilisi tiedot.

Käyttämään tallennustilan Analytics, sinun on otettava se yksitellen jokaisen palvelun, joita haluat seurata. Voit ottaa sen [Azure-portaalissa](https://portal.azure.com). Lisätietoja on artikkelissa [näytön tallennustilan-tilin Azure-portaalissa](storage-monitor-storage-account.md). Voit myös ottaa tallennustilan Analytics ohjelmallisesti REST-Ohjelmointirajapinnalla tai asiakirjakirjaston kautta. Tallennustilan Analytics kumpaankin palveluun on oma [Hae Blob palveluominaisuudet](https://msdn.microsoft.com/library/hh452239.aspx), [Hae jonon palveluominaisuudet](https://msdn.microsoft.com/library/hh452243.aspx), [Hae taulukon palveluominaisuudet](https://msdn.microsoft.com/library/hh452238.aspx)ja [Hae palvelun ominaisuuksien](https://msdn.microsoft.com/library/mt427369.aspx) toimintojen avulla.

Kootut tiedot tallennetaan tunnetun blob (for kirjaaminen) ja tunnetun taulukoiden (arvot), jota voi käyttää Blob-palvelun ja taulukon service API.

Tallennustilan Analytics on enintään 20 Teratavua tallennettuja tietoja, joka on erillinen tilisi tallennustilan kokonaismäärä rajoitus määrän. Saat lisätietoja laskutus- ja tietojen säilytyskäytännöt [tallennustilan Analytics ja laskutukseen](https://msdn.microsoft.com/library/hh360997.aspx). Lisätietoja tallennustilan tilin rajoituksista on artikkelissa [Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet](storage-scalability-targets.md).

Perusteellisempaa oppaan avulla tallennustilan analyysin ja muita työkaluja tunnistaa, diagnosointi ja Azure-tallennustilan liittyvien ongelmien vianmääritys-artikkelissa [näytön vianmäärityksen, ja vianmääritys Microsoft Azuren tallennustilaan](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="about-storage-analytics-logging"></a>Lisätietoja tallennustilan Analytics kirjaaminen

Tallennustilan Analytics kirjaa onnistuneiden ja epäonnistuneiden pyyntöjen yksityiskohtaisia tietoja tallennustilan-palveluun. Nämä tiedot voidaan valvoa yksittäisiä pyynnöt ja tallennustilan-palvelussa ongelmien vianmääritys. Pyynnöt kirjautunut parhaiten työmäärään välein.

Lokimerkintöjä luodaan vain silloin, kun ei tallennustilan aktiviteetin. Esimerkiksi jos tallennustilan tilin on tehtävä Blob palvelussa, mutta ei sen taulukon tai jonon palveluita, vain Blob-palveluun liittyvät lokit luodaan.

Tallennustilan Analytics kirjaaminen ei ole käytettävissä Azure tiedosto-palveluun.

### <a name="logging-authenticated-requests"></a>Todennettu kirjaaminen pyynnöt

Todennettu pyynnöt seuraavanlaisia kirjautunut:

- Onnistuneiden pyyntöjen.

- Epäonnistui pyynnöt, mukaan lukien aikakatkaisu, rajoitusta, verkon, todennus ja muut virheet.

- Pyyntöjen jaettu Access allekirjoitus (SAS), mukaan lukien epäonnistui ja onnistuneen pyynnöt.

- Pyynnöt analytics-tiedot.

Tallennustilan-Analytics itse, kuten lokitiedostojen luominen tai poistaminen, tekemät pyynnöt on oltava kirjautuneena. Täydellinen luettelo kirjaustiedot on kuvattu [tallennustilan Analytics kirjautunut toiminnot ja tilasanomat](https://msdn.microsoft.com/library/hh343260.aspx) ja [Analytics Log Tallennusmuoto](https://msdn.microsoft.com/library/hh343259.aspx) -aiheet.

### <a name="logging-anonymous-requests"></a>Lokiin kirjaaminen anonyymit pyynnöt
Anonyymit pyynnöt seuraavanlaisia kirjautunut:

- Onnistuneiden pyyntöjen.

- Palvelimen virheet.

- Asiakkaan ja palvelimen aikakatkaisu virheet.

- Epäonnistuneiden GET-pyyntöjen virhekoodi 304 (ei muokattu).

Kaikki muut epäonnistui anonyymit pyynnöt on oltava kirjautuneena. Täydellinen luettelo kirjaustiedot on kuvattu [tallennustilan Analytics kirjautunut toiminnot ja tilasanomat](https://msdn.microsoft.com/library/hh343260.aspx) ja [Analytics Log Tallennusmuoto](https://msdn.microsoft.com/library/hh343259.aspx) -aiheet.

### <a name="how-logs-are-stored"></a>Lokien tallentaminen
Kaikki lokit tallennetaan estä BLOB-säilö $logs, jotka luodaan automaattisesti, kun tallennustilan Analytics on otettu käyttöön tallennustilan tilin. $Logs säilö sijaitsee blob-nimitilaa-tallennustilan tilin, esimerkiksi: `http://<accountname>.blob.core.windows.net/$logs`. Tämä säilö ei voi poistaa, kun tallennustilan Analytics on otettu käyttöön, mutta sen sisältöä voi poistaa.

>[Azure.NOTE] $Logs säilö ei ole näkyvissä, kun säilön päällä, toiminto suoritetaan, kuten [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) -menetelmää. Se on voidaan käyttää suoraan. Esimerkiksi [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) -menetelmän avulla voi käyttää-BLOB `$logs` säilö.
Kun pyynnöt kirjautunut, tallennustilan Analytics Lataa keskitason tulokset lohkot. Tallennustilan Analytics määräajoin, Vahvista ruutujen ja tarkastella niitä offline-blob-muodossa.

Tietueiden kaksoiskappaleita saattaa olla saman tunnin luotu lokitiedot. Voit määrittää, onko tietueen kaksoiskappale valitsemalla **Pyyntötunnus** ja **toiminnon** numero.

### <a name="log-naming-conventions"></a>Kirjaudu nimeämiskäytännöt
Kunkin lokin kirjoitetaan seuraavassa muodossa.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

Seuraavassa taulukossa on kuvattu kunkin määrite Kirjaa nimen.

| Määrite         | Kuvaus                                                                                                                                                                                   |
|----------------   |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   |
| < palvelun nimi >    | Tallennustilan-palvelun nimi. Esimerkki: blob, taulukko tai jonossa.                                                                                                                          |
| VVVV              | Vuosilukujen nelinumeroista lokin. Esimerkki: 2011.                                                                                                                                           |
| MM                | Kirjaudu kaksinumeroinen kuukausi. Esimerkki: 07.                                                                                                                                             |
| PP                | Kirjaudu kaksinumeroinen kuukausi. Esimerkki: 07.                                                                                                                                             |
| hh                | Kaksinumeroinen tunti, joka ilmaisee aloitus tunnin lokit 24 tunnin UTC-muodossa. Esimerkki: 18.                                                                                     |
| mm                | Kaksinumeroinen luku, joka ilmaisee aloitus minuutit lokit. Tämä arvo on ei tue tallennustilan Analytics nykyinen versio ja sen arvo on aina 00.     |
| <counter>         | Nollasta laskuri kuusi numerot, joka ilmaisee numeron log BLOB-objektit luodaan tallennustilan palvelun tunnin ajanjakso. Laskuri alkaa 000000. Esimerkki: 000001.     |

Seuraavassa on valmis malli lokitiedoston nimi, joka yhdistää edellisessä kohdassa käytettyä.

    blob/2011/07/31/1800/000001.log

Seuraavassa on esimerkki URI, jonka avulla voidaan käyttää edelliseen lokitiedoston.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Tallennustilan pyyntö kirjautuessa tuloksena Kirjaa nimen numeroidulla tunti, kun pyydetty toiminto on valmis. Jos GetBlob pyyntö suoritettiin kello 6:30 esimerkiksi Valitse 7/31/2011 lokin kirjoitettavat seuraavat etuliite:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Kirjaudu metatiedot
Kaikki lokin Blob-objektien tallennetaan metatiedot, joilla voidaan tunnistaa blob sisältää kirjaaminen tietoja. Seuraavassa taulukossa on kuvattu kunkin metatiedot-määrite.

| Määrite     | Kuvaus                                                                                                                                                                                                                                                   |
|------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| LogType       | Tässä artikkelissa kuvataan, sisältääkö lokin lukeminen ja kirjoittaminen poistaminen toiminnot liittyvät tiedot. Tämä arvo voi olla yksi tai kolmen, erotettuina yhdistelmä. Esimerkki 1: Kirjoita; Esimerkki 2: Lue, kirjoita; Esimerkki 3: luku, kirjoitus, poista.    |
| Aloitusajan     | Merkinnän lokiin YYYY muodossa aikaisin aika-MM-DDThh:mm:ssZ. Esimerkki: 2011-07-31T18:21:46Z.                                                                                                                                             |
| Lopetusaika       | Merkinnän, kirjaudu sisään-YYYY lomakkeen uusimman aika-MM-DDThh:mm:ssZ. Esimerkki: 2011-07-31T18:22:09Z.                                                                                                                                               |
| LogVersion    | Lokitiedostojen muotoilu versio. Tällä hetkellä ainoa tuettu arvo on 1.0.                                                                                                                                                                                     |

Seuraavassa on luettelo näyttää käyttämällä edellisessä kohdassa käytettyä valmis malli-metatiedot.

- LogType = kirjoittaminen

- Aloitusajan = 2011-07-31T18:21:46Z

- Lopetusaika = 2011-07-31T18:22:09Z

- LogVersion = 1.0

### <a name="accessing-logging-data"></a>Kirjaaminen tietojen käyttäminen

Kaikki tiedot `$logs` säilö niitä voi käyttää Blob service API käyttämällä, mukaan lukien Azure myöntämä .NET-ohjelmointirajapinnan hallitun kirjastoon. Tallennustilan tilin järjestelmänvalvoja voi lukea ja Poista lokit, mutta ei voi luoda tai päivittää ne. Lokin metatietojen ja kirjaa nimen voidaan, kun kysely suoritetaan lokin. Tietyn tunti lokit näkyvän epäjärjestyksessä mahdollista, mutta metatiedot määrittää lokimerkintöjä aikajakson aina lokiin. Voit käyttää yhdistelmän log nimet ja metatietojen vuoksi, kun etsit tietty loki.

## <a name="about-storage-analytics-metrics"></a>Lisätietoja tallennustilan Analytics-arvot

Tallennustilan Analytics voi tallentaa arvot, jotka sisältävät pyynnöt tallennustilan palveluun koostetun tapahtuman Tilasto- ja kapasiteetin tietoja. Tapahtumien raportoidaan tasolla sekä API toiminto ja tallennustilaa palvelun tasolla ja kapasiteetin ilmoitetaan tallennustilan palvelun tasolla. Arvot tietoja voidaan käyttää avulla voit analysoida tallennustilan palvelujen käyttöä, ongelmien vianmääritys ja tallennustilaa vastaan pyyntöihin ja sovelluksia, jotka käyttävät palvelun suorituskyvyn parantamiseksi.

Käyttämään tallennustilan Analytics, sinun on otettava se yksitellen jokaisen palvelun, joita haluat seurata. Voit ottaa sen [Azure-portaalissa](https://portal.azure.com). Lisätietoja on artikkelissa [näytön tallennustilan-tilin Azure-portaalissa](storage-monitor-storage-account.md). Voit myös ottaa tallennustilan Analytics ohjelmallisesti REST-Ohjelmointirajapinnalla tai asiakirjakirjaston kautta. Tallennustilan Analytics kumpaankin palveluun on oma **Hae palveluominaisuudet** -toimintojen avulla.

### <a name="transaction-metrics"></a>Tapahtuman arvot

Lukuisia tiedot on tallennettu tunnin välein tai minuutin välein tallennustilan kunkin palvelun ja pyydetty API-toiminnon, mukaan lukien tunkeutumisen/juniin, käytettävyyttä, virheitä ja luokiteltu pyynnön prosenttiosuudet. Näet [Tallennustilan Analytics arvot Taulukkorakenteen](https://msdn.microsoft.com/library/hh343264.aspx) artikkelissa tapahtumatiedot kattavaan luetteloon.

Tapahtumatiedot on tallennettu kahdella tasolla – service tason ja API-toiminnon taso. Palvelun tasolla yhteenvetojen kaikki tilastotiedot pyytänyt API toimintojen kirjoitetaan taulukon kohteen tunnissa vaikka palvelu ei ole pyynnöt on tehty. Tasolla API toiminnon tilastotiedot vain kirjoitetaan kohteen toiminto on pyydettäessä, tunnin kuluessa.

Esimerkiksi jos teet **GetBlob** toimen Blob-palveluun, tallennustilan Analytics-arvot Kirjaudu pyynnön ja Sisällytä se koostetut tiedot sekä Blob-palvelun sekä **GetBlob** -toiminto. Kuitenkin ei ole **GetBlob** -toiminto on pyydetty tunnin aikana, jos kohde ei kirjoitetaan `$MetricsTransactionsBlob` kyseisen toiminnon.

Tapahtuman arvot tallennetaan pyynnöt ja tallennustilaa Analytics itse tekemät pyynnöt. Esimerkiksi tallennustilan Analytics pyynnöt kirjoittaa lokit ja taulukon kohteet tallennetaan. Saat lisätietoja siitä, miten nämä pyynnöt ovat laskuttaa [tallennustilan Analytics ja laskutukseen](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Kapasiteetin arvot

>[AZURE.NOTE] Tällä hetkellä kapasiteetin arvot ovat käytettävissä vain Blob-palveluun. Kapasiteetin mittaukset taulukkoa ja jono-palvelu on saatavana tallennustilan Analytics tulevissa versioissa.

Kapasiteetin tiedot on tallennettu päivittäin tallennustilan tilin Blob-palvelun ja kahden taulukon kohteiden kirjoitetaan. Yhden kohteen tarjoaa tilastotiedot käyttäjätiedot ja toinen on tietoja `$logs` käyttämä tallennustila Analytics blob-säilö. `$MetricsCapacityBlob` Taulukko sisältää seuraavat tilastotiedot:

- **Kapasiteetin**: tallennustilan tilin-Blob-objektien tavuina palvelun käyttämä tallennustilan määrä.

- **ContainerCount**: blob säilöt-tallennustilan tilin Blob-palvelun määrä.

- **ObjectCount**: vähimmäis- ja ei ole Estä tai sivun BLOB storage asiakkaan Blob-palvelun määrä.

Saat lisätietoja kapasiteetin arvot [Tallennustilan Analytics arvot Taulukkorakenteen](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Kuinka arvot tallennetaan

Kaikki arvot tiedot tallennustilan-palvelut on tallennettu varattu palvelun on kolme taulukkoa: tapahtuman tietoja yhdestä taulukosta, minuutit tapahtuman tietoja yhdestä taulukosta ja kapasiteetin tietoja toisesta taulukosta. Tapahtuman ja minuutti tapahtumatietoja koostuu pyynnön ja vastauksen tiedot ja kapasiteetin tiedot sisältävät tallennustilan käyttötiedot. Tunti mittarit, minuutit arvot ja kapasiteetti tallennustilan tilin Blob-palvelun niitä voi käyttää taulukoita ja jotka ovat nimeltään seuraavassa taulukossa kuvatulla tavalla.

| Arvot taso                         | Taulukoiden nimet                                                                                                                   | Tuetut versiot                                                                                                                        |
|------------------------------------   |-----------------------------------------------------------------------------------------------------------------------------  |---------------------------------------------------------------------------------------------------------------------------------------------- |
| Tunnin välein mittarit, ensisijainen sijainti      |  $MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue                                                  | Ennen 2013-08-15 vain versiot. Kun nämä nimet ovat tuetaan, on suositeltavaa, että voit siirtyä taulukoiden alla.  |
| Tunnin välein mittarit, ensisijainen sijainti      | $MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue               | Kaikki versiot, kuten 2013-08-15.                                                                                                               |
| Minuutit mittarit, ensisijainen sijainti      | $MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue         | Kaikki versiot, kuten 2013-08-15.                                                                                                               |
| Tunnin välein mittarit, toissijainen sijainti    | $MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue         | Kaikki versiot, kuten 2013-08-15. Lukuoikeudet geo tarpeettomat replikointi on otettava käyttöön.                                                    |
| Minuutit mittarit, toissijainen sijainti    | $MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue   | Kaikki versiot, kuten 2013-08-15. Lukuoikeudet geo tarpeettomat replikointi on otettava käyttöön.                                                    |
| Kapasiteetti (vain Blob-palvelu)          | $MetricsCapacityBlob                                                                                                          | Kaikki versiot, kuten 2013-08-15.                                                                                                               |


Nämä taulukot luodaan automaattisesti, kun tallennustilan Analytics on otettu käyttöön tallennustilan tilin. Ne ovat käyttää kautta nimitila-tallennustilan tilin, esimerkiksi:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Arvot tietojen käyttäminen

Kaikki arvot-taulukoiden tietojen niitä voi käyttää taulukon service API käyttämällä, mukaan lukien Azure myöntämä .NET-ohjelmointirajapinnan hallitun kirjastoon. Tallennustilan tilin järjestelmänvalvojaan lukea ja poistaa taulukon kohteita, mutta ei voi luoda tai päivittää ne.

## <a name="billing-for-storage-analytics"></a>Tallennustilan Analytics Laskutus

Tallennustilan Analytics käyttöön tallennustilan tilin omistajan; se ei ole käytössä oletusarvoisesti. Kaikki arvot tiedot kirjoitetaan tallennustilan tilin-palveluissa. Tällöin kunkin maksettavan korvauksen tallennustilan Analytics kirjoitus on laskutettava. Tallennustilan arvot tietojen käyttämä on lisäksi myös laskutettavan.

Laskutettavan ovat tallennustilan Analytics seuraavia toimintoja:

- Voit luoda kirjaaminen BLOB pyynnöt. 

- Voit luoda taulukon kohteet arvot pyynnöt.

Jos olet määrittänyt tietojen säilytyskäytännön, ei perittävän Poista tapahtumat kun tallennustilan Analytics poistaa vanhoja kirjaaminen ja arvot tietoja. Poista asiakkaan tapahtumat ovat kuitenkin laskutettavan. Saat lisätietoja säilytyskäytännöt [tallennustilan Analytics-tietojen Arkistointikäytäntöjen määrittäminen](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Tietoja laskutettavan pyynnöt

Jokaisen tilin tallennuspalvelu tehdyn pyynnön on laskutettavan tai ei-laskutettavan. Tallennustilan Analytics kirjaa kunkin yksittäisen pyynnön palveluun, kuten tila-sanoma, joka ilmaisee pyynnön käsittelyn. Vastaavasti tallennustilan Analytics tallentaa mittaukset palvelun ja palvelun, kuten prosenttien ja tiettyjen tila-viestien määrä API-toimintoja. Yhdessä nämä ominaisuudet auttavat analysoida laskutettavan pyynnöt ja parantamiseksi-sovelluksen vianmääritys pyynnöt palvelujen kanssa. Lisätietoja Laskutus-kohdassa [Tietoja Azure tallennustilan Laskutus - kaistanleveyden, tapahtumia, ja kapasiteetti](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Kun katsoo tallennustilan Analytics-tiedot, voit käyttää taulukoita [tallennustilan Analytics kirjautunut toiminnot ja tilasanomat](https://msdn.microsoft.com/library/azure/hh343260.aspx) artikkelissa ja selvittää, mitä pyynnöt laskutettavan. Sitten voit verrata lokit ja arvot tilasanomat nähdäksesi, jos olet veloitettujen erityisesti pyytää tietoja. Voit käyttää myös taulukot edellisessä aiheessa tutkia tallennuspalvelu tai yksittäisen API-toiminnon käytettävyys.

## <a name="next-steps"></a>Seuraavat vaiheet

### <a name="setting-up-storage-analytics"></a>Tallennustilan Analytics määrittäminen
- [Näytön tallennustilan-tilin Azure-portaalissa](storage-monitor-storage-account.md)
- [Ottaminen käyttöön ja tallennustilaa Analytics määrittäminen](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Tallennustilan Analytics kirjaaminen  
- [Lisätietoja tallennustilan Analytics kirjaaminen](https://msdn.microsoft.com/library/hh343262.aspx)
- [Tallennustilan Analytics lokin muoto](https://msdn.microsoft.com/library/hh343259.aspx)
- [Tallennustilan Analytics kirjautunut toimintojen ja tilasanomat](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Tallennustilan Analytics arvot
- [Lisätietoja tallennustilan Analytics arvot](https://msdn.microsoft.com/library/hh343258.aspx)
- [Tallennustilan Analytics arvot taulukko rakenne](https://msdn.microsoft.com/library/hh343264.aspx)
- [Tallennustilan Analytics kirjautunut toimintojen ja tilasanomat](https://msdn.microsoft.com/library/hh343260.aspx)  
