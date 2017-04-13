<properties
 pageTitle="Kehittäjän ohje - ymmärtää laitteen twins | Microsoft Azure"
 description="Azure IoT keskittimeen developer guide - käyttö laitteen twins tila- ja määritystietoja tietojen IoT keskittimeen ja laitteiden synkronointi"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="understand-device-twins---preview"></a>Tietoja laitteen twins - esikatselu

## <a name="overview"></a>Yleiskatsaus

*Laitteen twins* on otettava huomioon JSON-tiedostot, joihin voidaan tallentaa laitteen tiedot (metatiedot, määritykset ja ehdot). IoT keskittimeen jatkuu laite-sekä kunkin laitteen, johon muodostat yhteyden IoT keskittimeen. Tässä artikkelissa kuvataan:

* laitteen sekä rakenteen: *tunnisteita*, *haluamasi* ja *raportoitu ominaisuudet*, ja
* toiminnoista, joita laite twins suorittaa sovelluksille ja takaisin päättyy.

> [AZURE.NOTE] Tällä hetkellä voi käyttää vain IoT keskittimeen MQTT-protokollan avulla yhteyden muodostavien laitteiden laitteen twins. Lue lisätietoja [MQTT tukevat] [ lnk-devguide-mqtt] artikkelista ohjeita siitä, miten voit muuntaa olemassa olevan laitteen-sovelluksen käyttämään MQTT.

### <a name="when-to-use"></a>Käyttäminen

Laitteen twins avulla:

* Tallenna pilvipalveluun, kuten käyttöönoton sijainti luokkaan koneen laitteen tietyn metatiedon.
* Raportin nykyisen tilan tiedot, kuten käytettävissä olevat ominaisuudet ja laite-sovellukset, kuten-yhteyden kautta Matkapuhelin tai wifi laitteen ehdot.
* Synkronoinnin tilan pitkään käynnissä olevien työnkulkujen välillä laitteen sovellus- ja Edellinen loppuun, kuten takaisin Lopeta määrittäminen uuden laitteisto-version asentaminen ja laitteen sovelluksen raportoinnin Päivitysprosessista eri vaiheista.
* Kyselyn laitteen metatiedon, määrittäminen tai tila.

Käytä [laitteen cloud viestien] [ lnk-d2c] aikaleimattu tapahtumia, kuten aikasarjalle tunnistimen tietojen tai hälytykset jaksojen. [Cloud laitteen] menetelmillä[ lnk-methods] laitteita, kuten otetaan käyttöön puhallinta vuorovaikutteinen hallintaan.

## <a name="device-twins"></a>Laitteen twins

Laitteen twins tallentaa laitteen liittyviä tietoja, jotka:

- Voit synkronoida laitteen ehdot- ja määritystietoja käyttämällä laite- ja Edellinen päättyy.
- Kyselyn ja kohde pitkään suoritettavien käyttämällä sovelluksen takaisin loppu toimintoja.

Laitteen sekä elinkaari on linkitetty vastaavan [laitteen tunnistetietojen][lnk-identity]. Twins luodaan implisiittisesti ja poistetaan, kun uusi laitteen käyttäjätieto luodaan tai poistetaan IoT toiminnossa.

Laitteen sekä on JSON-asiakirjaan, joka sisältää:

* **Tunnisteet**. JSON asiakirjan lukeminen ja kirjoittaa ne uudelleen. Tunnisteet eivät ole näkyvissä sovelluksille.
* **Halutuksi ominaisuudet**. Laitteen kokoonpanotietoja tai ehto synkronointi yhdessä raportoidun ominaisuuksien avulla. Haluamasi ominaisuudet voidaan määrittää vain sovelluksen takaisin loppuun ja voi lukea laitteen-sovelluksessa. Laitteen sovelluksen aina ilmoituksen myös reaaliajassa muutokset, valitse haluamasi ominaisuudet.
* **Ilmoitettu ominaisuudet**. Synkronoi laitteen kokoonpanotietoja tai ehto käyttää yhdessä haluamasi ominaisuudet. Raportoidun ominaisuudet voidaan määrittää vain laite-sovelluksessa, ja voit voi lukea ja kysely sovelluksen takaisin loppuun mennessä.

Lisäksi laitteen sekä ylimmällä on vain luku-ominaisuuksia vastaavan käyttäjätietojen sisältämien [laitteen tunnistetietojen rekisterin][lnk-identity].

![][img-twin]

Tässä on esimerkki laitteen sekä JSON asiakirjan:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

