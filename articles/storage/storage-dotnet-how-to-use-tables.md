<properties
    pageTitle="Pääset alkuun käyttämällä .NET Azure-taulukkotallennus | Microsoft Azure"
    description="Voit tallentaa jäsenneltyjen tietojen avulla Azure-taulukkotallennus, NoSQL tietosäilö pilvipalveluun."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-table-storage-using-net"></a>Pääset alkuun käyttämällä .NET Azure-taulukkotallennus

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Yleiskatsaus

Azure-taulukkotallennus on palvelu, joka tallentaa NoSQL jäsenneltyjen tietojen pilveen. Taulukkotallennus on avain/määrite säilö schemaless suunnitteleminen. Koska taulukkotallennus on schemaless, on helppo mukauttaa tietojen sovelluksen evolve tarpeisiin. Tietojen käyttäminen on nopea ja edullinen kaikenlaisia sovelluksia varten. Taulukkotallennus on yleensä kustannukset merkittävästi pienempi kuin perinteisen SQL-samalla tietomääristä.

Taulukkotallennus avulla voit tallentaa joustavia tietojoukkoja, kuten verkkosovellusten, osoitteistoja tai laitteen tietojen muuntyyppisen metatiedot, joilla palvelu vaatii käyttäjätiedot. Voit tallentaa minkä tahansa kohteiden määrän taulukon ja tallennustilan tilin voi olla jokin muu luku taulukoiden kapasiteetin raja-tallennustilan tilin ylöspäin.

### <a name="about-this-tutorial"></a>Tässä opetusohjelmassa tietoja

Tässä opetusohjelmassa näytetään, miten voit kirjoittaa .NET koodia havainnollistetaan Yleisiä tilanteita, käyttämällä Azure taulukkotallennus, mukaan lukien luominen ja taulukon poistaminen ja lisääminen, päivittäminen, poistaminen ja taulukkotietojen kyselyt.

