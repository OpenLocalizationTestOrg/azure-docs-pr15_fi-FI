<properties
   pageTitle="Yleisiä Azure käyttöönoton vianmääritys | Microsoft Azure"
   description="Tässä artikkelissa käsitellään yleisten virheiden ratkaiseminen asentaessasi resurssien Azure Azure resurssien hallinnan avulla."
   services="azure-resource-manager"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   keywords="käyttöönottovirhe azure käyttöönottoa, ota käyttöön azure"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Vianmääritys yleisiä Azure käyttöönoton Azure resurssien hallinta

Tässä ohjeaiheessa kerrotaan, miten voit ratkaista joitakin yhteinen Azure käyttöönoton virheitä saattaa ilmetä. Jos tarvitset lisätietoja siitä, mikä meni vikaan käyttöönoton kanssa, katso ensin [Näytä käyttöönoton toiminnot](resource-manager-troubleshoot-deployments-portal.md) ja valitse palaa tämän artikkelin ohjeita virheen korjaamiseksi. Tässä ohjeaiheessa osat luettelo näkyy virhekoodi.

## <a name="invalid-template"></a>Virheellinen malli

Tämä virhe voi aiheuttaa virheitä useista erilaisista. 

### <a name="syntax-error"></a>Syntaksivirhe

Jos näyttöön tulee virhesanoma, joka ilmaisee mallia ei voitu vahvistus-syntaksin ongelma voi olla malliin. 

    Code=InvalidTemplate 
    Message=Deployment template validation failed

Tämä virhe on helppo tehdä koska mallin lausekkeet voivat olla monimutkaisia. Esimerkiksi seuraavat tallennustilan tilin nimi varauksen sisältää hakasulkeita joukkona, kolmea toimintoa, sulkeita kolmena, puolilainausmerkkeihin joukkona ja yhden ominaisuuden:

    "name": "[concat('storage', uniqueString(resourceGroup().id))]",

Jos et anna vastaavia syntaksi, mallin tuottaa arvo, joka on erilainen kuin aiot.

Kun saat tämän lajin virheitä, Tarkista huolellisesti lausekkeiden syntaksista. Harkitse JSON-editorin, kuten [Visual Studiossa](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) tai [Visual Studio koodi](resource-manager-vs-code.md), joka varoittaa, tietoja syntaksivirheitä. 

### <a name="incorrect-segment-lengths"></a>Virheellinen segmentin pituudet

Toisen Virheellinen malli-virhe ilmenee, kun resurssinimi ei ole oikeassa muodossa.

    Code=InvalidTemplate
    Message=Deployment template validation failed: 'The template resource {resource-name}' 
    for type {resource-type} has incorrect segment lengths.

Pääkansio Tasaa resurssit on oltava vähintään yksi segmentin kuin resurssin lajin sen nimen. Kunkin osion on eritellä vinoviiva mukaan. Seuraavassa esimerkissä tyyppi on kaksi osaa ja nimi on yksi segmentin, joten se on **kelvollinen nimi**.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "myHostingPlanName",
      ...
    }

Mutta seuraavassa esimerkissä on **nimi ei kelpaa** , koska se on sama määrä osia tyypiksi.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "appPlan/myHostingPlanName",
      ...
    }

Lapsen resurssien tyyppi ja nimi on sama määrä osia. Osia määrä on järkevää, koska koko nimi ja tyyppi lapsen sisältää ylemmän tason nimi ja tyyppi. Tämän vuoksi koko nimi on edelleen vähemmän kuin koko haluamasi osa. 

    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "contosokeyvault",
            ...
            "resources": [
                {
                    "type": "secrets",
                    "name": "appPassword",
                    ...
                }
            ]
        }
    ]

Käytön osat oikean voi olla vaikeaa Resurssienhallinta tiedostotyypit, joita käytetään yli resurssin tarjoajan kanssa. Esimerkiksi lukko resurssin soveltaminen sivuston edellyttää tyyppi, jossa on neljä osia. Tämän vuoksi nimi on kolme osia:

    {
        "type": "Microsoft.Web/sites/providers/locks",
        "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
        ...
    }

### <a name="copy-index-is-not-expected"></a>Kopioi indeksi ei odoteta

**InvalidTemplate** tämä virhe ilmenee, kun olet käyttänyt **Kopioi** osan osa malli, joka ei tue tämä elementti. Kopioi-osan voi käyttää vain resurssilaji. Et voi käyttää Kopioi ominaisuus sisällä resurssilaji. Kopioi sovelletaan virtual machine, mutta et voi käyttää sitä virtual tietokoneen OS levyjen. Joissakin tapauksissa voit muuntaa lapsen resurssin ylemmän tason resurssin kopioi silmukan luomiseen. Saat lisätietoja kopioimalla [luominen Azure resurssien hallinnan resurssit useita kertoja](resource-group-create-multiple.md).

### <a name="parameter-is-not-valid"></a>Parametri ei kelpaa

Jos malli määrittää sallitut parametrin arvot ja antaisit arvo, joka ei ole jokin näistä arvoista, näyttöön tulee sanoma, joka on samanlainen seuraavan virheilmoituksen:

    Code=InvalidTemplate; 
    Message=Deployment template validation failed: 'The provided value {parameter vaule} 
    for the template parameter {parameter name} is not valid. The parameter value is not 
    part of the allowed value(s)

Kaksinkertainen sallittu arvo mallin ja lisätä käyttöönoton aikana.

## <a name="not-found-or-resource-not-found"></a>Ei löydy (tai resurssia ei löydy)

Kun malliin sisältyy, joita ei voi selvittää resurssin nimi, näyttöön tulee virhesanoma:

    Code=NotFound;
    Message=Cannot find ServerFarm with name exampleplan.

Jos yrität puuttuvan resurssin mallin käyttöön, tarkista onko sinun on lisättävä riippuvuus. Resurssienhallinta optimoi käyttöönoton luomalla resurssien rinnakkain, kun se on mahdollista. Yksi resurssi on otettava käyttöön toiselle resurssille jälkeen, jos haluat luoda riippuvuuden muiden resurssin mallin **dependsOn** -osan avulla. Esimerkiksi web-sovelluksen ottaminen käyttöön, kun sovellus-palvelusopimus on olemassa. Jos et ole määrittänyt sovelluksen palvelusopimus määräytyy web app-Resurssienhallinta Luo molempien resurssien samanaikaisesti. Näyttöön tulee virhesanoma, jossa sanotaan, että sovelluksen palvelun suunnitelman resurssin ei löydy, koska sitä ei ole vielä ilmoittavaan ominaisuuden web Appissa. Tämä virhe estää määrittämällä riippuvuuden web App-sovelluksessa.

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }
 
