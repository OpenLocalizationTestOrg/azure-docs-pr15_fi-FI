<properties
 pageTitle="Vertaa Azure IoT keskittimeen Azure tapahtuman porttiin | Microsoft Azure"
 description="Korostuksen toiminnalliset erot ja käytä tapauksissa Azure IoT keskittimeen ja Azure tapahtuman keskittimet palveluiden vertailu."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="06/06/2016"
 ms.author="elioda"/>

# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Azure IoT-toiminnosta ja Azure tapahtuman keskittimet vertailu

Yksi lähinnä palvelupyynnöt IoT-toiminnossa on telemetriatietojen kerääminen laitteet. Tästä syystä IoT keskittimeen verrataan usein [Azure tapahtuman][]porttiin. Tapahtuman keskittimet on IoT keskittimeen, kuten tapahtuman käsittelyn palvelun kautta pienen viive ja suuri luotettavuutta tapahtuma- ja telemetriatietojen tunkeutumisen valtaviin tasolla pilveen.

Palvelujen on kuitenkin useita eroja, joka on kuvattu seuraavassa taulukossa.

| Alue | IoT-toiminnossa | Tapahtuman keskittimet |
| ---- | ------- | ---------- |
| Viestintä kuviot | Ottaa käyttöön laitteen pilven ja pilven laitteen viesti. | Ottaa käyttöön vain tapahtuman tunkeutumisen (yleensä pidetä laitteen pilven skenaarioissa). |
| Protokollan laitetuki | Tukee MQTT, AMQP, AMQP WebSockets ja HTTP kautta. Lisäksi IoT keskittimeen toimii [Azure IoT protokolla yhdyskäytävän][lnk-azure-protocol-gateway], mukautettavissa protokolla-yhdyskäytävän käyttöönoton tukemaan mukautettuja protokollia. | Tukee AMQP, AMQP WebSockets ja HTTP. |
| Tietoturva | Sisältää laitteen kohti käyttäjätieto- ja voi kumota pääsyn hallinta. Katso [IoT keskittimeen kehittäjän opas suojaus-osa]. | Sisältää tapahtuman keskittimet laajuinen [jaettuun käyttöön käytännöt][Event Hubs - security], kautta [Publisherin käytännöt]tukevat rajoitettu kumottujen varmenteiden[Event Hubs publisher policies]. IoT ratkaisuja tarvitaan usein toteuttamisesta mukautetun ratkaisun tukemaan laitteen kohti tunnistetiedot ja anti väärentää toimenpiteitä. |
| Toimintojen seuranta | Mahdollistaa IoT ratkaisuja laitteen käyttäjätietojen hallinta- ja yhteysongelmien tapahtumia, kuten yksittäisen laitteen todennus virheet, rajoittaminen ja virheellisiä poikkeukset on runsaasti tilaa. Näitä tapahtumia, joiden avulla voit helposti tunnistaa yhteysongelmien yksittäisen laitteen tasolla. | Paljastaa kooste arvot. |
| Asteikko | On optimoitu tuki miljoonia samanaikaisesti liitettyjen laitteiden välillä. | Tukee samanaikaista--enintään 5 000 AMQP yhteydet [Azure palvelun Bus kiintiön][]mukaan Lisää rajoitettu määrä. Toisaalta tapahtuman keskittimet avulla voit määrittää osio on lähetetty viesti. |
| Laitteen SDK: T | Sisältää [laitteen SDK: T] [ Azure IoT Hub SDKs] on laaja valikoima alustojen ja eri kielten osalta. | Tuettu .NET ja c-näppäinyhdistelmää. Sisältää myös AMQP ja HTTP Lähetä liittymät. |
| Tiedoston lataaminen | Mahdollistaa IoT ratkaisuja Lataa tiedostot pilvipalveluun laitteilla. Sisältää tiedosto-ilmoituksen päätepisteen työnkulun yhdistäminen ja toimintojen seuranta virheenkorjaus tuki-luokka. | Käyttää ryhmän valintaruutu kuvion pyytää tiedostoja laitteilla ja antaa laitteiden tallennustilan näppäintä tapahtuman manuaalisesti. |

Yhteenvedossa, vaikka vain Käyttötapaus laitteen cloud telemetriatietojen tunkeutumisen IoT toiminnosta on palvelu, joka on suunniteltu IoT laitteen yhteyksiä varten. Laajenna arvo propositioita pysyisi näissä tilanteessa IoT kielikohtaiset ominaisuuksilla säilyvät. Tapahtuman keskittimet on suunniteltu tapahtuman tunkeutumisen valtaviin mittakaava, sekä muun palvelinkeskuksen ja sisäisessä palvelinkeskuksen skenaariot kontekstissa.

Ei ole epätavallisten IoT keskittimeen ja tapahtuman keskittimet samaan ratkaisuun--missä IoT keskittimeen käsittelee laitteen cloud viestintä ja tapahtuman keskittimet käsittelee myöhemmässä vaiheessa tapahtuman tunkeutumisen kyselyjä reaaliaikainen käsittely-ohjelmien käyttäminen.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja käyttöönoton IoT toiminnosta on artikkelissa [skaalaus, HA ja DR][lnk-scaling].

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Sovelluskehittäjän opas][lnk-devguide]
- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway]

[Azure tapahtuman keskittimet]: ../event-hubs/event-hubs-what-is-event-hubs.md
[IoT keskittimeen kehittäjän opas suojaus-osa]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure palvelun Bus-kiintiön]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
