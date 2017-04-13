
<properties 
    pageTitle="Kalusto- ja Media Services .NET SDK liittyvien kohteiden hallinta" 
    description="Opi hallitsemaan varat ja liittyvien kohteiden kanssa Media Services SDK .NET varten." 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016"
    ms.author="juliako"/>


#<a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Kalusto- ja Media Services .NET SDK liittyvien kohteiden hallinta


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-manage-entities.md)
- [MUILLE KÄYTTÄJILLE](media-services-rest-manage-entities.md)


Tässä ohjeaiheessa esitellään, miten suoritettava Media Services hallintatehtäviä:

- Hae resurssi-viittaus 
- Hae työn viittaus 
- Luettele kaikki resurssit 
- Luettelo projektien ja resurssien 
- Luettele kaikki access-käytännöt 
- Luettele kaikki Locators
- Luettelointi suuria sivustokokoelmat kohteiden kautta
- Sijoituksen poistaminen 
- Työn poistaminen 
- Poista käyttöoikeuskäytäntö 

##<a name="prerequisites"></a>Edellytykset 

Katso [Lisätietoja ympäristön määrittäminen](media-services-set-up-computer.md)

##<a name="get-an-asset-reference"></a>Hae resurssi-viittaus

Usein käytetyt tehtävä on viittaaminen aiemmin resurssi Media Services-palveluissa. Koodin seuraavassa esimerkissä esitetään, miten voit saada resurssi viittaus palvelimessa kalusto-valikoimasta konteksti-objektin, omaisuuden tunnuksen perusteella
Seuraava koodiesimerkki objektin IAsset viittaaminen Linq kyselyn avulla.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

##<a name="get-a-job-reference"></a>Hae työn viittaus

Käsiteltäessä Media Services koodin tehtävien käsittely usein tarvitset tunnuksen perusteella aiemmin luodun työn viittaus Seuraava koodi-esimerkissä hankkiminen IJob objektin viittaus työt-valikoimasta.
WarningWarning voit joutua haluat työn viitata pitkään suoritettavien koodauksen työtä aloitettaessa, ja tarkista säikeen työn-tila on. Tältä tapauksissa menetelmä palauttaa säikeen, kun haluat hakea päivitetyt viittaus työn.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();
    
        return job;
    }

##<a name="list-all-assets"></a>Luettele kaikki resurssit

Kun resurssit ovat tallennustilan määrä kasvaa, kannattaa luettelo oman resurssit. Seuraava koodi-esimerkissä esitetään, kuinka haluat käydä läpi palvelimen konteksti objektin kalusto-sivustokokoelman. Kunkin resurssi ja koodiesimerkki myös kirjoittaa joitakin sen ominaisuusarvoihin konsolin. Esimerkiksi kunkin resurssi voi olla useita mediatiedostot. Esimerkki koodista kirjoittaa kaikki tiedostot, jotka liittyvät kunkin kohteiden ulos.



    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-jobs-and-assets"></a>Luettelo projektien ja resurssien

Tärkeää tehtävästä on luettelon resurssien liittyvät työssään Media Services-palveluissa. Koodin seuraavassa esimerkissä kerrotaan, miten luettelon IJob objekteja ja sitten kunkin projektin se näyttää tietoja työn ominaisuuksista, kaikki siihen liittyvät tehtävät, kaikki Lisää varat ja kaikki tulosteen varat. Tässä esimerkissä koodi voi olla hyötyä monia muita tehtäviä. Jos haluat luettelon tulosteen omaisuuden koodauksen työt, jotka suoritit aiemmin, koodi näyttää tulostus-resurssien käyttäminen. Kun tulostus-resurssi viittaus, voit sitten näyttää sisältöä muille käyttäjille tai sovellusten lataamalla se ja tarjoaa URL-osoitteet. 

Katso lisätietoja käyttöönotto välittää kalusto- [Toimittaa resurssien .NET Media Services SDK-paketissa](media-services-deliver-streaming-content.md).

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");
    
    
            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }
    
            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {
    
                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-all-access-policies"></a>Luettele kaikki Access-käytännöt

Media-palveluja voit määrittää käyttöoikeuskäytäntö sijoituksen tai sen tiedostot. Access-käytännöllä määritetään tiedoston tai omaisuuden (millaista access ja keston) käyttöoikeuksia. Media Services-koodisi yleensä määrittää käyttöoikeuskäytäntö IAccessPolicy-objektin luominen ja liittäminen aiemmin resurssi. Sitten voit luoda ILocator-objekti, jonka avulla voit antaa tutustuminen varat Media Services-palveluissa. Visual Studio projektin mukana toimitettava dokumentaatio sarjassa on useita koodiesimerkkejä, jotka osoittavat, miten voit luoda ja liittää resurssien käytön käytännöt ja locators.

Seuraava koodiesimerkki näyttää, miten luettelon kaikki access-käytännöt palvelimessa ja näyttää kunkin liittyvät oikeudet tyypin. Toinen hyödyllinen tapa näyttää käytäntöjen on luettelo ILocator objektien palvelimeen ja sitten kunkin locator, voit lisätä sen liittyvät käyttöoikeuskäytäntö sen AccessPolicy-ominaisuuden avulla.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");
    
        }
    }

##<a name="list-all-locators"></a>Luettele kaikki Locators

Locator on URL-osoite, jossa on suora polku käyttää omaisuuden sekä kohteen käyttöoikeudet locator liittyvät käyttöoikeuskäytäntö määrittämä. Kunkin resurssi voi olla joukko Locators sen ominaisuuden liittyy ILocator objekteja. Konteksti on myös Locators kokoelma, joka sisältää kaikki locators.

Seuraava koodiesimerkki näyttää kaikki locators palvelimessa. Se näyttää kunkin locator resurssi ja Accessin käytännön tunnus. Se myös näyttää käyttöoikeudet, päättymispäivän ja koko polku tyypin kohteen.

Huomaa, että amerikkalaisen locator polku on vain kohteen perus URL. Luo yksittäisiä tiedostoja, jotka käyttäjä tai sovellus saattaa Siirry suoraan polku-koodi on lisättävä tietyn tiedostopolku locator polku. Lisätietoja hallinnasta on ohjeaiheessa [Toimittaa resurssien .NET Media Services SDK-paketissa](media-services-deliver-streaming-content.md).

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>Luettelointi suuria sivustokokoelmat kohteiden kautta

Kun kyselyt kohteita ei palauttaa yhtä aikaa, koska julkisen muiden v2 rajoittaa kyselyn tuloksia 1000 tulokset 1000 kohteiden enimmäismäärä. Sinun on käytettävä Ohita ja ota, kun suuri sivustokokoelmat kohteiden kautta. 
    
Seuraavan funktion silmukat kaikki työt annettu Media Services-tilin kautta. Media-palveluiden palauttaa 1000 töitä projektien sivustokokoelman. Funktion tekee käyttäminen Ohita ja ota varmistaaksesi, että kaikki työ on lueteltu (siltä varalta, että tilisi on yli 1 000 töitä).
    
    static void ProcessJobs()
    {
        try
        {
    
            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;
    
            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }
    
                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

##<a name="delete-an-asset"></a>Sijoituksen poistaminen

Seuraavassa esimerkissä poistaa sijoituksen.

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();
    
        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");
    
    }

##<a name="delete-a-job"></a>Työn poistaminen

Työn poistaminen Tarkista työn tila tila-ominaisuuden esitetyllä tavalla. Työt valmiiksi tai peruutettuja voi poistaa, kun tietyt muut tilat, kuten jonossa, ajoitettu tai käsittelyyn, olevat työt on peruutettava ensin ja sitten niitä voi poistaa.
Koodin seuraavassa esimerkissä esitetään menetelmän työn poistamisen työn tilat tarkistus ja poistamalla sitten kun tila on päättynyt tai se on peruutettu. Tämä koodi määräytyy edellisessä osassa tehostavia työn viittaa tämän artikkelin: haluat työn viitata.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;
    
        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);
    
            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }
    
        }
    }


##<a name="delete-an-access-policy"></a>Poista käyttöoikeuskäytäntö

Koodin seuraavassa esimerkissä esitetään käytännön tunnuksen perusteella käyttöoikeuskäytäntö viittaa hankkiminen ja sen jälkeen voit poistaa käytännön.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();
    
        policy.Delete();
    
    }
    


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
