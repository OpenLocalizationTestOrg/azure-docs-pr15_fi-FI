<properties
 pageTitle="Sovelluskehittäjän opas - messaging | Microsoft Azure"
 description="Azure IoT keskittimeen developer guide - laitteen cloud ja cloud laitteen messaging"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="send-and-receive-messages-with-iot-hub"></a>Lähetä ja vastaanota viestit IoT keskittimeen kanssa

## <a name="overview"></a>Yleiskatsaus

IoT-toiminnossa on seuraavat tekstiviesti perusalkioiden viestintä-laitteen kanssa:

- [Laitteen cloud] [ lnk-d2c] laitteesta sovellukseen takaisin end-näppäinyhdistelmää.
- [Cloud laitteen] [ lnk-c2d] sovelluksesta takaisin loppuun (*palvelu* ja *pilvi*).

IoT keskittimeen viestinnän toimintoja Core ominaisuudet eivät luotettavasti ja kestävyys viestejä. Näiden ominaisuuksien käyttöön toimintakykyyn varattuina laitteen reunassa ja lataa piikkarit tapahtuman käsittelyn cloud-puolella. IoT keskittimeen toteuttaa *vähintään kerran* toimituksen oikeudet sekä laitteen cloud cloud laitteen viestiliikenteen.

IoT toiminto tukee useita [laitteen osoittava protokollat] [ lnk-protocols] (kuten MQTT, AMQP ja HTTP). Saumaton yhteensopivuus-protokollia yli tukemiseksi IoT keskittimeen määrittää [yhteisen viestimuodon] [ lnk-message-format] kaikki laitteen osoittava protokollat tukevat.

IoT keskittimeen paljastaa [tapahtumaa-toiminnossa yhteensopivan päätepisteen] [ lnk-compatible-endpoint] taustatietokantaan sovellukset vastaanottanut valitsemalla laitteen cloud viestien lukeminen ottaa käyttöön.

### <a name="when-to-use"></a>Käyttäminen

Viestintä on core ominaisuus, IoT toiminnossa. Käyttää aina, kun tarvitset lähettämiseen laitteestasi takaisin loppuun ja viestien lähettäminen laitteeseen takaisin loppu.

Katso IoT keskittimeen ja tapahtuman keskittimet palvelut vertailu, [vertailu IoT-toiminnosta ja tapahtuman keskittimet][lnk-compare].

## <a name="device-to-cloud-messages"></a>Laitteen cloud viestit

Voit lähettää laitteen cloud viestejä laitteen osoittava päätepisteen (**/devices/ {deviceId} / viestien/tapahtumien**) kautta. Taustatietokannan-palvelun vastaanottaa laitteen pilven kautta palvelun osoittava päätepiste (**/messages/events**), joka on yhteensopiva [Tapahtuman keskittimet][lnk-event-hubs]. Sen vuoksi, voit käyttää vakio- [tapahtuman keskittimet integrointi ja SDK: T] [ lnk-compatible-endpoint] laitteen cloud viestien.

IoT keskittimeen toteuttaa laitteen cloud messaging niin, että eroaa [Tapahtuman]porttiin[lnk-event-hubs]. IoT keskittimeen laitteen cloud viestit ovat tapahtuman keskittimet *tapahtumien* muistuttamaan enemmän kuin [Palvelun Bus] [ lnk-servicebus] *viestejä*.

Tämä toteutus on seuraavat vaikutukset:

* Lisää vastaavasti tapahtuman keskittimet tapahtumien laitteen cloud viestit ovat kestävät ja säilytetä IoT-toiminnossa enintään seitsemän päivän ajan (katso [laitteen pilvipalveluun määritysten asetukset][lnk-d2c-configuration]).
* Laitteen cloud viestit on osioitu koko kiinteä joukko osioita, joka on määritetty luomisen (katso [laitteen pilvipalveluun määritysten asetukset][lnk-d2c-configuration]).
* Analogously tapahtuman porttiin laitteen cloud viestien lukeminen asiakkaiden on käsiteltävä osiot ja checkpointing. Katso [Tapahtuman keskittimet - muissa tapahtumien][lnk-event-hubs-consuming-events].
* Tapahtuman keskittimet tapahtumia, kuten laitteen cloud viestejä voi olla enintään 256 kt ja voidaan ryhmitellä optimoi lähettää erissä. Erissä voi olla enintään 256 kt ja enintään 500 viestejä.

On-kuitenkin joitakin tärkeitä erotettu toisistaan IoT keskittimeen laitteen cloud viestintä- ja tapahtuman keskittimet:

* Kuvatulla tavalla [IoT pääsivuston käyttöoikeuksien hallinta] [ lnk-devguide-security] -osassa IoT keskittimeen sallii laite todennus ja käyttöoikeuksien hallinta.
* IoT keskittimeen sallii miljoonia samanaikaisesti yhdistettyjen laitteiden (katso [kiintiöiden ja rajoitusten][lnk-quotas]), kun taas tapahtuman keskittimet on rajoitettu 5000 kohti nimitilan AMQP yhteydet.
* IoT keskittimeen ei salli haluamaansa jakaminen käyttämällä **PartitionKey**. Laitteen cloud viestit ovat osioitu niiden alkuperäiseen **deviceId**perusteella.
* Skaalaus IoT keskittimeen on hieman erilainen kuin skaalaus tapahtuman keskittimet. Lisätietoja on artikkelissa [Skaalaus IoT keskittimeen][lnk-guidance-scale].

> [AZURE.NOTE] Tapahtuman keskittimet IoT-sivusto ei voi korvata kaikissa tilanteissa. Esimerkiksi-tapahtuman käsittely joitakin funktiolauseita voi olla tarpeen uudelleenosioimisesta tapahtumia, jotka koskevat eri ominaisuus tai kenttä ennen analysoidaan tietoja virtaa. Tässä skenaariossa voit käyttää tapahtumaa-toiminnossa irrottamisen käsittelyn myyntijakso stream kaksi osiin. Lisätietoja on artikkelissa [Azure tapahtuman keskittimet yleiskatsaus] *osioiden* [lnk-eventhub-partitions].

Lisätietoja käyttämisestä laitteen cloud viestintä-artikkelissa [IoT keskittimeen ohjelmointirajapinnan ja SDK: T][lnk-sdks].

> [AZURE.NOTE] Kun http:n laitteen cloud viestien lähettämiseen, ominaisuuden nimet ja arvot voivat sisältää aakkosnumeerisia merkkejä ASCII- plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="non-telemetry-traffic"></a>Muut telemetriatietojen liikenne

Usein lisäksi telemetriatietojen arvopisteiden laitteiden myös lähetettävien viestien ja pyynnöt, jotka edellyttävät suorittamisen ja käsittely-logiikkaa sovelluskerroksen. Esimerkiksi kriittinen ilmoituksia, jotka on käynnistäminen tietty toiminto uudelleen tai laitteen vastaukset komentoja, jotka on lähetetty uudelleen.

Saat lisätietoja paras tapa käsitellä tällaista viestiä [Opetusohjelma: IoT keskittimeen laitteen cloud viestien käsitteleminen] [ lnk-d2c-tutorial] opetusohjelma.

### <a name="device-to-cloud-configuration-options"></a>Laitteen cloud kokoonpanoasetusten määrittäminen

IoT-toiminnossa paljastaa seuraavia ominaisuuksia, jotta voit hallita laitteen cloud messaging.

* **Osion määrä**. Määrittää tämän ominaisuuden luominen määrittämään laitteen cloud tapahtuman erilaisille osioiden määrää.
* **Säilytys-aika**. Tämä ominaisuus määrittää laitteen cloud viestien säilytys-aika. Oletusarvo on yksi päivä, mutta se on lisättävä seitsemän päivän aikana.

Lisäksi analogously tapahtuman porttiin IoT toiminnossa voit hallita kuluttaja-laitteen pilveen ryhmille päätepiste.

Voit muokata kaikkia näitä ominaisuuksia ohjelmallisesti [IoT keskittimeen resurssin tarjoajaan REST API]kautta[lnk-resource-provider-apis], tai [Azure portal][lnk-management-portal].

### <a name="anti-spoofing-properties"></a>Anti väärentää ominaisuudet

Voit välttää väärentää laitteen cloud viesteihin IoT keskittimeen laitteen kaikki niihin viestit seuraavat ominaisuudet:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Kaksi ensimmäistä sisältävät **deviceId** ja **generationId** alkuperäiseen tai laitteen mukaan [laitteen käyttäjätietojen ominaisuuksien][lnk-device-properties].

**ConnectionAuthMethod** -ominaisuus sisältää muuntaa sarjaksi JSON-objekti, seuraavat ominaisuudet:

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="cloud-to-device-messages"></a>Cloud laitteen viestit

