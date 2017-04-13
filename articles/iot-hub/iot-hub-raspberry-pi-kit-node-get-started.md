<properties
 pageTitle="Aloita Vadelman pii 3 | Microsoft Azure"
 description="Aloittaminen vadelma pii 3, luoda Azure IoT-toiminnosta ja muodostaa yhteyttä pii IoT-toiminnossa"
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="get-started-with-raspberry-pi-3"></a>Vadelman pii 3 käytön aloittaminen

Tässä opetusohjelmassa aloitetaan mukaan Koulujen käsitteleminen vadelma pii 3 kyseisen käynnissä Raspbian perusteet. Valitse Lue laitteet yhdistämisestä kanssa [Azure IoT keskittimeen](iot-hub-what-is-iot-hub.md)pilvipalveluun saumattomasti. Käy Windows 10 IoT Core näytteiden [windowsondevices.com](http://www.windowsondevices.com/).

## <a name="lesson-1-configure-your-device"></a>Harjoitus 1: Määritä laitteen

![Harjoitus 1 E2E kaavio](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

Tämä Harjoitus ja käyttöjärjestelmän vadelma pii 3-laitteen määrittäminen, kehitysympäristön määritys ja pii-sovelluksen ottaminen käyttöön.

### <a name="configure-your-device"></a>Laitteen määrittäminen

Määritä vadelma pii-3, ensimmäinen käyttökerta ja asenna Raspbian-Käyttöjärjestelmää, vapaa käyttöjärjestelmille, joita on tarkoitettu vadelma pii-laite.

*Arvioitu kesto: puolessa tunnissa* 

[Siirry kohtaan "Määritä laitteen"](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)

### <a name="get-the-tools"></a>Hanki työkalut
Lataa työkalut ja ohjelmiston voivat laatia ja kehittää ensimmäisen sovelluksen vadelma pii 3.

*Arvioitu kesto: 20 minuutin ajan* 

[Siirry "Hanki työkalut"](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

### <a name="create-and-deploy-the-blink-application"></a>Luoda ja ottaa käyttöön blink-sovellus

Kloonaa otoksen Node.js sovelluksen Github ja ottaa käyttöön sovelluksen vadelma pii 3 tauluun gulp. Tämän esimerkkisovelluksen vilkkuu LED yhteydessä tauluun kahden sekunnin välein.

*Arvioitu kesto: 5 minuuttia* 

[Siirry ' Luo ja ottaa käyttöön sovelluksen vilkunta "](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

## <a name="lesson-2-create-your-iot-hub"></a>Harjoitus 2: Luo IoT-toiminnossa

![Harjoitus 2 E2E kaavio](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

Tämä Harjoitus luot vapaa Azure-tili, valmistella Azure IoT-toiminnosta ja luo ensimmäinen laitteen Azure IoT toiminnossa.

Suorita Harjoitus 1, ennen kuin aloitat tämän Harjoitus.

### <a name="get-the-azure-tools"></a>Hanki Azure Työkalut

Asenna Azure käyttöliittymä (Azure CLI).

*Arvioitu kesto: 10 minuutin* 

[Siirry Azure Hae-Työkalut](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

### <a name="create-your-iot-hub-and-register-your-raspberry-pi-3"></a>Luo IoT-toiminnosta ja rekisteröi vadelma pii-3

Luoda resurssiryhmän valmistella ensimmäisen Azure IoT-toiminnossa ja ensimmäisen laitteen lisääminen käyttämällä Azure CLI Azure IoT-toiminnossa. 

*Arvioitu kesto: 10 minuutin* 

[Valitse Luo IoT-toiminnosta ja rekisteröi vadelma pii-3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)


## <a name="lesson-3-send-device-to-cloud-messages"></a>Harjoitus 3: Laitteen cloud viestien lähettäminen

![Harjoitus 3 E2E kaavio](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

Tämä oppitunnissa lähetät viestejä oman pii IoT keskittimeen. Voit myös luoda Azure funktio-sovellusta, joka vastaanottaa saapuvia viestejä IoT-toiminnosta ja kirjoittaa ne Azure-taulukkotallennus.

Suorita opitut 1 ja 2 ennen kuin aloitat tämän Harjoitus.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Azure funktion sovelluksen ja Azure-tallennustilan tilin luominen

Azure Resurssienhallinta-mallin avulla voit luoda sovelluksen Azure-funktio ja Azure-tallennustilan tilin.

*Arvioitu kesto: 10 minuutin* 

[Siirry "Luo Azure funktion sovelluksen ja Azure-tallennustilan tilin"](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

### <a name="run-sample-application-to-send-device-to-cloud-messages"></a>Suorita laitteen cloud viestien lähettämiseen sovelluksen malli

Ottaa käyttöön ja suorita sovelluksen malli, joka lähettää viestejä IoT keskittimeen vadelma pii 3 laitteeseen.

*Arvioitu kesto: 10 minuutin* 

[Siirry "Suorita laitteen cloud viestien lähettämiseen sovelluksen malli"](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

### <a name="read-messages-persisted-in-azure-storage"></a>Luetut viestit samanlainen Azuren tallennustilaan
Valvoa laitteen cloud-viestejä, kun ne on kirjoitettu Azure-tallennustilan.

*Arvioitu kesto: 5 minuuttia* 

[Siirry "luetut viestit samanlainen Azuren tallennustilaan"](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)


## <a name="lesson-4-send-cloud-to-device-messages"></a>Harjoitus 4: Cloud laitteen viestien lähettäminen

![Harjoitus 4 E2E kaavio](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

Tämä Harjoitus demos viestien lähettämisestä Azure IoT-toiminnosta vadelma pii-3. Viestien hallinta käyttöön tai poistaminen käytöstä, joka on liitetty oman pii LED toiminnan. Sovelluksen malli on valmis, voit saada tämän tehtävän.

Suorita 1, 2 ja 3 ennen tämän Harjoitus opitut.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Suorita cloud laitteen viestien sovelluksen malli

Harjoitus 4 malli-sovellus toimii oman pii ja valvoo saapuvien viestien IoT-toiminnosta. Uuden tehtävän gulp lähettää viestejä oman pii IoT-toiminnosta LED vilkkuu.

*Arvioitu kesto: 10 minuutin* 

[Siirry "Pilven laitteen viestien malli-sovelluksen käyttämiseen"](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Valinnainen osa: käyttöön tai käytöstä LED toiminnan muuttaminen

Voit mukauttaa viestin LED henkilön käyttöön ja poistaminen käytöstä toiminnan muuttaminen.

*Arvioitu kesto: 10 minuutin* 

[Siirry ' valinnaisen osan: Muuta käyttöön tai pois LED toimintaa "](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)


## <a name="troubleshooting"></a>Vianmääritys

Jos täytät minkä tahansa troubles kirjataan aikana, voit Etsi ratkaisuja tällä sivulla.

[Siirry vianmääritys"](iot-hub-raspberry-pi-kit-node-troubleshooting.md)
