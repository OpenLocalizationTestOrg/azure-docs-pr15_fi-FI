<properties
    pageTitle="Paikallisen sovellukseen blob storage (Java) | Microsoft Azure"
    description="Opettele luomaan console-sovellus, jossa Lataa kuva Azure- ja näyttää kuvan selaimessa. Java-MALLIKOODEJA."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="on-premises-application-with-blob-storage"></a>Paikallisen sovellukseen Blob-objektien tallennustilaan

## <a name="overview"></a>Yleiskatsaus

Seuraavassa esimerkissä kerrotaan, miten voit Azure tallennustilan kuvat tallennetaan Azure. Tämän artikkelin koodi on console-sovellus, jossa Lataa kuva Azure ja luo sitten HTML-tiedosto, joka näyttää kuvan selaimessa.

## <a name="prerequisites"></a>Edellytykset

- Java Developer Kit (JDK), 1,6 tai uudempi versio on asennettu.
- Azure-SDK on asennettu.
- Java Azure-kirjastojen PURKKIIN ja sovellettavan riippuvuussuhteen tahansa tölkki on asennettu ja käyttää Java-kääntäjä muodosta polku on. Saat lisätietoja asentamisesta Java Azure-kirjastoja, [Lataa Java Azure-SDK](java-download-azure-sdk.md).
- Azure-tallennustilan tilin on määritetty. Tilin nimi ja tiliavain tiliavain tallennustilan tilin käytetään tämän artikkelin koodilla. Katso, [miten voit Luo tili tallennustilan](storage-create-storage-account.md#create-a-storage-account) lisätietoja tallennustilan tilin ja [Näytä ja kopioi tallennustilan pikanäppäinten](storage-create-storage-account.md#view-and-copy-storage-access-keys) luomista tietoja haetaan tili-näppäintä.

- Olet luonut paikallisen kuvatiedoston nimi tallennettu polku c:\\myimages\\image1.jpg. Voit myös muokata   **FileInputStream** konstruktoria esimerkissä käyttämään eri kuvan polku ja tiedostonimi.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Tiedoston lataaminen Azure-blob-säiliö avulla

Vaiheittaiset ohjeet on esitetty seuraavassa. Jos haluat ohittaa eteenpäin, koko koodi on jaettu tämän artikkelin.

Aloita tuonti Azure core tallennustilan luokat, Azure-blob-asiakasohjelman luokat, Java IO luokkia ja **URISyntaxException** luokan mukaan lukien koodi.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

Määritellä nimeltä **StorageSample**luokan ja Sisällytä Avaa hakasulje **{**.

    public class StorageSample {

**StorageSample** -luokkaa muuttujaa merkkijono, joka sisältää päätepisteen protokolla, tallennustilan tilin nimi ja tallennustilaa access avainta, Azure-tallennustilan tilin mukaisesti. Paikkamerkki-arvojen korvaaminen **oman\_tilin\_nimi** ja **oman\_tilin\_avaimen** oman tilin nimen ja tilin avain tarpeen mukaan.

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

Oman ilmoituksen **tärkeimmät**lisääminen, Sisällytä **Yritä** lohko ja Sisällytä tarvittavat Avaa sulkeiden **{**.

    public static void main(String[] args)
    {
        try
        {

Määritellä muuttujat seuraavanlaisen (kuvaukset ovat, miten niitä käytetään tässä esimerkissä):

-   **CloudStorageAccount**: alusta tilin objekti Azure-tallennustilan tilin nimi ja avaimen avulla ja Blob-objektien asiakkaan objektin luomiseen.
-   **CloudBlobClient**: käyttää blob-palvelun.
-   **CloudBlobContainer**: blob-säilö luomiseen käytetyt-BLOB-säilö luettelosta ja poista säilö.
-   **CloudBlockBlob**: paikallisen kuvatiedoston lataaminen säilön avulla.

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

Määritä **tili** muuttujan arvon.

    account = CloudStorageAccount.parse(storageConnectionString);

Määritä luku **serviceClient** muuttujan.

    serviceClient = account.createCloudBlobClient();

Määritä luku **säilö** muuttuja. Jäljempänä on kerrottu säilö **gettingstarted**viittaus.

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

Luo säilö. Tämä menetelmä luo säilö, jos sitä ei ole (ja palauttaa **arvon TOSI**). Jos säilö ole valittu, se palauttaa **Epätosi**. Vaihtoehtoinen **createIfNotExists** on **luominen** (joka voi palauttaa virheen, jos säilö on jo olemassa).

    container.createIfNotExists();

Voit määrittää anonyymin käytön säilö.

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

Viittaaminen estä Blob-objektien, jotka edustavat Blob-objektien tallennustilaan Azure.

    blob = container.getBlockBlobReference("image1.jpg");

Viittaaminen paikallisesti luomasi tiedosto, joka ladattava **Tiedosto** konstruktoria avulla. Varmista, että tiedosto on luotu ennen kuin suoritat koodi.

    File fileReference = new File ("c:\\myimages\\image1.jpg");

Lataa paikallisen tiedoston – puhelun **CloudBlockBlob.upload** -menetelmää. **CloudBlockBlob.upload** menetelmän ensimmäinen parametri on **FileInputStream** objekti, joka esittää paikallinen tiedosto, joka voi ladata Azure-tallennustilan. Toinen parametri on haluamasi kokoinen tavuina tiedoston.

    blob.upload(new FileInputStream(fileReference), fileReference.length());

Kutsu-niminen **MakeHTMLPage**, jotta basic HTML-sivu, jossa on avustaja-funktio ** &lt;kuva&gt; ** elementin tietolähteeseen asettaminen, joka on nyt Azure-tallennustilan tilin Blob-objektien. Tämän artikkelin käsitellään **MakeHTMLPage** koodi.

    MakeHTMLPage(container);

Tulostaa tilasanoma ja luotu HTML-sivun tietoja.

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

Sulje **Yritä** lohkon lisäämällä Sulje hakasulje: **}**

Käsittele seuraavin poikkeuksin:

-   **FileNotFoundException**: Voit ilmenee **FileInputStream** tai **FileOutputStream** konstruktoreja mukaan.
-   **StorageException**: on antanut Azure asiakkaan tallennustilan kirjaston mukaan.
-   **URISyntaxException**: Voit ilmenee **ListBlobItem.getUri** -menetelmällä.
-   **Poikkeus**: Yleinen poikkeus käsittely.

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Sulje **tärkeimmät** lisäämällä Sulje hakasulje: **}**

Luo **MakeHTMLPage** Luo perussivu HTML-menetelmää. Tässä menetelmässä on parametrin tyypin **CloudBlobContainer**, jota käytetään käydä läpi ladatut BLOB luettelo. Tätä menetelmää palauttaa poikkeukset tyypin **FileNotFoundException**, joka on antanut **FileOutputStream** konstruktoria ja **URISyntaxException**, joka voi olla virhe ilmenee, **ListBlobItem.getUri** -menetelmällä. Sisällytä avaava hakasulje, **{**.

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

Luo paikalliseen **index.html**-tiedostoon.

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

Kirjoittaa paikalliseen tiedostoon, lisääminen ** &lt;html&gt;**, ** &lt;otsikon&gt;**, ja ** &lt;tekstissä&gt; ** osat.

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

Käydä läpi ladatut BLOB luettelo. Kunkin Blob-objektien varten HTML-sivun luominen ** &lt;img&gt; ** elementti, joka on lähetetty URI blob, kun se on Azure-tallennustilan tilin sen **src** -määrite.
Vaikka lisäsit tässä esimerkissä vain yhden kuvan, jos olet lisännyt enemmän, koodi käydä ne kaikki.

Tässä esimerkissä oletetaan yksinkertaisuuden, kunkin ladatut blob on kuva. Jos olet päivittänyt BLOB kuin kuvia tai sivun BLOB sijaan estä BLOB-objektit, muuta koodin tarpeen mukaan.

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

Sulje ** &lt;tekstissä&gt; ** elementin ja ** &lt;html&gt; ** elementti.

    stream.println("</body>");
    stream.println("</html>");

Sulje paikallinen tiedosto.

    stream.close();

Sulje **MakeHTMLPage** lisäämällä Sulje hakasulje: **}**

Sulje **StorageSample** lisäämällä Sulje hakasulje: **}**

Seuraavassa on tässä esimerkissä koko koodi. Muokkaa paikkamerkin arvoja muista **oman\_tilin\_nimi** ja **oman\_tilin\_avain** käyttää tilin nimi ja tili-näppäintä.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

Lisäksi ladata paikallisen kuva Azure-tallennustilan, esimerkkikoodi luo paikallisen tiedoston namedindex.html, jonka voi avata selaimessa, voit tarkastella ladattuja kuva.

Koska koodi sisältää tilin nimi ja tiliavain tiliavain, varmista, että lähdekoodi on turvallinen.

## <a name="to-delete-a-container"></a>Säilön poistaminen

Koska perittävän tallennustilaa, haluat ehkä poistaa **gettingstarted** säilö, kun olet valmis tässä esimerkissä kokeileminen. Jos haluat poistaa säilön, käytä **CloudBlobContainer.delete** -menetelmää.

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

Soita **CloudBlobContainer.delete** menetelmää alustaminen **CloudStorageAccount**, **ClodBlobClient**ja **CloudBlobContainer** objektien prosessi on sama kuin **createIfNotExist** menetelmää. Seuraavassa on valmis esimerkki, joka poistaa **gettingstarted**säilö.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

Yleiskatsaus muiden blob storage luokat ja menetelmät Katso, [miten voit käyttää Java-Blob-objektien tallennustilaan](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Näistä linkeistä saat lisätietoja tallennustilan monimutkaisia tehtäviä.

- [Azure-tallennustilan SDK Java](https://github.com/azure/azure-storage-java)
- [Azure-tallennustilan asiakkaan SDK-ohje](http://dl.windowsazure.com/storage/javadoc/)
- [Azure liittyviä palveluja REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure-tallennustilan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/)
