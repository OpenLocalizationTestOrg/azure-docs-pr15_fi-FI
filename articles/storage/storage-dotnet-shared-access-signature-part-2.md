<properties
    pageTitle="Luominen ja käyttäminen SAS Blob-objektien tallennustilaan | Microsoft Azure"
    description="Tässä opetusohjelmassa näytetään, miten voit luoda jaettu käyttö allekirjoitukset käytettäväksi Blob-objektien tallennustilaan ja niiden tarjoaman ne asiakas-sovelluksista."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Jaettu Access allekirjoituksia, osa 2: Luominen ja käyttäminen SAS Blob-objektien tallennustilaan

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa tarkasteltavia [osa 1](storage-dotnet-shared-access-signature-part-1.md) tallennettu access allekirjoitukset (SAS) ja selitetään parhaat käytännöt ne. Osa 2 avulla voit luoda ja sitten jaettua käyttöä allekirjoitusten käyttäminen Blob-objektien tallennustilaan. Esimerkkejä on kirjoitettu C# ja käyttää Azure-tallennustilan asiakkaan kirjaston .NET. Koskee skenaariot ovat jaettu käyttö allekirjoitusten käyttäminen seuraavia ominaisuuksia:

- Säilön jaettu käyttö allekirjoituksen luominen
- Blob jaettu käyttö allekirjoituksen luominen
- Voit hallita allekirjoitukset säilön resurssien tallennettu access käytännön luominen
- Asiakas-sovelluksesta jaetun access-allekirjoitukset testaaminen

## <a name="about-this-tutorial"></a>Tässä opetusohjelmassa tietoja

Tässä opetusohjelmassa tarkastellaan luominen jaettua käyttöä allekirjoitukset säilöjen ja BLOB luomalla kaksi console-sovelluksia. Ensimmäisen console-sovelluksen Luo jaettu käyttö allekirjoitukset säilön ja blob. Sovellus on tallennustilan tilin avaimet. Toisen console-sovellus, joka toimii asiakassovellus, käyttää säilö ja Blob-objektien resurssien ensimmäisen sovelluksen luotu jaettu käyttö allekirjoitusten käyttäminen. Sovellus käyttää jaettua käyttöä allekirjoitukset todentavat käyttöoikeuden säilöön ja blob-resurssit - se ei ole tiliä avaimet.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>Osa 1: Luo jaettu käyttö allekirjoitukset Console-sovelluksen luominen

Varmista ensin, että sinulla on asennettu .NET Azure tallennustilan asiakas-kirjasto. Voit asentaa uusin kokoonpanon asiakkaan; kirjaston sisältävän [NuGet paketti]-(http://nuget.org/packages/WindowsAzure.Storage/ "NuGet-paketti") Tämä on suositeltava tapa varmistaa, että sinulla on viimeisin korjaukset. Voit ladata sen osana uusinta versiota [Azure SDK.NET](https://azure.microsoft.com/downloads/)asiakirjakirjaston.

Visual Studiossa Luo uusi Windows console-sovellus ja anna sille nimi **GenerateSharedAccessSignatures**. Lisää viitteet **Microsoft.WindowsAzure.Configuration.dll** ja **Microsoft.WindowsAzure.Storage.dll**, jommallakummalla seuraavista tavoista:

-   Jos haluat asentaa NuGet-paketti, asenna [NuGet asiakas](https://docs.nuget.org/consume/installing-nuget). Valitse Visual Studion **projektin | Hallitse NuGet pakettien**, Etsi online-tilassa **Azuren tallennustilaan**ja noudata asennusohjeita.
-   Voit myös etsiä kokoonpanon Azure-SDK-asennuksesi ja lisätä viittaukset niihin.

Program.cs tiedoston yläosassa Lisää **käyttämällä** seuraavia komentoja:

    using System.IO;    
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Muokkaa app.config-tiedostoa, niin, että se sisältää yhteysmerkkijonolla, joka osoittaa tallennustilan tilin määritys. Tämä pitäisi muistuttaa app.config-tiedostoa:

    <configuration>
      <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
      </startup>
      <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
      </appSettings>
    </configuration>

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Luo jaettu käyttö allekirjoituksen URI säilö

Aloita lisäämme menetelmä luo uuden säilön jaettua käyttöä allekirjoituksen. Tässä tapauksessa allekirjoitus ei ole liitetty tallennetut käyttöoikeuskäytäntö, joten se toimii URI ilmaisee, että sen voimassaolon ajan ja käyttöoikeudet, jotka se antaa tietoja.

Lisää ensin **Main()** menetelmä, jolla todennetaan tallennustilan tiliäsi ja luoda uuden säilön koodi:

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Insert calls to the methods created below here...

        //Require user input before closing the console window.
        Console.ReadLine();
    }

Lisää uusi menetelmä luo jaettu käyttö allekirjoituksen säilö ja palauttaa allekirjoituksen URI:

    static string GetContainerSasUri(CloudBlobContainer container)
    {
        //Set the expiry time and permissions for the container.
        //In this case no start time is specified, so the shared access signature becomes valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List;

        //Generate the shared access signature on the container, setting the constraints directly on the signature.
        string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Lisää seuraavat rivit **Main()** -menetelmä ennen kutsun **Console.ReadLine()**, voit soittaa **GetContainerSasUri()** ja kirjoittaa allekirjoituksen URI konsoli-ikkunan alareunassa:

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

Käännä ja suorita voidaan siirtää jaetun access allekirjoituksen URI uusi säilö. URI on samankaltainen kuin seuraavassa URI:       

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D

Kun olet suorittanut koodin, jaetun access-allekirjoitus, jonka loit säilö on voimassa seuraavan 24 tuntia. Allekirjoituksen antaa luettelon BLOB-säilössä ja kirjoittaa uuden Blob-objektien säilöön asiakkaan käyttöoikeudet.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Luo jaettu käyttö allekirjoituksen URI Blob

Seuraavaksi on kirjoittaa samalla koodia, voit luoda uuden blob säilöön ja Luo jaettu käyttö allekirjoituksen. Jaettu käyttö allekirjoitus ei ole liitetty tallennetut käyttöoikeuskäytäntö, niin, että se sisältää alkamisaika, voimassaolon ajan ja käyttöoikeustiedot URI-tunnusta.

Lisää uusi menetelmä, joka luo uuden Blob-objektien ja kirjoita tekstiä, valitse Luo jaettu käyttö allekirjoituksen ja palauttaa allekirjoituksen URI:

    static string GetBlobSasUri(CloudBlobContainer container)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a Shared Access Signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Set the expiry time and permissions for the blob.
        //In this case the start time is specified as a few minutes in the past, to mitigate clock skew.
        //The shared access signature will be valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

        //Generate the shared access signature on the blob, setting the constraints directly on the signature.
        string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

**Main()** menetelmä alareunassa Lisää seuraavat rivit **GetBlobSasUri()**, Soita puhelu **Console.ReadLine()**ennen ja kirjoittaa jaetun access-allekirjoituksen URI konsoli-ikkunassa:    

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();


Käännä ja suorita tulosteen uusi Blob-objektien jaettua käyttöä allekirjoituksen URI. URI on samankaltainen kuin seuraavassa URI:

    https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D

### <a name="create-a-stored-access-policy-on-the-container"></a>Tallennetun Access-käytännön luominen-säilö

Nyt-säilö, joka määrittää rajoitukset, jotka liittyvät se jaettu käyttö minkä tahansa allekirjoitusten luominen tallennetut käyttöoikeuskäytäntö.

Edellisessä esimerkissä on määritetty alkamisajan (tai epäsuorasti), voimassaolon ajan ja käyttöoikeudet jaettuun käyttöön allekirjoituksen URI itse. Seuraavissa esimerkeissä on määrittää nämä tallennetut käyttöoikeuskäytäntö eikä jaetun access-allekirjoitus. Tällöin mahdollistaa että muuta rajoitteen määritettyyn jaetun access-allekirjoitus.

Se on mahdollista, että vähintään yksi seuraavista jaetun access-allekirjoituksen rajoituksia, ja jäännös tallennetun access-käytännön. Voit määrittää vain alkamisaika, voimassaolon ajan ja käyttöoikeudet yhdessä paikassa tai. Et voi esimerkiksi määrittää käyttöoikeudet jaettuun access-allekirjoitus ja määrittää myös ne tallennettu käyttöoikeuskäytäntö.

Huomaa, että kun lisäät käyttöoikeuskäytäntö säilöön, on säilö aiemmin oikeuksien saaminen, Lisää uusi access-käytäntö ja valitse käyttöoikeudet säilö.

Lisää uusi menetelmä, joka luo uuden tallennetun access-käytännön säilön ja palauttaa käytännön nimi:

    static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
        string policyName)
    {
        //Create a new shared access policy and define its constraints.
        SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
        };

        //Get the container's existing permissions.
        BlobContainerPermissions permissions = container.GetPermissions();

        //Add the new policy to the container's permissions, and set the container's permissions.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        container.SetPermissions(permissions);
    }

**Main()** -menetelmä ennen **Console.ReadLine()**puhelu alareunassa Lisää seuraavat rivit ensimmäinen Tyhjennä kaikki olemassa olevat käytäntöjen ja valitse Soita **CreateSharedAccessPolicy()** menetelmää:    

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

Huomaa, että kun tyhjennät säilön access-käytäntöjä, sinun on ensin säilö aiemmin oikeuksien saaminen ja sitten Poista käyttöoikeudet, sitten määrittämällä käyttöoikeudet uudelleen.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>Luo jaettu käyttö allekirjoituksen URI-säilöä, joka käyttää käyttöoikeuskäytäntö

Seuraavaksi on Luo säilö, joka on luotu aiemmin, mutta tällä hetkellä emme lisätä allekirjoituksen, joka on luotu edellisessä esimerkissä käyttöoikeuskäytäntö toiseen jaettuun käyttöön allekirjoitus.

Lisää uusi menetelmä säilön toiseen jaettuun käyttöön allekirjoituksen luomiseen:

    static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Generate the shared access signature on the container. In this case, all of the constraints for the
        //shared access signature are specified on the stored access policy.
        string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

**Main()** -menetelmä ennen **Console.ReadLine()**puhelu alareunassa Lisää seuraavat rivit **GetContainerSasUriWithPolicy** menetelmää:

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>Luo jaettu käyttö allekirjoituksen URI-Blob-objektien, joka käyttää käyttöoikeuskäytäntö

Lopuksi lisäämme toiseen Blob-objektien luomiseen ja Luo jaettu käyttö allekirjoitus, joka liittyy käyttöoikeuskäytäntö samalla menetelmällä.

Lisää uusi menetelmä luo jaettu käyttö allekirjoituksen ja luoda:

    static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a shared access signature. " +
        "A stored access policy defines the constraints for the signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Generate the shared access signature on the blob.
        string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

**Main()** -menetelmä ennen **Console.ReadLine()**puhelu alareunassa Lisää seuraavat rivit **GetBlobSasUriWithPolicy** menetelmää:    

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

**Main()** menetelmä pitäisi näyttää tältä kokonaisuudessaan. Suorittamalla se kirjoittaa jaetun access-allekirjoituksen URI konsolipuussa kopioi ja liitä ne tekstitiedostoon käytettäviksi Tässä opetusohjelmassa toista osaa.

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Generate a SAS URI for the container, without a stored access policy.
        Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, without a stored access policy.
        Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
        Console.WriteLine();

        //Clear any existing access policies on container.
        BlobContainerPermissions perms = container.GetPermissions();
        perms.SharedAccessPolicies.Clear();
        container.SetPermissions(perms);

        //Create a new access policy on the container, which may be optionally used to provide constraints for
        //shared access signatures on the container and the blob.
        string sharedAccessPolicyName = "tutorialpolicy";
        CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

        //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        Console.ReadLine();
    }

Kun suoritat GenerateSharedAccessSignatures console-sovelluksen, näkyviin tulee konsoli-ikkunassa seuraava tulos. Nämä ovat, osa 2 opetusohjelman käytetään jaettua käyttöä allekirjoitukset.

![SAS-konsolin-tulostus-1][sas-console-output-1]

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>Osa 2: Testaa jaettua käyttöä allekirjoitukset Console-sovelluksen luominen

Voit esikatsella jaettua käyttöä allekirjoitukset luotu edellä olevissa esimerkeissä-olemme Luo toinen console-sovellus, joka käyttää toimintojen suorittaminen säilö ja blob allekirjoitukset.

> [AZURE.NOTE] Jos on kulunut yli 24 tuntia opetusohjelman alkuosa valmis, voit luoda allekirjoitukset eivät enää on voimassa. Tässä tapauksessa Suorita koodin luomiseen ajan tasalla jaettua käyttöä allekirjoitukset käytettäväksi opetusohjelman toisen osan ensimmäisen console-sovelluksessa.

