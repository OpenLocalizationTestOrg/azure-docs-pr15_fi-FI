<properties
    pageTitle="Azure Resurssienhallinta-pohjainen PowerShell komennot Azure-verkkosovelluksessa | Microsoft Azure"
    description="Opettele käyttämään uusi Azure Resurssienhallinta-pohjainen PowerShell-komennoilla voit hallita Azure-Web-sovelluksia."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Azure Web Apps-sovellusten hallinta Azure resurssien hallinta-pohjainen PowerShellin avulla#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Microsoft Azure PowerShellin versio 1.0.0 uusien komentojen on lisätty, joka antaa käyttäjä voi käyttää Azure Resurssienhallinta-pohjainen PowerShell-komennoilla voit hallita verkkosovelluksissa.

Lisätietoja resurssi-ryhmien hallinta on artikkelissa [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md). 

Lisätietoja parametrit ja PowerShellin cmdlet-komennot asetukset täydellinen luettelo on [Koko Cmdlet-viittaus on Web App Azure Resurssienhallinta-pohjainen PowerShellin cmdlet-komennot](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Sovelluksen palvelun hallinta-Palvelupaketit ##

### <a name="create-an-app-service-plan"></a>Luo sovellus-palvelusopimus ###
Voit luoda sovelluksen-palvelusopimus käyttämällä **Uutta AzureRmAppServicePlan** cmdlet-komento.

Seuraavassa on parametrien kuvaukset:

-   **Nimi**: nimi app palvelusopimus.
-   **Sijainti**: palvelun suunnitelman sijainti.
-   **ResourceGroupName**: resurssiryhmä, joka sisältää juuri luomasi sovelluksen palvelusopimus.
-   **Taso**: haluamasi hinnoittelu taso (oletusarvo on vapaa, muita vaihtoehtoja on jaettu, Basic, Vakio ja Premium)
-   **WorkerSize**: koon työntekijöiden (oletusarvo on pieni, jos taso-parametri on määritetty Basic, Standard tai Premium. Muut vaihtoehdot ovat Normaali ja suuri.)
-   **NumberofWorkers**: sovelluksessa työntekijöiden määrä palvelun suunnittelu (oletusarvo on 1). 

Esimerkki tätä cmdlet-komennolla:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Luo sovellus-palvelusopimus App Service-ympäristössä ###
Voit luoda sovelluksen-palvelusopimus app service-ympäristössä käyttämällä sama komento **Uusi AzureRmAppServicePlan** -komennon ylimääräisiä parametreilla ja määritä Ietokannan nimi ja Ietokannan 's resurssiryhmän nimi.

Esimerkki tätä cmdlet-komennolla:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Saat lisätietoja sovelluksen palvelun ympäristön tarkistaminen [App palvelun ympäristön esittely](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Luettelo aiemmin App palvelusopimusten vaihtoehdot ###

Olemassa olevan sovelluksen palvelusopimusten vaihtoehdot-luettelosta käyttämällä **Hae AzureRmAppServicePlan** cmdlet-komento.

Luettele kaikki sovelluksen palvelusopimusten vaihtoehdot kohdasta tilauksen avulla: 

    Get-AzureRmAppServicePlan

Jos luettelon kaikki sovelluksen palvelusopimusten vaihtoehdot tietyn resurssiryhmä-kohdassa, käytä:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Jos haluat tietyn sovelluksen palvelusopimus, käytä:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Määrittää aiemmin luodun sovelluksen palvelun suunnitteleminen ###

Voit muuttaa olemassa olevan sovelluksen-palvelusopimus asetusten avulla **Määrittäminen AzureRmAppServicePlan** cmdlet-komento. Voit muuttaa tason työntekijä koon ja työntekijöiden määrä 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Skaalaus sovelluksen-palvelusopimus ####

Jos haluat skaalata aiemmin luodun sovelluksen palvelun suunnitteleminen, käytä:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>-Sovelluksen-palvelun suunnittelu työntekijä koon muuttaminen ####

Voit muuttaa aiemmin luodun sovelluksen palvelun suunnitteleminen työntekijöiden kokoa avulla:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Sovelluksen palvelusopimus tason muuttaminen ####

Voit muuttaa aiemmin luodun sovelluksen palvelun suunnitteleminen taso avulla:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Poista aiemmin luodun sovelluksen palvelun suunnitteleminen ###

Voit poistaa aiemmin sovelluksen-palvelusopimus, kaikkien varattujen verkkosovelluksissa on siirretty tai poistettu ensin. Valitse avulla voit poistaa sovelluksen palvelusopimus **Poista AzureRmAppServicePlan** cmdlet-komento.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Sovelluksen palvelun Web-sovellusten hallinta ##

### <a name="create-a-web-app"></a>Web-sovelluksen luominen ###

Voit luoda verkkosovellukseen käyttämällä **Uutta AzureRmWebApp** cmdlet-komento.

Seuraavassa on parametrien kuvaukset:

- **Nimi**: web-sovelluksen nimi.
- **AppServicePlan**: isännöimiseen web-sovelluksen käyttää palvelusopimus nimi.
- **ResourceGroupName**: resurssiryhmä, joka isännöi App palvelusopimus.
- **Sijainti**: web app-sijaintiin.

Esimerkki tätä cmdlet-komennolla:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Web-sovelluksen luominen sovelluksen Service-ympäristössä ###

Web-sovelluksen luominen--sovelluksen palvelun ympäristössä (Ietokannan). Määritä Ietokannan ja resurssin ryhmänimi, joka Ietokannan kuuluu ylimääräisiä parametreilla saman **Uusi AzureRmWebApp** -komennon avulla.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Saat lisätietoja sovelluksen palvelun ympäristön tarkistaminen [App palvelun ympäristön esittely](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Poista aiemmin Web App-sovelluksessa ###

Voit poistaa aiemmin web app-sovelluksessa voit käyttää **Poista AzureRmWebApp** cmdlet-komento, sinun on määritettävä web-sovelluksen nimeä ja resurssien ryhmän nimeä.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Olemassa olevan Web Apps-sovellusten luettelosta ###

Luettelo aiemmin web Apps-sovellusten käyttäminen **Get-AzureRmWebApp** cmdlet-komento.

Luettelon kaikki verkkosovelluksissa kohdassa tilauksen, käytä:

    Get-AzureRmWebApp

Luettele kaikki verkkosovelluksissa tietyn resurssin-ryhmästä käytä:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Voit ladata tiettyyn web-sovelluksen avulla:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Olemassa olevan Web App-sovelluksen määrittäminen ###

Voit muuttaa asetukset ja määritykset aiemmin web-sovelluksen avulla **Määrittäminen AzureRmWebApp** cmdlet-komento. Tarkista parametrien täydellinen luettelo on [Cmdlet-viittaus linkki](https://msdn.microsoft.com/library/mt652487.aspx)

Esimerkki (1): käytä cmdlet yhteysmerkkijonon muuttamiseen

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Esimerkki (2): Lisää tai sovelluksen asetusten muuttaminen

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Esimerkki (3): 64-bittisessä tilassa web-sovelluksen määrittäminen

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Olemassa olevan Web-sovelluksen tilan muuttaminen ###

#### <a name="restart-a-web-app"></a>Käynnistä verkkosovellukseen ####

Web-sovelluksen nimi ja resurssien ryhmä on määritettävä käynnistämään verkkosovellukseen.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Lopeta verkkosovellukseen ####

Web-sovelluksen nimi ja resurssien ryhmä on määritettävä lopettavat verkkosovellukseen.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Käynnistä verkkosovellukseen ####

Alkavan verkkosovellukseen, sinun on määritettävä web-sovelluksen nimi ja resurssi-ryhmä.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Web App-julkaisemisen profiilien hallinta ###

Kunkin web-sovelluksen on julkaisun profiilia, joka voidaan julkaista sovelluksia, useita toimintoja voidaan suorittaa julkaiseminen profiilit.

#### <a name="get-publishing-profile"></a>Hae julkaiseminen profiili ####

Pääset julkaisun profiilin web-sovelluksen avulla:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Tämä komento echoes kuin hyvin tulos komentoriviltä julkaisun profiilin julkaisun profiilin tekstitiedostoon.

#### <a name="reset-publishing-profile"></a>Palauta profiilin julkaiseminen ####

Palauta molemmat julkaisun salasanan FTP- ja web käyttöönotto web app-käyttöä varten:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Web App-varmenteiden hallinta ###

Lisätietoja siitä, miten voit hallita web app-varmenteet on artikkelissa [SSL-varmenteita sidonta PowerShellin avulla](app-service-web-app-powershell-ssl-binding.md)


### <a name="next-steps"></a>Seuraavat vaiheet ###
- Lisätietoja PowerShellin Azure Resurssienhallinta tuki on artikkelissa [Azure PowerShellin Azure resurssien hallinnan.](../powershell-azure-resource-manager.md)
- Lisätietoja sovelluksen palvelun ympäristöissä on artikkelissa [Johdanto App Service-ympäristöön.](app-service-app-service-environment-intro.md)
- Lisätietoja hallinta App palvelun SSL-varmenteita PowerShellin avulla on artikkelissa [SSL-varmenteita sidonta PowerShellin.](app-service-web-app-powershell-ssl-binding.md)
- Lisätietoja Azure Resurssienhallinta-pohjainen PowerShell cmdlet-komentojen täydellisestä luettelosta Azure Web Apps-sovellukset, katso [Azure Cmdlet-viittaus Web Apps Azure Resurssienhallinta PowerShellin cmdlet-komennot,.](https://msdn.microsoft.com/library/mt619237.aspx)
- - Lisätietoja sovelluksen palvelu CLI hallinnasta on artikkelissa [Using Azure Resource Manager-Based XPlat CLI varten Azure Web Appissa.](app-service-web-app-azure-resource-manager-xplat-cli.md)
