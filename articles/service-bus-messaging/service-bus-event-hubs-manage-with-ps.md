<properties
    pageTitle="Palvelun Bus ja tapahtuman keskittimet resurssien hallinta PowerShellin avulla | Microsoft Azure"
    description="PowerShellin avulla voit luoda ja hallita palvelun Bus ja tapahtuman keskittimet resurssit"
    services="service-bus,event-hubs"
    documentationCenter=".NET"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="use-powershell-to-manage-service-bus-and-event-hubs-resources"></a>Palvelun Bus ja tapahtuman keskittimet resurssien hallinta PowerShellin avulla

Microsoft Azure PowerShell on komentosarjat ympäristössä, joiden avulla voit hallita ja automatisoida käyttöönotto- ja Azure palveluiden hallinta. Tässä artikkelissa käsitellään valmistelu ja palvelun Bus kohteiden, kuten nimitilan olevien tai tapahtuman keskittimet paikallisen PowerShellin Azure-konsolin avulla hallinta PowerShellin avulla.

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat, sinun on seuraavasti:

- Azure tilaus. Azure on tilauspohjaista ympäristö. Lisätietoja hankkiminen tilauksen näkyy [ostaa asetukset][], [jäsenen tarjoaa][]tai [ilmainen tili][].

- Tietokone, jossa PowerShellin Azure. Katso ohjeet [asentaminen ja määrittäminen PowerShellin Azure][].

- Yleisiä tietoja PowerShell-komentosarjojen NuGet pakettien ja .NET Framework.

## <a name="include-a-reference-to-the-net-assembly-for-service-bus"></a>Liittää viittauksen palvelun Bus luotaessa

Rajoitettu määrä PowerShellin cmdlet-komennot ovat käytettävissä palvelun Bus hallinta. Valmistelu sellaisia kohteita, eikä niitä julkaista kautta aiemmin Cmdlet-komentoja, voit käyttää .NET-asiakasohjelman palvelun Bus PowerShell sisällä, viittaamalla [palvelun Bus NuGet paketti].

Varmista ensin, että komentosarja Etsi **Microsoft.ServiceBus.dll** -kokoonpanoa, joka on asennettu NuGet-paketti. Joustava ollakseni komentosarja suorittaa seuraavasti:

1. Määrittää polku, josta se on kutsuttu.
2. Traverses polku, kunnes se löytää ‑kansioon `packages`. Tämä kansio luodaan, kun asennat NuGet paketit.
3. Rekursiivisesti haut `packages` kansio nimeltä **Microsoft.ServiceBus.dll**kokoonpanon.
4. Viittaa kokoonpanon, jotta tiedostotyypit ovat käytettävissä myöhempää käyttöä varten.

Näin, kuinka nämä vaiheet suoritetaan PowerShell-komentosarjaa:

```powershell

try
{
    # IMPORTANT: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}

```

## <a name="provision-a-service-bus-namespace"></a>Säännöstä palvelun Bus nimitila

Kun käsittelet palvelun Bus nimitilan, on kaksi cmdlet-komennot, voit käyttää sen sijaan, että .NET SDK: [Get-AzureSBNamespace][] ja [Uusi AzureSBNamespace][].

Tässä esimerkissä luodaan muutaman paikalliset muuttujat komentosarjan; `$Namespace` and `$Location`.

- `$Namespace`nimi jonka haluamme toimimaan palvelun Bus nimitilan.
- `$Location`tunnistaa tietokeskuksen, johon se on valmistella nimitilan.
- `$CurrentNamespace`tallentaa viittaus nimitilan, joka on noutaa (tai luo).

Todellinen komentosarja- `$Namespace` ja `$Location` voi välittää parametreja.

Tässä osassa komentosarja tekee seuraavat toimet:

1. Yritetään noutaa palvelun Bus nimitilan, jonka määritettyä nimeä.
2. Jos nimitila löydy, se ilmoittaa löytämän.
3. Jos nimitila ei löydy, se luo nimitilan ja noutaa juuri luomasi nimitilan.

    ``` powershell

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
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```
Valmistelu palvelun Bus muihin kohteisiin, luoda esiintymää `NamespaceManager` SDK objektin. Voit [Saada AzureSBAuthorizationRule][] cmdlet-komento todennus-sääntö, jota käytetään antaa yhteysmerkkijonon noutamiseen. Tässä esimerkissä tallentaa viittaus `NamespaceManager` esiintymä `$NamespaceManager` muuttuja. Komentosarja myöhemmin käyttää `$NamespaceManager` valmistelu muihin kohteisiin.

    ``` powershell
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    # Create the NamespaceManager object to create the Event Hub
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
    ```

## <a name="provisioning-other-service-bus-entities"></a>Palvelun Bus muihin kohteisiin valmistelu

Valmistelu muihin kohteisiin, kuten olevien, aiheet ja tapahtuman keskittimet, voit käyttää [.NET Ohjelmointirajapinta palvelun Bus][]. Lisää esimerkkejä, mukaan lukien muihin kohteisiin, joihin viitataan tämän artikkelin lopussa.

