<properties
 pageTitle="Vianmääritys | Microsoft Azure"
 description="Sivun vadelma pii Node.js takaamiseksi vianmääritys"
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

# <a name="troubleshooting"></a>Vianmääritys

## <a name="hardware-issues"></a>Laitteisto-ongelmat

### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Sovellus toimii hyvin, mutta LED ei vilkkuvaa

Tämä ongelma liittyy aina laitteiston piiri yhteydet. Seuraavien vaiheiden avulla voit tunnistaa ongelmia.

1. Tarkista, jos sinun on valittava kortin oikea **GPIO** . Tämä Harjoitus kaksi porttien on oltava **GPIO tulo GND (Pin-6)** ja **GPIO 04 (PIN-tunnuksen 7)**.
2. Oman LED polariteetti oikeellisuuden tarkistaminen. Pidempi jalan kertoo **positiivinen**, paitsi PIN-tunnuksen.
3. Käyttää **3.3V kiinnittäminen** ja **tulo GND PIN-tunnuksen** Vadelman pii 3. Käsittele oman pii Ohjauskoneen potenssiin. Tarkista, jos LED toimii moitteettomasti.

![LED määritys](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Laitteiston muita ongelmia

Lisätietoja vadelma pii 3: n yleisten ongelmien ratkaisemiseen on viitata [virallinen vianmäärityksen sivun](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Node.js paketin ongelmat

### <a name="no-response-during-gulp-tasks"></a>Ei vastausta gulp tehtävien aikana

Jos käytössä ilmenee ongelmia gulp tehtävät, voit lisätä `--verbose` virheenkorjaus-toiminto. Lopeta nykyinen gulp tehtävät ja yritä `Ctrl + C` ja suorita seuraava komento konsoli-ikkunassa voit tarkastella viestejä virheenkorjaus. Konsolin tuloste tulostetaan yksityiskohtaiset Virheilmoitukset voivat tulla. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Laitteen etsiminen ongelmat

Vianmääritysohjeita liittyviä ongelmia `devdisco` -komentoa, tarkista [Lueminut-tiedosto](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="other-npm-issues"></a>NPM muita ongelmia

Yritä päivittää NPM-paketin seuraavalla komennolla:

```bash
npm install -g npm
```

Jos ongelma on edelleen olemassa, kommenttisi on tämän artikkelin lopussa olevassa tai luo Github ongelman Microsoftin [Otoksen säilöön](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started)

## <a name="remote-debugging"></a>Remote virheenkorjaus

### <a name="run-the-sample-application-in-debug-mode"></a>Esimerkkisovelluksen käyttämiseen virheenkorjaus-tilassa

```bash
gulp run --debug
```

Kun virheenkorjaus-ohjelma on valmis, näyttöön tulee ```Debugger listening on port 5858``` konsolin tulosteessa.

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a>Määritä ja koodin etälaitteen yhdistäminen

Avaa sitten **Virheenkorjaus** -ruudun vasemmassa reunassa.

Napsauta vihreä **Käynnistä virheenkorjaus** (F5)-painiketta. Koodin ja Avaa **launch.json** -tiedosto, jonka haluat päivittää.

Päivitä seuraavat sisällön **launch.json** -tiedosto. Korvaa `[device hostname or IP address]` todellista laitteen IP-osoite tai isäntänimi.   

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Remote muistin määritys](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Liittää remote-sovellukseen

Napsauta vihreä **Käynnistä virheenkorjaus** (F5) painiketta Käynnistä virheenkorjaus. 

Lue lisätietoja virheenkorjaus [JavaScript-ja koodi](https://code.visualstudio.com/docs/languages/javascript#_debugging) .

![Etäyhteyksien virheenkorjaus vuorovaikutteisia](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Azure CLI ongelmat

Azure CLI on esikatselu muodosta. Voit viitata [Esikatselu asentaa opas](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) haku ratkaisuja.

Jos käytössä ilmenee jokin virheet-työkalulla, tiedoston [ongelman](https://github.com/Azure/azure-cli/issues) GitHub repo **ongelmat** -osassa.

Yleisiä ongelmia, vianmääritysohjeita Tarkista [Lueminut-tiedosto](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Python asennusongelmat

### <a name="legacy-installation-issues-macos"></a>Aiempien versioiden asennusongelmien (Mac OS)

**Pip**asennettaessa käyttöoikeusvirhe ilmenee, kun vanha paketteja, jotka asennetaan **su** käyttöoikeudet. Tämä tilanne ilmenee, koska aiempi asennus Python käyttämällä brew (Mac OS), ei poistetaan kokonaan. Jotkin Officen aiempi asennus **pip** -palvelupaketteja luotujen pääkansio, joka aiheuttaa virheen käyttöoikeus. Ratkaisu on näiden pääkansion asentama pakettien poistaminen. Seuraavien vaiheiden avulla voit suorittaa tämän tehtävän:

1. Siirry /usr/local/lib/python2.7/site-packages
2. Pakettien luominen pääkansio:`ls -l | grep root`
3. Poista pakettien vaiheen2 ohjeiden mukaisesti:`sudo rm -rf {package name}`
4. Asenna Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT keskittimeen ongelmat

Jos olet valmisteltu Azure IoT keskittimeen kanssa `azure-cli`, ja sinun on työkalun IoT-toiminnossa liittämisestä laitteiden hallinta, kokeile seuraavia työkaluja:

### <a name="device-explorer"></a>Laitteen Explorer

[Laitteen Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) suoritetaan Windows paikallisessa tietokoneessa ja muodostaa yhteyden IoT-toiminnossa Azure-tietokannassa. Se on seuraavia [IoT keskittimeen päätepisteet](iot-hub-devguide.md)yhteydessä:

- *Käyttäjätietojen hallinta* valmistelu ja hallitse rekisteröityjä IoT-toiminnossa.
- *Vastaanota laitteen pilveen* , jotta voit seurata IoT-toiminnossa laitteestasi lähetettyihin viesteihin.
- *Lähetä cloud laitteen* , jotta voit lähettää viestejä laitteet IoT-toiminnosta.

Määrittää oman `IoT hub connection string` tämän työkalun avulla voit käyttää sen ominaisuuksia.

### <a name="iot-hub-explorer"></a>IoT keskittimeen Explorer

[IoT keskittimeen Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/iothub-explorer/readme.md) on esimerkki useita CLI työkalu laitteen asiakkaiden hallinta. Työkalu mahdollistaa tunnistetietojen rekisterin laitteiden hallinta, valvoa laitteen cloud viestit ja lähettää cloud laitteen komennot.

(Ennakko) uusimman iothub explorer-työkalun asentaminen suorittamalla seuraavan komennon komentorivin ympäristössä:

```
npm install -g iothub-explorer@latest
```

Voit käyttää muita ohjeita iothub explorer komennot ja niiden parametrit seuraava komento:

```bash
iothub-explorer help
```

### <a name="use-azure-portal-to-manage-your-resources"></a>Azure portal avulla voit hallita resursseja

Kaikki nämä opitut koko CLI kokemus annetaan voit luoda ja hallita kaikki Azure resurssit. Haluat ehkä myös käyttämään [Azure portal](../azure-portal-overview.md) säännöstä, hallinta- ja virheenkorjaus Azure resurssit.

## <a name="azure-storage-issues"></a>Azure-tallennustilan ongelmien vianmääritys

[Microsoft Azure tallennustilan Explorer (ennakkoversio)](http://storageexplorer.com) on erillinen sovellus Microsoftilta, jonka avulla voit käyttää Windows, Mac OS ja Linux Azuren tallennustilaan tiedoilla. Tämän työkalun avulla voit muodostaa yhteyden taulukon ja sen tiedot ovat. Tällä työkalulla Azuren tallennustilaan-ongelmien vianmääritystä.
