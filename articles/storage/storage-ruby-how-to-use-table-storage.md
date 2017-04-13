<properties
    pageTitle="Opi käyttämään Azure-taulukkotallennus Ruby | Microsoft Azure"
    description="Voit tallentaa jäsenneltyjen tietojen avulla Azure-taulukkotallennus, NoSQL tietosäilö pilvipalveluun."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor=""/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-ruby"></a>Opi käyttämään Ruby-Azure-taulukkotallennus

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Yleiskatsaus

Tämän oppaan avulla voit suorittaa yleisiä tilanteita, joissa Azure-taulukosta-palvelun avulla. Mallit kirjoitetaan Ruby-Ohjelmointirajapinnan käyttäminen. Tilanteita, joissa kattaa Sisällytä **luominen ja taulukon poistaminen, lisääminen ja kysely-taulukon kohteiden**.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby-sovelluksen luominen

Ohjeet Ruby sovelluksen luomisesta on artikkelissa [Azure-AM kulkevia verkkosovelluksen Ruby](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).


## <a name="configure-your-application-to-access-storage"></a>Määritä sovelluksesi voi käyttää tallennustilan

Käytä Azure-tallennustilan, joudut lataamisesta ja käyttämisestä Ruby azure paketti, joka sisältää helppokäyttöisyys kirjastoja, joissa viestintä tallennustilan muiden palvelujen kanssa.

### <a name="use-rubygems-to-obtain-the-package"></a>Hanki paketin RubyGems avulla

1. Käytä käyttöliittymä esimerkiksi **PowerShell** (Windows), **terminaalissa** (Mac) tai **Bash** (Unix).

2. Kirjoita **gem asentaa azure** asentamalla gem ja riippuvuuksien komentoikkunassa.

### <a name="import-the-package"></a>Tuo pakkaaminen

Käyttää tuttuja tekstieditorissa, Lisää seuraava teksti Ruby tiedoston, jos aiot käyttää tallennustilan alkuun:

    require "azure"

## <a name="set-up-an-azure-storage-connection"></a>Azuren tallennustilaan-yhteyden määrittäminen

Azure moduulin lukee ympäristömuuttujat **AZURE\_TALLENNUSTILAN\_tilin** ja **AZURE\_TALLENNUSTILAN\_ACCESS\_AVAIMEN** , voit muodostaa yhteyden tiliisi Azuren tallennustilaan tarvittavat tiedot. Jos ympäristössä muuttujia ei määritetä, sinun on määritettävä tilitiedot ennen **Azure::TableService** käyttäminen seuraava koodi:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"

Voit hankkia nämä arvot perinteinen tai Resurssienhallinta-tallennustilan tilin Azure-portaalissa:

1. Kirjaudu sisään [Azure Portal](https://portal.azure.com).
2. Siirry tallennustilan-tili, jota haluat käyttää.
3. Valitse oikealla asetukset-sivu **Pikanäppäimet**.
4. Accessin näppäimet sivu, joka tulee näkyviin näet pikanäppäin 1 ja 2 pikanäppäin. Voit käyttää jommankumman nimenä. 
5. Kopioi Leikepöydälle avaimen kopio-kuvaketta. 

Voit hankkia nämä arvot perinteinen tallennustilan tililtä perinteinen Azure-portaalissa:

1. Kirjaudu sisään [perinteinen Azure portal](https://manage.windowsazure.com).
2. Siirry tallennustilan-tili, jota haluat käyttää.
3. Valitse siirtymisruudun alareunassa **Hallinta PIKANÄPPÄIMET** .
4. Pop-valintaikkunan näet tallennustilan tilin nimi, pikanäppäin ensisijainen ja toissijainen pikanäppäin. Pikanäppäin voit käyttää joko ensisijainen tai toissijainen tunnus. 
5. Kopioi Leikepöydälle avaimen kopio-kuvaketta.

## <a name="create-a-table"></a>Taulukon luominen

**Azure::TableService** objektin voit käsitellä taulukkoja ja kohteiden. Voit luoda taulukon käyttämällä **luominen\_table()** menetelmää. Seuraavassa esimerkissä luodaan taulukon tai Tulosta virheen, jos ei mitään.

    azure_table_service = Azure::TableService.new
    begin
      azure_table_service.create_table("testtable")
    rescue
      puts $!
    end

## <a name="add-an-entity-to-a-table"></a>Kohteen lisääminen taulukkoon

Jos haluat lisätä kohteen, luo ensin hash-objekti, joka määrittää kohteen ominaisuudet. Huomaa, että jokaisen kohteen on määritettävä **PartitionKey** ja **RowKey**. Nämä ovat kohteiden yksilölliset tunnukset ja ovat arvoja, jotka voidaan suorittaa kysely paljon nopeammin kuin muut ominaisuudet. Azure tallennuspaikka jakaa automaattisesti taulukon kohteiden monta tallennustilan solmujen **PartitionKey** . Kohteiden kanssa samaan **PartitionKey** on tallennettu saman solmun. **RowKey** on yksilöllinen tunnus kohteen kuuluu osioon.

    entity = { "content" => "test entity",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.insert_entity("testtable", entity)

## <a name="update-an-entity"></a>Päivitä kohteen

On olemassa olevan kohteen päivittää usealla eri tavalla:

* **päivittää\_entity():** Päivitä aiemmin kohteen korvaamalla se.
* **yhdistämisen\_entity():** Päivittää olemassa olevan kohteen yhdistämisessä ominaisuusarvoihin uusien olemassa olevan kohteen.
* **Lisää\_tai\_yhdistämisen\_entity():** Päivittää olemassa olevan kohteen korvaamalla se. Jos kohde ei ole olemassa, uuden lisätään:
* **Lisää\_tai\_korvaa\_entity():** Päivittää olemassa olevan kohteen yhdistämisessä ominaisuusarvoihin uusien olemassa olevan kohteen. Jos kohde ei ole olemassa, uuden lisätään.

Seuraavassa esimerkissä on kuvattu päivittäminen kohteen käyttämällä **päivittää\_entity()**:

    entity = { "content" => "test entity with updated content",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.update_entity("testtable", entity)

Kanssa **päivittää\_entity()** ja **yhdistämisen\_entity()**, jos kohde, jota olet päivittämässä ei ole sitten päivitys epäonnistuu. Tämän vuoksi, jos haluat tallentaa kohteen riippumatta siitä, onko se on jo olemassa, sen sijaan käytettävä **Lisää\_tai\_korvaa\_entity()** tai **Lisää\_tai\_yhdistämisen\_entity()**.

## <a name="work-with-groups-of-entities"></a>Kohteiden ryhmien käyttäminen

Joskus kannattaa lähettää useita yhdessä erä varmistamiseksi atomisia käsittelyn palvelin-toimintoja. Voit tehdä tämän luomalla ensin **erä** -objekti ja tee **suorittaa\_batch()** **TableService**menetelmää. Seuraavassa esimerkissä on kuvattu kaksi kohteiden RowKey 2 ja 3 erän lähettämistä. Huomaa, että se toimii vain kohteiden kanssa samaan PartitionKey.

    azure_table_service = Azure::TableService.new
    batch = Azure::Storage::Table::Batch.new("testtable",
      "test-partition-key") do
      insert "2", { "content" => "new content 2" }
      insert "3", { "content" => "new content 3" }
    end
    results = azure_table_service.execute_batch(batch)

## <a name="query-for-an-entity"></a>Kyselyn kohteen

Kyselyn kohteen taulukon, käytä **Hae\_entity()** menetelmää välittämällä taulukkonimi, **PartitionKey** ja **RowKey**.

    result = azure_table_service.get_entity("testtable", "test-partition-key",
      "1")

## <a name="query-a-set-of-entities"></a>Kyselyn kohteiden määrittäminen

Kyselyn joukon kohteiden taulukon, kyselyn hash-objektin luominen ja käyttäminen **kyselyn\_entities()** menetelmää. Seuraavassa esimerkissä käytön kaikki kohteet samaan **PartitionKey**kanssa:

    query = { :filter => "PartitionKey eq 'test-partition-key'" }
    result, token = azure_table_service.query_entities("testtable", query)

> [AZURE.NOTE] Jos tulosjoukko on liian suuri yhteen kyselyn palauttaa jatkumista tunnuksen palautetaan, jonka avulla voit hakea seuraaville sivuille.

## <a name="query-a-subset-of-entity-properties"></a>Kyselyn alijoukkoa kohteen ominaisuudet

Taulukon kyselyn noutaa todella ominaisuudet kohteen. Tällä menetelmällä nimeltä "ennuste" vähentää kaistanleveyden ja parantaa kyselyn suorituskykyä, erityisesti niiden suurimpia kohteita. Select-lauseen ja välittää haluat Tuo asiakkaan ominaisuuksien nimet.

    query = { :filter => "PartitionKey eq 'test-partition-key'",
      :select => ["content"] }
    result, token = azure_table_service.query_entities("testtable", query)

## <a name="delete-an-entity"></a>Poista kohde

Voit poistaa kohteen **poistaminen\_entity()** menetelmää. Sinun täytyy välittää Kirjoita taulukko, joka sisältää kohteen PartitionKey ja RowKey kohteen nimi.

        azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## <a name="delete-a-table"></a>Taulukon poistaminen

Voit poistaa taulukon **poistaminen\_table()** menetelmä ja salasana, kirjoita nimi, jonka haluat poistaa taulukon.

        azure_table_service.delete_table("testtable")

## <a name="next-steps"></a>Seuraavat vaiheet

Näistä linkeistä saat lisätietoja tallennustilan monimutkaisia tehtäviä:

- [Azure-tallennustilan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/)
- Valitse GitHub [Azure SDK Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) säilöön
