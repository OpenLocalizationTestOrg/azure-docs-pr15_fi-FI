<properties
    pageTitle="Hallinta PowerShellin Azure CDN | Microsoft Azure"
    description="Opettele Azure PowerShell cmdlet-komentojen avulla voit hallita Azure CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="casoper"/>


# <a name="manage-azure-cdn-with-powershell"></a>PowerShellin Azure CDN hallinta

PowerShellin on eniten joustavia tavoilla Azure CDN-profiileista ja päätepisteet.  Voit tehdä PowerShell vuorovaikutteisesti tai kirjoittamalla komentosarjoja hallintatehtävien automatisointiin.  Tässä opetusohjelmassa esitellään useita yleisimpiä tehtäviä suoritettavat PowerShellin Azure CDN-profiileista ja päätepisteet.

## <a name="prerequisites"></a>Edellytykset

PowerShellin avulla voit hallita Azure CDN-profiileista ja päätepisteet, sinulla on oltava asennettuna PowerShellin Azure-moduuli.  Lisätietoja PowerShellin Azure asentaminen ja yhteyden muodostaminen Azure avulla `Login-AzureRmAccount` cmdlet-komento, katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).

>[AZURE.IMPORTANT] Sinun on kirjauduttava sisään `Login-AzureRmAccount` ennen kuin voit suorittaa Azure PowerShellin cmdlet-komennot.

## <a name="listing-the-azure-cdn-cmdlets"></a>Luettelo Azure CDN cmdlet-komennot

Voit näyttää kaikki käyttämällä Azure CDN cmdlet-komennot `Get-Command` cmdlet-komento.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Ohjeiden saaminen

Näiden cmdlet-komentojen käyttäminen avulla voi saada apua `Get-Help` cmdlet-komento.  `Get-Help`käytön ja syntaksi ja halutessasi näyttää esimerkkejä.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Luettelo aiemmin Azure CDN-profiileista

`Get-AzureRmCdnProfile` Cmdlet-komento ilman parametreja hakee kaikki olemassa olevat CDN profiileja.

```powershell
Get-AzureRmCdnProfile
```

Tämä tulos voi olla piped luettelointi cmdlet-komennot.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

Voit palata yhden profiilin myös määrittämällä profiilin nimi ja resurssien ryhmä.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

>[AZURE.TIP] On mahdollista, että CDN profiileja, jolla on sama nimi kuin ne ovat eri ryhmissä.  Puuttuu `ResourceGroupName` -parametri palauttaa kaikki profiilit vastaavaa nimeä.

## <a name="listing-existing-cdn-endpoints"></a>Luettelo aiemmin CDN päätepisteet

`Get-AzureRmCdnEndpoint`Voit hakea yksittäisen päätepisteen tai profiilin päätepisteet.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>CDN-profiileista ja päätepisteet luominen

`New-AzureRmCdnProfile`ja `New-AzureRmCdnEndpoint` CDN-profiileista ja päätepisteet luomiseen käytetyistä.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Päätepisteen nimi käytettävyyden tarkistaminen

`Get-AzureRmCdnEndpointNameAvailability`Palauttaa objektin, joka ilmaisee päätepisteen nimi on käytettävissä.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Mukautetun toimialueen lisääminen

`New-AzureRmCdnCustomDomain`Lisää aiemmin luotu päätepiste mukautettua toimialuenimeä.

>[AZURE.IMPORTANT] CNAME-TIETUE on määritettävä DNS-palvelussa kuvatulla tavalla [yhdistämisestä sisällön lähettämisen (CDN) päätepisteen mukautettu toimialue](./cdn-map-content-to-custom-domain.md).  Voit testata ennen muokkaamista päätepisteen käyttämällä yhdistämistä `Test-AzureRmCdnCustomDomain`.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Päätepisteen muokkaaminen

`Set-AzureRmCdnEndpoint`Muokkaa aiemmin luotu päätepiste.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Poistaminen ja ennakkoversion-loading CDN resurssit

`Unpublish-AzureRmCdnEndpointContent`poistaa välimuistiin kohteiden, kun `Publish-AzureRmCdnEndpointContent` Lataa valmiiksi kalusto-tuettujen päätepisteet.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Aloitetaan/pysäyttäminen CDN päätepisteet
`Start-AzureRmCdnEndpoint`ja `Stop-AzureRmCdnEndpoint` avulla voidaan aloittaa ja lopettaa yksittäisiä päätepisteet tai päätepisteet ryhmät.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>CDN resurssien poistaminen

`Remove-AzureRmCdnProfile`ja `Remove-AzureRmCdnEndpoint` avulla voidaan poistaa profiilit ja päätepisteet.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Seuraavat vaiheet

Lue, miten voit automatisoida Azure CDN [.NET](./cdn-app-dev-net.md) tai [Node.js](./cdn-app-dev-node.md).

Lisätietoja CDN ominaisuuksista on artikkelissa [CDN yleiskatsaus](./cdn-overview.md).


