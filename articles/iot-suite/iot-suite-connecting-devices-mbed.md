<properties
   pageTitle="C käyttäminen mbed laite | Microsoft Azure"
   description="Tässä artikkelissa käsitellään laitteen liittäminen Azure IoT Suite valmiiksi remote seurantaa ratkaisun kirjoitettu käynnissä mbed laitteessa C-sovelluksesta."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Laitteen yhdistäminen remote seurantaa esimääritettyjä ratkaisun (mbed)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>Luoda ja suorittaa C otoksen ratkaisu

Seuraavissa ohjeissa kerrotaan ohjeet yhteyden [Mbed käyttävä Freescale FRDM-K64F] [ lnk-mbed-home] laitteen remote seurantaa ratkaisuksi.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Liitä verkko- ja työpöydän tietokoneen mbed-laite

1. Liitä Ethernet-kaapelin avulla verkon mbed-laite. Tämä vaihe on tarpeen, koska sovelluksen malli edellyttää internet-yhteyttä.

2. Katso [Käytön aloittaminen mbed] [ lnk-mbed-getstarted] muodostaa mbed laitteen pöytäkoneeseen.

3. Jos Pöytätietokoneen on käynnissä Windows-kohdassa [PC Configuration] [ lnk-mbed-pcconnect] määrittäminen mbed-laitteen käyttö sarja.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Luo mbed-projekti ja tuoda sample code

1. Siirry selaimellasi osoitteeseen mbed.org [kehittäjäsivusto](https://developer.mbed.org/). Jos sinulla ei ole vielä, näkyviin tulee vaihtoehto Luo tili (se on ilmainen). Muussa tapauksessa Kirjaudu sisään tilin tunnistetiedot. Valitse **kääntäjän** sivun oikeassa yläkulmassa. Tämä toiminto yhdistää *työtilan* liittymään.

2. Varmista, että käytät laitteisto-ympäristö näkyy ikkunan oikeassa yläkulmassa tai valitse Valitse laitteisto-ympäristö oikeassa alakulmassa olevaa kuvaketta.

3. Valitse **Tuo** päävalikosta. Valitse Tuo URL-Osoitteen linkin vieressä mbed maapallo-logon **napsauttamalla tätä** .

    ![][6]

4. Ponnahdusikkunan Kirjoita linkin otoksen koodin https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ sitten **Tuo**.

    ![][7]

5. Näet mbed kääntäjän-ikkunassa, myös projektin tuomisen tuonti eri kirjastoihin. Jotkin tarjotaan ja Azure IoT ryhmän ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)) ylläpitämä toiset ovat käytettävissä olevat mbed kirjastojen luettelon kolmannen osapuolen kirjastot.

    ![][8]

6. Avaa remote_monitoring\remote_monitoring.c-tiedosto ja etsi tiedosto seuraava koodi:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Korvaa [laitteen tunnus] ja [laitteen avaimen] laitteen tietojen käyttöön malli-ohjelman muodostaa yhteyden IoT-toiminnossa. IoT keskittimeen Hostname avulla voit korvata [IoTHub nimi] ja [Suffiksi IoTHub, eli azure devices.net]-paikkamerkkejä. Esimerkiksi jos IoT-toiminnossa Hostname **contoso.azure devices.net**, **contoso** on **hubName** ja kaikki sen jälkeen, kun se on **hubSuffix**:

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### <a name="walk-through-the-code"></a>Käy läpi koodi

