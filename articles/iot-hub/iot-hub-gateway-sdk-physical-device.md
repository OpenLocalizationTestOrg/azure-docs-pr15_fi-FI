<properties
    pageTitle="Todellista laitetta käyttäminen IoT yhdyskäytävän SDK | Microsoft Azure"
    description="Azure IoT yhdyskäytävän SDK ongelmatilanteita Texas säädösten SensorTag-laitteen käyttäminen tietojen lähettäminen IoT keskittimeen yhdyskäytävän käytössä Intel Edison Laske moduulissa kautta"
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-real-device-using-linux"></a>Azure IoT yhdyskäytävän SDK (beta) – lähettämiseen laitteen pilvipalveluun käyttämällä Linux reaali laitteen kanssa

Tätä vaiheittaista [Bluetooth pienen energiajärjestelmien otoksen] [ lnk-ble-samplecode] näytetään, miten voit käyttää [Azure IoT yhdyskäytävän SDK] [ lnk-sdk] , eteenpäin laitteen cloud-telemetriatietojen IoT keskittimeen fyysinen laitteessa ja kierrättäminen komennot IoT-toiminnosta fyysinen laitteeseen.

Tätä vaiheittaista kuuluvat:

* **Arkkitehtuuri**: Bluetooth pienen energiajärjestelmien otosten arkkitehtuuri tärkeitä tietoja.

* **Muodosta ja suorita**: Muodosta ja suorita otosten tarvittavat vaiheet.

## <a name="architecture"></a>Arkkitehtuuri

Ongelmatilanteita avulla voit luoda ja käyttää IoT yhdyskäytävän Intel Edison Laske moduuli, joka suoritetaan Linux. Yhdyskäytävä on luotu käyttämällä IoT yhdyskäytävän SDK-paketissa. Malli käyttää Texas säädösten SensorTag Bluetooth pieni energiajärjestelmien (taulukko) laitteen lämpötilan tietojen keräämiseen.

Kun suoritat yhdyskäytävän sitä:

- Muodostaa yhteyden SensorTag laitteen Bluetooth pieni energiajärjestelmien (taulukko)-protokollaa.
- Muodostaa yhteyden IoT keskittimeen HTTP-protokolla.
- Välittää telemetriatietojen SensorTag laitteesta IoT keskittimeen.
- Reitittää komennot IoT-toiminnosta SensorTag laitteeseen.

Yhdyskäytävän sisältää seuraavat moduulit:

- *Taulukko-moduulissa* , joka liittymät lämpötilan tietojen vastaanottamiseksi laitteen ja Lähetä-komennot laitteen taulukko-laitteen kanssa.
- *Laitteen moduuliin Cloud taulukko* , joka kääntää JSON viestit tulevat pilveen kyselyjä taulukko ohjeet *taulukko moduuli*.
- *Lokin moduuli* , joka kirjautuu yhdyskäytävän viestit.
- *Käyttäjätietojen yhdistäminen moduuli* , joka kääntää taulukko laitteen MAC-osoitteet ja Azure IoT keskittimeen laitteen käyttäjätietojen välillä.
- *IoT keskittimeen moduuli* , joka lataa telemetriatietojen tiedot IoT-toiminnosta ja vastaanottaa laitteen komennot IoT-toiminnosta.
- *Taulukko tulostimen moduuli* , joka tulkitsee telemetriatietojen taulukko-laitteesta ja tulostaa muotoillut tiedot konsoliin vianmääritys ja virheenkorjaus otetaan käyttöön.

### <a name="how-data-flows-through-the-gateway"></a>Miten tiedot kulkee yhdyskäytävän

Estä seuraavassa kaaviossa on kuvattu telemetriatietojen Lataa tietojen kulun myyntijakso:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_upload_data_flow.png)

Ohjeet, joita kohteen telemetriatietojen kestää matkustaminen taulukko laitteesta IoT keskittimeen ovat seuraavat:

