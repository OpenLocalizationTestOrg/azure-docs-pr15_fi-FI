<properties
    pageTitle="Sovelluksen yhdistäminen virtual verkon PowerShell-toiminnolla"
    description="Ohjeita siitä, miten voit muodostaa yhteyden ja käyttää virtual verkkojen PowerShell-toiminnolla"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="wpickett"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="ccompy"/>

# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>Sovelluksen yhdistäminen virtual verkon PowerShell-toiminnolla #

## <a name="overview"></a>Yleiskatsaus ##

Azure-sovelluksen palvelussa voit muodostaa tilauksen sovelluksen (web, matkapuhelin tai API) Azure virtual verkkoon (VNet). Tätä ominaisuutta kutsutaan VNet integrointi. VNet Integration-toimintoa ei sekoittaa App palvelun ympäristön-toiminto, jonka avulla voit suorittaa Azure App palvelun esiintymän virtual verkossa.

VNet integrointi-ominaisuus on käyttäjän käyttöliittymän uudessa portaalissa, joiden avulla voit integroida virtual verkkoja, jotka on otettu käyttöön perinteinen käyttöönotto-mallin tai Azure resurssien hallinnan käyttöönottomalli. Jos haluat lisätietoja ominaisuus, katso [liittää sovelluksen Azure virtual verkoston kanssa](web-sites-integrate-with-vnet.md).

Tässä artikkelissa on ei Käyttöliittymän artikkelista tietoja, mutta mieluummin ottamisesta käyttöön integrointi PowerShell-toiminnolla. Koska käyttöönoton mallien komennot ovat eri, tässä artikkelissa on kunkin käyttöönoton mallin osa.  

Ennen kuin jatkat tämän artikkelin, varmista, että sinulla on:

- Uusimmat Azure PowerShell SDK asennettuna. Voit asentaa tämä WWW-ympäristö asennusohjelma kanssa.
- Sovelluksen Azure App palvelu käynnissä Standard tai Premium-tuote.

## <a name="classic-virtual-networks"></a>Perinteinen virtual verkot ##

Tässä osassa kerrotaan virtual verkkojen, jotka käyttävät perinteinen käyttöönoton mallin kolme tehtävää:

1. Yhdistä sovelluksen aiemmin luotuja virtual verkkoon, jossa on yhdyskäytävän ja on määritetty pisteen sivuston yhteyksiä varten.
1. Päivitä VPN integrointi tiedot, kun sovellus.
1. Katkaise sovelluksen virtual verkossa.

### <a name="connect-an-app-to-a-classic-vnet"></a>Sovelluksen yhdistäminen perinteinen VNet ###

Voit muodostaa sovelluksen virtual verkkoon, nämä kolme seuraavasti:

1. Määritellä web App-sovellukseen, että se voi liittyä tietyn virtual verkon. Sovelluksen Luo sertifikaatti, joka annetaan virtual verkkoon pisteen sivuston yhteyksiä varten.
1. Lataa web app-sertifikaatin virtual verkon ja hakea pisteen sivuston VPN-paketin URI.
1. Päivitä web Appin VPN-yhteyden pisteen sivuston paketin URI.

Ensimmäisen ja kolmannen vaiheet ovat täysin scriptable, mutta toinen vaihe vaatii kertaluonteista, Manuaalinen toiminto portal tai access **SIJOITTAA** tai **KORJAUSTIEDOSTON** toimintojen suorittamiseen VPN Azure Resurssienhallinta päätepisteen kautta. Jos haluat, että tämä asetus on käytössä Azure tuelta. Ennen kuin aloitat, varmista, että sinulla on perinteinen virtual verkossa, jossa on kohdassa sivuston yhteyden jo käytössä ja otettu käyttöön yhdyskäytävän. Voit luoda yhdyskäytävän ja ottaa pisteen sivuston yhteyden, sinun on käytettävä portaalin ohjeiden etsiminen [luominen VPN-yhdyskäytävän][createvpngateway].

Perinteinen virtual verkko on oltava sovelluksen palvelun suunnitelma, joka sisältää sovellus, joka on integroitu saman tilauksen.

##### <a name="set-up-azure-powershell-sdk"></a>Azure PowerShell SDK määrittäminen #####

Avaa PowerShell-ikkuna ja Määritä Azure-tili ja tilauksen avulla:

    Login-AzureRmAccount

Tämä komento Avaa Azure tunnistetiedot saat kehotteen. Kun olet kirjautunut sisään, valitse tilaus, jota haluat käyttää jommallakummalla seuraavista komennoista. Varmista, että käytät tilauksesta, joka VPN ja-sovelluksen palvelusopimus.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

tai

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>Tässä artikkelissa käytetyt muuttujat #####

Helpottaa komennot on määrittää **$Configuration** PowerShell muuttujan kokoonpano kanssa.

Määritä muuttujan seuraavasti powershellissä seuraavilla parametreilla:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

Sovelluksen sijainti on oltava sijainti ilman välilyöntejä. Esimerkiksi Länsi US on westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

Seuraavan kohteen on varmenteen kirjoituskohtaa. Kirjoitettava polku paikallisessa tietokoneessa on oltava. Muista lisätä .cer lopussa.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Jos haluat nähdä, voit määrittää, kirjoita **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

Tässä osassa muiden oletetaan, että vain kuvatulla luotu muuttuja.

##### <a name="declare-the-virtual-network-to-the-app"></a>Määritellä virtual verkko-sovellukseen #####

Käytä seuraavaa komentoa, onko sovellus, että se käyttää tiettyä virtual verkoston. Tämä aiheuttaa sovelluksen luo tarvittavat varmenteita:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Jos tämä komento on muodostettu, **$vnet** pitäisi olla **Ominaisuudet** muuttujaan ei. **Ominaisuudet** -muuttuja pitäisi sisältää varmenteen allekirjoitusta ja varmennetta tiedot.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>Lataa web app-sertifikaatin virtual verkkoon #####

Manuaalinen, erikseen vaihe on tarvittavat kunkin tilauksen ja VPN-yhdistelmä. Toisin sanoen tilauksen A-sovellusten yrität muodostaa yhteyden, VPN-a, sinun on suoritettava tässä vaiheessa vain, kun määrität riippumatta siitä, kuinka monta sovellukset. Jos haluat lisätä uuden sovelluksen toiseen virtual verkkoon, ne on toiminto uudelleen. Syy on, että varmenteet joukko luodaan tilauksen tasolla Azure App palvelu ja määritä luodaan kerran kunkin virtual verkon, joka muodostaa sovellukset.

Varmenteet on jo määritetty, jos olet tehnyt nämä vaiheet, tai jos käyttämällä portaalin integroitu samassa virtual verkossa.

Ensimmäiseksi on luomiseen .cer-tiedosto. Toinen vaihe on .cer-tiedoston lataaminen virtual verkossa. Luomiseen .cer-tiedosto vaiheessa API-kutsu suorittamalla seuraavat komennot.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Varmenteen löytyvät sijainnissa, **$Configuration.GeneratedCertificatePath** määrittää.

Lataa sertifikaatti manuaalisesti, käytä [Azure portal] [ azureportal] ja **Selaa Virtual Network (perinteinen)** > **VPN-yhteydet** > **pisteen sivuston** > **hallinta varmenteet**. Tässä kohdassa Lataa sertifikaatti.

##### <a name="get-the-point-to-site-package"></a>Hae pisteen sivuston-paketti #####

Seuraava vaihe verkkosovellukseen VPN-yhteyden määrittäminen on hakee pisteen sivuston paketin ja anna web App-sovellukseen.

Tallenna tiedosto nimeltä GetNetworkPackageUri.json toiseen tietokoneeseen, esimerkiksi C:\Azure\Templates\GetNetworkPackageUri.json seuraavaa mallia.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


Määritä syöteparametrit:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Soita komentosarja:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


Muuttujan **$output. Outputs.packageUri** nyt sisältää paketin URI-tunnus annetaan web App-sovellukseen.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>Sovelluksen pisteen sivuston paketin lataaminen #####

Viimeinen vaihe on sovelluksen pakkauksessa. Suorita seuraava komento:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Jos viesti pyytää sinua vahvistamaan, että korvaat aiemman resurssin, varmista, että sallit laajennuksen käytön.

Kun tämä komento on muodostettu, sovelluksen olisi muodostettu virtual verkkoon. Vahvistamaan success Siirry app konsoliin ja kirjoita seuraava komento:

    SET WEBSITE_

Jos näkyvissä on nimeltään WEBSITE_VNETNAME, jossa on arvo, joka vastaa kohde virtual verkon nimeä ympäristömuuttuja, kaikki määritykset on onnistui.

### <a name="update-classic-vnet-integration-information"></a>Perinteinen VNet integrointi tietojen päivittäminen ###

Voit päivittää tai synkronoi tietoja, toista vaiheet, jotka olet toiminut luodessasi integrointi ensiksi. Nämä vaiheet ovat:

1. Määritä määritystiedot.
1. Määritellä virtual verkko-sovellukseen.
1. Pyydä pisteen sivuston paketin.
1. Pisteen sivuston paketin lataaminen sovelluksen.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Sovelluksen katkaista perinteinen VNet ###

Voit katkaista sovellus, sinun on kokoonpanotietoja, joka on määritetty VPN-integroinnin yhteydessä. Tietojen käyttäminen on sitten yksi-komento sovelluksen katkaiseminen virtual verkossa.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Resurssienhallinta virtual verkot ##

Resurssienhallinta virtual verkostoilla on Azure Resurssienhallinta API, joka yksinkertaistaa jotkin prosessit perinteinen virtual verkkojen verrattuna. On komentosarja, jonka avulla voit suorittaa seuraavia tehtäviä:

- Luo Resurssienhallinta virtual verkko ja sovelluksen integroida.
- Luoda yhdyskäytävän määrittäminen pisteen sivuston yhteyden aiemmin luotuja Resurssienhallinta virtual verkossa ja sitten integroi ohjeartikkelista se.
- Integroi ohjeartikkelista aiemmin luotuja Resurssienhallinta virtual verkossa, jossa yhdyskäytävän ja pisteen ja sivuston yhteyden käytössä.
- Katkaise sovelluksen virtual verkossa.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Resurssienhallinta VNet sovelluksen palvelun integrointi komentosarja ###

Kopioi seuraava komentosarja ja tallenna se tiedostoon. Jos et halua käyttää komentosarja, vapaasti tietoja siitä, miten voit määrittää asetuksia Resurssienhallinta virtual verkossa.


    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
          else
        {
          Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

Tallenna kopio komentosarja. Tässä artikkelissa, jonka nimi on V2VnetAllinOne.ps1, mutta voit käyttää toisella nimellä. Ei ole argumentteja tämän komentosarjan. Riittää, että se suoritetaan. Komentosarjan hyötyä ensimmäiseksi kehote, sinun on kirjauduttava. Kun olet kirjautunut sisään, komentosarja saa tilin tiedot ja palauttaa luettelon tilauksista. Laskeminen ei tunnistetietoja pyynnön, alkuperäinen komentosarjojen suorittamisen näyttää seuraavanlaiselta:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Tilauksen esittely (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) MS-testi (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Tilauksen Violetti esittely (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Valitse haluamasi vaihtoehto: 3

    Tili: ccompy@microsoft.com ympäristössä: AzureCloud tilaus: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d vuokraajan: 722278f fef1-499f-91ab-2323d011db47

    Kirjoita sovelluksesi resurssiryhmä: hcdemo rg Kirjoita sovelluksesi nimi: v2vnetpowershell mitä haluat tehdä?

    1) Lisää uusi Virtual verkko-ohjelmaan
    2) Lisää Virtual aiemmin LUOTUUN-verkko-ohjelmaan
    3) Sovelluksen Virtual verkon poistaminen

Muiden tässä osassa kuvataan kolme asetuksia.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Luo Resurssienhallinta VNet ja sen integrointi ###

Voit luoda uuden virtual verkkoon, joka käyttää resurssien hallinnan käyttöönottomalli ja integroida sovelluksen valitsemalla **1) uuden Virtual verkon lisääminen sovelluksen**. Tämä kehottaa sinua virtual verkon nimi. Kuten näet seuraavat asetukset Omat tapauksessa käytetty nimi v2pshell.

