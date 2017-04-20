> [AZURE.SELECTOR]
- [C Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
- [C Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
- [C-mbed](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
- [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)

## <a name="scenario-overview"></a>Skenaarion yhteenveto

Tässä tilanteessa voit luoda laite, joka lähettää seuraavat kaukomittaus seuranta [ratkaisu valmiiksi]kauko[lnk-what-are-preconfig-solutions]:

- Ulkoilman lämpötila
- Sisälämpötila
- Kosteus

Yksinkertaisuuden vuoksi laitteen koodi luo malliarvoja, mutta Kehotamme laajentaa real anturit muodostettaessa yhteyttä laitteeseesi ja todellinen kaukomittaus lähettämällä näyte.

Valmis tämän opetusohjelman, tarvitset Azure aktiivinen tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili-muutaman minuutin. Lisätietoja [Azure maksuton][lnk-free-trial].

## <a name="before-you-start"></a>Ennen kuin aloitat

Ennen laitteen kirjoittaa koodia, remote valmiiksi seuranta ratkaisu valmistella ja valmistella sitten kyseisen ratkaisun uuden mukautetun laitteen.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Tarjotaan valmiiksi etätietokoneen seuranta ratkaisu

Tässä opetusohjelmassa voit luoda laite lähettää tietoja [etätietokoneessa valvonnan] esiintymä[ lnk-remote-monitoring] ratkaisu valmiiksi. Jos et ole jo valmisteltu etäyhteyden valmiiksi seuranta ratkaisu Azure tilisi, tee seuraavat toimet:

1. Valitse <https://www.azureiotsuite.com/> -sivulla **+** voit luoda uuden ratkaisun.

2. Valitse **Valitse** paneelin **etätietokoneen valvonta** Luo uuden ratkaisun.

3. Sivulla **Remote luoda seuranta ratkaisu** valintasi **ratkaisun nimi** **alue** , jonka haluat ottaa käyttöön ja valitse haluamasi Azure-tilaus. Valitse sitten **Luo ratkaisu**.

4. Odota, kunnes valmisteluprosessi on valmis.

> [AZURE.WARNING] Esimääritettyjä ratkaisuja laskutettavat Azure-palveluiden käyttämisen. Muista poistaa tilauksesi valmiiksi ratkaisun, kun on valmis, jotta vältetään tarpeettomat kulut. Voit poistaa ennalta määritettyjä ratkaisu kokonaan tilauksesi käy <https://www.azureiotsuite.com/> -sivulla.

Etätietokoneen seuranta ratkaisu valmisteluprosessi on valmis, kun valitse ratkaisun dashboard avataan selaimen **käynnistäminen** .

![][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Valmistele laite etätietokoneen seuranta ratkaisu

> [AZURE.NOTE] Jos laite on valmisteltu jo-ratkaisu, voit ohittaa tämän vaiheen. Tarvitset tietää laitteen tunnistetietoja sovelluksen luonnin yhteydessä.

Laite muodostaa ennalta määritettyjä ratkaisu se on tunnistettava IoT keskitin kelvollisin valtuuksin. Laitteen tunnistetiedot voi hakea ratkaisua dashboard. Laitteen tunnistetiedot sisällyttää asiakkaan sovelluksen myöhemmin tässä opetusohjelmassa. 

Jos haluat lisätä uuden laitteen etätietokoneen seuranta ratkaisu, seuraavien vaiheiden ratkaisun koontinäytön:

1.  Valitse **Lisää laite**Raporttinäkymät-ikkunan vasemmassa alakulmassa.

    ![][1]

2.  Paneelin **Mukautetun laitteen** valitsemalla **Lisää uusi**.

    ![][2]

3.  **Haluan määrittää itse Laitetunnus**, laitteen-tunnus, kuten **mydevice**, valitse **Tarkista tunnus** , varmista, että nimi ei ole vielä käytössä, ja valitse sitten **Luo** valmistelu laite.

    ![][3]

5. Laitteen tunnistetiedot (Laitetunnus Hostname IoT keskittimeen ja laitteen avainta) muistiin, asiakassovelluksen tarvitsee niitä muodostaa etäyhteyden seuranta ratkaisu. Valitse sitten **Valmis**.

    ![][4]

6. Varmista, että laite näyttää laitteet-osassa. Laitteen tilana on **odottaa** , kunnes laite muodostaa yhteyden etätietokoneen seuranta ratkaisu.

    ![][5]

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png
[5]: ./media/iot-suite-selector-connecting/suite5.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/