Visual Studiossa Luo uusi Windows console-sovellus ja anna sille nimi **ConsumeSharedAccessSignatures**. Lisää viitteet **Microsoft.WindowsAzure.Configuration.dll** ja **Microsoft.WindowsAzure.Storage.dll**samoin kuin aiemmin.

Program.cs tiedoston yläosassa Lisää **käyttämällä** seuraavia komentoja:

    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

**Main()** menetelmä tekstiosaan Lisää seuraavat vakiot ja päivitä niiden arvojen jaettua käyttöä allekirjoitukset, jotka olet luonut opetusohjelman 1-osassa.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
    }

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Lisää menetelmä Kokeile säilö toimintoja käyttämällä jaetun Access-allekirjoitus

Seuraavaksi lisäämme menetelmän, joka testaa jaettu käyttö allekirjoituksen käyttäminen säilö edustavaa säilö joitakin toimintoja. Huomaa, että jaetun access-allekirjoitus on käyttää voit palauttaa viittauksen säilöön todennustapa access säilöön yksin allekirjoituksen perusteella.

Lisää Program.cs seuraavalla tavalla:

    static void UseContainerSAS(string sas)
    {
        //Try performing container operations with the SAS provided.

        //Return a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

        //Create a list to store blob URIs returned by a listing operation on the container.
        List<ICloudBlob> blobList = new List<ICloudBlob>();

        //Write operation: write a new blob to the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
            string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //List operation: List the blobs in the container.
        try
        {
            foreach (ICloudBlob blob in container.ListBlobs())
            {
                blobList.Add(blob);
            }
            Console.WriteLine("List operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("List operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Get a reference to one of the blobs in the container and read it.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            MemoryStream msRead = new MemoryStream();
            msRead.Position = 0;
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                Console.WriteLine(msRead.Length);
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        Console.WriteLine();

        //Delete operation: Delete a blob in the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Päivitä Soita **UseContainerSAS()** sekä jaettua käyttöä allekirjoitukset, jonka loit säilön **Main()** -menetelmää:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        Console.ReadLine();
    }


### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>Lisää menetelmä Kokeile Blob-toimintoja käyttämällä jaetun Access-allekirjoitus

Lopuksi lisäämme menetelmän, joka testaa jaettua käyttöä allekirjoituksen käyttäminen blob edustavan blob joitakin toimintoja. Tässä tapauksessa emme palauttaa viittauksen blob konstruktoria **CloudBlockBlob(String)**, kulkeva jaetun access-allekirjoituksen avulla. Muut todennus vaaditaan; se perustuu yksin allekirjoitus.

Lisää Program.cs seuraavalla tavalla:

    static void UseBlobSAS(string sas)
    {
        //Try performing blob operations using the SAS provided.

        //Return a reference to the blob using the SAS URI.
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

        //Write operation: Write a new blob to the container.
        try
        {
            string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Read the contents of the blob.
        try
        {
            MemoryStream msRead = new MemoryStream();
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                msRead.Position = 0;
                using (StreamReader reader = new StreamReader(msRead, true))
                {
                    string line;
                    while ((line = reader.ReadLine()) != null)
                    {
                        Console.WriteLine(line);
                    }
                }
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Delete operation: Delete the blob.
        try
        {
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Päivitä Soita **UseBlobSAS()** sekä jaettu käyttö allekirjoitukset, jonka loit blob **Main()** -menetelmää:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
        UseBlobSAS(blobSAS);
        UseBlobSAS(blobSASWithAccessPolicy);

        Console.ReadLine();
    }

Suorita console-sovellus ja noudata tulosteen nähdäksesi, mitkä toiminnot sallitaan mitä allekirjoitukset. Konsoli-ikkunassa tulos näyttää seuraavankaltaiselta:

![SAS-konsolin-tulostus-2][sas-console-output-2]

## <a name="next-steps"></a>Seuraavat vaiheet

[Jaettu käyttö allekirjoitukset, osa 1: Tietoja SAS-malli](storage-dotnet-shared-access-signature-part-1.md)

[Säilöjen ja BLOB anonyymin luku käytön hallinta](storage-manage-access-to-resources.md)

[Valtuutettu yhteys jaettuun käyttöön allekirjoituksen (REST API)](http://msdn.microsoft.com/library/azure/ee395415.aspx)

[Taulukon ja jonon SAS esittely](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-console-output-1]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-1.PNG
[sas-console-output-2]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-2.PNG
