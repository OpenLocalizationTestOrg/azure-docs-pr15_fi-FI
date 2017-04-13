<properties
 pageTitle="Laitteen määrittäminen | Microsoft Azure"
 description="Vadelma pii-3 ensimmäisen kerran käytettäväksi määrittäminen ja asentaminen Raspbian-Käyttöjärjestelmää, vapaa käyttöjärjestelmille, joita on tarkoitettu vadelma pii-laite."
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

# <a name="11-configure-your-device"></a>1.1 määrittäminen laitteeseen

## <a name="111-what-you-will-do"></a>1.1.1 mitä hyötyä

Oman pii ensimmäisen kerran käytön määrittäminen ja asentaminen Raspbian-käyttöjärjestelmän vapaa käyttöjärjestelmille, joita on tarkoitettu vadelma pii-laite. Jos täytät ilmenee ongelmia, Etsi ratkaisuja [sivun vianmääritys](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="112-what-you-will-learn"></a>1.1.2 mitä opit

Tässä osassa opit seuraavat asiat:

- Miten voit asentaa oman pii Raspbian
- Miten power määrittäminen oman pii USB-kaapelilla
- Oman pii yhdistämisestä Ethernet-kaapelin tai Wi-Fi-verkkoon
- Voit lisätä LED breadboard ja muodostaa yhteyttä pii

## <a name="113-what-you-need"></a>1.1.3 mitä tarvitset

Tässä osassa suorittamiseen tarvitset vadelma pii 3 Starter Kit-seuraavat osat:

- Taulun vadelma pii 3
- 16 gt MicroSD kortti
- 5V 2 a power toimitettava kuusi jalan micro USB-kaapeli
- Breadboard
- Yhdistimen verkkokaapeleita
- 560 ohmia vastus
- Utuiset 10mm LED
- Ethernet-kaapelin

![Starter Kit kohteita](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Tarvitset myös:

- Muodostaa yhteyttä pii langallisen tai langattoman yhteyden
- USB-SD sovittimen tai tarkasteleminen SD ‑tuotetunnuskortin OS kuvan tallentaminen MicroSD korttiin.
- Tietokone, jossa Windows, Mac tai Linux. Tietokoneen käytetään Raspbian asentaminen MicroSD kortin.
- Internet-yhteys tarvittavat työkalut ja ohjelmiston lataaminen

## <a name="114-install-raspbian-on-the-microsd-card"></a>1.1.4 asentaminen Raspbian MicroSD kortti

Valmistele MicroSD kortin kirjoittaa Raspbian kuva.

1. Lataa Raspbian.
  1. [Lataa](https://www.raspberrypi.org/downloads/raspbian/) Raspbian Jessie kanssa kuvapisteen zip-tiedosto.
  2. Pura Raspbian kuvan tietokoneesi kansiosta.
2. Asenna Raspbian MicroSD korttiin.
  1. [Lataa](https://www.etcher.io) ja asenna Etcher SD kortin asema-apuohjelma.
  2. Suorita Etcher ja valitse Raspbian kuvan, joka purit vaiheessa 1.
  3. Valitse MicroSD kortti-asema.
    Huomautus: Etcher ehkä jo valinnut oikean aseman.
  4. Valitse Flash asentamiseksi Raspbian MicroSD korttiin.
  5. Poista MicroSD kortti tietokoneesta, kun se on valmis.
    Huomautus: Se on turvallista poistaa MicroSD kortin suoraan, koska Etcher automaattisesti poistaa tai irrottaa valmistumisen MicroSD kortti.
  6. MicroSD-kortin lisääminen oman pii.

![Lisää kortti SD](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="115-power-on-your-pi"></a>1.1.5 oman pii virta

Oman piillä micro USB-kaapeli ja power toimittamista Power.

![Virta](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [AZURE.NOTE] On tärkeää avulla power Kit, joka on vähintään 2 a varmistaaksesi, että vadelma syötetään tarpeeksi power toimii oikein.

## <a name="116-connect-your-raspberry-pi-3-to-the-network"></a>1.1.6 Muodosta yhteys verkkoon vadelma pii-3

Voit muodostaa yhteyttä pii kiinteää verkkoa tai langattomaan verkkoon. Varmista, että samassa verkossa tietokoneesi on yhteydessä oman pii. Esimerkiksi voit muodostaa yhteyttä pii saman valitsin, joka tietokone on yhteydessä.

### <a name="1161-connect-to-a-wired-network"></a>1.1.6.1 yhdistäminen kiinteää verkkoa

Muodostaa yhteyttä pii kiinteää verkkoa Ethernet-kaapelin avulla. Kaksi merkkivalot oman piillä ottaminen käyttöön, jos yhteys on muodostettu.

![Muodostaa yhteyden Ethernet-kaapelin](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="1162-connect-to-a-wireless-network"></a>1.1.6.2 muodostaa langattomaan verkkoon

Noudata [näyttöön tulevia ohjeita](https://www.raspberrypi.org/learning/software-guide/wifi/) muodostaa yhteyttä pii langattoman verkon vadelma pii-Foundation. Nämä ohjeet edellyttää muodostaa ensin oman pii näyttö ja näppäimistö.

## <a name="117-connect-the-led-to-your-pi"></a>1.1.7 LED muodostaa yhteyttä pii

Tämän tehtävän suorittamiseen käyttää [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), yhdistimen verkkokaapeleita tai LED vastus. Ne muodostaa yhteyttä piin [Yleinen i](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO)-portit. 

![Breadboard LED ja vastus](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Yhdistä LED lyhentää jalan **GPIO tulo GND (Pin-6)**.
2. Muodostaa yhden jalan vastuksen LED pidempi jalan.
3. Yhdistä muut jalan vastuksen **GPIO 4 (PIN-tunnuksen 7)**.

Huomaa, että LED polariteetti on tärkeää. Aktiivinen pieni yleisesti nimeltään polariteetti-asetusta.

![Piikkijärjestyksen](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Congratulation! Olet määrittänyt onnistuneesti oman pii.

## <a name="118-summary"></a>1.1.8 yhteenveto

Tässä osassa oppinut määrittäminen oman pii asentamalla Raspbian, että pii muodostaa verkon ja LED muodostaa yhteyttä pii. Huomaa, että LED ei ole vielä Vaalea ylöspäin. Seuraavassa osassa Asenna tarvittavat työkalut ja ohjelmiston valmistelussa sovelluksen malli käytössä oman pii.

![Laite on valmis](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Seuraavat vaiheet

[1.2 Hanki työkalut](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
