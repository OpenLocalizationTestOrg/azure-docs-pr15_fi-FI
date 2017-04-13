<properties
    pageTitle="API-sovellusten ja ASP.NET-sovelluksen-palvelun käytön aloittaminen | Microsoft Azure"
    description="Lue, miten voit luoda ja ottaa käyttöön tarjoaman Azure App palvelun ASP.NET API-sovelluksen avulla Visual Studio 2015."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="rachelap"/>

# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>API-sovellukset, ASP.NET ja Swagger Azure sovelluksen-palvelun käytön aloittaminen

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Tämä on ensimmäinen opetusohjelmia, jotka osoittavat, miten käyttää Azure App palvelun toimintoja, jotka auttavat kehittämään ja isännöinnissä RESTful ohjelmointirajapinnan numerosarjojen.  Tässä opetusohjelmassa kerrotaan tuki API metatietojen Swagger-muodossa.

Opit:

* Voit luoda ja Azure App Service [API sovellukset](app-service-api-apps-why-best-platform.md) käyttöönotto Visual Studio 2015 työkalujen avulla.
* Voit automatisoida API etsiminen Swashbuckle NuGet-paketin avulla luodaan dynaamisesti Swagger API metatiedot.
* Miten luo automaattisesti Asiakas-koodi API-sovelluksen Swagger API metatietojen avulla.

## <a name="sample-application-overview"></a>Esimerkki sovellusten yleiskuvaus

Tässä opetusohjelmassa käsitteleminen Yksinkertainen Tehtävät-luettelon otoksen-sovellus. Sovellus on yhden sovelluksen (SPA) edusta ASP.NET-verkko-Ohjelmointirajapinnan Keskitaso ja ASP.NET-verkko-Ohjelmointirajapinnan-tietojen taso.

