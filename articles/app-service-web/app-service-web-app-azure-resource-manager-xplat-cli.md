<properties
    pageTitle="Azure Resurssienhallinta-pohjaiseen Office kaikissa ympäristöissä komentoriviltä työkaluja Azure Web App | Microsoft Azure"
    description="Opettele uusi Azure Resurssienhallinta-pohjaisten Office kaikissa ympäristöissä komentorivi-työkalujen avulla voit hallita Azure Web Apps-sovellusten."
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

# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-web-app"></a>Azure resurssien hallinta-pohjainen XPlat CLI käytön Azure Web Appissa#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Microsoft Azure Office kaikissa ympäristöissä komentorivivalitsimet työkalujen versio 0.10.5 versiossa on lisätty uusi komentoja. Nämä komennot antaa käyttäjä voi käyttää Azure Resurssienhallinta-pohjainen PowerShell-komennoilla voit hallita verkkosovelluksissa.

Lisätietoja resurssi-ryhmien hallinta on artikkelissa [Azure CLI Azure resurssit ja resurssiryhmien hallintaan käyttää](../xplat-cli-azure-resource-manager.md). 


## <a name="managing-app-service-plans"></a>Sovelluksen palvelun hallinta-Palvelupaketit ##

### <a name="create-an-app-service-plan"></a>Luo sovellus-palvelusopimus ###
Voit luoda sovelluksen-palvelusopimus käyttämällä **azure appserviceplan luominen** -komento.

Seuraavassa on parametrien kuvaukset:

-   **--resurssiryhmä**: resurssiryhmä, joka sisältää juuri luomasi sovelluksen palvelusopimus.
-   **--nimi**: nimi app palvelusopimus.
-   **--sijainti**: app suunnitelman sijainti.
-   **--taso**: haluamasi hinnoittelu sku (vaihtoehdot ovat: F1 (vapaa). D1 (jaettu). B1 (Basic pieni), B2 (Basic Normaali), ja B3 (Basic suuri). S1 (pieni vakio), S2 (Vakio Normaali), ja S3 (Normaali, suuri). P1 (Premium pieni), P2 (Normaali Premium), ja P3 (Premium suuri).)
-   **--esiintymät**: sovelluksessa työntekijöiden määrä palvelun suunnittelu (oletusarvo on 1).

Esimerkki tätä cmdlet-komennolla:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

### <a name="list-existing-app-service-plans"></a>Luettelo aiemmin App palvelusopimusten vaihtoehdot ###

Avulla luodun sovelluksen palvelusopimusten vaihtoehdot-luettelosta **azure appserviceplan luettelo** -komento.

Jos luettelon kaikki sovelluksen palvelusopimusten vaihtoehdot tietyn resurssiryhmä-kohdassa, käytä:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Jos haluat tietyn sovelluksen palvelusopimus, käytä **azure appserviceplan Näytä** -komento:

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Määrittää aiemmin luodun sovelluksen palvelun suunnitteleminen ###

Jos haluat muuttaa olemassa olevan sovelluksen-palvelusopimus asetusten, **azure appserviceplan config** -komennolla. Voit muuttaa olevat versiot ja työntekijöiden määrä 

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Skaalaus sovelluksen-palvelusopimus ####

Jos haluat skaalata aiemmin luodun sovelluksen palvelun suunnitteleminen, käytä:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Sovelluksen palvelusopimus SKU muuttaminen ####

Voit muuttaa aiemmin luodun sovelluksen palvelun suunnitteleminen sku avulla:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Poista aiemmin luodun sovelluksen palvelun suunnitteleminen ###

Voit poistaa aiemmin sovelluksen-palvelusopimus, kaikkien varattujen verkkosovelluksissa on siirretty tai poistettu ensin. Valitse komennolla **azure Web App-sovelluksen poistaminen** voit poistaa sovelluksen palvelusopimus.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-web-apps"></a>Sovelluksen palvelun Web-sovellusten hallinta ##

### <a name="create-a-web-app"></a>Web-sovelluksen luominen ###

