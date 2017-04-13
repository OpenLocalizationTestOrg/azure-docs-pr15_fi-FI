<properties
   pageTitle="Kehittäminen ja testaa ympäristöissä | Microsoft Azure"
   description="Opettele Azure Resurssienhallinta mallien avulla nopeasti ja johdonmukaisesti luominen ja poistaminen kehittämisen ja testaa ympäristöissä."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="01/22/2016"
   ms.author="tomfitz"/>

# <a name="development-and-test-environments-in-microsoft-azure"></a>Kehitys- ja Microsoft Azure-Ympäristöt

Mukautetut sovellukset yleensä on otettu käyttöön useita kehitystä ja testausta ympäristöissä ennen käyttöönottoa tuotannon. Kun paikallinen luodaan ympäristöissä, tietojenkäsittely resursseja joko hankittu tai varattu kunkin sovelluksen kussakin ympäristössä. Ympäristöjen sisältävät usein useita fyysisiä tai näennäiskoneiden tiettyjä määrityksiä, jotka on otettu manuaalisesti tai monimutkaisia Automaatiokomentosarjojen kanssa. Usein ominaisuuksissa kestää tuntia ja johtaa epäyhtenäinen käyttömahdollisuudet kaikissa ympäristöissä.

## <a name="scenario"></a>Skenaario ##

Kun olet valmistella kehittäminen ja testaa ympäristöissä: Microsoft Azure-maksat vain käyttämiisi.  Tässä artikkelissa kerrotaan, kuinka nopeasti ja johdonmukaisesti voit luoda, säilyttää, ja poista kehittämisen ja testaa ympäristöissä Azure Resurssienhallinta-mallien ja -parametrin tiedostojen käytön jäljempänä kuvatulla.

![Skenaario](./media/solution-dev-test-environments/scenario.png)

Kolme kehittäminen ja testauksen ympäristöissä näkyvät yläpuolella.  Web-sovelluksen ja SQL tietokanta resurssit, jotka on määritetty mallitiedosto on.  Sovelluksen ja tietokannan kunkin ympäristössä nimet ovat erilaisia ja määritetään yksilöivä parametritiedostot kutakin ympäristöä varten.  

Jos et ole tottunut Azure Resurssienhallinta käsitteitä, on suositeltavaa lukea [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md) on artikkelissa ennen tämän artikkelin lukeminen.

Haluat ehkä ensin käymällä läpi vaiheet tässä artikkelissa kuvatulla tavalla lukematta viitatun artikkelit ja myönnä nopeasti kokemusta Azure Resurssienhallinta mallien avulla. Jälkeen kun olet toiminut vaiheet, jonka voi vastauksia kysymyksiin, jotka on syntynyt etu useimmat aika – lisätyötä edelleen vaiheisiin ja lukemalla viitatun artikkelit.

## <a name="plan-azure-resource-use"></a>Azure resurssien käytön suunnitteleminen
Kun olet korkean tason rakenne sovelluksen, voit määrittää:

- Sovelluksen sisällytetään Azure resursseja. Ehkä sovellus muodosta ja ota se käyttöön Azure Web App, jossa Azure SQL-tietokanta nimellä.  Voi luoda sovelluksen näennäiskoneiden PHP ja MySQL IIS ja SQL Server-ja muita osia. [Azure-sovelluksen palvelu, Cloud Services ja näennäiskoneiden vertailu]( app-service-web/choose-web-site-cloud-service-vm.md) -artikkelissa neuvotaan saatat haluta käyttää sovelluksen Azure resursseja.
- Mitä palvelun tason tarpeita esimerkiksi käytettävyyden, suojaus ja mittakaava, jotka vastaavat sovelluksen.

## <a name="download-an-existing-template"></a>Lataa aiemmin luotu malli
Azure Resurssienhallinta-malli määrittää kaikki Azure resurssit, joka käyttää sovellusta. Useita malleja on jo olemassa, että voit ottaa suoraan Azure-portaaliin, tai Lataa, muokata ja tallentaa tietolähteen ohjausobjektin järjestelmän sovelluksen koodilla.  Suorita seuraavia ohjeita voit ladata aiemmin määritettyä mallia.

