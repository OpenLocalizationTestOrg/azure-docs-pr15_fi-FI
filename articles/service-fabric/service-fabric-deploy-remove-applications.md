<properties
   pageTitle="Palvelun kangasta sovelluksen käyttöönottoa | Microsoft Azure"
   description="Ottaa käyttöön ja poistaminen sovellusten palvelun kangasta"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="deploy-and-remove-applications-using-powershell"></a>Ottaa käyttöön ja poista sovellukset PowerShellin avulla

> [AZURE.SELECTOR]
- [PowerShellin](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Kerran [sovelluksen tyyppi on pakattu][10], se on valmis käyttöönoton Azure palvelun kangasta-klusterin. Käyttöönoton on kolme vaihetta:

1. Sovelluksen-paketin lataaminen
2. Rekisteröi sovelluksen tyyppi
3. Sovelluksen luonti

>[AZURE.NOTE] Jos käytät Visual Studio käyttöönotto ja virheenkorjaus paikallista kehittämistä klusterin-sovelluksia, kaikki seuraavat toimet käsitellään automaattisesti kautta PowerShell-komentosarja-sovelluksen projektin komentosarjat-kansiossa. Tässä artikkelissa on taustan mitä komentosarjoja tekemässä niin, että voit suorittaa samat toiminnot Visual Studio ulkopuolella.

## <a name="upload-the-application-package"></a>Sovelluksen-paketin lataaminen

Ladata sovelluspaketin siirtää sen sisäisiä palvelun kangasta osia jäsenten käytettävissä olevaan sijaintiin. PowerShellin avulla voit suorittaa lataus. Ennen kuin olet suorittanut tämän artikkelin kaikki PowerShell-komennot aina alkaa palvelun kangasta klusterin muodostaa [Yhteyden ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) avulla.

Oletetaan, että *MyApplicationType* -nimiseen kansioon, joka sisältää tarvittavat sovellusluettelo, palvelun luettelot ja koodin/hakumääritystietojen paketit. [Kopioi ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) -komento lataa paketin klusterin kuva-kaupasta. **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet-komento, joka on osa palvelun kangasta SDK PowerShell-moduulin, käytetään kuva kaupan tietoyhteyden yhteysmerkkijonon.  Voit tuoda SDK-moduulissa, suorittamalla:

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Voit kopioida sovelluspaketin *C:\users\ryanwi\Documents\Visual Studio 2015\Projects\MyApplication\myapplication\pkg\debug* *c:\temp\MyApplicationType* (nimeä "MyApplicationType" "virheenkorjaus-kansio). Seuraavassa esimerkissä latauksia paketin:

~~~
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
  	|   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───en-us
  	|         Microsoft.ServiceFabric.Internal.Strings.resources.dll
  	|         System.Fabric.Strings.resources.dll
  	|
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
~~~

## <a name="register-the-application-package"></a>Rekisteröi sovelluspaketin

Rekisteröiminen sovelluspaketin tekee sovelluksen tyyppi ja versio ilmoitettu käytettävissä sovellusluettelo käyttöä varten. Järjestelmä lukee paketti on ladattu edellisessä vaiheessa, tarkista (vastaa käytössä [Testi ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx) paikallisesti) paketin, käsitellä pakkauksesta ja kopioi käsitellyt paketin sisäinen sijaintiin.

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

[Rekisteröi ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125958.aspx) -komento palauttaa vain, kun järjestelmä on kopioitu onnistuneesti sovelluspaketin. Kuinka kauan tämä kestää määräytyy sovelluspaketin sisällön. Tarvittaessa **- TimeoutSec** parametrin voidaan antaa pidempi aikakatkaisu. (Aikakatkaisun oletusarvo on 60 sekuntia).

[Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) -komento näyttää kaikki onnistuneesti rekisteröidyn sovelluksen tyyppi-versiot.

## <a name="create-the-application"></a>Sovelluksen luominen

Voit vahvistaa sovelluksen käyttämällä sovelluksen tyyppi-versio, joka on rekisteröity onnistuneesti [Uusi ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) -komennolla. Kunkin sovelluksen nimen alussa on oltava *kangasta:* menetelmä ja sovelluksen jokaiselle esiintymälle yksilölliseksi. Tällä hetkellä luodaan oletusarvon kohdetyyppi-sovellus sovellusluettelo määritetyt palvelut.

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

[Hae ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) -komento näyttää kaikki sovelluksen esiintymät, jotka on luotu onnistuneesti, sekä niiden yleisen tilan.

[Hae ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) -komento näyttää kaikki service-esiintymät, jotka on luotu onnistuneesti sovelluksen esiintymää. Oletus-palveluja (jos saatavilla) on lueteltu täällä.

Useita Palvelusovellusten esiintymät voidaan luoda minkä tahansa määritetyn version rekisteröidyn sovelluksen tyyppi. Sovelluksen jokaiselle esiintymälle suorittaa yksinään, työ-kansio ja prosessi.

## <a name="remove-an-application"></a>Sovelluksen poistaminen

Kun sovelluksen esiintymää ei enää tarvita, voit poistaa sen pysyvästi [Poista ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) -komennon avulla. Tämä komento poistaa automaattisesti kaikki palvelut, jotka kuuluvat myös, sovellus poistetaan pysyvästi kaikki palvelun tilan. Tätä toimintoa ei voi perua ja sovelluksen tila ei voi palauttaa.

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

Tietyn sovelluksen tyyppi-versio ei enää tarvita, kun se olisi unregister [Poista ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) -komennon avulla. Sovelluksen paketin sisällön tyypin, valitse kuva-kaupan tallennustilaa rekisteröintiä käyttämättömät eri versiot. Sovelluksen tyyppi voidaan poistaa, kunhan mitään sovelluksia on määrätty sitä vastaan ja ei sovelluksen odottavat päivitykset viittaa siihen.

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

## <a name="troubleshooting"></a>Vianmääritys

### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopioi ServiceFabricApplicationPackage pyytää ImageStoreConnectionString

Palvelun kangasta SDK-ympäristön tulisi olla jo oikea oletusarvojen määrittäminen. Mutta tarvittaessa, kaikki komennot ImageStoreConnectionString olisi vastaa arvoa, joka käyttää palvelun kangasta klusterin. Voit etsiä klusterin luettelo noutaa [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) -komennolla:

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## <a name="next-steps"></a>Seuraavat vaiheet

[Palvelun kangasta sovelluksen päivitys](service-fabric-application-upgrade.md)

[Palvelun kangasta kunto esittely](service-fabric-health-introduction.md)

[Vianmääritys ja palvelun kangasta palvelun vianmääritys](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Palvelun kangasta sovelluksen malli](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
