<properties
    pageTitle="Taulukon tallennustilaa PHP käyttämisestä | Microsoft Azure"
    description="Opettele PHP-taulukon-palvelun avulla voit luoda ja poistaa taulukon lisääminen, poistaminen ja kyselyn taulukon."
    services="storage"
    documentationCenter="php"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-php"></a>Opi käyttämään PHP-taulukkotallennus

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Yleiskatsaus

Tämän oppaan avulla voit suorittaa yleisiä tilanteita, joissa Azure-taulukosta-palvelun avulla. Mallit on kirjoitettu PHP ja käyttää [Azure SDK PHP][download]. Tilanteita, joissa kattaa Sisällytä **luominen ja taulukon, poistaminen ja lisääminen, poistaminen, ja kyselyt-taulukon kohteiden**. Lisätietoja Azure-taulukosta-palvelu on kohdassa [vaiheisiin](#next-steps) .

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP-sovelluksen luominen

Vain vaatimus luodaan PHP-sovellusta, joka käyttää Azure-taulukosta-palvelu on oman koodiin PHP luokkien Azure SDK viittaava. Mikä tahansa Kehitystyökalut voit luoda sovellus, esimerkiksi Muistiossa.

Tämän oppaan avulla voit käyttää taulukon palvelun ominaisuuksia, jotka voidaan kutsua PHP-sovelluksen paikallisesti tai koodin suorittamisen Azure web rooli, työntekijän rooli tai sivuston.

## <a name="get-the-azure-client-libraries"></a>Hae Azure asiakas-kirjastot

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Määrittää taulukossa palvelun käyttämiseen

Jos haluat käyttää Azure-taulukosta service API, on:

1. Viittaus autoloader tiedoston [require_once] [ require_once] -lauseessa, ja
2. Viittaus käytöstä kaikki luokat.

Seuraavassa esimerkissä esitetään autoloader-tiedoston ja viittaa **ServicesBuilder** -luokkaan.

> [AZURE.NOTE] Tässä esimerkissä (ja muita Esimerkkejä tämän artikkelin) oletetaan, olet asentanut Azure kautta tunnus PHP asiakas-kirjastoissa. Jos olet asentanut kirjastot manuaalisesti, sinun on viittaus <code>WindowsAzure.php</code> autoloader tiedosto.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


Esimerkeissä `require_once` lauseessa on aina näkyvissä, mutta vain luokat, jotka ovat esimerkiksi suorittamiseen tarvittavat viitataan.

## <a name="set-up-an-azure-storage-connection"></a>Azure-tallennustilan-yhteyden määrittäminen

Azure-taulukosta-palvelun asiakasohjelmaa vahvistamiseen on kelvollinen yhteysmerkkijono. Taulukon palvelun yhteysmerkkijonon muoto on:

Live palvelun käyttöön:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Käyttää emulaattorin tallennustilan:

    UseDevelopmentStorage=true


Voit luoda minkä tahansa Azure service-asiakas, haluat käyttää **ServicesBuilder** -luokka. Voit:

* välittää yhteysmerkkijonon suoraan tai
* **CloudConfigurationManager (magnesiitin)** avulla voit tarkistaa useista ulkoisista lähteistä yhteysmerkkijonon varten:
    * Oletusarvon mukaan se sisältää ulkoisen tietolähteen - ympäristön muuttujat tuki
    * Voit lisätä uusia lähteitä pidentämällä **ConnectionStringSource** -luokka

Tässä kuvatut esimerkkejä yhteysmerkkijonon siirtyvät suoraan.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);


## <a name="create-a-table"></a>Taulukon luominen

**TableRestProxy** objektin avulla voit luoda taulukon **createTable** -menetelmällä. Kun luot taulukon, voit määrittää taulukon palvelun aikakatkaisu. (Katso lisätietoja taulukon palvelun aikakatkaisu [Asetus aikakatkaisu taulukon palvelun toimille][table-service-timeouts].)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Create table.
        $tableRestProxy->createTable("mytable");
    }
    catch(ServiceException $e){
        $code = $e->getCode();
        $error_message = $e->getMessage();
        // Handle exception based on error codes and messages.
        // Error codes and messages can be found here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
    }

