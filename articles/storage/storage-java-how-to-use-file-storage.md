<properties
    pageTitle="Java-tiedostosäilön käyttämisestä | Microsoft Azure"
    description="Opettele käyttämään Azure tiedoston palvelun lähettämistä, lataa-luettelosta ja poista tiedostot. Esimerkkejä, joiden on kirjoitettu Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-file-storage-from-java"></a>Opi käyttämään Java-tiedostojen tallennus

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä oppaassa opit suorittamaan peruslaskutoimituksia Microsoft Azure tiedoston tallennustilan-palvelusta. Kirjoitettu Java esimerkkejä, joiden avulla opit luominen osakkeet ja hakemistojen, lataaminen, luettelon ja poista tiedostot. Jos ole aiemmin käyttänyt Microsoft Azure tiedoston tallennuspalvelu, tapoja, joilla käsitteitä loppuun on erittäin hyödyllistä tietoa mallit.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java-sovelluksen luominen

Voit luoda malleja, tarvitset Java Development Kit (JDK) ja [Azure tallennustilan SDK Java]-[]. Voit myös kannattaa luoda Azure-tallennustilan tilin.

## <a name="setup-your-application-to-use-file-storage"></a>Sovelluksen käyttäminen tiedostojen tallentamisesta asetukset

Azure-tallennustilan ohjelmointirajapinnan käyttämään Lisää seuraavan lauseen Java-tiedosto, jos aiot käyttää tallennuspalvelu-alkuun.

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.file.*;

## <a name="setup-an-azure-storage-connection-string"></a>Azure-tallennustilan-yhteysmerkkijonon asetukset

Tiedostojen tallentamisesta käyttämään haluat muodostaa yhteyden tiliisi Azure-tallennustilan. Voit määrittää yhteyden merkkijono, joka muodostaa yhteyden tiliisi tallennustilan Käytämme olisi ensimmäiseksi. Määritä staattinen muuttuja, voit tehdä seuraavaksi.

    // Configure the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account_name;" +
        "AccountKey=your_storage_account_key";

> [AZURE.NOTE] Korvaa your_storage_account_name ja your_storage_account_key todellisten arvojen tallennustilan tilissäsi.

## <a name="connecting-to-an-azure-storage-account"></a>Yhteyden muodostaminen Azure-tallennustilan tilin

Muodostaa tallennustilan tilin haluat käyttää **CloudStorageAccount** -objektia kulkeva yhteysmerkkijonon sen **jäsentää** -menetelmää.

    // Use the CloudStorageAccount object to connect to your storage account
    try {
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
    } catch (InvalidKeyException invalidKey) {
        // Handle the exception
    }

**CloudStorageAccount.parse** ilmoittaa InvalidKeyException, joten sinun täytyy ladata sen yritä/todellinen-lohkon.

## <a name="how-to-create-a-share"></a>Toimintaohje: Luo jaettu kansio

Kaikki tiedostot ja kansiot-tiedostojen tallentamisesta sijaitsevat säilön kutsutaan **Jaa**. Tallennustilan tilin voi olla mahdollisimman paljon osakkeet tilin kapasiteetti-toiminnolla. Voit hankkia jaettuun ja sen sisällön käyttöoikeus, tarvitset tiedostojen tallennustila-asiakasohjelman.

    // Create the file storage client.
    CloudFileClient fileClient = storageAccount.createCloudFileClient();

Tiedoston tallennustilan asiakkaan avulla voit hankkia sitten jaettuun viittaus.

    // Get a reference to the file share
    CloudFileShare share = fileClient.getShareReference("sampleshare");

Voit luoda itse asiassa Jaa käyttämällä CloudFileShare objektin **createIfNotExists** -menetelmää.

    if (share.createIfNotExists()) {
        System.out.println("New share created");
    }

Tässä vaiheessa **jakaminen** pitää viittauksen **sampleshare**-resurssiin.

## <a name="how-to-upload-a-file"></a>Toimintaohje: tiedoston lataaminen

Azure tiedostoresurssin tallennustilan sisältää vähintään, johon tiedostot voivat sijaita pääkansio. Tässä osassa opit paikallisesta tallennussijainnista sivulle pääkansiossa jaetun tiedoston lataaminen.

Ensimmäinen vaihe tiedoston lataamisessa on hankkiminen hakemiston viittaa, joissa se olisi sijaitsevat. Voit tehdä tämän jaetun objektin **getRootDirectoryReference** -menetelmällä.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

