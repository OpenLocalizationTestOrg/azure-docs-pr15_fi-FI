<properties
    pageTitle="Säilöjen ja BLOB anonyymin luku käytön hallinta | Microsoft Azure"
    description="Opettele saataville säilöjen ja BLOB anonyymin käytön ja käyttää niitä ohjelmallisesti."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Säilöjen ja BLOB anonyymin luku käytön hallinta

## <a name="overview"></a>Yleiskatsaus

Oletusarvon mukaan vain tallennustilan tilin omistaja voi käyttää tallennustilan resursseja kyseisen tilin. Vain Blob-objektien tallennustilaan voidaan asettaa sallimaan anonyymi lukuoikeus säilö ja sen BLOB säilön käyttöoikeuksia niin, että voit myöntää oikeuden resursseja ilman jakaminen tiliavain.

Anonyymi käyttö on paras haluamaasi tiettyjä BLOB-objektit on aina käytettävissä luku anonyymin skenaarioita. Tarkemmin ohjausobjekti voit luoda jaetun access-allekirjoitus, joka mahdollistaa rajoitettu Edustajakäyttöoikeus käyttämällä eri käyttöoikeudet ja määritetyllä aikavälillä päälle. Saat lisätietoja jaettujen käytön allekirjoitusten luominen [Käyttämällä jaettu Access allekirjoitukset (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Säilöjen ja BLOB anonyymien käyttäjien käyttöoikeuksia

Oletusarvon mukaan säilön ja kaikki sen sisältämät BLOB voidaan käyttää vain tallennustilan tilin omistajan mukaan. Anonyymi käyttäjille lukuoikeudet säilön ja sen BLOB-objektit, voit määrittää, jotta yleisön säilö-käyttöoikeudet. Anonyymit käyttäjät voivat lukea BLOB kaikkien käytettävissä säilöön ilman todentamista pyynnön.

Säilöjen on säilö käyttöoikeuksien hallinta seuraavista vaihtoehdoista:

- **Täysi julkisen lukuoikeudet:** Säilön ja Blob-objektien tietoja voi lukea anonyymi pyynnön kautta. Asiakkaiden voi luetteloida BLOB säilöön kautta anonyymi pyynnön, mutta ei voi luetteloida säilöjen tallennustilan tilin.

- **BLOB vain julkisen lukuoikeus:** Tämän säilön BLOB-tietoja voi lukea kautta anonyymi pyynnön, mutta säilö tiedot eivät ole käytettävissä. Asiakkaiden ei luetteloi BLOB säilöön anonyymi pyynnön kautta.

- **Ei ole yleistä lukuoikeus:** Säilön ja Blob-objektien tietoja voidaan lukea vain tilin omistajan.

Voit määrittää säilön käyttöoikeutta seuraavilla tavoilla:

- [Azure portaaliin](https://portal.azure.com).
- Ohjelmallisesti käyttämällä tallennustilan asiakas-kirjaston tai REST-Ohjelmointirajapinnalla.
- PowerShellin avulla. Lisätietoja PowerShellin Azure-säilö käyttöoikeuksien määrittämisestä on artikkelissa [Azure PowerShellin Azure-tallennustilan kanssa](storage-powershell-guide-full.md#how-to-manage-azure-blobs).

### <a name="setting-container-permissions-from-the-azure-portal"></a>Azure-portaalista säilö käyttöoikeuksien määrittäminen

Määritä säilö käyttöoikeudet [Azure-portaalissa](https://portal.azure.com)seuraavasti:

1. Siirry tilisi tallennustilan Raporttinäkymät-ikkunan.
2. Säilön nimi luettelosta. Napsauttamalla nimeä paljastaa valitsemasi säilössä BLOB-objektit
3. **Käyttöoikeuskäytäntö** työkalurivistä.
4. **Access-laji** -kentässä Valitse yhteyttä haluamasi käyttöoikeustaso, kuten alla olevassa näyttökuvassa.

    ![Muokkaa säilö metatiedot-valintaikkuna](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="setting-container-permissions-programmatically-using-net"></a>Säilön ohjelmallisesti käyttämällä .NET oikeuksien asettaminen

Säilön käyttämällä .NET-asiakasohjelman kirjaston käyttöoikeuksien määrittäminen ensin hakea säilö aiempien käyttöoikeuksien **GetPermissions** -menetelmällä. Aseta **PublicAccess** -ominaisuuden **BlobContainerPermissions** objektille, joka palautetaan **GetPermissions** -menetelmällä. Kutsu-päivitetyt oikeuksilla **luetteloa** -menetelmää.

Seuraavassa esimerkissä asettaa säilön käyttöoikeutta koko julkisen lukuoikeus. Käyttöoikeuksien määrittämisen BLOB julkisen lukuoikeudet Aseta vain **PublicAccess** -ominaisuuden arvoksi **BlobContainerPublicAccessType.Blob**. Jos haluat poistaa kaikki käyttöoikeudet anonyymeille käyttäjille, aseta-ominaisuuden arvoksi **BlobContainerPublicAccessType.Off**.

    public static void SetPublicContainerPermissions(CloudBlobContainer container)
    {
        BlobContainerPermissions permissions = container.GetPermissions();
        permissions.PublicAccess = BlobContainerPublicAccessType.Container;
        container.SetPermissions(permissions);
    }

## <a name="access-containers-and-blobs-anonymously"></a>Säilöjen ja BLOB käyttää nimettömänä

Asiakas, joka käyttää säilöjen ja BLOB nimettömänä voi käyttää konstruktoria, joka ei edellytä tunnistetietoja. Seuraavissa esimerkeissä muutamalla eri tavalla päivämääräviitettä Blob palveluresursseja anonyymisti.

### <a name="create-an-anonymous-client-object"></a>Anonyymi asiakas-objektin luominen

Voit luoda uuden asiakas objektin anonyymin antamalla tilin Blob-palvelupäätepiste. Sinun on myös tiedettävä kyseisessä tilissä, joka on käytettävissä anonyymin käytön säilön nimi.

    public static void CreateAnonymousBlobClient()
    {
        // Create the client object using the Blob service endpoint.
        CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

        // Read the container's properties. Note this is only possible when the container supports full public read access.
        container.FetchAttributes();
        Console.WriteLine(container.Properties.LastModified);
        Console.WriteLine(container.Properties.ETag);
    }

### <a name="reference-a-container-anonymously"></a>Viittaus säilön nimettömänä

Jos sinulla on säilön nimettömänä käytettävissä oleva URL-osoite, voit viitata säilö suoraan.

    public static void ListBlobsAnonymously()
    {
        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

        // List blobs in the container.
        foreach (IListBlobItem blobItem in container.ListBlobs())
        {
            Console.WriteLine(blobItem.Uri);
        }
    }


### <a name="reference-a-blob-anonymously"></a>Viittaus blob nimettömänä

Jos sinulla on Blob-objektien, joka on käytettävissä anonyymin käytön URL-osoite, voit viitata Blob-objektien käyttäminen suoraan, että URL-osoite:

    public static void DownloadBlobAnonymously()
    {
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
        blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
    }

## <a name="features-available-to-anonymous-users"></a>Anonyymien käyttäjien käytettävissä olevat ominaisuudet

Seuraavasta taulukosta näet, mitkä toiminnot kun säilön Käyttöoikeusluettelon on määritetty sallimaan yleisön anonyymit käyttäjät voidaan kutsua.

| REST-toiminto                                         | Täysi julkisen lukuoikeus käyttöoikeudet | Vain BLOB julkisen lukuoikeus käyttöoikeudet |
|--------------------------------------------------------|-----------------------------------------|---------------------------------------------------|
| Luettelon säilöt                                        | Vain omistaja                              | Vain omistaja                                        |
| Luo säilö                                       | Vain omistaja                              | Vain omistaja                                        |
| Hanki säilö ominaisuudet                               | Kaikki                                     | Vain omistaja                                        |
| Hanki säilö metatiedot                                 | Kaikki                                     | Vain omistaja                                        |
| Määritä säilö metatiedot                                 | Vain omistaja                              | Vain omistaja                                        |
| Hanki säilö Käyttöoikeusluettelon                                      | Vain omistaja                              | Vain omistaja                                        |
| Säilön Käyttöoikeusluettelo määrittäminen                                      | Vain omistaja                              | Vain omistaja                                        |
| Poista säilö                                       | Vain omistaja                              | Vain omistaja                                        |
| Luettelon BLOB-objektit                                             | Kaikki                                     | Vain omistaja                                        |
| Blob-objektien valitseminen                                               | Vain omistaja                              | Vain omistaja                                        |
| Hae Blob                                               | Kaikki                                     | Kaikki                                               |
| Hae Blob-ominaisuudet                                    | Kaikki                                     | Kaikki                                               |
| Blob-ominaisuuksien määrittäminen                                    | Vain omistaja                              | Vain omistaja                                        |
| Hae Blob-metatiedot                                      | Kaikki                                     | Kaikki                                               |
| Määritä Blob-metatiedot                                      | Vain omistaja                              | Vain omistaja                                        |
| Sijoita estäminen                                              | Vain omistaja                              | Vain omistaja                                        |
| Hae lohkoluettelo (vain vahvistettu estää)                 | Kaikki                                     | Kaikki                                               |
| Hae lohkoluettelo (ei ole lohkot tai kaikki lohkot) | Vain omistaja                              | Vain omistaja                                        |
| Sijoita ruutuluettelo                                         | Vain omistaja                              | Vain omistaja                                        |
| Blob-objektien poistaminen                                            | Vain omistaja                              | Vain omistaja                                        |
| Blob-objektien kopioiminen                                              | Vain omistaja                              | Vain omistaja                                        |
| Tilannevedoksen Blob                                          | Vain omistaja                              | Vain omistaja                                        |
| Varaus-Blob                                             | Vain omistaja                              | Vain omistaja                                        |
| Lisää sivu                                               | Vain omistaja                              | Vain omistaja                                        |
| Hae alueet                                        | Kaikki                                     | Kaikki                                                  |
| Blob-objektien liittäminen                                            | Vain omistaja                              | Vain omistaja                                                  |


## <a name="see-also"></a>Katso myös

- [Azure-tallennustilan-palveluiden todennus](https://msdn.microsoft.com/library/azure/dd179428.aspx)
- [Jaettu käyttö allekirjoitusten (SAS) käyttäminen](storage-dotnet-shared-access-signature-part-1.md)
- [Valtuutettu käytön jaetun Access-allekirjoitus](https://msdn.microsoft.com/library/azure/ee395415.aspx)
