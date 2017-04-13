<properties
    pageTitle="Automatisoida palvelun kangasta sovellusten hallinta PowerShellin avulla | Microsoft Azure"
    description="Ottaa käyttöön, päivittäminen, Testaa ja palvelun kangasta sovellusten poistaminen PowerShell-toiminnolla."
    services="service-fabric"
    documentationCenter=".net"
    authors="rwike77"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-fabric"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="ryanwi"/>

# <a name="automate-the-application-lifecycle-using-powershell"></a>Automatisoida sovelluksen elinkaari PowerShellin avulla

[Palvelun kangasta sovelluksen elinkaari](service-fabric-application-lifecycle.md) monia voidaan automatisoida.  Tässä artikkelissa kerrotaan, miten voit automatisoida yleisiä tehtäviä käyttöönotosta, päivittäminen, poistaminen ja Azure palvelun kangasta sovellusten testaaminen PowerShellin avulla.  Hallitaan ja HTTP-ohjelmointirajapinnan sovelluksen hallinnan ovat käytettävissä myös. Katso lisätietoja [sovelluksen elinkaari](service-fabric-application-lifecycle.md) .  

## <a name="prerequisites"></a>Edellytykset
Että siirrät artikkelin tehtävät, varmista, että:

+ Tutustu palvelun kangasta käsitteitä kuvattu [palvelun kangasta teknisiä tietoja](service-fabric-technical-overview.md).
+ [Asenna runtime, SDK-paketissa, ja työkalut](service-fabric-get-started.md), joiden asentaa myös **ServiceFabric** PowerShell-moduulin.
+ [Ota käyttöön PowerShell komentosarjojen suorittamisen](service-fabric-get-started.md#enable-powershell-script-execution).
+ Aloita paikallisen klusterin.  Käynnistä uuden PowerShell-ikkunassa järjestelmänvalvojana ja suorita sitten SDK-kansiosta klusterin asennuksen komentosarjan:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
+ Ennen kuin suoritat tämän artikkelin kaikki PowerShell-komennot, ensin Muodosta yhteys palvelun kangasta paikallisen klusterin käyttämällä [**Yhdistä ServiceFabricCluster**](https://msdn.microsoft.com/library/azure/mt125938.aspx):`Connect-ServiceFabricCluster localhost:19000`
+ Seuraavat toimet edellyttävät v1 sovelluspaketin, jos haluat ottaa käyttöön ja v2 sovelluspaketin päivitystä varten. Lataa [sovelluksen **WordCount** -malli](http://aka.ms/servicefabricsamples) (sijaitsevat käytön aloittaminen-mallit). Muodosta ja sovelluksen Visual Studiossa (Napsauta ratkaisunhallinnassa ja valitse **paketti** **WordCount** ). Kopioi v1 paketin `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` , `C:\Temp\WordCount`. Kopioi `C:\Temp\WordCount` , `C:\Temp\WordCountV2`-päivitykseen v2-sovelluspaketin luominen. Avaa `C:\Temp\WordCountV2\ApplicationManifest.xml` tekstieditorissa. Muuta **ApplicationTypeVersion** -määritteen "1.0.0" "2.0.0" päivittää sovelluksen versionumeron **ApplicationManifest** -elementtiä. Tallenna muutettu ApplicationManifest.xml-tiedosto.

## <a name="task-deploy-a-service-fabric-application"></a>Tehtävä: Ota käyttöön palvelun kangasta-sovellus

Kun olet sisäisten ja pakattu sovelluksen (tai ladata sovelluspaketin), voit ottaa käyttöön palvelun kangasta paikallisen klusterin sovellus. : N käyttöönottoon liittyy ladata sovelluspaketin, rekisteröiminen sovelluksen tyyppi ja luoda sovelluksen esiintymää. Uuden sovelluksen klusteriin ottaa käyttöön tämän osion ohjeita.

### <a name="step-1-upload-the-application-package"></a>Vaihe 1: Lataa sovelluspaketin
Sovelluspaketin lataaminen kuva säilön siirtää sen sijainnissa käyttämään sisäisiä palvelun kangasta osia.  Sovelluspaketin on tarpeen sovellusluettelo palvelun luettelot ja koodi-määritys ja tietojen paketit sovelluksen ja palvelun esiintymät.  [**Kopioi ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125905.aspx) -komento lataa paketti. Esimerkki:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Vaihe 2: Rekisteröi sovelluksen tyyppi
Rekisteröiminen sovelluspaketin tekee sovelluksen tyyppi ja versio ilmoitettu käytettävissä sovellusluettelo käyttöä varten. Järjestelmä lukee paketti on ladattu vaiheessa 1, tarkista (käytössä [**Testi ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125950.aspx) paikallisesti vastaa)-paketti, käsitellä pakkauksesta ja kopioi käsitellyt paketin sisäinen sijaintiin.  [**Rekisteröi ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) cmdlet-komennon suorittaminen

```powershell
Register-ServiceFabricApplicationType WordCount
```
Voit tarkastella kaikki rekisteröity klusterin sovelluksen tyypit suorittamalla [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/azure/mt125871.aspx) cmdlet-komento:

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Vaihe 3: Luo sovelluksen esiintymää
Sovellus voi vahvistaa käyttämällä sovelluksen tyyppi-versio, joka on rekisteröity onnistuneesti [**Uusi ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125913.aspx) -komennon avulla. Kunkin sovelluksen nimi on määritetty osoitteessa käyttöön aika ja alettava **kangasta:** menetelmä ja sovelluksen jokaiselle esiintymälle yksilölliseksi. Sovelluksen nimi ja tyyppi sovellusversio on määritetty sovelluspaketin **ApplicationManifest.xml** -tiedosto. Jos oletusarvoinen palvelut on määritetty kohdetyyppi-sovellus sovellusluettelo, valitse ne luodaan tällä hetkellä.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

[**Hae ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx) -komento näyttää kaikki sovelluksen esiintymät, jotka on luotu onnistuneesti, sekä niiden yleisen tilan. [**Hae ServiceFabricService**](https://msdn.microsoft.com/library/azure/mt125889.aspx) -komento Luetteloi kaikki palvelun esiintymät, jotka on luotu onnistuneesti sovelluksen esiintymää. Oletus-palveluja (jos saatavilla) näkyvät.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Tehtävä: Päivitä palvelun kangasta-sovellus
Voit päivittää päivitetty sovelluspaketin aiemmin käyttöön palvelun kangasta-sovellukseen. Tämän tehtävän päivittää WordCount sovellus, jossa on otettu käyttöön "tehtävän: palvelun kangasta-sovellus." Lue lisätietoja – [palvelu kangasta sovelluksen päivittäminen](service-fabric-application-upgrade.md) .

Voit pitää asiat helposti tässä esimerkissä vain sovelluksen versionumeron on päivitetty luotu edellytykset WordCountV2 sovelluspaketin. Realistisesta tilanne aiheuttaa palvelun koodi tai määritysten tiedostojen päivittäminen ja joiden ja pakkaamista sovelluksen päivitetyn version lukuja.  

### <a name="step-1-upload-the-updated-application-package"></a>Vaihe 1: Lataa päivitetyt sovelluspaketin
WordCount v1-sovellus on valmis päivitettäväksi. Jos avaat PowerShell-ikkunan järjestelmänvalvoja ja kirjoita [**Hae ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx), näet kyseisen version 1.0.0 WordCount sovelluksen tyyppi on otettu käyttöön.  

Nyt voit kopioida päivitetyt sovelluspaketin palvelun kangasta kuva Store (sovelluksen pakettien tallennuspaikkaa palvelun kangasta mukaan). Parametrin **ApplicationPackagePathInImageStore** ilmoittaa palvelun kangasta, mistä ne löytyvät sovelluspaketin. Seuraavalla komennolla kopioi sovelluspaketin **WordCountV2** kuva kaupan:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Vaihe 2: Rekisteröi päivitetty sovellus
Seuraavaksi voit rekisteröidä sovelluksen uuden version Service kangasta, jota voi käyttää [**Rekisteröi ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) cmdlet-komennolla:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Vaihe 3: Käynnistä päivitys
Eri päivityksen parametreja, aikakatkaisu ja terveyteen ehdot voidaan ottaa käyttöön sovelluksen päivitykset. Lue lisätietoja [sovelluksen päivittäminen parametrit](service-fabric-application-upgrade-parameters.md) ja [päivitysprosessi](service-fabric-application-upgrade.md) asiakirjat. Kaikkien palveluiden ja esiintymät on _kunnossa_ päivityksen jälkeen.  Määritä **HealthCheckStableDuration** 60 sekuntia (niin, että palvelut ovat kunnossa vähintään 20 sekunnin ajan ennen päivitystä siirtyy seuraavan päivityksen toimialueen).  Set **UpgradeDomainTimeout** 1200 sekunnit ja **UpgradeTimeout** 3000 sekuntia. Määritä lopuksi **UpgradeFailureAction** **peruuttaminen**, joka pyytää, että palvelun kangasta peruutetaan aiemmasta versiosta sovelluksen Jos epäonnistuu päivityksen yhteydessä.

Voit aloittaa sovelluksen päivitys [**Käynnistä ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125975.aspx) cmdlet-komennolla:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Huomaa, että sovelluksen nimi on sama kuin aiemmin käyttöön v1.0.0 sovelluksen nimi (kangasta: / WordCount). Palvelun kangasta käyttää tätä nimeä tunnistamiseen, missä sovelluksessa käytön päivitetään. Jos määrität aikakatkaisu on liian lyhyt, aikakatkaisun virhe-sanoma, joka ilmoittaa ongelma saattaa ilmetä. Viitata [vianmääritys sovelluksen päivityksiä](service-fabric-application-upgrade-troubleshooting.md)tai suurenna aikakatkaisu.

### <a name="step-4-check-upgrade-progress"></a>Vaihe 4: Tarkista päivityksen edistyminen
Voit valvoa sovellusta päivityksen edistyminen [Palvelun kangasta Resurssienhallinnan](service-fabric-visualizing-your-cluster.md)avulla tai [**Hae ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125988.aspx) cmdlet-komennolla:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

Muutaman minuutin kuluttua [Get-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) cmdlet-komento näkyy, että kaikki päivityksen toimialueet on päivitetty (valmis).

## <a name="task-test-a-service-fabric-application"></a>Tehtävä: Testaa palvelun kangasta-sovellus

Kirjoittaa laadukkaita services kehittäjät on voi aiheuttaa epäluotettavista infrastruktuurin virheitä Testaa niiden palveluiden vakavuus. Palvelun kangasta antaa kehittäjät voivat aiheuttaa virheen toiminnot ja testaa palvelujen kanssa virheet käyttämällä chaos ja automaattisesti testi-skenaariot.  Lue lisätietoja [testattavuus yleiskatsaus](service-fabric-testability-overview.md) .

### <a name="step-1-run-the-chaos-test-scenario"></a>Vaihe 1: Suorita chaos testi-skenaario
Chaos testi skenaarion Luo virheitä koko palvelun kangasta klusterin yli. Skenaario pakkaa näkyvät yleensä kuukausien tai vuosien muutaman tunnin virheitä. Suuri vika Ylityökorvaus limitetty virheitä yhdistelmä löytää yläkulmassa tapaukset, jotka muuten vastattu. Seuraavassa esimerkissä suoritetaan chaos testi skenaarion 60 minuuttia.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Vaihe 2: Suorita automaattisesti testi-skenaario
Vikasietotilaa testaaminen skenaarion kohteet sen sijaan, että koko klusterin tietyn palvelun osio ja sen jättää muiden palvelujen muutu. Skenaario iteroivaa Simuloitu virheet-palvelun vahvistus järjestyksen kautta organisaatiosi liiketoimintalogiikan suorittamisen aikana. Virhe-palvelun vahvistus osoittaa ongelman, joka on vielä tutkimuksen. Automaattisesti testi aiheuttaa vika vain yksi kerrallaan, eikä skenaarion chaos testi, jossa voi aiheuttaa virheitä on useita. Seuraavassa esimerkissä suoritetaan automaattisesti testi 60 minuuttia kangasta: / WordCount/WordCountService palvelu.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Tehtävä: Palvelun kangasta sovelluksen poistaminen
Voit poistaa käyttöön sovelluksen esiintymää, valmistellun sovelluksen tyyppi poistaminen klusterin ja poista sovelluspaketin ImageStore.

### <a name="step-1-remove-an-application-instance"></a>Vaihe 1: Poista sovellusesiintymä
Kun sovelluksen esiintymää ei enää tarvita, voit poistaa sen pysyvästi [**Poista ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125914.aspx) cmdlet-komennolla. Tämä poistaa myös kaikki palvelut kuuluvat sovellus poistetaan pysyvästi kaikki palvelun tilan. Tätä toimintoa ei voi perua ja sovelluksen tila ei voi palauttaa.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Vaihe 2: Unregister sovelluksen tyyppi
Kun et enää tarvitse tiettyä sovelluksen tyyppi-versio, poista [**Poista ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125885.aspx) cmdlet-komennolla. Kuva kaupan sovelluspaketin tallennustilaa rekisteröintiä käyttämättömät eri versiot. Sovelluksen tyyppi voidaan poistaa, kunhan ei ole sovelluksia vahvistaa sitä vastaan tai odottavia sovelluksen päivityksiä viittaa siihen.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Vaihe 3: Poista sovelluspaketin
Kun sovelluksen tyyppi ei ole rekisteröity-sovelluspaketin voidaan poistaa kuvan kaupasta [**Poista ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt163532.aspx) cmdlet-komennolla.

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Seuraavat vaiheet
[Palvelun kangasta sovelluksen elinkaari](service-fabric-application-lifecycle.md)

[Sovelluksen ottaminen käyttöön](service-fabric-deploy-remove-applications.md)

[Sovelluksen päivitys](service-fabric-application-upgrade.md)

[Azure palvelun kangasta cmdlet-komennot](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Azure palvelun kangasta testattavuus cmdlet-komennot](https://msdn.microsoft.com/library/azure/mt125844.aspx)