Komentosarja tarkempia tietoja, jotka luodaan virtual-sivustosta. Jos halua, voit muuttaa arvoja. Tässä esimerkissä suorittamisen luotu virtual verkossa, jossa on seuraavat asetukset:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Jos haluat muuttaa mitään arvoja, kirjoita **Y** ja tee haluamasi muutokset. Kun olet tyytyväinen VPN-asetukset, kirjoita **N** tai painamalla Enter pyydettäessä asetusten muuttamisesta. Sieltä henkilökohtaisesta komentosarja selviää joitakin sen, mitä se ' tekemistä, kunnes se käynnistyy VPN-yhdyskäytävän luominen uudelleen. Vaiheen voi kestää jopa tunnissa. Tässä vaiheessa ei ole tilanneilmaisin, mutta komentosarja ilmoittaa, kun yhdyskäytävä on luotu.

Kun komentosarja on suoritettu, se lukee **Valmis**. Tässä vaiheessa sinun on Resurssienhallinta virtual verkossa, jossa on haluamasi nimi ja valitsit asetukset. Uusi virtual verkoston myös integroitu sovelluksen.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Integroi ohjeartikkelista aiemmin luotuja Resurssienhallinta-VNet ###

Kun olet integraation aiemmin luotuja virtual verkkoon, jos annat Resurssienhallinta virtual verkkoon, joka ei ole yhdyskäytävän tai pisteen sivuston connectivity-komentosarja määrittäminen,. Jos VNET on jo näitä asioita määrittäminen, komentosarja yhdistetään suoraan app integrointi. Aloita, riittää, että Valitse **2) olemassa olevan Virtual verkoston lisääminen sovelluksen**.

Tämä asetus toimii vain, jos sinulla on aiemmin luotuja Resurssienhallinta virtual, joka on saman tilauksen sovelluksen. Kun olet valinnut haluamasi vaihtoehto, voit valita jommankumman Resurssienhallinta virtual verkostojen luettelolla.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Valitse haluamasi vaihtoehto: 5

Valitse vain virtual verkko, jonka haluat yhdistää. Jos sinulla on jo yhdyskäytävään, joka on kohdassa sivuston yhteys on käytössä, komentosarja integroida sovelluksen yksinkertaisesti virtual verkossa. Jos sinulla ei ole yhdyskäytävän, sinun on määrittää yhdyskäytävän aliverkon. Yhdyskäytävä-osoitteiden on oltava VPN-osoitetilaa varten ja sitä ei voi olla muiden aliverkon. Jos olet virtual verkossa ilman yhdyskäytävän ja suorita tämä vaihe, asioita näyttää tältä:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

Tässä esimerkissä luotu VPN yhdyskäytävään, joka on seuraavat asetukset:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

Jos haluat muuttaa mitä tahansa näistä asetuksista, voit tehdä näin. Paina Enter, ja komentosarja luo käyttämäsi yhdyskäytävän ja liitä sovelluksen virtual verkkoon. Yhdyskäytävän luontiaikaa on edelleen tunti, vaikka, joten varmista, pidä mielessä. Kun kaikki on valmis, komentosarja lukee **Valmis**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Sovelluksen katkaista Resurssienhallinta VNet ###

Sovelluksen katkaiseminen virtual verkon ei viedä yhdyskäytävän tai pisteen sivuston yhteyden käytöstä. Voit, kun kaikki käyttämiesi se jotain muuta. Se myös ei irrota se muista sovelluksista muuhun kuin antamasi osaan. Jos haluat suorittaa tämän toiminnon, valitse **3) Virtual Network poistaminen sovelluksen**. Kun teet näin, näet seuraavankaltaiselta:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Komentosarja lukee Poista, vaikka se ei poista virtual verkkoon. Se on vain poistetaan integrointi. Varmista, että tämä on mitä haluat tehdä, kun komento käsitellään aivan nopeasti ja ilmoittaa, **Tosi,** kun se on valmis.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
