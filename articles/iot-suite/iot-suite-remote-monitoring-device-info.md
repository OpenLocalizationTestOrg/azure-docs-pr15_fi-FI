<properties
 pageTitle="Laitteen tiedot metatiedot remote seurantaa ratkaisun | Microsoft Azure"
 description="Azure-IoT kuvauksen valmiiksi ratkaisu remote seuranta- ja sen arkkitehtuuri."
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
 ms.date="09/12/2016"
 ms.author="dobett"/>

# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Laitteen tiedot metatiedot remote seurannassa valmiiksi ratkaisu

Azure IoT Suite remote seuranta esimääritettyjä ratkaisu esitellään toimintatavan laitteen metatietojen hallintaan. Tässä artikkelissa kuvataan tämä ratkaisu on aikaa, joiden avulla voit ymmärtää lähestymistapa:

- Mitä laitteen metatiedot tallentaa ratkaisun.
- Miten ratkaisun hallitsee laitteen metatiedot.

## <a name="context"></a>Konteksti

Remote seurantaa esimääritettyjä ratkaisu käyttää [Azure IoT keskittimeen] [ lnk-iot-hub] käyttöön tietojen lähettäminen pilveen laitteet. IoT keskittimeen sisältää [laitteen tunnistetietojen rekisterin] [ lnk-identity-registry] , IoT pääsivuston käyttöoikeuksien hallinta. IoT keskittimeen laitteen tunnistetietojen rekisteri on erillinen etäyhteyksien seuranta ratkaisu kielikohtaiset *laitteen rekisterin* , jonka laitteen tiedot metatiedot. Remote seurantaa ratkaisu käyttää [DocumentDB] [ lnk-docdb] tietokannan toteuttamisesta sen laitteen rekisteri palauttamista laitteen tiedot metatiedot. [Microsoft Azure IoT viittaus arkkitehtuuri] [ lnk-ref-arch] kuvaa tyypillinen IoT-ratkaisun laitteen rekisterin roolia.

> [AZURE.NOTE] Remote seurantaa esimääritettyjä ratkaisu pitää laitteen tunnistetietojen rekisterin synkronoinnin laitteen rekisterin kanssa. Molemmat yksilöivät kukin laite on yhdistetty IoT keskittimeen saman laitteen tunnuksen avulla.

[IoT keskittimeen laitteen hallinta esikatselu] [ lnk-dm-preview] Lisää IoT-keskittimeen, jotka muistuttavat laitteen tietojen hallintaominaisuudet tässä artikkelissa kuvattuja ominaisuuksia. Tällä hetkellä remote seurantaa ratkaisu käyttää vain yleensä käytettävissä olevia (GA) ominaisuuksia IoT toiminnossa.

## <a name="device-information-metadata"></a>Laitteen tiedot metatiedot

Laitteen tiedot metatietojen JSON tiedostoa laitteen rekisterin DocumentDB tietokanta on seuraava rakenne:

```
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

- **DeviceProperties**: itse laitteen kirjoittaa nämä ominaisuudet ja laite on tämä tiedoille myöntäjä. Esimerkki muiden laitteiden ominaisuuksia ovat valmistajan mallinumero ja sarjanumero. 
- **DeviceID**: laitteen yksilöllinen tunnus. Tämä arvo on sama rekisterissä IoT keskittimeen laitteen tunnistetiedot.
- **HubEnabledState**: IoT toiminnossa laitteen tila. Tämä arvo on aluksi **null** vasta, kun laite on ensin yhdistetään. Ratkaisu-portaalissa ei **ole** arvoa esitetään lukuna laite on "rekisteröity mutta ei ole."
- **CreatedTime**: aika laite on luotu.
- **DeviceState**: tilan ilmoittaa laite.
- **UpdatedTime**: aika, laite on päivitetty palvelun ratkaisu-portaalissa.
- **SystemProperties**: ratkaisu-portaalin kirjoittaa järjestelmän ominaisuuksia ja laite ei ole tietää ominaisuuksia. Esimerkki järjestelmäominaisuus, joka on **ICCID** , jos ratkaisu on sallittua kanssa ja yhteydessä SIM käyttävä laitteiden hallinta-palveluun.
- **Komentojen**: komentojen luettelo laite tukee. Laitteen antaa nämä tiedot ratkaisu.
- **CommandHistory**: lähettämä remote seurantaa ratkaisu laitteen ja komentojen tilan komentojen luettelo.
- **IsSimulatedDevice**: lippu, joka ilmaisee laitteen Simuloitu laitteen.
- **ID**: laitteen tämän asiakirjan yksilöllinen DocumentDB.

> [AZURE.NOTE] Laitetietojen voi sisältää myös kuvaamaan laite lähettää IoT keskittimeen telemetriatietojen metatiedot. Remote seurantaa ratkaisu käyttää telemetriatietojen metatiedot Mukauta tapaa, jolla koontinäyttö näyttää [dynaamisen telemetriatietojen][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Elinkaari

Kun luot laitteen ratkaisu-portaalissa, ratkaisun Luo merkinnän sen laitteen rekisterin edellä osoitetulla tavalla. Suuri osa tiedoista on alun perin stubbed ja **HubEnabledState** on määritetty **null**. Tässä vaiheessa ratkaisun luo myös merkinnän laitteen laitteen tunnistetietojen rekisteri, joka luo laitteen käyttää IoT keskittimeen todentamismenetelmä näppäimet.

Kun laite muodostaa ensin ratkaisun, se lähettää viestin laitteen tiedot. Tämä laite sanoman sisältää laitteiden ominaisuuksia, kuten laitteen valmistajan, mallinumero ja sarjanumero. Laitteen tiedot viestin myös laite tukee myös komento parametreja tietoja komentojen luettelo. Kun ratkaisun vastaanottaa viestiä, toiminto päivittää laitteen rekisterin laitteen tiedot-metatiedot.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Voit tarkastella ja muokata laitteen tiedot ratkaisu-portaalissa

Ratkaisu-portaalissa laiteluettelossa näkyvät seuraavat laitteiden ominaisuuksia sarakkeiksi: **tila**, **DeviceId**, **valmistajan**, **Mallinumero**, **sarjanumero**, **laitteisto**, **ympäristössä**, **suorittimen**ja **Asennettu RAM -Muistia**. Laitteen ominaisuudet **leveys** - ja **Pituusastetiedot** vaikuttavat Bing-kartan sijainti koontinäytössä. 

![Laiteluettelo][img-device-list]

Jos valitset **Muokkaa** **Laitteen** tietoruudussa ratkaisu-portaalissa, voit muokata nämä ominaisuudet. Näiden ominaisuuksien muokkaaminen päivittää laitteen DocumentDB tietokannan tietueen. Jos laite lähettää viesti on päivitetty laitteen tiedot, se korvaa kuitenkin ratkaisu-portaalissa tehdyt muutokset. Et voi muokata **DeviceId**, **isäntänimi**, **HubEnabledState**, **CreatedTime**, **DeviceState**ja **UpdatedTime** ominaisuudet ratkaisu-portaalissa, koska vain laitteessa on viranomainen näiden ominaisuuksien päälle.

![Laitteen muokkaaminen][img-device-edit]

Voit käyttää ratkaisu-portaalin laitteen poistaminen ratkaisu. Kun poistat laitteen, ratkaisun laitteen tiedot metatiedot poistaa ratkaisun laitteen rekisterin ja poistaa viennin IoT keskittimeen laitteen tunnistetietojen rekisteristä. Ennen kuin voit poistaa laitteen, sinun on poistettava se käytöstä.

![Laitteen poistaminen][img-device-remove]

## <a name="device-information-message-processing"></a>Laitteen tietojenkäsittely

Laitteen tiedot lähettämien viestien laitteen poikkeavat telemetriatietojen viestejä. Laitteen tiedot viestit sisältävät tietoja, kuten laitteiden ominaisuuksia, laite voi vastata komennot ja mitä tahansa komentoa historia. IoT-toiminnossa itse tuntee ei ole laitteen tiedot viestin metatiedot ja käsittelee viestin käsittelee laitteen cloud viestejä samalla tavalla. Remote seurantaa ratkaisussa [Azure Stream Analytics] [ lnk-stream-analytics] (ASA) työn lukee viestit IoT-toiminnosta. **DeviceInfo** stream analytics projektin suodattimet viestit, jotka sisältävät **"Objektilaji": "DeviceInfo"** ja välittää niitä, joka suoritetaan web työn **EventProcessorHost** Host (isäntä)-esiintymässä. Logiikan **EventProcessorHost** esiintymän käyttää laitteen tunnus DocumentDB tietueen etsiminen laitetta ja Päivitä tietue. Laitteen rekisterin tietue sisältää nyt tietoja, kuten laitteiden ominaisuuksia, komentoja ja komennon historia.

> [AZURE.NOTE] Laitteen tiedot viestin on vakio laitteen cloud-viesti. Ratkaisu erotetaan toisistaan laitteen tiedot viestit ja telemetriatietojen viestien ASA kyselyjen avulla.

## <a name="example-device-information-records"></a>Esimerkki laitteen tietueita.

Remote seurantaa esimääritettyjä ratkaisu käyttää kahdenlaisia laitteen tietueita: tietueiden Simuloitu laitteiden käyttöön ratkaisu ja tietueiden mukautetun ratkaisun yhdistäminen laitteiden kanssa.

### <a name="simulated-device"></a>Simuloitu laite

Seuraavassa esimerkissä esitetään Simuloitu laitteen JSON laitteen tiedot-tietue. Tämä tietue on määritetty **UpdatedTime**, joka ilmaisee laite on lähettänyt viestin **DeviceInfo** IoT keskittimeen arvo. Tietue sisältää joitakin yleisiä laitteiden ominaisuuksia, määrittää kuuden komennot Simuloitu laitteet tukevat ja on **IsSimulatedDevice** Merkitse arvoksi **1**.

```
{
  "DeviceProperties": {
    "DeviceID": "SampleDevice001_455",
    "HubEnabledState": true,
    "CreatedTime": "2016-01-26T19:02:01.4550695Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-01T15:28:41.8105157Z",
    "Manufacturer": "Contoso Inc.",
    "ModelNumber": "MD-369",
    "SerialNumber": "SER9009",
    "FirmwareVersion": "1.39",
    "Platform": "Plat-34",
    "Processor": "i3-2191",
    "InstalledRAM": "3 MB",
    "Latitude": 47.583582,
    "Longitude": -122.130622
  },
  "Commands": [
    {
      "Name": "PingDevice",
      "Parameters": null
    },
    {
      "Name": "StartTelemetry",
      "Parameters": null
    },
    {
      "Name": "StopTelemetry",
      "Parameters": null
    },
    {
      "Name": "ChangeSetPointTemp",
      "Parameters": [
        {
          "Name": "SetPointTemp",
          "Type": "double"
        }
      ]
    },
    {
      "Name": "DiagnosticTelemetry",
      "Parameters": [
        {
          "Name": "Active",
          "Type": "boolean"
        }
      ]
    },
    {
      "Name": "ChangeDeviceState",
      "Parameters": [
        {
          "Name": "DeviceState",
          "Type": "string"
        }
      ]
    }
  ],
  "CommandHistory": [],
  "IsSimulatedDevice": 1,
  "Version": "1.0",
  "ObjectType": "DeviceInfo",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleDevice001_455",
    "ConnectionDeviceGenerationId": "635894317219942540",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  },
  "SystemProperties": {
    "ICCID": null
  },
  "id": "7101c002-085f-4954-b9aa-7466980a2aaf"
}
```

### <a name="custom-device"></a>Mukautetun laitteen

Seuraavassa esimerkissä näkyy Mukautettu laitteen JSON laitteen tiedot-tietue, ja se on **IsSimulatedDevice** -merkinnän arvoksi **0**. Näet tämän mukautetun laitteen tukee kahta komennot ja että ratkaisu-portaalissa on lähettänyt **SetTemperature** komennon laitteeseen:

```
{
  "DeviceProperties": {
    "DeviceID": "mydevice01",
    "HubEnabledState": true,
    "CreatedTime": "2016-03-28T21:05:06.6061104Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-07T22:05:34.2802549Z"
  },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [
    {
      "Name": "SetHumidity",
      "Parameters": [
        {
          "Name": "humidity",
          "Type": "int"
        }
      ]
    },
    {
      "Name": "SetTemperature",
      "Parameters": [
        {
          "Name": "temperature",
          "Type": "int"
        }
      ]
    }
  ],
  "CommandHistory": [
    {
      "Name": "SetTemperature",
      "MessageId": "2a0cec61-5eca-4de7-92dc-9c0bc4211c46",
      "CreatedTime": "2016-06-07T21:05:18.140796Z",
      "Parameters": {
        "temperature": 20
      },
      "UpdatedTime": "2016-06-07T21:05:18.716076Z",
      "Result": "Expired"
    }
  ],
  "IsSimulatedDevice": 0,
  "id": "6184ae0f-2d94-4fbd-91cd-4b193aecc9d1",
  "ObjectType": "DeviceInfo",
  "Version": "1.0",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleCustom",
    "ConnectionDeviceGenerationId": "635947959068246845",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  }
}
```

Seuraavassa kuvassa näkyy JSON **DeviceInfo** viesti lähetetään päivittää laitteen tiedot metatiedot laitteen:

```
{ "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties": { "DeviceID":"mydevice01", "HubEnabledState":true },
  "Commands": [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    {"Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

## <a name="next-steps"></a>Seuraavat vaiheet

Kun lopetat koulujen, kuinka voit mukauttaa esimääritetyt ratkaisut, voit tutkia joitakin muita ominaisuuksia ja toimintoja IoT Suite valmiiksi ratkaisuja:

- [Ennakoivan ylläpito valmiiksi ratkaisun yleiskatsaus][lnk-predictive-overview]
- [Usein kysyttyjä kysymyksiä IoT Suite][lnk-faq]
- [IoT suojaus on alusta alkaen][lnk-security-groundup]



<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-ref-arch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dm-preview]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
