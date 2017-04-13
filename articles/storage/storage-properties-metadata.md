<properties
    pageTitle="Määritä ja noutaa ominaisuudet ja objektien Azuren tallennustilaan metatietojen | Microsoft Azure"
    description="Tallentaa mukautetun metatietojen objektien Azuren tallennustilaan ja määritä ja hakea järjestelmän ominaisuudet."
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

# <a name="set-and-retrieve-properties-and-metadata"></a>Määritä ja noutaa ominaisuudet ja metatiedot #

## <a name="overview"></a>Yleiskatsaus

Objektien Azuren tallennustilaan tuki järjestelmän ominaisuudet ja käyttäjän määrittämät metatiedot lisäksi ne sisältävät tietoja:

*   **Järjestelmän ominaisuudet.** Järjestelmän ominaisuudet ole tallennustilan kullekin resurssille. Jotkin niistä voidaan lukea tai määrittää toiset ovat vain luku-tilassa. Kattaa, valitse järjestelmä Jotkin ominaisuudet vastaavat tiettyjä vakio HTTP-otsikot. Azure-tallennustilan asiakkaan kirjasto ylläpitää näihin puolestasi.  

*   **Käyttäjän määrittämät metatiedot.** Käyttäjän määrittämät metatiedot metatiedot, joilla voit määrittää tietyn resurssin nimi-arvo-pari muodossa. Metatietojen avulla voit tallentaa tallennustilan; resurssiin liittyvien muita arvoja Nämä arvot vain oman varten ja eivät vaikuta miten resurssin toimii.  

Tallennustilan resurssin-ominaisuus ja metatietojen arvot haetaan on kaksivaiheinen prosessi. Ennen kuin voit lukea nämä arvot, erikseen noutaa **FetchAttributes** -menetelmällä ne.

> [AZURE.IMPORTANT] Ominaisuuden ja metatietojen arvot tallennustilan resurssin ei tallenneta, paitsi jos soitat **FetchAttributes** tavoilla. 

## <a name="setting-and-retrieving-properties"></a>Määrittäminen ja hakeminen ominaisuudet

Ominaisuusarvojen hakemiseen soittaminen **FetchAttributes** menetelmä Blob-objektien tai säilö-ominaisuuksia ja lukea arvot.

Objektin ominaisuuksien määrittämiseen Määritä ominaisuuden arvon ja sitten kutsu **SetProperties** -menetelmää.

Seuraava koodiesimerkki Luo säilön ja kirjoittaa joitakin sen ominaisuusarvoihin konsoli-ikkunassa:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
    //Create the service client object for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Fetch container properties and write out their values.
    container.FetchAttributes();
    Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
    Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
    Console.WriteLine("ETag: {0}", container.Properties.ETag);
    Console.WriteLine();

## <a name="setting-and-retrieving-metadata"></a>Määrittäminen ja hakeminen metatiedot

Voit määrittää metatietojen vähintään yksi Blob-objektien tai säilö resurssin nimi-arvo parit. Voit määrittää metatietojen lisääminen **metatietojen** sivustokokoelman resurssin nimen ja arvon tietoparin ja valitse Tallenna arvot palvelun **SetMetadata** -menetelmää.

> [AZURE.NOTE] Metatietojen nimen on oltava C#-tunnisteiden nimeämiskäytännöt.
 
Seuraava koodiesimerkki määrittää metatietojen säilö. Yksi arvo on määritetty kokoelmaan **Lisää** -menetelmällä. Muut arvoksi on määritetty syntaksin implisiittinen avain/arvo. Molemmat ovat kelvollisia.

    public static void AddContainerMetadata(CloudBlobContainer container)
    {
        //Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        //Set the container's metadata.
        container.SetMetadata();
    }

Metatietojen noutaminen kutsua **FetchAttributes** menetelmää Blob-objektien tai täytä **metatietojen** sivustokokoelman säilö ja sitten lukea arvoista, kuten alla olevassa esimerkissä.

    public static void ListContainerMetadata(CloudBlobContainer container)
    {
        //Fetch container attributes in order to populate the container's properties and metadata.
        container.FetchAttributes();

        //Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }

## <a name="see-also"></a>Katso myös  

- [Azure-tallennustilan asiakkaan kirjaston .NET-käyttöä varten](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
- [Azure tallennustilan asiakkaan kirjaston .NET-paketti](https://www.nuget.org/packages/WindowsAzure.Storage/) 
