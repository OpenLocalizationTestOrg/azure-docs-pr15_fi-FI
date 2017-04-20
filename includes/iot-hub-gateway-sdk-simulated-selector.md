> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)

Tämän vaihekuvauksen, [simuloituja laitteen Cloud Lataa näyte] kerrotaan miten [Azure IoT Gateway SDK] [ lnk-sdk] lähettää laitteen cloud kaukomittaus IoT keskittimeen Simuloitu laitteista.

Tässä vaihekuvauksessa käsitellään:

1. **Arkkitehtuuri**: Simuloitu laitteen Cloud Lataa näyte arkkitehtuurin tärkeitä tietoja.

2. **Koontiversio ja suorita**: vaiheet, jotka tarvitaan rakentamiseen ja mallin suorittamiseen.

## <a name="architecture"></a>Arkkitehtuuri

Simuloitu laitteen Cloud Lataa näyte on gateway, joka lähettää kaukomittaus Simuloitu laitteet keskittimeen IoT luoda SDK: N avulla. Simuloitu laitteita ei voi muodostaa yhteyttä suoraan IoT keskitin koska:

- Laitteet eivät käytä IoT keskitin ymmärtämällä protokolla.
- Laitteet eivät ole smart muistaa huolehtii IoT keskitin tunnistetiedot.

Yhdyskäytävän ratkaisee nämä ongelmat Simuloitu laitteiden seuraavilla tavoilla:

- Yhdyskäytävän ymmärtää Simuloitu laitteiden käyttämä protokolla laitteet laitteen cloud kaukomittaus vastaanottaa ja välittää nämä viestit IoT Hub-keskittimen ymmärtämällä protokollalla.
- Yhdyskäytävä tallentaa IoT keskitin käyttäjätietojen Simuloitu laitteiden puolesta ja toimii välityspalvelimena kun Simuloitu laitteiden lähettää viestejä IoT keskittimeen.

Seuraavassa kaaviossa esitetään pääkomponentit näytteen gateway-moduulit:

![][1]


> [AZURE.NOTE] Moduulien Älä välitä viestejä suoraan toisilleen. Moduulien julkaista viestejä sisäinen välittäjä, joka toimittaa viestit muihin moduuleihin, käyttämällä tilaus alla kaaviossa esitetyllä tavalla. Lisätietoja [IoT Gateway SDK: N käytön aloittaminen][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Nieltynä moduulissa

Tämä moduuli on lähtökohta tietonsa saavasta laitteet-yhdyskäytävän kautta- ja pilven. Näytteen moduuli suorittaa neljä tehtävät:

1.  Se luo Simuloitu lämpötilan lukemat.
    
    Huomautus: Jos käytät todellista laitteet, moduulin lukea näitä fyysisiä laitteita.

2.  Se sijoittaa sanoman sisällön yhdeksi Simuloitu lämpötilan lukemat.

3.  Simuloitu lämpötilan tiedot sisältävä viesti Lisää ominaisuus väärennetyn MAC-osoite.

4.  Se mahdollistaa viestin seuraavan moduulin ketjun.

> [AZURE.NOTE] Moduuli nimeltään **protokolla X nieltynä** yllä olevan kaavion kutsutaan **laitteen Simuloitu** lähdekoodi.

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt; - &gt; moduuli IoT keskitin tunnus

Tämä moduuli etsii viestejä, jotka sisältävät ominaisuuden, joka sisältää MAC-osoitteen protokollan nieltynä moduuli, Simuloitu laitteen lisäämistä. Jos moduuli löytää tällainen ominaisuus, se lisää toisen ominaisuuden kanssa IoT Hub laitteen avainta viestin ja sitten vapauttaa viestin seuraavan moduulin ketjun. Tämä on kuinka näyte kytkee IoT Hub laite käyttäjätietojen Simuloitu laitteiden kanssa. Kehittäjä määrittää MAC-osoitteet ja IoT keskitin käyttäjätietojen yhdistämisen manuaalisesti moduulin kokoonpanon osana. 

> [AZURE.NOTE]  Tämä esimerkki käyttää MAC-osoite laitteen yksilöllinen tunniste ja korreloi IoT Hub-hallintalaite. Voit kuitenkin kirjoittaa oman moduulin, joka käyttää eri yksilöllinen. Voi olla esimerkiksi laitteet, joissa on yksilöllinen sarjanumero tai kaukomittaus tiedot, jotka on upotettu, voit käyttää selvittäminen IoT Hub-laitteen nimi laitteelle.

### <a name="iot-hub-communication-module"></a>IoT keskitin viestintä-moduuli

Tässä moduulissa IoT-keskittimen kanssa kestää myöntämä aiemman-moduulin hallintalaite viestit ja lähettää viestin sisältöä IoT keskittimeen käyttämällä HTTP. HTTP on yksi ymmärtää IoT keskitin kolmea protokollaa.

Sen sijaan, että IoT keskitin yhteys Simuloitu kullekin laitteelle, Tässä moduulissa IoT keskittimeen yhdyskäytävä Avaa yhden HTTP-yhteyden ja multiplexes yhteyksiä Simuloitu laitteita tämän yhteyden kautta. Näin monet enemmän laitteet, Simuloitu tai muulla tavalla, kuin olisi mahdollista, jos se avata yksilöllisiä kullekin laitteelle yhteyden muodostaminen yhtä yhdyskäytävää.

![][2]


<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[Simuloitu laitteen Cloud Lataa näyte]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/sample_simulated_device_cloud_upload.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md