<properties
    pageTitle="Mukauttaminen valmiiksi ratkaisuja | Microsoft Azure"
    description="Ohjeita siitä, miten voit mukauttaa valmiiksi Azure IoT Suite ratkaisuja."
    services=""
    suite="iot-suite"
    documentationCenter=".net"
    authors="aguilaaj"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/11/2016"
     ms.author="aguilaaj"/>

# <a name="customize-a-preconfigured-solution"></a>Valmiiksi määritetyt ratkaisun mukauttaminen

Azure IoT ohjelmiston mukana esimääritetyt ratkaisut näytetään yhteistyössä aikana lopusta loppuun-ratkaisun suite palveluihin. Ensimmäinen merkki siitä on eri paikoista, jossa voit laajentaa ja mukauttaa tietyissä skenaarioissa ratkaisu. Seuraavissa osissa on nämä yleiset mukauttaminen pistettä.

## <a name="finding-the-source-code"></a>Lähdekoodin etsiminen

Esimääritetyt ratkaisut-lähdekoodi on käytettävissä GitHub seuraavien säilöjen tietoihin:

- Remote seuranta: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
- Ennakoivan ylläpito: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

Esimääritetyt ratkaisut lähdekoodin annetaan kuvaavat kuvioiden ja käytäntöjä toteuttamisesta käyttämällä Azure IoT Suite IoT ratkaisun lopusta loppuun-toiminto. Lisätietoja siitä, miten voivat laatia ja kehittää ratkaisuja GitHub säilöjen tietoihin.

## <a name="changing-the-preconfigured-rules"></a>Valmiiksi määritetyt sääntöjen muuttaminen

Remote seuranta-ratkaisu sisältää kolme [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) töiden toteuttamisesta laitteen tiedot, telemetriatietojen ja säännöt logiikan ratkaisun näkyviin.

Kolme stream analytics työt ja niiden syntaksi on kuvattu syvyyttä [Remote seuranta esimääritettyjä ratkaisu vaiheittaisissa ohjeissa](iot-suite-remote-monitoring-sample-walkthrough.md). 

Voit muokata suoraan, jos haluat muuttaa logiikan työt tai tietyn logiikan lisääminen käyttämässäsi skenaariossa. Voit etsiä Stream Analytics työt seuraavasti:
 