**Arvioitu kesto:** 45 minuuttia

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [.NET Azure tallennustilan asiakkaan kirjasto](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [.NET Azure hallintatoiminto](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Azure-tallennustilan tilin](storage-create-storage-account.md#create-a-storage-account)

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Lisää esimerkkejä

Katso Lisää esimerkkejä taulukkotallennus, [.NET Azure-taulukoiden tallennustilaan käytön aloittaminen](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Voit ladata sovelluksen malli ja suorittaa sen tai Selaa GitHub koodia.


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Lisää nimitilan ilmoitukset

Lisää seuraava `using` lauseet yläreunaan `program.cs` tiedosto:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types

### <a name="parse-the-connection-string"></a>Yhteysmerkkijonon jäsentää

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>Luo taulukko-palvelun asiakasohjelmaa

**CloudTableClient** -luokan avulla voit hakea taulukoiden tallennustilaan tallennettuja kohteita. Näin voit luoda palvelun-asiakasohjelmaa:

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

Nyt olet valmis, joka lukee tietoja ja kirjoittaa tietoja taulukkotallennus koodin kirjoittaminen.

## <a name="create-a-table"></a>Taulukon luominen

Tässä esimerkissä näytetään, miten voit luoda taulukon, jos sitä ei ole jo:

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Retrieve a reference to the table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table if it doesn't exist.
    table.CreateIfNotExists();

## <a name="add-an-entity-to-a-table"></a>Kohteen lisääminen taulukkoon

Kohteiden liittäminen C\# objektit käyttämällä mukautetun luokan johdettu **TableEntity**. Jos haluat lisätä kohteen taulukon, Luo luokka, joka määrittää oman kohteen ominaisuudet. Seuraava koodi määrittää toimija-luokka, joka käyttää asiakkaan Etunimi Sukunimi ja rivi-näppäintä osio-näppäintä. Yhdessä yksikön osio ja rivin avaimen yksilöivät kohde taulukossa. Kohteiden samalla osion avaimella voidaan suorittaa kysely nopeammin kuin sisältävän eri osioon näppäimet, mutta monipuolisen osio-näppäimellä mahdollistaa suurempi skaalattavuus rinnakkain-toimintoa.  Ominaisuuden, joka tallennetaan taulukon service-ominaisuuden on oltava tuettu tyypin, joka paljastaa sekä julkinen ominaisuus `get` ja `set`.
Myös että kohteen tyyppi *on* Näytä parametrin pienempi konstruktoria.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Taulukkotoiminnot, jotka liittyvät kohteet suoritetaan kautta **CloudTable** objektia, jonka loit aiemmin "Luo taulukon"-osassa. Suoritettava toiminto edustaa **TableOperation** objekti.  Seuraava koodi-esimerkissä **CloudTable** -objekti ja valitse **CustomerEntity** -objektin luominen.  Toiminnon valmistelemaan **TableOperation** objektin luodaan asiakkaan kohteen lisääminen taulukkoon.  -Toiminto on suoritettu soittamalla **CloudTable.Execute**.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    table.Execute(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Lisää kohteita erän

Voit lisätä kohteita erän taulukoksi kirjoitus-toiminnossa. Joitakin muita muistiinpanojen erä toimintoja:

-  Voit suorittaa päivitykset, poistaa ja lisää samassa yhden erän-toiminnossa.
-  Yhden erän toiminto voi olla enintään 100 kohteita.
-  Kaikki kohteet yhden erän-toiminnossa on oltava sama osio-näppäintä.
-  Vaikka ei voi suorittaa kysely nimellä eräkäsittelytoimintoa, sen on oltava vain toiminnon osalta.

<!-- -->
Seuraava koodiesimerkki Luo kaksi kohteen objektia ja lisää kunkin **TableBatchOperation** **Lisää** -menetelmällä. **CloudTable.Execute** kutsutaan suorittamaan toimintoa.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    table.ExecuteBatch(batchOperation);

## <a name="retrieve-all-entities-in-a-partition"></a>Hae kaikki kohteet-osio

Kyselyn taulukon kaikkien kohteiden-osion, käytä **TableQuery** objektia.
Seuraavassa esimerkissä koodi määrittää suodattimen kohteiden "Keila" missä osio-näppäintä. Tässä esimerkissä tulostaa kyselytulokset konsoliin kunkin kohteen kenttiä.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Noutamista osioon kohteiden

Jos et halua kyselyn osion kaikkia kohteita, voit määrittää alueen yhdistämällä osion avaimen suodatin, jossa on rivin avaimen suodattimen. Seuraava koodi-esimerkissä käytetään kaksi suodatinta saat kaikki kohteet-osion "Keila", jossa on aiempi kuin "E" aakkosten kirjaimella alkavaan rivin tunnusta (Etunimi) ja tulostaa kyselytulokset.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-single-entity"></a>Yhden kohteen hakeminen

Voit kirjoittaa kyselyn, joka noutaa yksi, tietyn kohteen. Seuraava koodi käyttää **TableOperation** Määritä asiakas "Ari Suominen".
Tämä menetelmä palauttaa yhden kohteen sijaan kokoelma ja **TableResult.Result** palautetun arvon on **CustomerEntity** objekti.
Kyselyn, joka määrittää osio-ja rivi on nopein tapa hakea yksittäisen kohteen taulukon-palvelusta.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="replace-an-entity"></a>Korvaa kohteen

Päivittämään kohteen noutaminen taulukosta-palvelusta, muokata kohteen objektia ja valitse Tallenna muutokset takaisin taulukon palvelu. Seuraava koodi muuttaa aiemmin luodun asiakkaan puhelinnumero. Sen sijaan, että puhelut, **Lisää**, koodi käyttää **Korvaa**. Tämä aiheuttaa kohde korvataan täysin palvelimessa, ellei kohteen palvelimessa on muuttunut, vaikka se on haettu, jolloin epäonnistuu.  Tämä virhe on estää sovelluksesi kirjoittamasta vahingossa toinen osa sovelluksen noutaminen – päivitys muutoksia.  Tämän virheen asianmukaisesta käsittelystä on hakea kohteen uudelleen, tee tarvittavat muutokset (jos se on edelleen voimassa) ja tee toinen **Korvaa** -toiminto.  Seuraavassa osassa kerrotaan, kuinka voit ohittaa tämän.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-0105";

       // Create the Replace TableOperation.
       TableOperation updateOperation = TableOperation.Replace(updateEntity);

       // Execute the operation.
       table.Execute(updateOperation);

       Console.WriteLine("Entity updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="insert-or-replace-an-entity"></a>Lisää tai-korvaa kohteen

**Korvaa** toimintojen epäonnistuu, jos kohde on muutettu, koska se on haettu palvelimesta.  Kohde on lisäksi noutaa palvelimen ensin onnistuisi **Korvaa** -toimintoa.
Joskus mutta et tiedä Jos kohde on palvelimessa, ja se tallennetaan nykyisen arvot eivät ole merkitystä. Päivityksesi on korvaa ne kaikki.  Tämän vuoksi käyttäisi **InsertOrReplace** -toimintoa.  Tämä toiminto Lisää kohde, jos se ei ole tai korvaa ne, jos se ei riippumatta siitä, kun viimeinen päivitys on tehty.  Koodin seuraavan esimerkin Ari Suominen asiakkaan yksikkö haetaan edelleen, mutta se tallennetaan sitten takaisin palvelimeen **InsertOrReplace**kautta.  Kohteen hakemista ja Päivitä-toimintojen tehdyt muutokset korvataan.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-1234";

       // Create the InsertOrReplace TableOperation.
       TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

       // Execute the operation.
       table.Execute(insertOrReplaceOperation);

       Console.WriteLine("Entity was updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="query-a-subset-of-entity-properties"></a>Kyselyn alijoukkoa kohteen ominaisuudet

Taulukon kyselyn noutaa todella ominaisuudet kohteen sen sijaan, että kaikki kohteen ominaisuudet. Tällä menetelmällä, jota kutsutaan ennuste, vähentää kaistanleveyden ja parantaa kyselyn suorituskykyä, erityisesti niiden suurimpia kohteita. Seuraava koodi kysely palauttaa vain kohteiden sähköpostiosoitteet taulukossa. Tämä on valmis käyttämällä **DynamicTableEntity** ja **EntityResolver**kyselyä. Saat lisätietoja ennuste- [esittely Upsert ja kyselyn ennuste blogi kirjaa][]tietoja. Huomaa, että ennuste ei tue paikallinen tallennus-emulointiohjelma, joten koodi toimii vain, kun käytät tilin taulukko-palvelusta.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## <a name="delete-an-entity"></a>Poista kohde

Voit helposti poistaa kohteen, kun on haettu, käyttämällä samoissa näkyvän kohteen päivittämistä varten.  Seuraava koodi noutaa ja poistaa asiakas.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       table.Execute(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Could not retrieve the entity.");

## <a name="delete-a-table"></a>Taulukon poistaminen

Lopuksi Seuraava koodiesimerkki poistaa taulukon tallennustilan tililtä. Taulukko, joka on poistettu ei ole käytettävissä luodaan poiston seuraavan ajanjakson uudelleen.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Delete the table it if exists.
    table.DeleteIfExists();

## <a name="retrieve-entities-in-pages-asynchronously"></a>Hae kohteita sivujen asynkronisesti

Jos luet on paljon kohteita ja haluat prosessin/näyttö kohteiden hakemisen odotetaan ne kaikki palauttaa, voit hakea kohteita Segmentoitu kyselyllä sijaan. Tässä esimerkissä näytetään, miten palauttamaan tulokset-sivujen avulla asynkroninen odotettava kuvion niin, että suorittamisen ei ole estetty Odotellessasi voit tulosten on suuri joukko. Saat lisätietoja asynkroninen Await kuvion käyttäminen .NET [asynkroninen ohjelmoinnin asynkroninen ja Await (C# ja Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;

    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;

        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut taulukkotallennus perusteet, näistä linkeistä saat lisätietoja tallennustilan monimutkaisia tehtäviä:

- On useita taulukon tallennustilan esimerkkejä käyttöönoton [.NET Azure-taulukoiden tallennustilaan](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)
- Näytä kaikki tiedot taulukkoon palvelun ohjeessa käytettävissä ohjelmointirajapinnan tietoja:
    - [Tallennustilan asiakkaan kirjaston .NET-käyttöä varten](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST API-viittaus](http://msdn.microsoft.com/library/azure/dd179355)
- Lue, miten voit yksinkertaistaa koodi, voit kirjoittaa Azuren tallennustilaan käyttämällä [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md) -käyttöä varten
- Näytä Lisää ominaisuus apuviivoja Saat lisätietoja lisäasetusten määrittäminen tiedot tallennetaan Azure.
    - [Pääset alkuun käyttämällä .NET Azure-Blob-säiliö](storage-dotnet-how-to-use-blobs.md) rakenteeton tietojen tallentamiseen.
    - [Yhteyden muodostaminen SQL-tietokanta käyttämällä .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) relaatiotietoja tallentamiseen.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

  [Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

  [Esittely Upsert ja kyselyn ennuste blogimerkintä]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
  [.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
  [How to: Programmatically access Table storage]: #tablestorage
