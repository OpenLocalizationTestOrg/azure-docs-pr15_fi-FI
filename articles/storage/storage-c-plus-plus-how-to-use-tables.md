<properties
    pageTitle="Opi käyttämään taulukkotallennus (C++) | Microsoft Azure"
    description="Voit tallentaa jäsenneltyjen tietojen avulla Azure-taulukkotallennus, NoSQL tietosäilö pilvipalveluun."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-table-storage-from-c"></a>Opi käyttämään C++-taulukkotallennus

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Yleiskatsaus  
Tässä oppaassa kerrotaan, kuinka voit suorittaa yleisiä tilanteita, joissa Azure-taulukosta tallennuspalvelu käyttämällä. Mallit kirjoitetaan c++ ja [Azure tallennustilan asiakkaan kirjasto C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md)avulla. Kattaa skenaariot ovat **luominen ja poistaminen taulukoksi** ja **taulukon kohteiden käsitteleminen**.

>[AZURE.NOTE] Tämän oppaan jotakin Azure-tallennustilan asiakkaan kirjaston C++ versio 1.0.0 ja yläpuolella. Suositellut versio on tallennustilan asiakkaan kirjaston 2.2.0, joka ei ole saatavilla [NuGet](http://www.nuget.org/packages/wastorage) tai [GitHub](https://github.com/Azure/azure-storage-cpp/)kautta.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="create-a-c-application"></a>C++-sovelluksen luominen  
Tämän oppaan voit käyttää tallennustilan ominaisuudet, joita voidaan suorittaa C++-sovelluksesta. Tällöin sinun on Azure-tallennustilan asiakkaan kirjaston for C++ ja Azure-tallennustilan tilin luominen Azure-tilaukseesi.  

Voit asentaa Azure-tallennustilan asiakkaan kirjaston C++-seuraavista tavoista:

-   **Linux:** Noudata annettuja [Azure tallennustilan asiakkaan kirjasto c++: n Lueminut](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) -sivulla.  
-   **Windows:** Valitse Visual Studion **Työkalut > NuGet pakettien hallinta > paketti hallinta-konsolin**. Kirjoita seuraava komento [NuGet paketin hallinta-konsolin](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ja paina Enter-näppäintä.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-table-storage"></a>Määritä sovellus käyttää taulukkotallennus  
Lisää seuraavat ovat lauseet haluamaasi Azure tallennustilan ohjelmointirajapinnan avulla voit käyttää taulukoita C++ tiedoston alkuun:  

    #include "was/storage_account.h"
    #include "was/table.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Azure-tallennustilan-yhteysmerkkijonon määrittäminen  
Azure-tallennustilan-asiakas käyttää tallennustilan yhteysmerkkijonon päätepisteet ja data management Services-palvelun käyttäminen tunnistetietojen tallentamiseen. Kun käynnissä asiakassovellus, sinun on määritettävä tallennustilan yhteysmerkkijonon seuraavassa muodossa. Käyttää *AccountName* ja *AccountKey* arvojen [Azure-portaalissa](https://portal.azure.com) näkyvä tallennustilan tilin nimi tallennustilan-tilisi ja tallennustilaa pikanäppäin. Lisätietoja tallennustilan asiakkaat ja pikanäppäinten on artikkelissa [tietoja Azure-tallennustilan tilit](storage-create-storage-account.md). Tässä esimerkissä näytetään, miten kielessä pitoon yhteysmerkkijonon staattinen kentän:  

    // Define the connection string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Voit esikatsella sovelluksen Windows-pohjaista paikallisesta tietokoneesta, voit käyttää Azure [tallennustilan emulaattorin](storage-use-emulator.md) , joka on asennettu [Azure SDK-paketissa](https://azure.microsoft.com/downloads/). Tallennustilan emulaattori on käytettävissä paikallista kehittämistä käyttämääsi laitteeseen Azure-Blob, jonossa ja taulukon palveluja tulokset vastaavat apuohjelma. Seuraavassa esimerkissä esitetään, miten kielessä pitoon yhteysmerkkijonon paikallinen tallennus emulaattorin staattisia kentän:  

    // Define the connection string with Azure storage emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Käynnistäminen Azure tallennustilan emulaattori, **Käynnistä** -painiketta tai paina Windows-näppäintä. Aloita kirjoittaminen **Azure-tallennustilan emulaattorin**ja valitse sitten **Microsoft Azure-tallennustilan emulaattorin** sovellusten luettelosta.  

Seuraavat esimerkit oletetaan, että olet käyttänyt jokin näistä tavoista saat tallennustilan yhteysmerkkijonon.  

## <a name="retrieve-your-connection-string"></a>Noutaminen yhteysmerkkijono  
Voit käyttää **cloud_storage_account** luokan edustavan tallennustilan tilin tiedot. Noutaa tallennustilan yhteysmerkkijonon tallennustilan tilin tiedot, voit käyttää jäsennä-menetelmää.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Seuraavaksi haluat viitata **cloud_table_client** -luokan, kuin se avulla saat viittaus objektien taulukot ja taulukon tallennuspalvelu tallennettuna kohteita. Seuraava koodi luo **cloud_table_client** objektin tallennuspaikka on haettu yläpuolella tilin avulla:  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

## <a name="create-a-table"></a>Taulukon luominen
**Cloud_table_client** objektin avulla saat viittaus objektien taulukot ja kohteita. Seuraava koodi luo **cloud_table_client** objektin ja käyttää sitä Luo uusi taulukko.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();  

## <a name="add-an-entity-to-a-table"></a>Kohteen lisääminen taulukkoon
Kohteen lisääminen taulukkoon, Luo uusi **table_entity** -objekti ja välittää sen **table_operation::insert_entity**. Seuraava koodi käyttää asiakkaan Etunimi rivi-näppäintä ja sukunimen osio-näppäintä. Yhdessä yksikön osio ja rivin avaimen yksilöivät kohde taulukossa. Kohteiden samalla osion avaimella voidaan suorittaa kysely nopeammin kuin sisältävän eri osioon näppäimet, mutta monipuolisen osio-näppäimellä mahdollistaa suurempi rinnakkain skaalattavuus. Lisätietoja on artikkelissa [Microsoft Azure-tallennustilan suorituskyky ja skaalattavuus tarkistusluettelon](storage-performance-checklist.md).

Seuraava koodi luo uuden esiintymän **table_entity** asiakkaan tietoja tallennetaan. Koodin Seuraava soittaa **table_operation::insert_entity** **table_operation** objektin kohteen lisääminen taulukon luominen, ja liittää sen taulukon uuden kohteen. Lopuksi koodi kutsuu **cloud_table** objektin execute-menetelmää. Ja uusi **table_operation** lähettää pyynnön asiakkaan uuden kohteen lisääminen "käyttäjät" taulukko taulukko-palveluun.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();

    // Create a new customer entity.
    azure::storage::table_entity customer1(U("Harp"), U("Walter"));

    azure::storage::table_entity::properties_type& properties = customer1.properties();
    properties.reserve(2);
    properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

    properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

    // Create the table operation that inserts the customer entity.
    azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

    // Execute the insert operation.
    azure::storage::table_result insert_result = table.execute(insert_operation);

## <a name="insert-a-batch-of-entities"></a>Lisää kohteita erän
Voit lisätä taulukon palveluun kohteiden erän kirjoitus-toiminnossa. Seuraava koodi luo **table_batch_operation** objektin ja lisää sitten kolme lisää siihen toimintoja. Lisää kunkin toiminnon lisätään luomalla uuden kohteen objektin, arvonsa määrittäminen ja sitten kutsumista Lisää **table_batch_operation** objektin kohteen liitettävä Lisää uusi toiminto. **Cloud_table.execute** kutsutaan suorittamaan toimintoa.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define a batch operation.
    azure::storage::table_batch_operation batch_operation;

    // Create a customer entity and add it to the table.
    azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

    azure::storage::table_entity::properties_type& properties1 = customer1.properties();
    properties1.reserve(2);
    properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
    properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

    // Create another customer entity and add it to the table.
    azure::storage::table_entity customer2(U("Smith"), U("Ben"));

    azure::storage::table_entity::properties_type& properties2 = customer2.properties();
    properties2.reserve(2);
    properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
    properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

    // Create a third customer entity to add to the table.
    azure::storage::table_entity customer3(U("Smith"), U("Denise"));

    azure::storage::table_entity::properties_type& properties3 = customer3.properties();
    properties3.reserve(2);
    properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
    properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

    // Add customer entities to the batch insert operation.
    batch_operation.insert_or_replace_entity(customer1);
    batch_operation.insert_or_replace_entity(customer2);
    batch_operation.insert_or_replace_entity(customer3);

    // Execute the batch operation.
    std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);

Huomautus seikat erän toiminnot:  

-   Voit suorittaa enintään 100 Lisää Poista yhdistäminen, korvaa, Lisää tai yhdistämisen ja Lisää tai korvaa yhden erän minkä tahansa yhdessä.  
-   Erä-toiminnolla voi olla Nouda-toiminto, jos se on vain toiminnon osalta.  
-   Kaikki kohteet yhden erän-toiminnossa on oltava sama osio-näppäintä.  
-   Erän-toiminto on rajoitettu 4 Megatavun tiedot.  

## <a name="retrieve-all-entities-in-a-partition"></a>Hae kaikki kohteet-osio
Kyselyn taulukon kaikkien kohteiden-osion, käytä **table_query** objektia. Seuraavassa esimerkissä koodi määrittää suodattimen kohteiden "Keila" missä osio-näppäintä. Tässä esimerkissä tulostaa kyselytulokset konsoliin kunkin kohteen kenttiä.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Print the fields for each customer.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

Tässä esimerkissä kysely näyttää kaikki kohteet, jotka vastaavat suodatusehtoja. Jos sinulla on suurten taulukoiden ja täytyy ladata taulukon kohteiden usein, on suositeltavaa, että voi tallentaa tiedot tallennustilan Azure-BLOB sijaan.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Noutamista osioon kohteiden
Jos et halua kyselyn osion kaikkia kohteita, voit määrittää alueen yhdistämällä osion avaimen suodatin, jossa on rivin avaimen suodattimen. Seuraava koodi-esimerkissä käytetään kaksi suodatinta saat kaikki kohteet-osion "Keila", jossa on aiempi kuin "E" aakkosten kirjaimella alkavaan rivin tunnusta (Etunimi) ja tulostaa kyselytulokset.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table query.
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
        azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
        azure::storage::query_comparison_operator::equal, U("Smith")),
        azure::storage::query_logical_operator::op_and,
        azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Loop through the results, displaying information about the entity.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

## <a name="retrieve-a-single-entity"></a>Yhden kohteen hakeminen
Voit kirjoittaa kyselyn, joka noutaa yksi, tietyn kohteen. Seuraava koodi käyttää **table_operation::retrieve_entity** Määritä asiakas "Esko Keila". Tämä menetelmä palauttaa yhden kohteen kokoelman sijaan ja palautettu arvo on **table_result**. Kyselyn, joka määrittää osio-ja rivi on nopein tapa hakea yksittäisen kohteen taulukon-palvelusta.  

    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Output the entity.
    azure::storage::table_entity entity = retrieve_result.entity();
    const azure::storage::table_entity::properties_type& properties = entity.properties();

    std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;

## <a name="replace-an-entity"></a>Korvaa kohteen
Jos haluat korvata kohteen, noutaminen taulukosta-palvelusta, yksikön objektin muokkaaminen ja valitse Tallenna muutokset takaisin taulukon palvelu. Seuraava koodi muuttaa aiemmin luodun asiakkaan puhelinnumero ja sähköpostiosoite. Sen sijaan, että puhelut **table_operation::insert_entity**, koodi käyttää **table_operation::replace_entity**. Tämä aiheuttaa kohde korvataan täysin palvelimessa, ellei kohteen palvelimessa on muuttunut, vaikka se on haettu, jolloin epäonnistuu. Tämä virhe on estää sovelluksesi kirjoittamasta vahingossa toinen osa sovelluksen noutaminen – päivitys muutoksia. Tämän virheen asianmukaisesta käsittelystä on hakea kohteen uudelleen, tee tarvittavat muutokset (jos se on edelleen voimassa) ja tee toinen **table_operation::replace_entity** -toiminto. Seuraavassa osassa kerrotaan, kuinka voit ohittaa tämän.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Replace an entity.
    azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
    properties_to_replace.reserve(2);

    // Specify a new phone number.
    properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

    // Specify a new email address.
    properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

    // Create an operation to replace the entity.
    azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result replace_result = table.execute(replace_operation);

## <a name="insert-or-replace-an-entity"></a>Lisää tai-korvaa kohteen
**table_operation::replace_entity** toimintojen epäonnistuu, jos kohde on muutettu, koska se on haettu palvelimesta. Lisäksi on hakea kohteen ensimmäisen kerran, jotta **table_operation::replace_entity** onnistuisi palvelimesta. Joskus, mutta et tiedä, jos kohde on palvelimessa, ja se tallennetaan nykyisen arvot eivät ole merkitystä, Päivityksesi korvaa ne kaikki. Tämän vuoksi käyttäisi **table_operation::insert_or_replace_entity** -toimintoa. Tämä toiminto Lisää kohde, jos se ei ole tai korvaa ne, jos se ei riippumatta siitä, kun viimeinen päivitys on tehty. Koodin seuraavan esimerkin Jeff Smith asiakkaan yksikkö haetaan edelleen, mutta se tallennetaan sitten takaisin palvelimeen **table_operation::insert_or_replace_entity**kautta. Kohteen noutaminen – päivitystoiminto tehdyt muutokset korvataan.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Insert-or-replace an entity.
    azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

    properties_to_insert_or_replace.reserve(2);

    // Specify a phone number.
    properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

    // Specify an email address.
    properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

    // Create an operation to insert-or-replace the entity.
    azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);

