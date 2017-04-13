<properties
    pageTitle="Todennus Xamarin iOS-Mobile-sovellusten käytön aloittaminen"
    description="Opettele todentaa käyttäjät Xamarin iOS-sovelluksen kautta tunnistetietojen toimittajat, mukaan lukien AAD, Google, Facebookiin, Twitteriin ja Microsoft erilaisia Mobile-sovellusten avulla."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinios-app"></a>Käyttöoikeuksien lisääminen Xamarin.iOS-sovellukseen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Tässä ohjeaiheessa esitellään, miten sovelluksen palvelun Mobile-sovelluksen hakeminen asiakassovellus käyttäjien todentamiseen. Tässä opetusohjelmassa käyttöoikeuksien lisääminen Xamarin.iOS pikaopas projektin identity-palvelun, joka tue sovelluksen-palvelun avulla. Kun parhaillaan todennettu onnistuneesti ja myöntänyt Mobile-sovelluksen, käyttäjän ID-arvo näkyy ja osaat käyttää rajoitetun taulukkotietoja.

Ensin sinun on suoritettava opetusohjelman [Xamarin.iOS sovelluksen luominen]. Jos et käytä ladatut pikaopas server-projekti-todennus tunniste-paketti on lisättävä projektin. Katso lisätietoja server tunniste-paketit [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Rekisteröi todennustavaksi sovelluksen ja sovelluksen palvelujen määrittäminen

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Todennettujen käyttäjien käyttöoikeuksien rajaamiseen

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. Valitse Visual Studiossa tai Xamarin Studio suorittaa asiakkaan projektin laitteen tai emulaattorin. Varmista, että 401 (ei oikeuksia), tilakoodi käsittelemättömän poikkeuksen käynnistyy, kun sovellus käynnistyy. Virhe on kirjautunut virheenkorjaus konsoliin. Niin Visual Studion pitäisi näkyä virheen kohde-ikkunassa.

&nbsp;&nbsp;Luvattoman virheeseen tapahtuu, koska sovellus yrittää käyttää Mobile-sovelluksen-taustatietokannan Todentamattomille käyttäjänä. Nyt *TodoItem* taulukon edellyttää käyttöoikeuden tarkistusta.

Seuraavaksi voit päivittää asiakas-sovelluksen pyynnön resurssien Mobile-sovelluksen taustasta todennetun käyttäjän kanssa.

##<a name="add-authentication-to-the-app"></a>Käyttöoikeuksien lisääminen sovellukseen

Tässä osassa muokataan sovellus ja kirjaudu sisään-näyttöön ennen tietojen näyttäminen. Kun sovellus käynnistyy, se ei ole muodostaa sovelluksen-palveluun ja eivät näy mitään tietoja. Kun ensimmäisen kerran, jotka käyttäjä suorittaa päivityksen, liike kirjautumisruutu tulee näkyviin, onnistumisen kirjautumisen jälkeen todo kohteet näytetään.

1. Asiakkaan Projectissa, Avaa tiedosto **QSTodoService.cs** ja lisää seuraava lauseella ja `MobileServiceUser` kanssa seuraaja QSTodoService luokkaan:

    ```
        using UIKit;
    ```

        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. Lisää uusi menetelmä nimeltä **QSTodoService** , **Tarkista** seuraavat määritykset:


        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Jos käytössäsi on kuin Facebook tunnistetietojen toimittaja, muuttaa välitetty **LoginAsync** jokin seuraavista toimista arvo: _microsoftaccount käytettävissä_, _Twitter-_, _Google_tai _WindowsAzureActiveDirectory_.

3. Avaa **QSTodoListViewController.cs**. Muokkaa menetelmä määritelmän **ViewDidLoad** poistaminen **RefreshAsync()** puhelu lähellä loppuun:

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. Muokkaa menetelmä **RefreshAsync** tarkistamiseen, jos **käyttäjä** -ominaisuus on tyhjä. Lisää seuraava koodi menetelmän määritys yläreunassa:

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. Visual Studiossa tai Xamarin Studio yhdistetty Xamarin muodosta isäntä Mac-tietokoneeseen, suorita kohdistamisen laitteen tai emulaattorin asiakkaan projekti. Tarkista, että sovellus näkyy mitään tietoja.

    Päivitä-ele minäkin alaspäin osia, jotka aiheuttavat kirjautumisnäyttö näkyvän luettelossa. Kun olet määrittänyt onnistuneesti kelvolliset tunnistetiedot-sovellus näkyy todo kohteet ja tekemäsi päivitykset tietoihin.


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Xamarin.iOS sovelluksen luominen]: app-service-mobile-xamarin-ios-get-started.md