Lisätietoja taulukon nimien rajoitukset artikkelissa [taulukon Service-tietomallin perusteet][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Kohteen lisääminen taulukkoon

Kohteen lisääminen taulukkoon, Luo uusi **kohde** -objekti ja välittää **TableRestProxy -> insertEntity**. Huomaa, että kun olet luonut kohteen, sinun on määritettävä `PartitionKey` ja `RowKey`. Nämä ovat kohteen yksilölliset tunnukset ja ovat arvoja, jotka voidaan suorittaa kysely paljon nopeammin kuin kohteen muita ominaisuuksia. Järjestelmä käyttää `PartitionKey` jakaa automaattisesti taulukon kohteiden monta tallennustilan solmujen kohdalla. Kohteet, joissa on sama `PartitionKey` saman solmun tallennetaan. (Suorittaa laskutoimituksia useiden kohteiden tallennettu saman solmun parempi kuin kohteet, jotka on tallennettu eri solmujen yli.) `RowKey` On yksilöllinen tunnus kuuluvaa osioon.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $entity = new Entity();
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate",
                         EdmType::DATETIME,
                         new DateTime("2012-11-05T08:15:00-08:00"));
    $entity->addProperty("Location", EdmType::STRING, "Home");

    try{
        $tableRestProxy->insertEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
    }

Lisätietoja taulukon ominaisuudet ja tietotyypeistä on [taulukon Service-tietomallin perusteet][table-data-model].

