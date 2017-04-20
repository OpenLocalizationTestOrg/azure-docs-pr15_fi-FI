> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)

## <a name="introduction"></a>Johdanto

[Aloittaminen IoT keskitin twins]-[lnk-twin-tutorial], voit määrittää laitteen metatiedot *tunnisteita*, raportin laitteen edellytykset *raportoitu ominaisuudet*laitteen-Appista ratkaisun taakse päässä oppinut ja kyselyn tiedot kaltaisten SQL-kielen avulla.

Tässä opetusohjelmassa opit käyttämään yhteydessä *ilmoitettu ominaisuuksia*, voit määrittää laitteen apps etäyhteyden kautta sekä *haluamasi ominaisuudet* . Tarkemmin sanoen tämä opas näyttää miten sekä on raportoitu ja halutut ominaisuudet käyttöön laitteen sovellusasetus monivaiheinen kokoonpanoa ja antaa näkyvyyttä ratkaisu tämän toiminnon tilan takaisin loppuun kaikkien laitteiden välillä.

Tässä opetusohjelmassa seuraa *kuvion haluttu tila* Laitehallinnan korkealla tasolla. Tämän kuvion perusajatus on antaa ratkaisun Taustatietokannassa määrittää haluttu tila hallittujen laitteiden lähettää tiettyjä komentoja. Tämä laite vastaa perustamisesta paras tapa saavuttaa haluttu tila (erittäin tärkeä IoT tilanteissa, joissa tietyn laitteen edellytykset vaikuttaa välittömästi suorittaa tiettyjä komentoja), Lisää ilmoittaessasi jatkuvasti taustatietokantaan nykytilan ja mahdollisia virhetilanteita. Haluttu tila kuvio on Instrumentaalinen hallinta suurten joukkojen laitteita, koska se mahdollistaa Taustatietokannassa on täysi näkyvyys määritysprosessi valtion välillä kaikki laitteet.
Löydät lisätietoja roolin kuvion haluttu tila Laitehallinnan [Azure IoT keskitin yleistä Laitehallinta]-[lnk-dm-overview].

> [AZURE.NOTE] Tilanteissa, joissa laitteita hallitaan interaktiivisemman muoti (Ota tuuletin käyttäjän hallitsema Appista), harkitse [cloud laitteen menetelmiä]käyttäen[lnk-methods].

Tässä opetusohjelmassa Taustatietokannassa sovellusmuutoksia kaukomittaus configuration target-laitteen ja seurauksena laite app, seuraa monivaiheisen prosessin määritys-päivityksen (vaativat esim ohjelmiston moduuli uudelleen), joka tässä opetusohjelmassa Simuloi yksinkertainen viiveellä).

Back-end tallentaa haluamasi ominaisuudet laitteen sekä kokoonpanon seuraavalla tavalla:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [AZURE.NOTE] Koska kokoonpanot voivat olla monimutkaisia objekteja, ne määritetään yleensä yksilölliset tunnukset (hajautusarvot tai [GUID-tunnuksia][lnk-guid]) helpottaa niiden vertailuja.

Laitteen sovellus ilmoittaa sen nykyisen kokoonpanon peilaus halutun ominaisuuden **telemetryConfig** on raportoitu ominaisuudet:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Huomaa, miten raportoidun **telemetryConfig** Lisää ominaisuus on **tila**, tilan kokoonpanon päivityksen aikana käytettävä.

Saapuessa uusi halutun kokoonpanon laitteen app ilmoittaa odottavat kokoonpano muuttamalla tiedot:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Sitten myöhemmin joidenkin laitteen app raportoi tämän toiminnon epäonnistumisen menestykseen edellä mainitun ominaisuuden päivittämällä.
Huomaa, kuinka on mahdollista, milloin tahansa kyselyn määrityksen tila kaikkien laitteiden välillä.

Tässä opetusohjelmassa näytetään, miten voit:

- Luo simuloidun laite, joka vastaanottaa uudelleen määritysten päivitykset ja ilmoittaa useita päivityksiä *raportoitu* ominaisuuksina määrityksen päivitys.
- Luoda back-end-app, joka päivittää laitteen halutun kokoonpanon ja kysyy määrityksen päivitys.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier