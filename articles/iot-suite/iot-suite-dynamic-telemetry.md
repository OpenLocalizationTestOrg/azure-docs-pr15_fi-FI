<properties
    pageTitle="Käytä dynaaminen telemetriatietojen | Microsoft Azure"
    description="Katso tämä opetusohjelma lisätietoja dynaaminen telemetriatietojen käyttäminen remote seurantaa esimääritettyjä ratkaisu."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="dobett"/>

# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Dynaaminen telemetriatietojen käyttäminen remote seurantaa esimääritettyjä ratkaisu

## <a name="introduction"></a>Johdanto

Dynaaminen telemetriatietojen mahdollistaa visualisointi minkä tahansa telemetriatietojen lähetetään remote seurantaa esimääritettyjä ratkaisu. Simuloitu laitteita, jotka esimääritettyjä ratkaisun käyttöönotto Lähetä lämpötilan ja kosteuden telemetriatietojen, jossa voit havainnollistaa koontinäytössä. Jos mukauttaa olemassa olevan Simuloitu laitteiden tai luoda uusia tiedostoja Simuloitu fyysisten laitteiden yhdistäminen esimääritettyjä ratkaisu voit lähettää esimerkiksi ulkoinen lämpötilan, JÄLLEENMYYNTIHINNAN tai tuulen nopeus telemetriatietojen muita arvoja. Voit esittää sitten tämä Lisää telemetriatietojen koontinäytössä.

Tässä opetusohjelmassa käyttää yksinkertainen Node.js Simuloitu laite, jota voit helposti muokata dynaaminen telemetriatietojen kokeilla.

Jotta voit suorittaa tässä opetusohjelmassa, sinun on:

- Aktiivinen Azure-tilaus. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion][lnk_free_trial].
- [Node.js] [ lnk-node] versio 0.12.x tai uudempi versio.

Voit suorittaa tässä opetusohjelmassa sellaisille käyttöjärjestelmille, kuten Windows- tai Linux, jossa voit asentaa Node.js käyttöön.

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="configure-the-nodejs-simulated-device"></a>Määritä Node.js Simuloitu laite

1. Remote seurantaa koontinäytössä napsauttamalla **+ Lisää laite** ja Lisää mukautetun laitteen. Pane merkille IoT-toiminnossa hostname laitteen tunnus ja laitteen avainta. Tarvitset niitä myöhemmin: Tämä opetusohjelma, kun valmistelet remote_monitoring.js laite-asiakassovellukseen.

2. Varmista, että Node.js versio 0.12.x tai uudempi on asennettu kehittäminen käyttämääsi laitteeseen. Suorita `node --version` komentokehotteeseen tai -käyttöliittymän version tarkistamisesta. Lisätietoja paketinhallinnan avulla voit asentaa Node.js Linux on [Asentaminen Node.js kautta pakettien hallinta][node-linux].

3. Kun olet asentanut Node.js, Kloonaa [azure-iot-SDK: t] uusimman version[ lnk-github-repo] säilöön kehittäminen tietokoneeseesi. Käytä aina **perusmuodon** haaran kirjastoihin ja näytteiden uusimman version.

4. FROM [azure-iot-SDK: t] paikallisen kopion[ lnk-github-repo] säilöön, kopioi seuraavat kaksi tiedostot solmu/laitteen/Mallit-kansion Tyhjennä kansio kehittäminen-tietokoneessa:

  - Packages.JSON
  - remote_monitoring.js

5. Avaa remote_monitoring.js-tiedosto ja Etsi seuraavan muuttujan määritelmän:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

6. Korvaa laitteen yhteysmerkkijonon **[IoT keskittimeen laitteen yhteysmerkkijonon]** . Käyttäminen arvot IoT keskittimeen hostname, laitetunnus ja laitteen avainta kirjoittamaasi vastausta muistiin vaiheessa 1. Laitteen yhteysmerkkijonon on seuraavanlainen:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Jos laitetunnus on **mydevice**IoT keskittimeen isäntänimi on **contoso** , yhteysmerkkijono näyttää seuraavalta:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

7. Tallenna tiedosto. Suorita seuraavat komennot käyttöliittymän tai komentokehote, joka sisältää Asenna tarvittavat paketit ja suorita sitten malli-sovelluksen tiedostot-kansiossa:

    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Noudata dynaamisen telemetriatietojen-toiminto

Koontinäytön näkyy lämpötilan ja kosteuden-telemetriatietojen aiemmin Simuloitu laitteilla:

![Oletusarvoisen koontinäytön][image1]

Jos valitset edellisessä osassa suoritit Node.js Simuloitu laitteen, näet lämpötila, kosteus ja ulkoiset lämpötilan telemetriatietojen:

![Lisää ulkoisia lämpötilan koontinäyttö][image2]

Remote seurantaa ratkaisu tunnistaa muita ulkoisen lämpötilan telemetriatietojen tyyppi ja Lisää kaavioon koontinäytössä automaattisesti.

## <a name="add-a-telemetry-type"></a>Lisää telemetriatietojen tyyppi

Seuraava vaihe on korvata Node.js Simuloitu laitetta, jonka uusi arvojoukon luoma telemetriatietojen:

1. Pysäytä Node.js Simuloitu laitteen kirjoittamalla komentokehote tai shell **Ctrl + C** .

2. Remote_monitoring.js-tiedosto näet aiemmin lämpötila, kosteus ja ulkoiset lämpötilan telemetriatietojen perustiedot-arvoja. Lisää perustiedot arvo **jälleenmyyntihinnan** seuraavasti:

    ```
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Node.js Simuloitu laitteen **generateRandomIncrement** -funktiota käytetään remote_monitoring.js tiedoston satunnaisia lisäys lisääminen perustiedot-arvoja. Satunnainen järjestys arvo **jälleenmyyntihinnan** lisäämällä koodin jälkeen aiemmin randomizations seuraavasti:

    ```
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Lisää uusi jälleenmyyntihinnan arvo JSON-paketti, laite lähettää IoT keskittimeen:

    ```
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Suorita Node.js Simuloitu laitteen avulla seuraava komento:

    ```
    node remote_monitoring.js
    ```

6. Noudata uusi JÄLLEENMYYNTIHINNAN telemetriatietojen tyyppi, joka näyttää kaaviossa koontinäytön:

![Lisää JÄLLEENMYYNTIHINNAN koontinäyttö][image3]

> [AZURE.NOTE] Voit joutua käytöstä ja ottaa Node.js laitteen **laitteet** -sivulla voit tarkastella muutosta heti Raporttinäkymät-ikkunan.

## <a name="customize-the-dashboard-display"></a>Raporttinäkymät-ikkunan näytön mukauttaminen

**Laitteen tiedot** viesti voi olla laitteen lähettää IoT keskittimeen telemetriatietojen metatietoa. Metatiedot voit määrittää laite lähettää telemetriatietojen tyypit. Muokkaa remote_monitoring.js-tiedosto, joka sisältää seuraavat **Komennot** -määritys **Telemetriatietojen** määritelmän **deviceMetaData** arvo. Seuraavat koodikatkelman näkyy **Komennot** -määritys (muista lisätä `,` jälkeen **Komennot** -määritys):

```
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [AZURE.NOTE] Remote seurantaa ratkaisu käyttää asennustavan vastine verrata tietoja telemetriatietojen virta metatiedot-määritys.

Lisääminen **Telemetriatietojen** määrityksen mukaisesti edellisen koodikatkelman muuta koontinäyttö toimintaa. Metatietojen voi kuitenkin sisältää myös **Näyttönimi** -määrite, voit mukauttaa näytön koontinäytön. Päivittää **Telemetriatietojen** metatietojen määrityksen seuraavat koodikatkelman esitetyllä tavalla:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Seuraavassa näyttökuvassa näkyy, miten tämä muutos Muokkaa kaavion selitteen koontinäytössä:

![Kaavion selitteen mukauttaminen][image4]

> [AZURE.NOTE] Voit joutua käytöstä ja ottaa Node.js laitteen **laitteet** -sivulla voit tarkastella muutosta heti Raporttinäkymät-ikkunan.

## <a name="filter-the-telemetry-types"></a>Suodattaa telemetriatietojen tyypit

Koontinäytössä kaavio näyttää oletusarvoisesti jokaisella arvosarjan telemetriatietojen-muodossa. Voit käyttää **Laitteen tiedot** metatiedot näyttämisen tietyn telemetriatietojen tyyppisiä kaaviossa. 

Jos haluat näyttää vain lämpötilan ja kosteuden telemetriatietojen kaavion, pois **Laitteen tiedot** **Telemetriatietojen** metatiedoista **ExternalTemperature** seuraavasti:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

Kaavion näkyvät enää **Ulkokäyttöön lämpötila** :

![Suodata telemetriatietojen koontinäytössä][image5]

Tämä muutos vaikuttaa vain kaavio näyttää. **ExternalTemperature** tietoarvojen edelleen tallennettujen ja käytettävissä minkä tahansa Taustajärjestelmä käsittelyä varten.

> [AZURE.NOTE] Voit joutua käytöstä ja ottaa Node.js laitteen **laitteet** -sivulla voit tarkastella muutosta heti Raporttinäkymät-ikkunan.

## <a name="handle-errors"></a>Virheiden käsittelyä

**Laitteen tiedot** metatiedot sen **tyyppiä** vastattava tietovirta, jos haluat näyttää kaavion, telemetriatietojen-arvojen tietotyyppi. Esimerkiksi jos metatiedot määrittää, että kosteus tietojen **tyyppi** on **kokonaisluku** ja **Kaksinkertainen** löytyy telemetriatietojen virta sitten kosteus telemetriatietojen eivät näy kaaviossa. **Kosteus** -arvot ovat kuitenkin yhä tallennettu ja käytettävissä minkä tahansa Taustajärjestelmä käsittelyä varten.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun nähnyt käyttämisestä dynaaminen telemetriatietojen, voit lukea lisää siitä, miten esimääritetyt ratkaisut käyttää laitteen tiedot: [laitteen tiedot metatiedot-kaukosäätimen seuranta esimääritettyjä ratkaisu][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image1]: media/iot-suite-dynamic-telemetry/image1.png
[image2]: media/iot-suite-dynamic-telemetry/image2.png
[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdks
