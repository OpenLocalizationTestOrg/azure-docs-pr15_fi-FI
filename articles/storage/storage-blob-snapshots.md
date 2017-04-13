<properties
    pageTitle="Vain luku-blob tilannevedoksen luominen | Microsoft Azure"
    description="Opettele luomaan tilannevedoksen Blob-objektien varmuuskopioida Blob-objektien tiedot ajoissa tietyllä hetkellä. Lisätietoja siitä, miten tilannevedoksia ovat laskuttaa sekä niiden avulla voit pienentää kapasiteetin kulut."
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

# <a name="create-a-blob-snapshot"></a>Blob-objektien tilannevedoksen luominen

## <a name="overview"></a>Yleiskatsaus

Tilannevedoksen on vain luku-versio, joka on otettu vaiheessa ajassa Blob-objektien. Tilannevedoksia on hyötyä BLOB varmuuskopioiminen. Kun olet luonut tilannevedoksen, voit lukea, kopioida tai poistaa sen, mutta et voi muokata.

Tilannevedoksen blob on sama kuin sen perus blob, paitsi että Blob-objektien URI on Blob-objektien URI osoittamaan kellonajan, jolloin tilannevedoksen on otettu **DateTime** -arvo. Jos sivun blob URI on esimerkiksi `http://storagesample.core.blob.windows.net/mydrives/myvhd`, URI muistuttaa tilannevedoksen `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`. 

> [AZURE.NOTE] Kaikki tilannevedoksia jakaa perus Blob-objektien URI. Vain perus Blob-objektien ja tilannevedoksen välinen ero on liitetyssä **DateTime** -arvon.

Blob voi olla minkä tahansa tilannevedoksia määrän. Tilannevedoksia käytössä, kunnes ne poistetaan erikseen. Tilannevedoksen et voi outlive sen perus blob. Voit luetteloida perus-Blob-objektien jäljittämiseksi nykyisen tilannevedoksia liittyvät tilannevedoksia.

Kun luot tilannevedoksen blob-järjestelmän ominaisuudet Blob-objektien kopioidaan tilannevedoksen, jossa on samat arvot. Perus Blob-objektien metatietojen kopioidaan myös tilannevedoksen, ellet määritä eri metatietojen näyttökuvan luomisen yhteydessä.

Kaikki liittyvät perus Blob-objektien vuokrasopimukset eivät vaikuta tilannevedoksen. Ei voi hankkia tilannevedoksen varausten.

## <a name="create-a-snapshot"></a>Tilannevedoksen luominen

Seuraava koodi-esimerkissä tilannevedoksen luominen .NET. Tässä esimerkissä määrittää erillisen metatietojen näyttökuvan luomisen yhteydessä.

    private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
    {
        // Create a new block blob in the container.
        CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

        // Add blob metadata.
        baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

        try
        {
            // Upload the blob to create it, with its metadata.
            await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

            // Sleep 5 seconds.
            System.Threading.Thread.Sleep(5000);

            // Create a snapshot of the base blob.
            // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
            // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
            Dictionary<string, string> metadata = new Dictionary<string, string>();
            metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
            await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
        }
        catch (StorageException e)
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
 

## <a name="copy-snapshots"></a>Kopioi tilannevedokset

Kopiointi BLOB-objektit ja tilannevedoksia noudata seuraavia sääntöjä:

- Voit kopioida tilannevedoksen sen perus Blob-objektien päälle. Edistää tilannevedoksen perus Blob-objektien sijaintiin, voit palauttaa blob aiemmassa versiossa. Tilannevedoksen säilyy, mutta kantaluku blob korvautuu tilannevedoksen kirjoitettavan kopion.

- Voit kopioida tilannevedoksen kohde Blob-objektien eri nimellä. Tuloksena oleva kohde Blob-objektien on kirjoitettava Blob-objektien ja tilannevedoksen luominen.

- Lähde-Blob-objektien kopioitaessa minkä tahansa lähde-blob-tilannevedosten ei kopioida kohteeseen. Kun kohde Blob-objektien korvautuu liitteenä, kaikki tilannevedoksia, alkuperäinen kohde Blob-objektien liittyvät säilyvät ennallaan.

- Kun luot tilannevedoksen lohko-blob-Blob-objektien vahvistettu ruutuluettelo kopioidaan myös tilannevedoksen. Ei mitään ei ole lohkot kopioidaan.

## <a name="specify-an-access-condition"></a>Määritä access-ehto

Voit määrittää access-ehdon niin, että tilannevedoksen luodaan vain, jos annettu ehto täyttyy. Voit määrittää access-ehto, **AccessCondition** -ominaisuuden avulla. Jos määritetty ehto ei täyty, tilannevedoksen ei ole luotu ja Blob-palvelu palauttaa tilakoodin HTTPStatusCode.PreconditionFailed.

## <a name="delete-snapshots"></a>Poista tilannevedokset

Et voi poistaa Blob-objektien kanssa tilannevedoksia, ellei tilannevedosten poistetaan myös. Voit poistaa tilannevedoksen yksitellen tai määrittää, että kaikki tilannevedoksia poistetaan, kun lähteen Blob-objektien poistetaan. Jos yrität poistaa Blob-objektien, joka on vielä tilannevedoksia, tuloksena on virhe.

Koodin seuraavassa esimerkissä esitetään poistamisesta blob ja sen tilannevedoksia .NET, jossa `blockBlob` on **CloudBlockBlob**muuttujan:

    await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);

## <a name="snapshots-with-azure-premium-storage"></a>Tilannevedoksia Azure Premium tallennustilan kanssa

Käyttäminen tilannevedoksia Premium tallennustilan noudattamalla seuraavia sääntöjä:

- Sivun Blob-objektien premium-tallennustilan tilin kohti tilannevedoksia enimmäismäärä on 100. Jos rajoituksen ylittyy, tilannevedos Blob-toiminto palauttaa virhekoodin 409 (**SnapshotCountExceeded**).

- Voit ottaa talteen tilannevedoksia sivun Blob-objektien premium-tallennustilan tilin 10 minuutin välein. Jos korko ylittyy, tilannevedos Blob-toiminto palauttaa virhekoodin 409 (**SnaphotOperationRateExceeded**).

- Et voi kutsua Hae Blob-objektien lukemaan tilannevedoksen sivun Blob-objektien premium tallennustilan tilillä. Soittavan Hae Blob-tilannevedoksen premium-tallennustilan tilin palauttaa virhekoodin 400 (**InvalidOperation**). Voit kuitenkin kutsua Hae Blob-objektien ominaisuudet ja Hae Blob metatiedot vastaan tilannevedoksen premium tallennustilan tilillä.

- Lukemaan tilannevedoksen tilannevedoksen kopioiminen toisen sivun Blob-objektien tilin kopioi Blob-toiminnon avulla. Kopiointi kohde-blob ei saa sisältää kaikki aiemmin tilannevedoksia. Jos kohde Blob-objektien tilannevedoksia, kopioi Blob-toiminto palauttaa virhekoodin 409 (**SnapshotsPresent**).

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Absoluuttinen URI palaa tilannevedoksen

C# koodin esimerkin luo tilannevedoksen, ja kirjoittaa ulos ensisijainen sijainti absoluuttinen URI.

    //Create the blob service client object.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();

    //Get a reference to a blob.
    CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
    blob.UploadText("This is a blob.");

    //Create a snapshot of the blob and write out its primary URI.
    CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
    Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);

## <a name="understand-how-snapshots-accrue-charges"></a>Kulujen kertymistavan tilannevedoksia ymmärtäminen

Tilannevedoksen, joka on vain luku-kopion blob, voi aiheuttaa Lisää tallennustilaa tiedonsiirtomaksuihin tiliisi. Sovelluksen suunniteltaessa on tärkeä ottaa huomioon nämä ovat ehkä kertymistavan niin, että voit pienentää tarpeettomia kustannuksia.

### <a name="important-billing-considerations"></a>Laskutuksen tärkeitä

Seuraava luettelo sisältää tärkeimmät kohdat ottaa huomioon, kun tilannevedoksen luominen.

- Tallennustilan tilin veloitetaan yksilöllinen lohkot tai sivuja, olivatpa ne sitten blob- tai tilannevedoksen. Tilisi ei maksamaan tilannevedoksia liittyvät blob, kunnes päivität blob, johon ne perustuvat muita kulut. Kun olet päivittänyt perus Blob-objektien, se ohjeistosta-sen tilannevedoksia. Tällöin perittävän yksilöllisiä lohkot tai sivuja kunkin Blob-objektien tai tilannevedoksen.