Voit lähettää cloud laitteen viestejä palvelun osoittava päätepisteen (**/messages/devicebound**) kautta. Laite vastaanottaa ne laitekohtaiset päätepisteen (**/devices/ {deviceId} / viestien/devicebound**) kautta.

Yksittäinen laite suunnattu cloud laitteen kustakin viestistä, määrittämällä **,** -ominaisuuden arvoksi **/devices/ {deviceId} / viestien/devicebound**.

>[AZURE.IMPORTANT] Kunkin laitteen jono sisältää enintään 50 cloud laitteen viestejä. Yritetään lähettää enemmän viestejä saman laitteen aiheuttaa virheen.

> [AZURE.NOTE] Kun lähetät cloud laitteen viestejä, ominaisuuden nimet ja arvot voivat sisältää aakkosnumeerisia merkkejä ASCII- plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="message-lifecycle"></a>Viestin elinkaari

Taata vähintään kerran viestin lähettämisen, IoT keskittimeen jatkuu cloud laitteen viestien laite kohden. Laitteiden erikseen hyväksyy sen IoT keskittimeen poistaminen jonossa *valmistumista* . Tämä takaa vikasietoisuudelle vastaan yhteys- ja laitteen virheet.

Seuraavassa kaaviossa on esitetty cloud laitteen viestin elinkaari tila-kaavio.

![Cloud laitteen viestin elinkaari][img-lifecycle]

Kun palvelun lähettää viestin, pidetään *Enqueued*. *Kun laite haluaa viestin IoT keskittimeen *lukitukset* (asettaa **näkymätön**) viestin* salliminen muiden viestiketjuissa siirtyminen samassa laitteessa muut viestit toimittamista varten. Laitteen viestiketjun on valmis viestin käsittelyn, se ilmoittaa IoT keskittimeen *Viimeistellään* viestin.

Laitteen Katso myös:

- *Hylkää* sanoman, joka aiheuttaa IoT toiminnossa voit määrittää **Deadlettered** -tilaan. Huomautus: laitteiden yhdistämällä MQTT et voi hylkää C2D viestejä.
- *Hylkää* **Enqueued**arvoksi sanoman, joka aiheuttaa IoT toiminnossa Vie viestin takaisin jonossa, ja siinä.

Viestiketjun saattaa epäonnistua käyttänyt viestin ilmoittamatta IoT toiminnossa. Tässä tapauksessa viestejä automaattisesti siirtymä **näkymätön** tilasta takaisin **Enqueued** tilaan *näkyvyys (tai Lukitse) aikakatkaisun*jälkeen. Aikakatkaisu oletusarvo on yksi minuutti.

Viestin voi siirtyä **Enqueued** ja **näkymättömät** hyötyä enintään, kuinka monta kertaa IoT keskittimeen **max toimituksen Laske** -ominaisuudessa määritetyn varten. Jälkeen siirtymät määrä IoT keskittimeen asettaa viestin tilaksi **Deadlettered**. Vastaavasti IoT keskittimeen asettaa viestin **Deadlettered** vanheneminen sen ajan kuluttua (katso [oletusaika][lnk-ttl]).

Opetusohjelma cloud laitteen viesteille, saat [Opetusohjelma: IoT keskittimeen cloud laitteen viestien lähettäminen][lnk-c2d-tutorial]. Aiheistaoffice.com siitä, miten eri ohjelmointirajapinnan ja SDK: T näyttää cloud laitteen toiminnot-kohdassa [IoT keskittimeen ohjelmointirajapinnan ja SDK: T][lnk-sdks].

> [AZURE.NOTE] Yleensä cloud laitteen viestien suorittaminen aina, kun viestin menetyksiä vaikuta sovelluksen logiikkaa. Esimerkiksi viestin sisältöä on on samanlainen onnistuneesti paikallinen tallennus- tai toiminto on suoritettu onnistuneesti. Viesti voi myös suorittamisesta lyhytkestoisia tiedot, joiden tappio ei vaikuttaa sovelluksen toimintoja. Joskus pitkään suoritettavien tehtävien voivat tehdä cloud laitteen viestin jälkeen toteaa paikallinen tallennus tehtävien kuvauksen. Sitten voit ilmoittaa sovelluksen takaisin loppuun laitteen cloud-viesteillä eri vaiheissa tehtävän edistymisestä.

### <a name="message-expiration-time-to-live"></a>Viestin erääntyminen (Live aika)