## <a name="query-a-subset-of-entity-properties"></a>Kyselyn alijoukkoa kohteen ominaisuudet  
Taulukon kyselyn noutaa todella ominaisuudet kohteen. Seuraava koodi kysely palauttaa vain kohteiden sähköpostiosoitteet taulukon käyttämällä **table_query::set_select_columns** -menetelmää.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define the query, and select only the Email property.
    azure::storage::table_query query;
    std::vector<utility::string_t> columns;

    columns.push_back(U("Email"));
    query.set_select_columns(columns);

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Display the results.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

        const azure::storage::table_entity::properties_type& properties = it->properties();
        for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
        {
            std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
        }

        std::wcout << std::endl;
    }

>[AZURE.NOTE] Joitakin ominaisuuksia kohteesta kyselyt on tehokkaampaa kuin haetaan kaikki ominaisuudet toiminto.

## <a name="delete-an-entity"></a>Poista kohde
Voit helposti poistaa kohteen, kun se on noudettu. Kun kohde on noudettu, soita **table_operation::delete_entity** kohdetta, jos haluat poistaa. Valitse Soita **cloud_table.execute** -menetelmää. Seuraava koodi noutaa ja poistaa kohteen osion avaimen "Keila" ja "Esko" rivin tunnus.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);  

## <a name="delete-a-table"></a>Taulukon poistaminen
Lopuksi Seuraava koodiesimerkki poistaa taulukon tallennustilan tililtä. Taulukko, joka on poistettu ei ole käytettävissä luodaan poiston seuraavan ajanjakson uudelleen.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun olet tutustunut taulukkotallennus perusteet, näistä linkeistä saat lisätietoja Azure-tallennustilan:  

-   [Opi käyttämään C++-Blob-objektien tallennustilaan](storage-c-plus-plus-how-to-use-blobs.md)
-   [Opi käyttämään jonon tallennustilaa C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Azure-tallennustilan resurssien c++ luettelo](storage-c-plus-plus-enumeration.md)
-   [Tallennustilan asiakkaan kirjasto C++-Ohje](http://azure.github.io/azure-storage-cpp)
-   [Azure tallennustilan dokumentaatio](https://azure.microsoft.com/documentation/services/storage/)