- Kun korvaat tekstilohkon sisällä lohko-blob-, estä veloitetaan myöhemmin yksilöllinen ruutuna. Tämä pätee, vaikka esto on sama lohko-tunnus ja samat tiedot kuin se on tilannevedoksen. Kun esto on vahvistettu uudelleen, se ohjeistosta-minkä tahansa tilannevedoksen vastaavaan ja olet veloitetaanko tietonsa. Sama pitää paikkansa sivun sivun Blob-objektien, joka on päivitetty samat tiedot.

- Estä Blob-objektien korvaamiseen **UploadFile**, **UploadText**, **UploadStream**tai **UploadByteArray** kutsumista korvaa kaikki lohkot blob. Jos sinulla on kyseisen blob liittyvät tilannevedoksen, kaikki lohkot perus Blob-objektien ja tilannevedoksen nyt diverge ja veloitetaan kaikkien sekä BLOB-lohkot. Tämä pätee, vaikka perus Blob-objektien ja tilannevedoksen tiedot ovat samat.

- Azure-Blob-palvelu ei ole keino selvittää, onko kaksi sisältää samanlaisia tietoja. Kukin lohko, joka on ladattu ja vahvistetun käsitellään yksilöllinen, vaikka se on samat tiedot ja sama estä. Koska kulujen kertymä yksilöllinen tekstilohkojen, on tärkeä ottaa huomioon, päivittämässä Blob-objektien, jossa on tilannevedoksen johtaa muita yksilöllinen lohkot ja muut kulut.

> [AZURE.NOTE] Parhaat käytännöt määräävät hallinta tilannevedoksia huolellisesti ylimääräisiä lisämaksujen välttämiseksi. On suositeltavaa hallinta tilannevedoksia seuraavalla tavalla:

> - Poistaa ja luo uudelleen tilannevedoksia liittyvät blob aina, kun päivität Blob-objektien, vaikka olet päivittämässä identtisillä tiedoilla, jollei sovellus-rakenne edellyttää, että sinulla on tilannevedoksia. Poistaminen ja luominen uudelleen Blob-objektien tilannevedoksia, voit varmistaa, että Blob-objektien ja tilannevedoksia ei diverge.

> - Jos ovat ylläpito blob tilannevedoksia, vältä kutsumista **UploadFile**, **UploadText**, **UploadStream**tai **UploadByteArray** päivittämään blob. Näistä menetelmistä korvaa kaikki-blob-lohkot, niin, että perus Blob-objektien ja tilannevedoksia diverge lopetetuista toiminnoista. Päivitä sen sijaan lohkot mahdollista vähiten **PutBlock** ja **PutBlockList** tavoilla.


### <a name="snapshot-billing-scenarios"></a>Tilannevedoksen Laskutus skenaariot


Seuraavat esimerkkitapausta kuvaavat kulujen kertymistavan estä Blob-objektien ja sen tilannevedoksia.

1 tilanteessa perus Blob-objektien ei ole päivitetty sen jälkeen, kun tilannevedos on tehty, niin kulut ovat kertyneet vain yksilöllisiä lohkot 1, 2 ja 3.

![Azure tallennustilan resurssit](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

Käyttötilanne 2 perus Blob-objektien on päivitetty, mutta tilannevedoksen ei ole. Estä 3 on päivitetty ja se sisältää samat tiedot ja sama tunnus, mutta se ei ole sama kuin estää tilannevedoksen 3. Tämän vuoksi tilin veloitetaan neljä lohkot.

![Azure tallennustilan resurssit](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

Tapaus 3 perus Blob-objektien on päivitetty, mutta tilannevedoksen ei ole. Lohko 3 on korvattu lohkon 4-peruste Blob-objektien kanssa, mutta tilannevedoksen kuvastaa edelleen lohko 3. Tämän vuoksi tilin veloitetaan neljä lohkot.

![Azure tallennustilan resurssit](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

4 tilanteessa perus Blob-objektien on päivitetty kokonaan, ja se sisältää yhtäkään sen alkuperäiseen lohkot. Tämän vuoksi tilin veloitetaan kaikki kahdeksan yksilöllinen lohkot. Tässä skenaariossa voi ilmetä, jos käytössäsi on update-menetelmää, kuten **UploadFile**, **UploadText**, **UploadFromStream**tai **UploadByteArray**, koska näistä tavoista korvaa kaikki blob sisällön.

![Azure tallennustilan resurssit](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Katso Lisää esimerkkejä Blob-objektien tallennustilaan, [Azure MALLIKOODEJA](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Voit ladata sovelluksen malli ja suorittaa sen tai Selaa GitHub koodia. 
