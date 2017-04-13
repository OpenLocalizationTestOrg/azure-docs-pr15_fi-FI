## <a name="create-an-iot-hub"></a>Luo IoT-toiminnossa

Luo IoT-toiminnossa Simuloitu laitteen muodostaa yhteyden. Seuraamalla näitä ohjeita noudattamalla voit suorittaa tämän tehtävän Azure-portaalissa.

1. Kirjautuminen [Azure portal][lnk-portal].

2. Valitse Jumpbar, **Uusi** > **Asioita Internet** > **Azure IoT toiminnossa**.

    ![Azure-portaalin Jumpbar][1]

3. Valitse **IoT keskittimeen** , sivu IoT-toiminnossa asetukset.

    ![IoT keskittimeen sivu][2]

    * Kirjoita **nimi** -ruutuun nimi IoT-toiminnossa. Jos **nimi** on voimassa ja käytettävissä olevien, vihreä valintamerkki näkyy **nimi** -ruutuun.
    * Valitse [hinnat ja mittakaava taso][lnk-pricing]. Tässä opetusohjelmassa ei edellytä tietyn taso. Käytä tässä opetusohjelmassa vapaa F1 taso.
    * **Resurssiryhmä**-Luo uusi resurssiryhmä tai valitse olemassa. Lisätietoja on artikkelissa [Azure resurssien käyttäminen resurssiryhmät][lnk-resource-groups].
    * **Sijainti**Valitse sijainti, johon haluat isännöidä IoT-toiminnossa. Tässä opetusohjelmassa, valitse lähimpään tallennuspaikka.

4. Kun olet valinnut IoT-toiminnossa asetukset, valitse **Luo**.  Voi kestää muutaman minuutin Azure luominen IoT-toiminnossa. Tarkista tila, voit valvoa edistymistä Startboard tai ilmoitukset-ruutu.

    ![Uusi IoT keskittimeen tila][3]

5. Kun IoT-toiminnossa on luotu, valitsemalla Uusi ruudun IoT-toiminnossa Avaa sivu, uusi IoT pääsivusto Azure-portaalissa. Pane merkille **isäntänimi**ja valitse sitten **jaettu käytön käytännöt**.

    ![Uusi IoT keskittimeen sivu][4]

6. **Jaettu käytäntöjen** , sivu- **iothubowner** käytännön, ja kopioi ja **iothubowner** sivu yhteysmerkkijonon mieleen. Lisätietoja on artikkelissa [käyttöoikeuksien valvonta] [ lnk-access-control] "Azure IoT keskittimeen kehittäjän oppaassa."

    ![Jaettuun käyttöön käytännöt sivu][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
