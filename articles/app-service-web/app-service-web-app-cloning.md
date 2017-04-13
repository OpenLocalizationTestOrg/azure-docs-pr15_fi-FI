<properties
    pageTitle="Web-sovelluksen Kloonaamalla PowerShellin avulla"
    description="Opettele Kloonaa Web Apps-sovellusten PowerShellin avulla uusi Web-sovelluksiin."
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
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App palvelun sovelluksen Kloonaamalla PowerShellin avulla#

Microsoft Azure PowerShell versio 1.1.0 versiossa uusi asetus on lisätty uusi-AzureRMWebApp, jotka antaa käyttäjälle mahdollisuus Kloonaa aiemmin Web-sovelluksen juuri luomasi sovellukseen toisella alueella tai samalla alueella. Tämä ottaa käyttöön asiakkaat voivat ottaa käyttöön useita sovelluksia eri alueiden nopeasti ja helposti.

Sovelluksen kloonaamalla ei tällä hetkellä tueta vain premium taso app palvelusopimusten vaihtoehdot. Uusi ominaisuus käyttää samaa rajoitukset Web Apps varmuuskopiointi-ominaisuudeksi on artikkelissa [Azure-sovelluksen palvelun verkkosovellukseen varmuuskopioida](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Lisätietoja käyttämisestä Azure Resurssienhallinta perusteella Azure PowerShell cmdlet-komentojen Web Apps-sovellusten Tarkista [Azure Resurssienhallinta perusteella PowerShell-komennot Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Olemassa olevan sovelluksen kloonaamalla ##

Skenaario: Aiemmin web-sovelluksen Etelä keskitetyn US alue-käyttäjä haluaa Kloonaa uusi verkkosovellukseen Pohjois keskitetyn US alueen sisältö. Tämä onnistuu käyttämällä PowerShell-cmdlet-komennon Azure Resurssienhallinta-versiota, voit luoda uuden online - SourceWebApp-vaihtoehto.

Tietää resurssin ryhmänimi, joka sisältää tietolähteen web app Microsoft käyttää seuraavaa PowerShell-komentoa (Tässä tapauksessa nimeltään lähde-Web App-sovelluksen) lähde web Appissa on artikkelissa:

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Jos haluat luoda uuden sovelluksen-palvelu-palvelupaketti, Microsoft käyttää uutta AzureRmAppServicePlan-komento, kuten seuraavassa esimerkissä

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Uusi AzureRmWebApp-komennon avulla voimme luoda uuden web-sovelluksen Pohjois keskitetyn US alueen ja sitominen se aiemmin premium-tason sovelluksen-palvelun suunnittelu, lisäksi Microsoft käyttää samaa resurssiryhmä lähde-web-sovelluksen tai määritä uusi resurssiryhmä, seuraavat osoittaa:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Kloonaa aiemmin online, myös kaikki siihen liittyvät käyttöönoton lähtö saapumisajat, käyttäjän on IncludeSourceWebAppSlots-parametrin, PowerShell-komentoa kuvataan parametrin uusi AzureRmWebApp-komennolla:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots $true

Voit kopioida aiemmin web-sovelluksen samalla alueella, käyttäjän on luotava uusi resurssiryhmä ja uusi sovellus-palvelun suunnittelu samalla alueella ja Kloonaa web-sovelluksen seuraavan PowerShell-komennon avulla

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Kloonaamalla olemassa olevan sovelluksen App Service-ympäristössä ##

Skenaario: Aiemmin web-sovelluksen Etelä keskitetyn US alue, käyttäjä haluaa Kloonaa sisältö uuteen web Appiin, aiemmin App palvelun ympäristössä (Ietokannan).

Tietää resurssin ryhmänimi, joka sisältää tietolähteen web app Microsoft käyttää seuraavaa PowerShell-komentoa (Tässä tapauksessa nimeltään lähde-Web App-sovelluksen) lähde web Appissa on artikkelissa:

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Tietää Ietokannan nimi ja resurssien ryhmänimi, joka Ietokannan kuuluu, käyttäjä voi käyttää uutta AzureRmWebApp-komento uusi web-sovelluksen luominen aiemmin Ietokannan, seuraavat osoittaa:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Sijainti-parametri on pakollinen vanha syystä, mutta kyseessä-sovelluksen luominen Ietokannan se ohitetaan. 

## <a name="cloning-an-existing-app-slot"></a>Aiemmin luodun sovelluksen paikka kloonaamalla ##

Skenaario: Käyttäjä haluaa Kloonaa aiemmin luodun sovelluksen Web-paikka, joko uusi verkkosovellukseen, tai uusi Web App-paikan. Uusi Web-sovellus voi olla sama kuin alkuperäinen Web App-paikka alue tai toisella alueella.

Tietää resurssin ryhmänimi, joka sisältää tietolähteen web app Microsoft käyttää seuraavaa PowerShell-komentoa lähteen web app paikka tiedot (Tässä tapauksessa nimeltään lähteen webappslot) sidottu Web App lähde-Web App-sovelluksen:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Seuraavassa esitellään Kloonaa uusi web App-sovellukseen lähteen web-sovelluksen luominen:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Määrittäminen ja sitten kloonaamalla liikenteen hallinta ##

Monille web Apps-sovellusten luominen ja määrittäminen Azure liikenteen hallinta ja haluat reitittää liikenteen kaikki nämä web-sovellusta, on n tärkeitä skenaario voit varmistaa, että asiakkaiden sovellukset ovat käytettävissä, kun kloonaamalla aiemmin web app-sovelluksessa voit halutessasi voit muodostaa yhteyden joko uuden liikenteen hallinta profiilin tai aiemmin luodun sekä online - Huomaa, että liikenne Manager-versiota tuetaan vain Azure Resurssienhallinta.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Uuden liikenteen hallinta profiilin luomisen aikana kloonaamalla sitten ###

Skenaario: Käyttäjä haluaa Kloonaa web-sovelluksen toisen alueen määritettäessä Azure Resurssienhallinta-liikenteen hallinta profiilin, jotka sisältävät sekä verkkosovelluksissa. Seuraavassa esitellään luominen Kloonaa lähteen web Appin uudet web App-sovellukseen määritettäessä uusi profiili liikenteen hallinta:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Uuden lisääminen monistaa Web App liikenteen hallinta-profiiliin ###

Skenaario: Käyttäjä on jo Azure Resurssienhallinta-liikenteen hallinta profiilia, joka hän haluat lisätä sekä verkkosovelluksissa päätepisteet. Tekemään niin ensin annettava tarvitaan aiemmin liikenteen hallinta Profiilitunnus annettava tilauksen tunnus, resurssiryhmän nimi ja aiemmin liikenteen hallinta profiilinimi.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Sen jälkeen ottaa liikenteen hallinta-tunnus, seuraavat esittelee luominen Kloonaa lähteen web Appin uudet web App-sovellukseen silloin, kun lisäät ne liikenteen hallinta-profiiliin:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Nykyiset rajoitukset ##

Tämä toiminto ei esikatselussa, voit lisätä uusia ominaisuuksia ajan kuluessa Yritämme, seuraavassa luettelossa on sovelluksen kloonaamalla nykyisen version tunnetut rajoitukset:

- Automaattinen mittakaava-asetuksia ei kopioitu
- Aikatauluasetuksia ei kopioitu
- VNET asetuksia ei kopioitu
- Sovelluksen tietoja ei ole automaattisesti määritetty kohde web Appissa
- Helppo Auth asetuksia ei kopioitu
- Kudu tunniste ei kopioitu
- Vihje sääntöjä ei kopioitu
- Tietokannan sisältö ei kopioitu


### <a name="references"></a>Viittaukset ###
- [Azure Resurssienhallinta perusteella Azure-verkkosovelluksessa PowerShell-komennot](app-service-web-app-azure-resource-manager-powershell.md)
- [Web-sovelluksen Kloonaamalla Azure-portaalissa](app-service-web-app-cloning-portal.md)
- [Azure-sovelluksen palvelun verkkosovellukseen varmuuskopiointi](web-sites-backup.md)
- [Azure Resurssienhallinta Azure liikenteen hallinta esikatselu-tuki](../../articles/traffic-manager/traffic-manager-powershell-arm.md)
- [Johdanto App Service-ympäristöön](app-service-app-service-environment-intro.md)
- [Azure PowerShell kanssa Azure resurssien hallinnan avulla](../powershell-azure-resource-manager.md)
