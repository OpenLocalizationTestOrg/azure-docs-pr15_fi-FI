<properties
    pageTitle="Opetusohjelma – aloittaminen Azure erä Python asiakkaan | Microsoft Azure"
    description="Lisätietoja Azure Siirtoerän ja niiden lisäämisestä kehittää yksinkertaisessa skenaariossa erä palvelua peruskäsitteet"
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/27/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-python-client"></a>Azure erä Python asiakkaan käytön aloittaminen

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

[Azure erä] perusteet[ azure_batch] ja [Erä Python] [ py_azure_sdk] asiakkaan keskustelemaan kirjoitettu Python pieni erä-sovellus. Odotamme miten kahden otoksen komentosarjojen käyttäminen käsittelemään rinnakkain työmäärää, valitse Linux näennäiskoneiden pilvi tarkoittaa ja miten ne käsitellä [Azuren tallennustilaan](./../storage/storage-introduction.md) tiedoston väliaikaisen ja noutaminen erä-palvelun. Lue yleisiä erä sovelluksen työnkulun voitto tärkeimmät osat erä esimerkiksi töitä, tehtäviä, jakavat perus ymmärtämisen ja Laske solmujen.

![Erän ratkaisu työnkulun (basic)][11]<br/>

## <a name="prerequisites"></a>Edellytykset

Tässä artikkelissa oletetaan, että sinulla on kokemusta Python ja Linux tuntemusta. Se myös oletetaan, että et pysty tilin luominen vaatimukset, jotka on määritetty alla Azure ja erä- ja -palvelut.

### <a name="accounts"></a>Tilit