1. Siirry [Azure pikaopas malleja](https://github.com/Azure/azure-quickstart-templates/) GitHub säilössä mallia. Valitse luettelosta näet "[201-web-sovelluksen-sql-tietokannan](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)"-kansioon. Koska monet mukautetut sovellukset ovat web-sovelluksen ja SQL-tietokantaan, tätä mallia käytetään esimerkkinä jäljempänä tässä artikkelissa voi auttaa sinua ymmärtämään, kuinka voit käyttää malleja. Tässä artikkelissa kerrotaan täysin kaikki tämän mallin Luo ja määrittää laajemmin on, mutta jos haluat luoda todellinen ympäristöissä organisaation sen avulla, kannattaa ymmärtänyt lukemalla [säännöstä web-sovelluksessa, jossa on SQL-tietokanta](app-service-web/app-service-web-arm-with-sql-database-provision.md) on artikkelissa.
Huomautus: Tässä artikkelissa on kirjoitettu [201-web-sovelluksen-sql-tietokanta](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) -mallin joulukuuta 2015-versiota. Alla olevia linkkejä valitsemalla mallin ja parametritiedostot ovat kyseisen mallin-versioon.
2. Valitse [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json) -tiedoston avulla voit tarkastella sen sisältöä 201-web-sovelluksen-sql-tietokanta-kansiossa. Tämä on Azure Resurssienhallinta-mallitiedoston. 
3. Napsauta näyttötilassa, "[raaka](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json)"-painiketta. 
4. Hiirellä Valitse tämän tiedoston sisällön ja tallentaa ne tietokoneeseen tiedoston nimeltä "TestApp1-Template.json." 
5. Mallin sisällön tutkia ja Huomaa seuraavat:
 - **Resurssit** -osa: Tämä osa määrittää tämän mallin avulla luotu Azure resurssien lajeja. Muuntyyppisten resurssien kesken Tämä malli luo [Azure Web App](app-service-web/app-service-web-overview.md) - ja [Azure SQL-tietokanta](sql-database/sql-database-technical-overview.md) resurssit. Jos haluat suorittaa ja hallita Internetin kautta tai näennäiskoneiden SQL-palvelimet, "[iis-2vm-sql-1vm](https://github.com/Azure/azure-quickstart-templates/tree/master/iis-2vm-sql-1vm)" tai "[hehkulampun app](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)"-mallien avulla, mutta [201-web-sovelluksen-sql-tietokanta](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) -malliin perustuvat tässä artikkelissa annettuja ohjeita.
 - **Parametrit** -osassa: Tässä osassa määrittää parametrit, jossa voi määrittää kullekin resurssille. Jotkin mallissa määritetystä parametrit on "oletusarvo-ominaisuuksia, samalla, kun muut käyttäjät eivät. Kun otat mallin Azure resursseja, Anna arvot kaikki parametrien, joka ei ole määritetty mallin Oletusarvo-ominaisuutta.  Jos et anna arvot parametrien oletusarvo ominaisuudet-mallin oletusarvo-parametrille määritetty arvo käytetään.

Mallin määrittää Azure resursseja luodaan ja parametrit kullekin resurssille voidaan määrittää. Voit lukea lisää malleja ja kuinka voit suunnitella omasi lukemalla artikkelin [parhaita käytäntöjä suunnitteluun Azure Resurssienhallinta malleja](best-practices-resource-manager-design-templates.md) .

## <a name="download-and-customize-an-existing-parameter-file"></a>Lataa ja mukauttaa parametrin aiemmin luotu tiedosto

Vaikka haluat todennäköisesti *sama* Azure resurssit luodaan kunkin ympäristössä, todennäköisesti haluat *eri* kunkin ympäristössä resurssien määrittäminen.  Tämä on mistä parametritiedostot tulee. Luo parametri-tiedostoja, jotka sisältävät yksilöllisiä arvoja kunkin ympäristössä, noudata seuraavia ohjeita.   

1. Tarkastella [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json) -tiedoston sisällön 201-web-sovelluksen-sql-tietokanta-kansiossa. Tämä on edellisessä osassa tallentaa mallitiedoston parametri-tiedosto. 
2. Napsauta näyttötilassa, "[raaka](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json)"-painiketta. 
3. Hiirellä Valitse tämän tiedoston sisällön ja tallentaa ne kolme erillisinä tiedostoina tietokoneeseen seuraavat nimillä:
 - TestApp1-parametrit-Development.json
 - TestApp1-parametrit-Test.json
 - TestApp1-parametrit-Pre-Production.json

3. Käytä tekstiä ja JSON-editorin, Muokkaa loit vaiheessa 3, luettelossa oikealla puolella parametriarvot tiedoston oikealla puolella **Parametrit** olevat *arvot* arvojen korvaaminen kehittäminen ympäristön Parametritiedostoa: 
 - **nimi**: *TestApp1DevApp*
 - **hostingPlanName**: *TestApp1DevPlan*
 - **siteLocation**: *Keskitetty USA*
 - **palvelimen nimi**: *testapp1devsrv*
 - **serverLocation**: *Keskitetty USA*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *Korvaa salasanalla*
 - **nimi**: *testapp1devdb*

4. Minkä tahansa tekstin tai JSON editorilla Muokkaa testitiedosto ympäristön parametrin loit vaiheessa 3, korvaaminen oikealla puolella **Parametrit** alla olevat *arvot* tiedoston parametrin arvot oikealla puolella olevat arvot:
 - **nimi**: *TestApp1TestApp*
 - **hostingPlanName**: *TestApp1TestPlan*
 - **siteLocation**: *Keskitetty USA*
 - **palvelimen nimi**: *testapp1testsrv*
 - **serverLocation**: *Keskitetty USA*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *Korvaa salasanalla*
 - **nimi**: *testapp1testdb*

5. Käytä tekstiä ja JSON-editorin, Muokkaa edeltävien parametri-tiedosto, jonka loit vaiheessa 3.  Mikä on alle korvaa tiedoston koko sisällön:

        {
          "$schema" : "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
          "contentVersion" : "1.0.0.0",
          "parameters" : {
        "administratorLogin" : {
          "value" : "testApp1Admin"
        },
        "administratorLoginPassword" : {
          "value" : "replace with your password"
        },
        "databaseName" : {
          "value" : "testapp1preproddb"
        },
        "hostingPlanName" : {
          "value" : "TestApp1PreProdPlan"
        },
        "serverLocation" : {
          "value" : "Central US"
        },
        "serverName" : {
          "value" : "testapp1preprodsrv"
        },
        "siteLocation" : {
          "value" : "Central US"
        },
        "siteName" : {
          "value" : "TestApp1PreProdApp"
        },
        "sku" : {
          "value" : "Standard"
        },
            "requestedServiceObjectiveName" : {
              "value" : "S1"
        }
          }
        }

Edeltävien parametrit-tiedostossa yllä **tuote** - ja **requestedServiceObjectiveName** -parametrit on *lisätty*, olisi ne ole lisännyt kehitys- ja parametrit-tiedostoissa. Tämä johtuu siitä, on määritetty nämä parametrit-mallissa oletusarvot ja kehitys- ja -ympäristöissä oletusarvot käytetään, mutta ennen tuotannon ympäristön muun kuin oletusarvoisen arvoja näiden parametrien käytetään.

Muun kuin oletusarvoisen-arvoja käytetään muuttujat vanhat tuotantoympäristössä johtuu Testaa nämä, haluat ehkä muuttaa tuotannon-ympäristössä niin, että ne myös voidaan testata parametrien arvot.  Kaikki nämä parametrit liittyvät Azure [isännöintipalvelu Web App-palvelupaketti](https://azure.microsoft.com/pricing/details/app-service/)tai **tuote** ja Azure [SQL-tietokanta](https://azure.microsoft.com/pricing/details/sql-database/)tai **requestedServiceObjectiveName** , joita käytetään sovellus.  Eri tuotteissa ja palveluiden objektiivisten nimet on eri kustannukset ja ominaisuudet ja tuki eri tason arvot.

Alla olevassa taulukossa on lueteltu nämä parametrit on määritetty malli ja sen sijaan, että edeltävien parametrit-tiedoston oletusarvot käytettävät arvot oletusarvot.

| Parametri | Oletusarvo | Tiedoston parametriarvo |
|---|---|---|
| **tuote** | Vapaa | Vakio |
| **requestedServiceObjectiveName** | S0 | S1 |

## <a name="create-environments"></a>Luo ympäristöissä
Kaikki Azure resurssit on luotava [Azure resurssiryhmä](azure-resource-manager/resource-group-overview.md). Resurssiryhmien avulla voit ryhmitellä Azure resurssit, jotta ne voidaan hallita yhdessä.  Resurssiryhmiä voi määrittää [käyttöoikeuksia](./active-directory/role-based-access-built-in-roles.md) siten, että tietyn organisaatiosi henkilöitä voit luoda, muokata, poistaa tai tarkastella niitä ja niiden välillä resurssit.  Ilmoitusten ja resursseja resurssiryhmän tietoja voi tarkastella [Azure-portaalissa](https://portal.azure.com). Resurssiryhmät luodaan Azure [alue](https://azure.microsoft.com/regions/).  Tässä artikkelissa kaikkien resurssien luodaan Yhdysvaltojen Keski-alueella. Kun alat luoda todellinen ympäristöissä, voit valita haluamasi alue, joka vastaa parhaiten tarpeitasi. 

Luo resurssiryhmiä kussakin ympäristössä jollakin seuraavista tavoista.  Kaikki tavat saavuttaa saman tuloksen.

###<a name="azure-command-line-interface-cli"></a>Azure rivin komentoliittymä (CLI)

Varmista, että voit olla CLI [asennettu](xplat-cli-install.md) joko Windows OS X- tai Linux tietokoneen ja joihin sinulla on [yhteys](xplat-cli-connect.md) [Azure AD-tili](./active-directory/active-directory-how-subscriptions-associated-directory.md) (kutsutaan myös työpaikan tai oppilaitoksen tili) Azure-tilaukseen. CLI komentoriviltä kirjoittamalla alla komento kehitysympäristö Resurssiryhmän luominen.

    azure group create "TestApp1-Development" "Central US"

Komento voi palauttaa seuraavasti, jos se onnistuu:

    info:    Executing command group create
    + Getting resource group TestApp1-Development
    + Creating resource group TestApp1-Development
    info:    Created resource group TestApp1-Development
    data:    Id:                  /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development
    data:    Name:                TestApp1-Development
    data:    Location:            centralus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Voit luoda testiympäristössä resurssiryhmän kirjoittamalla komennon alla:

    azure group create "TestApp1-Test" "Central US"

Voit luoda vanhat tuotantoympäristössä resurssiryhmän kirjoittamalla komennon alla:

    azure group create "TestApp1-Pre-Production" "Central US"

###<a name="powershell"></a>PowerShellin

Varmista, että käytössäsi on Azure PowerShell 1.01 tai uudempi Windows-tietokoneeseen asennetussa yhdistettyjä tilauksen yksilöityjä [asentaminen ja määrittäminen PowerShellin Azure](powershell-install-configure.md) on artikkelissa [Azure AD-tili](./active-directory/active-directory-how-subscriptions-associated-directory.md) (kutsutaan myös työpaikan tai oppilaitoksen tili). PowerShell-komentoriviltä kirjoittamalla alla komento kehitysympäristö Resurssiryhmän luominen.

    New-AzureRmResourceGroup -Name TestApp1-Development -Location "Central US"

Komento voi palauttaa seuraavasti, jos se onnistuu:

    ResourceGroupName : TestApp1-Development
    Location          : centralus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development

Voit luoda testiympäristössä resurssiryhmän kirjoittamalla komennon alla:

    New-AzureRmResourceGroup -Name TestApp1-Test -Location "Central US"

Voit luoda vanhat tuotantoympäristössä resurssiryhmän kirjoittamalla komennon alla:

    New-AzureRmResourceGroup -Name TestApp1-Pre-Production -Location "Central US"

###<a name="azure-portal"></a>Azure portal

1. Kirjautuminen [Azure portal](https://portal.azure.com) (kutsutaan myös työpaikan tai oppilaitoksen) [Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) -tilillä. Valitse Uusi--> hallinta--> resurssiryhmä ja "Kehittäminen TestApp1" Kirjoita resurssin ryhmän nimi-ruutuun, valitse tilauksesi ja valitse "Keskitetyn US" resurssin ryhmän oletussijainti-ruutuun alla olevassa kuvassa esitetyllä tavalla.
   ![Portal](./media/solution-dev-test-environments/rgcreate.png)
2. Napsauta Luo resurssiryhmän Luo-painiketta.
3. Valitse Selaa, resurssin ryhmiin luettelossa alaspäin ja valitse sitten resurssiryhmien alla kuvatulla tavalla.
   ![Portal](./media/solution-dev-test-environments/rgbrowse.png) 
4. Resurssin ryhmiä esiin, kun näet resurssin ryhmät-sivu ja uusi resurssiryhmä.
   ![Portal](./media/solution-dev-test-environments/rgview.png)
5. Luo TestApp1-testi ja TestApp1 Pre-tuotannon resurssin ryhmittelee samalla tavalla kuin loit yllä TestApp1 kehittäminen resurssiryhmä.

##<a name="deploy-resources-to-environments"></a>Resurssien käyttöön ympäristöissä

Ota käyttöön Azure resurssien resurssin ryhmiin kussakin ympäristössä mallitiedoston ratkaisun ja -parametri-tiedostojen käytön kussakin ympäristössä jommallakummalla seuraavista tavoista.  Lopettamista saavuttaa saman tuloksen.

###<a name="azure-command-line-interface-cli"></a>Azure rivin komentoliittymä (CLI)

CLI komentoriviltä kirjoittamalla komennon alla ottamaan resursseja resurssiryhmän loit kehitysympäristö, [polku] tilalle polku edelliset vaiheet tallennettuja tiedostoja.

    azure group deployment create -g TestApp1-Development -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Development.json 

Jälkeen "Odotetaan käyttöönoton suorittamiseen"-viesti näkyviin muutaman minuutin kuluttua-komento palauttaa seuraavasti, jos se onnistuu:

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "Deployment1"
    + Waiting for deployment to complete
    data:    DeploymentName     : Deployment1
    data:    ResourceGroupName  : TestApp1-Development
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : XXXX-XX-XXT20:20:23.5202316Z
    data:    Mode               : Incremental
    data:    Name                           Type          Value
    data:    -----------------------------  ------------  ----------------------------
    data:    siteName                       String        TestApp1DevApp
    data:    hostingPlanName                String        TestApp1DevPlan
    data:    siteLocation                   String        Central US
    data:    sku                            String        Free
    data:    workerSize                     String        0
    data:    serverName                     String        testapp1devsrv
    data:    serverLocation                 String        Central US
    data:    administratorLogin             String        testapp1Admin
    data:    administratorLoginPassword     SecureString  undefined
    data:    databaseName                   String        testapp1devdb
    data:    collation                      String        SQL_Latin1_General_CP1_CI_AS
    data:    edition                        String        Standard
    data:    maxSizeBytes                   String        1073741824
    data:    requestedServiceObjectiveName  String        S0
    info:    group deployment create command OKx

Jos komento ei toimi, ratkaista virhesanomia ja yritä sitten uudelleen.  Yleisimpien ongelmien käyttävät parametriarvot, joka ei noudata Azure nimeämiskäytäntö rajoitteet. Muut vianmääritysvihjeitä löytyy [vianmääritys resurssin ryhmän ominaisuuksissa Azure-tietokannassa](./resource-manager-troubleshoot-deployments-cli.md) on artikkelissa.

CLI komentoriviltä kirjoittamalla komennon alla ottamaan resursseja resurssiryhmän loit testiympäristössä, [polku] tilalle polku edelliset vaiheet tallennettuja tiedostoja.

    azure group deployment create -g TestApp1-Test -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Test.json

CLI komentoriviltä kirjoittamalla komennon alla ottamaan resursseja resurssiryhmän loit vanhat tuotantoympäristössä [polku] tilalle polku edelliset vaiheet tallennettuja tiedostoja.

    azure group deployment create -g TestApp1-Pre-Production -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Pre-Production.json
  
###<a name="powershell"></a>PowerShellin

PowerShellin Azure (1.01 tai uudempi versio) komentoriviltä kirjoittamalla komennon alla ottamaan resursseja resurssiryhmän loit kehitysympäristö, [polku] tilalle polku edelliset vaiheet tallennettuja tiedostoja.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Development -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Development.json -Name Deployment1 

Jälkeen näkyviin vilkkuvan kohdistimen muutaman minuutin kuluttua-komento palauttaa seuraavasti, jos se onnistuu:

    DeploymentName    : Deployment1
    ResourceGroupName : TestApp1-Development
    ProvisioningState : Succeeded
    Timestamp         : XX/XX/XXXX 2:44:48 PM
    Mode              : Incremental
    TemplateLink      : 
    Parameters        : 
                        Name             Type                       Value     
                        ===============  =========================  ==========
                        siteName         String                     TestApp1DevApp
                        hostingPlanName  String                     TestApp1DevPlan
                        siteLocation     String                     Central US
                        sku              String                     Free      
                        workerSize       String                     0         
                        serverName       String                     testapp1devsrv
                        serverLocation   String                     Central US
                        administratorLogin  String                     testapp1Admin
                        administratorLoginPassword  SecureString                         
                        databaseName     String                     testapp1devdb
                        collation        String                     SQL_Latin1_General_CP1_CI_AS
                        edition          String                     Standard  
                        maxSizeBytes     String                     1073741824
                        requestedServiceObjectiveName  String                     S0        
                        
    Outputs           :

  Jos komento ei toimi, ratkaista virhesanomia ja yritä sitten uudelleen.  Yleisimpien ongelmien käyttävät parametriarvot, joka ei noudata Azure nimeämiskäytäntö rajoitteet. Muut vianmääritysvihjeitä löytyy [vianmääritys resurssin ryhmän ominaisuuksissa Azure-tietokannassa](./resource-manager-troubleshoot-deployments-powershell.md) on artikkelissa.

  PowerShell-komentoriviltä kirjoittamalla komennon alla ottamaan resursseja resurssiryhmän loit testiympäristössä, [polku] tilalle polku edelliset vaiheet tallennettuja tiedostoja.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Test -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Test.json -Name Deployment1

  PowerShell-komentoriviltä kirjoittamalla komennon alla ottamaan resursseja resurssiryhmän loit vanhat tuotantoympäristössä [polku] tilalle polku edelliset vaiheet tallennettuja tiedostoja.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Pre-Production -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Pre-Production.json -Name Deployment1

Mallin ja parametri-tiedostoja voidaan versiotietoja ja ylläpidetään sovelluksen koodilla Ohjausobjektin lähde-järjestelmässä.  Komentosarjan tiedostot ja tallentaa ne omassa koodissasi yllä olevien komentojen voi tallentaa myös.

> [AZURE.NOTE] Voit ottaa mallin käyttöön Azure suoraan napsauttamalla [säännöstä Web-sovelluksessa, jossa on SQL-tietokanta](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) on artikkelissa "Käyttöönotto ja Azure"-painiketta.  Voi olla tämä hyödyllisiä tietoja, mutta linkin ei, joiden avulla voit muokata-versiota, ja Tallenna malli ja parametrin arvot sovelluksen-koodia, siten lisätietoja Tämä menetelmä ei koske tämän artikkelin.

## <a name="maintain-environments"></a>Ylläpidä ympäristöissä
Koko kehityksen eri ympäristöissä Azure resurssien määrittäminen voidaan yhtenäistä muuttaa tarkoituksella tai vahingossa.  Tämä voi aiheuttaa tarpeettomia vianmääritys ja ongelmanratkaisua sovellusten kehittämisen aikana.

1. Voit muuttaa ympäristöjen avaamalla [Azure portal](https://portal.azure.com). 
2. Kirjaudu sisään sen samaa tiliä, jota käytit edellä kuvatulla tavalla. 
3. Kuvan alapuolella, valitse Selaa--> resurssiryhmät (joudut ehkä vierittämään haluat tarkastella resurssiryhmiä) näkyvät.
   ![Portal](./media/solution-dev-test-environments/rgbrowse.png)
4. Kun olet napsauttanut resurssin ryhmiä seuraavassa kuvassa, näkyvät resurssin ryhmät-sivu ja kolme alla olevassa kuvassa esitetyllä tavalla edellisessä vaiheessa luomasi resurssiryhmät. Valitse TestApp1 kehittäminen resurssiryhmä, jolloin näkyviin tulee sivu, jossa on luettelo edellisessä vaiheessa Valmis TestApp1 kehittäminen resurssin käyttöönoton mallissa luoma resurssit.  Poista TestApp1DevApp online-resurssin napsauttamalla TestApp1DevApp TestApp1 kehittäminen resurssien ryhmä-sivu ja valitsemalla sitten Poista TestApp1DevApp Web app-sivu.
   ![Portal](./media/solution-dev-test-environments/portal2.png)
5. Valitse "Kyllä", kun portaalin kysyy, onko olet varma, että haluat poistaa resurssin. TestApp1 kehittäminen resurssien ryhmä-sivu sulkemalla ja avaamalla se uudelleen näkyvät nyt se ilman heti sen poistettuasi Web Appia.  Resurssiryhmä sisältö on nyt erilainen kuin niiden pitäisi olla. Voit kokeilla edelleen useita resursseja poistaminen useita resurssiryhmiä tai muuttamalla jopa joitakin resurssien käyttöasetukset. Käytä voitu PowerShell [Poista AzureResource](https://msdn.microsoft.com/library/azure/dn757676.aspx) -komennon sijaan Azure portaalin resurssin poistaminen resurssiryhmä tai tai "azure resurssin poistaminen"-komentoa CLI saman tehtävän suorittamiseen.
6. Saat kaikki määrityksistä, jotka ovat pitäisi olla takaisin tilaan, ne on oltava resurssi-ryhmien ja resurssien käyttöön uudelleen ympäristöjen [käyttöönotto resurssien ympäristöt](#deploy-resources-to-environments) -osan avulla voit käyttää samoja komentoja resurssin ryhmiin, mutta korvaa "Deployment1" ja "Deployment2."
7.  Kuten kuvassa näkyy vaiheessa 4 TestApp1 kehittäminen-sivu yhteenveto-osassa, näet, että poistamisen portaali edellisessä vaiheessa online olemassa uudelleen, tavalla muut resurssit olet saattanut poistaminen. Jos olet muuttanut mitään resursseista määritykset, varmista, että ne on asetettu takaisin arvot-parametrin tiedostojen liian. Yksi otat käyttöön oman ympäristöissä Azure Resurssienhallinta malleja eduista on, että voit helposti uudelleen ottaa käyttöön ympäristöissä takaisin tunnetut tilaan milloin tahansa.
8. Jos napsautat "Edellisen käyttöönoton" alla olevassa kuvassa alla olevan tekstin, näkyviin tulee sivu, jossa näkyy resurssiryhmän käyttöönoton historia.  Koska olet käyttänyt ensimmäisen käyttöönoton ja "Deployment2" nimi "Deployment1" toisen käyttöönottoa varten, sinun on kaksi merkintää.  Käyttöönoton valitsemalla Näytä sivu, joka näyttää tulokset kunkin käyttöönottoa varten.
  ![Portal](./media/solution-dev-test-environments/portal3.png)



## <a name="delete-environments"></a>Poista ympäristöissä
Kun olet valmis-ympäristön avulla, kannattaa poistaa sen, jotta et maksamaan enää käytät Azure resurssien käyttökustannukset.  Ympäristöissä poistaminen on sitäkin helpompaa kuin niiden luominen.  Yksittäiset Azure resurssiryhmät on luotu kussakin ympäristössä edelliset vaiheet ja sitten resurssit otettiin käyttöön resurssi-ryhmiin. 

Poista ympäristöissä jollakin seuraavista tavoista.  Kaikki tavat saavuttaa saman tuloksen.

### <a name="azure-cli"></a>Azure CLI

Kirjoita CLI kehotteen, seuraavasti:

    azure group delete "TestApp1-Development"

Kun sinulta kysytään, kirjoita y ja paina sitten enter kehitysympäristö ja kaikki sen resurssit poistaminen. Muutaman minuutin kuluttua komento palauttaa seuraavasti:

    info:    group delete command OK

Kirjoita CLI kehotteen, poista jäljellä ympäristöissä seuraavasti:

    azure group delete "TestApp1-Test"
    azure group delete "TestApp1-Pre-Production"
  
### <a name="powershell"></a>PowerShellin

PowerShellin Azure (1.01 tai uudempi versio) komentoriviltä kirjoittamalla alla komento poistaa resurssiryhmän ja sen sisältöä.    

    Remove-AzureRmResourceGroup -Name TestApp1-Development

Jos olet varma, että haluat poistaa resurssiryhmän Anna y-ja sen jälkeen enter-näppäintä.

Kirjoita Poista jäljellä ympäristöissä:

    Remove-AzureRmResourceGroup -Name TestApp1-Test
    Remove-AzureRmResourceGroup -Name TestApp1-Pre-Production

### <a name="azure-portal"></a>Azure portal

1. Siirry Azure-portaalissa resurssiryhmät samoin kuin edellä kuvatut vaiheet. 
2. Valitse TestApp1 kehittäminen resurssiryhmä ja valitse sitten Poista TestApp1 kehittäminen resurssien ryhmä-sivu. Uusi sivu tulee näkyviin. Kirjoita resurssin ryhmänimi ja valitse Poista-painiketta.
![Portal](./media/solution-dev-test-environments/rgdelete.png)
3. Poista TestApp1-testi ja TestApp1 Pre-tuotannon resurssin ryhmittelee TestApp1 kehittäminen resurssiryhmä poistetaan samalla tavalla.

Riippumatta siitä, voit käyttää menetelmä resurssiryhmät ja kaikki resurssit, se ei enää ole olemassa ja enää maksamaan resurssien laskutuksen kulut.  

Pienennä Azure resurssien käyttö kulujen aikana sovellusten kehittämisen [Azure automaatio](automation/automation-intro.md) voit ajoittaa maksamaan työt:

- Pysäytä näennäiskoneiden päivittäin lopussa ja käynnistä ne päivittäin alkuun.
- Poista koko ympäristöissä päivittäin lopussa ja luo ne uudelleen aina päivän alkuun.
 
Nyt kun olet listan, miten helppoa on luomiseen, säilyttää, ja poista kehittämisen ja testaa ympäristöissä, Lisätietoja on siitä, mitä olet juuri ollut kokeileminen edellä kuvatut vaiheet ja lukea tämän artikkelin viittausten tarkemmin.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Edustajan hallintaoikeuden](./active-directory/role-based-access-control-configure.md) eri resursseihin määrittämällä Microsoft Azure AD-ryhmille tai käyttäjille, joilla voi suorittaa alijoukkoa Azure resurssien toimenpiteet tietyille rooleille kunkin ympäristössä.
- Resurssiryhmät kussakin ympäristössä ja/tai yksittäisten resurssien [määrittäminen tunnisteet](resource-group-using-tags.md) . Voi lisätä "Ympäristön" tunnisteen resurssiryhmien ja määritä sen arvoksi ympäristön nimet vastaavat. Tunnisteita voi olla hyödyllistä, kun haluat järjestää resurssien laskutus tai hallinta.
- Seurata ilmoitusten ja resurssien ryhmän resurssien [Azure portaalissa](https://portal.azure.com)laskutus.