Nyt kun olet luonut jaetun kansion pääkansion viittaa, voit ladata tiedoston päälle käyttämällä seuraava koodi.

    // Define the path to a local file.
    final String filePath = "C:\\temp\\Readme.txt";

    CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
    cloudFile.uploadFromFile(filePath);

## <a name="how-to-create-a-directory"></a>Toimintaohje: Hakemiston luominen

Voit myös järjestää tallennustilan sijoittamalla alikansioiden sen sijaan, että ne kaikki pääkansiossa sisällä tiedostoja. Voit luoda mahdollisimman paljon kansioita, kuten tilisi sallii Azure tiedoston tallennuspalvelu. Seuraava koodi luo alikansiot hakemiston **sampledir** pääkansioon.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the sampledir directory
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    if (sampleDir.createIfNotExists()) {
        System.out.println("sampledir created");
    } else {
        System.out.println("sampledir already exists");
    }

## <a name="how-to-list-files-and-directories-in-a-share"></a>Toimintaohje: tiedostojen ja kansioiden jaettuun-luettelo

Tiedostojen ja kansioiden sisällä olevaa jaettua luettelo hankkiminen tehdään helposti kutsumalla **listFilesAndDirectories** CloudFileDirectory viittaus. Menetelmä palauttaa ListFileItem objekteja, jotka voit käydä-luettelo. Esimerkiksi seuraava koodi sisältyviä tiedostoja ja kansioita pääkansion sisällä.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
        System.out.println(fileItem.getUri());
    }


## <a name="how-to-download-a-file"></a>Toimintaohje: Lataa tiedosto

Jokin suoritat tallentamista vasten useammin toimintoja ei ladata tiedostoja. Seuraavassa esimerkissä koodin Lataa SampleFile.txt ja näyttää sen sisältöä.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the directory that contains the file
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    //Get a reference to the file you want to download
    CloudFile file = sampleDir.getFileReference("SampleFile.txt");

    //Write the contents of the file to the console.
    System.out.println(file.downloadText());

## <a name="how-to-delete-a-file"></a>Toimintaohje: tiedoston poistaminen

Toinen yleisiä tiedostojen tallennustila-toiminto on tiedoston poistaminen. Seuraava koodi poistaa SampleFile.txt **sampledir**hakemiston tallennetun tiedoston.


    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory where the file to be deleted is in
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    String filename = "SampleFile.txt"
    CloudFile file;

    file = containerDir.getFileReference(filename)
    if ( file.deleteIfExists() ) {
        System.out.println(filename + " was deleted");
    }


## <a name="how-to-delete-a-directory"></a>Toimintaohje: kansion poistaminen

Hakemiston poistetaan varsin yksinkertaisten tehtävän, vaikka huomattava ei voi poistaa kansio, joka sisältää silti tiedostot tai muihin kansioihin.

    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory you want to delete
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    // Delete the directory
    if ( containerDir.deleteIfExists() ) {
        System.out.println("Directory deleted");
    }


## <a name="how-to-delete-a-share"></a>Toimintaohje: jaettuun poistaminen

Poistaminen jaettuun tehdään **deleteIfExists** kutsumista CloudFileShare aluetta. Tässä on esimerkki koodi, joka toimii.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the file client.
       CloudFileClient fileClient = storageAccount.createCloudFileClient();

       // Get a reference to the file share
       CloudFileShare share = fileClient.getShareReference("sampleshare");

       if (share.deleteIfExists()) {
           System.out.println("sampleshare deleted");
       }
    } catch (Exception e) {
        e.printStackTrace();
    }

## <a name="next-steps"></a>Seuraavat vaiheet

Jos haluat lisätietoja muiden Azure tallennustilan API, noudata näitä linkkejä.

- [Java-kehityskeskus](http://azure.microsoft.com/develop/java/)
- [Azure-tallennustilan SDK Java](https://github.com/azure/azure-storage-java)
- [Azure-tallennustilan SDK androidille](https://github.com/azure/azure-storage-android)
- [Azure-tallennustilan asiakkaan SDK-ohje](http://dl.windowsazure.com/storage/javadoc/)
- [Azure liittyviä palveluja REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure-tallennustilan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/)
- [Siirrä tiedot ja AzCopy komentorivivalitsimet-apuohjelma](storage-use-azcopy.md)
