<properties
    pageTitle="Aloita taulukkotallennus ja Visual Studio yhdistetyt palvelut (ASP.NET) | Microsoft Azure"
    description="Pikaviestien käyttäminen Visual Studio ASP.NET-projektiksi Azure-taulukkotallennus, kun yhteyden Visual Studiossa tallennustilan tilin yhdistetyt palvelut"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-table-storage-and-visual-studio-connected-services-aspnet"></a>Aloita taulukkotallennus ja Visual Studio yhdistetyt palvelut (ASP.NET)

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Yleiskatsaus
Tässä artikkelissa kuvataan, miten käyttäminen Azure-taulukkotallennus Visual Studiossa, kun olet luonut tai Viitattu Azure-tallennustilan tilin ASP.NET-Projectin Visual Studio **Lisää yhdistetyt palvelut** -valintaikkunan. Tässä artikkelissa kerrotaan, miten voit suorittaa yleisiä tehtäviä Azure taulukot, mukaan lukien luominen ja poistaminen taulukon sekä taulukon kohteiden käsitteleminen. Mallit on kirjoitettu C\# koodi ja käytä [Microsoft Azure tallennustilan asiakkaan kirjasto .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx). Yleisiä tietoja käytöstä Azure taulukon tallennustila on artikkelissa [Azure-taulukkotallennus käyttämällä .NET käytön aloittaminen](storage-dotnet-how-to-use-tables.md).

Azure-taulukkotallennus avulla voit tallentaa rakenteellisen suurista tietomääristä. Palvelu on NoSQL datastore, joka hyväksyy todennetut kutsut ja Azure pilveen sivuihin. Azure-taulukoiden soveltuvat erinomaisesti rakenteellisia ja relaatio tietojen tallentaminen.


## <a name="access-tables-in-code"></a>Access-taulukot-koodi

1. Varmista, että nimitilan ilmoitukset C#-tiedoston yläosassa lauseiden **käyttäminen** .

         using Microsoft.Azure;
         using Microsoft.WindowsAzure.Storage;
         using Microsoft.WindowsAzure.Storage.Auth;
         using Microsoft.WindowsAzure.Storage.Table;

2. Pyydä **CloudStorageAccount** objekti, joka esittää tallennustilan tilin tiedot. Seuraava koodi avulla saat-tallennustilan yhteysmerkkijonon ja Azure palvelun tallennustilan tilin tiedot.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Huomautus** - käyttää kaikkia edellä koodin lukukoodin seuraavat esimerkit.

3. Pyydä **CloudTableClient** -objektin viittaamaan taulukko-objektien tallennustilaan tilisi.  

        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. **CloudTable** objektin tietyn taulukon ja kohteiden hakeminen

        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Taulukon luominen koodi

Luo Azure taulukko Lisää **CreateIfNotExistsAsync()** puhelun Edellinen-koodiin.

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Kohteen lisääminen taulukkoon

Kohteen lisääminen taulukkoon Luo luokka, joka määrittää oman kohteen ominaisuudet. Seuraava koodi määrittää luokka kohteen **CustomerEntity** , joka käyttää asiakkaan Etunimi Sukunimi ja rivi-näppäintä osio-näppäintä.

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

Taulukon toimia kohteiden on valmis käyttämällä **CloudTable** objekti, jonka loit aiemmin "Taulukot Access-koodiin." **TableOperation** -objekti edustaa toimintoa tehtävä. Seuraava koodi-esimerkissä luomisesta **CloudTable** -objekti ja **CustomerEntity** -objekti. Toiminnon valmistelemaan **TableOperation** luodaan asiakkaan kohteen lisääminen taulukkoon. -Toiminto on suoritettu soittamalla CloudTable.ExecuteAsync.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Lisää kohteita erän

Voit lisätä useita kohteita yhden kirjoitus taulukkoon. Seuraava koodiesimerkki Luo kaksi kohteen objektia ("Esko Keila" ja "Ari Suominen"), lisää ne **TableBatchOperation** -objektin lisääminen-menetelmällä ja käynnistää toiminnon soittamalla **CloudTable.ExecuteBatchAsync**.

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Saat kaikki kohteiden osio
Kyselyn taulukon kaikkien kohteiden-osion, käytä **TableQuery** objektia. Seuraavassa esimerkissä koodi määrittää suodattimen kohteiden "Keila" missä osio-näppäintä. Tässä esimerkissä tulostaa kyselytulokset konsoliin kunkin kohteen kenttiä.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity>
        resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
        }
        } while (token != null);

        return View();


## <a name="get-a-single-entity"></a>Hae yhden kohteen
Voit kirjoittaa kyselyn saat yhteen, tietyn kohteen. Seuraava koodi käyttää **TableOperation** objektin määrittää asiakkaan nimi "Ari Suominen". Tämä menetelmä palauttaa yhden kohteen sijaan **CustomerEntity** objekti on kokoelma ja **TableResult.Result** palautetun arvon. Kyselyn, joka määrittää osio-ja rivi on nopein tapa hakea yksittäisen kohteen taulukon-palvelusta.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);
    
    // Print the phone number of the result.
    if (retrievedResult.Result != null)
        Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Poista kohde
Voit poistaa kohteen, kun tiedät sen. Seuraava koodi näyttää nimeltä "Ari Suominen" asiakas-kohteen, ja jos se havaitsee, se poistaa sen.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