Pääobjektin, ovat järjestelmän ominaisuuksia ja säilön objektit `tags` sekä `reported` ja `desired properties`. `properties` Säilö on vain luku-osia (`$metadata`, `$etag`, ja `$version`), joka on kuvattu vastaavasti [sekä metatietojen] [ lnk-twin-metadata] ja [optimistisen] [ lnk-concurrency] osia.

### <a name="reported-property-example"></a>Raportoidun ominaisuus-Esimerkki

Edellä olevassa esimerkissä laitteen sekä sisältää `batteryLevel` ominaisuus, joka ilmoittaa laite-sovellus. Tämä ominaisuus mahdollistaa sellaisten kysely ja toimivat laitteet raportoidun viimeisen tason perusteella. Toinen esimerkki on laitteen sovelluksen raportin laitteen ominaisuuksia tai yhteysasetukset.

Huomaa, miten raportoidun ominaisuudet yksinkertaistaa skenaariot, jossa uudelleen kiinnostuneita ominaisuuden tunnetut viimeisen arvon. Käytä [laitteen cloud viestien] [ lnk-d2c] Jos uudelleen käsiteltävä laitteen telemetriatietojen aikaleimattu tapahtumia, kuten aikasarjalle järjestys lomakkeessa.

### <a name="desired-configuration-example"></a>Uudet määritykset-Esimerkki

Edellä olevassa esimerkissä `telemetryConfig` haluamasi ja raportoidun ominaisuuksia käytetään takaisin pää- ja laitteen sovelluksen synkronoimaan laite telemetriatietojen-määritys. Esimerkki:

1. Sovelluksen takaisin loppuun määrittää haluamasi ominaisuuden uudet määritykset-arvolla. Näin halutun ominaisuuden tiedoston osa:

        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
        
2. Laitteen sovelluksen ilmoitetaan heti, jos yhteydessä muutoksen, tai ensimmäisen Yhdistä. Laitteen sovellus sitten raportoi päivitetyt määritys (tai -virhe-ehdon käyttäminen `status` ominaisuutta). Näin raportoidun ominaisuudet osa:

        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...

3. Sovelluksen takaisin loppuun pitää määritys-toiminnon tulokset seuraat useita laitteita, [Kyselyt] [ lnk-query] twins.

> [AZURE.NOTE] Yllä katkelmat esimerkeissä, optimoitu mahdollisilla tavoilla koodata laitteen määrittäminen ja sen tilan luettavuutta. IoT keskittimeen asettamatta laitteen twins haluamasi ja raportoidun ominaisuudet tiettyä rakennetta.

Monissa tapauksissa twins käytetään synkronoimaan pitkään suoritettavien toimintoja, kuten laiteohjelmiston. Viitata [haluamasi ominaisuuksien määrittäminen laitteet] [ lnk-twin-properties] lisätietoja siitä, miten voit synkronoida ja seurata pitkään ominaisuuksien avulla käynnissä toimintojen kaikissa laitteissa.

## <a name="back-end-operations"></a>Taustatietokannan toiminnot

Uudelleen toimii sekä käyttämällä tarjoamia HTTP seuraavat atomisia toiminnot:

1. **Noutaa sekä tunnuksen mukaan**. Tämä toiminto palauttaa sisältöä sekä asiakirjana, tunnisteiden ja halutessasi ilmoittaa ja järjestelmän ominaisuudet.
2. **Päivitä osittain sekä**. Tämän toiminnon avulla takaisin päässä Päivitä osittain sekä tunnisteita tai haluamasi ominaisuudet. Osittaisen päivityksen ilmaistaan JSON-asiakirja, joka lisää tai päivittää mainituista ominaisuuksista. Ominaisuuksien `null` poistetaan. Esimerkiksi seuraava Luo uuden haluamasi ominaisuuden arvon `{"newProperty": "newValue"}`, korvaa olemassa olevan arvon `existingProperty` kanssa `"otherNewValue"`, ja poistaa kokonaan `otherOldProperty`. Toisiin aiemmin haluamasi ominaisuudet ja tunnisteita tapahtuu muutoksia:

        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }

3. **Korvaa haluamasi ominaisuudet**. Tämän toiminnon avulla takaisin loppu kokonaan korvaa kaikki aiemmin luodut haluamasi ominaisuudet ja korvata JSON asiakirjan yhteensopivuuden `properties/desired`.
4. **Korvaa tunnisteet**. Analogously tämän toimintojen avulla korvaamaan haluamasi ominaisuudet takaisin päässä kokonaan korvaa kaikki aiemmin tunnisteet ja korvata JSON asiakirjan yhteensopivuuden `tags`.

