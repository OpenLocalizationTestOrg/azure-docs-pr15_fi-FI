<properties 
    pageTitle="Voit luoda ja hallita Azure Redis välimuistin Azure komentorivivalitsimet käyttöliittymässä (Azure CLI) | Microsoft Azure" 
    description="Katso asennusohjeet Azure-CLI missä tahansa ympäristössä, miten sitä käytetään yhteyden muodostamiseen Azure-tili ja voit luoda ja hallita Redis.txt välimuistin Azure-CLI." 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>Voit luoda ja hallita Azure Redis välimuistin Azure komentorivivalitsimet käyttöliittymässä (Azure CLI)

> [AZURE.SELECTOR]
- [PowerShellin](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

Azure-CLI on erinomainen tapa hallita Azure infrastruktuurin tahansa ympäristössä. Tässä artikkelissa kerrotaan, miten voit luoda ja hallita käyttämällä Azure-CLI Azure Redis välimuistin kopioita.

## <a name="prerequisites"></a>Edellytykset

Voit luoda ja hallita Azure CLI esiintymiä Azure Redis välimuistiin, on tehtävä seuraavat toimet.

-   Sinulla on oltava Azure-tili. Jos sinulla ei ole, voit luoda [ilmaisen tilin](https://azure.microsoft.com/pricing/free-trial/) vain hetkeksi.
-   [Asenna Azure CLI](../xplat-cli-install.md).
-   Liitä Azure CLI asennuksen omalla Azure-tilillä tai työpaikan tai oppilaitoksen Azure-tili ja kirjaudu sisään Azure CLI käyttämästä `azure login` komento. Niiden erot ja osaat valita, artikkelissa [Yhdistä Azure Azure komentorivin Interface (Azure CLI)-tilaukseen](../xplat-cli-connect.md).
-   Ennen kuin suoritat jokin seuraavista komennoista, siirry Azure-CLI Resurssienhallinta tilaan suorittamalla `azure config mode arm` komento. Lisätietoja on kohdassa [Azure Resurssienhallinta-tilassa](../xplat-cli-azure-resource-manager.md#set-the-azure-resource-manager-mode).

## <a name="redis-cache-properties"></a>Redis välimuistin ominaisuudet

Seuraavat ominaisuudet käytetään, kun luominen ja päivittäminen Redis välimuistin esiintymät.

| Ominaisuus            | Vaihda                                  | Kuvaus                                                                                                                                                                                                                                          |
|---------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nimi                | -n,--nimi                              | Redis.txt välimuistin nimi.                                                                                                                                                                                                                             |
| resurssiryhmä      | -g,--resurssiryhmä                    | Resurssiryhmän nimi.                                                                                                                                                                                                                          |
| sijainti            | -l,--sijainti                          | Voit luoda välimuistin sijainti.                                                                                                                                                                                                                            |
| kokoa                | + z tai--kokoa                              | Redis.txt välimuistin kokoa. Kelvollisia arvoja: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]                                                                                                                                                                  |
| tuote                 | -x---sku                               | Redis tuote. On oltava jokin seuraavista: [Basic, Vakio, Premium]                                                                                                                                                                                             |
| EnableNonSslPort    | -e, – ota käyttöön-ei-ssl-portti               | EnableNonSslPort Redis välimuistin ominaisuus. Lisää tämän merkinnän, jos haluat ottaa käyttöön välimuistin ei SSL-portti                                                                                                                                    |
| Redis määritys | -c,--Redis.txt-määritys               | Redis määritys. Kirjoita muotoiltu JSON merkkijonon määritysten avaimet ja arvot tähän. Muoto: "{" ":""," ":" "}"                                                                                                                                     |
| Redis määritys | -f,--Redis.txt--kokoonpanotiedosto          | Redis määritys. Kirjoita määritysten näppäimet ja tähän arvot sisältävän tiedoston polku. Tiedoston tapahtuma muoto: {"": "","": ""}                                                                                                                |
| Shard määrä         | -r,--shard määrä                       | Luo Premium-klusterin välimuistin asentaminen Shards määrä.                                                                                                                                                                               |
| VPN     | -v,--virtual verkko                   | Kun isännöinnin VNET-välimuistin määrittää tarkan ARM Resurssitunnus virtual verkoston käyttöön Redis.txt välimuisti. Esimerkin muoto: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| avaimen tyyppi            | -t,--avaimen tyyppi                          | Uusi avain tyyppi. Kelvollisia arvoja: [ensisijainen, toissijainen]                                                                                                                                                                                             |
| StaticIP            | -p, – staattinen ip-< staattinen ip >             | Kun isännöinnin VNET-välimuistin määrittää välimuistin aliverkon yksilöllinen IP-osoite. Jos ei anneta, yksi valitaan aliverkosta puolestasi.                                                                                                |
| Aliverkon              | t,--aliverkon<subnet>                    | Kun isännöinnin VNET-välimuistin määrittää nimen, johon ottamaan välimuistin aliverkon.                                                                                                                                                    |
| VirtualNetwork      | -v,--virtual verkon < virtual-verkko > | Kun isännöinnin VNET-välimuistin määrittää tarkan ARM Resurssitunnus virtual verkoston käyttöön Redis.txt välimuisti. Esimerkin muoto: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Tilauksen        | -s,--tilaus                      | Tilauksen tunnus.                                                                                                                                                                                                                         |

## <a name="see-all-redis-cache-commands"></a>Näet kaikki Redis välimuisti-komennot

Jos haluat nähdä kaikki Redis välimuistin komentojen ja niiden parametrien, käytä `azure rediscache -h` komento.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Luoda Redis.txt välimuistin

Voit luoda Redis välimuistin käyttämällä seuraava komento:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Lisätietoja komennon suorittaminen `azure rediscache create -h` komento.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Poista aiemmin Redis välimuisti

Poista Redis välimuistin, käytä seuraavaa komentoa:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Lisätietoja komennon suorittaminen `azure rediscache delete -h` komento.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Luettele kaikki Redis tallentaa tilauksen tai resurssiryhmä

Luettelon kaikki Redis tallentaa tilauksen tai resurssiryhmä, käytä seuraavaa komentoa:

    azure rediscache list [options]

Lisätietoja komennon suorittaminen `azure rediscache list -h` komento.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Näytä aiemmin Redis välimuistin ominaisuudet

Olemassa olevan Redis välimuistin ominaisuudet näkyviin seuraavalla komennolla:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Lisätietoja komennon suorittaminen `azure rediscache show -h` komento.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>
## <a name="change-settings-of-an-existing-redis-cache"></a>Olemassa olevan Redis välimuistin asetusten muuttaminen

Jos haluat muuttaa aiemmin Redis välimuistin asetukset, käytä seuraavaa komentoa:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Lisätietoja komennon suorittaminen `azure rediscache set -h` komento.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>Todennusavain uudistaa aiemmin Redis välimuisti

Uusi aiemmin Redis välimuistin todennus-näppäintä, käytä seuraavaa komentoa:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Määritä `Primary` tai `Secondary` varten `key-type`.

Lisätietoja komennon suorittaminen `azure rediscache renew-key -h` komento.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Olemassa olevan Redis välimuistin luettelon ensisijainen ja toissijainen näppäimet

Luettelon ensisijainen ja toissijainen avaimet aiemmin Redis välimuistin Käytä seuraavaa komentoa:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Lisätietoja komennon suorittaminen `azure rediscache list-keys -h` komento.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
