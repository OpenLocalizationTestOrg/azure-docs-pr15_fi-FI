<properties
    pageTitle="Azure erä jakavat Linux solmujen | Microsoft Azure"
    description="Opettele käsittelemään rinnakkain Laske-työmääriä jakavat Linux näennäiskoneiden Azure osalta, valitse."
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="marsma" />

# <a name="provision-linux-compute-nodes-in-azure-batch-pools"></a>Azure erä jakavat Linux Laske solmujen valmistelu

Voit suorittaa rinnakkaisia Laske työmääriä näennäiskoneiden Linux- ja Windows Azure erä. Tässä artikkelissa kuvataan, miten voit luoda jakavat Linux Laske solmujen erä-palvelun sekä [Erän Python] [ py_batch_package] ja [Erä .NET] [ api_net] asiakkaan kirjastoihin.

> [AZURE.NOTE] [Sovelluksen paketit](batch-application-packages.md) ovat ei tällä hetkellä tueta Linux Laske solmuissa.

## <a name="virtual-machine-configuration"></a>Virtuaalikoneen määritys

Kun luot Laske solmujen resurssivarantoon osalta, käytettävissä on kaksi vaihtoehtoista, joista voit valita solmun koon ja käyttöjärjestelmän: Cloud Services-kokoonpanon määritys ja virtuaalikoneen määritykset.

**Cloud Services-kokoonpanon määritys** on Windowsin Laske solmujen *vain*. Käytettävissä olevat Laske solmu koot on lueteltu [pilvipalveluihin koot](../cloud-services/cloud-services-sizes-specs.md)ja [Azure Vierailijan OS versioista ja SDK yhteensopivuus-matriisissa](../cloud-services/cloud-services-guestos-update-matrix.md)on lueteltu käyttöjärjestelmien. Luodessasi resurssivarantoon, joka sisältää Azure pilvipalveluihin solmujen, sinun täytyy määrittää vain solmun koon ja sen "OS perhe-" joka mainittiin artikkelit ei löydy. For Windows jakavat Laske solmujen, pilvipalveluihin käytetään useimmin.

**Virtuaalikoneen määritys** tarjoaa Laske solmujen Linux- ja Windows-kuvat. Käytettävissä olevat Laske solmu koot on lueteltu [koot näennäiskoneiden Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) ja [koot näennäiskoneiden Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Luodessasi resurssivarantoon, joka sisältää virtuaalikoneen määritysten solmujen kokoa solmut, virtuaalikoneen kuva viittaus ja erä solmu agentti tuote on asennettava solmut on määritettävä.

### <a name="virtual-machine-image-reference"></a>Virtuaalikoneen kuva viittaus

Erän palvelun käyttää [virtuaalikoneen asteikko määrittää](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) Linux Laske solmujen. Nämä näennäiskoneiden käyttöjärjestelmän kuvat ovat myöntämä [Azure Marketplacesta][vm_marketplace]. Kun määrität viittauksen virtuaalikoneen kuvan, voit määrittää Marketplace virtuaalikoneen kuvan ominaisuuksia. Seuraavat ominaisuudet ovat pakollisia, kun luot viittauksen virtuaalikoneen kuva:

| **Kuva viittaus ominaisuudet** | **Esimerkki** |
| ----------------- | ------------------------ |
| Publisherin         | Kanoninen                |
| Tarjouksen             | UbuntuServer             |
| TUOTE               | 14.04.4-LTS              |
| Versio           | uusimman                   |

> [AZURE.TIP] Saat lisätietoja näiden ominaisuuksien ja [Siirry ja valitse Linux virtuaalikoneen kuvat CLI tai PowerShellin Azure](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md)Marketplace kuvat-luettelo. Huomaa, että kaikki Marketplace-kuvat ovat sillä hetkellä yhteensopiva erä. Lisätietoja on artikkelissa [solmu agentti tuote](#node-agent-sku).

### <a name="node-agent-sku"></a>Solmun agentti SKU

Erän solmu agent on ohjelmaan, joka toimii kunkin ryhmän solmu ja tarjoaa ohjaus-ja liittymän solmu ja erä-palvelun välillä. Useilla eri käyttöotot solmu edustajan nimeltään tuotteissa, eri käyttöjärjestelmien. Kun luot Virtual kokoonpanossa, sinun on ensin määritettävä virtuaalikoneen kuva viittaus ja Määritä kuva asennetaan solmu-agentti. Yleensä kunkin solmu agent tuote on yhteensopiva useita virtuaalikoneen kuvia. Seuraavassa on muutamia esimerkkejä solmu agentti tuotteissa:

* Batch.node.Ubuntu 14.04
* Batch.node.centos 7
* Batch.node.Windows amd64

> [AZURE.IMPORTANT] Kaikki virtuaalikoneen kuvilla, jotka ovat käytettävissä olevat ovat yhteensopivia tällä hetkellä käytettävissä erän solmu agenttien vuoksi. Sinun on käytettävä erä SDK: T käytettävissä solmu-agentti tuotteissa ja virtuaalikoneen kuvia, jossa ne ovat yhteensopivia luettelon. Katso lisätietoja tämän artikkelin [luettelon virtuaalikoneen kuvia](#list-of-virtual-machine-images) .

## <a name="create-a-linux-pool-batch-python"></a>Luoda Linux varannon: erä Python

Seuraavat koodikatkelman näyttää, miten [Microsoft Azure erä asiakkaan kirjasto Python] [ py_batch_package] Ubuntu palvelimen resurssivarantoon Laske solmun luomiseksi. Oppaat erä Python moduulin tukikäytännöistä [azure.batch paketti] [py_batch_docs] -luku tiedostot.

Tämä koodikatkelman Luo [ImageReference] [ py_imagereference] erikseen ja määrittää eri ominaisuuksia (publisher-tarjouksen tuote-versio). Tuotannon koodissa, suosittelemme, että käytät [list_node_agent_skus] [ py_list_skus] menetelmä ja valitse käytettävissä olevat kuva ja solmu agentti SKU yhdistelmät suorituksen aikana.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Kuten edellä mainittiin, on suositeltavaa, että sijaan [ImageReference] [ py_imagereference] erikseen, voit käyttää [list_node_agent_skus] [ py_list_skus] menetelmän valitseminen dynaamisesti tuetut solmu agentti/Marketplace kuva yhdistelmät. Seuraavat Python koodikatkelman näyttää tätä menetelmää käyttö.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Luoda Linux varannon: erä .NET

Seuraavat koodikatkelman näyttää esimerkin käytöstä [Erä .NET] [ nuget_batch_net] asiakirjakirjaston luominen Ubuntu palvelimen resurssivarantoon Laske solmujen. Voit etsiä [erä .NET-oppaat] [ api_net] MSDN-sivuston.

Seuraavat koodikatkelman käyttää [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] menetelmää voit valita luettelosta, tällä hetkellä tueta Marketplace kuva ja solmu agentti SKU yhdistelmät. Tämä menetelmä ei olisi, koska tuetut yhdistelmien voivat muuttua aika. Yleisimmin tuettujen kombinaatioiden lisätään.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

Vaikka edellisen koodikatkelman käytetään [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] menetelmä, jolla dynaamisesti luettelo ja valitse tuettu kuva ja solmu agentti SKU yhdistelmät (suositus), voit myös määrittää [ImageReference] [ net_imagereference] erikseen:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Luettelo virtuaalikoneen kuvat

Seuraavassa taulukossa on lueteltu Marketplace virtuaalikoneen kuvat, jotka ovat yhteensopivia käytettävissä erän solmu käyttäjäagenttien tässä artikkelissa on päivitetty. On tärkeää muistaa, että tämä luettelo ei ole lopullinen, koska kuvia ja solmu käyttäjäagenttien voi lisätä tai poistaa milloin tahansa. On suositeltavaa, että erä sovellusten ja palveluiden Käytä aina [list_node_agent_skus] [ py_list_skus] (Python) ja [ListNodeAgentSkus] [ net_list_skus] (erä .NET) ja valitse tällä hetkellä käytettävissä tuotteissa.

> [AZURE.WARNING] Seuraavassa on luettelo voi muuttaa milloin tahansa. Käytä aina **luettelon solmu agentti SKU** menetelmiä erä API luettelo ja valitse sitten yhteensopiva virtuaalikoneen ja solmu agentti tuotteissa, kun suoritat erätyöt.

| **Publisherin** | **Tarjouksen** | **Kuva SKU** | **Versio** | **Solmun agentti SKU tunnus** |
| ------- | ------- | ------- | ------- | ------- |
| Kanoninen | UbuntuServer | 14.04.0-LTS | uusimman | Batch.node.Ubuntu 14.04 |
| Kanoninen | UbuntuServer | 14.04.1-LTS | uusimman | Batch.node.Ubuntu 14.04 |
| Kanoninen | UbuntuServer | 14.04.2-LTS | uusimman | Batch.node.Ubuntu 14.04 |
| Kanoninen | UbuntuServer | 14.04.3-LTS | uusimman | Batch.node.Ubuntu 14.04 |
| Kanoninen | UbuntuServer | 14.04.4-LTS | uusimman | Batch.node.Ubuntu 14.04 |
| Kanoninen | UbuntuServer | 14.04.5-LTS | uusimman | Batch.node.Ubuntu 14.04 |
| Kanoninen | UbuntuServer | 16.04.0-LTS | uusimman | Batch.node.Ubuntu 16.04 |
| Credativ | Debian | 8 | uusimman | Batch.node.debian 8 |
| OpenLogic | CentOS | 7.0 | uusimman | Batch.node.centos 7 |
| OpenLogic | CentOS | 7.1 | uusimman | Batch.node.centos 7 |
| OpenLogic | CentOS HPC | 7.1 | uusimman | Batch.node.centos 7 |
| OpenLogic | CentOS | 7.2 | uusimman | Batch.node.centos 7 |
| Oracle | Oracle-Linux | 7.0 | uusimman | Batch.node.centos 7 |
| SUSE | openSUSE | 13.2 | uusimman | Batch.node.OpenSuSE 13.2 |
| SUSE | openSUSE harppaus | 42.1 | uusimman | Batch.node.OpenSuSE 42.1 |
| SUSE | SLES HPC | 12 | uusimman | Batch.node.OpenSuSE 42.1 |
| SUSE | SLES | 12 SP1 | uusimman | Batch.node.OpenSuSE 42.1 |
| Microsoft-mainokset | Vakio-tiedot-tiede-AM | Vakio-tiedot-tiede-AM | uusimman | Batch.node.Windows amd64 |
| Microsoft-mainokset | Linux-tiedot-tiede-AM | linuxdsvm | uusimman | Batch.node.centos 7 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2-SP1 | uusimman | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012 Palvelinkeskukseen | uusimman | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012 R2-Palvelinkeskukseen | uusimman | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Windows-palvelin-Technical-esikatselu | uusimman | Batch.node.Windows amd64 |

## <a name="connect-to-linux-nodes"></a>Linux solmujen yhdistäminen

Kehityksen aikana tai vianmäärityksen aikana voit saatat huomata tarpeen kirjautua lisääminen resurssivarantoon solmujen. Toisin kuin Windowsin Laske solmujen et voi käyttää Remote Desktop Protocol (RDP) Linux solmujen yhdistäminen. Sen sijaan erä-palvelun avulla SSH käyttöön kunkin solmun remote-yhteyden.

Python koodikatkelman Luo käyttäjän resurssivarantoon, joka vaaditaan etäyhteyden kunkin solmun. Se tulostaa kunkin solmun suojattu runko (SSH)-yhteyden tietoja.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Tässä on esimerkki tulosteen, joka sisältää neljä Linux solmujen resurssivarantoon edellisen koodi:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Huomaa, että sen sijaan, että salasana, voit määrittää julkisen SSH-avaimen luodessasi käyttäjän solmu. Python SDK: ssa, tämä on valmis käyttämällä **ssh_public_key** -parametrin [ComputeNodeUser][py_computenodeuser]. .NET-: Tämä on valmis käyttämällä [ComputeNodeUser][net_computenodeuser]. [SshPublicKey] [net_ssh_key] ominaisuus.

## <a name="pricing"></a>Hinnat

Azure erä on muodostettu Azure Pilvipalveluiden ja Azuren näennäiskoneiden tekniikka. Erän palvelun itse tarjotaan maksutta, mikä tarkoittaa, että vain Laske resursseille perittävän erä ratkaisujen tarjoaman. Kun valitset **Cloud Services-kokoonpanon määritys**, voit veloitetaan mukaan [pilvipalveluihin hinnat] [ cloud_services_pricing] rakenne. Valitessasi **Virtuaalikoneen määritysten**voit veloitetaan mukaan [näennäiskoneiden hinnat] [ vm_pricing] rakenne.

## <a name="next-steps"></a>Seuraavat vaiheet

### <a name="batch-python-tutorial"></a>Erän Python opetusohjelma

Tietoja käsittelemisestä erä käyttämällä Python tarkemmin opetusohjelmaan Tutustu [Azure erä Python asiakkaan käytön aloittaminen](batch-python-tutorial.md). Sen avustaja- [koodin otoksen] [ github_samples_pyclient] on avustaja-funktion `get_vm_config_for_distro`, joka näyttää toisen menetelmällä hakea virtuaalikoneen määritykset.

### <a name="batch-python-code-samples"></a>Erän Python MALLIKOODEJA

Kuittaa ulos muiden [Python koodi näytteiden] [ github_samples_py] [azure-erä-objektit] [ github_samples] useita komentosarjoja, joita noudattamalla voit suorittaa yleisiä erä kuten resurssivarantoon, projektin tai tehtävän luominen-GitHub säilö. [Lueminut] [ github_py_readme] , mukana Python näytteiden on lisätietoja Asenna tarvittavat paketit.

### <a name="batch-forum"></a>Erän keskustelupalsta

[Azure erä keskustelupalstalla] [ forum] MSDN-sivuston on kätevä keskustella Siirtoerän ja esittää kysymyksiä tietoja. Hyödyllisiä "stickied" viestien lukeminen ja kirjaa kysymyksiisi, kun ne aiheutuvat aikana voit luoda erää ratkaisuja.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