Jos olet kiinnostunut ohjelman toiminta, tässä osassa käsitellään joitakin sample code tärkeisiin osiin. Jos haluat suorittaa koodin, Ohita [Muodosta](#buildandrun)ja suorita ohjelma.

#### <a name="defining-the-model"></a>Mallin määrittäminen

Tässä esimerkissä käytetään [Sarjatoiminto] [ lnk-serializer] kirjastossa voit määrittää mallin, joka määrittää viestit laitteen IoT keskittimeen lähettää ja vastaanottaa IoT-toiminnosta. Tässä esimerkissä **Contoso** -nimitilan määrittää **termostaatin** mallin, joka määrittää **lämpötilan**, **ExternalTemperature**ja **kosteuden** telemetriatietojen tiedot metatietoja, kuten laitteen tunnus, laitteiden ominaisuuksia ja komentoja, joka vastaa laitteen kanssa:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

Mallimäärityksen liittyvät ovat **SetTemperature** ja **SetHumidity** komentoja, joka vastaa laitteen määritykset:

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### <a name="connecting-the-model-to-the-library"></a>Yhteyden muodostaminen kirjaston mallin

Funktiot **sendMessage** ja **IoTHubMessage** ovat asiakirjan osien uudelleen käyttäminen koodi lähettäminen telemetriatietojen laitteesta ja viestien liittämisestä komento käsittelytoimintoja IoT toiminnossa.

#### <a name="the-remotemonitoringrun-function"></a>Remote_monitoring_run-funktio

Ohjelman **tärkeimmät** funktio käynnistää **remote_monitoring_run** -funktio, kun sovellus käynnistyy suorittamaan laitteen toiminta IoT keskittimeen-laitteen asiakkaaksi. **Remote_monitoring_run** -toiminto koostuu lähinnä Sisäkkäisiä funktioita paria:

- **ympäristön\_alusta** ja **ympäristö\_deinit** suorittaa käyttöjärjestelmäkohtaiset alustaminen ja Sammuta.
- **Sarjatoiminto\_alusta** ja **Sarjatoiminto\_deinit** alusta ja alustaa varaustiedoista Sarjatoiminto-kirjastoa.
- **IoTHubClient\_Luo** ja **IoTHubClient\_Destroy** luoda asiakas-kahva **iotHubClientHandle**-yhteyden muodostamisesta IoT-toiminnossa laite-tunnistetiedoilla.

**Remote_monitoring_run** -funktion pääosaa ohjelma suorittaa seuraavia toimintoja käyttämällä **iotHubClientHandle** kahvaa:

- Luo erillisen Contoso termostaatin mallin ja määrittää viestin takaisinkutsuja kaksi komentoja.
- Lähettää laitteen itse, mukaan lukien komentoja, se tukee oman IoT keskittimeen Sarjatoiminto-kirjaston käytön tietoja. Kun valitsemalla vastaanottaa viestiä, se muuttuu laitteen tila koontinäytön **odotetaan** **käynnissä**.
- Käynnistää **samalla, kun** silmukka, joka lähettää lämpötilan, ulkoiset lämpötila ja kosteus arvot IoT keskittimeen sekunnin välein.

Käyttöä seuraavassa on IoT keskittimeen käynnistyksen lähetetty esimerkkiviesti **DeviceInfo** :

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

Viitteen tässä on lähetetty IoT keskittimeen **Telemetriatietojen** esimerkkiviesti:

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

Viitteen tässä on esimerkki **komento** vastaanotettu IoT-toiminnosta:

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>
### <a name="build-and-run-the-program"></a>Muodosta ja suorita ohjelma

1. Valitse ohjelma luo **Käännä** . Voit turvallisesti ohittaa kaikki varoitukset, mutta jos luo Luo virheitä, korjaa ne ennen kuin jatkat.

2. Jos Luo on tarkistettu, mbed kääntäjän sivuston Luo .bin-tiedosto, jonka projektin nimen ja lataa se paikallisessa tietokoneessa. Kopioi laitteen .bin-tiedosto. Käynnistä ja suorita ohjelma .bin tiedostossa laitteen .bin-tiedoston tallentaminen laitteeseen valitaan. Voit käynnistää ohjelman manuaalisesti milloin tahansa painamalla mbed laitteeseen Palauta-painiketta.

3. Muodosta yhteys SSH-asiakassovellus, kuten painovärit, muste laitteella. Voit määrittää, että laitteesi käyttää Windows Device Manager valitsemalla portti.

    ![][11]

4. Valitse painovärit, muste, **Serial** yhteystyyppi. Laitteen muodostaa tavallisesti osoitteessa 9600 siirtonopeuden, joten kirjoita 9600 **nopeus** -ruutuun. Valitse sitten **Avaa**.

5. Ohjelma käynnistyy suorittamista. Saatat joutua Palauta tauluun (painamalla CTRL + Break tai paina taulun Palauta-painiketta), jos ohjelma ei käynnisty automaattisesti yhdistät.

    ![][10]

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer
