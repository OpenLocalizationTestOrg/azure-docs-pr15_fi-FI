<properties 
    pageTitle="Azure PowerShell resurssien hallinnan | Microsoft Azure" 
    description="Johdanto PowerShellin Azure avulla voit ottaa käyttöön useita resursseja kuin Azure resurssiryhmä." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="tomfitz"/>

# <a name="using-azure-powershell-with-azure-resource-manager"></a>Azure PowerShell kanssa Azure resurssien hallinnan avulla

> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST-OHJELMOINTIRAJAPINNALLA](resource-manager-rest-api.md)


Azure Resurssienhallinta toteuttaa Moderni lähestymistapa Azure resurssien elinkaari-ohjausobjektiin. Sen sijaan, että luomisen ja ylläpidon yksittäisiä resursseja, Aloita määrittämällä kuvittelemassa koko ratkaisun, kuten blogia, valokuvavalikoima, SharePoint portal tai Wikin. Mallin--ratkaisun määritettäviä esitys--avulla voit määrittää resurssiryhmä, joka sisältää kaikki tarvittavat resurssit tukemaan ratkaisu. Sitten ottaminen käyttöön ja hallita looginen yksikkönä, resurssiryhmä. 

Tässä opetusohjelmassa kerrotaan Azure PowerShellin Azure resurssien hallinnan käyttäminen. Se käydään läpi-ratkaisun käyttöönotto ja kyseisen ratkaisun käsitteleminen. Käytät PowerShellin Azure ja Resurssienhallinta mallin käyttöön:

- SQL server - tietokannan isännöimiseen
- SQL-tietokanta - tietojen tallennusta varten
- Palomuurisäännöt - sallimaan web app tietokantayhteyden muodostamisessa
- Sovelluksen palvelusopimus - ominaisuuksia ja kustannukset web Appin määrittämistä varten
- Web-sivuston - web-sovellusta
- Web-config - tietokantaan yhteysmerkkijonon tallentamista varten 
- Ilmoitusten säännöt - seuranta suorituskykyä ja virheiden ratkaiseminen
- Sovelluksen tietoa - automaattinen mittakaava-asetuksia

## <a name="prerequisites"></a>Edellytykset

Jotta voit suorittaa tässä opetusohjelmassa, sinun on:

- Azure-tili
  + Voit [avata Azure-tili maksutta](/pricing/free-trial/?WT.mc_id=A261C142F): Saat hyvitykset avulla voit kokeilla maksettu Azure services ja senkin jälkeen, kun niitä käytetään enintään voit pitää tilin ja käyttää vapaa Azure-palvelut, kuten sivustot. Luottokorttisi koskaan veloitetaan, ellet nimenomaisesti muuttaminen ja pyydä veloitetaan.
  
  + Voit [aktivoida MSDN tilaajan edut](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Your MSDN-tilauksen käyttöösi hyvitykset joka kuukausi, jonka maksettu Azure-palveluiden avulla.
- Azure PowerShell 1.0. Tietoja tässä versiossa ja asennusohjeet Katso, [miten asennetaan ja määritetään PowerShellin Azure](powershell-install-configure.md).

Tässä opetusohjelmassa on suunniteltu PowerShell aloittelijoille, mutta se olettaa ymmärtää peruskäsitteet, kuten moduulit, cmdlet-komennot ja istuntoja.

## <a name="get-help-for-cmdlets"></a>Hanki ohjeita cmdlet-komennot

Saat yksityiskohtaisia ohjeita cmdlet, joissa kerrotaan tässä opetusohjelmassa, hanki ohjeita cmdlet-komennon avulla 

    Get-Help <cmdlet-name> -Detailed

Saat ohjeita Get-AzureRmResource cmdlet-komento, esimerkiksi:

    Get-Help Get-AzureRmResource -Detailed

Saat apua yhteenvedon resurssien moduulia cmdlet-komentojen luettelo kirjoittamalla 

    Get-Command -Module AzureRM.Resources | Get-Help | Format-Table Name, Synopsis

Tulos näyttää seuraavat ote samalla:

    Name                                   Synopsis
    ----                                   --------
    Find-AzureRmResource                   Searches for resources using the specified parameters.
    Find-AzureRmResourceGroup              Searches for resource group using the specified parameters.
    Get-AzureRmADGroup                     Filters active directory groups.
    Get-AzureRmADGroupMember               Get a group members.
    ...

Saat koko ohjeita cmdlet-komento, kirjoita muodossa-komento:

    Get-Help <cmdlet-name> -Full
  
## <a name="login-to-your-azure-account"></a>Azure-tiliisi kirjautuminen

Ennen kuin käsittelet ratkaisu, sinun täytyy tiliisi kirjautuminen.

Kirjautuminen Azure-tiliin, **Lisää AzureRmAccount** cmdlet-komennon avulla

    Add-AzureRmAccount

Cmdlet pyytää Azure-tilin kirjautumistiedot. Jälkeen kirjautumisesta, se lataa-käyttäjätilin asetusten, jotta ne ovat käytettävissä PowerShellin Azure. 

Voimassaolo päättyy tilin asetukset, joten sinun täytyy päivittää toisinaan. Päivitä tilin asetukset, suorita **Lisää AzureRmAccount** uudelleen. 

>[AZURE.NOTE] Resurssienhallinta-moduulit edellyttää lisää AzureRmAccount. Julkaisuasetukset-tiedosto ei ole tarpeeksi.     

Jos sinulla on useita tilauksia, antaa Tilaustunnus, haluatko käyttää käyttöönoton **Määrittäminen AzureRmContext** cmdlet-komento.

    Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

## <a name="create-a-resource-group"></a>Resurssiryhmän luominen

Ennen kuin otat resurssien tilaukseen, sinun on luotava, joka sisältää resursseja resurssiryhmän. 

Voit luoda resurssiryhmä käyttämällä **Uutta AzureRmResourceGroup** cmdlet-komento.

Komennon resurssiryhmän ja määrittää sen sijaintiin **sijainnin** parametrin nimi **nimi** -parametrin avulla. Perusteella olemme havaitsi edellisessä osassa Käytämme "Länsi USA" sijainnin.

    New-AzureRmResourceGroup -Name TestRG1 -Location "West US"
    
Tulosteesta tulee seuraavanlainen:

    ResourceGroupName : TestRG1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1

Resurssiryhmä on luotu.

## <a name="deploy-your-solution"></a>-Ratkaisun käyttöönotto

Tässä ohjeaiheessa ei näy mallin luomisesta ja keskustella mallin rakenne. Katso nämä tiedot [yhtä aikaa muiden kanssa Azure Resurssienhallinta mallit](resource-group-authoring-templates.md) ja [Resurssienhallinta mallin ongelmatilanteita](resource-manager-template-walkthrough.md). Voit ottaa käyttöön ennalta määritetyt [säännöstä Web-sovelluksessa, jossa on SQL-tietokanta](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) -mallin [Azure pikaopas](https://azure.microsoft.com/documentation/templates/)malleista.

Sinulla on resurssiryhmän ja sinulla on malliin, jotta voit nyt valmis ottamaan resurssiryhmä mallin määritelty infrastruktuuri. Voit ottaa käyttöön **Uusi AzureRmResourceGroupDeployment** cmdlet-komento resursseilla. Malli määrittää useita oletusarvot, jotka Käytämme, joten sinun ei tarvitse antaa näiden parametrien arvot. Tavallinen syntaksi näyttää seuraavanlaiselta:

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -administratorLogin exampleadmin -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

Määritä resurssiryhmän ja mallin sijainti. Jos malli on paikallinen tiedosto, käytä **- TemplateFile** parametri ja määritä mallin polku. Voit määrittää **-tilassa** parametrin **vaiheittainen** tai **Valmis**. Oletusarvon mukaan Resurssienhallinta suorittaa lisäävän päivityksen aikana käyttöönoton; Tämän vuoksi ei ole välttämätöntä voit määrittää **-tilassa** milloin haluat **vaiheittainen**. Käyttöönoton kolmesta tilasta erot on artikkelissa [Azure Resurssienhallinta mallilla sovelluksen käyttöönotto](resource-group-template-deploy.md). 

###<a name="dynamic-template-parameters"></a>Dynaamisen Malliparametrit

Jos olet tutustunut PowerShellin, tiedät, että voit käy läpi cmdlet-komento käytettävissä parametrit kirjoittamalla miinusmerkki (-) ja painamalla sitten SARKAIN-näppäintä. Sama toiminto toimii myös mallin määrittävät parametrit. Heti, kun kirjoitat mallin nimi, cmdlet hakee mallin, se jäsentää ja lisää Malliparametrit komento dynaamisesti. Tämä on hyvin helppo mallin parametrin arvot.

Kun kirjoitat komento, sinua kehotetaan puuttuu pakollinen parametrin **administratorLoginPassword**. Ja, kun kirjoitat salasanan, suojattu merkkijonoarvo jää alle. Tämä strategia jättää pois riskiä tekstimuodossa salasanaa.

    cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    administratorLoginPassword: ********

Jos malli sisältää parametri, jonka nimi on sama jokin ottamaan malli (kuten myös **ResourceGroupName** mallisi, joka on sama kuin [Uusi AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) cmdlet-komento **ResourceGroupName** -parametrin parametrin)-komentoa, voit pyydetään antamaan arvon parametrin postfix **FromTemplate** (kuten **ResourceGroupNameFromTemplate**) kanssa. Yleensä kannattaa välttää tämän sekaannusta nimeämällä parametrit ei ole samannimistä parametreiksi käytettävien toimintojen käyttöönotto.

Komento suoritetaan, ja palauttaa viestejä, koska resurssit luodaan. Kädessä näet käyttöönoton tulos.

    DeploymentName    : azuredeploy
    ResourceGroupName : TestRG1
    ProvisioningState : Succeeded
    Timestamp         : 4/11/2016 7:26:11 PM
    Mode              : Incremental
    TemplateLink      :
                Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json
                ContentVersion : 1.0.0.0
    Parameters        :
                Name             Type                       Value
                ===============  =========================  ==========
                skuName          String                     F1
                skuCapacity      Int                        1
                administratorLogin  String                  exampleadmin
                administratorLoginPassword  SecureString
                databaseName     String                     sampledb
                collation        String                     SQL_Latin1_General_CP1_CI_AS
                edition          String                     Basic
                maxSizeBytes     String                     1073741824
                requestedServiceObjectiveName  String       Basic

    Outputs           :
                Name             Type                       Value
                ===============  =========================  ==========
                siteUri          String                     websites5wdai7p2k2g4.azurewebsites.net
                sqlSvrFqdn       String                     sqlservers5wdai7p2k2g4.database.windows.net
                    
    DeploymentDebugLogLevel :

Vain muutama vaihe luodaan ja ottaa käyttöön monimutkaisia verkkosivuston tarvittavat resurssit. 

### <a name="log-debug-information"></a>Virheenkorjaus lokitiedot

Kun otat mallin käyttöön, voit kirjautua lisätietoja pyynnön ja vastauksen määrittämällä **- DeploymentDebugLogLevel** parametri, kun **Uusi AzureRmResourceGroupDeployment**käynnissä. Nämä tiedot auttavat käyttöönoton vianmääritys. Oletusarvo on **ei mitään** toisin sanoen pyyntöä tai vastauksen sisältö on kirjautunut. Voit määrittää kirjaamisen sisällön pyynnön ja vastauksen.  Saat lisätietoja vianmääritys ominaisuuksissa ja vianmäärityksen lisätietoja kirjaamisen [vianmääritys resurssin ryhmän ominaisuuksissa Azure PowerShellin avulla](resource-manager-troubleshoot-deployments-powershell.md). Seuraavassa esimerkissä kirjaa sisällön pyynnön ja vastauksen sisällön käyttöönoton.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -DeploymentDebugLogLevel All -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

> [AZURE.NOTE] Kun DeploymentDebugLogLevel-parametrin määrittäminen Mieti tietotyyppi on kulkeva käyttöönoton yhteydessä. Kirjaaminen tietoja pyynnön ja vastauksen, joita voi mahdollisesti Näytä luottamuksellisia tietoja, jotka noudetaan käyttöönotto-toimintojen avulla. 


## <a name="get-information-about-your-resource-groups"></a>Tietojen resurssiryhmien tarkasteleminen

Kun olet luonut resurssiryhmä, resurssiryhmien hallinta Cmdlet-komentoja Resurssienhallinta-moduulin avulla.

- Jos haluat resurssiryhmä-tilaukseesi, käytä **Get-AzureRmResourceGroup** cmdlet-komento:

        Get-AzureRmResourceGroup -ResourceGroupName TestRG1
    
    Joka palauttaa seuraavat tiedot:

        ResourceGroupName : TestRG1
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG
        
        ...

    Jos et määritä resurssin ryhmänimi, cmdlet-komento palauttaa kaikki resurssiryhmät tilauksen.

- Pääset resursseja resurssiryhmän käyttää **Etsi AzureRmResource** cmdlet-komennosta ja sen **ResourceGroupNameContains** -parametria. Ilman parametreja Etsi AzureRmResource saa kaikkien resurssien Azure-tilaukseesi.

        Find-AzureRmResource -ResourceGroupNameContains TestRG1
    
     Joka palauttaa luettelon resurssien rekisterissä, kuten:
        
        Name              : sqlservers5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Sql/servers/sqlservers5wdai7p2k2g4
        ResourceName      : sqlservers5wdai7p2k2g4
        ResourceType      : Microsoft.Sql/servers
        Kind              : v2.0
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
        Tags              : {System.Collections.Hashtable}
        ...
            
- Voit käyttää tunnisteiden loogisesti järjestää resurssit-tilaukseesi ja hakea **Etsi AzureRmResource** ja **Etsi AzureRmResourceGroup** cmdlet-komennot resursseilla.

        Find-AzureRmResource -TagName displayName -TagValue Website

        Name              : webSites5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Web/sites/webSites5wdai7p2k2g4
        ResourceName      : webSites5wdai7p2k2g4
        ResourceType      : Microsoft.Web/sites
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
                
      There is much more you can do with tags. For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).

## <a name="add-to-a-resource-group"></a>Resurssin lisääminen ryhmään

Jos haluat lisätä resurssin resurssiryhmän, voit käyttää **Uusi AzureRmResource** cmdlet-komento. Kuitenkin lisääminen resurssin tällä tavalla voi aiheuttaa tulevien sekaannusta koska mallin ei ole uusi resurssi. Jos olet asentanut uudelleen vanhan mallin, keskeneräiseen ratkaisu käyttöönottoa. Jos otat usein, löydät sen helpompaa ja luotettava Lisää uusi resurssi mallin ja asentaa se uudelleen.

## <a name="move-a-resource"></a>Siirrä resurssi

Voit siirtää olemassa olevat resurssien uusi resurssiryhmä. Katso esimerkkejä, [Siirrä resurssien uusi resurssiryhmä ja tilauksen](resource-group-move-resources.md).

## <a name="export-template"></a>Vie malli

Aiemmin luodun resurssiryhmän (kautta PowerShell- tai muita menetelmiä kuten portaalin jokin käyttöön) voit tarkastella resurssiryhmän Resurssienhallinta-malli. Mallin vieminen on kaksi edut:

1. Voit automatisoida ratkaisun tulevien versioiden helposti, koska kaikki infrastruktuurin on määritetty malli.

2. Voit tutustut mallin syntaksi katsomalla osoitteessa JavaScript Object Notation (JSON), joka edustaa ratkaisu.

Powershellissä voit joko Luo mallin, joka vastaa resurssiryhmä nykyisen tilan tai hakea tietyn käyttöönottoa käytetty malli.

Resurssiryhmä mallin vieminen on hyötyä, kun olet tehnyt muutoksia resurssiryhmä ja hakeminen nykyisestä tilasta JSON-esitys. Kuitenkin luotu mallissa on vain vähän parametrit ja muuttujien ei ole. Arvot-mallissa on koodattu. Ennen kuin otat luotu mallin, haluat ehkä muuntaa Lisää arvot parametrit niin voit mukauttaa eri ympäristöissä käyttöönotto.

Tietyn käyttöönottoa mallin vieminen on hyötyä, kun haluat tarkastella todellinen mallin, jota käytettiin ottamaan resurssit. Malli sisältää kaikki parametrit ja määritetyn alkuperäisen käyttöönottoa varten. Kuitenkin jos joku muu organisaatiossa on tehnyt muutoksia resurssiryhmä ulkopuolella, mikä on määritetty malli, tämä malli ei vastaa resurssiryhmän nykyisen tilan.

> [AZURE.NOTE] Mallin vientitoiminto on esikatselu ja kaikki resurssityypit tällä hetkellä tueta vieminen mallina. Kun yrität viedä mallin, näkyviin voi tulla virhe, joka ilmoittaa resurssien ei viety. Tarvittaessa voit manuaalisesti määrittää nämä resurssit malliin jälkeen ladata sen verkosta.

###<a name="export-template-from-resource-group"></a>Mallin vieminen resurssiryhmä

Resurssiryhmä mallin katselemista **Vie AzureRmResourceGroup** cmdlet-komennon suorittaminen

    Export-AzureRmResourceGroup -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\TestRG1.json
    
###<a name="download-template-from-deployment"></a>Mallin lataaminen käyttöönotto

Voit ladata tiettyyn käyttöönottoa malli- **Tallenna AzureRmResourceGroupDeploymentTemplate** cmdlet-komennon suorittaminen

    Save-AzureRmResourceGroupDeploymentTemplate -DeploymentName azuredeploy -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\azuredeploy.json

## <a name="delete-resources-or-resource-group"></a>Poista resursseja tai resurssiryhmä

- Resurssin poistaminen resurssiryhmän, **Poista AzureRmResource** cmdlet-komennon avulla Tämä cmdlet-komento poistaa resurssi, mutta ei poista resurssiryhmän.

    Tämä komento poistaa TestSite sivuston TestRG1 resurssiryhmä.

        Remove-AzureRmResource -Name TestSite -ResourceGroupName TestRG1 -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01

- Voit poistaa resurssiryhmä-sovelluksella **Poista AzureRmResourceGroup** cmdlet-komento. Tämä cmdlet-komento poistaa resurssiryhmän ja resursseja.

        Remove-AzureRmResourceGroup -Name TestRG1
        
    Sinua pyydetään vahvistamaan poiston.
        
        Confirm
        Are you sure you want to remove resource group 'TestRG1'
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

## <a name="deployment-script"></a>Käyttöönoton komentosarja

Tämän ohjeaiheen osoittanut vain yksittäisiä cmdlet-komennot aiemmissa käyttöönoton esimerkeissä tarvittavat resurssit käyttöön Azure. Seuraavassa esimerkissä esitetään käyttöönoton komentosarja, joka luo resurssiryhmän ja ottaa käyttöön resurssit.

    <#
      .SYNOPSIS
      Deploys a template to Azure
      
      .DESCRIPTION
      Deploys an Azure Resource Manager template

      .PARAMETER subscriptionId
      The subscription id where the template will be deployed.

      .PARAMETER resourceGroupName
      The resource group where the template will be deployed. Can be the name of an existing or a new resource group.

      .PARAMETER resourceGroupLocation
      Optional, a resource group location. If specified, will try to create a new resource group in this location. If not specified, assumes resource group is existing.

      .PARAMETER deploymentName
      The deployment name.

      .PARAMETER templateFilePath
      Optional, path to the template file. Defaults to template.json.

      .PARAMETER parametersFilePath
      Optional, path to the parameters file. Defaults to parameters.json. If file is not found, will prompt for parameter values based on template.
    #>

    param(
      [Parameter(Mandatory=$True)]
      [string]
      $subscriptionId,

      [Parameter(Mandatory=$True)]
      [string]
      $resourceGroupName,

      [string]
      $resourceGroupLocation,

      [Parameter(Mandatory=$True)]
      [string]
      $deploymentName,

      [string]
      $templateFilePath = "template.json",

      [string]
      $parametersFilePath = "parameters.json"
    )

    #******************************************************************************
    # Script body
    # Execution begins here 
    #******************************************************************************
    $ErrorActionPreference = "Stop"

    # sign in
    Write-Host "Logging in...";
    Add-AzureRmAccount;

    # select subscription
    Write-Host "Selecting subscription '$subscriptionId'";
    Set-AzureRmContext -SubscriptionID $subscriptionId;

    #Create or check for existing resource group
    $resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
    if(!$resourceGroup)
    {
      Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
      if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
      }
      Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
      New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
    }
    else{
      Write-Host "Using existing resource group '$resourceGroupName'";
    }

    # Start the deployment
    Write-Host "Starting deployment...";
    if(Test-Path $parametersFilePath) {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
    } else {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
    }

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Resurssienhallinta-mallien luomisesta on artikkelissa [Azure Resurssienhallinta-mallien yhtä aikaa muiden kanssa](./resource-group-authoring-templates.md).
- Lisätietoja mallien käyttämisessä on artikkelissa [Azure Resurssienhallinta mallilla sovelluksen käyttöönotto](./resource-group-template-deploy.md).
- Esimerkki projektin otat käyttöön Katso [käyttöönotto microservices laadukkaampi Azure](app-service-web/app-service-deploy-complex-application-predictably.md).
- Tietoja vianmäärityksestä, ei onnistunut käyttöönotto-kohdassa [vianmääritys resurssin ryhmän ominaisuuksissa Azure-tietokannassa](./resource-manager-troubleshoot-deployments-powershell.md).