1. Taulukko-laite luo lämpötilan otoksen ja lähettää sen yhdyskäytävän taulukko-moduulissa kautta Bluetooth.
2. Taulukko-moduulin vastaanottaa otosten ja julkaisee broker sekä laitteen MAC-osoite.
3. Käyttäjätietojen yhdistäminen moduulin vastataan viestiä ja käyttää sisäinen taulukko laitteen MAC-osoitteen muuntamiseksi IoT keskittimeen laitteen jäsenyyden (laitteen tunnus ja laitteen avainta). Se julkaisee sitten uusi viesti, jossa on lämpötilan esimerkkitiedot laitteen, laitteen tunnus ja laitteen avainta MAC-osoite.
4. IoT keskittimeen moduulin vastaanottaa tämän uuden viestin (luomien tunnistetietojen yhdistämistä moduulin) ja julkaisee IoT keskittimeen.
5. Lokin moduulin kirjaa kaikki viestit välittäjän levylle.

Estä seuraavassa kaaviossa on kuvattu laitteen komento tietojen kulun myyntijakso:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_command_data_flow.png)

1. IoT keskittimeen moduulin tekee säännöllisesti kyselyn uudet viestit-komento IoT-toiminnossa.
2. Kun IoT keskittimeen moduulin vastaanottaa komento uuden viestin, se julkaisee sen välittäjän.
3. Käyttäjätietojen yhdistäminen moduulin vastataan komento viestin ja käyttää sisäinen taulukko käännettävä IoT keskittimeen laitteen tunnus laitteeseen MAC-osoite. Se julkaisee sitten uusi viesti, joka sisältää kohteen laitteen MAC-osoitteen viestin ominaisuudet-määrityksen.
4. Taulukko pilveen laitteen moduuliin vastataan viestiä ja kääntää taulukko-moduulin ERISNIMI taulukko-Ohje. Se julkaisee sitten uusi viesti.
5. Taulukko-moduulin vastataan viestiä ja suorittaa i/o-ohjepaketti toimittamalla taulukko-laitteen kanssa.
6. Lokin moduulin kirjaa kaikki viestit välittäjän levylle.

## <a name="prepare-your-hardware"></a>Laitteiston valmisteleminen

Tässä opetusohjelmassa oletetaan, että käytössäsi on kytketty Intel Edison tauluun [Texas säädösten SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) laite.

### <a name="set-up-the-edison-board"></a>Edison taulun määrittäminen

Ennen kuin aloitat, sinun kannattaa varmistaa, että voit muodostaa Edison laitteen langattomaan verkkoon. Jos haluat Edison-laitteen määrittäminen yhdistäminen isäntätietokoneen. Intel sisältää seuraavat käyttöjärjestelmät apuviivat käytön aloittaminen:

- [Windowsin 64-bittinen Intel Edison kehittäminen-taulun aloittaminen][lnk-setup-win64].
- [Aloita Intel Edison kehittäminen tauluun 32-bittinen Windows-][lnk-setup-win32].
- [Intel Edison kehittäminen tauluun Mac OS x: ssä aloittaminen][lnk-setup-osx].
- [Käytön aloittaminen Intel® Edison tauluun Linux][lnk-setup-linux].

Edison-laitteen määrittäminen ja tutustu siihen, kaikki on suoritettava nämä vaiheet "käytön aloittaminen-artikkelit lukuun ottamatta viimeisessä vaiheessa"Valitse IDE", joka on tarpeettomia nykyisen opetusohjelmaan. Edison määritysprosessi lopussa on seuraavasti:

- Flashed oman Edison uusimman laitteisto-ohjelmiston kanssa.
- Muodostaa serial yhteyden isännältä Edison.
- Voit määrittää salasanan ja ottaa WiFi-että Edison **configure_edison** -komentosarjan suorittaminen

### <a name="enable-connectivity-to-the-sensortag-device-from-your-edison-board"></a>Ota käyttöön connectivity SensorTag laitteeseen Edison taulun

