<properties
    pageTitle="Käsittele suurissa tietojoukkoja Data Factory ja erä | Microsoft Azure"
    description="Tässä artikkelissa käsitellään käsitellä Azure Data Factory-myyntijakso erittäin suuri tietomäärien Azure erän rinnakkain käsittely-ominaisuuksien avulla."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="spelluru"/>

# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a>Käsittele suurissa tietojoukkoja Data Factory ja erä käyttäminen
Tässä artikkelissa kuvataan arkkitehtuuri malli-ratkaisun, joka siirtää ja käsittelee suurissa tietojoukkoja automaattisen ja ajoitetun tavalla. Lopusta loppuun-ongelmatilanteita toteuttamisesta käyttämällä Azure Data Factory ja Azure erää ratkaisu sisältää myös. 

Tässä artikkelissa on pidempi kuin Microsoftin tyypillinen artikkelissa, koska se sisältää koko malli-ratkaisun vaiheista. Jos ole aiemmin käyttänyt Siirtoerän ja Data Factory, saat lisätietoja näiden palvelujen ja miten ne toimivat yhdessä. Jos tiedät jotain palveluista ja ovat suunnittelemisesta ja suunnittelee ratkaisun, voit saattaa keskittyä vain artikkelin [arkkitehtuuri-osassa](#architecture-of-sample-solution) ja Jos kehität prototyypin tai ratkaisun, myös haluat ehkä kokeilla vaiheittaiset ohjeet [vaiheittaisissa ohjeissa](#implementation-of-sample-solution). Olemme kutsu kommenttisi sisällön ja miten sitä käytetään.

Ensin katsotaan miten Data Factory ja erä-palveluiden avulla käsittelemisessä suuria tietojoukkoja pilveen.     

## <a name="why-azure-batch"></a>Miksi Azure erä?
Azure erä avulla voit suorittaa suurissa rinnakkais- ja tehokas tietojenkäsittely (HPC) sovelluksia tehokkaasti pilveen. Platform-palvelu, joka ajoittaa Laske kuormittavaa työtä voidaan suorittaa näennäiskoneiden hallitun kokoelma on, ja voit automaattisesti asteikko Laske resurssien tarpeiden työt.

Erän-palvelussa Azure Laske resurssien sovellusten suorittaminen rinnakkain ja mittakaava määrittäminen. Voit suorittaa tarvittaessa tai ajoitetun työt, etkä tarvitse manuaalisesti, määrittäminen ja luominen sekä hallinta HPC-klusterin, yksittäisiä näennäiskoneiden, virtual verkkojen tai monimutkaisia projektin tehtävän ajoitus infrastruktuurin.

Tutustu seuraaviin artikkeleihin, jos et ole tottunut Azure erä, kun se helpottaa tietoja ratkaisu, joka on kuvattu tämän artikkelin arkkitehtuuri/soveltaminen.   

- [Azure erä perusteet](../batch/batch-technical-overview.md)
- [Erän ominaisuuden yhteenveto](../batch/batch-api-basics.md)

(valinnainen) Lisätietoja Azure erä on artikkelissa [Azure erän oppimispolku](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Miksi Azure Data Factory?
Tietoja Factory on pilvipohjainen tietojen integrointi-palvelu, orchestrates ja automatisoi siirto ja tietojen muunnos. Data Factory-palvelun avulla, voit luoda hallitut tietojen putkistot, joka siirtää tietoja paikallisen ja cloud Keskitetty tietosäilö myymälät tiedot (esimerkiksi: Azure-Blob-säiliö), ja prosessin ja muunna palveluihin Azure Hdinsightiin ja Azure koneen oppiminen avulla. Voit myös ajoittaa tietojen putkistot ajoitetun tavalla (kerran tunnissa, päivittäin, viikoittain, jne.) ja valvo käynnissä ja hallita niitä yhdellä silmäyksellä tunnistaa ongelmat ja toimia. 

Tutustu seuraaviin artikkeleihin, jos et ole tottunut Azure Data Factory, kun se helpottaa ratkaisu, joka on kuvattu tämän artikkelin arkkitehtuuri/soveltaminen tietoja.  

- [Azure Data Factory esittely](data-factory-introduction.md)
- [Luo ensimmäinen tietojen-myyntijakso](data-factory-build-your-first-pipeline.md)   

(valinnainen) Lisätietoja Azure Data Factory on artikkelissa [Azure Data Factory oppimispolku](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Tietoja Factory ja erä yhdessä
Data Factory sisältää valmiita toimintoja, kuten kopioi tehtävän kopioida tai siirtää tietojen lähteen tietosäilö kohde tietojen säilön ja tietojen Hadoop varausyksiköt (HDInsight) käyttäminen Azure rakenne tehtävän. Katso [Tietoja muunnos toimintojen](data-factory-data-transformation-activities.md) tuetut muunnos toimintojen luettelo. 

Voit myös luoda mukautettuja .NET tehtäviä tai käsitellä omia logiikan tietoja ja suorittaa näitä toimia Azure HDInsight-klusterin tai VMs Azure erä resurssivarantoon. Kun Azure Siirtoerän, voit määrittää automaattisen näytöstä resurssivarantoon (lisääminen tai poistaminen työmäärää perusteella VMs) antaisit kaavan perusteella.     

## <a name="architecture-of-sample-solution"></a>Esimerkki ratkaisun arkkitehtuuri
Vaikka arkkitehtuuri, joka on kuvattu tämän artikkelin on yksinkertainen ratkaisun, asiat kiinnostavat monimutkaisia tilanteita, joissa esimerkiksi riskin mallinnus palvelujen, kuvankäsittely ja värien ja genomien analyysi. 

Kaavio kuvaa, 1) kuinka Data Factory orchestrates tietojen siirto ja käsittelyn ja 2) kuinka Azure erä käsittelee tiedot rinnakkain tavalla. Ladattava ja tulostettava kaavio yhteyteen (11 x 17 tuumaa. tai -koon A3): [HPC ja tietojen tiedonsiirron Azure Siirtoerän ja Data Factory](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Suurissa tietojen käsittely-kaavio](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

Seuraavassa luettelossa on perusvaiheet yhteydessä. Ratkaisu sisältää koodin ja muodostaa lopusta loppuun-ratkaisun selitykset.

1.  **Määritä Azure erän resurssivarantoon Laske solmujen (VMs)**. Voit määrittää näkyvien solmujen määrän ja kunkin solmun koko.

2.  **Luo erillisen Azure Data Factory** , joka on määritetty kohteisiin, jotka edustavat Azure-blob-säiliö, Azure erä Laske palvelu, i/tiedot ja työnkulun/putkijohto ja toimintoja, jotka siirtäminen ja muuntaminen tiedot.

3.   **Luo mukautetun .NET Data Factory myyntijakso-tehtävän**. Tehtävä on Käyttäjäkoodi, joka suoritetaan Azure erä resurssivarantoon.

4.  **Säilön suuria määriä BLOB Azuren tallennustilaan syötteen tiedot**. Tiedot jaetaan looginen sektoreiden (yleensä ajan mukaan).

5.  Toissijainen paikkaan **data Factory kopioi tiedot, jotka käsitellään rinnakkain** .

6.  **Data Factory suoritetaan mukautetut aktiviteetit, Varaa erän resurssivarannon avulla**. Tietoja Factory voidaan suorittaa toimintoja samanaikaisesti. Tehtävätyypin käsittelee sektorin tietoa. Tulokset on tallennettu Azure-tallennustilan.

7.  **Data Factory siirtää lopputulokset kolmannen sijaintiin**, jaettavaksi sovelluksen kautta tai muiden Kuvatyökalut lisäkäsittelyä varten.

## <a name="implementation-of-sample-solution"></a>Esimerkki-ratkaisun käyttöönotto
Esimerkki ratkaisu on tarkoituksella yksinkertainen ja, kuinka voit käyttää tietojen Factory ja erä yhdessä käsittelemään tietojoukkoja. Ratkaisu laskee yksinkertaisesti syötteen tiedostoissa, jotka on järjestetty aikasarjan hakusana ("Microsoft") esiintymien lukumäärän. Se siirtää tulosteen tiedostojen määrä.

**Aika**: Jos Azure, Data Factory ja erä tuttu ja alla luetellut edellytykset täyttyvät, emme arvioida tämä ratkaisu on 1 – 2 tuntia.

### <a name="prerequisites"></a>Edellytykset

#### <a name="azure-subscription"></a>Azure-tilaus
Jos sinulla ei ole Azure tilaus, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Katso [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Azure-tallennustilan tilin
Voit käyttää Azure-tallennustilan tilin Tässä opetusohjelmassa tietojen tallentamista varten. Jos sinulla ei ole Azure-tallennustilan tilin, katso [tallennustilan tilin luominen](../storage/storage-create-storage-account.md#create-a-storage-account). Näyte käyttää Blob-objektien tallennustilaan.

#### <a name="azure-batch-account"></a>Azure erä-tili
Luo [Azure portal](http://manage.windowsazure.com/)erä Azure-tili. Katso [Luo ja hallitse Azure erä tilin](../batch/batch-account-create-portal.md). Huomautus Azure erä tilin nimi ja tilin avainta. Voit myös Azure erä-tilin luominen [Uusi AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet-komento. Saat lisätietoja tämän cmdlet-komennolla [Azure erä PowerShell cmdlet-komentojen käytön aloittaminen](../batch/batch-powershell-cmdlets-get-started.md) .

Näyte käyttää Azure erä (epäsuorasti joko Azure Data Factory-myyntijakso) tietojen Laske solmujen (näennäiskoneiden hallitun kokoelman) resurssivarantoon rinnakkain tavalla.

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a>Erän resurssivarantoon azuren näennäiskoneiden (VMs)
Luo **Azure erä resurssivarantoon** vähintään 2 Laske solmujen kanssa.

1.  [Azure portal](https://portal.azure.com)Valitse vasemmanpuoleisessa valikossa **Selaa** ja valitse **Erä tilit**. 
2. Valitse tilisi Azure erä Avaa **Erä tili** -sivu. 
3. Napsauta **jakavat** -ruutua.
4. Valitse Lisää-painiketta, valitsemalla työkaluriviltä lisääminen resurssivarantoon **jakavat** -sivu.
    1. Kirjoita tunnus resurssivarantoon (**Sovellussarjan tunnus**). Huomaa, että **ryhmän tunnus**; tarvitse sitä Data Factory-ratkaisua luotaessa. 
    2. Määritä **Windows Server 2012 R2** käyttöjärjestelmän perhe-asetukselle.
    3. Valitse **solmu hinnoittelu taso**.
    4. Kirjoita **2** arvona **Kohde varattu** -asetus.
    5. Kirjoita **2** arvona **Maks tehtäviä kohden solmu** -asetus.
    6. Valitse **OK** , jos haluat luoda ryhmän. 
    
#### <a name="azure-storage-explorer"></a>Azure-tallennustilan Explorer   
[Azure tallennustilan Explorer 6 (työkalu)](https://azurestorageexplorer.codeplex.com/) tai [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (-ClumsyLeaf-ohjelmisto). Voit käyttää työkaluja tarkistetaan ja mukaan lukien lokit cloud isännöimä sovellustesi Azuren tallennustilaan projektien tietojen muuttamista.

1.  Luo säilö **mycontainer** yksityinen Accessissa (ei anonyymin käytön)

2.  Jos käytössäsi on **CloudXplorer**, luo kansiot ja alikansiot seuraavat rakenteen kanssa:

    ![](./media/data-factory-data-processing-using-batch/image3.png)

    **Inputfolder** ja **outputfolder** ole ylätason **mycontainer,** kansioiden ja **inputfolder** alikansiot, joiden päivämäärä aikaleimat (YYYY-MM-DD-HH).

    Jos käytät **Azure-tallennustilan Explorer**seuraavassa vaiheessa, sinun on ladattava tiedostot, joiden nimet: inputfolder/2015-11-16-00/file.txt, inputfolder/2015-11-16-01/file.txt ja niin edelleen. Tämä vaihe luo automaattisesti kansioihin.

3.  Luo tekstin tiedoston **file.txt** tietokoneen sisältöä, joka on **Microsoft**. Esimerkiksi: "testata mukautetut aktiviteetit Microsoft testi mukautetut aktiviteetit Microsoft".

4.  Tiedoston lataaminen Azure Blob-objektien tallennustilaan syötteen seuraavat kansiot.

    ![](./media/data-factory-data-processing-using-batch/image4.png)

    Jos käytössäsi on **Azure-tallennustilan Explorer**, Lataa tiedosto **file.txt** **mycontainer**. Valitse **Kopioi** luoda kopion blob-painiketta. **Kopioi Blob** -valintaikkunassa muuttaa **blob kohdenimi** **inputfolder/2015-11-16-00/file.txt.** Toista tämä vaihe luoda inputfolder/2015-11-16-01/file.txt, inputfolder/2015-11-16-02/file.txt, inputfolder/2015-11-16-03/file.txt, inputfolder/2015-11-16-04/file.txt ja niin edelleen. Tämä toiminto luo automaattisesti kansioihin.

3.  Luo toinen säilö: **customactivitycontainer**. Mukautetut aktiviteetit zip-tiedoston lataaminen tämä säilö.

#### <a name="visual-studio"></a>Visual Studio
Asenna Microsoft Visual Studio 2012 tai uudempi mukautetut erä aktiviteetit, Data Factory-ratkaisussa käytettävän luomiseen.

### <a name="high-level-steps-to-create-the-solution"></a>Ylätason ratkaisun luomisen vaiheet

1.  Voit luoda mukautetun toiminnon, joka sisältää tietojen käsittely-logiikkaa.
2.  Azure tiedot-factory, joka käyttää mukautetun tehtävän luominen

### <a name="create-the-custom-activity"></a>Mukautetun tehtävän luominen

Data Factory mukautetut aktiviteetit on esimerkki tämä ratkaisu. Näyte käyttää Azure erä mukautetun tehtävän suorittamiseen. Tutustu [käyttää mukautettuja toimintoja Azure Data Factory-myyntijakso](data-factory-use-custom-activities.md) perustiedoille kehittää mukautettuja toimintoja ja käyttää niitä Azure Data Factory putkistot.

Voit luoda .NET mukautetut aktiviteetit, joiden avulla voit Azure Data Factory-myyntijakso-haluat luoda **.NET luokkakirjasto** projektin, luokka, joka sisältää kyseisen **IDotNetActivity** -liittymän. Tämän liittymän on vain yksi keino: **Suorita**. Näin allekirjoituksen-menetelmän:

    public IDictionary<string, string> Execute(
                IEnumerable<LinkedService> linkedServices,
                IEnumerable<Dataset> datasets,
                Activity activity,
                IActivityLogger logger)

Menetelmä on muutama tärkeimmät osat, jotka ymmärtämistä.

-   Tapa käyttää neljää parametreja:

    1.  **linkedServices**. Linkitetyn services, joka linkittää i/tietolähteitä järjestelmäelementtien luettelo (esimerkiksi: Azure-Blob-säiliö), tietojen factory. Tässä esimerkissä on vain yksi linkitetyn palvelun tyypin syötteen ja tulosteen Azure-tallennustilan.

    2.  **tietojoukkoja**. Tämä on tietojoukkoja järjestelmäelementtien luettelo. Tämä parametri avulla saat syöttö- ja tietojoukkoja määrittämiä rakenteita ja sijainnit.

    3.  **tehtävän**. Tämä parametri edustaa nykyisen Laske kohteen – tässä tapauksessa erä Azure-palvelu.

    4.  **lokin**. Lokin voit kirjoittaa virheenkorjaus kommentteja pinnalla, putkijohto ja "Käyttäjä"-loki.

-   Menetelmä palauttaa sanasto, jonka avulla voidaan ketjut mukautetut aktiviteetit yhdessä tulevaisuudessa. Tämä ominaisuus ei ole vielä toteutettu, tyhjä sanaston tuotto niin menetelmää. 

#### <a name="procedure-create-the-custom-activity"></a>Toimintosarja: Luo mukautetut aktiviteetit

1.  Visual Studio .NET luokkakirjasto projektin luominen

    1.  Käynnistä **Visual Studio 2012**/**2013/2015**.

    2.  Valitse **Tiedosto**, valitse **Uusi**ja sitten **Projekti**.

    3.  **Mallit**ja valitse **Visual C\#**. Käytät tätä vaiheittaista C\#, mutta tahansa .NET-kielen avulla voit kehittää mukautetut aktiviteetit.

    4.  Valitse **Luokkakirjasto** erilaiset projektityypit oikeanpuoleisesta luettelosta.

    5.  Kirjoita **MyDotNetActivity** **nimi**.

    6.  Valitse **C:\\SYÖTTÖ** **sijainti**. Luo kansio **SYÖTTÖ** , jos sitä ei ole.

    7.  Valitse **OK** , jos haluat luoda projektin.

2.  Valitse **Työkalut**, osoita **NuGet pakettien hallinta**ja valitse **Paketti hallinta-konsolin**.

3.  **Paketin hallinta-konsolin**Suorita seuraava komento tuo **Microsoft.Azure.Management.DataFactories**.

            Install-Package Microsoft.Azure.Management.DataFactories

4.  Tuo **Azuren tallennustilaan** NuGet paketin projektiin. Sinun on tämän paketin, koska tässä esimerkissä Blob-säiliö Ohjelmointirajapinnan käyttäminen.

        Install-Package Azure.Storage

5.  Lisää seuraavat **käyttämällä** direktiivejä lähdetiedosto projektissa.

        using System.IO;
        using System.Globalization;
        using System.Diagnostics;
        using System.Linq;

        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Runtime;

        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

6.  Siirry **MyDotNetActivityNS** **nimitilan** nimi.

        namespace MyDotNetActivityNS

7.  Luokan nimen muuttaminen **MyDotNetActivity** ja saada **IDotNetActivity** käyttöliittymästä alla kuvatulla tavalla.

        public class MyDotNetActivity : IDotNetActivity

8.  Toteuta **MyDotNetActivity** -luokan **IDotNetActivity** -liittymän (Lisää) **Execute** -menetelmää ja kopioi seuraavaa ratkaisuvaihtoehtoa. [Suorita menetelmä](#execute-method) on kohdassa käyttää tätä tapaa logiikan kuvauksen.

        /// <summary>
        /// Execute method is the only method of IDotNetActivity interface you must implement.
        /// In this sample, the method invokes the Calculate method to perform the core logic.  
        /// </summary>
        public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
        {

            // declare types for input and output data stores
            AzureStorageLinkedService inputLinkedService;

            Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
            foreach (LinkedService ls in linkedServices)
                logger.Write("linkedService.Name {0}", ls.Name);

            // using First method instead of Single since we are using the same
            // Azure Storage linked service for input and output.
            inputLinkedService = linkedServices.First(
                linkedService =>
                linkedService.Name ==
                inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
                as AzureStorageLinkedService;

            string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
            string folderPath = GetFolderPath(inputDataset);
            string output = string.Empty; // for use later.

            // create storage client for input. Pass the connection string.
            CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
            CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();

            // initialize the continuation token before using it in the do-while loop.
            BlobContinuationToken continuationToken = null;
            do
            {   // get the list of input blobs from the input storage client object.
                BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                         true,
                                         BlobListingDetails.Metadata,
                                         null,
                                         continuationToken,
                                         null,
                                         null);

                // Calculate method returns the number of occurrences of
                // the search term (“Microsoft”) in each blob associated
                // with the data slice.
                //
                // definition of the method is shown in the next step.
                output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

            } while (continuationToken != null);

            // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
            Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

            folderPath = GetFolderPath(outputDataset);

            logger.Write("Writing blob to the folder: {0}", folderPath);

            // create a storage object for the output blob.
            CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
            // write the name of the file.
            Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

            logger.Write("output blob URI: {0}", outputBlobUri.ToString());
            // create a blob and upload the output text.
            CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
            logger.Write("Writing {0} to the output blob", output);
            outputBlob.UploadText(output);

            // The dictionary can be used to chain custom activities together in the future.
            // This feature is not implemented yet, so just return an empty dictionary.
            return new Dictionary<string, string>();
        }


9.  Lisää luokan helper seuraavista tavoista. Näistä menetelmistä on kutsuttu **Execute** -menetelmällä. Tärkeintä **Laske** -menetelmä eristää tunnus, jolla iteroivaa kunkin blob kautta.

        /// <summary>
        /// Gets the folderPath value from the input/output dataset.
        /// </summary>
        private static string GetFolderPath(Dataset dataArtifact)
        {
            if (dataArtifact == null || dataArtifact.Properties == null)
            {
                return null;
            }

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            if (blobDataset == null)
            {
                return null;
            }

            return blobDataset.FolderPath;
        }

        /// <summary>
        /// Gets the fileName value from the input/output dataset.
        /// </summary>

        private static string GetFileName(Dataset dataArtifact)
        {
            if (dataArtifact == null || dataArtifact.Properties == null)
            {
                return null;
            }

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            if (blobDataset == null)
            {
                return null;
            }

            return blobDataset.FileName;
        }

        /// <summary>
        /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
        /// and prepares the output text that is written to the output blob.
        /// </summary>

        public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
        {
            string output = string.Empty;
            logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
            foreach (IListBlobItem listBlobItem in Bresult.Results)
            {
                CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
                if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
                {
                    string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
                    logger.Write("input blob text: {0}", blobText);
                    string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
                    var matchQuery = from word in source
                                     where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                     select word;
                    int wordCount = matchQuery.Count();
                    output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
                }
            }
            return output;
        }


    **GetFolderPath** menetelmä palauttaa polun kansio, joka osoittaa dataset ja **GetFileName** menetelmä palauttaa Blob-objektien tai tiedoston, joka osoittaa tietojoukon nimi.

        "name": "InputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "file.txt",
                "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",


    **Laske** -menetelmä laskee avainsana **Microsoftin** input-tiedostoissa (BLOB-kansiossa) esiintymien määrän. Hakusanoja ("Microsoft") on koodattu koodissa.

10.  Käännä projektin. Valitse valikosta **Luo** ja valitse **Muodosta ratkaisu**.

11.  Avaa **Resurssienhallinta**ja siirry **bin\\virheenkorjaus** tai **bin\\Vapauta** kansion muodosta tyypin mukaan.

12.  Luo zip-tiedoston **MyDotNetActivity.zip** , joka sisältää-binaaritiedostoja ** \\bin\\virheenkorjaus** kansio. Voit sisällyttää MyDotNetActivity. **pdb** tiedoston niin, että saat lisätietoja, kuten rivinumero lähdekoodi, jotka aiheuttivat ongelman, kun virhe ilmenee.

    ![](./media/data-factory-data-processing-using-batch/image5.png)

13.  Lataa **MyDotNetActivity.zip** kuin Blob-objektien blob-säilöön: **customactivitycontainer** **StorageLinkedService** linkitetty **ADFTutorialDataFactory** palvelun Azuren Blob-objektien tallennustilaan käyttää. Luo blob-säilö **customactivitycontainer** , jos sitä ei ole jo olemassa.

#### <a name="execute-method"></a>Suorita menetelmä

Tässä osassa on enemmän tietoja ja huomautuksia Execute-menetelmä koodia.

1.  Jäsenet varten syötteen sivustokokoelman läpikäyminen löytyvät [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) nimitilan. Blob-sivustokokoelman läpikäyminen edellyttää **BlobContinuationToken** -luokan avulla. Oikeastaan sinun on käytettävä lähettäminen-silmukan kanssa kuin koskevat sulkeminen silmukan tunnuksen aikana. Lisätietoja on artikkelissa [käyttämisestä .NET-Blob-objektien tallennustilaan](../storage/storage-dotnet-how-to-use-blobs.md). Tavallinen ympyrä on mukana:

        // Initialize the continuation token.
        BlobContinuationToken continuationToken = null;
        do
        {
        // Get the list of input blobs from the input storage client object.
        BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                true,
                                          BlobListingDetails.Metadata,
                                          null,
                                          continuationToken,
                                          null,
                                          null);
        // Return a string derived from parsing each blob.
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

        } while (continuationToken != null);

    Lisätietoja [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) -menetelmä ohjeissa.

2.  Toimi kautta BLOB joukon loogisesti koodi siirtyy sisällä Älä-silmukan aikana. **Execute** -tapaa Älä-samalla, kun silmukan välittää BLOB luettelo **Laske**-menetelmää. Menetelmä palauttaa merkkijonomuuttujan **tulos** , joka on saatu ottaa iteroitava läpi kaikki BLOB-osiossa.

    Se palauttaa hakusanoja (**Microsoft**) esiintymien lukumäärän Blob-objektien välitetään **Laske** -menetelmää.

        output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);

3.  **Laske** -menetelmä on tehty työ, kun se on kirjoitettava uusi Blob-objektien. Jotta BLOB käsitteleminen jokaisen joukko, uusia Blob-objektien voidaan kirjoittaa tuloksiin. Voit kirjoittaa uuden Blob-objektien, Etsi tulostus-tietojoukko.

        // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

4.  Koodin kutsuu myös avustaja-menetelmää: **GetFolderPath** hakemiseen kansiopolku (tallennustilan säilö-nimi).

        folderPath = GetFolderPath(outputDataset);

    **GetFolderPath** muuttaa sarakemuotoa tietojoukko objektin AzureBlobDataSet, jossa on kansiopolku-ominaisuus.

        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

        return blobDataset.FolderPath;

5.  Koodin kutsuu **GetFileName** -menetelmä noutamiseen tiedostonimi (blob-nimi). Koodi on samanlainen kuin yllä koodin hankkiminen kansiopolku.

        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

        return blobDataset.FileName;

6.  Tiedoston nimi on kirjoitettu luomalla URI-objekti. URI-konstruktori palauttaa säilön nimi käyttämällä **BlobEndpoint** -ominaisuus. Kansion polku ja tiedostonimi lisätään käyttää tulosteen Blob-objektien URI.  

        // Write the name of the file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

7.  Tiedoston nimi on kirjoitettu ja nyt tulosteen merkkijonon kirjoittaa uuden Blob-objektien voit **Laske** -menetelmää:

        // Create a blob and upload the output text.
        CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
        logger.Write("Writing {0} to the output blob", output);
        outputBlob.UploadText(output);


### <a name="create-the-data-factory"></a>Tietoja factory luominen

[Mukautetun tehtävän luominen](#create-the-custom-activity) -osan mukautetut aktiviteetit luodaan ja kanssa binaaritiedostot zip-tiedoston ja PDB-tiedosto ladataan Azure-blob-säilö. Tässä osassa Luo Azure **tietojen factory** **myyntijakso** , joka käyttää **Mukautetut aktiviteetit**kanssa.

Mukautetun toiminnon syötteen tietojoukko edustaa BLOB-objektit (tiedostot) syötteen kansio (mycontainer\\inputfolder) Blob-objektien tallennustilaan. Tehtävän tulostus-tietojoukko edustaa tulostus-kansiossa tulosteen BLOB-objektit (mycontainer\\outputfolder) Blob-objektien tallennustilaan.

Pudota yhden tai useamman syötteen kansioihin:

    mycontainer -\> inputfolder
        2015-11-16-00
        2015-11-16-01
        2015-11-16-02
        2015-11-16-03
        2015-11-16-04

Esimerkiksi pudottaa tiedostoja (file.txt) seuraavat sisällölle kansioista.

    test custom activity Microsoft test custom activity Microsoft

Jokainen syötteen kansio vastaa Azure Data Factory sektoria vaikka kansiossa on 2 tai useita tiedostoja. Kun kukin sektori käsitellään putkijohto, mukautetut aktiviteetit iteroivaa läpi kaikki sitä syötteen kansio BLOB.

Näet viisi tulostus-tiedostoja, joissa on sama sisältö. Kohdetiedosto käsittelemästä 2015-11 – 16-00-kansiossa olevan tiedoston on esimerkiksi seuraavat sisältö:

    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.

Jos poistat useita tiedostoja (file.txt, file2.txt, file3.txt), jolla on sama sisältö syötteen kansio, näet kohdetiedosto seuraavia sisältö. Kullakin kansiolla (2015-11 – 16-00, jne.) vastaa tässä esimerkissä sektoria, vaikka kansio on valittuna useita syötteen tiedostoja.

    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.


Kohdetiedosto on kolme riviä nyt yksi sektori (2015-11 – 16-00) liittyvän kansion syötteen tiedoston (blob).

Tehtävän luodaan kunkin tehtävän suorittaminen. Tässä esimerkissä on vain yksi tehtävä putkijohto. Kun sektoria käsitellään putkijohto, mukautetut aktiviteetit suoritetaan Azure erän käsittelemään sektoria. Koska käsittelyalueella on viisi sektoreiden (kukin sektori voi olla useita BLOB tai tiedosto), on viisi Azure osalta luodut tehtävät. Tehtävän suoritettaessa erän on todella, jossa on mukautetut aktiviteetit.

Seuraava vaihe vaiheelta on lisätiedot.

#### <a name="step-1-create-the-data-factory"></a>Vaihe 1: Tietojen factory luominen

1.  Jälkeen kirjautumisesta [Azure portal](https://portal.azure.com/), toimi seuraavasti:

    1.  Valitse **Uusi** vasemmalla olevasta valikosta.

    2.  Valitse **tietojen + Analytics** **Uusi** sivu.

    3.  Valitse **Data Factory** **tietojen analytics** -sivu.

2.  Kirjoita nimi **CustomActivityFactory** **uudet tiedot factory** -sivu. Azure tietojen factory nimen on oltava yksilöivä. Jos saat virheilmoituksen: **tietojen factory nimi "CustomActivityFactory" ei ole käytettävissä**, tietojen factory (esimerkiksi **yournameCustomActivityFactory**) nimen muuttaminen ja yritä uudelleen.

3.  Valitse **RESURSSINIMI**ja valitse aiemmin resurssiryhmä tai luo resurssiryhmä.

4.  Varmista, että käytät oikeaa tilauksen ja alueen kohtaa, johon haluat luoda tietojen tehdas.

5.  Valitse **Luo** **Uusi tietojen factory** -sivu.

6.  Näet luomisen **raporttinäkymät-ikkunan** Azure-portaalin tietojen factory.

7.  Kun tietojen tehdas on luotu, näyttöön tulee tietojen Tehdas-sivun, joka näyttää tiedot factory sisällön.

 ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>Vaihe 2: Luo linkitetyn palvelut

Linkitetyn services linkittäminen Microsoft Data tai laskea Azure tietojen factory-palvelut. Tässä vaiheessa voit linkittää tiedot factory **Azure-tallennustilan** tilin ja **Azure erä** tili.

#### <a name="create-azure-storage-linked-service"></a>Luo linkitetty Azure-tallennustilan

1.  Valitse **tekijän ja ota käyttöön** ruutu, **CustomActivityFactory** **DATA FACTORY** -sivu. Näet tietojen Factory-editorin.

2.  Napsauta komentopalkin **tallentaa uudet tiedot** ja valitse **Azure-tallennustilan.** Raportissa pitäisi näkyä JSON komentosarjan luominen Azure-tallennustilan linkitetty service-editorissa.

    ![](./media/data-factory-data-processing-using-batch/image7.png)

3.  Korvaa **tilinimen** pikanäppäin Azure-tallennustilan tilin nimi Azure-tallennustilan tilin ja **tili-näppäintä** . Lisätietoja tallennustilan pikanäppäin hankkiminen on artikkelissa [näkymän, kopioi ja muodosta uudelleen sarjanumerot tallennustilan pikanäppäimet](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

4.  Valitse **Ota käyttöön** komentopalkista linkitetyn palvelun käyttöön.

    ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a>Luo linkitetty Azure erä-palvelu

Tässä vaiheessa voit luoda linkitetyn palvelu, jota käytetään tietojen Factory mukautetun tehtävän suorittamiseen **Azure erä** tilin.

1.  Napsauta **Uusi Laske** komentopalkin ja valitse **Azure erä.** Raportissa pitäisi näkyä JSON komentosarjan luominen Azure erä linkitetty service-editorissa.

2.  JSON-komentosarja:

    1.  Korvaa **tilinimen** Siirtoerän Azure-tilisi nimi.

    2.  Korvaa **Pikanäppäin** Azure erä tilin pikanäppäin.

    3.  Kirjoita ryhmän tunnus **poolName** -ominaisuuden**.** Tämän ominaisuuden voit määrittää sovellussarjan joko nimi tai altaan ID-tunnuksellasi.

    4.  Kirjoita haluamasi erä URI **batchUri** JSON-ominaisuuden. 
    
        > [AZURE.IMPORTANT] **URL-osoite** - **Azure erä tili-sivu** on seuraavanlainen: \<accountname\>. \<alueen\>. batch.azure.com. JSON **batchUri** -ominaisuuden on poistettava **"accountname."** URL-osoitteesta. Esimerkki: `"batchUri": "https://eastus.batch.azure.com"`.

        ![](./media/data-factory-data-processing-using-batch/image9.png)

        **PoolName** -ominaisuutta voit myös määrittää ryhmän nimen sijaan ryhmän tunnus.

        > [AZURE.NOTE] Data Factory-palvelu ei tue tarvittaessa vaihtoehto Azure erän kuin Hdinsightista. Voit käyttää vain sellaisia oman Azure erä resurssivarantoon Azure tietojen factory.

    5.  Määritä **StorageLinkedService** **linkedServiceName** -ominaisuuden. Edellisessä vaiheessa luomasi linkitetyn palvelua. Tämä käytetään väliaikaisen alueen tiedostojen ja lokitiedot.

3.  Valitse **Ota käyttöön** komentopalkista linkitetyn palvelun käyttöön.

#### <a name="step-3-create-datasets"></a>Vaihe 3: Luo tietojoukkoja

Tässä vaiheessa voit luoda tietojoukkoja syöttö- ja tiedot.

#### <a name="create-input-dataset"></a>Kirjoita tietojoukon luominen

1.  **Editorin** Data Factory varten Valitse työkalurivillä **Uusi tietojoukko** -painiketta ja valitse **Azure-Blob-säiliö** avattavasta valikosta.

2.  Korvaa JSON oikeanpuoleisessa ruudussa seuraavat JSON koodikatkelman:

        {
            "name": "InputDataset",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
                "typeProperties": {
                    "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
                    "format": {
                        "type": "TextFormat"
                    },
                    "partitionedBy": [
                        {
                            "name": "Year",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "yyyy"
                            }
                        },
                        {
                            "name": "Month",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "MM"
                            }
                        },
                        {
                            "name": "Day",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "dd"
                            }
                        },
                        {
                            "name": "Hour",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "HH"
                            }
                        }
                    ]
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        }


     Voit luoda putkijohto myöhemmin tätä vaiheittaista kanssa alkamisaika: 2015-11-16T00:00:00Z-ja päättymisaika: 2015-11-16T05:00:00Z. Se on ajoitettu tuottamaan tietojen **kerran tunnissa**, joten 5/i sektoreiden (välillä **00**: 00:00 -\> **05**: 00:00).

     **Korkojakso** ja syötteen tietojoukko **väli** on määritetty **tuntia** ja **1**, mikä tarkoittaa, että syötteen sektori on käytettävissä kerran tunnissa.

     Seuraavassa on kunkin sektorin, joka edustaa **SliceStart** järjestelmän muuttujaan yllä JSON koodikatkelman käynnistystä.

  	| **Sektori** | **Aloitusaika**          |
  	|-----------|-------------------------|
  	| 1         | 2015-11-16T**00**: 00:00 |
  	| 2         | 2015-11-16T**01**: 00:00 |
  	| 3         | 2015-11-16T**02**: 00:00 |
  	| 4         | 2015-11-16T**03**: 00:00 |
  	| 5         | 2015-11-16T**04**: 00:00 |

     **Kansiopolku** lasketaan sektoria alkamisajan (**SliceStart**) vuoden, kuukauden, päivän ja tunti-osaa käyttäen. Tämän vuoksi näin miten syötteen kansio on yhdistetty sektoria.

  	| **Sektori** | **Aloitusaika**          | **Syötteen kansio**  |
  	|-----------|-------------------------|-------------------|
  	| 1         | 2015-11-16T**00**: 00:00 | 2015-11 – 16 -**00** |
  	| 2         | 2015-11-16T**01**: 00:00 | 2015-11 – 16 -**01** |
  	| 3         | 2015-11-16T**02**: 00:00 | 2015-11 – 16 -**02** |
  	| 4         | 2015-11-16T**03**: 00:00 | 2015-11 – 16 -**03** |
  	| 5         | 2015-11-16T**04**: 00:00 | 2015-11 – 16 -**04** |

3.  Valitse **Ota käyttöön** luoda ja ottaa käyttöön **InputDataset** -taulukko-painiketta. 

#### <a name="create-output-dataset"></a>Tulosteen tietojoukon luominen

Tässä vaiheessa voit luoda toisen tietojoukko tyypin AzureBlob tulosteen tietojen esittämiseen.

1.  **Editorin** Data Factory varten Valitse työkalurivillä **Uusi tietojoukko** -painiketta ja valitse **Azure-Blob-säiliö** avattavasta valikosta.

2.  Korvaa JSON oikeanpuoleisessa ruudussa seuraavat JSON koodikatkelman:

        {
            "name": "OutputDataset",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
                "typeProperties": {
                    "fileName": "{slice}.txt",
                    "folderPath": "mycontainer/outputfolder",
                    "partitionedBy": [
                        {
                            "name": "slice",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "yyyy-MM-dd-HH"
                            }
                        }
                    ]
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }


    Tulostus-Blob-objektien tai tiedosto luodaan kunkin syötteen sektorin. Näin miten tulostus-tiedoston nimeksi kunkin sektorin. Tulos kaikki tiedostot luodaan tulostus-kansiossa: **mycontainer\\outputfolder**.


  	| **Sektori** | **Aloitusaika**          | **Kohdetiedosto**       |
  	|-----------|-------------------------|-----------------------|
  	| 1         | 2015-11-16T**00**: 00:00 | 2015-11 – 16 -**00. txt** |
  	| 2         | 2015-11-16T**01**: 00:00 | 2015-11 – 16 -**01. txt** |
  	| 3         | 2015-11-16T**02**: 00:00 | 2015-11 – 16 -**02. txt** |
  	| 4         | 2015-11-16T**03**: 00:00 | 2015-11 – 16 -**03. txt** |
  	| 5         | 2015-11-16T**04**: 00:00 | 2015-11 – 16 -**04. txt** |

     Muista, että kaikki syötteen kansiossa olevat tiedostot (esimerkiksi: 2015-11 – 16-00) sektoria alkamisajan ja osana: 2015-11 – 16-00. Kun tämä sektoria käsitellään, mukautetut aktiviteetit tarkistaa kautta tiedostoille ja tuottaa numerolla esiintymää hakusana ("Microsoft"), kohdetiedosto riville. Jos määritettynä on kolme tiedostoja kansioon 2015-11 – 16-00, kohdetiedosto on kolme riviä: 2015-11 – 16-00.txt.

3.  Valitse Luo ja ota käyttöön **OutputDataset**työkalurivin **käyttöönotto** .

#### <a name="step-4-create-and-run-the-pipeline-with-custom-activity"></a>Vaihe 4: Luominen ja suorittaminen putkijohto, jossa on mukautetut aktiviteetit

Tässä vaiheessa voit luoda putkijohto ja yksi tehtävä aiemmin luomasi mukautetut aktiviteetit.

> [AZURE.IMPORTANT] Jos **file.txt** kirjaimilla blob-säilö kansiot ole vielä ladattu palvelimeen, tee niin ennen kuin luot putkijohto. **IsPaused** -ominaisuudeksi on määritetty false myyntijakso JSON, jotta putkijohto suorittaa heti, jossa **alkamispäivä** on jo mennyt.

1.  Tietoja Factory editori valitsemalla **Uusi myyntijakso** komentopalkista. Jos komento ei ole näkyvissä, valitse **... (Kolme pistettä)** Voit tarkastella sitä.

2.  Korvaa oikeanpuoleisen ruudun JSON JSON seuraavaa komentosarjaa:

        {
            "name": "PipelineCustom",
            "properties": {
                "description": "Use custom activity",
                "activities": [
                    {
                        "type": "DotNetActivity",
                        "typeProperties": {
                            "assemblyName": "MyDotNetActivity.dll",
                            "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                            "packageLinkedService": "AzureStorageLinkedService",
                            "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                        },
                        "inputs": [
                            {
                                "name": "InputDataset"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "OutputDataset"
                            }
                        ],
                        "policy": {
                            "timeout": "00:30:00",
                            "concurrency": 5,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MyDotNetActivity",
                        "linkedServiceName": "AzureBatchLinkedService"
                    }
                ],
                "start": "2015-11-16T00:00:00Z",
                "end": "2015-11-16T05:00:00Z",
                "isPaused": false
           }
        }

    Huomaa seuraavat seikat:

    -   On vain yksi tehtävä putkijohto ja tyyppi, joka on: **DotNetActivity**.

    -   **AssemblyName** on määritetty dll-Kirjaston nimen: **MyDotNetActivity.dll**.

    -   **Aloituskohta** on määritetty **MyDotNetActivityNS.MyDotNetActivity**. Se on lähinnä \<nimitilan\>. \<LuokanNimi\> koodisi.

    -   **PackageLinkedService** on määritetty **StorageLinkedService** , joka osoittaa blob-säiliö, jossa on mukautetut aktiviteetit zip-tiedoston. Jos käytössäsi on eri Azuren tallennustilaan tilien/i-tiedostojen ja mukautetut aktiviteetit zip-tiedoston, sinun on luotava Azure-tallennustilan, jotka on linkitetty toiseen palveluun. Tässä artikkelissa oletetaan, että käytät saman Azure-tallennustilan tilin.

    -   **PackageFile** on määritetty **customactivitycontainer/MyDotNetActivity.zip**. Se on muodossa: \<containerforthezip\>/\<nameofthezip.zip\>.

    -   Mukautetut aktiviteetit otetaan syötteeksi **InputDataset** ja **OutputDataset** kuin tulos.

    -   Mukautetut aktiviteetit **linkedServiceName** -ominaisuus osoittaa **AzureBatchLinkedService**, joka kertoo, mukautetut aktiviteetit tarvitsee Azure erän Azure Data Factory.

    -   **Samanaikainen** -asetus on tärkeää. Jos käytössäsi on oletusarvo, joka on 1, vaikka sinulla on 2 tai Lisää Laske Azure erä resurssivarantoon solmujen, sektorit käsitellään peräkkäin. Sen vuoksi sinun on ei hyödyntämistä Azure erän rinnakkain käsittely-ominaisuus. Jos määrität **samanaikainen** suuremmat arvot, vaikkapa 2, se tarkoittaa sitä, että kaksi sektoreiden (vastaa kahden tehtävän Azure osalta) voidaan käsitellä samalla kertaa, jolloin sekä VMs Azure erän varanto on käytössä. Tämän vuoksi ominaisuuden samanaikainen asianmukaisesti.

    -   Vain yhtä tehtävää (sektoria) on suoritettu oletusarvoisesti AM milloin tahansa. Syy on, että oletusarvoisesti **Suurin tehtäviä AM kohden** on määritetty Azure erä resurssivarantoon 1. Osana edellytykset resurssivarantoon luoduissa tämän ominaisuuden arvoksi 2, jotta kaksi Data Factory sektoreiden voi käyttää AM samanaikaisesti.


    -   **isPaused** -ominaisuudeksi on määritetty epätosi oletusarvoisesti. Putkijohto suorittaa välittömästi tässä esimerkissä, koska sektorit käynnistää menneisyydessä. Voit määrittää tämän ominaisuuden TOSI, jos haluat keskeyttää putkijohto ja aseta se takaisin käynnistämään EPÄTOSI.

    -   **Alkamisaika** - ja **päättymisajat** ovat viiden tunnin toisistaan ja sektoreiden on valmistettu kerran tunnissa, jolloin viisi sektoreiden on valmistettu putkijohto mukaan.

3.  Valitse **Ota käyttöön** ottamaan putkijohto komentopalkista.

#### <a name="step-5-test-the-pipeline"></a>Vaihe 5: Testaa putkijohto

Tässä vaiheessa voit testata putkijohto pudottamalla tiedostot syötteen kansioihin. Aloitetaan testaaminen putkijohto yhden syötteen kansion tiedostoon.

1.  Valitse **kaavio**Azure-portaalissa Data Factory-sivu.

    ![](./media/data-factory-data-processing-using-batch/image10.png)

2.  Kaavionäkymässä, kaksoisnapsauta syötteen tietojoukko: **InputDataset**.

    ![](./media/data-factory-data-processing-using-batch/image11.png)

3.  Raportissa pitäisi näkyä kaikki viisi sektorit valmis kanssa **InputDataset** -sivu. Huomaa **SEKTORIA ALOITUSAIKA** ja **PÄÄTTYMISAIKA SEKTORIA** kunkin sektorin.

    ![](./media/data-factory-data-processing-using-batch/image12.png)

4.  Valitse **OutputDataset**nyt **Kaavionäkymä**.

5.  Olisi huomaat, että viisi tulosteen sektorit on valmis-tilaan, jos ne on jo valmistettu.

    ![](./media/data-factory-data-processing-using-batch/image13.png)

6.  Azure portal avulla voit tarkastella **sektoreiden** liittyvät **tehtävät** ja nähdä, mitä kukin sektori toimi AM. Katso [tietoja Factory ja erä integrointi](#data-factory-and-batch-integration) -osassa. 

7.  Raportissa pitäisi näkyä **outputfolder** **mycontainer** tulostus-tiedostoja Azuren Blob-objektien tallennustilaan.

    ![](./media/data-factory-data-processing-using-batch/image15.png)

    Raportissa pitäisi näkyä viisi tulostus-tiedostoja, yhteen kunkin syötteen sektoria. Kunkin kohdetiedosto pitäisi olla seuraava tulos muistuttaa sisältö:

        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.

    Seuraavassa kaaviossa on kuvattu, miten Data Factory sektorit yhdistäminen tehtävät Azure osalta. Tässä esimerkissä on sektori on vain yksi Suorita.

    ![](./media/data-factory-data-processing-using-batch/image16.png)

8.  Nyt yritetään useita tiedostoja kansioon. Tiedostojen luominen: **file2.txt**, **file3.txt**, **file4.txt**ja **file5.txt** , jolla on sama sisältö, kuten file.txt kansiossa: **2015-11-06-01**.

9.  Tulostus-kansioon, **Poista** kohdetiedosto: **2015-11 – 16-01.txt**.

10. Napsauta nyt sektoria **OutputDataset** , sivu **SEKTORIA ALOITUSAIKA** asettaminen **11/16/2015 01:00:00 Suomen**, ja valitse **Suorita** , Suorita/uudelleen-process sektoria. Nyt sektoria on viisi tiedostoja yhden tiedoston sijaan.

    ![](./media/data-factory-data-processing-using-batch/image17.png)

11. Kun sektoria suoritetaan ja sen tila on **Valmis**, tarkista tämän sektorin (**2015-11 – 16-01.txt**) kohdetiedosto sisällön **mycontainer** Blob-objektien tallennustilaan **outputfolder** . Pitäisi olla kaikille tiedostoille sektoria viivan.

        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
        2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.


> [AZURE.NOTE] Jos et poistaa tulosteen tiedoston 2015-11 – 16-01.txt ennen kuin yrität viisi input-tiedostoja, näet yhden rivin edelliseen sektoria asennuksesta ja viisi riviä nykyisen sektoria asennuksesta. Oletusarvon mukaan sisältöä liitetään tulosteen tiedoston, jos se on jo olemassa.

#### <a name="data-factory-and-batch-integration"></a>Tietoja Factory ja erä integrointi
Data Factory-palvelu luo työn Azure erän nimi: **syöttö-poolname:job-xxx**. 

![Azure tietojen Factory - Ehdota](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Projektin tehtävän luodaan sektoria kunkin tehtävän suorittamisen. Jos määritettynä on 10 sektoreiden valmis käsiteltäväksi, 10 tehtävät luodaan työn. Käytössä voi olla useampi kuin yksi sektori suorittaminen rinnakkain, jos sinulla on useita Laske solmujen varannon. Jos suurin tehtävien suorittaminen solmu kohden on määritetty > 1, voi olla useampi kuin yksi sektori käytössä sama suorittaminen.

Tässä esimerkissä on viisi sektoreiden, joten viisi tehtävät Azure osalta. **Samanaikainen** arvoksi **5** putkisto-ja Azure Data Factory ja **Suurin-ruutuun tehtävät kohti AM** JSON arvoksi **2** Azure erä varannon **2** VMs, tehtäviä suoritetaan nopea (Tarkista tehtävien alkamis- ja päättymisajat).

Portaalin avulla voit tarkastella erän työ ja sen tehtävät, jotka liittyvät **sektoreiden** ja mitä kukin sektori toimi AM. 

![Azure tietojen Factory - erä projektitehtävät](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>Putkijohto korjaaminen

Tavallinen muutama tapa virheenkorjaus kuuluu:

1.  Jos syötteen sektoria ei ole määritetty **valmiiksi**, varmista, että syötteen kansiorakenne ovat oikein ja file.txt olemassa syötteen kansioihin.

    ![](./media/data-factory-data-processing-using-batch/image3.png)

2.  Kirjaudu tiedot, jotka auttavat ongelmien vianmääritys mukautetun tehtävän **Execute** -menetelmä **IActivityLogger** objektin avulla. Kirjautuneena viestit näkyvät käyttäjän\_0. lokitiedoston.

    Valitse **OutputDataset** , sivu sektoria näkevän sitä varten **Tietojen MUOTOILU** -sivu. Näet **tehtävän suoritetaan** sitä. Raportissa pitäisi näkyä suorittaa sektoria yksi tehtävä. Jos valitset **Suorita** painikkeita, voit aloittaa suorittaa saman sektoria toiseen tehtävään.

    Kun napsautat tehtävän suorittaminen, näet ja lokitiedostojen luettelo **Tehtävän suorittaminen tiedot** -sivu. Näet kirjautuneena viestit **käyttäjän\_0. lokin** tiedosto. Kun näyttöön tulee virhesanoma, näet kolme tehtävä on suoritettu koska uudelleenyritysmäärä on määritetty putkijohto ja tehtävän JSON 3. Kun napsautat tehtävän suorittaminen, näet lokitiedostojen, voit tarkastella virheen vianmääritys.

    ![](./media/data-factory-data-processing-using-batch/image18.png)

    Valitse lokitiedostojen-luettelosta **käyttäjän 0.log**. Valitse oikeassa paneelissa ovat tulokset **IActivityLogger.Write** -menetelmällä.

    ![](./media/data-factory-data-processing-using-batch/image19.png)

    Valitse järjestelmä-0.log järjestelmän virhesanomat ja poikkeukset.

        Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...

        Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...

        Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module

        Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully

3.  Sisällytä **PDB** -tiedoston zip-tiedoston niin, että virheen yksityiskohtaiset tiedot on tietoja, kuten **Soita pinon** , kun näyttöön tulee virhesanoma.

4.  Kaikki tiedostot, mukautetut aktiviteetit zip-tiedosto on oltava **ylimmän tason** ei alikansiot kanssa.

    ![](./media/data-factory-data-processing-using-batch/image20.png)

5.  Varmista, että **assemblyName** (MyDotNetActivity.dll), **aloituskohta** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip) ja **packageLinkedService** (osoitettava Azure-blob-säiliö, joka sisältää zip-tiedoston) on määritetty oikein arvot.

6.  Jos kiinteä virheen ja haluat Käsittele sektoria uudelleen, napsauta **OutputDataset** sivu sektoria hiiren kakkospainikkeella ja valitse **Suorita**.

    ![](./media/data-factory-data-processing-using-batch/image21.png)

    > [AZURE.NOTE] 
    > Näet **säilö** nimeltä Azure-Blob-objektien tallennustilaan: **adfjobs**. Tämä säilö ei poisteta automaattisesti, mutta voit turvallisesti poistaa sen, kun olet valmis testaus ratkaisu. Vastaavasti Data Factory-ratkaisun Luo luodun Azure erän **työn** nimeltä: **syöttö -\<altaan tunnusnimi/\>: työn 0000000001**. Voit poistaa tämän työn jälkeen voit testata ratkaisun halutessasi.
7. Mukautetut aktiviteetit ei käytä paketti **app.config** -tiedostosta. Sen vuoksi, jos koodi lukee kaikki yhteyden merkkijonot määritystiedostoa, se ei toimi suorituksen aikana. Parhaiden käytäntöjen avulla Azure erän ollessa pitoon mitään tietoja **Azure KeyVault**käyttää varmenteen-pohjaisessa palvelussa lyhennys keyvault suojata ja jakaa sertifikaatin Azure erä resurssivarannon avulla. .NET mukautetut aktiviteetit jälkeen voit käyttää tietoja KeyVault suorituksen aikana. Tämä ratkaisu on yleinen ja mihin tahansa salainen, ei pelkästään yhteysmerkkijonon skaalata.

    On helpompi vaihtoehtoinen (mutta ei parhaiden käytäntöjen mukaisesti): Voit luoda **Azure SQL-linkitetty palvelun** yhteydessä merkkijonon asetukset, tietojoukon, joka käyttää linkitetyn palvelun luominen, ja ketjut dataset kuin tyhjä syötteen tietojoukko mukautetun .NET-toimintoon. Voit käyttää linkitetyn palvelun yhteysmerkkijonon mukautetut aktiviteetit koodissa ja se toimii moitteettomasti suorituksen aikana.  

#### <a name="extend-the-sample"></a>Laajenna otosten

Voit laajentaa tässä esimerkissä lisätietoja Azure Data Factory ja Azure erä ominaisuuksia. Käsitellä sektoreiden eri aikaväli, toimi seuraavasti:

1.  Lisää seuraavat alikansiot **inputfolder**: 2015-11 – 16-05, 2015 11 – 16-06 201-11 – 16-07, 2011-11 – 16-08, 2015-11 – 16-09 ja Aseta syöte näiden kansioissa olevia tiedostoja. Muuta myyntijakso päättymisaika `2015-11-16T05:00:00Z` , `2015-11-16T10:00:00Z`. **Kaavionäkymä**Kaksoisnapsauta **InputDataset**ja vahvista syötteen sektorit ovat valmiita. Kaksoisnapsauta **OuptutDataset** Nähdäksesi tulosteen sektoreiden tila. Jos ne ovat valmiina, valitse outputfolder tulostus-tiedostoja.

2.  Suurenna tai Pienennä selvittääksesi, miten se vaikuttaa suorituskykyyn erityisesti, joka esiintyy Azure erän käsittely-ratkaisun **samanaikainen** -asetus. (Vaihe 4: luominen ja suorittaminen putkijohto Saat lisätietoja **samanaikainen** -asetusta.)

3.  Luo resurssivarantoon suurempi/pienet **enintään tehtäviä AM kohden**. Käytä luomasi uusi ryhmä, Päivitä Azure-erä linkitetty palvelun Data Factory-ratkaisussa. (Vaihe 4: luominen ja suorittaminen putkijohto Saat lisätietoja **enintään tehtäviä kohden AM** -asetusta.)

4.  Voit luoda Azure erä resurssivarantoon **Automaattinen skaalaus** -ominaisuus. Skaalaus automaattisesti Azure erä-ryhmän Laske solmujen muutetaan dynaaminen käsittelyn sovelluksen käyttämää. Voit esimerkiksi luoda azure erä resurssivarantoon 0 erillinen VMs ja Automaattinen skaalaus-kaava, montako odottavat tehtävät:
 
    Yhden AM kohti odottaa tehtävän kerralla (esimerkiksi: Anna odottavat tehtävät -> viisi VMs):

        pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
        $TargetDedicated = max(pendingTaskSampleVector);

    Yksi kerrallaan riippumatta odottavien tehtävien määrä AM Maks:

        pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
        $TargetDedicated = (max(pendingTaskSampleVector)>0)?1:0;

    Katso lisätietoja [automaattisesti asteikko Laske solmujen Azure erä resurssivarantoon](../batch/batch-automatic-scaling.md) . 

    Jos altaan on käytössä oletusarvoisesti [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), erä-palvelua voi kestää 15-30 minuuttia, ennen kuin suoritat mukautetut aktiviteetit AM valmistelemaan.  Jos altaan käyttää eri autoScaleEvaluationInterval, erä-palvelun voi kestää autoScaleEvaluationInterval + 10 minuuttia. 
     
5. Esimerkki-ratkaisussa **Execute** -menetelmä käynnistää **Laske** menetelmän, joka käsittelee syöttötiedot-sektori tuottamaan tulostus-tietojen muotoilu. Voit kirjoittaa omia menetelmä Käsittele syöttötiedot ja Laske menetelmä puhelu Execute-menetelmä korvaa menetelmä kutsu.

 


### <a name="next-steps-consume-the-data"></a>Seuraavat vaiheet: tarjoaman tiedot

Kun voit käsitellä tietoja, voit käyttää sen online työkaluja, kuten **Microsoft Power BI**. Seuraavassa on linkkejä voi auttaa sinua ymmärtämään Power BI- ja miten sitä käytetään Azure:

-   [Tutustu Power BI-tietojoukko](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-data/)

-   [Power BI Desktopin käytön aloittaminen](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/)

-   [Power BI-tietojen päivittäminen](https://powerbi.microsoft.com/en-us/documentation/powerbi-refresh-data/)

-   [Azure- ja Power BI - yhteenveto](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Viittaukset

-   [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

    -   [Azure Data Factory-palvelun esittely](data-factory-introduction.md)

    -   [Azure Data Factory käytön aloittaminen](data-factory-build-your-first-pipeline.md)

    -   [Mukautettujen toimintojen käyttäminen Azure Data Factory-myyntijakso](data-factory-use-custom-activities.md)

-   [Azure erä](https://azure.microsoft.com/documentation/services/batch/)

    -   [Azure erä perusteet](../batch/batch-technical-overview.md)

    -   [Azure erä ominaisuuksien yleiskatsaus](../batch/batch-api-basics.md)

    -   [Voit luoda ja hallita Azure erä tilin Azure-portaalissa](../batch/batch-account-create-portal.md)

    -   [Azure erä kirjaston .NET käytön aloittaminen](../batch/batch-dotnet-get-started.md)


[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx

