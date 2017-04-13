<properties
 pageTitle="Hanki työkalut (Ubuntu 16.04) | Microsoft Azure"
 description="Lataa ja asenna tarvittavat työkalut ja että pii sovelluksen ensimmäisen malli-ohjelmiston Ubuntu."
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

# <a name="12-get-the-tools-ubuntu-1604"></a>1.2 saada työkaluja (Ubuntu 16.04)

> [AZURE.SELECTOR]
- [Windows 7: n +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [Mac OS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 mitä hyötyä

Lataa Kehitystyökalut ja vadelma pii-3 sovelluksen ensimmäisen malli-ohjelmiston. Jos täytät ilmenee ongelmia, Etsi ratkaisuja [sivun vianmääritys](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="122-what-you-will-learn"></a>1.2.2 mitä opit

Tässä osassa opit seuraavat asiat:

- Git ja Node.js asentaminen
  - [Git](https://git-scm.com) on jaettu Avaa lähde versio ohjausobjektin järjestelmä. Tämä Harjoitus otoksen hakeminen on tallennettu Git.
  - [Node.js](https://nodejs.org/en/) on JavaScript-runtime monipuolisia paketin ekosysteemiin kanssa.
- Voit asentaa muita Node.js Kehitystyökalut NPM avulla.
  - Pienin sallittu Node.js versio on 4,5 l..
  - [NPM](https://www.npmjs.com) on yksi Node.js package esimiehen

## <a name="123-what-do-you-need"></a>1.2.3 mitä on hyvä

- Internet-yhteyttä Kehitystyökalut ja ohjelmiston lataaminen
- Tietokone, joka toimii Ubuntu 16.04 tai uudempi versio 

## <a name="124-install-git-nodejs-and-npm"></a>1.2.4 Git, Node.js ja NPM asentamiseen

Käytä näppäinyhdistelmää `Ctrl + Alt + T` Avaa pääte-ikkuna ja suorittamalla seuraavat komennot:

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 asentaa muita Node.js Kehitystyökalut

[Gulp.js](http://gulpjs.com) avulla voit automatisoida oman pii otoksen sovelluksen käyttöönotto. [Laitteen etsiminen-cli](https://github.com/Azure/device-discovery-cli) käyttää myös IoT laitteidesi verkon tietojen hakemiseen.

Asenna `gulp` ja `device-discovery-cli` suorittamalla pääte-ikkunassa seuraava komento:

```bash
sudo npm install -g device-discovery-cli gulp
```

Jos kohtaat ongelmia asentamisessa Ubuntu Node.js ja lisää nämä Kehitystyökalut, on artikkelissa [vianmääritysoppaan](iot-hub-raspberry-pi-kit-node-troubleshooting.md) ratkaisu yleisimpiin ongelmiin.

## <a name="126-install-visual-studio-code"></a>1.2.6 Asenna Visual Studio-koodin.

[Lataa](https://code.visualstudio.com/docs/setup/linux) ja asenna Visual Studio-koodin. Visual Studio-koodi on kevyt, mutta tehokas lähde-koodi-editori, Windows, Linux ja Mac OS. Tämän editorin käyttäminen opetusohjelman myöhemmin-mallikoodin muokkaaminen.

## <a name="127-summary"></a>1.2.7 yhteenveto

Olet asentanut tarvittavat Kehitystyökalut ja sovelluksen ensimmäisen malli-ohjelmiston. Seuraavassa osassa luominen, ottaminen käyttöön ja suorita oman pii sovelluksen malli.

## <a name="next-steps"></a>Seuraavat vaiheet

[1.3 luoda ja ottaa käyttöön sovelluksen vilkunta malli](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)