### <a name="create-an-event-hub"></a>Luo tapahtumaa-toiminnossa

Tässä osassa komentosarja luo neljä Lisää paikalliset muuttujat. Muuttujia voi käyttää esiintymää `EventHubDescription` objekti. Komentosarja tekee seuraavat toimet:

1. Käyttämällä `NamespaceManager` objektia, tarkista, jos tapahtuma-toiminnossa merkittyä `$Path` on olemassa.
2. Jos sitä ei ole, luo `EventHubDescription` ja siirtää sitä `NamespaceManager` luokan `CreateEventHubIfNotExists` menetelmää.
3. Kun valitset tapahtuma-toiminto on käytettävissä, Luo kuluttaja ryhmän käyttäen `ConsumerGroupDescription` ja `NamespaceManager`.

    ``` powershell

    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"

    # Check if the Event Hub already exists
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

### <a name="create-a-queue"></a>Luo jono

Jonossa tai aiheen luominen suorittaa nimitilan-valintaruudun, kuten edellisessä osassa.

    if ($NamespaceManager.QueueExists($Path))
    {
        Write-Output "The [$Path] queue already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] queue in the [$Namespace] namespace..."
        $QueueDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.QueueDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $QueueDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $QueueDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $QueueDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $QueueDescription.EnableBatchedOperations = $EnableBatchedOperations
        $QueueDescription.EnableDeadLetteringOnMessageExpiration = $EnableDeadLetteringOnMessageExpiration
        $QueueDescription.EnableExpress = $EnableExpress
        $QueueDescription.EnablePartitioning = $EnablePartitioning
        $QueueDescription.ForwardDeadLetteredMessagesTo = $ForwardDeadLetteredMessagesTo
        $QueueDescription.ForwardTo = $ForwardTo
        $QueueDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        if ($LockDuration -gt 0)
        {
            $QueueDescription.LockDuration = [System.TimeSpan]::FromSeconds($LockDuration)
        }
        $QueueDescription.MaxDeliveryCount = $MaxDeliveryCount
        $QueueDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $QueueDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        $QueueDescription.RequiresSession = $RequiresSession
        if ($EnablePartitioning)
        {
            $QueueDescription.SupportOrdering = $False
        }
        else
        {
            $QueueDescription.SupportOrdering = $SupportOrdering
        }
        $QueueDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateQueue($QueueDescription);
    }

### <a name="create-a-topic"></a>Aiheen luominen

    if ($NamespaceManager.TopicExists($Path))
    {
        Write-Output "The [$Path] topic already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] topic in the [$Namespace] namespace..."
        $TopicDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.TopicDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $TopicDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $TopicDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $TopicDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $TopicDescription.EnableBatchedOperations = $EnableBatchedOperations
        $TopicDescription.EnableExpress = $EnableExpress
        $TopicDescription.EnableFilteringMessagesBeforePublishing = $EnableFilteringMessagesBeforePublishing
        $TopicDescription.EnablePartitioning = $EnablePartitioning
        $TopicDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        $TopicDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $TopicDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        if ($EnablePartitioning)
        {
            $TopicDescription.SupportOrdering = $False
        }
        else
        {
            $TopicDescription.SupportOrdering = $SupportOrdering
        }
        $TopicDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateTopic($TopicDescription);
    }

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa edellyttäen basic työnkulkua valmisteluun palvelun Bus kohteiden PowerShellin avulla. Vaikka rajoitettu määrä PowerShellin cmdlet-komennot ovat käytettävissä hallinta palvelun Bus messaging viittaamalla Microsoft.ServiceBus.dll kokoonpanossa, lähes minkä tahansa toiminnon kohteita, voit suorittaa voit myös tehdä PowerShell-komentosarjaa .NET asiakas-kirjastojen avulla.

Käytettävissä nämä blogimerkinnät on Lisää esimerkkejä:

- [Palvelun Bus olevien, aiheet ja tilauksista PowerShell-komentosarjaa luominen](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Palvelun Bus-Namespace ja käyttämällä PowerShell-komentosarjaa tapahtumaa-toiminnossa luominen](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Jotkin valmiita komentosarjat ovat myös ladattavissa:

- [Palvelun Bus PowerShell-komentosarjojen](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[Osta asetukset]: http://azure.microsoft.com/pricing/purchase-options/
[jäsenen tarjoaa]: http://azure.microsoft.com/pricing/member-offers/
[maksuton-tili]: http://azure.microsoft.com/pricing/free-trial/
[Palvelun Bus NuGet paketti]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Hae AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Uusi AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Hae AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[Palvelun Bus .NET Ohjelmointirajapinta]: https://msdn.microsoft.com/en-us/library/azure/mt419900.aspx
[Asenna ja määritä PowerShellin Azure]: ../powershell-install-configure.md
