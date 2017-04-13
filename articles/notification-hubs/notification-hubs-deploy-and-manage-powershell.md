<properties 
    pageTitle="Ottaa käyttöön ja hallita ilmoituksen keskittimet PowerShellin avulla" 
    description="Voit luoda ja hallita ilmoituksen keskittimet automatisointiin PowerShellin avulla" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Ottaa käyttöön ja hallita ilmoituksen keskittimet PowerShellin avulla

##<a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kerrotaan Luo ja hallitse Azure ilmoituksen keskittimet PowerShellin käyttäminen. Yleiset automaatio-toiminnot näkyvät tässä aiheessa.

+ Luo ilmoitus-toiminnossa
+ Määritä tunnistetiedot

Jos haluat luoda uuden bus nimitilan ilmoituksen keskittimien myös, katso [Palvelun Bus hallinta PowerShellin avulla](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Ilmoitusten keskittimet hallinta ei tue suoraan kuuluva Azure PowerShell cmdlet-komentoja. PowerShell-paras vaihtoehto on viittaamaan Microsoft.Azure.NotificationHubs.dll kokoonpanon. Kokoonpano on jaettu [Microsoft Azure ilmoituksen keskittimet NuGet paketti](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).


## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- Azure tilaus. Azure on tilauspohjaista ympäristö. Lisätietoja hankkiminen tilauksen näkyy [Hankkiminen asetukset], [Jäsenen tarjoaa]tai [Maksuttoman kokeiluversion käyttäjäksi].

- Tietokone, jossa PowerShellin Azure. Katso ohjeet [asentaminen ja määrittäminen PowerShellin Azure].

- Yleisiä tietoja PowerShell-komentosarjojen NuGet pakettien ja .NET Framework.


## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Myös viittauksen palvelun Bus luotaessa

Azure ilmoituksen keskittimet hallinta ei ole vielä mukana PowerShellin Azure PowerShell cmdlet-komennoista. Valmistelu ilmoituksen keskittimet, voit käyttää [Microsoft Azure ilmoituksen keskittimet NuGet paketin](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)säädetty .NET-asiakas.

Varmista ensin, että komentosarjan voit etsiä **Microsoft.Azure.NotificationHubs.dll** kokoamiseen, johon on asennettu Visual Studio projektin NuGet pakettina. Joustava ollakseni komentosarja suorittaa seuraavasti:

1. Määrittää polku, johon se on kutsuttu.
2. Traverses polku, kunnes se löytää ‑kansioon `packages`. Tämä kansio luodaan, kun asennat NuGet pakettien Visual Studio projektien.
3. Rekursiivisesti haut `packages` kansio nimeltä **Microsoft.Azure.NotificationHubs.dll**kokoonpanon.
4. Viittaa kokoonpanon, jotta tiedostotyypit ovat käytettävissä myöhempää käyttöä varten.

Näin, kuinka nämä vaiheet suoritetaan PowerShell-komentosarjaa:

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>NamespaceManager-luokan luominen

Valmistelu ilmoituksen keskittimet luominen SDK [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) luokan esiintymä. 

Voit käyttää todennus-sääntö, jota käytetään antaa yhteysmerkkijonon noutamiseen PowerShellin Azure mukana [Get-AzureSBAuthorizationRule] cmdlet-komento. Viittaus tallennetaan `NamespaceManager` esiintymä `$NamespaceManager` muuttuja. Käytämme `$NamespaceManager` valmistelu ilmoitus-toiminnossa.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Uusi ilmoitus-toiminnossa valmistelu 

Valmistelu uusi ilmoitus-keskittimeen, käytä [.NET API ilmoituksen keskittimien].

Tässä osassa komentosarjan voit määrittää neljä paikalliset muuttujat. 

1. `$Namespace`: Määritä nimitilan nimen kohtaa, johon haluat luoda ilmoituksen-toiminnossa.
2. `$Path`: Määritä tämä polku uusi ilmoitus-toiminnossa nimi.  Esimerkiksi "MyHub".    
3. `$WnsPackageSid`: Määrittää tämän paketin SUOJAUSTUNNUS puolestasi Windows-sovellus [Windows keskihajonta Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Asetukseksi salausavaimen puolestasi Windows-sovellus [Windows keskihajonta keskelle](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Muuttujia käytetään muodostaa yhteyttä nimitilan ja luo uusi ilmoitus-toiminnossa määritetty käsittelemään for Windows-sovellus Windows ilmoituksen Services (WNS) ilmoitukset WNS tunnuksilla. Tietojen hankkiminen paketin SUOJAUSTUNNUS ja salausavaimen-kohdassa, [Ilmoitus keskittimet käytön aloittaminen](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) -opetusohjelma. 

+ Komentosarjan koodikatkelman käyttää `NamespaceManager` objektin Tarkista, jos ilmoitus-toiminnossa merkittyä `$Path` on olemassa.

+ Jos sitä ei ole, Luo komentosarja `NotificationHubDescription` kanssa WNS tunnistetiedot, ja siirtää sitä `NamespaceManager` luokan `CreateNotificationHub` menetelmää.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Lisäresursseja

- [Hallitse palvelun Bus PowerShellin avulla](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [Palvelun Bus olevien, aiheet ja tilauksista PowerShell-komentosarjaa luominen](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Palvelun Bus-Namespace ja käyttämällä PowerShell-komentosarjaa tapahtumaa-toiminnossa luominen](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Jotkin valmiita komentosarjat ovat myös ladattavissa:
- [Palvelun Bus PowerShell-komentosarjojen](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)
 

[Osta asetukset]: http://azure.microsoft.com/pricing/purchase-options/
[Jäsenen tarjouksia]: http://azure.microsoft.com/pricing/member-offers/
[Maksuton kokeiluversio]: http://azure.microsoft.com/pricing/free-trial/
[Asenna ja määritä PowerShellin Azure]: ../powershell-install-configure.md
[.NET-Ohjelmointirajapinnan ilmoituksen keskittimien]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Hae AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
 