- **Azure-tili**: Jos sinulla ei vielä ole Azure tilauksen, [vapaa Azure-tilin luominen][azure_free_account].
- **Erän tilin**: kun sinulla on Azure tilaus [Azure erä-tilin luominen](batch-account-create-portal.md).
- **Tallennustilan tilin**: Katso [Lisätietoja Azure](../storage/storage-create-storage-account.md)-tallennustilan tilin [Luo tallennustilan tili](../storage/storage-create-storage-account.md#create-a-storage-account) .

### <a name="code-sample"></a>Koodiesimerkki

Python opetusohjelman [koodin otoksen] [ github_article_samples] on yhtä monta erä MALLIKOODEJA löydy [azure-erä-objektit] [ github_samples] säilöön-GitHub. Voit ladata kaikki objektit valitsemalla **Kloonaa tai Lataa > Lataa ZIP-** säilöön-kotisivulla tai valitsemalla [azure-erä-mallit-master.zip] [ github_samples_zip] suoraan latauslinkki. Kun olet purkanut ZIP-tiedoston sisällön, kaksi komentosarjat opetusohjelmassa löytyvät `article_samples` hakemisto:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Python ympäristössä

Paikallisen työasema *python_tutorial_client.py* komentosarjan suorittamiseen tarvitset **Python kääntäjän** yhteensopiva version **2.7** tai **3.3 +**. Komentosarja on testattu Linux-ja Windows.

### <a name="cryptography-dependencies"></a>salausmenetelmä riippuvuudet

Sinun on asennettava [salaus] riippuvuudet[ crypto] vaatii kirjastossa `azure-batch` ja `azure-storage` Python paketit. Käyttämällä jotakin vastaava omaa ympäristöäsi seuraavien toimintojen tai [salaus asennuksen] viitata[ crypto_install] lataustiedot:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`

* CentOS

    `yum update && yum install -y gcc openssl-dev libffi-devel python-devel`

* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`

* Windows

    `pip install cryptography`

>[AZURE.NOTE] Jos asentaminen Python 3.3 + Linux-argumentille Python riippuvuudet python3 vastineet. Valitse esimerkiksi Ubuntu:`apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`

### <a name="azure-packages"></a>Azure-paketit

Asenna seuraavaksi **Azure Siirtoerän** ja **Azure-tallennustilan** Python-paketit. Voit tehdä tämän **pip** ja *requirements.txt* täällä:

`/azure-batch-samples/Python/Batch/requirements.txt`

Ongelma asentaa Siirtoerän ja tallennustilaa paketteja **pip** -komennolla:

`pip install -r requirements.txt`

Voit asentaa [azure erä] [ pypi_batch] ja [azure-tallennustilan] [ pypi_storage] Python paketteja manuaalisesti:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [AZURE.TIP] Sinun täytyy ehkä etuliite oman komentoja `sudo` lataamasta tiliä käytettäessä. Esimerkiksi `sudo pip install -r requirements.txt`. Katso lisätietoja asentamisesta Python pakettien [Asentamista pakettien] [ pypi_install] readthedocs.io käyttöön.

## <a name="batch-python-tutorial-code-sample"></a>Erän Python opetusohjelman koodi-Esimerkki

Erän Python opetusohjelman koodin otosten koostuu kaksi Python komentosarjojen ja muutamia datatiedostot.

- **python_tutorial_client.PY**: toimii Siirtoerän ja tallennustilaa palveluihin suorittaminen rinnakkain työmäärää Laske solmuissa (näennäiskoneiden). *Python_tutorial_client.py* komentosarja suoritetaan paikallisen työasemalle.

- **python_tutorial_task.PY**: komentosarja, joka voidaan laskea solmujen Azure suorittamiseen todellinen työmäärä. Esimerkin lisäksi *python_tutorial_task.py* jäsentää tekstin ladattujen Azuren tallennustilaan (input-tiedosto)-tiedostoon. Valitse se tuottaa tekstitiedosto (kohdetiedosto), joka sisältää ylimmän kolme sanat, jotka näkyvät lähdetiedoston luettelo. Kohdetiedosto luotuaan *python_tutorial_task.py* Lataa tiedosto Azure-tallennustilan. Tämä on ladattavissa asiakkaan komentosarjan suorittaminen työasemalle. *Python_tutorial_task.py* komentosarja suoritetaan rinnakkain useita Laske solmuissa erä-palvelussa.

- **./data/taskdata\*.txt**: nämä kolme tekstitiedostoista antaa syötteen Laske solmut suoritettavat tehtävät.

Seuraavassa kaaviossa on kuvattu ensisijainen toiminnot, jotka tehdään asiakas- ja tehtävä-komentosarjoja. Tavallinen tämä työnkulku on tyypillisiä monta Laske-ratkaisuja, jotka on luotu erä. Samalla, kun se osoittaa ei kaikkia ominaisuuksia, jotka ovat käytettävissä erä-palvelussa, erä lähes kaikissa skenaarioissa on tämän työnkulun osiin.

![Erän esimerkin][8]<br/>

[**Vaihe 1.**](#step-1-create-storage-containers) Luo **säilöjen** Azure-Blob-objektien tallennustilaan.<br/>
[**Vaihe 2.**](#step-2-upload-task-script-and-data-files) Tehtävän komentosarjan ja input tiedostojen lataaminen säilöjen.<br/>
[**Vaihe 3.**](#step-3-create-batch-pool) Luoda erää **resurssivarantoon**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3 a.** Resurssivarannon **StartTask** Lataa tehtävän komentosarja (python_tutorial_task.py) solmujen, kun he liittyä ryhmään.<br/>
[**Vaihe 4.**](#step-4-create-batch-job) Luoda erää **työn**.<br/>
[**Vaihe 5.**](#step-5-add-tasks-to-job) **Tehtävien** lisääminen projektiin.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5 a.** Tehtävät ajoitetaan suorittamaan solmuissa.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5 b.** Kunkin tehtävän sen syöttötiedot lataamisen Azuren tallennustilaan ja valitse suoritus alkaa.<br/>
[**Vaihe 6.**](#step-6-monitor-tasks) Seuraa tehtäviä.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6 a.** Valmiiksi tehtävät ne Lataa tulosteen tietonsa Azure-tallennustilan.<br/>
[**Vaihe 7: ssä.**](#step-7-download-task-output) Lataa tehtävän tulosteen tallennustilan.

Mainittiin, kaikki erän ratkaisu suorittaa tarkka seuraavasti ja voivat sisältää useita enemmän, mutta tässä esimerkissä kuvataan löytyvät erä ratkaista yleisiä prosesseja.

## <a name="prepare-client-script"></a>Valmistele asiakaskomentosarja

Ennen kuin suoritat otosten, lisätä *python_tutorial_client.py*Siirtoerän ja tallennustilaa tilin tunnistetiedot. Jos et ole vielä niin, Avaa tiedosto Suosikit-editorissa ja päivitä seuraavat rivit tunnistetiedot.

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
batch_account_name = "";
batch_account_key  = "";
batch_account_url  = "";

# Storage account credentials
storage_account_name = "";
storage_account_key  = "";
```

Löydät Siirtoerän ja tallennustilaa tilin tunnistetiedot kuluessa kunkin palvelun tili-sivu [Azure portal][azure_portal]:

![Erän tunnistetiedot portaalissa][9]
![tallennustilan tunnistetiedot-portaalissa][10]<br/>

Seuraavissa osissa on analyysi käyttämä komentosarjat käsittelemään työmäärää erä palvelussa vaiheet. On suositeltavaa viitata säännöllisesti komentosarjat-muokkausohjelmassa artikkelin loput tene työskentelyn aikana.

Siirry seuraava rivi **python_tutorial_client.py** Aloita vaihe 1:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Vaihe 1: Luo tallennustilan säiliöiden

![Luo säilöjen Azuren tallennustilaan][1]
<br/>

Erän sisältää valmiita tuen käyttäminen Azure-tallennustilan. Säilöjen tallennustilan tilisi antaa erä tilisi suoritettavien tehtävien tarvitsemia tiedostoja. Säilöjen tarjoavat myös tallennuspaikka tiedot, jotka tuottavat tehtävät. Ensiksi *python_tutorial_client.py* komentosarja tekee on kolme säilöjen luominen [Azure-Blob-säiliö](../storage/storage-introduction.md#blob-storage):

- **sovelluksen**: Tämä säilö tallentaa tehtävien *python_tutorial_task.py*Python komentosarjaa.
- **syötteen**: tehtävien Lataa käsittelemään *syötteen* säilöstä datatiedostot.
- **tulos**: Kun Tehtävien käsittely tiedoston, he lataavat tulokset *tulosteen* säilöön.

Jotta voit käsitellä tallennustilan tilin ja luo säilöt-Käytämme [azure-tallennustilan] [ pypi_storage] paketin luominen [BlockBlobService] [ py_blockblobservice] objektin--"blob asiakas." Nyt luoda kolme säilöt-tallennustilan tilin blob-asiakas.

```python
 # Create the blob client, for use in obtaining references to
 # blob storage containers and uploading files to containers.
 blob_client = azureblob.BlockBlobService(
     account_name=_STORAGE_ACCOUNT_NAME,
     account_key=_STORAGE_ACCOUNT_KEY)

 # Use the blob client to create the containers in Azure Storage if they
 # don't yet exist.
 app_container_name = 'application'
 input_container_name = 'input'
 output_container_name = 'output'
 blob_client.create_container(app_container_name, fail_on_exist=False)
 blob_client.create_container(input_container_name, fail_on_exist=False)
 blob_client.create_container(output_container_name, fail_on_exist=False)
```

Säilöjen on luotu, kun sovellus nyt ladata tiedostoja, jotka käyttävät tehtäviin.

> [AZURE.TIP] [Azure-Blob-säiliö-Python käyttämisestä](../storage/storage-python-how-to-use-blob-storage.md) on hyvä yleiskatsaus Azure-tallennustilan säiliöiden ja BLOB-objektit. Sen pitäisi olla lukeminen luettelon yläosassa kuin alat erän kanssa.

## <a name="step-2-upload-task-script-and-data-files"></a>Vaihe 2: Tehtävien komentosarja ja tietojen tiedostojen lataaminen

![Tehtävä-sovelluksen ja syötteen (tiedot)-tiedostojen lataaminen säiliöiden][2]
<br/>

Lataa tiedostotoiminto *python_tutorial_client.py* määrittää ensin **sovelluksen** ja **syötteen** tiedostopolkujen hallintatyökalua, sellaisena kuin ne ovat paikallisessa tietokoneessa. Valitse Lataa tiedostot säilöjen, jonka loit edellisessä vaiheessa.

```python
 # Paths to the task script. This script will be executed by the tasks that
 # run on the compute nodes.
 application_file_paths = [os.path.realpath('python_tutorial_task.py')]

 # The collection of data files that are to be processed by the tasks.
 input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                     os.path.realpath('./data/taskdata2.txt'),
                     os.path.realpath('./data/taskdata3.txt')]

 # Upload the application script to Azure Storage. This is the script that
 # will process the data files, and is executed by each of the tasks on the
 # compute nodes.
 application_files = [
     upload_file_to_container(blob_client, app_container_name, file_path)
     for file_path in application_file_paths]

 # Upload the data files. This is the data that will be processed by each of
 # the tasks executed on the compute nodes in the pool.
 input_files = [
     upload_file_to_container(blob_client, input_container_name, file_path)
     for file_path in input_file_paths]
```

Käyttämällä luettelon ymmärtämistä `upload_file_to_container` funktio kutsutaan kaikille tiedostoille kokoelmien ja kaksi [ResourceFile] [ py_resource_file] sivustokokoelmat täytetään. `upload_file_to_container` -Funktio on alla:

```
def upload_file_to_container(block_blob_client, container_name, file_path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """
    blob_name = os.path.basename(file_path)

    print('Uploading file {} to container [{}]...'.format(file_path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles

[ResourceFile] [ py_resource_file] sisältää tehtävien osalta ja Azure-tallennustilan, jota ladataan Laske-solmu, ennen kuin tehtävä on suoritettu tiedoston URL-osoite. [ResourceFile][py_resource_file]. **blob_source** ominaisuus määrittää olemassa Azuren tallennustilaan tiedoston täydellinen URL-osoite. URL-Osoitteen voi olla myös jaetun access-allekirjoitus (SAS), joka sisältää tiedoston suojattu käyttö. Useimmat tehtävälajit osalta sisältävät *ResourceFiles* -ominaisuutta, mukaan lukien:

- [CloudTask][py_task]
- [StartTask][py_starttask]
- [JobPreparationTask][py_jobpreptask]
- [JobReleaseTask][py_jobreltask]

Tässä esimerkissä ei käytä JobPreparationTask tai JobReleaseTask tehtävälajit, mutta voit lukea lisää tietoja niiden [asennuksen valmistelu ja valmistuminen projektitehtävien Azure erän Laske solmujen](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Jaettu käyttö allekirjoitus (SAS)

Jaettu käyttö allekirjoitukset ovat merkkijonoja, jotka on suojattu käyttö säilöjen ja BLOB Azuren tallennustilaan. *Python_tutorial_client.py* komentosarja käyttää sekä Blob-objektien ja säilö jaettu access allekirjoitukset ja kerrotaan, miten voit hankkia jaettua käyttöä allekirjoituksen merkkijonon tallennustilan-palvelusta.

- **Blob-objektien jaettu access allekirjoitukset**: ryhmän StartTask käyttää blob jaettua käyttöä allekirjoitukset, kun lataamisen tallennustilan tehtävän komentosarjan ja input-datatiedostoja (Katso alla [Vaihe 3](#step-3-create-batch-pool) ). `upload_file_to_container` *Python_tutorial_client.py* -funktion sisältää koodi, joka hakee kunkin Blob-objektien jaettua käyttöä allekirjoitus. Se tekee soittamalla [BlockBlobService.make_blob_url] [ py_make_blob_url] tallennustilan-moduulissa.

- **Säilön jaettua käyttöä allekirjoitus**: kunkin tehtävän sen käsitelleet Laske-solmu, kun se latauksia sen kohdetiedosto *tulosteen* säilöön Azure-tallennustilan. Voit tehdä *python_tutorial_task.py* käyttää säilö jaettua käyttöä allekirjoitus, joka sisältää säilö kirjoitusoikeudet. `get_container_sas_token` *Python_tutorial_client.py* -funktion hakee säilö jaettua käyttöä allekirjoitus, joka sitten välitetään komentorivin argumenttina tehtäviin. Vaihe #5 [tehtävien lisääminen projektin](#step-5-add-tasks-to-job)käsitellään säilö SAS käyttö.

> [AZURE.TIP] Kaksiosainen sarjat-jaettu käyttö allekirjoituksia, tutustu [osa 1: tietoja SAS mallin](../storage/storage-dotnet-shared-access-signature-part-1.md) ja [osa 2: luominen ja käyttäminen SAS Blob-palvelussa](../storage/storage-dotnet-shared-access-signature-part-2.md), saat lisätietoja tallennustilan tilisi tiedot suojattu käyttömahdollisuus.

## <a name="step-3-create-batch-pool"></a>Vaihe 3: Luoda erää resurssivarantoon

![Luoda erää varannon][3]
<br/>

Erän **varanto** on kokoelma Laske solmujen (näennäiskoneiden), erä suorittaa projektin tehtävät.

Sen jälkeen, kun se lataa tehtävän komentosarjan ja datatiedostot tallennustilan tilin, *python_tutorial_client.py* erä Python-moduulin käynnistää sen vuorovaikutus erä-palvelussa. Voit tehdä tämän [BatchServiceClient] [ py_batchserviceclient] on luotu:

```python
 # Create a Batch service client. We'll now be interacting with the Batch
 # service in addition to Storage.
 credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                              _BATCH_ACCOUNT_KEY)

 batch_client = batch.BatchServiceClient(
     credentials,
     base_url=_BATCH_ACCOUNT_URL)
```

Seuraavaksi Laske solmujen resurssivarantoon luodaan puhelun erä tiliä `create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Kun luot resurssivarantoon, voit määrittää [PoolAddParameter] [ py_pooladdparam] , joka määrittää useita ominaisuuksia ryhmään:

- **Tunnus** altaan (*id* - pakollinen)<p/>Useimpien kohteiden osalta, jossa uusi ryhmä on oltava yksilöllinen tunnus erä-tilisi. Koodi viittaa kannan sen tunnuksen avulla, ja se on miten Azure- [portaalin]resurssivarantoon tunnistaa[azure_portal].

- **Laske solmujen määrän** (*target_dedicated* - pakollinen)<p/>Tämä ominaisuus määrittää, kuinka monta VMs altaan käyttöön. On tärkeää muistaa, että kaikki erän-tileillä on oletusarvo- **kiintiön** , joka rajoittaa **sydämiä** (ja täten Laske solmujen) erä tilillä. Voit etsiä oletusarvon kiintiön ja ohjeet (kuten sydämiä erä tilisi enimmäismäärä) [kiintiön Suurenna](batch-quota-limit.md#increase-a-quota) [kiintiön](batch-quota-limit.md)ja Azure erä palvelun rajoitukset. Jos tiedä, jossa kysytään, "Miksi ei Omat resurssivarantoon saavuttaa yli X solmujen?" core-kiintiön voi aiheuttaa.

- **Käyttöjärjestelmä** solmujen (*virtual_machine_configuration* **tai** *cloud_service_configuration* - pakollinen)<p/>*Python_tutorial_client.py*on luoda Linux solmujen käyttämällä [VirtualMachineConfiguration]resurssivarantoon[py_vm_config]. `select_latest_verified_vm_image_with_node_agent_sku` Toimivat `common.helpers` yksinkertaistaa [Azuren näennäiskoneiden Marketplace] käsitteleminen[ vm_marketplace] kuvia. Lisätietoja Marketplace kuvien käyttämisestä on kohdassa [Säännöstä Linux Laske Azure erä jakavat solmujen](batch-linux-nodes.md) .

- **Laske solmujen koosta** (*vm_size* - pakollinen)<p/>Koska määritämme alkamispäivämäärän Linux solmujen, tutustu [VirtualMachineConfiguration][py_vm_config], emme Määritä AM koko (`STANDARD_A1` tässä esimerkissä) [koot näennäiskoneiden Azure](../virtual-machines/virtual-machines-linux-sizes.md). Uudelleen Lisätietoja on kohdassa [säännöstä Linux Laske Azure erä jakavat solmujen](batch-linux-nodes.md) .

- **Aloita tehtävä** (*start_task* - ei ole pakko täyttää)<p/>Mukana edellä fyysinen solmujen ominaisuudet, voit myös määrittää [StartTask] [ py_starttask] (se ei ole pakollinen) resurssivarantoon varten. StartTask suorittaa kunkin solmun solmun yhdistää altaan ja aina, kun solmu käynnistetään. StartTask on hyötyä valmistelemiseksi Laske solmujen tehtäviä, kuten tehtävien suoritettavien sovellusten asentaminen suorittamista varten.<p/>Tämän esimerkkisovelluksen StartTask kopioi tiedostot, jotka se lataamisen säilöstä (joka on määritetty StartTask **resource_files** -ominaisuuden avulla) StartTask *toimi directory* *jaettuun* kansioon, joka käyttää solmun kaikki tehtävät. Kopioidaan `python_tutorial_task.py` kunkin solmun jaettu kansio, kun solmu yhdistää resurssivarantoon, jotta kaikki tehtävät, jotka suoritetaan solmun voit käyttää sitä.

Saatat huomata puhelu `wrap_commands_in_shell` avustaja-funktiota. Tämä funktio on kokoelma, erillisessä komentoja ja luo yksittäinen komentorivi vastaava tehtävän komentorivin ominaisuus.

Myös huomattavat yllä koodikatkelman on kaksi ympäristömuuttujien StartTask **Komentorivi** -ominaisuuden käyttö: `AZ_BATCH_TASK_WORKING_DIR` ja `AZ_BATCH_NODE_SHARED_DIR`. Kukin Laske solmu erä varannon määritetään automaattisesti useita ympäristömuuttujat, jotka ovat erä. Prosessi, joka on suorittaa tehtävän käyttää muuttujia ympäristössä.

> [AZURE.TIP] Lisätietoja ympäristön muuttujat, jotka ovat käytettävissä erä resurssivarantoon sekä tiedot tehtävän toimimasta hakemistojen selaaminen, Laske solmuissa kohdassa **ympäristöasetuksia tehtävien** ja **tiedostojen ja kansioiden** [Azure erä ominaisuuksien yleiskatsaus](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Vaihe 4: Luoda erää työ

![Luoda erää työn][4]<br/>

Erän **työ** on kokoelma, tehtävät ja Laske solmujen resurssivarantoon liittyy. Projektin tehtävien suorittaa liittyvät resurssivarantoon Laske solmuissa.

Voit käyttää projektin ei vain järjestäminen ja tehtäviin liittyvät työmääriä, mutta myös tiettyjä rajoituksia – kuten suurin runtime työn (ja sen tehtävien tunnisteesta) käyttöön seurantaa varten ja projektin prioriteetin suhteessa muihin kohteisiin erä-tili. Tässä esimerkissä kuitenkin työ on liitetty vain ryhmän, joka on luotu vaiheessa #3. Muita ominaisuuksia ei ole määritetty.

Kaikki erän työt liittyy tiettyjä resurssivarantoon. Tämä kytkentä ilmaisee projektin tehtävät suorittaa solmut. Voit määrittää ryhmän käyttämällä [PoolInformation] [ py_poolinfo] ominaisuutta, kuten alla koodikatkelman.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Nyt kun työ on luotu, tehtävät lisätään työn tekemiseen.

## <a name="step-5-add-tasks-to-job"></a>Vaihe 5: Tehtävien lisääminen projektiin

![Tehtävien lisääminen projektiin][5]<br/>
*(1) tehtävät lisätään työ, (2) tehtävät ajoitetaan voidaan suorittaa solmujen ja (3) tehtävät ladata tiedostojen käsittely*

Erän **tehtävät** ovat yksittäisiä yksiköitä tehdyn työn, joka suorittaa Laske solmut. Tehtävä on komentorivin ja suorittaa komentosarjoja tai suoritettavat tiedostot, jotka määrität kyseisen komentoriviltä.

Todellisuudessa tekemässä töitä, tehtäviä on lisättävä projektin. Kunkin [CloudTask] [ py_task] on määritetty komentorivi-ominaisuus ja [ResourceFiles] [ py_resource_file] (kuten ryhmän StartTask kanssa), joka tehtävän Lataa solmu ennen sen komentoriviltä suoritetaan automaattisesti. Esimerkin lisäksi kunkin tehtävän käsittelee vain yhden tiedoston. Näin ollen ResourceFiles sen sivustokokoelman on yksi elementti.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [AZURE.IMPORTANT] Kun hän käyttää ympäristömuuttujat kuten `$AZ_BATCH_NODE_SHARED_DIR` tai suorita jokin sovellus ei löydy solmun `PATH`-komento tehtävärivien täytyy käynnistää käyttöliittymä erikseen, kuten kanssa `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Tämä vaatimus ei tarvita, jos tehtävien suorittaminen sovelluksen solmun `PATH` ja viittaa kaikki ympäristömuuttujat.

Sisällä `for` yllä koodikatkelman silmukka, voit tarkastella tehtävän komentorivin muodostetaan viisi komentorivin argumentit, jotka on välitetty *python_tutorial_task.py*kanssa:

1. **tiedostopolku**: Tämä on paikallinen polku tiedoston solmun olemassa. Kun ResourceFile objekti on `upload_file_to_container` on luotu edellä vaiheessa 2-tiedostonimi on käytetty tämän ominaisuuden ( `file_path` parametrin ResourceFile konstruktorissa). Tämä ilmaisee, että tiedoston löytyvät *python_tutorial_task.py*solmun samassa kansiossa.

2. **NUMWORDS**: ylimmät *N* sanat on kirjoitettava kohdetiedosto.

3. **storageaccount**:, joka omistaa säilön, johon tehtävä tulokset ladattava tallennustilan tilin nimi.

4. **storagecontainer**: nimi, johon tulostus-tiedostoja ladataan säilytykseen.

5. **sastoken**: jaettua käyttöä allekirjoitus (SAS), joka sisältää Azuren tallennustilaan **tulosteen** säilöön kirjoitusoikeudet. *Python_tutorial_task.py* komentosarja käyttää jaettua käyttöä allekirjoitusta kun Luo sen BlockBlobService viittaus. Tämä on kirjoitusoikeus säilöön tarvitsematta pikanäppäin tallennustilan tilin.

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Vaihe 6: Näytön tehtävät

![Näytön tehtävät][6]<br/>
*Komentosarjan (1) valvoo tilan tehtävät ja (2) tehtävät lataa tiedot Azuren tallennustilaan*

Kun tehtäviin on lisätty projektiin, ne jonossa ja ajoittaa suorittamisen käyttöön liittyvän työn resurssivarantoon sijaitsevien solmujen suorittaminen automaattisesti. Voit määrittää asetusten perusteella erä käsittelee kaikki tehtävän queuing, ajoitus, yritetään ja muiden tehtävien hallinta-tehtävät puolestasi.

On monia tavoista seurantaan tehtävän suorittaminen. `wait_for_tasks_to_complete` *Python_tutorial_client.py* -funktion on esimerkki seurannan tehtävien tiettyjen tilan tässä tapauksessa [Valmis] [ py_taskstate] tila.

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>Vaihe 7: Lataa tehtävän tulos

![Lataa tehtävän tulosteen tallennustila][7]<br/>

Nyt kun työ on valmis, tuloste tehtävät voi ladata Azure-tallennustilan. Tämä tehdään puhelun `download_blobs_from_container` - *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [AZURE.NOTE] Puhelun `download_blobs_from_container` *python_tutorial_client.py* määrittää, että tiedostot ladattu koti-kansioon. Vapaasti muokata tämän tulosteen sijainti.

## <a name="step-8-delete-containers"></a>Vaihe 8: Poista säilöt

Perittävän tiedot, jotka sijaitsevat Azuren tallennustilaan, koska se on aina kannattaa poistaa minkä tahansa BLOB-objektit, erä töiden ei enää tarvita. *Python_tutorial_client.py*, valitse tämä tehdään kolme puhelut [BlockBlobService.delete_container][py_delete_container]:

```
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Vaihe 9: Työn ja ryhmän poistaminen

Viimeisessä vaiheessa sinua kehotetaan työn ja resurssivarantoon, jotka on luotu *python_tutorial_client.py* -komentosarjan poistaminen. Vaikka ei peri projektien ja tehtävien itse *ovat* veloitetaan Laske solmujen. Näin on suositeltavaa varata solmujen vain tarvittaessa. Käyttämättömien jakavat poistaminen voi olla ylläpito yhteydessä.

BatchServiceClient [JobOperations] [ py_job] ja [PoolOperations] [ py_pool] molemmissa vastaavia poistaminen menetelmiä, joita kutsutaan, jos Vahvista poistaminen:

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [AZURE.IMPORTANT] Ota huomioon, jotka veloitetaan Laske resurssien--poistaminen käyttämättömät jakavat minimoi kustannukset. Huomioon lisäksi, että poistaminen resurssivarantoon poistaa kaikki kyseisen ryhmän Laske solmut ja tietojen käsittelyssä solmut olevan vakava altaan poistamisen jälkeen.

## <a name="run-the-sample-script"></a>Esimerkki-komentosarjan suorittaminen

Kun suoritat *python_tutorial_client.py* komentosarja opetusohjelman [koodin otoksen][github_article_samples]-konsolin tulos on seuraava. Ei keskeytä osoitteessa `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` ryhmän Laske solmujen luodaan alkanut, ja komennot ryhmän Käynnistä tehtävä on suoritettu. Käytä [Azure portal] [ azure_portal] seurannassa lisääminen resurssivarantoon Laske solmujen, työn ja tehtävien aikana ja suorittamisen jälkeen. Käytä [Azure portal] [ azure_portal] tai [Microsoft Azure-tallennustilan Explorer] [ storage_explorer] voit tarkastella tallennustilan resurssit (säilöjä ja BLOB), jotka on luotu sovellus.

>[AZURE.TIP] Suorita *python_tutorial_client.py* -komentosarja-toimintoa `azure-batch-samples/Python/Batch/article_samples` hakemisto. Tiedostossa käytetään suhteellinen polku `common.helpers` moduulin tuominen, joten jotkut `ImportError: No module named 'common'` Jos ei toimi komentosarjaa tähän kansioon.

Tyypillinen suoritusaika on **noin 5 – 7 minuuttia** , kun suoritat otosten sen oletusarvo-asetukset.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a>Seuraavat vaiheet

Vapaasti tehdä muutoksia *python_tutorial_client.py* ja *python_tutorial_task.py* kokeilemaan eri Laske skenaarioita. Kokeile esimerkiksi suorittamisen viiveen lisääminen *python_tutorial_task.py* simuloida pitkään suoritettavien tehtävien ja seurata niitä portaalissa. Kokeile lisäämään useita tehtäviä tai Laske solmujen määrän muuttaminen. Lisää logiikka etsiä ja Salli aiemmin resurssivarantoon, nopeuden suorittamisen ajaksi.

Nyt kun osaat käyttää erä ratkaista basic työnkulun, on, kun haluat tarkastella erä palvelun lisäominaisuuksia.

- Tarkista [Azure erä yleiskatsaus ominaisuuksista](batch-api-basics.md) artikkelista, jossa on suositeltavaa, jos ole ennen käyttänyt palvelua.
- Käynnistä erä kehittäminen artikkeliin kohdassa **Development perusteellisempaa** [erä oppimispolku][batch_learning_path].
- Tutustu eri soveltaminen käsittely "ylimmät N sanat" työmäärää [TopNWords] erän[ github_topnwords] malli.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage

[pypi_install]: http://python-packaging-user-guide.readthedocs.io/en/latest/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Luo säilöjen Azuren tallennustilaan"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Tehtävä-sovelluksen ja syötteen (tiedot)-tiedostojen lataaminen säiliöiden"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Luoda erää resurssivarantoon"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Luoda erää työn"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Tehtävien lisääminen projektiin"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Näytön tehtävät"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Lataa tehtävän tulosteen tallennustila"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Erän ratkaisu työnkulku (koko kaavio)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Erän tunnistetiedot-portaalissa"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Tallennustilan tunnistetiedot-portaalissa"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Erän ratkaisu työnkulun (mahdollisimman vähän kaavio)"