Ennen kuin suoritat otosten haluat varmistaa, että Edison taulun muodostaa SensorTag laitteeseen.

Sinun on ensin varmistaa, että oman Edison muodostaa SensorTag laitteeseen.

1. Salli bluetooth Edison- ja Tarkista versionumero on **5.37**.
    
    ```
    rfkill unblock bluetooth
    bluetoothctl --version
    ```

2. Suorita **bluetoothctl** -komento. Voit nyt vuorovaikutteinen bluetooth-käyttöliittymän. 

3. Kirjoita komento **virta** power bluetooth-ohjain ylöspäin. Tulos muistuttaa pitäisi näkyä seuraavat:
    
    ```
    [NEW] Controller 98:4F:EE:04:1F:DF edison [default]
    ```

4. Kun edelleen vuorovaikutteinen bluetooth-liittymän Lisää-komento **Tarkista** etsimään bluetooth-laitteet. Tulos muistuttaa pitäisi näkyä seuraavat:
    
    ```
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

5. Kirjastonäkymiä SensorTag laitteen painamalla pientä painiketta (vihreä LED olisi flash). Edison olisi Tutustu SensorTag laitteen:
    
    ```
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```
    
    Tässä esimerkissä näet, että SensorTag laitteen MAC-osoite on **A0:E6:F8:B5:F6:00**.

6. **Skannaa ulos** -komento kirjoittamalla tarkistuksen poistaminen käytöstä.
    
    ```
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

7. SensorTag laitteeseesi sen MAC-osoitteen avulla kirjoittamalla **yhteyden \<MAC-osoite >**. Huomaa, että alla otoksen tulosteen lyhennetty:
    
    ```
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```
    
    Huomautus: Voit lisätä GATT ominaispiirteet laitteen uudelleen **määritteiden luettelo** -komennolla.

