<properties
 pageTitle="Tuo Vie IoT keskittimeen laitteen käyttäjätiedoista | Microsoft Azure"
 description="Käsitteitä ja .NET-koodiin katkelmat joukkona IoT keskittimeen laitteen käyttäjätietojen hallintaa varten"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

# <a name="bulk-management-of-iot-hub-device-identities"></a>Joukkosähköposti IoT keskittimeen laitteen käyttäjätietojen hallinta

Kunkin IoT-toiminnossa on laitteen tunnistetietojen rekisterin avulla voit luoda laitteen kohti resurssit-palvelun, kuten jono, joka sisältää tapahtumakartoitus cloud laitteen viestejä. Laitteen tunnistetietojen rekisterin avulla voit hallita laitteen osoittava päätepisteet. Tässä artikkelissa käsitellään tuominen ja vieminen laitteen käyttäjätietojen kerralla, ja laitteen tunnistetietojen rekisteristä.

Tuominen ja vieminen toimintojen take paikka *työt* , joiden avulla voit suorittaa joukkona palvelun toimintojen vastaan IoT-toiminnossa kontekstissa.

**RegistryManager** -luokkaan kuuluvat **ExportDevicesAsync** ja **ImportDevicesAsync** menetelmiä, jotka käyttävät **työn** puitteissa. Näistä tavoista avulla voit viedä, tuominen ja synkronoiminen IoT keskittimeen laite-rekisterin kokonaisuudessaan.

## <a name="what-are-jobs"></a>Mitkä ovat työt?

Laitteen tunnistetietojen rekisterin toiminnot käyttämällä **työ** -järjestelmän kun toiminto:

*  On mahdollisesti pitkä suoritusaika verrattuna vakio suorituksenaikainen toimintoja, tai
*  Palauttaa käyttäjän suuria määriä tietoa.

Tällaisissa tapauksissa sen sijaan, että yhden API-kutsu odottaa tai esto toiminnon tulos-toiminto luo asynkronisesti **työn** kyseisen IoT-toiminnossa. Valitse toiminto palauttaa heti **JobProperties** objektin.

Seuraavat C#-koodikatkelman näytetään, miten Luo vientityö:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

Voit sitten kysely palautettu **JobProperties** metatietojen **projektin** tila **RegistryManager** -luokka.

Seuraavat C#-koodikatkelman näytetään, miten lisääminen viiden sekunnin välein nähdäksesi, jos työ on suoritettu suoritetaan:

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Vie laitteet