Web-sovelluksen luominen-komennolla **azure Web App-sovelluksen luominen** .

Seuraavassa on parametrien kuvaukset:

- **--nimi**: web-sovelluksen nimi.
- **--suunnitelman**: isännöimiseen web-sovelluksen käyttää palvelusopimus nimi.
- **--resurssiryhmä**: resurssiryhmä, joka isännöi App palvelusopimus.
- **--sijainti**: web app-sijaintiin.

Esimerkki tätä cmdlet-komennolla:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-web-app"></a>Poista aiemmin Web App-sovelluksessa ###

Voit poistaa aiemmin web app-sovelluksessa voit **poistaa azure Web App-sovelluksen** komennolla, sinun on määritettävä web-sovelluksen nimeä ja resurssien ryhmän nimeä.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-web-apps"></a>Olemassa olevan Web Apps-sovellusten luettelosta ###

Luettelo aiemmin web Apps-sovellusten **azure Web App-sovelluksen luettelo** -komennolla.

Luettele kaikki verkkosovelluksissa tietyn resurssin-ryhmästä käytä:

    azure webapp list --resource-group ContosoAzureResourceGroup

Voit ladata tiettyyn web-sovelluksen komennolla **azure Web App-sovelluksen Näytä** .

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-web-app"></a>Olemassa olevan Web App-sovelluksen määrittäminen ###

Jos haluat muuttaa asetukset ja määritykset aiemmin web-sovelluksen, komennolla **azure Web App-sovelluksen config määrittäminen** .

Esimerkki (1): Muuta verkkosovellukseen php-versio 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Esimerkki (2): Lisää tai sovelluksen asetusten muuttaminen

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Haluat tietää, mitä muut määritys voidaan muuttamisen käyttämällä **azure Web App-sovelluksen config set -h** -komentoa.

### <a name="change-the-state-of-an-existing-web-app"></a>Olemassa olevan Web-sovelluksen tilan muuttaminen ###

#### <a name="restart-a-web-app"></a>Käynnistä verkkosovellukseen ####

Web-sovelluksen nimi ja resurssien ryhmä on määritettävä käynnistämään verkkosovellukseen.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Lopeta verkkosovellukseen ####

Web-sovelluksen nimi ja resurssien ryhmä on määritettävä lopettavat verkkosovellukseen.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Käynnistä verkkosovellukseen ####

Alkavan verkkosovellukseen, sinun on määritettävä web-sovelluksen nimi ja resurssi-ryhmä.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Web App-julkaisemisen profiilien hallinta ###

Kunkin web-sovelluksessa on julkaisun profiilia, joka voidaan julkaista sovelluksia.

#### <a name="get-publishing-profile"></a>Hae julkaiseminen profiili ####

Pääset julkaisun profiilin web-sovelluksen avulla:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Tämä komento echoes julkaisun profiili-käyttäjänimi ja salasana, komentorivillä.

### <a name="manage-web-app-hostnames"></a>Web App-isäntänimet hallinta ###

Voit hallita hostname sidontojen web App- **azure Web App-sovelluksen config isäntänimet** -komennon avulla  

#### <a name="list-hostname-bindings"></a>Luettelon hostname sidontojen ####

Pääset nykyisen hostname sidontojen, web-sovelluksen avulla:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Lisää hostname sidontojen ####

Lisää hostname sidontojen web App-sivustoa:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Hostname sidontojen poistaminen ####

Voit poistaa hostname sidontojen-sovelluksella:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

### <a name="next-steps"></a>Seuraavat vaiheet ###
- Lisätietoja Azure Resurssienhallinta CLI tuki on artikkelissa [Azure-CLI avulla voit hallita Azure resurssit ja resurssiryhmät.](../xplat-cli-azure-resource-manager.md)
- Lisätietoja hallinta App palvelun PowerShellin avulla on artikkelissa [Using Azure Resource Manager-Based PowerShellin Azure Web Apps-sovellusten hallinta.](app-service-web-app-azure-resource-manager-powershell.md)
