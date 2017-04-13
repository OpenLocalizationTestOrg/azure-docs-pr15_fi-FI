<properties
   pageTitle="Miten voit luoda NSGs Azure resurssien hallinta PowerShellin avulla | Microsoft Azure"
   description="Lue, miten voit luoda ja ottaa käyttöön NSGs Azure resurssien hallinta PowerShellin avulla"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-in-resource-manager-by-using-powershell"></a>Miten voit luoda NSGs resurssien hallinta PowerShellin avulla

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään resurssien hallinnan käyttöönottomalli. Voit myös [luoda NSGs perinteinen käyttöönotto-mallissa](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Esimerkki PowerShell-komennot alla toiminta yksinkertainen ympäristössä, jossa on jo luotu perusteella yllä skenaario. Jos haluat suorittaa komentoja, niin kuin ne näkyvät tässä asiakirjassa-ensin Muodosta testiympäristössä ottamalla [tätä mallia](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), valitse **käyttöönotto Azure**, korvaa parametrien oletusarvot tarvittaessa ja portaalin ohjeiden avulla.

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Edusta-aliverkon NSG luominen
Voit luoda NSG nimeltä yllä skenaarion perusteella *NSG edusta* -noudattamalla seuraavia ohjeita:

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Jos et ole aikaisemmin käyttänyt PowerShellin Azure-artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) ja noudata ohjeita päässä Azure Kirjaudu ja valitse tilauksen.

2. Käytön salliminen Internetistä portti 3389 suojauksen säännön luominen

        $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name rdp-rule -Description "Allow RDP"
            -Access Allow -Protocol Tcp -Direction Inbound -Priority 100
            -SourceAddressPrefix Internet -SourcePortRange *
            -DestinationAddressPrefix * -DestinationPortRange 3389

3. Sallitaan Internetistä porttia 80 suojauksen säännön luominen

        $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name web-rule -Description "Allow HTTP"
            -Access Allow -Protocol Tcp -Direction Inbound -Priority 101
            -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix *
            -DestinationPortRange 80

4. Lisää uusi NSG, **NSG edusta**-niminen edellä luotu säännöt.

        $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName TestRG -Location westus
        -Name "NSG-FrontEnd" -SecurityRules $rule1,$rule2

5. Tarkista, NSG luotuja sääntöjä.

        $nsg

    Tulos näyttää vain suojaussäännöt:

        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule",
                                   "Description": "Allow RDP",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "3389",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 100,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 },
                                 {
                                   "Name": "web-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule",
                                   "Description": "Allow HTTP",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "80",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 101,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

6. Liitä edellä *edusta* -aliverkon luotu NSG.

                    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
                    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
                        -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $nsg

                Output showing only the *FrontEnd* subnet settings, notice the value for the **NetworkSecurityGroup** property:

                    Subnets           : [
                                          {
                                            "Name": "FrontEnd",
                                            "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                            "AddressPrefix": "192.168.1.0/24",
                                            "IpConfigurations": [
                                              {
                                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                              },
                                              {
                                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                              }
                                            ],
                                            "NetworkSecurityGroup": {
                                              "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                            },
                                            "RouteTable": null,
                                            "ProvisioningState": "Succeeded"
                                          }

    >[AZURE.WARNING] Yllä olevien komentojen tulos näyttää VPN kokoonpano-objekti, joka on käytettävissä vain tietokoneeseen, jossa PowerShell sisällön. Sinun on suoritettava `Set-AzureRmVirtualNetwork` cmdlet-komento, Tallenna asetukset Azure.

7. Tallenna uudet asetukset tulevat VNet Azure.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Tulos näyttää NSG osaan:

        "NetworkSecurityGroup": {
          "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
        }

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Uudelleen aliverkon NSG luominen
Voit luoda NSG nimeltä *NSG Taustajärjestelmä* yllä skenaarion perusteella, noudattamalla seuraavia ohjeita:

1. Sallitaan edusta-aliverkosta porttia 1433 (SQL Serverin käyttämä oletusportti) suojaussäännön luominen

        $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name frontend-rule -Description "Allow FE subnet"
            -Access Allow -Protocol Tcp -Direction Inbound -Priority 100
            -SourceAddressPrefix 192.168.1.0/24 -SourcePortRange *
            -DestinationAddressPrefix * -DestinationPortRange 1433

2. Luo suojauksen sääntö estäminen Internet-yhteys.

        $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name web-rule -Description "Block Internet"
            -Access Deny -Protocol * -Direction Outbound -Priority 200
            -SourceAddressPrefix * -SourcePortRange *
            -DestinationAddressPrefix Internet -DestinationPortRange *

3. Lisää uusi NSG, nimeltä **NSG Taustajärjestelmä**edellä luotu säännöt.

        $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName TestRG -Location westus -Name "NSG-BackEnd"
            -SecurityRules $rule1,$rule2

4. Liitä edellä *Taustajärjestelmä* aliverkon luotu NSG.

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd
            -AddressPrefix 192.168.2.0/24 -NetworkSecurityGroup $nsg

    Siirtää *taustatietokannan* aliverkon auki, Huomaa **NetworkSecurityGroup** -ominaisuuden arvon:

        Subnets           : [
                      {
                        "Name": "BackEnd",
                        "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                        "AddressPrefix": "192.168.2.0/24",
                        "IpConfigurations": [...],
                        "NetworkSecurityGroup": {
                          "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "RouteTable": null,
                        "ProvisioningState": "Succeeded"
                      }

5. Tallenna uudet asetukset tulevat VNet Azure.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet


## <a name="how-to-remove-an-nsg"></a>Voit poistaa NSG

Jos haluat poistaa aiemmin NSG, eli *NSG edusta* noudattamalla tässä tapauksessa vaiheen avulla:

Suorita **Poista AzureRmNetworkSecurityGroup** alla ja muista lisätä NSG on resurssiryhmä.

            Remove-AzureRmNetworkSecurityGroup -Name "NSG-FrontEnd" -ResourceGroupName "TestRG"