1. Siirry [Azure-portaaliin](https://portal.azure.com).
2. Siirry saman niminen IoT ratkaisuksi resurssiryhmä. 
3. Valitse Azure Stream Analytics työ haluat muokata. 
4. Pysäytä työn valitsemalla **Lopeta**olevat komennot. 
5. Muokkaa tulojen, kysely ja tulostaa.

    Yksinkertainen muokkaus on kyselyn **säännöt** työn käyttämään **"<"** sijaan **">"**. Ratkaisu-portaalin näkyy edelleen **">"** kun säännön muokkaaminen, mutta huomaat toiminta on käännetty pohjana työn muutos vuoksi.

6. Aloita työ

> [AZURE.NOTE] Remote seurantaa Raporttinäkymät-ikkunan määräytyy tietyt tiedot, jotta töitä muuttaminen voi aiheuttaa Raporttinäkymät-ikkunan epäonnistuu.

## <a name="adding-your-own-rules"></a>Oman sääntöjen lisääminen

Lisäksi muuttaminen esimääritettyjä Azure Stream Analytics työt, voit käyttää Azure portaalin voit lisätä uusia töitä tai lisätä uusia kyselyjä olemassa olevat projektit.

## <a name="customizing-devices"></a>Laitteiden mukauttaminen

Yleisimmät tunniste toimia toimii laitteiden tietyn käyttämässäsi skenaariossa. Laitteiden käyttämisen eri tavoilla. Näistä tavoista lisätä muuttaminen vastaamaan käyttämässäsi skenaariossa Simuloitu laitteen tai laitteen fyysinen muodostaminen ratkaisun [IoT laitteen SDK][] avulla.

Katso vaiheittaiset ohjeet laitteiden lisäämisessä remote seurantaa esimääritettyjä ratkaisu, [Iot Suite yhteyden laitteet](iot-suite-connecting-devices.md) ja [Remote seurantaa C SDK-malli](https://github.com/Azure/azure-iot-sdks/tree/master/c/serializer/samples/remote_monitoring) , joka on suunniteltu toimimaan remote seurantaa esimääritettyjä ratkaisu.

### <a name="creating-your-own-simulated-device"></a>Luoda omia Simuloitu laite

Remote seurantaa ratkaisun lähdekoodin (edellä mainitut) sisältyy, on .NET simulator. Tämä simulator on yksi valmisteltu ratkaisun osana, ja voit muuttaa eri metatietojen telemetriatietojen lähettäminen tai niihin vastaaminen eri komentoja.

Valmiiksi määritetyt simulator remote seurantaa esimääritettyjä ratkaisussa on jäähdytyslaite laite, joka tietokoneesta kuuluu äänimerkki lämpötilan ja kosteuden telemetriatietojen simulator [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) Projectissa voi muokata, kun olet forked GitHub säilö.

### <a name="available-locations-for-simulated-devices"></a>Käytettävissä olevat sijainnit Simuloitu laitteille

Oletusarvoiset sijainneista on Seattle/Redmond, Washington, Yhdysvallat. Voit muuttaa seuraavista paikoista [SampleDeviceFactory.cs][lnk-sample-device-factory].


### <a name="building-and-using-your-own-physical-device"></a>Luominen ja käyttäminen (fyysinen) laitteen

[Azure IoT SDK: T](https://github.com/Azure/azure-iot-sdks) avulla kirjastojen yhteyden monia laitteen tyypit (kielet ja käyttöjärjestelmät) IoT ratkaisuihin.

## <a name="modifying-dashboard-limits"></a>Muokkaaminen Raporttinäkymät-ikkunan rajoitukset

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Laitteiden näytetään Raporttinäkymät-ikkunan avattavasta määrä

Oletusarvo on 200. Voit muuttaa tätä numeroa [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Bing-kartan ohjausobjektissa näytettävä PIN määrä

Oletusarvo on 200. Voit muuttaa tätä numeroa [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Telemetriatietojen graph ajanjakso

Oletusarvo on 10 minuuttia. Voit muuttaa tätä [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-setting-up-application-roles"></a>Sovelluksen roolit määrittäminen manuaalisesti

Seuraavassa kerrotaan, miten **järjestelmänvalvoja** ja **vain luku** -sovelluksen roolit lisääminen esimääritettyjä ratkaisu. Huomaa, että esimääritetyt ratkaisut azureiotsuite.com-sivuston valmistelu jo sisältävät **järjestelmänvalvoja** ja **vain luku** -roolit.

**Vain luku** -roolin jäsenet näkevät raporttinäkymät ja laite luettelosta, mutta ei voi lisätä laitteet, muuttaa laitteen määritteet tai lähettää komentoja.  **Järjestelmänvalvojan** rooli jäsenillä on täydet oikeudet kaikki toiminnot ratkaisussa.

1. Siirry [Azure perinteinen portal][lnk-classic-portal].

2. Valitse **Active Directorysta**.

3. Napsauta käytit, kun ratkaisu valmisteltu AAD vuokraajan nimeä.

4. Valitse **sovellukset**.

5. Valitse sama esimääritettyjä ratkaisun nimi sovelluksen nimi. Jos et näe sovellus-luettelossa, valitse **sovellukset yritykseni omistaa** **Näytä** avattava ja napsauttaa valintamerkkiä.

6.  Valitse sivun alareunassa **Hallita luettelon** ja **Lataa luettelon**.

7. Tämä Lataa .json tiedoston paikallisessa tietokoneessa.  Avaa tiedosto muokattavaksi valittua tekstieditorissa.

8. .Json tiedoston kolmannella rivillä on seuraavat tiedot:

  ```
  "appRoles" : [],
  ```
  Korvaa tämän seuraavasti:

  ```
  "appRoles": [
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Administrator access to the application",
  "displayName": "Admin",
  "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
  "isEnabled": true,
  "value": "Admin"
  },
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Read only access to device information",
  "displayName": "Read Only",
  "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
  "isEnabled": true,
  "value": "ReadOnly"
  } ],
  ```

9. Tallenna päivitetty .json-tiedosto (Voit korvata aiemmin luotu tiedosto).

10.  Valitse sivun alareunassa Azure-hallinta-portaalin **Hallinta luettelon** sitten **Lataa luettelon** edellisessä vaiheessa .json-tiedoston lataaminen.

11. Olet lisännyt **järjestelmänvalvoja** ja **vain luku** -roolien nyt sovelluksen.

12. Määritä rooli käyttäjälle hakemistossa, katso [azureiotsuite.com oikeudet][lnk-permissions].

## <a name="feedback"></a>Palaute

Onko sinulla mukauttaminen haluat nähdä asianmukaiset tässä asiakirjassa? Lisää ominaisuus ehdotuksia [Käyttäjän ääni](https://feedback.azure.com/forums/321918-azure-iot)- tai kommentti tämän artikkelin ohjeiden mukaisesti. 

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja esimääritetyt ratkaisut diaesitysten-asetuksista on seuraavissa artikkeleissa:

- [Logiikan App yhdistäminen Azure IoT Suite Remote seuranta valmiiksi ratkaisu][lnk-logicapp]
- [Dynaaminen telemetriatietojen käyttäminen remote seurantaa esimääritettyjä ratkaisu][lnk-dynamic]
- [Laitteen tiedot metatiedot remote seurannassa valmiiksi ratkaisu][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[Laitteen IoT SDK-paketissa]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
