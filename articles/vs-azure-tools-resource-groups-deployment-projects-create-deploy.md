<properties
   pageTitle="Azure resurssin ryhmän Visual Studio projektien | Microsoft Azure"
   description="Visual Studio avulla voit luoda Azure resurssin yhteisessä projektissa ja ota käyttöön resurssien Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="tomfitz" />

# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Luominen ja käyttöönotto Azure resurssiryhmät Visual Studio kautta

Visual Studio ja [Azure SDK-paketissa](https://azure.microsoft.com/downloads/)voit luoda projektin, joka ottaa käyttöön infrastruktuuri ja koodin Azure. Voit esimerkiksi määrittää web-isäntään, sivuston ja tietokannan, kun sovellus ja ottaa käyttöön, infrastruktuuri ja koodin. Vaihtoehtoisesti voit määrittää virtuaalikoneen, VPN- ja tallennustilaa tili ja ottaa käyttöön kyseisen infrastruktuurin sekä komentosarjan, joka suoritetaan virtuaalikoneen. **Azure resurssiryhmä** käyttöönotto-projektin mahdollistaa käyttöön tarvittavien resurssien yhteen, toistettavien toiminnossa. Saat lisätietoja käyttöönotto ja resurssien hallinta [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md).

Azure resurssiryhmä projektien sisältää Azure Resurssienhallinta JSON malleihin, jotka määrittävät resursseja, joilla voit ottaa käyttöön Azure. Lisätietoja Resurssienhallinta-mallin osista on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](resource-group-authoring-templates.md). Visual Studio avulla voit muokata malleja ja työkaluja, jotka helpottavat mallien käyttämisestä.

Tässä ohjeaiheessa käyttöönottoa web app- ja SQL-tietokantaan. Vaiheet ovat kuitenkin lähes sama tyyppi resurssin. Voit kuin helposti ottaa Virtual Machine ja sen liittyvät resurssit. Visual Studio käyttöönotto Yleisiä tilanteita, joissa on useita eri starter-mallit.