Tämä virhe näkyy myös, kun resurssi sijaitsee eri resurssiryhmä kuin se otetaan käyttöön. Tässä tapauksessa täydellinen resurssin nimen [resourceId-funktion](resource-group-template-functions.md#resourceid) avulla.

    "properties": {
        "name": "[parameters('siteName')]",
        "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
    } 

Jos yrität [viittauksesta](resource-group-template-functions.md#referenc) tai [listKeys](resource-group-template-functions.md#listkeys) -funktioiden käyttäminen resurssi, joka ei voi selvittää, näyttöön tulee seuraava virhe:

    Code=ResourceNotFound;
    Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource 
    group {resource group name} was not found.

Etsi lauseke, joka sisältää **viittaus** -funktiota. Tarkista, että parametriarvot ovat oikein.

## <a name="storage-account-already-exists-or-already-taken"></a>Tallennustilan tili on jo olemassa (tai jo)

Tallennustilan-tileissä sinun on määritettävä resurssi, joka on yli Azure yksilöivä nimi. Jos et anna yksilöivä nimi, näyttöön tulee virhesanoma, kuten:

    Code=StorageAccountAlreadyTaken 
    Message=The storage account named mystorage is already taken.

Voit luoda oman nimeämiskäytäntöä [uniqueString](resource-group-template-functions.md#uniquestring) -funktion tuloksella ketjuttamalla yksilöivä nimi.

    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]",
    "type": "Microsoft.Storage/storageAccounts",

Jos ottaa tilauksen tallennustilan-tili on sama nimi kuin aiemmin luodun tallennustilan-tilin kanssa, mutta eri sijainnissa, näyttöön tulee virhesanoma, joka ilmoittaa tallennustilan-tili on jo olemassa eri sijainnissa. Poista olevan tallennustilan-tilin tai antaa olevan tallennustilan tilin samaan sijaintiin.

## <a name="account-name-invalid"></a>Tilin nimi ei kelpaa

Näet **AccountNameInvalid** virheen, kun yrität Anna tallennustilan tilin nimi, joka sisältää kiellettyjä merkkejä. Tallennustilan tilin nimi on 3 ja 24 merkkiä välillä ja käytä numerot ja pienet kirjaimet.

## <a name="no-registered-provider-found"></a>Ei löydy rekisteröity toimittaja

Kun otat resurssi, näyttöön voi tulla seuraava virhekoodi ja viestin:

    Code: NoRegisteredProviderFound
    Message: No registered resource provider found for location {ocation} 
    and API version {api-version} for type {resource-type}.

Näyttöön tulee seuraava virheilmoitus kolme syistä:

1. Resurssin laji ei tue sijainti
2. API-versio ei tue resurssin laji
3. Resurssin palvelu ei ole rekisteröity tilauksen

Virhesanoma tulisi ilmoittaa sinulle ehdotuksia tuetut sijainnit ja API versiot. Voit muuttaa mallin johonkin ehdotettu arvo. Useimmat palveluntarjoajat on rekisteröity automaattisesti Azure portaalin tai käytät komentorivivalitsimet käyttöliittymän mukaan, mutta ei kaikkiin. Jos et ole käyttänyt tietyn resurssin palveluntarjoaja ennen, joudut ehkä rekisteröidä toimittajaan. Voit tutustua lisätietoja resurssin tarjoajat PowerShell tai Azure CLI kautta.

### <a name="powershell"></a>PowerShellin

Jos haluat nähdä rekisteröinti tilan, käytä **Get-AzureRmResourceProvider**.

    Get-AzureRmResourceProvider -ListAvailable

Voit rekisteröidä palveluntarjoaja, käytä **Rekisteröi AzureRmResourceProvider** ja haluat rekisteröidä resurssin tarjoajan nimi.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

Hanki tuetut sijainnit resurssin tietyn tyyppisiä, käytä:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Jos haluat tietyn tyyppiseen resurssin tuetut API-versiot, käytä:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

### <a name="azure-cli"></a>Azure CLI

Voit tarkistaa, onko palvelu, `azure provider list` komento.

    azure provider list

Voit rekisteröidä resurssin toimittajan `azure provider register` komento ja määritä *nimitilan* rekisteröidä.

    azure provider register Microsoft.Cdn

Voit tuoda tuetut sijainnit ja API-versioiden resurssi-palvelun avulla:

    azure provider show -n Microsoft.Compute --json > compute.json

## <a name="operation-not-allowed"></a>Toiminto ei ole sallittu

Voit joutua ongelmat käyttöönoton ylittää kiintiön, jotka voivat olla resurssiryhmä, tilaukset, tilien ja muiden käyttöalueen kohden. Tilauksen voi esimerkiksi määrittää alueen sydämiä määrän rajoittaminen. Jos yrität käyttöön virtual tietokoneessa, jossa Lisää sydämiä kuin sallittu summa, näyttöön tulee virhesanoma, jossa sanotaan, kiintiö on ylitetty.
Valmis kiintiö on artikkelissa [Azure tilaus ja palvelun rajoitukset, kiintiöiden ja rajoitukset](azure-subscription-service-limits.md).

Voit tarkistaa tilauksen kiintiöt sydämiä `azure vm list-usage` Azure-CLI komento. Seuraavassa esimerkissä havainnollistetaan core kiintiön ilmainen kokeiluversio tili on 4:

    azure vm list-usage

Joka palauttaa:

    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

Jos otat käyttöön malli, joka luo enintään neljä sydämiä Länsi US alueen, näyttöön tulee käyttöönottovirhe, joka näyttää tältä:

    Code=OperationNotAllowed
    Message=Operation results in exceeding quota limits of Core. 
    Maximum allowed: 4, Current in use: 4, Additional requested: 2.

Tai powershellissä, voit käyttää **Get-AzureRmVMUsage** cmdlet-komento.

    Get-AzureRmVMUsage

Joka palauttaa:

    ...
    CurrentValue : 0
    Limit        : 4
    Name         : {
                     "value": "cores",
                     "localizedValue": "Total Regional Cores"
                   }
    Unit         : null
    ...

Tällaisissa tapauksissa kannattaa portaalista ja arkistoida tukipyyntö nostaa kiintiön alue, johon haluat ottaa käyttöön.

> [AZURE.NOTE] Muista, että resurssin ryhmien-kiintiön yksittäisiä alueittain, ei koko tilausta varten. Jos haluat ottaa käyttöön 30 sydämiä Länsi Yhdysvalloissa, sinun on 30 Resurssienhallinta sydämiä Länsi Yhdysvalloissa Kysy. Jos haluat ottaa käyttöön 30 sydämiä kaikista alueet, joihin voit käyttää, kysy 30 Resurssienhallinta sydämiä kaikilla alueilla.

## <a name="invalid-content-link"></a>Virheellinen sisältö-linkki

Kun näyttöön tulee virhesanoma:

    Code=InvalidContentLink
    Message=Unable to download deployment content from ...

Yritit todennäköisesti linkittäminen sisäkkäisiä malli, jota ei ole käytettävissä. Tarkista URI antamasi sisäkkäisiä mallin. Jos malli on tallennustilan tilillä, varmista, että URI on käytettävissä. Voit joutua lähettämään SAS-tunnuksen. Lisätietoja on artikkelissa [linkitetyn mallien Azure resurssien hallinnan avulla](resource-group-linked-templates.md).

## <a name="authorization-failed"></a>Todennus epäonnistui

Käyttöönoton aikana saattaa tulla virhesanoma, koska asiakas- tai palvelun pääasiallista yrittää ottaa resursseja ei ole käyttöoikeutta, voivat suorittaa näitä toimia. Azure Active Directory mahdollistaa tai järjestelmänvalvoja määrittää, mitkä käyttäjätietojen voi käyttää mitä resurssit, joiden tarkkuutta hyvien aste. Esimerkiksi jos tiliisi on määritetty lukija, et voi luoda resurssit. Tällöin näet virhe, joka ilmaisee, että luvan epäonnistui.

Saat lisätietoja Roolipohjainen käyttöoikeuksien valvonta [Azure Role-Based käyttöoikeuksien hallinta](./active-directory/role-based-access-control-configure.md).

Lisäksi Roolipohjainen käyttöoikeuksien valvonta-toimintojen käyttöönotto voidaan rajoittaa tilaus hallintakäytäntöjä. Järjestelmänvalvoja pakottaa kautta käytäntöjä nimeämiskäytännön kaikkien resurssien tilauksen käyttöön. Esimerkiksi järjestelmänvalvoja voi vaatia, että tietty tunniste-arvo on tarkoitettu resurssilaji. Jos ei täytettävä käytännön vaatimukset, näyttöön tulee virhesanoma, kun käyttöönoton aikana. Saat lisätietoja käytännöt [Käyttöä koskevasta käytännöstä resurssien hallintaa ja hallita](resource-manager-policy.md).

## <a name="troubleshooting-virtual-machines"></a>Näennäiskoneiden vianmääritys

| Virhe | Artikkelit |
| -------- | ----------- |
| Mukautettu komentosarja tunniste virheet | [Windowsin AM tunniste virheet](./virtual-machines/virtual-machines-windows-extensions-troubleshoot.md)<br />tai<br />[Linux AM tunniste virheet](./virtual-machines/virtual-machines-linux-extensions-troubleshoot.md) |
| Valmisteluvirheitä OS kuva | [Uuden Windows AM virheet](./virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md)<br />tai<br />[Uusi Linux AM virheet](./virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md ) |
| Kohdistus-virheet | [Windowsin AM kohdistus virheet](./virtual-machines/virtual-machines-windows-allocation-failure.md)<br />tai<br />[Linux AM kohdistus virheet](./virtual-machines/virtual-machines-linux-allocation-failure.md) |
| Suojattu runko (SSH) virheet muodostettaessa | [Suojattu runko yhteydet Linux AM](./virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md) |
| Yhteyden muodostaminen AM-sovelluksen virheet | [Windowsin AM-sovelluksen](./virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md)<br />tai<br />[Linux AM-sovelluksen](./virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md) |
| Etätyöpöytäyhteys virheet | [Windowsin AM Remote työpöydän yhteydet](./virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md) |
| Yhteyden virheet ovat mallirakenteeseen | [Käyttöön virtuaalikoneen uusi Azure solmu](./virtual-machines/virtual-machines-windows-redeploy-to-new-node.md) |
| Cloud service-virheet | [Cloud palvelun käyttöönoton ongelmia](./cloud-services/cloud-services-troubleshoot-deployment-problems.md) |

## <a name="troubleshooting-other-services"></a>Muiden palvelujen vianmääritys

Seuraavassa taulukossa ei ole luettelo kaikista aiheista vianmääritysohjeita Azure. Sen sijaan se keskitytään käyttöönottoon tai resurssien määrittäminen liittyvät ongelmat. Jos tarvitset apua suorituksenaikainen resurssiin liittyvien ongelmien vianmääritys, Azure palvelun ohjeissa.

| Palvelun | Artikkelissa |
| -------- | -------- |
| Automaatio | [Vianmääritysvihjeitä Azure automaatio Yleiset virheet](./automation/automation-troubleshooting-automation-errors.md) |
| Azure pino | [Microsoft Azure pinon vianmääritys](./azure-stack/azure-stack-troubleshooting.md) |
| Azure pino | [Web-sovellusten ja Azure pino](./azure-stack/azure-stack-webapps-troubleshoot-known-issues.md ) |
| Tietoja Factory | [Data Factory-ongelmien vianmääritys](./data-factory/data-factory-troubleshoot.md) |
| Palvelun kangasta | [Yleisten ongelmien vianmääritys, kun otat käyttöön Azure palvelun kangasta palvelut](./service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) |
| Sivuston palauttaminen | [Valvo ja suojauksen näennäiskoneiden ja fyysiset palvelimet vianmääritys](./site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Tallennustilan | [Valvonta, diagnosointi ja vianmääritys Microsoft Azuren tallennustilaan](./storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple | [StorSimple laitteen käyttöönoton ongelmien vianmääritys](./storsimple/storsimple-troubleshoot-deployment.md) |
| SQL-tietokantaan | [Yhteyden muodostaminen Azure SQL-tietokantaan ongelmien vianmääritys](./sql-database/sql-database-troubleshoot-common-connection-issues.md) |
| SQL-tietovarasto | [Azure SQL-tietovarasto vianmääritys](./sql-data-warehouse/sql-data-warehouse-troubleshoot.md) |

## <a name="understand-when-a-deployment-is-ready"></a>Kun käyttöönoton on valmiina ymmärrä

Azure Resurssienhallinta raportit success käyttöönoton kun kaikkien palveluntarjoajien tuotto käyttöönoton onnistuneesti. Kuitenkin viestiä ei välttämättä tarkoita, että resurssiryhmä on "aktiivinen ja valmis käyttäjien." Esimerkiksi käyttöönoton ehkä ladata päivityksiä, odota malli resursseja tai asentaa monimutkaisia komentosarjoja. Resurssienhallinta ei tiedä, kun näiden tehtävien suorittaa, koska ne eivät ole toimintoja, jotka palveluntarjoaja seuraa. Tällaisissa tapauksissa se voi olla jonkin aikaa, ennen kuin resurssit ovat käyttövalmiina tosielämän. Tuloksena on todennäköisesti käyttöönoton tilan onnistuu jonkin aikaa, ennen kuin käyttöönoton voi käyttää.

Voit estää Azure raportoinnin käyttöönotto onnistui, mutta luomalla mukautetun komentosarjan mukautetun mallin. Komentosarja tietää, kuinka voit valvoa järjestelmää valmiuden koko käyttöönoton ja palauttaa onnistuu vain, kun käyttäjät voivat käsitellä koko käyttöönotto. Jos haluat varmistaa, että alanumerosi on viimeiseksi Suorita-mallissa **dependsOn** -ominaisuudella.

## <a name="next-steps"></a>Seuraavat vaiheet

- Tietoja tarkistaminen toiminnot-kohdassa [valvonta toimintojen resurssien hallinta](resource-group-audit.md).
- Tietoja määrittämään virheet käyttöönoton aikana toiminnot-kohdassa [Näytä käyttöönoton toiminnot](resource-manager-troubleshoot-deployments-portal.md).
