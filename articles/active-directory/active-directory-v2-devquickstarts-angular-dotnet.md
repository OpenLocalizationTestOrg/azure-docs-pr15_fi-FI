<properties
    pageTitle="Azure AD v2.0 AngularJS aloittaminen | Microsoft Azure"
    description="Miten voit luoda kulma JS yhdelle sivulle-sovellusta, joka kirjautuu sekä henkilökohtainen Microsoft-tilin käyttäjille ja työpaikan tai oppilaitoksen tilit."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="add-sign-in-to-an-angularjs-single-page-app---net"></a>Lisää AngularJS yksittäiseltä sivulta sovelluksen - .NET-kirjautuminen

Tässä artikkelissa lisätään kirjautumalla muistiin Microsoft-tilit, AngularJS-sovellusta Azure Active Directory-v2.0 päätepiste.  V2.0 päätepisteen avulla voit suorittaa yhteen integration-sovelluksen ja todentaa sekä henkilökohtainen ja työpaikan tai oppilaitoksen tilin käyttäjille.

Tässä esimerkissä on yksinkertainen tehtäväluettelo yhdelle sivulle sovellus, joka tallentaa taustatietokannan REST API-kirjoitettu .NET 4.5 MVC framework ja suojattu OAuth haltijan-tunnukset Azure AD-tehtävät.  AngularJS sovelluksen käyttämällä Microsoftin Avaa lähde JavaScript todennus kirjaston [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) täyttökahva koko kirjautumisprosessin ja hankkia tunnusten soitettavien REST-Ohjelmointirajapinnalla.  Samoissa voi suojata muiden REST API, kuten [Microsoft Graph](https://graph.microsoft.com)tarkistamiseen.

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

## <a name="download"></a>Lataa

Aluksi sinun on Lataa ja asenna Visual Studio.  Sitten voit Kloonaa tai [Lataa](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) rakenne-sovelluksen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

Rakenne-sovelluksen sisältää kaikki asiakirjan osien uudelleen käyttäminen yksinkertainen AngularJS sovelluksen koodi, mutta puuttuu kaikki käyttäjätiedot liittyvät osat.  Jos et halua seurata, voit sen sijaan Kloonaa [Lataa](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) Esimerkki valmiista.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## <a name="register-an-app"></a>Sovelluksen rekisteröiminen

Ensin [Sovelluksen rekisteröinnin Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)-sovelluksen luominen tai noudattamalla seuraavia [toimimalla seuraavasti](active-directory-v2-app-registration.md).  Varmista, että:

- Lisää **WWW** -ympäristö, kun sovellus.
- Kirjoita oikeat **Uudelleenohjata URI**. Tässä esimerkissä oletusarvo on `https://localhost:44326/`.
- Jätä käyttöön **Salli implisiittinen Flow** -valintaruutu. 

Kopioi alaspäin **Tunnus** , joka on määritetty sovelluksen, tarvitset niitä myöhemmin. 

## <a name="install-adaljs"></a>Asenna adal.js
Alkavan, siirry projektin voit ladata ja asentaa adal.js.  Jos sinulla on asennettu [bower](http://bower.io/) , voit suorittaa vain tämä komento.  Minkä tahansa riippuvuuden versio lähdeluetteloiden, valitse uudempi versio.
```
bower install adal-angular#experimental
```

Vaihtoehtoisesti voit ladata manuaalisesti [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) ja [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Lisää molempien tiedostojen `app/lib/adal-angular-experimental/dist` hakemistoon `TodoSPA` projektin.

Avaa projekti Visual Studiossa, ja ladata adal.js tärkeimmät sivun leipäteksti lopussa:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>REST API määrittäminen

Vaikka emme olet määrittämässä asioita, aloitetaan Taustajärjestelmä REST API-työskentelyä.  Avaa projekti ylimmällä `web.config` ja korvaa `audience` arvo.  REST API Vahvista se saa AJAX-pyyntöjen kulma sen sovelluksesta tunnusten käyttämällä tätä arvoa.

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>
    
...
```

Tämä on tutustutaan käyttävät keskustella REST-Ohjelmointirajapinnalla toiminta aina.  Vapaasti Tee koodissa, mutta jos haluat lisätietoja suojaaminen verkko-ohjelmointirajapinnan kanssa Azure AD-Lue [tämän artikkelin](active-directory-v2-devquickstarts-dotnet-api.md). 

## <a name="sign-users-in"></a>Allekirjoita-käyttäjät
Aika, kirjoittaa joitakin tunnuskoodi.  Olet ehkä jo jo huomannut, adal.js sisältää AngularJS-palvelu, joka toistetaan hyvin varustettuja kulma reititys.  Aloita lisäämällä adal moduulin sovelluksen:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Voit nyt alusta `adalProvider` sovelluksen tunnuksella:

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

Hyvien, adal.js on nyt kaikki haluamasi tiedot, se on suojattu sovellus ja kirjaudu sisään käyttäjät.  Voit pakottaa tietyllä reitillä sovelluksessa kirjautunut sisään, kaikki kestää on yksi rivi koodi:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Nyt, kun käyttäjä napsauttaa `TodoList` linkkiä adal.js automaattisesti ohjaa Azure AD kirjautumista varten tarvittaessa.  Voit lähettää sisäänkirjautumisongelmien ja kirjaudu ulos pyynnöt myös eksplisiittisesti kutsumalla käyttäjän ohjaimet adal.js:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## <a name="display-user-info"></a>Näyttää Käyttäjätiedot
Nyt, kun käyttäjä on kirjautunut, sinun on luultavasti kirjautunut sisään käyttäjän todennusta tietojen sovelluksen käyttämistä varten.  Adal.js paljastaa nämä tiedot: ssä `userInfo` objekti.  Jos haluat käyttää tätä objektia näkymässä, lisäämällä adal.js vastaavan ohjauskoneen pääkansion laajuus:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Sitten voit käsitellä suoraan `userInfo` objektin näkymässä: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

Voit käyttää myös `userInfo` objektin määrittääksesi, jos käyttäjä on kirjautunut vai ei.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>Soita REST-Ohjelmointirajapinnalla
Lopuksi on aika hakea joitakin tunnuksia ja soita REST-Ohjelmointirajapinnalla luoda, lukea, päivittää ja poistaa tehtäviä.  Hyvin Arvaa mitä?  Sinun ei tarvitse tehdä *myös*.  Adal.js automaattisesti huolehtia käytön, välimuistiin tallentaminen ja päivittäminen tunnukset.  Se myös huolehtia näiden tunnusten liittäminen lähtevien AJAX-pyyntöjen, joita voi lähettää REST-Ohjelmointirajapinnalla.  

Miten täsmälleen tämä toimii? On kaikki [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), joka sallii Lähtevät ja saapuvat viestit http muuntamiseen adal.js taikaa alkuun.  Lisäksi adal.js oletetaan, että pyynnöt lähettää samaa toimialuetta tavalla ikkunan kannattaa käyttää tunnusten on sama kuin AngularJS app Sovellustunnus tarkoitettu.  Tämän vuoksi on käytetty saman Sovellustunnus kulma sovellus ja NodeJS REST-Ohjelmointirajapinnalla.  Voit ohittaa tämän ja kerro adal.js tunnusten hakeminen muiden REST API tarvittaessa - mutta Oheisessa yksinkertaisessa skenaariossa, oletusarvoiset suorittaa.

Näin koodikatkelman, joka näyttää, miten helppoa on pyynnöt ja haltijan tunnusten lähettäminen Azure AD:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Onnittelen!  Azure AD integroitu yksittäiseltä sivulta sovelluksen on nyt valmis.  Siirry eteenpäin, ruusukkeeseen kestää.  Se voi todentaa käyttäjät suojatusti Soita sen Taustajärjestelmä REST API OpenID yhdistämistä käyttämällä ja Hae käyttäjän perustietoja.  Ulos-ruutuun se tukee kaikki käyttäjät, joilla omalla Microsoft-Account tai Azure AD-työpaikan tai oppilaitoksen tiliä.  Suorita sovellus ja siirry selaimen `https://localhost:44326/`.  Kirjaudu sisään käyttämällä henkilökohtainen Microsoft-tili tai työpaikan tai oppilaitoksen tiliä.  Tehtävien lisääminen käyttäjän Tehtävät-luettelo ja kirjaudu ulos.  Yritä kirjautua sisään muiden tilin tyyppi. Jos tarvitset Azure AD-vuokraajan työpaikan/oppilaitoksen käyttäjien luominen [avulla opit jokin seuraavassa](active-directory-howto-tenant.md) (se on ilmainen).

Jatka v2.0 päätepisteen liittyviä palaa Microsoftin [v2.0 Sovelluskehittäjän opas](active-directory-appmodel-v2-overview.md).  Muita tietolähteitä Kuittaa ulos:

- [Azure-malleja GitHub >>](https://github.com/Azure-Samples)
- [Azure AD-Pinon ylivuoto >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- Azure AD-dokumentaatio [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Päivitysten hakeminen tuotteitamme

On suositeltavaa, kun suojauksen tapaukset esiintyvät käymällä [Tämän sivun](https://technet.microsoft.com/security/dd252948) ja tilaamalla neuvoa suojausvaroitusten ilmoitusten saaminen.