Tässä artikkelissa kerrotaan Visual Studio 2015 päivityksen 2 ja Microsoft Azure SDK .NET 2.9. Jos Azure SDK 2.9 Visual Studio 2013-käyttökokemuksen on suurelta sama. Voit käyttää versioiden Azure SDK 2.6 tai uudempi; käyttöliittymän käyttökokemuksen voi olla eri kuin käyttöliittymän suorittaa tässä artikkelissa. Suosittelemme, että asennat uusimman version [Azure SDK](https://azure.microsoft.com/downloads/) ennen kuin aloitat ohjeita. 

## <a name="create-azure-resource-group-project"></a>Projektin luominen Azure resurssiryhmä

Prosessin myöhemmässä vaiheessa voit luoda Azure resurssiryhmä-project **Web Appin + SQL** -malli.

1. Visual Studiossa, valitse **Tiedosto**, **Uusi projekti**, valitse **C#** tai **Visual Basic**. Valitse **Cloud**ja valitse sitten **Azure resurssiryhmä** projektin.

    ![Cloud käyttöönotto-projektin](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Valitse malli, jonka haluat ottaa käyttöön Azure resurssien hallinta. Huomaa on monia erilaisia vaihtoehtoja perustuvat haluat ottaa käyttöön projektin tyyppi. Tämän aiheen Valitse **Web Appissa + SQL** -malli.

    ![Mallin valitseminen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Voit valita malli on vain pohjana; Voit lisätä ja poistaa resursseja täyttämään käyttämässäsi skenaariossa.

    >[AZURE.NOTE] Visual Studio hakee käytettävissä olevat mallit luettelo. Luettelon voivat muuttua.

    Visual Studio Luo resurssin ryhmän käyttöönottoprojektin web app-ja SQL-tietokantaan.

1. Olet luonut näkyviin laajentamalla solmut käyttöönoton Projectissa.

    ![Näytä solmujen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Koska Microsoft valitsi Web Appin + tässä esimerkissä SQL-malli, saat seuraavat tiedostot: 

  	|Tiedostonimi|Kuvaus|
  	|---|---|
  	|Ottaa käyttöön AzureResourceGroup.ps1|PowerShell-komentosarja, joka käynnistää Azure resurssien hallinnan käyttöönotto PowerShell-komennoilla.<br />**Huomautus** Visual Studio käyttää tätä PowerShell-komentosarjaa ottaa mallin käyttöön. Komentosarjan tekemäsi muutokset vaikuttavat käyttöönotto Visual Studiossa, joten varovainen.|
  	|WebSiteSQLDatabase.json|Resurssienhallinta-malli, joka määrittää infrastruktuurin haluat ottaa käyttöön Azure ja parametreja, voit antaa käyttöönoton aikana. Se myös määrittää resurssia, joten Resurssienhallinta ottaa käyttöön oikeassa järjestyksessä resurssien väliset riippuvuudet.|
  	|WebSiteSQLDatabase.parameters.json|Parametrit-tiedosto, joka sisältää mallin tarvitsemia arvoja. Voit siirtää parametriarvot mukauttaminen kunkin käyttöönotto.|

    Kaikkien resurssien ryhmän käyttöönottoprojektien sisältää perustiedot tiedostot. Muiden projektien saattavat sisältää muita tiedostoja tukemaan muita toimintoja.

## <a name="customize-the-resource-manager-template"></a>Mukauta Resurssienhallinta-malli

Voit mukauttaa käyttöönottoprojektin muokkaamalla JSON-mallit, jotka kuvaavat resursseja, jonka haluat ottaa käyttöön. JSON JavaScript Object Notation lyhenne ja sarjoitettu tietoja muodossa, joka on helppo käyttää. JSON-tiedostoja käyttää rakennetta, jotka viittaavat kunkin tiedoston yläosassa. Halutessasi voit kartoittaa rakenteen voit ladata ja analysoi. Rakenne määrittää, mitkä osat ovat kelvollisia, tyypit ja muotoilut-kentät, mahdolliset arvot ja numeroarvot ja niin edelleen. Lisätietoja Resurssienhallinta-mallin osista on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](resource-group-authoring-templates.md).

Voit käsitellä mallin avaamalla **WebSiteSQLDatabase.json**.

Visual Studio editorin työkalujen avustaminen muokkaaminen Resurssienhallinta mallia. **JSON** Jäsentelyikkunaa on helppo määritelty mallin elementtien.

![Näytä JSON Jäsennys](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Valita jonkin elementit jäsennyksen siirryt mallin osa ja korostaa vastaavat JSON.

![Siirry JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Voit lisätä resurssin joko valitsemalla JSON Jäsentelyikkunaa yläreunassa **Lisää resurssi** -painiketta tai **resurssien** hiiren kakkospainikkeella ja valitsemalla **Lisää uusi resurssi**.

![Lisää resurssi](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Tässä opetusohjelmassa **Tallennustilan** -tili ja anna sille nimi. Anna nimi, joka on enintään 11 merkkiä ja sisältää vain lukuja ja pienet kirjaimet.

![Lisää tallennustilaa](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Huomaa, että on ei vain lisätä, mutta myös parametrin tallennustilan tilin tyyppi ja muuttuja-tallennustilan tilin nimi.

![Näytä jäsennys](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

**StorageType** -parametri on valmiiksi määritettyjä sallitut sisältötyypit ja oletuslajin. Voit jättää kyseiset arvot tai muokata niitä, käyttämässäsi skenaariossa. Jos et halua kenen tahansa ottaa käyttöön **Premium_LRS** -tallennustilan tilin tämän mallin avulla, Poista sivusto sallitut sisältötyypit. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Visual Studio sisältää myös intellisense voi auttaa sinua ymmärtämään, mitkä ominaisuudet ovat käytettävissä, kun mallin muokkaaminen. Esimerkiksi sovelluksen palvelusopimus ominaisuuksien muokkaaminen **HostingPlan** resurssin Siirry ja lisää **ominaisuuksia**arvo. Huomaa, että intellisense näyttää käytettävissä olevat arvot ja kuvaa, kyseinen arvo.

![Näytä intellisense](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Voit määrittää **numberOfWorkers** 1.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## <a name="deploy-the-resource-group-project-to-azure"></a>Ota käyttöön Azure resurssiryhmä-projekti

Olet nyt valmis ottamaan projektin. Kun otat käyttöön Azure resurssiryhmä-projektin, voit ottaa käyttöön Azure resurssiryhmä. Resurssiryhmä on looginen ryhmittely resurssien, joilla on yhteiset elinkaari.

1. Valitse käyttöönoton projektin solmun hiiren kakkospainikkeella ja **Ota käyttöön** > **Uudet käyttöönoton**.

    ![Ottaa käyttöön, uudet käyttöönoton-valikkovaihtoehto](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    **Ota resurssiryhmä** -valintaikkuna.

    ![Ottaa käyttöön resurssiryhmä-valintaikkuna](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. Valitse aiemmin luotu resurssiryhmä tai luoda uuden avattava **resurssiryhmä** -ruutuun. Jos haluat luoda resurssiryhmän, Avaa **Resurssiryhmä** avattava luetteloruutu-ruutu ja valitse **Luo uusi**.

    ![Ottaa käyttöön resurssiryhmä-valintaikkuna](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    **Luo resurssiryhmä** -valintaikkuna. Anna Kirjoita ryhmän nimi ja sijainti ja valitse **Luo** -painiketta.

    ![Luo resurssiryhmä-valintaikkuna](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. Muokkaa käyttöönoton parametrit valitsemalla **Muokkaa parametrit** -painikkeen.

    ![Muokkaa parametrit-painike](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)

1. Anna tyhjä parametrien arvot ja valitse **Tallenna** -painiketta. Tyhjä parametrit ovat **hostingPlanName**, **administratorLogin**tai **administratorLoginPassword** **nimi**.

    **hostingPlanName** määrittää nimen [App palvelusopimus](./app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) luomiseen. 
    
    **administratorLogin** määrittää SQL Server-järjestelmänvalvojan käyttäjänimi. Älä käytä yleisiä järjestelmänvalvojan nimiä, kuten **sa** tai **järjestelmänvalvoja**. 
    
    **AdministratorLoginPassword** määrittää SQL Server-järjestelmänvalvojan salasanan. **Tallenna salasanat pelkkänä tekstinä parametrit-tiedostoon** -asetus ei ole suojattu; Tämän vuoksi Älä valitse tämä asetus. Koska salasanaa ei tallenneta pelkkänä tekstinä, sinun on annettava salasana uudelleen käyttöönoton aikana. 
    
    **nimi** määrittää nimen, tietokannan luomiseen. 

    ![Muokkaa parametrit-valintaikkuna](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
1. Voit ottaa käyttöön projektin Azure **käyttöönotto** -painiketta. PowerShell console avautuu Visual Studio esiintymän ulkopuolella. Kirjoita SQL Server-järjestelmänvalvojan salasanan PowerShell console pyydettäessä. **PowerShell console saattaa takana muita kohteita tai pienennettynä tehtäväpalkissa.** Tämä konsoli Etsi ja valitse se antamaan salasana.

    >[AZURE.NOTE] Visual Studio voi pyytää asentamaan Azure PowerShell cmdlet-komentoja. Tarvitset Azure PowerShell-cmdlet-komennot käyttöön resurssiryhmät. Jos näyttöön tulee kehote, asenna ne.
    
1. Käyttöönotto voi kestää muutaman minuutin kuluttua. **Tulostus** -Windowsin käyttöönoton tilan tarkasteleminen. Kun asennus on valmis, viimeisen viestin ilmaisee onnistuneen ympäristö, jonka suurin piirtein seuraavanlaisen:

        ... 
        18:00:58 - Successfully deployed template 'c:\users\user\documents\visual studio 2015\projects\azureresourcegroup1\azureresourcegroup1\templates\websitesqldatabase.json' to resource group 'DemoSiteGroup'.


1. Selaimessa Avaa [Azure portal](https://portal.azure.com/) ja kirjaudu sisään-tiliisi. Voit avata resurssiryhmän valitsemalla **resurssiryhmien** ja resurssiryhmä, voit ottaa käyttöön.

    ![Valitse ryhmä](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Näet kaikki käyttöön resurssit. Huomaa, että tallennustilan tilin nimi ei ole tasan, voit määrittää kyseiselle resurssille lisääminen. Tallennustilan tilin on oltava yksilöllinen. Mallin lisää automaattisesti merkkijonon nimi, jonka annoit yksilöivä nimi. 

    ![Näytä resurssit](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Jos muutosten tekeminen ja haluat projektin Ota uudelleen, valitse aiemmin resurssiryhmä Azure resurssin yhteisessä projektissa pikavalikosta. Hiiren kakkospainikkeella ja valitse **Ota käyttöön**ja valitse resurssiryhmä, voit ottaa käyttöön.

    ![Ottaa käyttöön Azure resurssiryhmä](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Ottaa käyttöön koodia infrastruktuurin kanssa

Tässä vaiheessa infrastruktuuri on otettu käyttöön, kun sovellus, mutta ei ole otettu käyttöön projektin todellinen koodia. Tässä ohjeaiheessa esitellään verkkosovellukseen ja SQL-tietokantataulukot ottamisesta käyttöönoton aikana. Jos otat Virtual Machine, sen sijaan, että verkkosovellukseen, jonka haluat suorittaa lisäkoodin tietokoneeseen osana käyttöönotto. Prosessi, jossa käyttöönotto koodi verkkosovellukseen tai Virtual Machine määrittämisestä on lähes sama.

1. Projektin lisääminen Visual Studio-ratkaisuun. Napsauta ratkaisun hiiren kakkospainikkeella ja valitse **Lisää** > **Uusi projekti**.

    ![Lisää projekti](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. Lisää **ASP.NET Web-sovelluksen**. 

    ![web-sovelluksen lisääminen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Valitse **MVC** ja Poista kenttä isännän **pilveen** , koska resurssien ryhmän projektiin suorittaa tehtävän.

    ![Valitse MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Kun Visual Studio Luo web app, katso molemmat projektit-ratkaisun.

    ![projektien näyttäminen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Haluat nyt, varmista, että resurssin ryhmän projektin tuntee uusi projekti. Palaa resurssin ryhmän projektin (AzureResourceGroup1). Napsauta **viittaukset** ja valitse **Lisää viittaus**.

    ![Lisää hakemisto](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Valitse web app-projekti, jonka loit.

    ![Lisää hakemisto](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Lisäämällä viittauksen linkittäminen resurssien ryhmä project web app-projekti ja määrittää automaattisesti kolme keskeisiä ominaisuuksia. Näet nämä ominaisuudet viittauksen **Ominaisuudet** -ikkunassa.

      ![Katso viittaus](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
    
    Ominaisuuksia ovat seuraavat:

    - **Lisäominaisuuksien** sisältää web-käyttöönottopaketti väliaikaisen sijaintiin, jossa on miten Azure-tallennustilan. Huomautus kansion (ExampleApp) ja (package.zip)-tiedoston. Nämä arvot toimi parametreiksi, kun sovellus otetaan käyttöön. 
    - **Sisällytä polku** on polku, jossa pakkaus on luotu. **Sisällytä kohteet** on komento, joka suorittaa käyttöönotto. 
    - **Oletusarvon luominen; Paketin** mahdollistaa käyttöönoton voivat laatia ja luoda web käyttöönottopaketti (package.zip).  
    
    Ei olisikaan Julkaise profiili kuin käyttöönoton noutaa tarvittavat tiedot Luo paketti ominaisuuksista.
      
1. Resurssin lisääminen malliin.

    ![Lisää resurssi](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Tällä hetkellä valitsemalla **Web käyttöönottaminen verkkosovelluksissa**. 

    ![Lisää web käyttöönotto](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Ota resurssien ryhmä-projekti, jonka resurssiryhmän uudelleen. Tällä hetkellä on joitakin uuden parametreja. Sinun ei tarvitse antaa arvot **_artifactsLocation** tai **_artifactsLocationSasToken** , koska Visual Studio luo automaattisesti nämä arvot. Sinulla on kuitenkin määrittää kansion ja tiedostonimi (kuten **ExampleAppPackageFolder** ja **ExampleAppPackageFileName** seuraavan kuvan mukaisesti) käyttöönottopaketti sisältävän polun. Anna näyttöön tuli aiemmassa viittaus-ominaisuuksia (**ExampleApp** ja **package.zip**) arvot.

    ![Lisää web käyttöönotto](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    **Palvelutietojen tallennustilan tilin**valitse se otettu käyttöön tässä resurssiryhmä.
    
1. Kun asennus on valmis, valitse web app-portaalissa. Valitse Siirry sivuston URL-osoite.

    ![sivuston selaaminen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. Huomaa, että olet ottanut onnistuneesti oletusarvon ASP.NET-sovellus.

    ![Näytä sovelluksen käyttöön](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja hallinta – portaalin resurssien on artikkelissa [Azure-portaalissa voit hallita Azure resursseja](./azure-portal/resource-group-portal.md).
- Lisätietoja mallit-kohdassa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](resource-group-authoring-templates.md).
