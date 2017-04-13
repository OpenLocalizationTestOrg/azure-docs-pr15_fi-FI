<properties
    pageTitle="Opi käyttämään Java-taulukkotallennus | Microsoft Azure"
    description="Voit tallentaa jäsenneltyjen tietojen avulla Azure-taulukkotallennus, NoSQL tietosäilö pilvipalveluun."
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


# <a name="how-to-use-table-storage-from-java"></a>Opi käyttämään Java-taulukkotallennus

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä oppaassa kerrotaan, kuinka voit suorittaa yleisiä tilanteita, joissa Azure-taulukosta tallennustilan-palvelun avulla. Mallit on kirjoitettu Java ja käytä [Java Azure tallennustilan SDK-paketissa][]. Taulukoiden **luominen**, **luettelon**ja **poistaminen** sekä **lisääminen**, **Kyselyt**, **muokkaaminen**ja **poistaminen** kohteiden sisällyttäminen kattaa käyttötavoista taulukon. Lisätietoja taulukoista on kohdassa [vaiheisiin](#Next-Steps) .

Huomautus: SDK: ta ei ole käytettävissä sovelluskehittäjille, jotka käyttävät Azuren tallennustilaan Android-laitteisiin. Lisätietoja [Android Azure tallennustilan SDK-paketissa][].

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java-sovelluksen luominen

Tämän oppaan voit käyttää tallennustilan ominaisuuksia, jotka voidaan suorittaa Java-sovelluksen paikallisesti tai web-roolin tai työntekijän rooli Azure koodin.

Tällöin sinun on asennettava Java Development Kit (JDK) ja Azure-tallennustilan tilin luominen Azure-tilaukseesi. Kun olet tehnyt, sinun on varmistaa, että kehittäminen järjestelmän täyttää vähimmäisvaatimukset ja riippuvuudet, jotka on lueteltu GitHub [Azure-tallennustilan SDK Java][] -säilössä. Jos järjestelmä täyttää näitä vaatimuksia, voit noudattaa lataaminen ja asentaminen järjestelmään, säilöstä Java Azure-tallennustilan kirjastoja koskevia ohjeita. Kun olet suorittanut sellaiset tehtävät, voi luoda Java-sovellusta, jolla tämän artikkelin esimerkeissä käytetään.

## <a name="configure-your-application-to-access-table-storage"></a>Määritä sovellus käyttää taulukkotallennus

Lisää seuraavat Tuontilauseet Java-tiedosto, johon voit käyttää Microsoft Azuren tallennustilaan ohjelmointirajapinnan avulla taulukoiden alkuun:

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

## <a name="setup-an-azure-storage-connection-string"></a>Azure-tallennustilan-yhteysmerkkijonon asetukset

Azure-tallennustilan-asiakas käyttää tallennustilan yhteysmerkkijonon päätepisteet ja data management Services-palvelun käyttäminen tunnistetietojen tallentamiseen. Kun asiakassovellus, sinun on määritettävä tallennustilan yhteysmerkkijonon muodossa, käyttämällä *AccountName* ja *AccountKey* arvojen [Azure-portaalissa](https://portal.azure.com) näkyvä tallennustilan tilin nimi ja tallennustilaa tilisi ensisijainen pikanäppäin. Tässä esimerkissä näytetään, miten kielessä pitoon yhteysmerkkijonon staattisia kentän:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

Microsoft Azure-roolin sisällä suoritettavien sovelluksen merkkijono voidaan tallentaa service-kokoonpanotiedosto *ServiceConfiguration.cscfg*, ja niitä voi käyttää puhelun **RoleEnvironment.getConfigurationSettings** tapaan. Tässä on esimerkki yhteysmerkkijonon käytön **asetus** osan nimeltä *StorageConnectionString* palvelun määritystiedostossa:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Seuraavat esimerkit oletetaan, että olet käyttänyt jokin näistä tavoista saat tallennustilan yhteysmerkkijonon.

## <a name="how-to-create-a-table"></a>Toimintaohje: taulukon luominen

**CloudTableClient** objektin avulla saat viittaus objektien taulukot ja kohteita. Seuraava koodi luo **CloudTableClient** objektin ja luo uusi **CloudTable** objekti, joka edustaa "käyttäjät"-taulukon. (Huomautus: muilla tavoilla voit luoda **CloudStorageAccount** objekteja, Lisätietoja on artikkelissa **CloudStorageAccount** [Azure tallennustilan Client SDK-viittaus].)

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = tableClient.getTableReference(tableName);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-tables"></a>Toimintaohje: taulukoiden luettelo

Saat taulukkoluettelon-kutsu **CloudTableClient.listTables()** -menetelmä noutamiseen taulukoiden nimien iterable luettelo.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-an-entity-to-a-table"></a>Toimintaohje: kohteen lisääminen taulukkoon

Kohteiden liittäminen Java-objektien käyttäminen soveltamisesta **TableEntity**mukautetun luokan. Asiasta **TableServiceEntity** luokan toteuttaa **TableEntity** ja käyttää heijastusta yhdistää nopeamman ja navigoinnin menetelmiä, ominaisuuksia nimetyt ominaisuuksia. Kohteen lisääminen taulukkoon, luo ensin luokka, joka määrittää oman kohteen ominaisuudet. Seuraava koodi määrittää kohteen-luokka, joka käyttää asiakkaan Etunimi rivi-näppäintä painettuna ja sukunimen osio-näppäintä. Yhdessä yksikön osio ja rivin avaimen yksilöivät kohde taulukossa. Kohteiden samalla osion avaimella voidaan suorittaa kysely nopeammin kuin sisältävän eri osiointiavaimet.

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;

        public String getEmail() {
            return this.email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        public String getPhoneNumber() {
            return this.phoneNumber;
        }

        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

Liittyvät kohteet taulukossa toiminnot vaativat **TableOperation** objekti. Tämä objekti määrittää toiminto voidaan suorittaa kohde, johon voidaan suorittaa **CloudTable** objektia. Seuraava koodi luo uuden esiintymän **CustomerEntity** luokan asiakkaan tietoja tallennetaan. Koodin Seuraava soittaa **TableOperation.insertOrReplace** **TableOperation** objektin kohteen lisääminen taulukon luominen, ja liittää sen uuden **CustomerEntity** . Lopuksi koodi kutsuu **execute** -menetelmää **CloudTable** objektin "käyttäjät"-taulukko-ja uuden **TableOperation**, joka lähettää pyynnön tallennuspalvelu asiakkaan uuden kohteen lisääminen "käyttäjät" taulukko tai korvata kohteen, jos se on jo olemassa.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-a-batch-of-entities"></a>Toimintaohje: Lisää kohteita erän

Voit lisätä taulukon palvelun kohteiden erän kirjoitus-toiminnossa. Seuraava koodi luo **TableBatchOperation** objektin ja valitse Lisää kolme lisää siihen toimintoja. Lisää kunkin toiminnon lisätään luomalla uuden kohteen objektin, arvonsa määrittäminen ja sitten kutsumista **Lisää** **TableBatchOperation** objektin kohteen liitettävä Lisää uusi toiminto. Koodin kutsuu sitten **Suorita** **CloudTable** objektin "käyttäjät"-taulukko-ja **TableBatchOperation** -objekti, joka lähettää taulukon toimintojen erän yksittäisen pyynnön tallennustilan-palveluun.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Huomautus seikat erän toiminnot:

- Voit suorittaa enintään 100 Lisää, poista, Yhdistä, korvaa, Lisää tai yhdistää, ja Lisää tai korvaa-toimintoja tahansa yksittäisen osalta.
- Erä-toiminnolla voi olla Nouda-toiminto, jos se on vain toiminnon osalta.
- Kaikki kohteet yhden erän-toiminnossa on oltava sama osio-näppäintä.
- Erän-toiminto on rajoitettu 4 Megatavua tiedot.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Toimintaohje: noutaa kaikki kohteet-osio

Kysely-osion kohteiden taulukon, voit käyttää **TableQuery**. Soita **TableQuery.from** tietyn taulukon, joka palauttaa määritetyn Tulostyyppi kyselyn luomiseen. Seuraava koodi määrittää suodattimen kohteiden "Keila" missä osio-näppäintä. **TableQuery.generateFilterCondition** on helper menetelmä suodattimien kyselyjen luomiseen. Soittaminen **missä** **TableQuery.from** tapa käyttää suodatinta kyselyn palauttamien viittaus. Kun kysely suoritetaan puhelun **CloudTable** objektin **suorittaminen** , se palauttaa **kyselymerkkijonon** määritetty **CustomerEntity** Tulostyyppi kanssa. Voit käyttää **kyselymerkkijonon** määrää saat silmukassa tarjoaman tulokset. Tämä koodi tulostaa kyselytulokset konsoliin kunkin kohteen kenttiä.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Toimintaohje: noutamista osioon kohteiden

Jos et halua kyselyn osion kaikkia kohteita, voit määrittää alueen käyttämällä vertailuoperaattorit suodattimen. Seuraava koodi yhdistää kaksi suodatinta aakkosten "Keila", josta rivin tunnusta (Etunimi) alkaa kirjaimella "E" ylöspäin saat kaikki kohteet-osioon. Valitse tulostaa kyselytulokset. Jos käytössäsi on lisätty taulukkoon erän kohteiden Lisää tämän oppaan osio, vain kaksi kohteet palautetaan tällä hetkellä (Ben ja Tiina Smith); Esko Keila ei sisälly.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY,
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter,
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-single-entity"></a>Toimintaohje: noutaa yhden kohteen

Voit kirjoittaa kyselyn, joka noutaa yksi, tietyn kohteen. Seuraava koodi kutsuu **TableOperation.retrieve** osio-näppäintä ja rivin avaimen parametrien määrittäminen asiakkaan "Esko Keila", **TableQuery** luomisesta ja suodattimien avulla voit tehdä saman myös sijaan. Kun, Nouda toiminto palauttaa yhden kohteen kokoelman sijaan. **GetResultAsType** menetelmä muuttaa sarakemuotoa tehtävän kohde **CustomerEntity** objektin tyypin tuloksen. Jos tämä ei ole yhteensopiva määritetty kyselyn tyyppi, järjestelmässä poikkeus. Null-arvon palautetaan, jos ei ole kohteella tarkka osio ja vastaa rivi-näppäintä. Kyselyn, joka määrittää osio-ja rivi on nopein tapa hakea yksittäisen kohteen taulukon-palvelusta.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-modify-an-entity"></a>Toimintaohje: kohteen muokkaaminen

Jos haluat muokata kohteen, noutaa taulukon-palvelusta, muutosten tekeminen yksikön objektin ja tallentaa muutokset taulukon palvelun korvaa tai yhdistäminen-toiminnossa. Seuraava koodi muuttaa aiemmin luodun asiakkaan puhelinnumero. Sen sijaan, että puhelut **TableOperation.insert** , kuten on ollut Lisää, koodi kutsuu **TableOperation.replace**. **CloudTable.execute** menetelmä soittaa taulukko-palvelun ja kohteen korvataan, ellei toisesta sovelluksesta sitä muokkasi kellonajan tämän sovelluksen noutaa sen jälkeen. Tällöin ilmenee poikkeus ja kohde on voidaan hakea, muokata ja tallentaa uudelleen. Tämän optimistisen uudelleen kuvion yhteistä hajautettu tallennustilan-järjestelmä.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-query-a-subset-of-entity-properties"></a>Toimintaohje: kyselyn alijoukkoa kohteen ominaisuudet

Taulukon kyselyn noutaa todella ominaisuudet kohteen. Tällä menetelmällä, jota kutsutaan ennuste, vähentää kaistanleveyden ja parantaa kyselyn suorituskykyä, erityisesti niiden suurimpia kohteita. Seuraava koodi kysely palauttaa vain kohteiden sähköpostiosoitteet taulukon käyttämällä **Valitse** menetelmä. Tulokset ovat suunnitellut kokoelmaan **merkkijonon** **EntityResolver**, jota ei palautettu palvelimesta kohteiden tyyppi-muuntaminen avulla. Voit lukea lisää ennuste [Azure-taulukoiden: esittely Upsert ja kyselyn ennuste][]. Huomaa, että ennuste ei tue paikallinen tallennus-emulointiohjelma, joten tämä koodi suoritetaan vain, kun käyttämällä tiliä, taulukko-palvelusta.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery =
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString :
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-or-replace-an-entity"></a>Toimintaohje: Lisää tai korvaa kohteen

Usein haluat kohteen lisääminen taulukkoon ilman tietää, jos se on jo taulukossa. Lisää tai korvaa-toiminnon avulla voit tehdä yksittäisen pyynnön, jossa Lisää kohde, jos se ei ole tai korvaa aiemmin luotuun, jos se ei. Pohjalta edellisen esimerkkejä seuraava koodi Lisää tai korvaa kohteen "Walter Harp". Kun olet luonut uuden kohteen, koodi kutsuu **TableOperation.insertOrReplace** -menetelmää. Tämä koodi kutsuu **suorittaa** **CloudTable** -objekti, jossa on taulukon ja lisää sitten tai korvaa taulukon toiminto parametreille. Jos haluat päivittää vain osan kohteen, **TableOperation.insertOrMerge** -menetelmää voidaan käyttää sen sijaan. Huomaa, että Lisää ja tai-korvaaminen ei tueta paikallinen tallennus-emulointiohjelma niin koodi suoritetaan vain, kun käyttämällä tiliä, taulukko-palvelusta. Voit lukea lisätietoja Lisää tai korvaa ja Lisää tai yhdistämisen tämän [Azure-taulukoiden: esittely Upsert ja kyselyn ennuste][].

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-an-entity"></a>Toimintaohje: Poista kohde

Voit helposti poistaa kohteen, kun se on noudettu. Kun kohde on noudettu, puhelu **TableOperation.delete** ja poistettava kohde. Soita sitten **CloudTable** objektin **suorittaminen** . Seuraava koodi noutaa ja poistaa asiakas.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-table"></a>Toimintaohje: taulukon poistaminen

Lopuksi seuraava koodi poistaa taulukon tallennustilan tililtä. Taulukko, joka on poistettu ei ole käytettävissä ehkä luoda uudelleen tietyn ajan jälkeen poistaminen, yleensä alle 40 sekuntia.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut taulukkotallennus perusteet, lue, miten voit tehdä monimutkaisia tallennustilan tehtävät seuraavista linkeistä mukaisesti.

- [Azure-tallennustilan SDK Java][]
- [Azure-tallennustilan asiakkaan SDK-ohje][]
- [Azure-tallennustilan REST-Ohjelmointirajapinnalla][]
- [Azure-tallennustilan tiimin blogia][]

Lisätietoja on Katso myös [Java Developer Center](/develop/java/).


[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure-tallennustilan SDK Java]: https://github.com/azure/azure-storage-java
[Azure-tallennustilan SDK androidille]: https://github.com/azure/azure-storage-android
[Azure-tallennustilan asiakkaan SDK-ohje]: http://dl.windowsazure.com/storage/javadoc/
[Azure-tallennustilan REST-Ohjelmointirajapinnalla]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure-tallennustilan tiimin blogia]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure taulukot: Upsert ja kyselyn ennuste esittely]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
