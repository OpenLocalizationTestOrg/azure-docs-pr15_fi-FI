> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)

## <a name="introduction"></a>Johdanto

Laitteen twins on otettava huomioon JSON-tiedostot, joihin voidaan tallentaa laitteen tiedot (metatiedot, määritykset ja ehdot). IoT keskittimeen jatkuu laite-sekä kunkin laitteen, johon muodostat yhteyden IoT keskittimeen.

Laitteen twins avulla:

* Tallentaa laitteen metatiedon takaisin päästä.
* Raportin nykyisen tilan tiedot, kuten käytettävissä olevat ominaisuudet ja ehdot (esimerkiksi connectivity käytetty menetelmä) laitteen-sovellukset.
* Synkronoida (esimerkiksi laitteisto- ja määritystietoja päivitykset) pitkään käynnissä olevien työnkulkujen tilan laite-sovellus ja uudelleen.
* Kyselyn laitteen metatiedon, määrittäminen tai tila.

> [AZURE.NOTE] Laitteen twins on suunniteltu synkronointi ja kyselyt laitteen määrityksiä ja ehtoja. Käytetään [laitteen cloud viestejä] [ lnk-d2c] jaksojen aikaleimattu tapahtuma (esimerkiksi telemetriatietojen virtaa aikapohjaisten tunnistimen tiedot)-ja [cloud laitteen menetelmiä] [ lnk-methods] laitteita, kuten tuuletin käyttäjän hallita-sovelluksen ottaminen vuorovaikutteisista ohjausobjekteista varten.

Laitteen twins IoT-toiminnossa on tallennettu, ja se sisältää:

* *tunnisteet*-laitteen metatiedon voi käyttää vain takaisin loppuun;
* *haluamasi ominaisuudet*, JSON objektit uudelleen ja havaittavissa laitteen; sovelluksessa muokattavissa ja
* *raportoidun ominaisuudet*-JSON objektit millä laite-sovellus ja lukea takana lopussa. Tunnisteet ja ominaisuudet eivät saa sisältää taulukoita, mutta objektit voivat olla ylemmän tason.

![][img-twin]

Sovelluksen takaisin loppuun voit lisäksi kyselyn laitteen twins edellä mainittujen tietojen perusteella.
Viitata [ymmärtäminen laitteen twins] [ lnk-twins] Saat lisätietoja laitteen twins ja [IoT keskittimeen kyselykielen] [ lnk-query] viittaus kyselyt.

> [AZURE.NOTE] Tällä hetkellä voi käyttää vain IoT keskittimeen MQTT-protokollan avulla yhteyden muodostavien laitteiden laitteen twins. Lue lisätietoja [MQTT tukevat] [ lnk-devguide-mqtt] artikkelista ohjeita siitä, miten voit muuntaa olemassa olevan laitteen-sovelluksen käyttämään MQTT.

Tässä opetusohjelmassa näytetään, miten voit:

- Luo taustatietokantaan-sovellusta, joka lisää laitteen sekä ja luo Simuloitu laitteen, raportoi yhteyden sen kanavan kuin *ilmoitettu ominaisuuden* laitteen sekä *tunnisteita* .
- Kyselyn uudelleen sovelluksestasi suodattimien käyttäminen tunnisteet ja ominaisuuksista, jotka on aiemmin luotu laitteet.


<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md