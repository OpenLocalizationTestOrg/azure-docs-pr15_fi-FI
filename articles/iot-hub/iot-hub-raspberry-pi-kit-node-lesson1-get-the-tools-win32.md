<properties
 pageTitle="Hanki työkalut (Windows 7: n +) | Microsoft Azure"
 description="Lataa ja asenna tarvittavat työkalut ja että pii sovelluksen ensimmäisen malli-ohjelmiston Windows 7: ssä ja sitä uudemmissa versioissa."
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

# <a name="12-get-the-tools-windows-7-"></a>1.2 Hanki työkalut (Windows 7: n +) 

> [AZURE.SELECTOR]
- [Windows 7: n +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [Mac OS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 mitä hyötyä

Lataa Kehitystyökalut ja vadelma pii-3 sovelluksen ensimmäisen malli-ohjelmiston. Jos täytät ilmenee ongelmia, Etsi ratkaisuja [sivun vianmääritys](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="122-what-you-will-learn"></a>1.2.2 mitä opit
- Git ja Node.js asentaminen
  - [Git](https://git-scm.com) on jaettu Avaa lähde versio ohjausobjektin järjestelmä. Tämä Harjoitus otoksen hakeminen on tallennettu Git.
  - [Node.js](https://nodejs.org/en/) on JavaScript-runtime monipuolisia paketin ekosysteemiin kanssa.
- Voit asentaa muita Node.js Kehitystyökalut NPM avulla.
  - Node.js pienin versiovaatimusten on 4,5 l..
  - [NPM](https://www.npmjs.com) on Node.js paketin esimiehen.

## <a name="123-what-you-need"></a>1.2.3 mitä tarvitset

- Internet-yhteyttä Kehitystyökalut ja ohjelmiston lataaminen
- Tietokone, jossa on käynnissä Windows

## <a name="124-install-git-and-nodejs"></a>1.2.4 Git ja Node.js asentaminen

Valitse Lataa ja asenna Git ja Windows l Node.js alla olevia linkkejä.

- [Hae Git for Windowsissa](https://git-scm.com/download/win/)
- [Hanki Windows Node.js l.](https://nodejs.org/en/)

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 asentaa muita Node.js Kehitystyökalut

[Gulp.js](http://gulpjs.com) avulla voit automatisoida oman pii otoksen sovelluksen käyttöönotto. [Laitteen etsiminen-cli](https://github.com/Azure/device-discovery-cli) käyttää myös IoT laitteidesi verkon tietojen hakemiseen.

Avaa komentokehote järjestelmänvalvojana. Asenna `gulp` ja `device-discovery-cli` suorittamalla seuraavan komennon:

```bash
npm install -g device-discovery-cli gulp
```
    
Jos kohtaat ongelmia Node.js ja lisää nämä Node.js Kehitystyökalut asentaminen tietokoneeseen, on artikkelissa [vianmääritysoppaan](iot-hub-raspberry-pi-kit-node-troubleshooting.md) ratkaisu yleisimpiin ongelmiin.

## <a name="126-install-visual-studio-code"></a>1.2.6 Asenna Visual Studio-koodin.

[Lataa](https://code.visualstudio.com/docs/setup/windows) ja asenna Visual Studio-koodin. Visual Studio-koodi on kevyt, mutta tehokas lähde-koodi-editori, Windows, Linux ja Mac OS. Tämän editorin käyttäminen opetusohjelman myöhemmin-mallikoodin muokkaaminen.

## <a name="127-summary"></a>1.2.7 yhteenveto

Olet asentanut tarvittavat Kehitystyökalut ja sovelluksen ensimmäisen malli-ohjelmiston. Seuraavassa osassa luominen, ottaminen käyttöön ja suorita oman pii sovelluksen malli.

## <a name="next-steps"></a>Seuraavat vaiheet

[1.3 luoda ja ottaa käyttöön sovelluksen vilkunta malli](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)
