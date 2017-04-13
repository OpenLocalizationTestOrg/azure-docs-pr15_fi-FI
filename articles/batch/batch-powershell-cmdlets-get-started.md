<properties
   pageTitle="Aloita erä PowerShellin Azure | Microsoft Azure"
   description="Katso lyhyt esittely avulla voit hallita Azure erä palvelua Azure PowerShellin cmdlet-komennot"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/20/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>Aloita Azure erä PowerShellin cmdlet-komennot
Azure erä PowerShellin cmdlet-komennot voit suorittaa ja komentosarjan monia samoja tehtäviä voit suorittaa Siirtoerän API, Azure portaalin ja Azure käyttöliittymä (CLI). Näin voit tehdä erä tilien hallinta ja erä-resurssien kuten jakavat, projektien ja tehtävien käsitteleminen cmdlet-komennot lyhyt esittely.

Luettelo erä cmdlet-komennot ja yksityiskohtainen cmdlet-komennon syntaksi Hae [Azure erä cmdlet-viittaus](https://msdn.microsoft.com/library/azure/mt125957.aspx).

Tässä artikkelissa perustuu PowerShellin Azure versio 3.0.0 cmdlet-komennoista. On suositeltavaa, että voit päivittää Azure-PowerShell usein, jotta voit hyödyntää palvelupäivityksistä ja parannukset.

## <a name="prerequisites"></a>Edellytykset

Voit suorittaa seuraavat toimenpiteet PowerShellin Azure avulla voit hallita erä resursseja.

* [Asenna ja määritä PowerShellin Azure](../powershell-install-configure.md)

* Suorita muodostaa (Azure erä cmdlet-komennot alusta Azure Resurssienhallinta-moduulissa)-tilaukseen **Kirjautuminen AzureRmAccount** cmdlet-komento:

    `Login-AzureRmAccount`

* **Rekisteröi erä tarjoajan nimitila**. Tämä toiminto on vain voidaan suorittaa **tilauskohtaisten kerran**.

    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Hallitse erä tilit ja avaimet

### <a name="create-a-batch-account"></a>Erän tilin luominen

**Uusi AzureRmBatchAccount** Luo erä tilin määritetyn resurssiryhmä. Jos sinulla ei vielä ole resurssiryhmä, luo sellainen suorittamalla [Uusi AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) cmdlet-komento. Määritä Azure alueiden **sijainti** -parametrissa, kuten "Keskitetyn USA". Esimerkki:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Luo sitten erä tilin <*tilin_nimi*> tilin ja sijainti ja nimi resurssin ryhmän nimi, joka määrittää resurssi-ryhmässä. Erän-tilin luominen voi kestää jonkin aikaa suorittamiseen. Esimerkki:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [AZURE.NOTE] Erän tilin nimen on oltava yksilöllinen resurssiryhmän Azure alue, 3 – 24 merkkiä sisältävät ja pieniä kirjaimia ja numeroita vain.

### <a name="get-account-access-keys"></a>Hae tilin pikanäppäimet
**Hae AzureRmBatchAccountKeys** näyttää erä Azure-tiliin liittyvät pikanäppäimet. Esimerkiksi suorittaa seuraavat saat tili, jonka loit ensisijainen ja toissijainen-näppäimiä.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Luo uusi pikanäppäin
**Uusi AzureRmBatchAccountKey** Luo uuden ensisijaisen tai toissijaisen tilin tunnuksen Azure erä-tilille. Luo uusi perusavain erä tilin, esimerkiksi:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [AZURE.NOTE] Luo uusi toissijainen avain määrittää "Toissijainen" **KeyType** -parametrin. Sinun on luotava ensisijainen ja toissijainen näppäimet erikseen.

### <a name="delete-a-batch-account"></a>Poista erä-tili
**Poista AzureRmBatchAccount** poistaa Siirtoerän-tili. Esimerkki:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Kun sinulta kysytään, Vahvista poistettava tili. Tilin poistaminen voi kestää jonkin aikaa suorittamiseen.

## <a name="create-a-batchaccountcontext-object"></a>BatchAccountContext objektin luominen

Tarkistamiseen avulla erä PowerShellin cmdlet-komennot, kun voit luoda ja hallita erä jakavat, työt, tehtävät ja muut resurssit luominen BatchAccountContext-objektin tallentamiseen tilin nimi ja avaimet:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Voit siirtää BatchAccountContext objektin cmdlet-komennot, jotka käyttävät **BatchContext** -parametrin.

> [AZURE.NOTE] Asiakkaan perusavaimen käytetään todennusta varten, mutta voit valita erikseen salaisuus muuttamalla BatchAccountContext-objektin **KeyInUse** -ominaisuuden avulla: `$context.KeyInUse = "Secondary"`.

## <a name="create-and-modify-batch-resources"></a>Luoda ja muokata erä resurssit
Cmdlet-komentoja, kuten **Uusi AzureBatchPool**, **Uusi AzureBatchJob**ja **Uusi AzureBatchTask** avulla voit luoda resurssien erä-tilissä. Käytettävissä ovat vastaavat **Get -** ja **Set-** cmdlet-komentojen päivittäminen aiemmin resurssien ominaisuuksien ja **Poista-** cmdlet-komennot, poista resursseja erä-tilissä.

Kun monilla näiden cmdlet-komennot, lisäksi kulkeva BatchContext kohdetta, sinun täytyy luoda tai välittää objekteja, jotka sisältävät yksityiskohtaiset resurssiasetukset, kuten seuraavassa esimerkissä. Katso Lisää esimerkkejä cmdlet-komento, joita yksityiskohtaiset ohjeet.

### <a name="create-a-batch-pool"></a>Luoda erää varannon

Kun luominen tai päivittäminen erä resurssivarantoon, valitse cloud-palvelun määritykset tai Laske solmujen (katso [erä ominaisuus yleiskatsaus](batch-api-basics.md#pool))-käyttöjärjestelmän virtuaalikoneen konfiguroinnin. Valintasi määrittää, onko Laske solmujen kuvallinen mainitun [Azure Vieras OS julkaisee](../cloud-services/cloud-services-guestos-update-matrix.md#releases) tai jokin tuettu Linux tai Windowsin AM kuvista Azure Marketplacesta.

Kun suoritat **Uusi AzureBatchPool**, välittää käyttöjärjestelmän asetukset PSCloudServiceConfiguration tai PSVirtualMachineConfiguration objekti. Esimerkiksi seuraavan cmdlet-komennon Luo uusi ryhmä erän kokoa pieni Laske solmujen cloud palvelun määrityksessä kuvallinen tason 3 (Windows Server 2012) uusimman käyttöjärjestelmän version kanssa. Tässä kohdassa **CloudServiceConfiguration** -parametri ilmaisee *$configuration* muuttujan PSCloudServiceConfiguration objektina. **BatchContext** -parametri ilmaisee aiemmin määritettyä muuttujan *$context* BatchAccountContext objektina.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Uuden ryhmän Laske solmujen kohde määrä määräytyy automaattisen skaalauksen poistaminen kaava. Tässä tapauksessa kaava on vain **$TargetDedicated = 4**, joka ilmaisee ryhmän Laske solmujen määrän on 4 enintään.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Kyselyn jakavat, työt, tehtävät ja muut tiedot

Käytä cmdlet kuten **Get-AzureBatchPool**, **Hae AzureBatchJob**ja **Hae AzureBatchTask** kyselyn kohteita, luoda erää-tilissä.

### <a name="query-for-data"></a>Tietokysely

Esimerkkinä Etsi oman jakavat **Get-AzureBatchPools** avulla. Oletusarvoisesti tämä kyselyjä ja kaikki jakavat, valitse tili, jos olet jo tallentanut BatchAccountContext objektin *$context*:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>OData-suodatus

Voit antaa OData-suodatin, vain objekteja, käyttämällä **Suodatin** -parametrin avulla. Voit esimerkiksi hakea kaikki jakavat tunnukset "myPool" alkaen:

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Tämä menetelmä ei ole mahdollisimman joustavia "Where-objektin" käyttäminen paikallisen putkijohto. Kuitenkin kyselyn lähetetään saa erä-palveluun suoraan niin, että kaikki suodattaminen tapahtuu palvelinpuolen tallentaminen Internet-kaistanleveys.

### <a name="use-the-id-parameter"></a>Käytä Id-parametrin

Vaihtoehtoinen OData-suodatin on käyttää **Id** -parametrin. Kyselyn tietyn ryhmän tunnus "myPool" kanssa:

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

**Id** -parametrin tukee vain tuotetunnus-haku, ei yleismerkkejä ja OData-tyyppiseksi suodattimet.

### <a name="use-the-maxcount-parameter"></a>Käytä MaxCount-parametri

Oletusarvon mukaan kunkin cmdlet-komento palauttaa enintään 1 000 objekteja. Jos enimmäismäärä on saavutettu, Tarkenna haluat tuoda takaisin vähemmän objekteja tai enintään käyttämällä **MaxCount** -parametri on määritetty. Esimerkki:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Jos haluat poistaa yläraja, Määritä **MaxCount** 0 tai vähemmän.

### <a name="use-the-powershell-pipeline"></a>Käytä PowerShell-myyntijakso

Erän cmdlet-komentojen avulla voidaan hyödyntää PowerShell Lähetä tietoa cmdlet-komentojen välinen putkijohto. Tämä on samoin kuin parametri, joka määrittää, mutta helpottaa useiden kohteiden käsittelemistä.

Kuten Etsi ja palauttaa näkyviin kaikki tehtävät-kohdassa tilin:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Uudelleenkäynnistys (Käynnistä) resurssivarantoon Laske jokaisen solmu:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Sovelluksen paketin hallinta

Sovelluksen pakettien lisäämistapaa vaivaton sovellusten oman jakavat Laske solmujen. Erän PowerShellin cmdlet-komennot, jossa voit ladata ja sovelluksen pakettien hallinta erä tilisi ja käyttöönotto paketti versiot solmujen laskemiseen.

**Luo** sovelluksen:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Lisää** sovelluspaketin:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Määritä **oletusversio** sovelluksen:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Luettelon** sovelluksen-paketit

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Poista** sovellus-paketti

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

Sovelluksen **poistaminen**

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

>[AZURE.NOTE] Sinun on poistettava kaikista sovelluksen sovelluksen paketin versiot, ennen kuin voit poistaa sovelluksen. Näyttöön tulee "Ristiriidan"-virhe, jos yrität poistaa sovellus, joka on tällä hetkellä sovelluksen pakettien.

### <a name="deploy-an-application-package"></a>Sovelluksen paketin käyttöönotto

Voit määrittää vähintään yksi sovellus pakettien käyttöönottoa varten, kun luot resurssivarantoon. Määrittäessäsi paketin varannon luontia milloin se otetaan käyttöön kunkin solmun kuin solmu liitokset resurssivarantoon. Pakettien myös otettu käyttöön, kun solmu on käynnistettävä tai reimaged.

Määritä `-ApplicationPackageReference` -asetus käyttöön sovelluspaketin resurssivarantoon solmujen, kun he liittyä ryhmään resurssivarantoon luotaessa. Ensin **PSApplicationPackageReference** objektin luominen ja määrittäminen haluat ottaa käyttöön ryhmän Laske solmujen sovelluksen-tunnus ja pakkaus versiolla:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Luo altaan nyt ja määritä viittaus pakkaus argumenttina `ApplicationPackageReferences` vaihtoehto:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

Voit Lisätietoja sovelluksen pakettien [Azure erä sovelluksen pakettien sovellusten käyttöön](batch-application-packages.md).

>[AZURE.IMPORTANT] [Linkki Azure-tallennustilan tilin](#linked-storage-account-autostorage) on käyttää sovelluksen pakettien erä-tiliisi.

### <a name="update-a-pools-application-packages"></a>Päivitä resurssivarantoon sovelluksen pakettien

Jos haluat päivittää aiemmin sovellussarjan liitettyihin sovellukset, Luo PSApplicationPackageReference objektin ominaisuuksin (sovelluksen tunnus ja pakkaus versio):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Seuraavaksi hakee altaan erä, Tyhjennä kaikki olemassa olevat pakettien, lisää oma uusi paketti-viittaus, ja päivittää erä palvelun uusia resurssivarantoon asetuksia:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Olet nyt päivittää ryhmän ominaisuudet erä-palvelussa. Todella käyttöön uuden sovelluspaketin solmujen ryhmään laskea, kuitenkin on käynnistettävä uudelleen tai reimage näiden solmujen. Voit käynnistää kunkin solmun varannon tällä komennolla:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

>[AZURE.TIP] Voit ottaa käyttöön sovelluksen paketit Laske solmujen resurssivarantoon. Jos haluat *lisätä* sovelluspaketin sijaan korvaaminen tällä hetkellä käyttöön paketteja, Ohita `$pool.ApplicationPackageReferences.Clear()` rivin yläpuolella.

## <a name="next-steps"></a>Seuraavat vaiheet

* Yksityiskohtainen cmdlet-komennon syntaksi ja esimerkkejä on artikkelissa [Azure erä cmdlet-viittaus](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Saat lisätietoja sovellukset ja sovelluksen pakettien osalta [Azure erä sovelluksen pakettien sovellusten käyttöön](batch-application-packages.md).
