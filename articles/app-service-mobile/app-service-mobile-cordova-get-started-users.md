<properties
    pageTitle="Lisää todennus Apache Cordova Mobile-sovellusten kanssa | Azure sovelluksen-palvelu"
    description="Opettele todentaa kautta tunnistetietojen toimittajat, mukaan lukien Google, Facebookiin, Twitteriin ja Microsoft erilaisia Apache Cordova sovelluksesi käyttäjät Azure App palvelun Mobile-sovellusten avulla."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-apache-cordova-app"></a>Käyttöoikeuksien lisääminen Apache Cordova-sovellukseen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
    
## <a name="summary"></a>Yhteenveto

Tässä opetusohjelmassa käyttöoikeuksien lisääminen todolist pikaopas projektia Apache Cordova tuetut identity-palvelun avulla. Tässä opetusohjelmassa perustuu [Mobile-sovellusten käytön aloittaminen] -opetusohjelma, jotka sinun täytyy tehdä.

##<a name="register"></a>Rekisteröi todennustavaksi sovelluksen ja sovelluksen kertakirjauspalvelun asetusten määrittäminen

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Katso tämä video, jossa vastaavat vaiheet](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

##<a name="permissions"></a>Todennettujen käyttäjien käyttöoikeuksien rajaamiseen

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nyt voit varmistaa, että Taustajärjestelmä anonyymi käyttö on poistettu käytöstä. Visual Studiossa Avaa projekti, jonka loit, kun olet suorittanut [Mobile-sovellusten käytön aloittaminen]-opetusohjelma ja valitse Suorita sovellus **Google Android emulaattorin** ja varmista odottamattomia yhteysvirhe näkyy, kun sovellus käynnistyy.

Seuraavaksi päivität sovelluksen todentaa käyttäjät ennen pyytää resurssien Mobile-sovelluksen taustasta.

##<a name="add-authentication"></a>Käyttöoikeuksien lisääminen sovellukseen

1. Projektin avaaminen **Visual Studio**ja avaa sitten `www/index.html` tiedoston muokkaamista varten.

2. Etsi `Content-Security-Policy` metatunniste pää-osassa.  Tarvitset lisää OAuth-isäntä sallittu lähteiden luetteloon.

  	| Toimittaja               | SDK tarjoajan nimi | OAuth Host (isäntä)                  |
  	| :--------------------- | :---------------- | :-------------------------- |
  	| Azure Active Directory | aad               | https://Login.Windows.NET   |
  	| Facebook               | Facebook          | https://www.Facebook.com    |
  	| Google                 | Google            | https://accounts.google.com |
  	| Microsoft              | microsoftaccount käytettävissä  | https://Login.live.com      |
  	| Twitter-                | Twitter-           | https://API.Twitter.com     |

    Esimerkin sisältö--suojauskäytäntö (käytössä Azure Active Directory) on seuraavanlainen:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.windows.net https://yourapp.azurewebsites.net; style-src 'self'">

    Sinun on korvattava `https://login.windows.net` OAuth isännällä yllä olevasta taulukosta.  Katso lisätietoja tämän metatunniste [sisältö--suojauskäytäntö ohjeissa] .

    Huomaa, että jotkin käyttöoikeustarkistuspalvelun eivät edellytä sisältö--suojauskäytäntö muuttuu, kun käytetään tarvittavat mobiililaitteissa.  Esimerkiksi sisältö--suojauskäytäntö muutoksia ei tarvita, kun käytät Google-todennus Android-laitteessa.

3. Avaa `www/js/index.js` tiedoston muokattavaksi, Etsi `onDeviceReady()` -menetelmä ja koodin Lisää asiakkaan luominen-kohdassa seuraavasti:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Huomautus Tämä koodi korvaa aiemmin luotu koodi, joka luo taulukon viittauksissa ja päivittää Käyttöliittymän.

    Login()-menetelmä käynnistää todennus palvelussa. Login() tapa on asynkroninen-funktiota, joka palauttaa JavaScript-Promise.  Loput alustus on sijoitettu promise vastauksen niin, että se ei suoriteta ennen kuin login() menetelmä on valmis.

4. Juuri lisäämäsi koodissa korvaa `SDK_Provider_Name` kirjautuminen tarjoajan nimi. Esimerkiksi Azure Active Directory-käyttämällä `client.login('aad')`.

4. Suorita projektin.  Kun projekti on lopettanut valmistellaan, sovelluksen Näytä valitun käyttöoikeustarkistuspalvelun OAuth-kirjautumissivulle.

##<a name="next-steps"></a>Seuraavat vaiheet

* Lue lisää [Tietoja käyttöoikeudesta] Azure App palvelussa.
* Jatka opetusohjelman lisäämällä [Push-ilmoitukset] Apache Cordova-sovellukseen.

Opettele käyttämään SDK: T.

* [Apache Cordova SDK-paketissa]
* [ASP.NET-palvelimen SDK-paketissa]
* [Node.js Server SDK-paketissa]

<!-- URLs. -->
[Mobile-sovellusten käytön aloittaminen]: app-service-mobile-cordova-get-started.md
[Sisältö--suojauskäytäntö dokumentaatio]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Push-ilmoitukset]: app-service-mobile-cordova-get-started-push.md
[Tietoja käyttöoikeuden]: app-service-mobile-auth.md
[Apache Cordova SDK-paketissa]: app-service-mobile-cordova-how-to-use-client-library.md 
[ASP.NET-palvelimen SDK-paketissa]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK-paketissa]: app-service-mobile-node-backend-how-to-use-server-sdk.md
