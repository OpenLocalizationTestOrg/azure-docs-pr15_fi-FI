<properties
 pageTitle="Azuren tallennustilaan blob sisällön Azure CDN vanhenemista hallita | Microsoft Azure"
 description="Tutustu erilaisiin ohjaukseen varten BLOB-Azure CDN välimuistiin tallentamisen aika-to-live."
 services="cdn"
 documentationCenter=""
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="multiple"
 ms.topic="article"
 ms.date="09/15/2016"
 ms.author="casoper"/>


# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>Voimassaolon Azure CDN Azuren tallennustilaan blob sisällön hallinta

> [AZURE.SELECTOR]
- [Azure Web Apps-/ pilvipalveluihin, ASP.NET tai IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure Blob-objektien tallennuspalvelu](cdn-manage-expiration-of-blob-content.md)

[Azuren](../storage/storage-introduction.md) tallennustilaan [blob-palvelu](../storage/storage-introduction.md#blob-storage) on useita Azure-pohjaiset alkuperistä Azure CDN integroitu.  Kaikkien käytettävissä blob sisältöä voidaan välimuistiin Azure CDN, kunnes sen--elinaika (TTL) kuluu.  TTL määräytyy Azuren tallennustilaan HTTP-vastauksen [ *Välimuistin hallinnan* otsikko](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) .

>[AZURE.TIP] Voit halutessasi määrittää ei TTL blob.  Tässä tapauksessa Azure CDN koskee automaattisesti oletusarvoista TTL seitsemän päivän.
>
>Lisätietoja Azure CDN osaa voidaan käyttää nopeuttaa BLOB-objektit ja muut tiedostot on artikkelissa [Azure CDN yleiskatsaus](./cdn-overview.md).
>
>Lisätietoja Azure-tallennustilan blob-palvelusta on artikkelissa [Blob-palvelun käsitteitä](https://msdn.microsoft.com/library/dd179376.aspx). 

Tässä opetusohjelmassa esitellään useita tapoja, joilla voit määrittää TTL Blob-objektien Azuren tallennustilaan.  

## <a name="azure-powershell"></a>Azure PowerShell

[PowerShellin Azure](../powershell-install-configure.md) on nopein, tehokas tapa Azure palvelujen hallintaan.  Käytä `Get-AzureStorageBlob` cmdlet Blob-objektien viittaaminen Aseta `.ICloudBlob.Properties.CacheControl` ominaisuus. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

>[AZURE.TIP] Voit käyttää myös PowerShell [CDN-profiileista](./cdn-manage-powershell.md)ja päätepisteet.

## <a name="azure-storage-client-library-for-net"></a>.NET Azure tallennustilan asiakkaan kirjasto

Voit määrittää Blob-objektien TTL käyttämällä .NET-ominaisuuden [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) [Azure tallennustilan asiakkaan kirjasto .NET](../storage/storage-dotnet-how-to-use-blobs.md) avulla.

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
        
        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

>[AZURE.TIP] [Azure Blob Storage näytteiden .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)käytettävissä on monia muita .NET MALLIKOODEJA.

## <a name="other-methods"></a>Muita menetelmiä

- [Azure käyttöliittymä](../xplat-cli-install.md)

    Kun lataamista blob, Määritä *cacheControl* -ominaisuuden avulla `-p` vaihtaminen.  Tässä esimerkissä TTL 1 tunti (3 600 sekuntia).

    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

- [Azure liittyviä palveluja REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dd179355.aspx)

    Ominaisuus on määritetty *x-ms-blob-välimuistin-komponenttien* [Blob-objektien valitseminen](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx)ja [Sijoittaa ruutuluettelo](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx) [Blob-ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/azure/ee691966.aspx) pyynnön.

- Kolmannen osapuolen tallennustilan hallintatyökalut

    Kolmannen osapuolen joitakin Azuren tallennustilaan hallintatyökalut avulla voit määrittää ominaisuus *CacheControl* BLOB. 

## <a name="testing-the-cache-control-header"></a>*Välimuistin ohjausobjekti* otsikon testaaminen

Voit helposti tarkistaa oman BLOB TTL.  Selaimen [Kehitystyökalut](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)avulla Testaa yhteyttä blob on myös *Välimuisti-komponenttien* vastauksen otsikon.  Voit myös tarkastella vastauksen otsikot työkalu, kuten **wget**, [Postman](https://www.getpostman.com/)tai [Fiddler](http://www.telerik.com/fiddler) .

## <a name="next-steps"></a>Seuraavat vaiheet

- [Lue lisätietoja *Cache-Control* -otsikko](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
- [Opi hallitsemaan Azure CDN pilvipalvelussa sisällön vanheneminen](./cdn-manage-expiration-of-cloud-service-content.md)