**TableRestProxy** -luokka on kaksi vaihtoehtoisia menetelmiä kohteiden lisääminen: **insertOrMergeEntity** ja **insertOrReplaceEntity**. Näistä tavoista, Luo uusi **kohde** ja välittää parametrina joko tapaan. Kummassakin menetelmässä Lisää kohde, jos sitä ei ole. Jos kohde on jo olemassa, **insertOrMergeEntity** ominaisuusarvoihin päivittyy, jos jo ominaisuudet ja lisää uusia ominaisuuksia, jos ne eivät ole, kun taas **insertOrReplaceEntity** kokonaan korvaa olemassa olevan kohteen. Seuraavassa esimerkissä esitetään, miten voit käyttää **insertOrMergeEntity**. Jos kohteella `PartitionKey` "tasksSeattle" ja `RowKey` "1" ei vielä ole, se lisätään. Jos se on aiemmin lisätty (katso yllä olevassa esimerkissä), `DueDate` ominaisuuden päivitetään, ja `Status` ominaisuuden lisätään. `Description` Ja `Location` ominaisuuksia päivitetään, mutta arvoilla, jotka tehokkaasti jättäminen ennalleen. Jos jälkimmäisessä kaksi ominaisuuksista on ei ole lisätty esimerkin mukaisesti, mutta mukaisessa kohde-kohteen, niiden olemassa olevat arvot säilyvät muuttumattomina.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    //Create new entity.
    $entity = new Entity();

    // PartitionKey and RowKey are required.
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");

    // If entity exists, existing properties are updated with new values and
    // new properties are added. Missing properties are unchanged.
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
    $entity->addProperty("Location", EdmType::STRING, "Home");
    $entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

    try {
        // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
        // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
        $tableRestProxy->insertOrMergeEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="retrieve-a-single-entity"></a>Yhden kohteen hakeminen

**TableRestProxy -> getEntity** -menetelmällä voit hakea yksittäisen kohteen kyselyistä sen `PartitionKey` ja `RowKey`. Osion avaimen alapuolella olevasta esimerkistä `tasksSeattle` ja rivin avaimen `1` välitetään **getEntity** -menetelmää.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entity = $result->getEntity();

    echo $entity->getPartitionKey().":".$entity->getRowKey();

## <a name="retrieve-all-entities-in-a-partition"></a>Hae kaikki kohteet-osio

Kohteen kyselyt on rakennettava suodattimien avulla (Lisätietoja on artikkelissa [Kyselyt taulukot ja kohteiden][filters]). Noutaa kaikki kohteet-osion, käytä suodatin "PartitionKey eq *osion_nimi*". Seuraavassa esimerkissä esitetään, voit hakea kaikkia kohteita `tasksSeattle` osio siirtämällä suodattimen **queryEntities** -menetelmää.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "PartitionKey eq 'tasksSeattle'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Alijoukon-osion kohteiden hakeminen

Edellisessä esimerkissä käytetty samoissa voidaan noutaa kaikki alijoukko yksiköt-osio. Voit hakea kohteita alijoukko määräytyvät käytät suodatinta (Lisätietoja on artikkelissa [Kyselyt taulukot ja kohteiden][filters]). Seuraavassa esimerkissä esitetään, miten suodattimen avulla voit hakea kaikki kohteet, että `Location` ja `DueDate` alle määritetyn päivämäärän.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entity-properties"></a>Kohteen ominaisuuksien alijoukon hakeminen

Kyselyn voi hakea alijoukkoa kohteen ominaisuudet. Tätä tapaa kutsutaan *Ennuste*, vähentää kaistanleveyden ja parantaa kyselyn suorituskykyä, erityisesti niiden suurimpia kohteita. Määritä ominaisuus noudetaan välittää ominaisuuden nimi **kyselyn -> addSelectField** -menetelmää. Tätä menetelmää voit kutsua useita kertoja voit lisätä täsmättäviä ominaisuuksia. Jos suoritat **TableRestProxy -> queryEntities**, palautettujen kohteiden on vain valitut ominaisuudet. (Jos haluat palauttaa taulukon kohteiden alijoukkoa, Käytä suodatinta kyselyissä edellä kuvatulla.)

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $options = new QueryEntitiesOptions();
    $options->addSelectField("Description");

    try {
        $result = $tableRestProxy->queryEntities("mytable", $options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    // All entities in the table are returned, regardless of whether
    // they have the Description field.
    // To limit the results returned, use a filter.
    $entities = $result->getEntities();

    foreach($entities as $entity){
        $description = $entity->getProperty("Description")->getValue();
        echo $description."<br />";
    }

## <a name="update-an-entity"></a>Päivitä kohteen

Olemassa olevan kohteen voidaan päivittää käyttämällä **kohde -> AsetaOminaisuus** - ja **yritys -> addProperty** menetelmiä yksikön ja soittaminen **TableRestProxy -> updateEntity**. Seuraava esimerkki noutaa kohteen, muokkaa yhden ominaisuuden, poistaa uuden ominaisuuden ja Lisää uusi ominaisuus. Huomaa, että voit poistaa ominaisuuden sen arvo on **Null**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

    $entity = $result->getEntity();

    $entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

    $entity->setPropertyValue("Location", null); //Removed Location.

    $entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

    try {
        $tableRestProxy->updateEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-an-entity"></a>Poista kohde

Jos haluat poistaa kohteen, siirtää taulukon nimen ja yrityksen `PartitionKey` ja `RowKey` **TableRestProxy -> deleteEntity** -menetelmää.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete entity.
        $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Huomaa, että samanaikainen tarkistuksien, voit määrittää kohteen Etag poistetaan **DeleteEntityOptions -> setEtag** -menetelmällä ja lähettämistä **DeleteEntityOptions** objektin **deleteEntity** kuin neljännen parametria.

## <a name="batch-table-operations"></a>Erän taulukkotoiminnot

Voit suorittaa useita toimintoja yksittäisen pyynnön **TableRestProxy -> Erä** -menetelmää. Seuraavassa kuviossa tehdään toimintojen lisääminen **BatchRequest** objekti ja sitten lähettämistä **BatchRequest** objektin **TableRestProxy -> Erä** -menetelmää. Jos haluat lisätä toiminnon **BatchRequest** -objekti, voit soittaa jollakin seuraavista tavoista useita kertoja:

* **addInsertEntity** (Lisää insertEntity-toiminto)
* **addUpdateEntity** (Lisää updateEntity-toiminto)
* **addMergeEntity** (Lisää mergeEntity-toiminto)
* **addInsertOrReplaceEntity** (Lisää insertOrReplaceEntity-toiminto)
* **addInsertOrMergeEntity** (Lisää insertOrMergeEntity-toiminto)
* **addDeleteEntity** (Lisää deleteEntity-toiminto)

Seuraavassa esimerkissä esitetään, miten voit suorittaa **insertEntity** ja **deleteEntity** yksittäisen pyynnön:

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;
    use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    // Create list of batch operation.
    $operations = new BatchOperations();

    $entity1 = new Entity();
    $entity1->setPartitionKey("tasksSeattle");
    $entity1->setRowKey("2");
    $entity1->addProperty("Description", null, "Clean roof gutters.");
    $entity1->addProperty("DueDate",
                          EdmType::DATETIME,
                          new DateTime("2012-11-05T08:15:00-08:00"));
    $entity1->addProperty("Location", EdmType::STRING, "Home");

    // Add operation to list of batch operations.
    $operations->addInsertEntity("mytable", $entity1);

    // Add operation to list of batch operations.
    $operations->addDeleteEntity("mytable", "tasksSeattle", "1");

    try {
        $tableRestProxy->batch($operations);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Saat lisätietoja jonottaminen taulukon toimintojen [Suorittamiseen ryhmätapahtumat][entity-group-transactions].

## <a name="delete-a-table"></a>Taulukon poistaminen

Lopuksi voit poistaa taulukon välittää **TableRestProxy -> deleteTable** -menetelmä taulukkonimi.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete table.
        $tableRestProxy->deleteTable("mytable");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet oppinut perustiedot Azure-taulukosta-palvelun, näistä linkeistä saat lisätietoja tallennustilan monimutkaisia tehtäviä.

- [Azuren tallennustilaan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/)

Lisätietoja on Katso myös [PHP Developer Center](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
