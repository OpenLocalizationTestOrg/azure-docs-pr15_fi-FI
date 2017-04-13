<properties
   pageTitle="Skaalaa ACS-klusterin ja Azure-CLI | Microsoft Azure"
   description="Miten käyttämällä Azure-CLI Azure säilö palvelun yhteyttä klusterin."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, säilöt-mikro-palveluja, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="timlt"/>

# <a name="scale-an-azure-container-service"></a>Skaalaa Azure säilö-palvelu

Voit skaalata ulos Azure säilö Service (ACS) on Azure CLI-työkalulla solmujen määrän. Kun käytät Azure-CLI skaalata, työkalu palauttaa uusi kokoonpanotiedosto edustava säilö käytetty muuta.

## <a name="about-the-command"></a>Tietoja-komennosta

Azure-CLI on oltava Azure Resurssienhallinta-tilassa, voit käsitellä Azure säilöjen. Voit liikkua Resurssienhallinta tilaan soittamalla `azure config mode arm`. `acs` Komento on ali-komennon nimi `scale` tekee kaikki säilön palvelun asteikko-toiminnot. Saat ohjeita eri parametrit, joita käytetään asteikko-komennon suorittamalla `azure acs scale --help`, joka siirtää jotakin tämän tapaista:

```azurecli
azure acs scale --help

help:    The operation to scale a container service.
help:
help:    Usage: acs scale [options] <resource-group> <name> <new-agent-count>
help:
help:    Options:
help:      -h, --help                               output usage information
help:      -v, --verbose                            use verbose output
help:      -vv                                      more verbose with debug output
help:      --json                                   use json output
help:      -g, --resource-group <resource-group>    resource-group
help:      -n, --name <name>                        name
help:      -o, --new-agent-count <new-agent-count>  New agent count
help:      -s, --subscription <subscription>        The subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="use-the-command-to-scale"></a>Skaalaa-komennon avulla

Skaalata säilö palvelu, sinun on tietää **resurssiryhmä** ja **Azure säilö Service (ACS) nimi**ja määritä myös uusi määrää agenttien vuoksi. Käyttämällä summa on pienempi tai sitä uudemmassa versiossa voit skaalata alaspäin tai ylöspäin tarpeen mukaan.

Haluat ehkä tietää mitä nykyisen agenttien säilö palvelu-määrää ennen kokoa. Käytä `azure acs show <resource group> <ACS name>` komento palauttaa ACS config. Huomautus <mark>Laske</mark> tulos.

#### <a name="see-current-count"></a>Katso nykyinen määrä

```azurecli
azure acs show containers-test containerservice-containers-test

info:    Executing command acs show
data:
data:     Id                 : /subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test
data:     Name               : containerservice-containers-test
data:     Type               : Microsoft.ContainerService/ContainerServices
data:     Location           : westus
data:     ProvisioningState  : Succeeded
data:     OrchestratorProfile
data:       OrchestratorType : DCOS
data:     MasterProfile
data:       Count            : 1
data:       DnsPrefix        : myprefixmgmt
data:       Fqdn             : myprefixmgmt.westus.cloudapp.azure.com
data:     AgentPoolProfiles
data:       #0
data:         Name           : agentpools
data:         <mark>Count          : 1</mark>
data:         VmSize         : Standard_D2
data:         DnsPrefix      : myprefixagents
data:         Fqdn           : myprefixagents.westus.cloudapp.azure.com
data:     LinuxProfile
data:       AdminUsername    : azureuser
data:       Ssh
data:         PublicKeys
data:           #0
data:             KeyData    : ssh-rsa <ENCODED VALUE>
data:     DiagnosticsProfile
data:       VmDiagnostics
data:         Enabled        : true
data:         StorageUri     : https://<storageid>.blob.core.windows.net/
```  

#### <a name="scale-to-new-count"></a>Skaalaa uusi määrä

Se on todennäköisesti veroetuja, skaalata säilö-palvelun soittamalla `azure acs scale` ja **resurssiryhmä**, **ACS nimen**ja **agentti määrä**. Kun säilö palvelu skaalata, Azure CLI palauttaa JSON merkkijonon säilö-palvelua, mukaan lukien uudet agentti Laske uudet määritykset.

```azurecli
azure acs scale containers-test containerservice-containers-test 10

info:    Executing command acs scale
data:    {
data:        id: '/subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test',
data:        name: 'containerservice-containers-test',
data:        type: 'Microsoft.ContainerService/ContainerServices',
data:        location: 'westus',
data:        provisioningState: 'Succeeded',
data:        orchestratorProfile: { orchestratorType: 'DCOS' },
data:        masterProfile: {
data:            count: 1,
data:            dnsPrefix: 'myprefixmgmt',
data:            fqdn: 'myprefixmgmt.westus.cloudapp.azure.com'
data:        },
data:        agentPoolProfiles: [
data:            {
data:                name: 'agentpools',
data:                <mark>count: 10</mark>,
data:                vmSize: 'Standard_D2',
data:                dnsPrefix: 'myprefixagents',
data:                fqdn: 'myprefixagents.westus.cloudapp.azure.com'
data:            }
data:        ],
data:        linuxProfile: {
data:            adminUsername: 'azureuser',
data:            ssh: {
data:                publicKeys: [
data:                    { keyData: 'ssh-rsa <ENCODED VALUE>' }
data:                ]
data:            }
data:        },
data:        diagnosticsProfile: {
data:            vmDiagnostics: { enabled: true, storageUri: 'https://<storageid>.blob.core.windows.net/' }
data:        }
data:    }
info:    acs scale command OK
``` 

## <a name="next-steps"></a>Seuraavat vaiheet

- [Klusterin käyttöönotto](container-service-deployment.md)