8. Voit nyt katkaista käyttämän laitteen **Katkaise yhteys** -komento ja sulje bluetooth-liittymän, **Sulje** -komennon avulla:
    
    ```
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Olet nyt valmis suorittamaan taulukko yhdyskäytävän otosten Edison laitteen.

## <a name="run-the-ble-gateway-sample"></a>Suorita taulukko yhdyskäytävä-Esimerkki

Suorittaa taulukko otosten oman Edison, tarvitset valmiiksi kolme tehtävää:

- Määrittää kahden otoksen laitteen IoT-toiminnossa.
- Luoda IoT yhdyskäytävän SDK Edison laitteen.
- Määritä ja suorita taulukko otosten Edison laitteen.

Kirjoittaminen milloin IoT yhdyskäytävän SDK tukee vain yhdyskäytävät, jotka käyttävät taulukko moduulit Linux.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Kahden otoksen laitteen määrittäminen IoT-toiminnossa

- [Luo IoT-toiminnossa] [ lnk-create-hub] Azure-tilaukseesi tarvitset oman keskittimeen vaiheiden suorittamiseen nimi. Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -vain muutaman minuutin.
- Lisää kutsutaan **SensorTag_01** IoT keskittimeen yhteen laitteeseen ja sen tunnuksen ja laitteen avaimen muistiin. Voit käyttää [laitteen Explorer tai iothub explorer] [ lnk-explorer-tools] työkalujen laite lisääminen edellisessä vaiheessa luomasi IoT-toiminnosta ja sen avaimen hakemiseen. Kun määrität yhdyskäytävän, yhdistää laite SensorTag laitteeseen.

### <a name="build-the-iot-gateway-sdk-on-your-edison-device"></a>Muodosta Edison laitteen IoT yhdyskäytävän SDK-paketissa

**Git** Edsion-versio ei tue submodules. Lataa koko lähteen IoT yhdyskäytävän SDK Edison, sinulla on kaksi vaihtoehtoa:

- Vaihtoehto #1: Kloonaa [Azure IoT yhdyskäytävän SDK] [ lnk-sdk] säilöön-että Edison ja Kloonaa kunkin submodule säilö manuaalisesti.
- Vaihtoehto #2: Kloonaa [Azure IoT yhdyskäytävän SDK] [ lnk-sdk] säilöön työpöytäversiossa missä **git** tukee submodules ja kopioi sitten Valmis säilöön submodules sivulle oman Edison kanssa.

Jos valitset vaihtoehdon #2, Kloonaa IoT yhdyskäytävän SDK- ja kaikki sen submodules **git** seuraavista komennoista avulla:

```
git clone --recursive https://github.com/Azure/azure-iot-gateway-sdk.git 
git submodule update --init --recursive
```

Olisi sitten zip koko paikallisen säilöön yksittäisen arkisto-tiedostoon, ennen kuin voit kopioida Edison. Voit käyttää apuohjelmaa, kuten **pscp** **painovärit, muste** arkistotiedoston kopioiminen Edison mukana toimitetut. Esimerkki:

```
pscp .\gatewaysdk.zip root@192.168.0.45:/home/root
```

Kun sinulla kopion IoT yhdyskäytävän SDK tietovaraston oman Edison, voit luoda sen missä kansio, joka sisältää SDK seuraava komento:

```
./tools/build.sh
```

### <a name="configure-and-run-the-ble-sample-on-your-edison-device"></a>Määritä ja suorita Edison laitteen taulukko-Esimerkki

Jos haluat käynnistää ja suorita otosten, haluat määrittäminen kunkin moduuli, jolla osallistuu yhdyskäytävän. Tässä määrityksessä on annettu JSON tiedostossa ja sinun on määritettävä kaikki viisi osallistuvien moduulit. On nimeltään **gateway_sample.json** , jonka avulla voit aloittaa etsimisen oman kokoonpanotiedosto säilössä annettu mallitiedostossa JSON. Tiedosto on **esimerkkejä, joiden/ble_gateway_hl/src** -kansiossa IoT yhdyskäytävän SDK-tietovarasto paikallisen kopion.

Seuraavissa osissa kuvataan tämä taulukko otosten määritystiedosto ja oletetaan, että IoT yhdyskäytävän SDK säilö on **/home/root/azure-iot-gateway-sdk /** Edison laitteen-kansioon. Jos säilö on muualla, polut pitäisi muuttaa vastaavasti:

#### <a name="logger-configuration"></a>Lokin määritys

Oletetaan, että yhdyskäytävän säilö sijaitsee kansion **/home/root/azure-iot-gateway-sdk /**, Määritä lokin moduulia seuraavasti:

```json
{
    "module name": "logger",
    "module path": "/home/root/azure-iot-gateway-sdk/build/modules/logger/liblogger_hl.so",
    "args":
    {
        "filename":"/home/root/gw_logger.log"
    }
}
```

#### <a name="ble-module-configuration"></a>Taulukko moduulin määritys

Taulukko-laitteen otoksen määrityskohde olettaa Texas säädösten SensorTag laite. Normaali taulukko laitteeseen, jotka voivat toimia, kun GATT, joka on peripheral pitäisi toimia, mutta on päivitettävä GATT ominaisen tunnukset ja tietojen (Kirjoita ohjeet). Lisää SensorTag laitteen MAC-osoite: 

```json
{
  "module name": "SensorTag",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/ble/libble_hl.so",
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

#### <a name="iot-hub-module"></a>IoT keskittimeen moduuli

Lisää IoT-toiminnossa nimi. Liite-arvo on yleensä **azure devices.net**:

```json
{
  "module name": "IoTHub",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/iothub/libiothub_hl.so",
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport": "HTTP"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Käyttäjätietojen yhdistäminen moduulin määritys

Lisää oman SensorTag ja laitteen tunnus MAC-osoite ja avain IoT-toiminnossa on lisätty **SensorTag_01** laitteen:

```json
{
  "module name": "mapping",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/identitymap/libidentity_map_hl.so",
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>Taulukko tulostimen moduulin määritys

```json
{
    "module name": "BLE Printer",
    "module path": "/home/root/azure-iot-gateway-sdk/build/samples/ble_gateway_hl/ble_printer/libble_printer.so",
    "args": null
}
```

#### <a name="routing-configuration"></a>Reititys määritys

Seuraavat määritykset varmistaa seuraavasti:
- **Lokin** moduulin saa ilmoituksen ja kirjaudu kaikkiin viesteihin.
- **SensorTag** -moduulin lähettää viestejä **Yhdistäminen** ja **Taulukko tulostimen** moduulit.
- **Yhdistäminen** -moduulin lähettää viestejä voidaan lähettää IoT keskittimeen **IoTHub** -moduuli.
- **IoTHub** -moduulin lähettää viestejä takaisin **Yhdistäminen** -moduuli.
- **Yhdistäminen** -moduulin lähettää viestejä takaisin **SensorTag** moduuli.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "SensorTag" }
  ]