Kaikki edellä mainitut toiminnot tukevat [optimistisen] [ lnk-concurrency] ja vaatia **ServiceConnect** käyttöoikeus, [Suojaus] -määritysten[ lnk-security] artikkelissa.

Lisäksi seuraavat toiminnot uudelleen tehdä kyselyn käyttäminen kaltaisessa SQL- [kyselyn kielen]twins[lnk-query], ja suorita suurten tietojoukkojen käyttämällä [työt]twins[lnk-jobs].

## <a name="device-operations"></a>Laitteen toiminnot

Laite-sovellus toimii sekä käyttämällä atomisia seuraavat toimenpiteet:

1. **Noutaa sekä**. Tämä toiminto palauttaa sekä asiakirjan sisällön (tunnisteiden ja haluttu, ilmoitettu ja järjestelmän ominaisuudet) tällä hetkellä yhteydessä laitteen.
2. **Osittain päivityksen raportoitu ominaisuudet**. Tämä toiminto ottaa käyttöön tällä hetkellä yhteydessä laitteen raportoidun ominaisuudet osittaisen päivityksen. Tämä käyttää samaa JSON päivityksen muotoa takaisin loppu vastakkaisten osittaisen päivityksen haluamasi ominaisuudet.
3. **Käyttäjätietueissa haluamasi ominaisuudet**. Tällä hetkellä yhteydessä laitteen halutessasi ilmoittaa haluamasi ominaisuudet päivitykset heti, kun näyttöön reaaliajassa. Laite vastaanottaa saman lomakkeen päivityksen (osittain tai kokonaan vaihto) suorittaa uudelleen.

Kaikki edellä mainitut toiminnot vaativat **DeviceConnect** , oikeudet, määritysten [Suojaus] -[ lnk-security] artikkelissa.

[Azure IoT laitteen SDK: T] [ lnk-sdks] avulla on helppo käyttää useita kieliä ja ympäristöjen yllä toiminnot. Saat lisätietoja IoT keskittimeen perusalkioiden haluamasi ominaisuudet synkronoinnin tiedot löytyvät [laitteen uuden yhteyden työnkulku][lnk-reconnection].

> [AZURE.NOTE] Tällä hetkellä laitteen twins voi käyttää vain IoT keskittimeen MQTT-protokollan avulla yhteyden muodostavien laitteiden.

## <a name="reference-topics"></a>Viittaus aiheet:

Viittaus seuraavissa ohjeaiheissa on lisätietoja IoT-toiminnossa käytön hallinnasta.

## <a name="tags-and-properties-format"></a>Tunnisteet ja ominaisuudet-muoto

Tunnisteet-haluamasi ja raportoidun ominaisuudet ovat JSON-objektit, joissa on seuraavat rajoitukset:

* Kaikki avaimet JSON objekteja ovat kirjainkoko on merkitsevä 128 merkki UNICODE-merkkijonoja. Sallittu merkkien pois UNICODE-ohjausmerkit (osia C0 ja C1), ja `'.'`, `' '`, ja `'$'`.
* JSON objektin kaikki arvot voivat olla seuraavia JSON: totuusarvo, luku-, merkkijono-, objekti. Matriisit eivät ole sallittuja.

## <a name="twin-size"></a>Sekä kokoa

IoT keskittimeen pakottaa 8 Knowledge Base-enimmäiskoko arvojen mukaan `tags`, `properties/desired`, ja `properties/reported`, lukuun ottamatta vain luku-osat.
Koon lasketaan mukaan lukien kaikki merkit lukuun ottamatta UNICODE hallita merkkiä (osia C0 ja C1) ja tilaa `' '` milloin se näkyy merkkijonovakio ulkopuolella.
IoT keskittimeen hylkää virheen vuoksi kaikki toiminnot, jotka suurentaa ylittävät tiedostot.

## <a name="twin-metadata"></a>Sekä metatiedot

IoT keskittimeen ylläpitää viimeisimmän päivityksen JSON kullekin objektille haluamasi ja raportoidun ominaisuuksien aikaleimaa. Aikaleimat ovat UTC-ajan ja koodattu [ISO8601] muodossa `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Esimerkki:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

Nämä tiedot on käytettävissä jokaisella tasolla (et pelkästään JSON rakenteen lehdet) voit säilyttää päivitykset, jotka poistaa objektin.

## <a name="optimistic-concurrency"></a>Optimistisen

Tunnisteet haluamasi ja raportoidun ominaisuudet kaikki tue optimistisen.
Tunnisteet on etag, kuten [RFC7232], joka vastaa haluamaasi tunnistetta JSON-esitys. Voit tehdä tämän ehdollinen päivityksen toimintojen takaisin lopusta yhdenmukaisuutta.

Haluamasi ja raportoidun ominaisuuksia ei ole etags, mutta se on `$version` arvo, joka on varmasti vaiheittainen. Analogously, etag versio voidaan käyttää esimerkiksi (laitteen sovellus raportoidun ominaisuuden) tai Edellinen loppuun haluamasi ominaisuuden päivittäminen osapuolen voidaan valvoa yhtenäisyyden päivitykset.

Versioiden on hyötyä, kun observing agentti (esimerkiksi laite-sovellus on noudattaen haluamasi ominaisuudet) myös pitää täsmäyttää races Nouda-toiminnon tulos ja Päivitä-ilmoitus. [Laitteen uuden yhteyden työnkulku] -osassa[ lnk-reconnection] sisältää enemmän tietoa.

## <a name="device-reconnection-flow"></a>Laitteen uuden yhteyden työnkulku

IoT keskittimeen Säilytä yhteys katkaistu-laitteiden haluamasi ominaisuudet päivitysilmoitukset. Seuraa laitteella, joka muodostaa yhteyden tulee hakea koko haluamasi ominaisuudet-asiakirjan lisäksi tilattaessa päivitysilmoitukset. Valita vahinkojen races päivitysilmoitukset – koko noutamista, seuraavat työnkulku on varmistettava:

1. Laitteen sovelluksen muodostaa yhteyden IoT-toiminnossa.
2. Laitteen sovelluksen tilaa haluamasi ominaisuuksien päivitysilmoitukset.
3. Laitteen app hakee koko asiakirjan haluamasi ominaisuudet.

Laitteen sovellus voi ohittaa kaikki ilmoitukset, joiden `$version` pienempi tai yhtä kuin haetut asiakirjan koko versio. Tämä on mahdollista, koska IoT keskittimeen takaa versiot aina lisäys.

> [AZURE.NOTE] Tämä logiikka on jo käytössä [Azure IoT laitteen SDK: T][lnk-sdks]. Kuvaus kannattaa käyttää vain, jos laitteen sovellus ei voi käyttää mitä tahansa Azure IoT laitteen SDK: T ja suoraan ohjelman MQTT käyttöliittymässä.

## <a name="additional-reference-material"></a>Lisää aineistoa

Kehittäjän opas muiden viittauksen aiheet ovat seuraavat:

- [IoT keskittimeen päätepisteet] [ lnk-endpoints] kuvataan eri päätepisteet, jotka kunkin IoT-toiminnossa paljastaa avaamiseen suorituksenaikainen ja hallintaa varten.
- [Rajoitusten ja kiintiöiden] [ lnk-quotas] kuvataan kiintiön, jotka koskevat IoT keskittimeen-palveluun ja rajoittava toiminnan toiminta palvelua käytettäessä.
- [IoT keskittimeen laite- ja palveluluettelon SDK: T] [ lnk-sdks] näyttää eri kielellä SDK: T voit Käytä tätä, kun laite ja palvelun sovelluksista, jotka toimivat IoT keskittimeen ja kehittää.
- [Kyselyn kielen twins, menetelmistä ja töiden] [ lnk-query] kuvataan kyselykieltä, voit hakea tietoja IoT-toiminnosta laitteen twins, menetelmät ja projektit.
- [IoT keskittimeen MQTT tuki] [ lnk-devguide-mqtt] on lisätietoja IoT keskittimeen tuki MQTT-protokollaa.

## <a name="next-steps"></a>Seuraavat vaiheet

Olet nyt asiat laitteen twins tietoja, voi olla kiinnostuneita Sovelluskehittäjän opas seuraavissa ohjeaiheissa:

- [Suora menetelmän laitteessa][lnk-methods]
- [Ajoita työt useilla eri laitteilla][lnk-jobs]

Jos haluat kokeilla osa käsitteistä, joka on kuvattu tämän artikkelin voi olla kiinnostuneita IoT keskittimeen opetusohjelmassa:

- [Laitteen sekä käyttäminen][lnk-twin-tutorial]
- [Sekä ominaisuuksien käyttäminen][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png