## <a name="view-device-telemetry-in-the-dashboard"></a>Näytä laitteen kaukomittaus koontinäytön

Etätietokoneen seuranta ratkaisu-Raporttinäkymät-ikkunan avulla voit tarkastella kaukomittaus, jotka lähettävät laitteet keskittimen IoT.

1. Selaimessa palaa remote seuranta ratkaisu dashboard, valitse **laitteet** - **laitteet-luettelosta**siirtymällä vasemmassa paneelissa.

2. Pitäisi näkyä **laitteet-luettelosta**, että laitteen tila on nyt **käytössä**.

    ![][18]

3. Valitse koontinäytön palauttaa, valitse laite **laite näkymä** avattavasta sen kaukomittaus **Dashboard** . -Esimerkkisovelluksen kaukomittaus on 50 yksikön sisäinen lämpötila 55 yksikköä, kun ulkoilman lämpötila ja kosteus 50 yksiköt. Huomaa, että oletusarvon mukaan koontinäytön näkyy vain lämpötilan ja kosteuden arvoja.

    ![][img-telemetry]

## <a name="send-a-command-to-your-device"></a>Komennon lähettäminen laitteeseen

Dashboardin remote seuranta ratkaisu-voit lähettää komentoja laitteiden IoT keskittimen kautta. Etätietokoneen seuranta ratkaisu voi esimerkiksi lähettää komento asettaa laitteen sisäisen lämpötilan.

1. Valitse Etäkäytön seuranta ratkaisu, dashboard **laitteet** **laitteet-luettelosta**siirtymällä vasemmassa paneelissa.

2. Valitse **laitteet-luettelosta**laite **Laitteen tunnus** .

3. **Laitteen tiedot** -paneelissa Valitse **Komennot**.

    ![][13]

4. Valitse **komento** avattavasta **SetTemperature**ja kirjoittamalla uuden arvon lämpötila **lämpötila** . Valitsemalla **Lähetä komento** komennon laitteelle.

    ![][14]

    > [AZURE.NOTE] Historia komento näyttää aluksi komennon tila **Odottava**. Kun laite Kuittaa komento, tila muuttuu **onnistumisen**kannalta.

5. Koontinäyttö Varmista, että laite on nyt lähettää 75 arvona lämpötila.

## <a name="next-steps"></a>Seuraavat vaiheet

[Mukauttaminen ratkaisuja valmiiksi] artikkelin[ lnk-customize] kuvataan muutamia tapoja voit laajentaa tämän mallin. Voi kuulua avulla todellista anturit ja toteuttaa muita komentoja.

Saat lisätietoja [azureiotsuite.com-sivuston käyttöoikeuksia][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
