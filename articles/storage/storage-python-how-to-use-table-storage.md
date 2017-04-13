<properties
    pageTitle="Taulukon tallennustilaa Python käyttämisestä | Microsoft Azure"
    description="Voit tallentaa jäsenneltyjen tietojen avulla Azure-taulukkotallennus, NoSQL tietosäilö pilvipalveluun."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-python"></a>Opi käyttämään Python-taulukkotallennus

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Yleiskatsaus

Tämän oppaan avulla voit suorittaa yleisiä tilanteita, joissa Azure-taulukosta tallennuspalvelu käyttämällä. Mallit on kirjoitettu Python ja käytä [Microsoft Azure tallennustilan SDK Python]. Asianmukaiset käyttötavoista Sisällytä luominen ja poistaminen taulukoksi, lisääminen ja kysely-taulukon kohteiden lisäksi.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Taulukon luominen

**TableService** objektin voit taulukon palveluiden kanssa. Seuraava koodi luo **TableService** objektin. Lisää lähellä sivun, jossa Python tiedostojen, jota haluat käyttää ohjelmallisesti Azuren tallennustilaan koodi:

    from azure.storage.table import TableService, Entity

Seuraava koodi luo **TableService** objektin tallennustilan tilin nimi ja avaimen avulla.  Korvaa 'myaccount' ja 'OmaAvain' tilin nimi ja avaimen avulla.

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## <a name="add-an-entity-to-a-table"></a>Kohteen lisääminen taulukkoon

Jos haluat lisätä kohteen, luo ensin sanaston tai kohde, joka määrittää kohteen ominaisuuksien nimiä ja arvot. Huomaa, että jokaisen kohteen, sinun on määritettävä **PartitionKey** ja **RowKey**. Nämä ovat kohteiden yksilölliset tunnukset. Voit tehdä kyselyn käyttäminen nämä arvot paljon nopeammin kuin voit tehdä kyselyn muita ominaisuuksia. Järjestelmä käyttää **PartitionKey** taulukon kohteiden jakaa monta tallennustilan solmujen automaattisesti.
Henkilöt, joilla on sama **PartitionKey** on tallennettu saman solmun. **RowKey** on yksilöllinen tunnus osio, joka kuuluu yritys.

Voit lisätä taulukon kohteen, välittää sanasto-objekti **Lisää\_kohteen** menetelmää.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

Voit myös siirtää **kohteen** luokan esiintymä **Lisää\_kohteen** menetelmää.

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## <a name="update-an-entity"></a>Päivitä kohteen

Tämä koodi näytetään, miten voit korvata olemassa olevan kohteen vanhaa versiota päivitetyn version.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

Jos yritys, joka päivitetään ei ole, päivitystoiminto epäonnistuu. Jos haluat tallentaa kohteen riippumatta siitä, onko se oli ennen, käytä **Lisää\_tai\_replace_entity**.
Seuraavassa esimerkissä ensimmäinen kutsu korvaa olemassa olevan kohteen. Kutsu Lisää uuden kohteen, koska ei ole määritetty **PartitionKey** kohteella ja **RowKey** on taulukossa.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

## <a name="change-a-group-of-entities"></a>Kohteiden ryhmän muuttaminen

Joskus kannattaa lähettää useita toimintoja yhdessä erä varmistamiseksi atomisia käsittely-palvelin. Voit tehdä tämän, voit käyttää **TableBatch** -luokka. Kun haluat lähettää erän, voit soittaa **Vahvista\_erän**. Huomaa, että kaikki kohteet on oltava sama osion voidaksesi voi muuttaa erissä. Alla olevassa esimerkissä laskee yhteen kaksi kohteiden erän.

    from azure.storage.table import TableBatch
    batch = TableBatch()
    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    batch.insert_entity(task10)
    batch.insert_entity(task11)
    table_service.commit_batch('tasktable', batch)

Erissä voidaan myös contex hallinnan syntaksilla:

    task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
    task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

    with table_service.batch('tasktable') as batch:
        batch.insert_entity(task12)
        batch.insert_entity(task13)


## <a name="query-for-an-entity"></a>Kyselyn kohteen

Kyselyn kohteen taulukon, käytä **Hae\_kohteen** menetelmää välittämällä **PartitionKey** ja **RowKey**.

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## <a name="query-a-set-of-entities"></a>Kyselyn kohteiden määrittäminen

Tämän esimerkin löytää Seattle kaikkien tehtävien perusteella **PartitionKey**.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## <a name="query-a-subset-of-entity-properties"></a>Kyselyn alijoukkoa kohteen ominaisuudet

Taulukon kyselyn noutaa todella ominaisuudet kohteen.
Tätä tapaa kutsutaan *Ennuste*, vähentää kaistanleveyden ja parantaa kyselyn suorituskykyä, erityisesti niiden suurimpia kohteita. **Valitse** parametrin käytettävä ja välittää nimet, ominaisuudet, jotka haluat tuoda asiakkaalle.

Seuraava koodi kysely palauttaa vain kohteiden kuvaukset taulukossa.

[AZURE.NOTE] Seuraavat koodikatkelman toimii vain vastaan pilvitallennuspalveluun. Tämä ei tue tallennustilan emulaattori.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
    for task in tasks:
        print(task.description)

## <a name="delete-an-entity"></a>Poista kohde

Voit poistaa kohteen sen osio ja rivin avaimen avulla.

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## <a name="delete-a-table"></a>Taulukon poistaminen

Seuraava koodi poistaa taulukon tallennustilan tilin.

    table_service.delete_table('tasktable')

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut taulukkotallennus perusteet, noudata näitä linkkejä lisätietoja.

- [Python Developer Center](/develop/python/)
- [Azure liittyviä palveluja REST-Ohjelmointirajapinnalla](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure-tallennustilan tiimin blogia]
- [Microsoft Azuren tallennustilaan Python SDK]

[Azure tallennustilan ryhmäblogi]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azuren tallennustilaan Python SDK]: https://github.com/Azure/azure-storage-python
