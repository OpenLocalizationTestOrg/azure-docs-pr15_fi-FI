<properties
    pageTitle="Valmistele ja microservices laadukkaampi Azure-käyttöönotto"
    description="Opettele microservices Azure App palvelun yhtenä yksikkönä ja ennakoitavissa tavalla, JSON resurssin ryhmän mallit ja PowerShell-komentosarjojen koostuva-sovelluksen ottaminen käyttöön."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/06/2016"
    ms.author="cephalin"/>


# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Valmistele ja microservices laadukkaampi Azure-käyttöönotto #

Tässä opetusohjelmassa kerrotaan, miten valmistelu ja ottaa käyttöön sovelluksen koostuva [microservices](https://en.wikipedia.org/wiki/Microservices) [Azure App palvelun](/services/app-service/) yhtenä yksikkönä ja JSON resurssin ryhmän mallit ja PowerShell-komentosarjojen ennakoitavissa tavalla. 

Kun valmistelu ja käyttöönotto suuri asteikko-sovelluksia, jotka koostuvat erittäin erillisen microservices, toistettavissa ja ennustettavuutta ovat tärkeitä menestyksen salaisuus. [Azure-sovelluksen-palvelun](/services/app-service/) avulla voit luoda microservices, jotka sisältävät web Apps-sovelluksista, mobiilisovellukset, API-sovellukset ja logiikka sovellusten. [Azure resurssien hallinnan](../azure-resource-manager/resource-group-overview.md) avulla voit hallita kaikkia microservices kokonaisuutena ja resurssien riippuvuudet, kuten tietokannasta ja tietolähteen hallinta-asetukset. Nyt voit myös ottaa jokin sovellus, JSON-mallit ja yksinkertainen PowerShell-komentosarjojen avulla. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-do"></a>Mitä hyötyä ##

Opetusohjelman otetaan käyttöön sovellus, joka sisältää:

-   Kahden web apps (eli kaksi microservices)
-   Taustassa SQL-tietokantaan
-   Sovelluksen asetusten yhteyden merkkijonot ja tietolähteen ohjausobjektin
-   Hakemuksen tiedot, ilmoitukset, automaattisen skaalauksen poistaminen asetukset

## <a name="tools-you-will-use"></a>Voit käyttää Työkalut ##

Tässä opetusohjelmassa voit käyttää seuraavia työkaluja. Koska se ei ole täydellinen keskustelun Työkalut, valitsen helppokäyttöisiä lopusta loppuun-skenaario ja vain Anna lyhyt esittely, ja josta löydät lisätietoja sitä. 

### <a name="azure-resource-manager-templates-json"></a>Azure Resurssienhallinta-mallit (JSON) ###
 
Aina, kun luot verkkosovellukseen Azure-sovelluksen käytössä, esimerkiksi Azure Resurssienhallinta käyttää JSON mallin luomiseen koko resurssiryhmä osan tiedoilla. Monimutkainen mallin [Azure Marketplacesta](/marketplace) , kuten [Skaalattava WordPress](/marketplace/partners/wordpress/scalablewordpress/) sovellus voi olla MySQL-tietokantaan, tallennustilan tilejä, sovelluksen palvelusopimus-web-sovelluksen itse ilmoitusten sääntöjä, sovelluksen asetukset, Automaattinen skaalaus-asetukset ja lisää ja kaikki nämä mallit ovat käytettävissä PowerShellin kautta. Lisätietoja siitä, miten voit ladata ja käyttää malleja on artikkelissa [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md).

Katso lisätietoja Azure Resurssienhallinta-mallien [Yhtä aikaa muiden kanssa Azure Resurssienhallinta-mallit](../resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Visual Studio Azure SDK 2.6 ###

Uusin SDK sisältää parannuksia JSON-editorissa Resurssienhallinta malli-tukeen. Voit tämän toiminnon avulla voit nopeasti resurssin ryhmän mallin luominen alusta alkaen tai avaa aiemmin luotu JSON-malli (kuten ladatut valikoima-malli) muokattavaksi, täytä parametrit-tiedosto ja jopa resurssiryhmä suoraan Azure resurssiryhmä-ratkaisun käyttöönotto.

Lisätietoja on artikkelissa [Azure SDK 2.6 Visual Studio](/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 tai uudempi versio ###

Alku-version 0.8.0, PowerShellin Azure-asennuksen sisältö lisäksi Azure moduulin Azure Resurssienhallinta-moduuli. Uusi moduuli mahdollistaa resurssiryhmät käyttöönoton komentosarja.

Lisätietoja on artikkelissa [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure resurssin Explorer ###

[Esikatselu-työkalun](https://resources.azure.com) avulla voit tarkastella kaikkien resurssien tilauksen ja yksittäisten resurssien ryhmien JSON määritelmät. Hallintatyökalun voit muokata resurssin JSON määritelmät, poista koko hierarkian resurssien ja luo uudet resurssit.  Tietoja helposti saatavilla tässä työkalussa on erittäin hyödyllinen mallin yhtä aikaa, koska se näyttää, mitkä ominaisuudet pitää määrittää tietyn tyyppiseen resurssin oikeat arvot, jne. Voit myös luoda resurssiryhmän [Azure-portaalin](https://portal.azure.com/)ja valitse Tarkasta explorer-työkalu auttaa templatize resurssiryhmän JSON sen määritteitä.

### <a name="deploy-to-azure-button"></a>Ottaa käyttöön Azure-painike ###

Jos käytät GitHub tietolähteen ohjausobjektin, voit sijoittaa [käyttöönotto Azure-painike](/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) kyselyjä oman Lueminut-tiedosto. MD, joka mahdollistaa Ota avaimen käyttöönoton Azure-Käyttöliittymä. Vaikka voit tehdä minkä tahansa yksinkertaisen web App-laajentaa tämän toiminnon käyttöön koko resurssiryhmä käyttöönotto sijoittamalla azuredeploy.json tiedosto säilöön pääkansio. JSON tiedoston, joka sisältää resurssien ryhmä-mallin, käytetään luomaan resurssiryhmän käyttöönotto Azure-painike. Esimerkki on artikkelissa [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) -malli, jota käytät Tässä opetusohjelmassa.

## <a name="get-the-sample-resource-group-template"></a>Hae otoksen resurssien ryhmä-malli ##

Nyt aloitetaan oikean siihen.

1.  Siirry [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) sovelluksen Service-malli.

2.   Valitse readme.md, **Ota Azure**.
 
3.  On otettava [käyttöön-,-azure](https://deploy.azure.com) -sivustoon ja pyytää syötteen käyttöönoton parametrit. Huomaa, että suurin osa kentistä on täydennetty säilön nimi ja jotkin satunnaisia merkkijonot puolestasi. Voit muuttaa kaikki kentät, mutta vain asiat, jotka on annettava ovat SQL Server-järjestelmänvalvojan kirjautuminen ja salasana ja valitse sitten **Seuraava**.
 
    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)

4.  Valitse **Ota** käyttöön käyttöönottoprosessin. Kun prosessi on suoritettu loppuun, valitse http://todoapp*XXXX*. azurewebsites.net linkki sovelluksen selaamalla. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)

    Käyttöliittymän hidastua hieman selatessasi ensin siihen koska sovellukset ovat vain käynnistäminen mutta saada itse, että se on täysin sovellus.

5.  Takaisin sisään käyttöönotto-sivulla napsauttamalla saat näkyviin uuden sovelluksen Azure-portaalissa **hallinta** -linkkiä.

6.  Napsauta **Essentials** avattavasta valikosta resurssien ryhmä-linkkiä. Huomaa myös, että web Appissa on jo yhteydessä GitHub **Ulkoisen projektin**alle säilöön. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
 
7.  Valitse resurssi-ryhmän sivu huomaat, että on jo kaksi web Apps-sovellusten ja yksi resurssiryhmän SQL-tietokantaan.

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)
 
Kaikki, näyttöön tuli vain muutaman lyhyt minuutin kuluttua täysin käyttöön kahden-microservice-sovelluksen, kaikki komponentit, riippuvuudet, asetukset, tietokannan ja jatkuva julkaiseminen määritetään automaattinen tiedonsiirron Azure Resurssienhallinta. Kaikki tämä määritetty kahden asian mukaan:

-   Ota käyttöön Azure-painike
-   azuredeploy.JSON repo pääsivustoon

Voit ottaa käyttöön sama sovellus kymmenlukuun, sataan tai tuhansia ajat ja täsmälleen sama kokoonpano aina. Toistuvuuden ja tämän menetelmän ennustettavuutta mahdollistaa käyttöön high-akseli-sovellusten Helppokäyttötoiminnot ja LUOTTAMUSVÄLI.

## <a name="examine-or-edit-azuredeployjson"></a>Tarkastele AZUREDEPLOY (tai Muokkaa). JSON ##

Nyt katsotaan GitHub säilö määritysten. Sinun oltava JSON-editorin käyttäminen Azure .NET SDK-paketissa, joten jos et ole jo asentanut [Azure .NET SDK 2.6](/downloads/)tehdä sen.

1.  Kloonaa [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) säilö tuttuja git-työkalun avulla. Alla olevassa näyttökuvassa voimme tehdä tämän ryhmän Explorerissa Visual Studio 2013: ssa.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)

2.  Avaa azuredeploy.json Visual Studiossa säilöön-kansiosta. Jos et näe JSON Jäsennys, sinun täytyy asentaa Azure .NET SDK.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Ei valitsen kuvaamaan tarkasti JSON-muoto, mutta [Lisää resursseja](#resources) -osassa on linkkejä käyttöön liittyviä resurssin ryhmän mallin kieli. Tässä kohdassa käytän vain näet, kiinnostavia ominaisuuksia, joiden avulla voit aloittaa tekeminen oman mukautetun mallin sovelluksen käyttöönottoa varten.

### <a name="parameters"></a>Parametrit ###

Tutustu parametrit-osiosta, että nämä parametrit on mitä **käyttöönotto Azure** -painike kysyy, haluatko syöte. Sivuston takana **käyttöönotto Azure** -painiketta Lisää syöte Käyttöliittymän azuredeploy.json määritelty parametreilla. Näitä parametreja käytetään koko resurssin määritelmiä, kuten resurssien nimet, ominaisuusarvoihin jne.

### <a name="resources"></a>Resurssit ###

Resurssit-solmu näet, että määritetään 4 ylimmän tason resurssit, kuten SQL Server-esiintymän, sovellus-palvelusopimus ja kaksi verkkosovelluksissa. 

#### <a name="app-service-plan"></a>Sovelluksen palvelusopimus ####

Aloitetaan JSON yksinkertainen päätason resurssin. Valitse JSON-jäsennys-sovelluksen palvelusopimus nimeltä **[hostingPlanName]** Korosta JSON vastaava koodi. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Huomaa, että `type` elementti määrittää merkkijonon App Service-palvelupakettia, (sitä kutsuttiin palvelinfarmiin sitten pitkiä, pitkä aika), ja muut elementit ja ominaisuudet on täytetty JSON-tiedostossa määritettyjä parametreilla tämän resurssin ei ole sisäkkäisiä resursseja.

>[AZURE.NOTE] Huomaa myös, että arvo `apiVersion` kertoo version käyttämään JSON resurssin määritys mukana REST-Ohjelmointirajapinnalla ja se voi vaikuttaa kuinka resurssin muotoiltu sisällä Azure `{}`. 

#### <a name="sql-server"></a>SQL Server ####

Valitse seuraavaksi SQL Server-resurssi nimeltä JSON jäsennyksessä **SQL Server** .

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)
 
Huomaa seuraavat asiat korostetun JSON-koodi:

-   Parametrien käyttäminen varmistaa, että luodut resurssit ovat nimeltä ja määritetty siten, että tekijöistä yhdenmukaisia toisiinsa.
-   SQL Server-resurssi on kaksi sisäkkäisiä resurssit, jokaisella on eri arvon `type`.
-   Sisäkkäiset resurssit `“resources”: […]`, jossa on määritetty tietokannan ja palomuurisäännöt, on `dependsOn` elementti, joka määrittää päätason SQL Server-resurssin Resurssitunnus. Tämä kertoo Azure Resurssienhallinta "ennen kuin voit luoda tämän resurssin, jotka ovat muu resurssi on jo; ja jos mallissa on määritetty kyseiselle resurssille, luo kyseisen ensin".

    >[AZURE.NOTE] Lisätietoja siitä, miten voit käyttää `resourceId()` toimi, katso lisätietoja artikkelista [Azure Resurssienhallinta malli-Funktiot](../resource-group-template-functions.md).

-   Tarkistamaan, `dependsOn` elementti on, että Azure Resurssienhallinta tietää resursseja voidaan luoda rinnakkain ja resursseja on luotava peräkkäin. 

#### <a name="web-app"></a>Web App-sovelluksessa ####

Nyt siirtyä japanin todellinen web Apps-sovellusten, jotka ovat monimutkaisempia. Valitse Korosta sen JSON-koodin JSON-jäsennyksessä [variables('apiSiteName')]-verkkosovellus. Huomaat asioita uudistamassa paljon mielenkiintoisemmaksi. Tähän tarkoitukseen voin puhua ominaisuuksista yksi kerrallaan:

##### <a name="root-resource"></a>Pääkansio resurssi #####

Web-sovelluksen määräytyy kahden eri resursseja. Tämä tarkoittaa, että Azure Resurssienhallinta luoda web-sovelluksen vain sovelluksen palvelusopimus ja SQL Server-esiintymän luomisen jälkeen.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>Sovelluksen asetukset #####

App-asetukset on määritetty sisäkkäisiä resurssiksi.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

Valitse `properties` elementti `config/appsettings`, sinulla on kaksi sovelluksen asetukset-muodossa `“<name>” : “<value>”`.

-   `PROJECT`on [KUDU asetus](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) , joka kertoo Azure käyttöönoton mikä projektin käyttäminen usean projektin Visual Studio-ratkaisuun. Voin näet, miten myöhemmin tietolähteen ohjausobjekti on määritetty, mutta ToDoApp-koodi on usean projektin Visual Studio-ratkaisuun, annettava tämän asetuksen.
-   `clientUrl`on vain asetus, joka sovelluksen koodi sovelluksen käyttää.

##### <a name="connection-strings"></a>Yhteysmerkkijonon #####

Yhteyden merkkijonot määritetään sisäkkäisiä resurssiksi.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

Valitse `properties` elementti `config/connectionstrings`, nimi: arvo-pari, joissa muotoilu myös määritellään kunkin yhteysmerkkijonon `“<name>” : {“value”: “…”, “type”: “…”}`. Saat `type` elementti, mahdolliset arvot ovat `MySql`, `SQLServer`, `SQLAzure`, ja `Custom`.

>[AZURE.TIP] Lopullinen luettelo yhteyden merkkijonon tyypit, suorita seuraava komento PowerShellin Azure: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
    
##### <a name="source-control"></a>Tietolähteen ohjausobjekti #####

Tietolähteen hallinta-asetukset on määritetty sisäkkäisiä resurssiksi. Azure Resurssienhallinta käyttää tämän resurssin määrittämiseen jatkuva julkaiseminen (Katso caveat `IsManualIntegration` myöhemmin) ja myös, jos haluat projektin koodi sovelluksen käyttöönottoa automaattisesti JSON-tiedoston käsittelemisen aikana.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`ja `branch` pitäisi olla melko intuitiivisen ja osoitettava Git tietovaraston ja julkaistava-haaran nimi. Uudelleen ne on määritetty syöteparametrit mukaan. 

Huomaa `dependsOn` elementti, joka lisäksi web app-resurssin itse `sourcecontrols/web` riippuu myös `config/appsettings` ja `config/connectionstrings`. Tämä johtuu siitä, kun `sourcecontrols/web` on määritetty, Azure käyttöönottoprosessin yrittää automaattisesti käyttöön, luominen ja Käynnistä sovellus-koodi. Vuoksi lisääminen tämä riippuvuus avulla voit varmistaa, että sovellus on käyttöoikeudet tarvittavat sovelluksen asetusten ja yhteyden merkkijonojen ennen sovelluksen koodi suoritetaan. 

>[AZURE.NOTE] Huomaa myös, että `IsManualIntegration` on määritetty `true`. Tämä ominaisuus on tarpeen tässä opetusohjelmassa, koska et itse asiassa omista GitHub säilöön, ja näin ei varsinaisesti myöntää Azure, voit määrittää jatkuva julkaistaan [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) (eli siirtää automaattisen säilöön päivitykset Azure). Voit käyttää oletusarvoa `false` määritetyn säilön vain, jos olet määrittänyt omistajan GitHub tunnistetiedot ennen [Azure portal](https://portal.azure.com/) . Toisin sanoen jos olet määrittänyt tietolähteen ohjausobjektin GitHub tai BitBucket minkä tahansa sovelluksen [Azure Portal](https://portal.azure.com/) aiemmin, käyttäjän-tunnistetiedoillasi sitten Azure muista tunnistetiedot ja käyttää niitä aina, kun kaikki GitHub tai BitBucket sovelluksen käyttöönotto myöhemmin. Jos et ole vielä tämä käyttöönoton JSON mallin epäonnistuu kun Azure Resurssienhallinta yrittää määrittää web-sovelluksen tietolähteen hallinta-asetukset, koska se ei voi kirjautua GitHub tai BitBucket säilöön omistajan tunnuksilla.

## <a name="compare-the-json-template-with-deployed-resource-group"></a>Vertaa JSON mallin käyttöön resurssiryhmä ##

Tässä kohdassa voit käydä läpi kaikki web sovelluksen lavat [Azure-portaalissa](https://portal.azure.com/), mutta ei muuta työkalua, joka ei aivan hyödyllinen, jos Lisää. Siirry [Azure resurssin Explorer](https://resources.azure.com) esikatselu-työkalua, jonka voit resurssiryhmät JSON esittäminen-tilausten, ne ovat todella Azure Taustajärjestelmä. Näet myös, miten Azure resurssiryhmä JSON hierarkian vastaa mallitiedoston, jota käytetään asiakirja sitten kannattaa luoda hierarkian.

Esimerkiksi kun Siirry [Azure resurssin Explorer](https://resources.azure.com) -työkalua ja laajentamalla solmut hallinnassa on näkyvissä resurssiryhmän ja päätason resurssien avulla kerätään-kohdassa niiden vastaaviin resurssityypit.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Jos verkkosovellukseen alirakenteeseen, sinun pitäisi nähdä web app määritysten tiedot samalla näyttökuva alla:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Taas sisäkkäisiä resurssien pitäisi olla samankaltainen kaltaisia JSON mallitiedosto hierarkian ja pitäisi näkyä app-asetukset, yhteysmerkkijonoja, jne., näkyvät oikein JSON-ruudussa. Nämä asetukset ei ole ongelma saattaa johtua JSON-tiedoston, ja auttavat vianmääritys JSON mallitiedosto.

## <a name="deploy-the-resource-group-template-yourself"></a>Ottaa resurssien ryhmä-malli ##

**Ota Azure** -painike on erinomainen, mutta sen avulla voit ottaa käyttöön azuredeploy.json resurssien ryhmä-mallissa, vain, jos sinulla on jo miten azuredeploy.json GitHub. Azure .NET SDK on myös työkaluja, voit ottaa käyttöön minkä tahansa JSON-mallitiedoston suoraan paikallisessa tietokoneessa. Toiminto, noudata seuraavia ohjeita:

1.  Valitse Visual Studion **Tiedosto** > **Uusi** > **projektin**.

2.  Valitse **Visual C#** > **Cloud** > **Azure resurssiryhmä**, valitse **OK**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)

3.  **Valitse Azure-malli**Valitse **Tyhjä malli** ja valitse **OK**.

4.  Vedä azuredeploy.json uuden projektin **Mallit** -kansioon.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)

5.  Avaa ratkaisunhallinnassa, kopioidut azuredeploy.json.

6.  Vain esittelyyn, vuoksi lisääminen vakio sovelluksen tietoja resurssien Microsoftin JSON-tiedostoon valitsemalla **Lisää resurssin**. Jos haluat muuttaa vain käyttöönottoon JSON-tiedostoa, siirry käyttöönotto-ohjeita.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)

7.  Valitse **Sovelluksen tietoa Web Apps -sovelluksista**, varmista, että nykyinen sovellus-palvelu, suunnitella ja web App-sovelluksessa on valittuna, ja valitse sitten **Lisää**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)

    Voit nyt voi nähdä useita uusia resursseja, että sen mukaan, resurssi ja kuvaus, ovat riippuvaisia App palvelusopimus tai web app. Nämä resurssit eivät ole käytössä niiden aiemmin määritelmän ja haluat muuttaa.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
 
8.  Valitse Korosta sen JSON-koodin **appInsights Automaattinen skaalaus** JSON-jäsennys. Tämä on sovelluksen palvelusopimus skaalauksen-asetusta.

9.  Korostettu JSON-koodia, Etsi `location` ja `enabled` ominaisuudet ja määrittää ne alla kuvatulla tavalla.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)

10. Valitse Korosta sen JSON-koodin **CPUHigh appInsights** JSON-jäsennys. Tämä on ilmoituksen.

11. Etsi `location` ja `isEnabled` ominaisuudet ja määrittää ne alla kuvatulla tavalla. Toimi samoin muut kolme ilmoitukset (Violetti sipulit).

    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)

12. Olet nyt valmis ottamaan. Projektin hiiren kakkospainikkeella ja valitse **Ota käyttöön** > **Uudet käyttöönoton**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)

13. Kirjaudu sisään Azure-tili, jos et ole vielä tehnyt niin.

14. Valitse aiemmin luotu resurssiryhmä-tilaukseesi tai luoda uuden, valitse **azuredeploy.json**ja valitse sitten **Muokkaa parametrit**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)

    Voit nyt voi muokata kaikkia mallitiedostossa nice taulukon parametrit. Oletukset määrittävät parametrit on jo oletusarvot ja sallittujen arvoluettelon määrittävät parametrit näkyvät avattavista luetteloista.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)

15. Täytä tyhjään parametrit ja käyttää [ToDoApp GitHub repo osoite](https://github.com/azure-appservice-samples/ToDoApp.git) **repoUrl**. Valitse **Tallenna**.
 
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)

    >[AZURE.NOTE] Automaattisen skaalauksen poistaminen toimintoa tarjota **Vakio** taso tai uudempi versio ja suunnitelman tason hälytykset ominaisuuksia tarjota **Basic** taso, tai sitä uudempi versio, sinun on määritettävä **tuote** -parametrin **Vakio** tai **Premium** jotta voit nähdä kaikki uuden sovelluksen tietoja resurssien Vaalea ylöspäin.
    
16. Valitse **Ota käyttöön**. Jos valitsit **tallentaa salasanoja**, salasana tallennetaan parametrin tiedoston **tekstimuodossa**. Muussa tapauksessa sinua pyydetään Kirjoita tietokannan salasana käyttöönottoprosessin aikana.

Joka on tämä! Nyt haluat Siirry [Azure-portaalin](https://portal.azure.com/) ja [Azure resurssin Explorer](https://resources.azure.com) -työkalua nähdä uuden ilmoitukset ja lisätä oman JSON Automaattinen skaalaus-asetukset käyttöön sovellus.

Tämän osan vaiheet toteuttaa pääasiassa seuraavasti:

1.  Valmiiden mallitiedosto
2.  Luonut parametrin tiedoston mukana mallitiedosto
3.  Ottaa käyttöön mallitiedosto, jonka parametri

Viimeisessä vaiheessa tehdään helposti PowerShell-cmdlet-komennon. Jos haluat nähdä, mitä Visual Studio asentaminen, kun se on otettu käyttöön sovelluksen, Avaa Scripts\Deploy AzureResourceGroup.ps1. Ole koodi on useita, mutta vain valitsen Korosta olennaiset koodia haluat ottaa mallitiedosto, jonka parametri.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

Viimeinen cmdlet-komento, `New-AzureResourceGroup`, on todella suorittaa toiminnon haluamasi vaihtoehto. Kaikki tämä olisi osoitettava sinulle, sillä avulla, on suhteellisen yksinkertaista ottamaan cloud sovelluksen laadukkaampi. Aina, kun suoritat cmdlet samaa Parametritiedostoa samaan malliin, jotka aiot saman tuloksen.

## <a name="summary"></a>Yhteenveto ##

DevOps toistettavissa ja ennakoitavuutta ovat onnistuneen käytön microservices koostuva suuri asteikko-sovelluksen pikanäppäimet. Tässä opetusohjelmassa on otettu käyttöön, Azure kahden microservice sovelluksen koottuja ryhmänä Azure Resurssienhallinta-mallin avulla. Hopefully se on antanut sinulle voit koota Käynnistä sovelluksen Azure muuntaminen mallin valmistelu ja ottaa käyttöön laadukkaampi knowledge. 

## <a name="next-steps"></a>Seuraavat vaiheet ##

Lue, miten [käyttää joustava menetelmiä ja julkaista jatkuvasti microservices sovelluksen vaivattomasti](app-service-agile-software-development.md) ja käyttöönoton tekniikoita, kuten [flighting käyttöönoton](app-service-web-test-in-production-controlled-test-flight.md) helposti.

<a name="resources"></a>
## <a name="more-resources"></a>Lisää resursseja ##

-   [Azure Resurssienhallinta mallin kieli](../resource-group-authoring-templates.md)
-   [Authoring Azure Resurssienhallinta-mallit](../resource-group-authoring-templates.md)
-   [Azure Resurssienhallinta malli-Funktiot](../resource-group-template-functions.md)
-   [Sovelluksen Azure Resurssienhallinta mallin ottaminen käyttöön](../resource-group-template-deploy.md)
-   [Azure PowerShell kanssa Azure resurssien hallinnan avulla](../powershell-azure-resource-manager.md)
-   [Resurssiryhmä-käyttöönotoissa Azure-tietokannassa vianmääritys](../resource-manager-troubleshoot-deployments-portal.md)




 
