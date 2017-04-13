<properties
    pageTitle="CORS tukea sovelluksen palvelun | Microsoft Azure"
    description="Opettele käyttämään CORS tuki Azure Azure sovelluksen-palvelussa."
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
    ms.topic="get-started-article"
    ms.date="08/27/2016"
    ms.author="rachelap"/>

# <a name="consume-an-api-app-from-javascript-using-cors"></a>Sovelluksen Ohjelmointirajapinnan käyttäminen CORS JavaScript tarjoaman

Sovelluksen palvelussa tukee [Rajat Origin resurssien jakaminen (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), joka mahdollistaa JavaScript-asiakkaat voivat soittaa toimialueiden ohjelmointirajapinnan, joita isännöidään API-sovelluksissa. Sovelluksen Servicen avulla voit määrittää oman API CORS pääsy kirjoittamatta koodia oman API.

Tässä artikkelissa on kaksi osaa:

* [CORS määrittäminen](#corsconfig) -osassa kerrotaan yleisesti määrittäminen CORS API-sovelluksen, web App-sovelluksen tai mobiilisovelluksessa. Toiminto on käytettävissä tasaisesti kaikki kehyksiä, jotka tukevat App palvelun, kuten .NET, Node.js ja Java. 

* Alkaen [jatkuvaa .NET käytön aloittaminen-opetusohjelmat](#tutorialstart) -osassa, on artikkelissa on opas, joka osoittaa CORS tukevat rakentamalla, mitä [ensimmäisen API-sovellusten käytön aloittaminen opetusohjelma](app-service-api-dotnet-get-started.md)kirjoitit. 

## <a id="corsconfig"></a>Azure-sovelluksen palvelun CORS määrittäminen

Voit määrittää CORS Azure-portaalissa tai [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md) -työkalujen avulla.

#### <a name="configure-cors-in-the-azure-portal"></a>Määritä CORS Azure-portaalissa

8. Siirry [Azure portal](https://portal.azure.com/)selaimessa.

2. Valitse **Sovelluksen palvelut**ja valitse sitten API sovelluksen nimeä.

    ![Valitse API sovellus-portaalissa](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. Valitse **asetukset** -sivu, joka avaa oikealla puolella **API sovellus** -sivu Etsi **Ohjelmointirajapinta** -osio ja valitse sitten **CORS**.

    ![Valitse CORS asetukset-sivu](./media/app-service-api-cors-consume-javascript/clicksettings.png)

11. Tekstin ruutuun kirjoita URL-Osoitteen tai URL-osoitteet, jotka haluat sallia JavaScript puhelut peräisin.


    Jos olet asentanut JavaScript-sovelluksen nimeltä todolistangular web App-sovellukseen, kirjoita "https://todolistangular.azurewebsites.net". Vaihtoehtoisesti voit kirjoittaa tähtimerkkiä (*) voit määrittää, että kaikki origin toimialueet hyväksytään.


13. Valitse **Tallenna**.

    ![Valitse Tallenna](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Kun valitset **Tallenna**, API-sovelluksen hyväksyy JavaScript kutsut määritetyt URL-osoitteet.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Määritä CORS Azure Resurssienhallinta-työkalujen avulla

Voit myös määrittää CORS API-sovelluksen komentoriviltä työkaluilla, kuten [PowerShellin Azure](../powershell-install-configure.md) ja [Azure CLI](../xplat-cli-install.md) [Azure Resurssienhallinta mallien](../resource-group-authoring-templates.md) avulla. 

Esimerkki Azure Resurssienhallinta-malli, joka määrittää CORS-ominaisuuden Avaa [säilössä Tässä opetusohjelmassa otoksen sovelluksen azuredeploy.json tiedoston](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Etsi kohta malli, joka näyttää seuraavalta:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>Jatkuvaa .NET opetusohjelman käytön aloittaminen

Jos seuraat Node.js tai Java aloittaminen sarjan API-sovellusten, olet suorittanut hakeminen aloittaminen sarja. Siirry [seuraavat vaiheet](#next-steps) -osassa ehdotuksia edelleen tietoa API sovellukset.

Jäljempänä tässä artikkelissa on jatkamisen .NET aloittaminen sarjan ja oletetaan, että [ensimmäinen opetusohjelman](app-service-api-dotnet-get-started.md)onnistuneesta päättymisestä.

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>Ota käyttöön uuden verkkosovellukseen ToDoListAngular-projekti

[Ensimmäinen opetusohjelman](app-service-api-dotnet-get-started.md)luomasi Keskitaso API-sovelluksen ja tietojen tason API-sovellus. Tässä opetusohjelmassa voit luoda yhden sovelluksen (SPA) verkkosovellukseen, että puhelut Keskitaso API-sovellus. SPA toimisi, on otettava CORS Keskitaso API-sovellukseen. 

[Sovelluksen ToDoList malli](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list)ToDoListAngular projekti on yksinkertainen AngularJS-asiakas, joka kutsuu Keskitaso ToDoListAPI verkko-Ohjelmointirajapinnan projektin. JavaScript-koodia *app/scripts/todoListSvc.js* tiedoston puhelujen Ohjelmointirajapinnan AngularJS HTTP-palvelun avulla. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 
        
            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>Uusi web-sovelluksen ToDoListAngular projektin luominen

Uuden sovelluksen Service-web-sovelluksen luominen ja ota se käyttöön projektin menettely muistuttaa tuli [luominen](app-service-api-dotnet-get-started.md#createapiapp)ja käyttöönotto API-sovelluksen ensimmäisen opetusohjelman sarjassa. Ainoa ero on sovelluksen tyyppi on **Web App** eikä **API-sovelluksessa**.  Katso valintaikkunat näyttökuvia 

1. **Ratkaisunhallinnassa**ToDoListAngular projektin hiiren kakkospainikkeella ja valitse sitten **Julkaise**.

3.  Valitse **Microsoft Azure-sovelluksen palvelun**ohjatun **Sivuston julkaisemisen** **profiili** -välilehti.

5. Valitse **Sovelluksen Service** -valintaikkunassa **Uusi**.

3. Kirjoita **Luo sovelluksen palvelu** -valintaikkunan **Hosting** -välilehdessä **Web-sovelluksen nimi** , joka on yksilöllinen *azurewebsites.net* toimialueeseen. 

5. Valitse ensin Azure **tilauksen** haluat käsitellä.

6. Valitse aiemmin luomasi saman resurssiryhmä **Resurssiryhmä** -pudotusvalikosta.

4. Valitse aiemmin luomasi vastaavan palvelupaketin **App palvelusopimus** -pudotusvalikosta. 

7. Valitse **Luo**.

    Visual Studio Luo web app, luo se Julkaise profiilin ja näyttää **yhteyden** **Sivuston julkaiseminen** ohjatun toiminnon vaihe.

    **Julkaise** Älä napsauta vielä. Seuraavassa osassa Soita Keskitaso API-sovellus, jossa on käytössä sovelluksen-palvelussa uusi web App-sovelluksen määrittäminen. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>Keskitason URL-Osoitteen määrittämisestä web app-asetukset

1. Siirry [Azure-portaaliin](https://portal.azure.com/)ja siirry web-sovelluksen, jonka loit isännöimiseen TodoListAngular (edusta) project **Web App** -sivu.

2. Valitse **Asetukset > sovellusasetukset**.

3. Valitse **sovelluksen asetukset** -kohdassa Lisää seuraavan avaimen ja arvon:

  	|Avain|Arvo|Esimerkki
  	|---|---|---|
  	|toDoListAPIURL|https://{Your Keskitaso API sovelluksen nimi} .azurewebsites .net|https://todolistapi0121.azurewebsites.NET|

4. Valitse **Tallenna**.

    Kun koodi suoritetaan Azure-arvoksi ohittaa, joka sijaitsee *seuraavan koodin korostetut* localhost URL-osoite. 

    Koodi, joka hakee asetusarvon on käytössä *index.cshtml*:

        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>

    *TodoListSvc.js* koodi käyttää asetus:

        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>Ota käyttöön uuden web-sovelluksen ToDoListAngular web-projekti

*  Valitse **Julkaise**Visual Studion **yhteyden** **Sivuston julkaiseminen** ohjatun toiminnon vaiheessa.

    Visual Studio ottaa käyttöön ToDoListAngular projektin uusi web-sovellus ja avaa web Appin URL-osoite selaimen. 

### <a name="test-the-application-without-cors-enabled"></a>Testaa sovelluksen ilman CORS käytössä 

2. Avaa selaimen Kehitystyökalut konsoli-ikkuna.

3. Napsauta selaimen ikkunan, jossa näkyy AngularJS-Käyttöliittymä, **Tehtävät-luettelo** -linkkiä.

    JavaScript-koodia yrittää soittaa Keskitaso API-sovelluksen, mutta kutsu epäonnistuu, koska edustan on käynnissä toimialueesta kuin uudelleen. Kehittäjän työkalut konsolin selainikkunassa näyttää rajat origin virhesanoma.

    ![Usean origin virhesanoma](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>Määritä CORS Keskitaso API-sovellukseen

Tässä osassa määrittäminen Keskitaso ToDoListAPI API app Azure CORS-asetusta. Tämä asetus mahdollistaa Keskitaso API app vastaanottaa JavaScript-puhelut, jonka loit ToDoListAngular project web Appista.

8. Selaimessa Siirry [Azure portal](https://portal.azure.com/).

2. Valitse **Sovelluksen palvelut**ja valitse sitten ToDoListAPI (Keskitaso) API-sovellus.

    ![Valitse API sovellus-portaalissa](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. Valitse **asetukset** -sivu, joka avaa oikealla puolella **API sovellus** -sivu Etsi **Ohjelmointirajapinta** -osio ja valitse sitten **CORS**.

    ![Valitse CORS-portaalissa](./media/app-service-api-cors-consume-javascript/clicksettings.png)

12. Kirjoita tekstiruutuun URL-osoite ToDoListAngular (edusta)-verkkosovelluksessa. Esimerkiksi jos olet asentanut ToDoListAngular project web App-sovellukseen nimeltä todolistangular0121, Salli kutsut URL-Osoitteen `https://todolistangular0121.azurewebsites.net`.

    Vaihtoehtoisesti voit kirjoittaa tähtimerkkiä (*) voit määrittää, että kaikki origin toimialueet hyväksytään.

13. Valitse **Tallenna**.

    ![Valitse Tallenna](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Kun valitset **Tallenna**, API-sovelluksen hyväksyy JavaScript kutsut määritetty URL-osoite. ToDoListAPI0223 API-sovelluksen hyväksyy tämän näyttökuva JavaScript asiakkaan kutsut ToDoListAngular web Appista.

### <a name="test-the-application-with-cors-enabled"></a>Testaa sovelluksen CORS käytössä

* Avaa selain web Appin HTTPS URL-osoitteeseen. 

    Tällä hetkellä sovelluksen avulla voit tarkastella, lisääminen, muokkaaminen ja poistaminen Tehtäväkohteita. 

    ![Tehtäväluettelo otoksen sovellus-sivulla](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>Sovelluksen palvelun CORS ja verkko-Ohjelmointirajapinnan CORS

Verkko-Ohjelmointirajapinnan Projectissa voit asentaa [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-paketti, voit määrittää koodin mitkä toimialueet oman API hyväksyy JavaScript kutsut.
 
Verkko-Ohjelmointirajapinnan CORS tuki on joustavampaa kuin sovelluksen palvelun CORS tuki. Esimerkiksi koodissa voit määrittää eri hyväksytyssä alkuperistä toisen toiminnon menetelmistä samalla, kun sovellus palvelun CORS määritettävä hyväksytyssä alkuperistä kaikkien API-sovelluksen menetelmiä joukkona.

> [AZURE.NOTE] Älä yritä käyttää verkko-Ohjelmointirajapinnan CORS ja sovelluksen palvelun CORS API-sovelluksessa. Sovelluksen palvelun CORS täyttävä ja verkko-Ohjelmointirajapinnan CORS ei ole vaikutusta. Esimerkiksi jos käyttöön yhden sovelluksen palvelun origin toimialueen ja ota käyttöön kaikki origin toimialueet verkko-Ohjelmointirajapinnan koodin, Azure API-sovelluksen vain hyväksyy kutsut määrittämäsi toimialue Azure-tietokannassa.

### <a name="how-to-enable-cors-in-web-api-code"></a>Ottamisesta käyttöön CORS verkko-Ohjelmointirajapinnan koodi

Seuraavat vaiheet on yhteenveto prosessi, jossa verkko-Ohjelmointirajapinnan CORS tukipalveluiden ottaminen käyttöön. Lisätietoja on artikkelissa [Ottaminen käyttöön rajat Origin pyynnöt ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. Verkko-Ohjelmointirajapinnan Projectissa Asenna [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-paketti.

1. Sisällytä `config.EnableCors()` koodin **WebApiConfig** luokan, kuten seuraavassa esimerkissä **Rekisteröi** -menetelmää. 

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. Lisää verkko-Ohjelmointirajapinnan, ohjaimen `using` tietosuojatiedot `System.Web.Http.Cors` nimitilan, ja lisää `EnableCors` määrite ohjauskoneen luokan tai yksittäinen toimenpide tavoista. Seuraavassa esimerkissä CORS tuki koskee koko ohjauskoneen.

        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController
 
## <a name="using-azure-api-management-with-api-apps"></a>Azure API hallinnasta API-sovelluksissa

Jos käytät Azure API hallinnan API-sovelluksen, määrittää CORS API hallinta sijaan API-sovelluksessa. Lisätietoja on seuraavissa resursseissa:

* [Yleistä Azure API-hallinnasta (video: CORS alkaa 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [API cross toimialueen käytäntöjen hallinta](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)
 
## <a name="troubleshooting"></a>Vianmääritys

Siltä varalta, että ongelma tulla vastaan loppuun tässä opetusohjelmassa, seuraavassa on joitakin vianmäärityksen ideoita.

* Varmista, että käytät [Azure SDK for Visual Studio 2015.NET](http://go.microsoft.com/fwlink/?linkid=518003)uusimman version.

* Varmista, että olet kirjoittanut `https` CORS-asetusta ja varmista, että käytät `https` suorittaa edusta-WWW-sovellus.

* Varmista, että olet kirjoittanut CORS asetus Keskitaso API-sovelluksessa, ei edusta-WWW-sovelluksessa.

* Jos olet määrittäminen CORS sovelluksen koodin ja Azure App palvelu, Huomaa App palvelun CORS-asetus ohittaa riippumatta siitä, mitä olet tekemässä sovelluksen koodin. 

Lisätietoja Visual Studio ominaisuuksia, jotka helpottavat vianmääritysohjeita on kohdassa [vianmääritys Azure App palvelun Visual Studiossa](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Seuraavat vaiheet 

Tässä artikkelissa näyttöön tuli App palvelun CORS tuen ottaminen niin, että asiakkaan JavaScript-koodia myös soittaa Ohjelmointirajapinnan toimialueesta. Lisätietoja API-sovellukset, lue [todennus-sovelluksen-palvelun esittely](../app-service/app-service-authentication-overview.md)ja siirry [API-sovellusten käyttöoikeuksien](app-service-api-dotnet-user-principal-auth.md) opetusohjelman.
