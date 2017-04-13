<properties
    pageTitle="Hallitse palvelun Bus PowerShellin | Microsoft Azure"
    description="Hallitse palvelun Bus PowerShell-komentosarjojen kanssa"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="sethm"/>

# <a name="manage-service-bus-with-powershell"></a>Hallitse palvelun Bus PowerShellin avulla

## <a name="overview"></a>Yleiskatsaus

Microsoft Azure PowerShell on komentosarjat ympäristössä, joiden avulla voit hallita ja automatisoida käyttöönotto- ja Azure oman työmääriä hallinnointi. Tässä artikkelissa käsitellään valmistelu ja palvelun Bus kohteiden, kuten nimitilan olevien tai tapahtuman keskittimet paikallisen PowerShellin Azure-konsolin avulla hallinta PowerShellin avulla.

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavat edellytykset:

- Azure tilaus. Azure on tilauspohjaista ympäristö. Lisätietoja hankkiminen tilauksen näkyy [Hankkiminen asetukset][], [Jäsenen tarjoaa][]tai [Maksuttoman kokeiluversion käyttäjäksi][].

- Tietokone, jossa PowerShellin Azure. Katso ohjeet [asentaminen ja määrittäminen PowerShellin Azure][].

- Yleisiä tietoja PowerShell-komentosarjojen NuGet pakettien ja .NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Myös viittauksen palvelun Bus luotaessa

Rajoitettu määrä PowerShellin cmdlet-komennot ovat käytettävissä palvelun Bus hallinta. Valmistelu sellaisia kohteita, eikä niitä julkaista kautta aiemmin Cmdlet-komentoja, voit käyttää palvelun Bus [palvelun Bus NuGet paketti][].NET-asiakas.

Varmista ensin, että komentosarja Etsi **Microsoft.ServiceBus.dll** -kokoonpanoa, joka on asennettu NuGet-paketti. Joustava ollakseni komentosarja suorittaa seuraavasti:

1. Määrittää polku, johon se on kutsuttu.
2. Traverses polku, kunnes se löytää ‑kansioon `packages`. Tämä kansio luodaan, kun asennat NuGet paketit.
3. Rekursiivisesti haut `packages` kansio nimeltä **Microsoft.ServiceBus.dll**kokoonpanon.
4. Viittaa kokoonpanon, jotta tiedostotyypit ovat käytettävissä myöhempää käyttöä varten.

Näin, kuinka nämä vaiheet suoritetaan PowerShell-komentosarjaa:

```
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error "Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script."
}
```

## <a name="provision-a-service-bus-namespace"></a>Säännöstä palvelun Bus nimitila

Kaksi PowerShellin cmdlet-komennot tue palvelun Bus nimitilan toimintoja. .NET SDK-ohjelmointirajapinnan sijaan voit käyttää [Get-AzureSBNamespace][] ja [Uusi AzureSBNamespace][].

Tässä esimerkissä luodaan muutaman paikalliset muuttujat komentosarjan; `$Namespace` and `$Location`.

- `$Namespace`on nimi, jonka haluamme toimimaan palvelun Bus nimitilan.
- `$Location`näet, jossa komentosarja valmistelee nimitilan tietokeskuksen.
- `$CurrentNamespace`tallentaa viittaus nimitilan, joka komentosarja hakee (tai luo).

Todellinen komentosarja- `$Namespace` ja `$Location` voi välittää parametreja.

Tämän osan komentosarjan suorittaa seuraavia tehtäviä:

1. Yritetään noutaa palvelun Bus nimitilan, jonka määritettyä nimeä.
2. Jos nimitila löydy, se ilmoittaa löytämän.
3. Jos nimitila ei löydy, se luo nimitilan ja noutaa juuri luomasi nimitilan.

    ```
    $Namespace = "MyServiceBusNS"
    $Location = "West US"
    
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
    
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```

Valmistelu palvelun Bus muiden kohteiden luominen SDK [NamespaceManager][] luokan esiintymä.
Voit [Saada AzureSBAuthorizationRule][] cmdlet-komento todennus-sääntö, jota käytetään antaa yhteysmerkkijonon noutamiseen. Viittaus tallennetaan `NamespaceManager` esiintymä `$NamespaceManager` muuttuja. Käytämme `$NamespaceManager` jäljempänä komentosarja valmistelu muihin kohteisiin.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-other-service-bus-entities"></a>Palvelun Bus muihin kohteisiin valmistelu

Valmistella muihin kohteisiin, kuten olevien, aiheet ja tapahtuman keskittimet, jotta voit käyttää [.NET Ohjelmointirajapinta palvelun Bus][]. Tässä artikkelissa keskitytään vain tapahtuman keskittimet, mutta muiden kohteiden vaiheet ovat samanlaisia. Lisää esimerkkejä on muihin kohteisiin, mukaan lukien viitataan lisäksi tämän artikkelin lopussa.

Tässä osassa komentosarja luo neljä Lisää paikalliset muuttujat. Muuttujia voi käyttää esiintymää `EventHubDescription` objekti. Komentosarja suorittaa seuraavia tehtäviä:

1. Käyttämällä `NamespaceManager` objektia, tarkista, jos tapahtuma-toiminnossa merkittyä `$Path` on olemassa.
2. Jos sitä ei ole, luo `EventHubDescription` ja siirtää sitä `NamespaceManager` luokan `CreateEventHubIfNotExists` menetelmää.
3. Kun valitset tapahtuma-toiminto on käytettävissä, Luo kuluttaja ryhmän käyttäen `ConsumerGroupDescription` ja `NamespaceManager`.

    ```
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
        
    # Check to see if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }
        
    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

## <a name="migrate-a-namespace-to-another-azure-subscription"></a>Nimitilan siirtäminen toiseen Azure-tilaukseen

Seuraavia komentoja järjestyksessä siirtää nimitilan Azure tilauksesta toiseen. Voit suorittaa tämän toiminnon nimitila tulee olla jo active ja PowerShell-komentojen käyttäjän on oltava järjestelmänvalvoja lähde- ja kohdesivustojen tilaukset.

```
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa annettujen perusrakenteen valmisteluun palvelun Bus kohteiden PowerShellin avulla. Mitään, joita voit tehdä .NET asiakas-kirjastoja, myös mahdollisuuksista PowerShell-komentosarjaa.

Ei lisää esimerkkejä käytettävissä nämä blogimerkinnät:

- [Palvelun Bus olevien, aiheet ja tilauksista PowerShell-komentosarjaa luominen](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Palvelun Bus-Namespace ja käyttämällä PowerShell-komentosarjaa tapahtumaa-toiminnossa luominen](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Jotkin valmiita komentosarjat ovat myös ladattavissa:
- [Palvelun Bus PowerShell-komentosarjojen](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[Osta asetukset]: http://azure.microsoft.com/pricing/purchase-options/
[Jäsenen tarjouksia]: http://azure.microsoft.com/pricing/member-offers/
[Maksuton kokeiluversio]: http://azure.microsoft.com/pricing/free-trial/
[Asenna ja määritä PowerShellin Azure]: ../powershell-install-configure.md
[Palvelun Bus NuGet paketti]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Hae AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Uusi AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Hae AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[Palvelun Bus .NET Ohjelmointirajapinta]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.aspx
[NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