![Sovelluksen API sovellusten mallikaavio](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Seuraavassa on [AngularJS](https://angularjs.org/) edusta näyttökuva.

![API sovellusten Esimerkki sovelluksen tehtäväluettelo](./media/app-service-api-dotnet-get-started/todospa.png)

Visual Studio-ratkaisun sisältää kolme projektia:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** - edustatietokanta: AngularJS-SPA, joka kutsuu keskitaso.

* **ToDoListAPI** - Keskitaso: ASP.NET-verkko-Ohjelmointirajapinnan-projekti, joka kutsuu tietojen tason Tehtäväkohteita CRUD toimintojen suorittamiseen.

* **ToDoListDataAPI** - tietojen tason: ASP.NET-verkko-Ohjelmointirajapinnan-projekti, joka suorittaa Tehtäväkohteita CRUD laskutoimituksia.

Kolmen tason-arkkitehtuuri on monta arkkitehtuureihin, voit toteuttaa API-sovellusten avulla ja käytetään tässä vain esittelyä varten. Kunkin tason koodi on mahdollisimman yksinkertainen, osoittamaan API sovellusten ominaisuuksista. esimerkiksi tietojen tason käyttää palvelimen muistia tietokannan sijaan sen pysyvyyttä järjestelmä.

Tässä opetusohjelmassa on valmis, valitse on kaksi verkko-Ohjelmointirajapinnan projektia, määrittäminen ja suorittamalla pilveen App Service API-sovelluksissa.

Sarjan seuraavaan opetusohjelmaan ottaa käyttöön SPA edusta pilveen.

## <a name="prerequisites"></a>Edellytykset

* ASP.NET-verkko-Ohjelmointirajapinnan - opetusohjelman ohjeita oletetaan basic tuntemus ASP.NET- [Verkko-Ohjelmointirajapinnan 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) Visual Studiossa käsittelemisestä on.

* Azure-tili – voit [avata Azure-tili maksutta](/pricing/free-trial/?WT.mc_id=A261C142F) tai [aktivoida Visual Studio tilaajan etuja](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

    Jos haluat aloittaa Azure App palvelun, ennen kuin kirjaudut Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751). Voit luoda lyhytkestoinen starter-sovelluksen heti sovelluksen käytössä, **ei ole luottokortin kohteet**ja ei ole sitoumukset.

* Visual Studio 2015 [Azure SDK.NET](https://azure.microsoft.com/downloads/archive-net-downloads/) - SDK ja asentaa Visual Studio 2015 automaattisesti, jos sinulla ei ole jo.

    * Visual Studiossa, valitse Ohje -> Tietoja Microsoft Visual Studio ja varmista, että sinulla on "Azure App palvelun Työkalut v2.9.1" tai uudempi.

    ![Azure sovelluksen Työkalut vesion](./media/app-service-api-dotnet-get-started/apiversion.png)

    >[AZURE.NOTE] Sen mukaan, kuinka monta SDK riippuvuudet on jo käyttämääsi laitteeseen SDK asentaminen saattaa kestää kauan, useita minuuteista vähintään puolessa tunnissa.

## <a name="download-the-sample-application"></a>Lataa sovelluksen malli

1. Lataa [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) säilö.

    Voit **Ladata ZIP** -painiketta tai Kloonaa säilö paikallisessa tietokoneessa.

2. Avaa ToDoList-ratkaisun Visual Studio 2015 tai 2013.
   1. Sinun on luotettava kunkin ratkaisu.
        ![Suojausvaroitus](./media/app-service-api-dotnet-get-started/securitywarning.png)

3. Rakenna ratkaisu (CTRL + VAIHTO + B) palauttaa NuGet-paketit.

    Jos haluat nähdä toiminnon sovelluksen ennen niiden käyttöönottoa, voit suorittaa sen paikallisesti. Varmista, että ToDoListDataAPI on käynnistys projektin ja suorita ratkaisu. Odotat tulee näkyviin HTTP 403-virhe selaimessa.

## <a name="use-swagger-api-metadata-and-ui"></a>Käytä Swagger API metatietojen ja Käyttöliittymä

API metatietojen [Swagger](http://swagger.io/) 2.0-tuki on valmiina Azure App palvelu. Kunkin API-sovelluksen voit määrittää URL-Osoitteen päätepisteen, joka palauttaa metatietojen API Swagger JSON-muodossa. Kyseisen päätepisteen palauttanut metatietojen avulla voidaan luoda asiakas-koodin.

ASP.NET-verkko-Ohjelmointirajapinnan project voi luoda Swagger metatietojen dynaamisesti [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-paketin avulla. Swashbuckle NuGet-paketti on jo asennettu ToDoListDataAPI ja ToDoListAPI projekteissa, jotka olet ladannut.

Tämän opetusohjelman-osassa voit tarkastella luotu Swagger 2.0-metatiedot ja yritä, Testaa Käyttöliittymä, joka perustuu Swagger metatiedot.

1. Määritä projektin aloitus ToDoListDataAPI projektin (**ei** ToDoListAPI projektin).

    ![Projektin aloitus ToDoDataAPI määrittäminen](./media/app-service-api-dotnet-get-started/startupproject.png)

2. Painamalla F5-näppäintä tai valitse **Virheenkorjaus > Käynnistä virheenkorjaus** Suorita projekti virheenkorjaus-tilassa.

    Selain avautuu ja näyttää HTTP 403 virhesivun.

3. Selaimen osoiterivillä Lisää `swagger/docs/v1` loppuun rivi ja paina sitten Rivinvaihtonäppäintä. (URL-osoite on `http://localhost:45914/swagger/docs/v1`.)

    Tämä on käyttää Swashbuckle palauttaa Swagger 2.0 JSON metatietojen Ohjelmointirajapinnan oletusarvoinen URL-osoite.

    Jos käytät Internet Explorer-selain pyytää sinua lataamaan *v1.json* tiedoston.

    ![Lataa IE JSON metatiedot](./media/app-service-api-dotnet-get-started/iev1json.png)

    Jos käytät Chrome, Firefox tai reuna, selain näyttää JSON selainikkunassa. Eri selaimissa JSON käsitellään eri tavalla ja selainikkunan voi etsiä täsmälleen esimerkin.

    ![JSON metatietojen Chromessa](./media/app-service-api-dotnet-get-started/chromev1json.png)

    Seuraavassa esimerkissä näkyy Swagger metatietojen ensimmäinen osa Ohjelmointirajapinnan kanssa määritelmä Get-menetelmää. Metatiedot on mitä ohjaa Swagger Käyttöliittymä, jota käytetään seuraavissa vaiheissa ja sen avulla opetusohjelman myöhemmin osassa luo automaattisesti Asiakas-koodi.

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

4. Sulje selain ja Lopeta Visual Studio virheenkorjaus.

5. Napsauta **Ratkaisunhallinnassa**ToDoListDataAPI, projektin *App_Start\SwaggerConfig.cs* -tiedoston sitten rivin 174 kohtaan ja kommentointi seuraava koodi.

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    *SwaggerConfig.cs* -tiedosto luodaan, kun olet asentanut Swashbuckle paketin projektin. Tiedosto on useita tapoja määrittäminen Swashbuckle.

    Olet uncommented koodi mahdollistaa Swagger Käyttöliittymä, jota käytetään seuraavien ohjeiden mukaisesti. Kun luot projektin verkko-Ohjelmointirajapinnan API app project-mallin avulla, koodi on Kommentoitu ulos oletusarvoisesti suojauksen mittaa.

6. Suorita projektin uudelleen.

7. Selaimen osoiterivillä Lisää `swagger` loppuun rivi ja paina sitten Rivinvaihtonäppäintä. (URL-osoite on `http://localhost:45914/swagger`.)

8. Kun Swagger Käyttöliittymän-sivu tulee näkyviin, valitse **ToDoList** Saat käytettävissä.

    ![Swagger Käyttöliittymän käytettävissä olevat menetelmät](./media/app-service-api-dotnet-get-started/methods.png)

9. Napsauta luettelon ensimmäinen **Hae** -painiketta.

10. Kirjoita tähti **Parametrit** -kohdan arvoksi `owner` parametri, ja valitse sitten **Kokeile suoritetaan**.

    Kun lisäät todennus myöhemmin opetusohjelmat, keskitaso on todellinen Käyttäjätunnus tietojen taso. Nyt kaikki tehtävät on tähti omistaja niiden tunnisteeksi samalla, kun sovellus suoritetaan ilman todennus on käytössä.

    ![Swagger Käyttöliittymän Kokeile](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    Swagger-Käyttöliittymän kutsuu ToDoList Get menetelmä ja näyttää vastauksen koodi ja JSON tulokset.

    ![Swagger Käyttöliittymän kokeile, tulokset](./media/app-service-api-dotnet-get-started/gettryitout.png)

11. Valitse **Julkaise**ja valitse sitten ruudun kohdassa **Mallin rakenne**.

    Tekstiruutu, jossa voit määrittää Post-menetelmää parametriarvo prefills valitsemalla mallin rakenne. (Jos tämä ei toimi Internet Explorerissa, käytä toista selainta tai parametriarvo kirjoittaa seuraavan vaiheen.)  

    ![Swagger Käyttöliittymän kokeile, Kirjaa](./media/app-service-api-dotnet-get-started/post.png)

12. Muuta JSON- `todo` parametrin syötetty kenttään niin, että se näyttää samalta kuin seuraavassa esimerkissä tai korvaa kuvaus omalla tekstillä:

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

13. Valitse **Kokeile suoritetaan**.

    ToDoList API palauttaa HTTP 204 vastauksen koodi, joka ilmaisee onnistui.

14. Napsauta ensimmäisen **Hae** -painiketta ja valitse sitten sivun osan **Kokeile ulos** -painiketta.

    Get-menetelmä vastaus sisältää nyt uusi kohde.

15. Valinnainen: Kokeile myös hyllytetty, poistaa, ja poistaa tunnus menetelmillä.

16. Sulje selain ja Lopeta Visual Studio virheenkorjaus.

Swashbuckle toimii ASP.NET-verkko-Ohjelmointirajapinnan projektin. Jos haluat lisätä Swagger metatietojen luominen aiemmin luodusta projektista, asenna Swashbuckle-paketti.

>[AZURE.NOTE] Swagger metatietojen on yksilöllinen tunnus kutakin API-toimintoa. Oletusarvon mukaan Swashbuckle voi luoda kaksoiskappaleet Swagger toiminnon tunnukset verkko-Ohjelmointirajapinnan-ohjain menetelmistä. Tämä tapahtuu, jos ohjaimen on ylikuormitettu HTTP-menetelmiä, kuten `Get()` ja `Get(id)`. Lisätietoja Osastollasi käsittelemisestä on artikkelissa [mukauttaminen Swashbuckle luodut API määritykset](app-service-api-dotnet-swashbuckle-customize.md). Jos luot verkko-Ohjelmointirajapinnan projektin Visual Studiossa Azure API App mallin avulla, koodi, joka luo yksilölliset tunnukset-toiminnon lisätään automaattisesti *SwaggerConfig.cs* -tiedosto.  

## <a id="createapiapp"></a>API-sovelluksen luominen Azure ja ota se käyttöön koodi

Tässä osassa voit käyttää Azure työkaluja, joilla on integroitu ohjatun Visual Studio **Julkaise** uusi API-sovelluksen luominen Azure-tietokannassa. Valitse Ota käyttöön uuden API-sovelluksen ToDoListDataAPI-projekti ja soita Ohjelmointirajapinnan suorittamalla Swagger-Käyttöliittymä.

1. **Ratkaisunhallinnassa**ToDoListDataAPI projektin hiiren kakkospainikkeella ja valitse sitten **Julkaise**.

    ![Valitse Julkaise Visual Studiossa](./media/app-service-api-dotnet-get-started/pubinmenu.png)

2.  Valitse ohjatun **Sivuston julkaisemisen** **profiili** -vaiheessa **Microsoft Azure App palvelua**.

    ![Valitse Azure App palvelun sivuston julkaiseminen](./media/app-service-api-dotnet-get-started/selectappservice.png)

3. Kirjautuminen Azure-tili, jos et ole vielä tehnyt niin tai päivitä tunnistetiedot, jos ne on päättynyt.

4. Valitse sovelluksen Service-valintaikkunassa Azure **tilauksen** , jota haluat käyttää ja valitse sitten **Uusi**.

    ![Valitse uusi sovellus-palvelun valintaikkuna](./media/app-service-api-dotnet-get-started/clicknew.png)

    **Luo sovelluksen palvelu** -valintaikkunan **Hosting** -välilehti tulee näkyviin.

    Koska olet käyttöönotto verkko-Ohjelmointirajapinnan projektista, jossa on asennettu Swashbuckle, Visual Studio oletetaan, että haluat API-sovelluksen luominen. Tämä on merkitty **API sovelluksen nimi** -otsikon mukaan ja se, että **Muuta tyyppi** avattavasta luettelosta on määritetty **API**-sovellukseen.

    ![Sovelluksen tyyppi App Service-valintaikkunassa](./media/app-service-api-dotnet-get-started/apptype.png)

5. Kirjoita **API sovelluksen nimi** , joka on yksilöllinen *azurewebsites.net* toimialueeseen. Voit hyväksyä oletusnimen, jota Visual Studio ehdottaa.

    Jos kirjoitat nimi, jonka joku muu on jo käytössä, näkyviin tulee punainen huutomerkki oikealle.

    API-sovelluksen URL-osoite on `{API app name}.azurewebsites.net`.

6. **Resurssiryhmä** avattavasta, valitse **Uusi**ja kirjoita sitten "ToDoListGroup" tai muu nimi, jos käytät mieluummin.

    Resurssiryhmä on kokoelma Azure resursseja, kuten API sovellukset, VMs, ja niin edelleen. Tässä opetusohjelmassa on helpointa luoda uusi resurssiryhmä, koska, joka on helppo poistaa yhdessä vaiheessa Azure resurssien avulla voit luoda opetusohjelman.

    Tässä ruudussa voit aiemmin luotu [resurssiryhmä](../azure-resource-manager/resource-group-overview.md) tai luoda uuden kirjoittamalla nimi, joka ei ole sama kuin mikä tahansa aiemmin resurssiryhmä-tilaukseesi.

7. Napsauta vieressä **App palvelun suunnitteleminen** avattavasta **Uusi** -painiketta.

    Olevassa näyttökuvassa esitetään esimerkkiarvojen **API sovelluksen nimi**, **tilaus**ja **Resurssiryhmä** --, että arvot ovat erilaisia.

    ![Luo sovelluksen Service-valintaikkuna](./media/app-service-api-dotnet-get-started/createas.png)

    Seuraavia ohjeita voit luoda uusi resurssiryhmä App Service-suunnitteleminen. Sovelluksen palvelusopimus määrittää Laske resurssit, joiden API-sovellus toimii. Esimerkiksi jos vapaa taso, API-sovellus toimii jaetun VMs joitakin maksettu tasoa, se suorittamisen erillinen VMs aikana. Sovelluksen palvelusopimusten vaihtoehdot tietoja on artikkelissa [sovelluksen palvelun suunnitelmien yleiskatsaus](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

8. Kirjoita **Sovelluksen-palvelun suunnittelu määrittäminen** -valintaikkunan "ToDoListPlan" tai muu nimi, jos käytät mieluummin.

9. Valitse **sijainti** avattavasta luettelosta, joka on lähimpänä sinulle sijaintiin.

    Tämä asetus määrittää, mitkä Azure palvelinkeskuksen sovelluksen suoritetaan. Valitse sijainti, voit pienentää [viive](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)lähellä.

10. Valitse **Valintaluettelo,** **vapaa**.

    Tässä opetusohjelmassa vapaa hinnoittelu taso antaa suorituskyky riittää.

11. **Määritä sovellus palvelun suunnitteleminen** -valintaikkunan valitsemalla **OK**.

    ![OK Valitse kohdassa Määritä sovellus-palvelusopimus](./media/app-service-api-dotnet-get-started/configasp.png)

12. **Luo sovelluksen Service** -valintaikkunassa valitsemalla **Luo**.

    ![Valitse Luo sovelluksen palvelun luominen-valintaikkuna](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Visual Studio Luo API-sovellus ja Julkaise-profiili, jossa on kaikki tarvittavat asetukset API-sovellukseen. Avaa sitten ohjatun **Sivuston julkaiseminen** , joka tulee käyttää Ota käyttöön-projekti.

    Ohjatun **Sivuston julkaisemisen** avautuu **yhteys** -välilehti (kuten alla).

    **Yhteys** -välilehti **ja **sivustonimi** ** -asetusten osoittamalla API-sovellus. **Käyttäjänimi** ja **salasana** ovat käyttöönoton tunnistetiedot, jotka Azure luo puolestasi. Käyttöönoton jälkeen Visual Studio avautuu selaimessa **Linkin URL-osoite** (joka on tarkoitus vain **Linkin URL-osoite**).  

13. Valitse **Seuraava**.

    ![Valitse seuraava Julkaise yhteys-välilehti](./media/app-service-api-dotnet-get-started/connnext.png)

    Seuraavaan välilehteen on **asetukset** -välilehti (kuten alla). Tässä voit muuttaa ottamaan virheenkorjaus koontiversio, virheiden poistamiseen [etäyhteyden](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)muodosta määritys-välilehti. Välilehti on myös useita **Tiedoston Julkaisuasetukset**:

    * Poista kohde muita tiedostoja
    * Precompile julkaisemisen aikana
    * Tiedostojen jättäminen pois App_Data-kansioon

    Tässä opetusohjelmassa, sinun ei tarvitse jokin seuraavista. Etsimistä toiminnoista on artikkelissa [Toimintaohje: käyttöönotto Web projektin käyttämällä yhdellä napsautuksella julkaiseminen Visual Studiossa](https://msdn.microsoft.com/library/dd465337.aspx).

14. Valitse **Seuraava**.

    ![Valitse seuraava asetukset-välilehdessä sivuston julkaiseminen](./media/app-service-api-dotnet-get-started/settingsnext.png)

    Seuraavaksi on **Esikatselu** -välilehdessä (kuten alla), mitä näet tiedostot aiot voi kopioida projektin API-sovelluksen avulla. Kun olet käyttöönotto projektin API-sovellukseen, jonka jo ottanut käyttöön aiemmin, vain muuttuneet tiedostot kopioidaan. Jos haluat nähdä luettelon mitä kopioidaan, voit valita **Käynnistä esikatselu** -painiketta.

15. Valitse **Julkaise**.

    ![Valitse Julkaise esikatselu-välilehdessä sivuston julkaiseminen](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Visual Studio ottaa käyttöön ToDoListDataAPI projektin uusi API-sovellukseen. **Kohde** -ikkunassa kirjautuu onnistunut käyttöönotto ja "onnistunut"-sivu tulee näkyviin selainikkunassa avattu API-sovelluksen URL-osoite.

    ![Tulosteen ikkunan onnistunut käyttöönotto](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![Uusi sovellus on luotu API-sivu](./media/app-service-api-dotnet-get-started/appcreated.png)

16. Lisää "swagger" URL-osoite selaimen osoiteriville ja paina sitten Enter-näppäintä. (URL-osoite on `http://{apiappname}.azurewebsites.net/swagger`.)

    Selain näyttää saman Swagger Käyttöliittymän, näyttöön tuli aiemmin, mutta nyt se toimii pilveen. Get-menetelmä Kokeile ja huomaat, että olet takaisin oletusarvon 2 Seurattavien kohteiden. Paikallinen kone muistissa on tallennettu aiemmin tekemäsi muutokset.

17. Avaa [Azure portal](https://portal.azure.com/).

    Azure-portaali on web-liittymän Azure resurssit, kuten API sovellusten hallintaan.

18. Valitse **Lisää palveluja > App Services**.

    ![Etsi sovellus palvelut](./media/app-service-api-dotnet-get-started/browseas.png)

19. Etsi ja valitse uusi API-sovellus **App palvelut** -sivu. (Azure-portaalissa, joka avaa oikealla windows kutsutaan *lavat*.)

    ![Sovelluksen palvelut-sivu](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    Kaksi lavat Avaa. Yksi sivu on yleiskatsaus API-sovellus ja yksi on paljon, voi tarkastella ja muuttaa asetuksia.

20. Etsi **Ohjelmointirajapinta** -osio ja **API**määritelmä **asetukset** -sivu.

    ![API määritelmän asetukset-sivu](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    **API määritys** -sivu avulla voit määrittää URL-Osoitetta, joka palauttaa Swagger 2.0 metatietojen JSON-muodossa. Kun Visual Studio Luo API-sovellus, se asettaa API määritelmä URL-Osoitteen oletusarvoon, Swashbuckle luodut metatiedot, joilla näyttöön tuli aiempaa versiota, joka on API-sovellus on kantaluku URL-osoite sekä `/swagger/docs/v1`.

    ![API määritelmä URL-osoite](./media/app-service-api-dotnet-get-started/apidefurl.png)

    Kun valitset API sovelluksen Luo asiakkaan koodi, Visual Studio hakee metatiedot tätä URL-Osoitetta.

## <a id="codegen"></a>Luo tietojen tason asiakas-koodi

Swagger integroiminen Azure API sovellusten eduista on automaattinen koodin luominen. Luotua Luokat helpottavat kirjoittaa koodia, soittaa API-sovelluksen.

ToDoListAPI projekti on jo luotu asiakas-koodi, mutta seuraavissa vaiheissa poistaa sen ja uudelleen, niin näet koodin luominen hallinnasta.

1. Visual Studio **Ratkaisunhallinnassa**ToDoListAPI Projectissa Poista *ToDoListDataAPI* -kansio. **Varoitus: Poistaa vain kansion, et ToDoListDataAPI projektissa.**

    ![Poista luotua koodi](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    Tämä kansio on luotu käyttämällä koodin luontiprosessi, jotka aiot käydä läpi.

2. Napsauta ToDoListAPI-projekti ja valitse sitten **Lisää > REST API Client**.

    ![REST API asiakkaan lisääminen Visual Studio](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. **Lisää REST API asiakas** -valintaikkunassa **Swagger URL-osoite**ja valitse sitten **Valitse Azure resurssi**.

    ![Valitse Azure resurssi](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

4. Valitse **Sovelluksen Service** -valintaikkunassa Laajenna resurssiryhmä käyttämäsi Tässä opetusohjelmassa ja valitse API-sovellus ja valitse sitten **OK**.

    ![Valitse API apusovellus koodin luominen](./media/app-service-api-dotnet-get-started/codegenselect.png)

    Huomaa, kun palaat **Lisää REST API asiakas** -valintaikkuna, tekstiruutu on annettu sisään API-määritys URL-osoite-arvo, joka näyttöön tuli aiemmassa portaalin.

    ![API määritelmä URL-osoite](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] Vaihtoehtoinen tapa hankkia metatietojen koodin luominen on URL-osoitteen suoraan kautta, vaan Selaa-valintaikkunassa. Tai jos haluat luoda asiakas-koodin ennen kuin otat käyttöön Azure voi suorittaa verkko-Ohjelmointirajapinnan projektin paikallisesti, siirry URL-osoitteeseen, joka sisältää Swagger JSON-tiedosto, Tallenna tiedosto ja **Valitse aiemmin luotu tiedosto Swagger metatiedot** -vaihtoehto.

5. Valitse **Lisää REST API asiakas** -valintaikkunassa **OK**.

    Visual Studio luo kansion nimeltä jälkeen API-sovellus ja luo asiakkaan luokat.

    ![Luotu-asiakasohjelman koodi tiedostot](./media/app-service-api-dotnet-get-started/codegenfiles.png)

6. Avaa ToDoListAPI projektin *Controllers\ToDoListController.cs* rivillä 40, joka kutsuu Ohjelmointirajapinnan avulla luotu asiakkaan koodi.

    Seuraavat koodikatkelman näyttää kuinka koodin muodostaa asiakkaan kohteen ja soittaa Get-menetelmää.

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }

        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    Konstruktorin parametrin saa päätepisteen URL-Osoitteen `toDoListDataAPIURL` asetus. Seuraavan koodin korostetut kyseinen arvo on määritetty paikallisen IIS Express URL-osoitteeseen API projektin niin, että voit suorittaa sovelluksen paikallisesti. Jos jätät konstruktorin parametri, oletusarvoinen päätepiste on URL-osoite, jotka olet luonut koodin.

7. Asiakas-luokan muodostetaan Ohjelmointirajapinta app nimesi; perusteella eri nimellä Muuta *Controllers\ToDoListController.cs* koodia, siten, että tyyppinimi vastaa mitä luotiin projektin. Esimerkiksi jos API-sovelluksen ToDoListDataAPI071316 nimennyt koodia muuttaa:

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

tähän:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-to-host-the-middle-tier"></a>Isännöimiseen Keskitaso API-sovelluksen luominen

Aiemmin voit [luotu tietojen tason API-sovellus ja siihen koodin otettu käyttöön](#createapiapp).  Nyt voit seurata Keskitaso API sovelluksen samalla tavalla.

1. Napsauta **Ratkaisunhallinnassa**hiiren kakkospainikkeella Keskitaso ToDoListAPI project (ei tietoja tason ToDoListDataAPI) ja valitse sitten **Julkaise**.

    ![Valitse Julkaise Visual Studiossa](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

2.  Valitse **Microsoft Azure-sovelluksen palvelun**ohjatun **Sivuston julkaisemisen** **profiili** -välilehti.

3. Valitse **Sovelluksen Service** -valintaikkunassa **Uusi**.

4. **Luo sovelluksen palvelu** -valintaikkunan **Hosting** -välilehden Hyväksy oletusasetukset **API sovelluksen nimi** tai nimi, joka on yksilöllinen *azurewebsites.net* toimialueeseen.

5. Valitse ensin Azure **tilaus** olet käyttänyt.

6. **Resurssiryhmä** avattavasta Valitse aiemmin luomasi saman resurssiryhmä.

7. - **Sovelluksen palvelusopimus** avattavassa luettelossa valitse aiemmin luomasi vastaavan palvelupaketin. Kyseinen arvo oletusarvo sitä.

8. Valitse **Luo**.

    Visual Studio Luo API-sovellus, luo se Julkaise profiilin ja näyttää **yhteyden** **Sivuston julkaiseminen** ohjatun toiminnon vaihe.

9.  Ohjatun **Sivuston julkaisemisen** **yhteyden** vaiheessa Valitse **Julkaise**.

    Visual Studio ottaa käyttöön ToDoListAPI projektin uusi API-sovellus ja avaa API-sovelluksen URL-osoite selaimen. "Onnistuneesti luotu-sivu tulee näkyviin.

## <a name="configure-the-middle-tier-to-call-the-data-tier"></a>Soita tietojen tason Keskitaso määrittäminen

Jos Keskitaso API-sovellus on nimeltään nyt, se yritä kutsua käyttämällä localhost URL-Osoitetta, joka on yhä seuraavan koodin korostetut tietojen taso. Tässä osassa kirjoitat tietoja taso API sovelluksen URL-Osoitteen ympäristön-asetus Keskitaso API-sovelluksessa. Kun koodin Keskitaso API-sovelluksessa hakee tietoja tason URL-osoite-asetusta, ympäristön-asetus ohittaa seuraavan koodin korostetut ominaisuudet.

1. Siirry [Azure portal](https://portal.azure.com/)ja siirry sitten API-sovellukseen, jonka loit isännöimiseen TodoListAPI (Keskitaso) projektin **API sovellus** -sivu.

2. Valitse **Sovellusasetukset**API-sovelluksen **asetukset** -sivu.

3. API-sovelluksen **Asetukset** -sivu, valitse **sovelluksen asetukset** -kohdasta kohtaan ja lisää seuraava avain ja arvo. Arvo on julkaistu Tässä opetusohjelmassa ensimmäisen API-sovelluksen URL-osoite.

  	| **Avain** | toDoListDataAPIURL |
  	|---|---|
  	| **Arvo** | https://{Your tietojen taso API sovelluksen nimi} .azurewebsites .net |
  	| **Esimerkki** | https://todolistdataapi.azurewebsites.NET |

4. Valitse **Tallenna**.

    ![Valitse sovelluksen asetusten tallentaminen](./media/app-service-api-dotnet-get-started/asinportal.png)

    Koodin suorittamisen Azure-arvoksi nyt ohittaa, joka sijaitsee seuraavan koodin korostetut localhost URL-osoite.

## <a name="test"></a>Testi

1. Siirry selainikkunassa, uusi Keskitaso API sovellus, jonka loit vain ToDoListAPI URL-osoite. Voit siirtyä sinne API-sovelluksen tärkeimmät sivu-portaalin URL-Osoitetta napsauttamalla.

2. Lisää "swagger" URL-osoite selaimen osoiteriville ja paina sitten Enter-näppäintä. (URL-osoite on `http://{apiappname}.azurewebsites.net/swagger`.)

    Selain näyttää saman Swagger Käyttöliittymän, näyttöön tuli aiemmin, ToDoListDataAPI, mutta nyt `owner` ei ole pakollinen kenttä Get-toiminto, koska Keskitaso API-sovelluksen lähettää arvon tietojen tason API-sovelluksen puolestasi. (Kun teet todennus-opetusohjelmat, keskitaso Lähetä todellinen käyttäjätunnukset `owner` parametrin; varten nyt se on vaikea-koodaus tähti.)

3. Yritä Get-menetelmällä ja muita tapoja Vahvista, että Keskitaso API sovelluksen onnistuneesti soittaa tietojen tason API-sovellus.

    ![Swagger Käyttöliittymän Get-menetelmää](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Vianmääritys

Niin kuin tässä opetusohjelmassa kerrotaan ongelma voi ilmetä ovat vianmäärityksen ideoita:

* Varmista, että käytät [Azure SDK.NET](http://go.microsoft.com/fwlink/?linkid=518003)uusimman version.

* Kaksi projektien nimet ovat samanlaiset (ToDoListAPI, ToDoListDataAPI). Jos kohteita ei tarkista ohjeiden mukaisesti ohjeita, kun käsittelet projektin, varmista, että olet avannut oikea projekti.

* Jos olet yrityksen verkossa ja yrität ottaa käyttöön sovelluksen Azure-palvelu palomuurin läpi, varmista, että porttien 443 ja 8172 on avoinna Web-käyttöön. Jos et voi avata portit, voit käyttää muiden käyttöönoton kautta.  Katso [Azure App palveluun sovelluksen käyttöönotto](../app-service-web/web-sites-deploy.md).

* "Reitin nimet on oltava yksilöllinen" virheitä--voitu saat nämä Jos käyttöönottaminen vahingossa väärä projektin API-sovelluksen ja käyttöönotto oikea siihen myöhemmin. Voit korjata ongelman poistamalla laukaisun oikea projekti API-sovellukseen ja ohjatun **Sivuston julkaisemisen** **asetukset** -välilehden Valitse **Poista kohde muita tiedostoja**.

Kun sinulla on käynnissä Azure App palvelun ASP.NET-Ohjelmointirajapinnan sovelluksen, voit halutessasi lisätietoja Visual Studio-ominaisuuksia, jotka helpottavat vianmääritys. Lisätietoja kirjaamisen remote virheenkorjaus ja lisää-kohdassa [vianmääritys Azure App palvelun Visual Studiossa](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Nähnyt käyttöönotto aiemmin verkko-Ohjelmointirajapinnan projektien API sovelluksiin, Luo asiakkaan koodi API-sovellusten ja tarjoaman .NET asiakkaiden API-sovelluksia. Seuraavaan opetusohjelmaan sarjassa näkyy käyttämisestä [CORS tarjoaman JavaScript-asiakkaiden API-sovelluksia](app-service-api-cors-consume-javascript.md).

Lisätietoja asiakkaan koodin luominen näy GitHub.com [Azure/AutoRest](https://github.com/azure/autorest) säilö. Ohjeita ongelmat, jotka on luotu-asiakas Avaa [ongelman AutoRest säilössä](https://github.com/azure/autorest/issues).

Jos haluat luoda täysin uusia API app projekteja, käytä **Azure API sovelluksen** -malli.

![Visual Studio API-sovelluksen malli](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

**Azure API App** project-malli on sama kuin valitseminen **Tyhjä** ASP.NET 4.5.2 mallia, valitsemalla valintaruutu, jos haluat lisätä verkko-Ohjelmointirajapinnan ja asentamalla Swashbuckle NuGet. Lisäksi mallin Lisää tarkoituksena on estää kaksoiskappaleiden Swagger toiminnon tunnukset luomisen Swashbuckle määritysten koodia. Kun olet luonut API App projektin, voit ottaa käyttöön sen API-sovelluksen samalla tavalla kuin tässä opetusohjelmassa tuli.