Vie IoT keskittimeen laite-rekisterin kokonaisuudessaan Blob-objektien [Azuren tallennustilaan](https://azure.microsoft.com/documentation/services/storage/) -säilöön [Jaetut Access allekirjoituksen](https://msdn.microsoft.com/library/ee395415.aspx) **ExportDevicesAsync** menetelmän avulla.

Tätä menetelmää avulla voit luoda luotettava varmuuskopioiden laitteen tietojen blob-säilö, joka voidaan hallita.

**ExportDevicesAsync** -menetelmässä on kaksi parametria:

*  *Merkkijono* , joka sisältää URI blob-säilöstä. Tämä URI on oltava SAS-tunnuksen, joka antaa säilö kirjoitusoikeudet. Työn Luo estä Blob-objektien tähän säilöön sarjoitettu Vie laitteen tietojen tallennusta varten. SAS tunnuksen on oltava seuraavat oikeudet:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

*  *Totuusarvo* , joka ilmaisee, jos haluat jättää pois todennus näppäimet Vie tiedoista. Jos **Epätosi**, avaimet sisältyvät Vie todennus tulosteen; Muussa tapauksessa avaimet viedään **null**.

Seuraavat C#-koodikatkelman esitetään, kuinka voit aloittaa vientityö, joka sisältää laitteen todennus näppäimet tietojen vienti ja sitten äänestyksen järjestäminen käyttöönotto:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

Työn tallentaa tulos annettu blob-säilö estä Blob-objektien kanssa nimi **devices.txt**. Tulosta tiedot koostuu JSON muuntaa sarjaksi laitteen tiedot yhteen laitteeseen riviä kohden.

Seuraavassa esimerkissä esitetään tiedot:

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Jos tarvitset tietojen koodissa, voit helposti poistaa nämä tiedot **ExportImportDevice** -luokan avulla. Seuraavat C#-koodikatkelman esitetään, kuinka voit lukea laitteen tiedot, jotka on aiemmin viety estä Blob-objektien:

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [AZURE.NOTE]  Voit käyttää myös **RegistryManager** -luokan **GetDevicesAsync** -menetelmä hakeaksesi laitteet luettelo. Tämän menetelmän on kuitenkin vaikea pää 1000 laitteen objekteja, jotka palautetaan määrän. Odotettu käyttötapaus **GetDevicesAsync** menetelmän on kehittäminen skenaariot tukeen virheenkorjaus ja tuotannon työmääriä ei suositella.

## <a name="import-devices"></a>Tuo laitteet

**RegistryManager** -luokan **ImportDevicesAsync** menetelmän avulla voit suorittaa joukkona tuominen ja synkronointi IoT keskittimeen laite-rekisterissä. Kuten **ExportDevicesAsync** -menetelmä **ImportDevicesAsync** tapa käyttää **työn** puitteissa.

Huolehtia **ImportDevicesAsync** menetelmällä, koska lisäksi valmistelu uudet laitteet laitteen tunnistetietojen rekisterissä, se voi myös päivittää ja poistaa aiemmin laitteet.

> [AZURE.WARNING]  Tuontitoimintoa ei voi kumota. Muista aina varmuuskopioida tiedot käyttämällä **ExportDevicesAsync** menetelmä blob-säilö, ennen kuin teet joukkomuutoksia laitteen tunnistetietojen rekisteriä.

**ImportDevicesAsync** menetelmä on kaksi parametria:

*  *Merkkijono* , joka sisältää URI [Tallennustilan Azure](https://azure.microsoft.com/documentation/services/storage/) -Blob-objektien säilöstä käyttää *syötteen* projektille. Tämä URI on oltava SAS-tunnuksen, joka antaa lukuoikeudet säilöön. Tämä säilö on oltava Blob-objektien kanssa nimi **devices.txt** , joka sisältää laitteen tunnistetietojen rekisteriä tuominen sarjoitettu laitteen tiedot. Tietojen tuominen on oltava sama JSON-muotoon, joka **ExportImportDevice** työn käyttää, kun se luo **devices.txt** Blob-objektien laitteen tiedot. SAS tunnuksen on oltava seuraavat oikeudet:

    ```
    SharedAccessBlobPermissions.Read
    ```

*  *Merkkijono* , joka sisältää URI [Tallennustilan Azure](https://azure.microsoft.com/documentation/services/storage/) -Blob-objektien säilöstä käytettävä *tulostus* työstä. Työn Luo estä Blob-objektien tähän säilöön tallentamiseen valmistunut tuonti **työn**virhe mitään tietoja. SAS tunnuksen on oltava seuraavat oikeudet:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

> [AZURE.NOTE]  Kaksi parametria voit osoittaa saman blob-säilö. Erillinen parametrit käyttöön vain tiedot haluat hallita tulosteen säilö vaatii lisäoikeuksia.

Seuraavat C#-koodikatkelman näkyy Tuo projektin aloittaminen:

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>Tuo toiminta

**ImportDevicesAsync** menetelmän avulla voit suorittaa seuraavat joukkona toimet laitteen tunnistetietojen rekisteriä:

-   Uusien tiedostojen rekisteröinti samalla kertaa
-   Olemassa olevan laitteiden poistot samalla kertaa
-   Joukkosähköposti tilamuutosten (ottaminen käyttöön tai poistaminen käytöstä laitteet)
-   Uuden laitteen todennus näppäinyhdistelmien varauksen samalla kertaa
-   Laitteen todennus avaimet Automaattinen Uudistaminen joukkosähköposti

Voit suorittaa edellisen toimintojen sisällä yhden **ImportDevicesAsync** puhelun yhdistelmän. Voit esimerkiksi rekisteröidä uusia laitteita ja poistaa tai päivittää aiemmin laitteiden samanaikaisesti. Käytettäessä yhdessä **ExportDevicesAsync** menetelmä toiseen voi siirtää kokonaan kaikissa laitteissa yhden IoT-toiminnossa.

Valitse kunkin laitteen tietojen tuominen Sarjatoiminto valinnainen **ImportMode on %** -ominaisuuden avulla voit määrittää prosessin kohden-laite. **ImportMode on %** -ominaisuus on seuraavat vaihtoehdot:

| ImportMode on % |  Kuvaus |
| -------- | ----------- |
| **createOrUpdate** | Laite ei ole määritetty **tunnus**, jos se on juuri rekisteröity. <br/>Jos laite on jo, olemassa olevia tietoja korvautuu annettu syöttötiedot riippumatta siitä, missä **ETag** -arvo. |
| **Luo** | Laite ei ole määritetty **tunnus**, jos se on juuri rekisteröity. <br/>Jos laite on jo olemassa, virheen kirjoitetaan lokitiedostoon. |
| **päivittäminen** | Jos laite on jo määritetty **tunnus**, olemassa olevia tietoja korvautuu annettu syöttötiedot riippumatta siitä, missä **ETag** -arvo. <br/>Jos laite ei ole, virheen kirjoitetaan lokitiedostoon. |
| **updateIfMatchETag** | Jos laite on jo määritetty **tunnus**, olemassa olevia tietoja korvautuu annettu syöttötiedot vain silloin, kun ei **ETag** tarkkaa vastinetta. <br/>Jos laite ei ole, virheen kirjoitetaan lokitiedostoon. <br/>Jos **ETag** ristiriita-virheen kirjoitetaan lokitiedostoon. |
| **createOrUpdateIfMatchETag** | Laite ei ole määritetty **tunnus**, jos se on juuri rekisteröity. <br/>Jos laite on jo, olemassa olevia tietoja korvautuu annettu syöttötiedot vain silloin, kun ei **ETag** tarkkaa vastinetta. <br/>Jos **ETag** ristiriita-virheen kirjoitetaan lokitiedostoon. |
| **Poista** | Jos laite on jo määritetty **tunnus**, se poistetaan riippumatta siitä, missä **ETag** -arvo. <br/>Jos laite ei ole, virheen kirjoitetaan lokitiedostoon. |
| **deleteIfMatchETag** | Jos laite on jo määritetty **tunnus**, se poistetaan vain silloin, kun ei **ETag** tarkkaa vastinetta. Jos laite ei ole, virheen kirjoitetaan lokitiedostoon. <br/>Jos ETag ristiriita-virheen kirjoitetaan lokitiedostoon. |

> [AZURE.NOTE] Jos Sarjatoiminto tiedot erikseen Määritä laitteen **ImportMode on %** merkinnän, oletusarvoisesti **createOrUpdate** tuontitoiminnon aikana.

## <a name="import-devices-example--bulk-device-provisioning"></a>Tuo laitteiden Esimerkki – joukkosähköposti laitteen käyttöönotto 

Seuraavat C# koodin otosten kuvataan, miten voit luoda useita laitteen käyttäjätietoja:

- Sisällytä todennus-näppäimiä.
- Kirjoittaa estä Blob-objektien laitteen tiedot.
- Tuo laitteen tunnistetietojen rekisterin laitteet.

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Tuo laitteiden Esimerkki – joukkona poistaminen

Seuraava koodi malli näytetään, miten voit poistaa lisäämäsi käyttämällä edellisen laitteet:

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>Käytön SAS URI-säilö


Seuraava koodi malli näytetään, miten voit luoda [SAS URI](../storage/storage-dotnet-shared-access-signature-part-2.md) luku, kirjoittaminen ja poista käyttöoikeuksien blob-säilö:

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa opit, voit suorittaa joukkona laitteen tunnistetietojen rekisteriä vastaan IoT-toiminnossa. Saat lisätietoja hallinnasta Azure IoT keskittimeen seuraavista linkeistä:

- [Käyttö-arvot][lnk-metrics]
- [Toimintojen seuranta][lnk-monitor]

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Sovelluskehittäjän opas][lnk-devguide]
- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md