Cloud laitteen kaikkiin lähetettäviin on vanheneminen-aika. Tällä hetkellä on määritetty joko ( **ExpiryTimeUtc** -ominaisuudessa)-palvelun tai IoT keskittimeen oletusarvon *oletusaika* määritettyä IoT keskittimeen-ominaisuuden avulla. Katso [Cloud laitteen asetukset][lnk-c2d-configuration].

> [AZURE.NOTE] Yhteisen voi hyödyntää viestin erääntyminen ja estää viestien lähettäminen yhteys katkaistu laitteet on lyhyt aika Live arvojen määrittäminen. Tämän menetelmän kertyy saman tuloksen kuin ylläpito laite-yhteystilaa aikana johtaa siihen tehokkaampi. Kun pyydät viestin kuittaussanomien, IoT toiminto ilmoittaa laitteet, jotka voivat vastaanottaa viestejä ja laitteet, jotka eivät ole online tai on epäonnistunut.

### <a name="message-feedback"></a>Viestin palaute

Kun lähetät viestin cloud laitteen, palvelun voi pyytää viestin kohti palautetta mahdollisiin Lopputila viestin toimittamisen.

- Jos asetat **Ack** -ominaisuuden arvoksi **positiivinen**, IoT keskittimeen Luo palaute-sanoman, jos ja vain, jos cloud laitteen viestin saavuttanut **Valmis** -tilaan.
- Jos asetat **Ack** -ominaisuuden arvoksi **negatiivinen**, IoT keskittimeen Luo palautetta, viestin vain, jos, cloud laitteen viestin saavuttaa **Deadlettered** tila.
- Jos asetat **Ack** -ominaisuuden arvoksi **koko**, IoT keskittimeen Luo palautetta viestin kummassakin tapauksessa.

> [AZURE.NOTE] Jos **Ack** on **täynnä**, et saa palautetta viestin se tarkoittaa palautetta viestin vanhentunut. Palvelu ei tiedä, mitä on tapahtunut alkuperäisen viestin. Harjoitus-palvelu on varmistettava, että se voi käsitellä palautetta, ennen kuin se päättyy. Suurin voimassaolon ajan kaksi päivää, joka sallii paljon aikaa palvelun suoritetaan uudelleen virheen ilmetessä.

[Päätepisteet]esitetyllä tavalla[lnk-endpoints], IoT keskittimeen toimittaa viesteinä palautetta palvelun osoittava päätepisteen (**/messages/servicebound/feedback**) kautta. Semantiikkaan liittyvien saada palautetta ovat samat kuin cloud laitteen viestit ja on saman [viestin elinkaari][lnk-lifecycle]. Mahdollisuuksien mukaan viestin palautetta erämuotoinen yhden viestin, on muotoa:

| Ominaisuus | Kuvaus |
| -------- | ----------- |
| EnqueuedTime | Aikaleima, joka ilmoittaa, kun viesti on luotu. |
| Käyttäjätunnus | `{iot hub name}` |
| ContentType | `application/vnd.microsoft.iothub.feedback.json` |

Tekstissä on JSON muuntaa sarjaksi taulukko tietueet, joissa seuraavat ominaisuudet:

| Ominaisuus | Kuvaus |
| -------- | ----------- |
| EnqueuedTimeUtc | Aikaleima, joka ilmoittaa, kun viestin tulos on tapahtunut. Esimerkiksi valmis laitteen tai viestin vanhentunut. |
| OriginalMessageId | **MessageId** , johon palautetta nämä tiedot koskevat cloud laitteen viestin. |
| StatusCode | Pakollinen kokonaisluku. Käyttää IoT keskittimeen luomia palautetta sanomia. <br/> 0 = onnistui <br/> 1 = viesti on vanhentunut <br/> 2 = suurin toimituksen määrä on saattanut ylittyä <br/> 3 = message hylätty |
| Kuvaus | Merkkijonon arvot **StatusCode**varten. |
| DeviceId | Kohdelaite, johon tämän osan palautetta liittymisestä cloud laitteen viestin **DeviceId** . |
| DeviceGenerationId | Kohdelaite, johon tämän osan palautetta liittymisestä cloud laitteen viestin **DeviceGenerationId** . |


>[AZURE.IMPORTANT] Palvelu on määritettävä **MessageId** cloud laitteen viestin voi yhdistää sen palautetta alkuperäisen viestin.

Seuraavassa esimerkissä esitetään palautetta viestin tekstiosaan.

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

