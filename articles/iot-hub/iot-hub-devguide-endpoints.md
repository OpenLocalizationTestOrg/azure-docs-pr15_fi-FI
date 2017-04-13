<properties
 pageTitle="Sovelluskehittäjän opas - IoT keskittimeen päätepisteet | Microsoft Azure"
 description="Azure IoT keskittimeen developer guide - IoT keskittimeen päätepisteet tietoja"
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

# <a name="reference---iot-hub-endpoints"></a>Viittaus - toiminnossa IoT päätepisteet

## <a name="list-of-iot-hub-endpoints"></a>IoT keskittimeen päätepisteet luettelo

Azure IoT-toiminnossa on usean vuokraajan-palvelu, joka paljastaa eri toimijoiden niiden toimintoja. Seuraavassa kaaviossa on esitetty eri päätepisteet IoT keskittimeen paljastaa.

![IoT keskittimeen päätepisteet][img-endpoints]

Seuraavassa on päätepisteet kuvaus:

* **Resurssin toimittaja**. IoT keskittimeen resurssin provider hyödyntää [Azure Resurssienhallinta] [ lnk-arm] , jonka avulla Azure tilauksen omistaja ja luoda IoT keskittimet ja Päivitä IoT keskittimeen ominaisuudet. IoT keskittimeen ominaisuudet ohjaavat [keskittimeen tason suojauskäytäntöjä][lnk-accesscontrol], eikä laitteen tason käyttöoikeuksien hallinta ja toimintojen asetukset cloud laitteen ja laitteen cloud viesti. IoT keskittimeen resurssin tarjoajaan mahdollistaa myös viedä [laitteen käyttäjätietojen][lnk-importexport].
* **Tunnistetietojen hallinta**. Kunkin IoT-toiminnossa paljastaa HTTP muiden päätepisteet laitteen jäsenyyksiä hallitaan joukko (luominen, hakea, päivittäminen ja poistaminen). [Laitteen käyttäjätietojen] [ lnk-device-identities] käytetään laitteen todennus ja käyttöoikeuksien hallinta.
* **Sekä hallinta**. Kunkin IoT-toiminnossa paljastaa palvelun osoittava HTTP muiden päätepisteen kysely ja Päivitä [laitteen twins] joukko[ lnk-twins] (Päivitä tunnisteet ja ominaisuudet).
* **Töiden hallinta**. Kunkin IoT-toiminnossa paljastaa palvelun osoittava HTTP muiden päätepiste kyselyn ja hallita [töitä]joukko[lnk-jobs].
* **Laitteen päätepisteet**. Laitteen tunnistetietojen rekisterin valmisteltu pienoisohjelman IoT keskittimeen paljastaa päätepisteet, laitteen avulla voit lähettää ja vastaanottaa viestejä joukko:
    - *Laitteen cloud lähettämiseen*. Käytä tämän päätepisteen [laitteen cloud viestien]lähettämiseen[lnk-d2c].
    - *Vastaanota cloud laitteen viestit*. Laite käyttää tämän päätepisteen kohdennettujen [cloud laitteen viestien][lnk-c2d].
    - *Aloita tiedostojen lataaminen palvelimeen*. Laitteen käyttää tämän päätepisteen vastaanottamiseen Azure tallennustilan SAS-URI IoT-toiminnosta Lataa [Tiedosto][lnk-upload].
    - *Noutaa ja Päivitä sekä ominaisuudet*. Laitteen käyttää tätä päätepisteet sen [laitteen sekä][lnk-twins]käyttäjän ominaisuudet.
    - *Vastaanota suoraan menetelmiä pyynnöt*. Laite käyttää tätä päätepisteet kuunnella [suoraan menetelmiä][lnk-methods]'s pyynnöt.

    Nämä päätepisteet niitä julkaista [MQTT v3.1.1][lnk-mqtt], HTTP 1.1 ja [AMQP 1.0] [ lnk-amqp] protokollat. Huomaa, että AMQP on myös käytettävissä päälle [WebSockets] [ lnk-websockets] porttiin 443.
    
    Twins' ja menetelmiä endpoins ovat käytettävissä vain [MQTT v3.1.1][lnk-mqtt].

* **Palvelupäätepisteet**. Kunkin IoT-toiminnossa paljastaa joukko päätepisteet sovelluksen takaisin päässä avulla yhteydenpito laitteet. Nämä päätepisteet ovat tällä hetkellä vain näkyviä käyttämällä [AMQP] [ lnk-amqp] protokolla, lukuun ottamatta menetelmän kutsu päätepiste, joka on määritetty HTTP 1.1 kautta.
    - *Vastaanota laitteen cloud viestit*. Tämä päätepiste on yhteensopiva [Azure tapahtuman keskittimet][lnk-event-hubs]. Taustatietokannan palvelu käyttää sitä lukea kaikki [laitteen cloud viestien] [ lnk-d2c] lähettämä laitteet.
    - *Cloud laitteen Lähetä ja vastaanota toimituksen vahvistus*. Nämä päätepisteet ottaa käyttöön sovelluksen takaisin päässä luotettava [cloud laitteen viestien][lnk-c2d], ja vastaanottamiseen vastaavan toimitus- tai vanheneminen vahvistus.
    - *Vastaanota tiedosto ilmoitukset*. Tämä tekstiviesti päätepiste voit saada ilmoituksen, kun tiedoston lataaminen onnistuu laitteet. 
    - *Suora menetelmä kutsu*. Tämän päätepisteen avulla taustatietokantaan palvelu [Suora menetelmä] [ lnk-methods] laitteessa.

- [IoT keskittimeen ohjelmointirajapinnan ja SDK: T] [ lnk-sdks] artikkelissa voidaan käyttää näitä päätepisteet usealla tavalla.

Lopuksi on tärkeää muistaa, että kaikki IoT keskittimeen päätepisteet käyttää [TLS] [ lnk-tls] protokolla ja päätepistettä on joskus määritetty salaamattomana/suojaamaton kanavat.

## <a name="field-gateways"></a>Kentän yhdyskäytävät

IoT ratkaisussa *kentän yhdyskäytävän* sijaitsee laitteet ja IoT keskittimeen päätepisteet välillä. On yleensä sijaitsevat lähellä laitteet. Laitteet yhteyttä suoraan kentän yhdyskäytävän käyttämällä tukemat laitteet-protokollaa. Kenttä-yhdyskäytävä yhdistää IoT keskittimeen päätepisteen avulla protokolla, joka tue IoT toiminnossa. Kentän yhdyskäytävän voi olla erittäin erityisiä laitteisto tai ohjelmisto, joka suorittaa loppuun skenaario, jonka yhdyskäytävä on tarkoitettu pienen power tietokoneessa.

Voit käyttää [Azure IoT yhdyskäytävän SDK] [ lnk-gateway-sdk] toteuttamisesta kentän yhdyskäytävä. Tämä SDK on tiettyjen toimintojen, kuten mahdollisuus multiplex tietoliikenne useilla laitteilla sivulle samaa yhteyttä IoT toiminnossa.

## <a name="next-steps"></a>Seuraavat vaiheet

Muiden viittauksen aiheita IoT keskittimeen developer oppaan ovat:

- [Kyselykielen twins, menetelmistä ja työt][lnk-devguide-query]
- [Kiintiöiden ja rajoittaminen][lnk-devguide-quotas]
- [IoT keskittimeen MQTT tuki][lnk-devguide-mqtt]

[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md