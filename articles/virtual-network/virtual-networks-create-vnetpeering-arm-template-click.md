<properties
   pageTitle="Luo VNet Peering Resurssienhallinta mallien avulla | Microsoft Azure"
   description="Opettele luomaan VPN-peering mallit resurssien hallinnan avulla."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-vnet-peering-using-resource-manager-templates"></a>Luo VNet Peering Resurssienhallinta mallien käyttäminen

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Jos haluat luoda VNet, peering Resurssienhallinta mallien avulla, noudata seuraavia ohjeita:

1. Jos et ole aikaisemmin käyttänyt PowerShellin Azure-artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) ja noudata ohjeita päässä Azure Kirjaudu ja valitse tilauksen.

    > [AZURE.NOTE] PowerShell cmdlet-komennon hallintaan VNet peering toimitetaan [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Seuraava teksti näkyy VNet peering linkin määrittelyn VNet1 VNet2, yllä skenaarion perusteella. Kopioi alla ja tallenna se VNetPeeringVNet1.json-tiedostoon.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. Aikatiedot näyttää VNet peering linkin määrittelyn VNet2 VNet1, yllä skenaarion perusteella.  Kopioi alla ja tallenna se VNetPeeringVNet2.json-tiedostoon.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    Yllä mallin tarkastelu liittyy muutama määritettäviä ominaisuuksia VNet peering:

  	|Vaihtoehto|Kuvaus|Oletusarvo|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Onko vertaisjärjestelmä VNet osoitetilaa on virtual_network tunnisteen mukaan.|Kyllä|
  	|AllowForwardedTraffic|Onko liikenne ei peräisin peered VNet on hyväksytty tai kohteet poistetaan.|Ei|
  	|AllowGatewayTransit|Sallii peer VNet käyttämään VNet-yhdyskäytävä.|Ei|
  	|UseRemoteGateways|Käytä omaa peer VNet yhdyskäytävää. Vertaisjärjestelmä VNet on määritetty yhdyskäytävän ja AllowGatewayTransit valittuna. Et voi käyttää tämä vaihtoehto, jos sinulla on määritetty yhdyskäytävä.|Ei|

    Kunkin linkkiä VNet peering on yllä ominaisuuksia. Voit esimerkiksi määrittää AllowVirtualNetworkAccess tosi VNet peering linkin VNet1 VNet2 ja määritä sen arvoksi False VNet peering linkin toiseen suuntaan.


4. Mallitiedoston ottamaan voit suorittaa uusi AzureRmResourceGroupDeployment cmdlet-komennolla voit luoda tai päivittää käyttöönotto. Lisätietoja Resurssienhallinta mallien avulla Katso tässä [artikkelissa](../resource-group-template-deploy.md).

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] Korvaa ryhmän nimi ja mallin resurssitiedoston tarpeen mukaan.

    Alla on esimerkki perustuu yllä skenaariota:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    Tulos näyttää:

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    Tulos näyttää:

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. Kun asennus on valmis, voit käyttää cmdlet-komennon alla olevista peering tila:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Tulos näyttää:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Kun tässä skenaariossa peering on muodostettu, sinun pitäisi aloittaa virtual minkä tahansa tietokoneesta yhteyden virtual-konetta sekä VNets. Oletusarvon mukaan AllowVirtualNetworkAccess on TOSI ja VNet peering käytön ERISNIMI käyttöoikeusluettelot sallimaan välisen VNets valmistelu. Voit edelleen käyttää verkon ryhmän (NSG) suojaussäännöt estää tietyn aliverkosta tai näennäiskoneiden hieno syyt hallitsemaan access kaksi virtual verkkojen välillä väliset yhteydet.  Saat lisätietoja NSG sääntöjen luomisesta tutustu tämän [artikkelin](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Jos haluat luoda VNet, peering-tilauksissa, noudata seuraavia ohjeita:

1. Kirjaudu Azure luottamuksellisten käyttäjän A on tili tilauksen a ja suorita seuraava cmdlet-komento:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

    Tämä ei ole tarpeen, peering voi muodostaa, vaikka käyttäjien Nosta yksitellen peering pyynnöt niiden vastaaviin Vnets, kunhan pyynnöt vastaa. Muut VNet sellaisten käyttäjä lisäämisellä käyttäjät paikalliseen VNet Tee asennus on helpompaa.

2. Kirjaudu Azure sellaisten käyttäjien-B tilillä tilauksen b ja suorita seuraavat cmdlet-komento:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. -Käyttäjän A käyttäjän kirjautuminen istunnon cmdlet suoritetaan:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    Näin miten JSON-tiedosto on määritetty.  

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. Käyttäjä-B kirjautuminen istunnossa ajamalla seuraavan cmdlet-komennon:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

    Näin miten JSON-tiedosto on määritetty:

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

    Kun tässä skenaariossa peering on muodostettu, sinun pitäisi aloittaa virtual minkä tahansa tietokoneesta yhteydet virtual minkä tahansa tietokoneeseen, sekä VNets eri-tilauksissa.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Tässä skenaariossa voit ottaa käyttöön alla muodostaa VNet peering malli-malli.  Sinun on määritettävä AllowForwardedTraffic-ominaisuuden arvoksi True, joka sallii verkon virtual laitteen peered VNet lähettämiseen ja vastaanottamiseen liikenne.

    Seuraavassa on mallin, kun luot VNet, peering-HubVNet VNet1. Huomaa, että AllowForwardedTraffic on määritetty epätosi.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. Seuraavassa on mallin, kun luot VNet, peering-VNet1 HubVnet. Huomaa, että AllowForwardedTraffic on määritetty tosi.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Kun peering on muodostettu, voit viitata tämän [artikkelin](virtual-network-create-udr-arm-ps.md) Määritä käyttäjän määrittämä routes(UDR) uudelleenohjaaminen VNet1 liikenne kautta virtual laitteen, voit käyttää sen ominaisuuksia. Kun määrität reitin seuraava osoite, voit määrittää sen peer VNet HubVNet virtual laitteen IP-osoitteeseen.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Jos haluat luoda peering virtual verkkojen eri käyttöönoton mallien välillä, noudata seuraavia ohjeita:
1. Alla olevassa teksti näkyy VNet peering linkin määrittelyn VNET1 VNET2 tässä skenaariossa. Vain yksi linkki tarvitaan vertaiskoneeseen perinteinen virtual verkon Azure resurssien hallinnan virtual verkkoon.

    Muista laittaa tilauksen-tunnus, jossa perinteinen virtual verkko- tai VNET2 sijaitsee ja muuta MyResouceGroup sopivaa resurssia ryhmän nimeä.

    {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "parametrit": {}, "muuttujat": {}, "resurssit": [{"apiVersion": "2016-06-01", "tyyppi": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings", "nimi": "VNET1/LinkToVNET2", "sijainti": "[resourceGroup () .location]", "ominaisuudet": {"allowVirtualNetworkAccess": arvo on TOSI, "allowForwardedTraffic": epätosi "allowGatewayTransit": arvo on EPÄTOSI, "useRemoteGateways": arvo on EPÄTOSI, "remoteVirtualNetwork": {"tunnus": "[resourceId (' Microsoft.ClassicNetwork/virtualNetworks,"VNET2")]"}}}]}

2. Ottaa käyttöön mallitiedoston luominen tai päivittäminen käyttöönoton seuraavan cmdlet-komennon suorittaminen

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. Kun asennus on valmis, voit suorittaa seuraavan cmdlet-komennon voit tarkastella peering tila:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Kun peering on muodostettu perinteinen VNet ja Resurssienhallinta VNet välillä, sinun pitäisi aloittaa yhteydet VNET1 virtual minkä tahansa tietokoneesta virtual koneen VNET2 ja päinvastoin.