### <a name="cloud-to-device-configuration-options"></a>Cloud laitteen kokoonpanoasetusten määrittäminen

Kunkin IoT-toiminnossa paljastaa seuraavat asetusvaihtoehdot cloud laitteen messaging.

| Ominaisuus | Kuvaus | Alue- ja oletusarvo |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | Cloud laitteen viestien (TTL) oletusarvo. | ISO_8601 aikaväli ylöspäin 2D (vähintään 1 minuutti). Oletusarvo: 1 tunti. |
| maxDeliveryCount | Suurin toimituksen cloud laitteen kohden-laite käyttöoikeuksien määrä. | 1 – 100. Oletus: 10. |
| feedback.ttlAsIso8601 | Palvelun sidottujen palautetta viestien säilytys. | ISO_8601 aikaväli ylöspäin 2D (vähintään 1 minuutti). Oletusarvo: 1 tunti. |
| feedback.maxDeliveryCount | Suurin toimitus palautetta jonon määrä. | 1 – 100. Oletus: 100. |

Lisätietoja on artikkelissa [Luo IoT keskittimet][lnk-portal].

## <a name="read-device-to-cloud-messages"></a>Laitteen cloud viestien lukeminen

IoT keskittimeen paljastaa päätepisteen taustatietokantaan palvelujen oman keskittimeen vastaanottanut laitteen cloud viestien lukemista varten. Päätepisteen tapahtuma keskittimeen-yhteensopiva, jonka avulla voit käyttää mitä tahansa järjestelmiä tapahtuman keskittimet palvelun tukee viestien lukeminen.

Kun käytät [Azure palvelun Bus SDK .NET] [ lnk-servicebus-sdk] tai [Tapahtuman keskittimet - tapahtuman suoritin Host][lnk-eventprocessorhost], voit käyttää minkä tahansa IoT keskittimeen yhteyden merkkijonot oikeat käyttöoikeudet. Tapahtuma-toiminnossa nimeksi käyttää **viestejä tai tapahtumat** .

Kun käytät SDK: T (tai tuotteen integroinnit), jotka eivät tiedä, IoT keskittimeen, hakemalla tapahtumaa-toiminnossa yhteensopivan päätepiste ja tapahtumaa-toiminnossa yhteensopivan nimi IoT keskittimeen asetuksista [Azure portal][lnk-management-portal]:

1. Valitse **viestit**IoT keskittimeen-sivu.
2. **Laitteen cloud asetukset** -kohdasta Etsi seuraavat arvot: **tapahtumaa-toiminnossa yhteensopivan päätepisteen**, **tapahtumaa-toiminnossa yhteensopivan nimi**ja **osiot**.

    ![Laitteen cloud asetukset][img-eventhubcompatible]

> [AZURE.NOTE] SDK vaatiiko **isäntänimi** tai **Namespace** arvon poistaminen **tapahtumaa-toiminnossa yhteensopivan päätepisteen**mallia. Esimerkiksi jos tapahtumaa-toiminnossa yhteensopivan päätepisteen **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, **Hostname** olisi **iothub-ns-myiothub-1234.servicebus.windows.net**ja **Namespace** olisi **iothub-ns-myiothub-1234**.

Voit käyttää mitä tahansa jaettua käyttöä suojauskäytäntö, joka on määritetty tapahtumaa-toiminnossa yhdistäminen **ServiceConnect** käyttöoikeudet.

Jos tarvitset luonnissa tapahtumaa-toiminnossa-yhteysmerkkijonon avulla aiempia tietoja, käytä seuraavaa kaavaa:

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

Seuraavassa on luettelo SDK: T ja integroinnit, voit käyttää tapahtumaa-toiminnossa yhteensopivan päätepisteet, joka paljastaa IoT-toiminnossa:

* [Java tapahtuman keskittimet asiakas](https://github.com/hdinsight/eventhubs-client)
* [Apache myrsky spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). Näet [tietolähteen spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) GitHub.
* [Apache ohjattu integrointi](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## <a name="reference-topics"></a>Viittaus aiheet:

Viittaus seuraavissa ohjeaiheissa on lisätietoja viestien vaihtaminen IoT keskittimeen kanssa.

## <a name="message-format"></a>Viestimuoto

IoT keskittimeen viestien kuuluvat:

* Joukko *järjestelmän ominaisuudet*. Ominaisuudet, jotka IoT keskittimeen tulkitsee tai määrittää. Tämä on ennalta.
* *Palvelusovelluksen ominaisuuksien*joukko. Sanasto, sovellus määrittää merkkijono-ominaisuuksien ja Accessissa, eikä hänen nimeään tarvitse, jos haluat poistaa viestin tekstiosaan. IoT toiminnossa Muokkaa koskaan nämä ominaisuudet.
* Peittävä binaarinen teksti.

Katso lisätietoja siitä, kuinka viesti on koodattu eri protokollat [IoT keskittimeen ohjelmointirajapinnan ja SDK: T][lnk-sdks].

Seuraavassa taulukossa on lueteltu järjestelmän ominaisuudet IoT keskittimeen sähköpostiviesteissä.

| Ominaisuus | Kuvaus |
| -------- | ----------- |
| MessageId | Viestin käytettäviä kuviot pyynnön vastaus käyttäjän asetettavan tunnus. Muoto: Kirjainkoko on merkitsevä tekstimerkkijonon (enintään 128: aa merkkiä) aakkosnumeerisia merkkejä ASCII-7-bittisten + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Järjestysnumero | Luku (yksilöllinen laitteen jonon kohden) määrittämä IoT keskittimeen cloud laitteen kaikkiin viesteihin. |
| Jos haluat | [Cloud] laitteen määritetty kohde[ lnk-c2d] viestejä. |
| ExpiryTimeUtc | Päivämäärä ja aika, viestin erääntyminen. |
| EnqueuedTime | Päivämäärä ja kellonaika IoT toiminnosta on vastaanottanut viestin. |
| CorrelationId | Merkkijono-ominaisuus, joka sisältää tavallisesti pyynnön pyynnön vastaus kuviot MessageId vastausviestien. |
| Käyttäjätunnus | Voit määrittää viestien alkuperän tunnus. Kun viestejä luomat IoT keskittimeen, se määritetään `{iot hub name}`. |
| ACK | Palautteen viestin luominen. Tätä ominaisuutta käytetään cloud laitteen viestien pyytää IoT-toiminnossa Luo palautetta viestien tuloksena viestin kulutus laite. Mahdolliset arvot: **ei mitään** (oletus): ei palautetta viesti on luonut, **positiivinen**: saada palautetta viestin, jos viesti on valmis, **negatiivinen**: saat viestin palautetta, jos viesti on päättynyt (tai suurin toimituksen määrä on saavutettu) ilman laitteen tai **täydet**suorittamisen: sekä positiivisia ja negatiivisia. Lisätietoja on artikkelissa [viestin palautetta][lnk-feedback]. |
| ConnectionDeviceId | Määrittää IoT keskittimeen laitteen cloud viesteille tunnus. Se sisältää laitteella, joka on lähettänyt viestin **deviceId** . |
| ConnectionDeviceGenerationId | Määrittää IoT keskittimeen laitteen cloud viesteille tunnus. Se sisältää **generationId** ( [laitteen käyttäjätietojen ominaisuuksien]mukaan[lnk-device-properties]) laite, jossa on lähettänyt viestin. |
| ConnectionAuthMethod | Todentamismenetelmä määrittää IoT keskittimeen laitteen cloud viesteihin. Tämä ominaisuus on tietoja todennetaan viestin lähettäminen laitteeseen todentamismenetelmän. Lisätietoja [laitteen cloud anti väärentää][lnk-antispoofing].|

## <a name="communication-protocols"></a>Viestintä-protokollat

IoT keskittimeen avulla voidaan käyttää [MQTT]laitteissa[lnk-mqtt], MQTT kautta WebSockets, [AMQP][lnk-amqp], AMQP WebSockets ja HTTP protokollat laitteen Include yhteyksiä varten. Seuraavassa taulukossa on korkean tason suosituksia valittua protokollan:

| Protokolla | Milloin kannattaa valita tämä protokolla |
| -------- | ------------------------------------ |
| MQTT <br> MQTT WebSocket päälle     | Käytä kaikissa laitteissa, joihin ei tarvitse liittää useilla eri laitteilla (jokaisella on oma laitteen kohti tunnistetiedot) saman TLS-yhteyden kautta. |
| AMQP <br> AMQP WebSocket päälle    | Käyttää kentän ja pilvi yhdyskäytävien hyödyntää yhteyden limittävän kaikissa laitteissa. |
| HTTP    | Käytä laitteita, jotka eivät tue muita protokollia. |

Valitessasi laitteen Include viestinnälle oman protokolla, ota huomioon seuraavat seikat:

* **Cloud laitteen kuvio**. HTTP ei ole tehokas tapa toteuttaa server push. Sellaisenaan, kun käytät HTTP-laitteet äänestyksen järjestäminen IoT keskittimeen cloud laitteen viestien. Tämä vaihtoehto on tehotonta laitteen ja IoT toiminnossa. Valitse nykyinen HTTP-ohjeiden mukaan laitteilta olisi äänestyksen järjestäminen viestien 25 minuutin välein tai Lisää. Toisaalta MQTT ja AMQP tukevat server push vastaanotettaessa cloud laitteen viestejä. Niiden avulla heti työntää viestien IoT-toiminnosta laitteeseen. Jos toimituksen viive on merkitystä, MQTT tai AMQP ovat parhaiten protokollat käyttämään. Vain harvoin yhdistettyjen laitteiden HTTP toimii myös.
* **Kentän yhdyskäytävät**. MQTT ja HTTP käytettäessä et voi muodostaa useilla laitteilla (jokaisella on oma laitteen kohti tunnistetiedot) saman TLS-yhteyden avulla. Näin, [kenttä yhdyskäytävän skenaarioita]varten[lnk-azure-gateway-guidance]-protokollat ovat lainkaan, koska ne edellyttävät jokin TLS yhteyden kentän yhdyskäytävän ja IoT keskittimeen välillä kunkin laitteen yhdistetty kenttä-yhdyskäytävä.
* **Pieni resurssin laitteet**. MQTT ja HTTP-kirjastot on pienempi vaatiman, kuin AMQP-kirjastot-tallennustilan. Sellaisenaan, jos laite on rajoitettu resurssit (esimerkiksi, pienempi kuin 1 Megatavua RAM-Muistia)-protokollat voi olla käytettävissä vain yhteyskäytännön toteutus.
* **Verkon ohitus**. Vakio AMQP-protokolla käyttää porttia 5671, kun MQTT seuraa porttia 8883, joka saattaa aiheuttaa ongelmia, jotka on suljettu-HTTP protokollat verkot. MQTT WebSockets päälle, AMQP WebSockets ja HTTP ovat käytettävissä, jota käytetään tässä skenaariossa.
* **Tietojen kooksi**. MQTT ja AMQP ovat binaarinen protokollia, jotka aiheuttavat tiiviimmän paketteja kuin HTTP.

> [AZURE.NOTE] HTTP käytettäessä cloud laitteen viestien olisi kyselyn kunkin laitteen 25 minuutin välein tai Lisää. Aikana, on kuitenkin hyväksyttävä useammin 25 minuutin välein lisääminen.

## <a name="port-numbers"></a>Porttinumerot

Laitteet voivat viestiä käyttämällä eri protokollat Azure IoT-toiminnosta. Yleensä sen ratkaisun perustuva protokolla valinta. Seuraavassa taulukossa on lueteltu lähtevä, jossa on oltava laite, kun haluat käyttää tiettyä protokollaa avoimet portit:

| Protokolla | Portit |
| -------- | ------- |
| MQTT     | 8883    |
| MQTT WebSockets päälle | 443    |
| AMQP     | 5671    |
| AMQP WebSockets päälle | 443    |
| HTTP     | 443     |
| LWM2M (hallinta) | 5684 |

Kun olet luonut IoT-toiminnossa Azure alueen, valitsemalla säilyy toimiva elinkaaren sama IP-osoite. Kuitenkin säilyttää palvelun laatua, jos Microsoft siirtyy IoT-toiminnossa eri asteikko valitse se on määritetty uusi IP-osoite.

## <a name="notes-on-mqtt-support"></a>Muistiinpanojen MQTT tuki

IoT-toiminto ottaa käyttöön seuraavat rajoitukset ja tietyn toiminnan MQTT v3.1.1-protokollan:

  * **QoS 2 ei tueta**. Kun laite on jokin muu sähköpostiohjelma **QoS 2**sisältävän viestin, IoT keskittimeen sulkee verkkoyhteyttä. Kun laite on jokin muu sähköpostiohjelma tilaa aiheen **QoS**2, IoT keskittimeen antaa suurin QoS tason 1 **SUBACK** -paketissa.
  * **Säilytä viestit eivät säily**. Jos laite on jokin muu sähköpostiohjelma julkaisee viestin Säilytä-merkin 1, Lisää IoT keskittimeen **x-osallistua-säilyttää** sovelluksen ominaisuus viestiin. Tässä tapauksessa IoT keskittimeen ei säily Säilytä viestin, mutta välittää sen sijaan taustatietokantaan-sovellukseen.

Lisätietoja on artikkelissa [IoT keskittimeen MQTT tukevat][lnk-devguide-mqtt].

Lopullinen huomiota kuin kannattaa tutustua [Azure IoT protokolla yhdyskäytävän] [ lnk-azure-protocol-gateway] , jonka avulla voit ottaa käyttöön, sillä suoraan IoT toiminnosta on tehokas mukautettu protokolla yhdyskäytävän. Azure IoT protokolla yhdyskäytävän avulla voit mukauttaa sopimaan brownfield MQTT ominaisuuksissa laite-protokollan tai muita mukautettuja protokollia. Tämän menetelmän edellyttävät, suorittaa ja toimivat mukautettu protokolla-yhdyskäytävä.

## <a name="additional-reference-material"></a>Lisää aineistoa

Kehittäjän opas muiden viittauksen aiheet ovat seuraavat:

- [IoT keskittimeen päätepisteet] [ lnk-endpoints] kuvataan eri päätepisteet, jotka kunkin IoT-toiminnossa paljastaa avaamiseen suorituksenaikainen ja hallintaa varten.
- [Rajoitusten ja kiintiöiden] [ lnk-quotas] kuvataan kiintiön, jotka koskevat IoT keskittimeen-palveluun ja rajoittava toiminnan toiminta palvelua käytettäessä.
- [IoT keskittimeen laite- ja palveluluettelon SDK: T] [ lnk-sdks] näyttää eri kielellä SDK: T voit Käytä tätä, kun laite ja palvelun sovelluksista, jotka toimivat IoT keskittimeen ja kehittää.
- [Kyselyn kielen twins, menetelmistä ja töiden] [ lnk-query] kuvataan kyselykieltä, voit hakea tietoja IoT-toiminnosta laitteen twins, menetelmät ja projektit.
- [IoT keskittimeen MQTT tuki] [ lnk-devguide-mqtt] on lisätietoja IoT keskittimeen tuki MQTT-protokollaa.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet oppinut lähettää ja vastaanottaa viestejä IoT keskittimeen kanssa, voit tarkastella Sovelluskehittäjän opas seuraavissa ohjeaiheissa:

- [Lataa tiedostot laitteesta][lnk-devguide-upload]
- [Laitteen IoT toiminnossa käyttäjätietojen hallinta][lnk-devguide-identities]
- [IoT pääsivuston käyttöoikeuksien hallinta][lnk-devguide-security]
- [Synkronoinnin tilan ja määritykset laitteen twins avulla][lnk-devguide-device-twins]
- [Suora menetelmän laitteessa][lnk-devguide-directmethods]
- [Ajoita työt useilla eri laitteilla][lnk-devguide-jobs]

Jos haluat kokeilla osa käsitteistä, joka on kuvattu tämän artikkelin voi olla kiinnostuneita IoT keskittimeen opetusohjelmassa:

- [Azure IoT keskittimeen käytön aloittaminen][lnk-getstarted-tutorial]
- [Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen][lnk-c2d-tutorial]
- [Voit käsitellä IoT keskittimeen laitteen cloud viestit][lnk-d2c-tutorial]


[img-lifecycle]: ./media/iot-hub-devguide-messaging/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide-messaging/eventhubcompatible.png

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-management-portal]: https://portal.azure.com
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-portal]: iot-hub-create-through-portal.md

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-compatible-endpoint]: iot-hub-devguide-messaging.md#read-device-to-cloud-messages
[lnk-protocols]: iot-hub-devguide-messaging.md#communication-protocols
[lnk-message-format]: iot-hub-devguide-messaging.md#message-format
[lnk-d2c-configuration]: iot-hub-devguide-messaging.md#device-to-cloud-configuration-options
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-ttl]: iot-hub-devguide-messaging.md#message-expiration-time-to-live
[lnk-c2d-configuration]: iot-hub-devguide-messaging.md#cloud-to-device-configuration-options
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-feedback]: iot-hub-devguide-messaging.md#message-feedback
[lnk-antispoofing]: iot-hub-devguide-messaging.md#anti-spoofing-properties
[lnk-compare]: iot-hub-compare-event-hubs.md

[lnk-devguide-upload]: iot-hub-devguide-file-upload.md
[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx


[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