```

Jos haluat suorittaa otosten Suorita **ble_gateway_hl** binaarinen polku välittämistä JSON-kokoonpanotiedosto. Jos olet käyttänyt **gateway_sample.json** -tiedoston, suoritettava komento näyttää seuraavanlaiselta:

```
./build/samples/ble_gateway_hl/ble_gateway_hl ./samples/ble_gateway_hl/src/gateway_sample.json
```

Voit joutua painamaan pieni painike SensorTag kirjastonäkymiä ennen kuin suoritat otosten laitteessa.

Kun suoritat otosten, voit käyttää [laitteen Explorer tai iothub explorer] [ lnk-explorer-tools] yhdyskäytävän välittää SensorTag laitteesta viestien seuranta-työkalu.

## <a name="send-cloud-to-device-messages"></a>Cloud laitteen viestien lähettäminen

Taulukko-moduulin tukee myös lähettämisen ohjeita Azure IoT toiminnosta laitteeseen. Voit käyttää [Azure IoT keskittimeen laitteen Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) tai [IoT keskittimeen Explorer](https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer) , joka taulukko yhdyskäytävän moduulin välittää taulukko laitteen JSON-viestien lähettämiseen. Esimerkiksi jos käytössäsi on Texas säädösten SensorTag laitteen sitten voit lähettää JSON seuraavista ilmoituksista laitteeseen IoT-toiminnosta.

- Palauttaa kaikki merkkivalot ja summeri (poistaa ne käytöstä)

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

- Määritä i/o "etäyhteyksien"

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Punainen LED ottaminen käyttöön

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Vihreä LED ottaminen käyttöön

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

- Ota käyttöön summeri

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

HTTP-protokolla muodostaa IoT keskittimeen käyttävän laitteen oletusominaisuuksien on tarkistaa minuutin välein 25 uusi-komennon. Sen vuoksi, jos lähetät useita eri komentoja joudut odottamaan laitteen vastaanottamaan jokaisen komennon 25 minuuttia.

> [AZURE.NOTE] Yhdyskäytävän myös tarkistaa uusien komentojen aina, kun se käynnistyy, jotta voit pakottaa sen käsitellä komennon lopettaminen ja käynnistäminen yhdyskäytävän.

## <a name="next-steps"></a>Seuraavat vaiheet

Halutessasi artikkelista monimutkaisemman ymmärtämisen IoT yhdyskäytävän SDK ja kokeilla koodiesimerkkejä seuraavasta developer opetusohjelmassa ja resurssit:

- [Azure IoT yhdyskäytävän SDK-paketissa][lnk-sdk]

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Sovelluskehittäjän opas][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/ble_gateway_hl
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-setup-win64]: https://software.intel.com/get-started-edison-windows
[lnk-setup-win32]: https://software.intel.com/get-started-edison-windows-32
[lnk-setup-osx]: https://software.intel.com/get-started-edison-osx
[lnk-setup-linux]: https://software.intel.com/get-started-edison-linux
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/


[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
