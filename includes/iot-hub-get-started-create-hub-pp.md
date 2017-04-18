## <a name="create-a-device-management-enabled-iot-hub"></a>Luo laitteen hallinta käytössä IoT-toiminnossa

Esikatselu on IoT keskittimeen hallinta, on luotava laitteen hallinta käytössä IoT-toiminnossa. Kun IoT keskittimeen Laitehallinnan yleiseen käyttöön, tässä opetusohjelmassa päivitetään. Seuraavia ohjeita noudattamalla voit suorittaa tämän tehtävän Azure-portaalissa.

1.  Kirjautuminen [Azure portal].
2.  Jumpbar, valitse **Uusi**ja valitse sitten **Asioita Internet**ja valitse sitten **Azure IoT toiminnossa**.

    ![][img-new-hub]

3.  Valitse **IoT keskittimeen** , sivu IoT-toiminnossa asetukset.

    ![][img-configure-hub]

  -   Kirjoita **nimi** -ruutuun nimi IoT-toiminnossa. Jos **nimi** on voimassa ja käytettävissä olevien, vihreä valintamerkki näkyy **nimi** -ruutuun.
  -   Valitse **hinnoittelu ja skaalausta taso**. Tässä opetusohjelmassa ei edellytä tietyn taso.
  -   **Resurssiryhmä**-Luo uusi resurssiryhmä tai valitse olemassa. Lisätietoja on artikkelissa [Azure resurssien käyttäminen resurssiryhmät].
  -   **Ota hallinta - ESIKATSELU**ruutu.
  -   **Sijainti**Valitse sijainti, johon haluat isännöidä IoT-toiminnossa. IoT keskittimeen hallinta on käytettävissä vain Yhdysvaltojen Itä, Pohjois Europe ja Itä-Aasian julkisen esikatselun aikana.

    > [AZURE.NOTE]Jos valitset Älä valintaruudun **käyttöön**Laitehallinnan-mallit eivät toimi.<br/>Valitsemalla **Ota hallinta**voit luoda IoT tuetaan vain Yhdysvaltojen Itä, Pohjois Europe ja Itä-Aasian ja tuotannon tilanteita, joissa ei ole tarkoitettu esikatselu. Ei voi siirtää laitteet ja solun laitteen hallinta käytössä keskittimet ulos.

4.  Kun olet valinnut IoT-toiminnossa asetukset, valitse **Luo**. Voi kestää muutaman minuutin Azure luominen IoT-toiminnossa. Tarkista tila, voit valvoa edistymistä **Startboard** tai **ilmoitukset** -ruutu.

    ![][img-monitor]

5.  Kun IoT-toiminnossa on luotu, että keskittimeen, sivu avautuu automaattisesti. Pane merkille **isäntänimi**ja valitse sitten **jaettu käytön käytännöt**.

    ![][img-keys]

6.  Valitse **iothubowner** -käytäntö ja valitse kopioi ja **iothubowner** sivu yhteysmerkkijonon mieleen. Kopioi se uuteen paikkaan, voit käyttää myöhemmin sillä tarvitset sitä tässä opetusohjelmassa suorittamiseen.

    > [AZURE.NOTE] Tuotannon tilanteissa Varmista, että et **iothubowner** -tunnistetiedoilla.

    ![][img-connection]

Olet nyt luonut laitteen hallinta käytössä IoT toiminnossa. Sinun on suoritettava tässä opetusohjelmassa yhteysmerkkijonon.

## <a name="create-a-device-identity"></a>Laitteen käyttäjätietojen luominen

Tässä osassa käytät kutsutaan [IoT keskittimeen Explorer] solmu-työkalua[ iot-hub-explorer] laitteen käyttäjätietojen luominen opetusohjelmassa.

1. Suorita komentorivin ympäristön seuraavasti:

    npm Asenna -giothub-explorer@latest

2. Suorita seuraava komento keskittimeen, voit korvata muistaminen Kirjaudu `{service connection string}` kopioimasi IoT keskittimeen yhteysmerkkijonolla:

    Kirjautuminen iothub explorer "{palvelun yhteysmerkkijonon}"

3. Lopuksi kutsutaan uusien laitteen käyttäjätietojen luominen `myDeviceId` -komennolla:

    iothub explorer luominen myDeviceId--yhteysmerkkijono

Pane merkille laitteen yhteysmerkkijonoa tulos. Laitteen sovellus käyttää tämän yhteysmerkkijonon IoT-toiminnossa kuin laitteen yhdistäminen.

![][img-identity]

Viitata [aloittaminen IoT keskittimeen] [ lnk-getstarted] tavan luoda laitteen käyttäjätietoja ohjelmallisesti.

<!-- images and links -->
[img-new-hub]: media/iot-hub-get-started-create-hub-pp/image1.png
[img-configure-hub]: media/iot-hub-get-started-create-hub-pp/image2.png
[img-monitor]: media/iot-hub-get-started-create-hub-pp/image3.png
[img-keys]: media/iot-hub-get-started-create-hub-pp/image4.png
[img-connection]: media/iot-hub-get-started-create-hub-pp/image5.png
[img-identity]: media/iot-hub-get-started-create-hub-pp/devidentity.png

[Azure portal]: https://portal.azure.com/
[iot-hub-explorer]: https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
[Resurssiryhmien avulla voit hallita Azure resursseja]: ../articles/azure-portal/resource-group-portal.md
