<properties
 pageTitle="Sovelluskehittäjän opas - tiedoston lataaminen | Microsoft Azure"
 description="Azure IoT keskittimeen developer guide - IoT keskittimeen laitteesta tiedostojen lataamisesta"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="upload-files-from-a-device"></a>Lataa tiedostot laitteesta

## <a name="overview"></a>Yleiskatsaus

Mahdollisimman yksityiskohtaista [IoT keskittimeen päätepisteet] [ lnk-endpoints] artikkelissa laitteiden aloittaa tiedostojen lataaminen palvelimeen lähettämällä ilmoituksen kautta laitteen osoittava päätepisteen (**/devices/ {deviceId} / tiedostot**).  Kun laite ilmoittaa IoT-toiminto on valmis Lataa, IoT toiminto luo tiedoston latauksen odotussanomat, joka voi vastaanottaa palvelun osoittava päätepisteen (**/messages/servicebound/filenotifications**) – viesteinä.

Sen sijaan, että varastoimista viestejä kautta IoT-toiminnossa itse IoT keskittimeen toimii sijaan lähettäjä tiliin liittyvät Azure-tallennustilan. Laite pyytää tallennustilan tunnuksen, joka liittyy laitteen haluaa ladata tiedoston IoT-toiminnosta. Laitteen käyttää SAS URI-tiedoston lataaminen tallennustilan ja kun lataus on valmis laitteen lähettää ilmoituksen suorituksesta IoT keskittimeen. IoT keskittimeen varmistaa, että tiedosto on ladattu ja lisää sitten tiedoston lataamisen ilmoituksen uusi palvelu osoittava tiedoston ilmoituksen tekstiviesti päätepiste.

Ennen kuin voit ladata tiedoston IoT keskittimeen laitteesta, sinun on määritettävä oman keskittimeen liittämällä [Azure-tallennustilan] [ lnk-associate-storage] tilin siihen.

Laitteen voit sitten [alustaminen lataus] [ lnk-initialize] ja [Ilmoita IoT keskittimeen] [ lnk-notify] Kun lataus on valmis. Voit myös kun laite ilmoittaa IoT keskittimeen lataus on valmis, palvelu voi luoda [ilmoituksen][lnk-service-notification].

### <a name="when-to-use"></a>Käyttäminen

Käyttää IoT keskittimeen tätä toimintoa, kun haluat ladata tiedoston laitteesta taustatietokantaan-palveluun sijaan kautta IoT keskittimeen viestin lähettämistä.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Azure-tallennustilan tilin liittäminen IoT-toiminnossa

Voit käyttää tiedoston lataaminen-toimintoa linkki Azure-tallennustilan tilin ensin IoT keskittimeen. Voit tehdä tämän joko [Azure portal][lnk-management-portal], tai ohjelmallisesti [IoT keskittimeen resurssin tarjoajaan REST API][lnk-resource-provider-apis]. Kun liittämäsi Azure-tallennustilan tilin IoT-keskittimeen, palvelu palauttaa SAS URI laitteeseen, kun laite aloittaa tiedoston lataamisen pyynnön.

> [AZURE.NOTE] [Azure IoT keskittimeen SDK: T] [ lnk-sdks] käsitellä automaattisesti haetaan SAS URI, tiedoston lataaminen ja ilmoittamalla IoT-toiminto on valmis Lataa.

## <a name="initialize-a-file-upload"></a>Tiedostojen lataamisen alusta

IoT toiminnosta on päätepiste erityisesti laitteiden pyytää tallennuksessa SAS URI Lataa tiedosto. Laitteen aloittaa tiedoston lataus lähettämällä VIESTIIN IoT-toiminnossa `{iot hub}.azure-devices.net/devices/{deviceId}/files` kanssa JSON jotakin seuraavista:

```
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IoT keskittimeen palauttaa seuraavia toimintoja, joita laite käyttää ensin lataamaan tiedosto:

```
{
    "correlationId": "somecorrelationid",
    "hostname": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Poistetuista: tiedostojen lataamisen, jossa GET alusta

> [AZURE.NOTE] Tässä osassa kuvataan saaminen SAS URI IoT-toiminnosta poistetuista-toimintoja. Käytä edellä kuvatun POST-menetelmää.

IoT toiminnosta on muiden päätepisteet tukemaan tiedostojen lataamisen, yksi saadaksesi SAS URI tallennus-ja toinen ilmoittamaan valmiit Lataa IoT-toiminnossa. Laitteen aloittaa tiedoston lataus lähettämällä GET-palvelussa IoT-toiminnossa `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. Valitsemalla palauttaa SAS URI tietyn ladattava tiedosto ja Korrelaatiotunnus, jota käytetään, kun lataus on valmis.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Ilmoita IoT-toiminto on valmis tiedoston lataaminen

Laite on vastuussa tiedoston lataaminen tallennustilan käyttämällä Azure tallennustilan SDK: T. Kun lataus on valmis, laite lähettää VIESTIIN IoT keskittimeen `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` kanssa JSON jotakin seuraavista:

```
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Arvon `isSuccess` on totuusarvo tiedosto on ladattu vai ei. Tilakoodi `statusCode` tallennus-tiedoston latauksen tila ja `statusDescription` vastaa `statusCode`.

## <a name="reference-topics"></a>Viittaus aiheet:

Viittaus seuraavissa ohjeaiheissa on lisätietoja laitteesta tiedostojen lataamisesta.

## <a name="file-upload-notifications"></a>Tiedoston latauksen odotussanomat

Kun laite lataa tiedoston, ja ilmoittaa IoT keskittimeen Lataa suorituksesta, palvelu luo myös ilmoitus, joka sisältää tiedoston nimi ja tallennustilaa sijainti.

[Päätepisteet]esitetyllä tavalla[lnk-endpoints], IoT keskittimeen toimittaa tiedoston latauksen odotussanomat palvelun osoittava päätepisteen (**/messages/servicebound/fileuploadnotifications**) – viesteinä. Vastaanota semantiikkaan liittyvien tiedoston latauksen odotussanomat varten ovat samat kuin cloud laitteen viestit ja saman [viestin elinkaari]on[lnk-lifecycle]. Noutaa tiedoston lataamisen ilmoitus päätepisteen viesti on JSON-tietue, jossa seuraavat ominaisuudet:

| Ominaisuus | Kuvaus |
| -------- | ----------- |
| EnqueuedTimeUtc | Aikaleima, joka ilmoittaa, kun ilmoituksessa, joka on luotu. |
| DeviceId | **DeviceId** laitteella, joka on ladattu tiedosto. |
| BlobUri | Ladatun tiedoston URI. |
| BlobName | Ladatun tiedoston nimi. |
| LastUpdatedTime | Aikaleima, joka ilmoittaa, kun tiedosto on päivitetty. |
| BlobSizeInBytes | Ladatun tiedoston kokoa. |

**Esimerkki**. Tämä on tiedoston lataamisen ilmoitusviestin Esimerkki-tekstiosaan.

```
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Tiedoston lataaminen ilmoituksen kokoonpanoasetusten määrittäminen

Kunkin IoT-toiminnossa paljastaa tiedoston latauksen odotussanomat määritysten seuraavista vaihtoehdoista:

| Ominaisuus | Kuvaus | Alue- ja oletusarvo |
| -------- | ----------- | ----------------- |
| **enableFileUploadNotifications** | Määrittää, onko tiedoston latauksen odotussanomat kirjoitetaan tiedoston ilmoitukset päätepiste. | Bool. Oletus: TOSI. |
| **fileNotifications.ttlAsIso8601** | Tiedoston latauksen odotussanomat (TTL) oletusarvo. | ISO_8601 välin 48 H ylöspäin (vähintään 1 minuutti). Oletusarvo: 1 tunti. |
| **fileNotifications.lockDuration** | Lukitse tiedoston lataamisen ilmoitukset jonossa kesto. | 5 – 300 sekunnin (vähintään viisi sekuntia). Oletusarvo: 60 sekuntia. |
| **fileNotifications.maxDeliveryCount** | Suurin toimituksen Laske tiedoston lataaminen ilmoituksen jonossa. | 1 – 100. Oletus: 100. |

## <a name="additional-reference-material"></a>Lisää aineistoa

Kehittäjän opas muiden viittauksen aiheet ovat seuraavat:

- [IoT keskittimeen päätepisteet] [ lnk-endpoints] kuvataan eri päätepisteet, jotka kunkin IoT-toiminnossa paljastaa avaamiseen runtime ja hallintaa varten.
- [Rajoitusten ja kiintiöiden] [ lnk-quotas] kuvataan kiintiön, jotka koskevat IoT keskittimeen-palveluun ja rajoittava toiminnan toiminta palvelua käytettäessä.
- [IoT keskittimeen laite- ja palveluluettelon SDK: T] [ lnk-sdks] näyttää eri kielellä SDK: T voit Käytä tätä, kun laite ja palvelun sovelluksista, jotka toimivat IoT keskittimeen ja kehittää.
- [Kyselyn kielen twins, menetelmistä ja töiden] [ lnk-query] kuvataan kyselykieltä, voit hakea tietoja IoT-toiminnosta laitteen twins, menetelmät ja projektit.
- [IoT keskittimeen MQTT tuki] [ lnk-devguide-mqtt] on lisätietoja IoT keskittimeen tuki MQTT-protokollaa.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt voit ladata tiedostoja käyttämällä IoT keskittimeen laitteilla olet oppinut, voi olla kiinnostuneita Sovelluskehittäjän opas seuraavissa ohjeaiheissa:

- [Laitteen IoT toiminnossa käyttäjätietojen hallinta][lnk-devguide-identities]
- [IoT pääsivuston käyttöoikeuksien hallinta][lnk-devguide-security]
- [Synkronoinnin tilan ja määritykset laitteen twins avulla][lnk-devguide-device-twins]
- [Suora menetelmän laitteessa][lnk-devguide-directmethods]
- [Ajoita työt useilla eri laitteilla][lnk-devguide-jobs]

Jos haluat kokeilla osa käsitteistä, joka on kuvattu tämän artikkelin, voit tarkastella käyttämällä seuraavia IoT keskittimeen opetusohjelma:

- [Pilveen kanssa IoT keskittimeen laitteilla tiedostojen lataaminen][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
