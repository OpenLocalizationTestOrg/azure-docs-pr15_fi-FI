<properties
    pageTitle="Aloita Mobile-sovellusten tunnistus Xamarin.Forms sovelluksessa | Microsoft Azure"
    description="Opettele todentaa kautta tunnistetietojen toimittajat, mukaan lukien AAD, Google, Facebookiin, Twitteriin ja Microsoft erilaisia Xamarin lomakkeiden sovelluksesi käyttäjät Mobile-sovellusten avulla."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinforms-app"></a>Käyttöoikeuksien lisääminen Xamarin.Forms-sovellukseen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa esitellään, miten sovelluksen palvelun Mobile-sovelluksen hakeminen asiakassovellus käyttäjien todentamiseen. Tässä opetusohjelmassa käyttöoikeuksien lisääminen Xamarin.Forms pikaopas projektin identity-palvelun, joka tue sovelluksen-palvelun avulla. Kun parhaillaan todennettu onnistuneesti ja myöntänyt Mobile-sovelluksen, käyttäjän ID-arvo näkyy ja osaat käyttää rajoitetun taulukkotietoja.

##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa parhaan tuloksen Suosittelemme, että suoritat ensin [Xamarin.Forms sovelluksen luominen](app-service-mobile-xamarin-forms-get-started.md) opetusohjelman. Kun suoritat tässä opetusohjelmassa, sinun on Xamarin.Forms projekti, joka on usean ympäristö TodoList sovellus.

Jos et käytä ladattuja pikaopas server-projekti-todennus tunniste-paketti on lisättävä projektin. Katso lisätietoja server tunniste-paketit [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Rekisteröi todennustavaksi sovelluksen ja sovelluksen palvelujen määrittäminen

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Todennettujen käyttäjien käyttöoikeuksien rajaamiseen

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]


##<a name="add-authentication-to-the-portable-class-library"></a>Käyttöoikeuksien lisääminen kannettavat luokkakirjasto

Mobile-sovellusten käyttää [LoginAsync] tunniste-menetelmä [MobileServiceClient] sisään käyttäjä, jolla sovellus todentaminen. Tässä esimerkissä käytetään palvelimen rajoitetun käyttöoikeuden työnkulun, joka näyttää palveluntarjoajan kirjautumisen LIP-sovelluksessa. Lisätietoja on kohdassa [palvelimen rajoitetun käyttöoikeuden](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow). Antamaan parantaa käyttökokemusta tuotannon sovelluksen kannattaa käyttää sen sijaan [asiakkaan rajoitetun käyttöoikeuden](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow). 

Todentamismenetelmä Xamarin.Forms projektin **IAuthenticate** -liittymän määritetään kannettavat luokkakirjasto sovelluksen. Voit myös päivittää kannettavat luokkakirjasto Lisää **- Kirjautuminen** painike, jossa käyttäjä napsauttaa Aloita todennus määritelty käyttöliittymän. Todentaminen onnistuu, kun tiedot on ladattu mobiilisovelluksen Taustajärjestelmä.

Käyttöön kunkin sovelluksen tukemat ympäristö **IAuthenticate** -käyttöliittymä.


1. Avaa App.cs projektista ja **kannettavat** nimi, joka on kannettava luokkakirjasto projekti, valitse Lisää sitten seuraavat Visual Studiossa tai Xamarin Studio `using` lause:

        using System.Threading.Tasks;

2. App.cs, Lisää seuraavat `IAuthenticate` liittymän definition välittömästi ennen `App` luokan definition.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }

3. Seuraavat staattinen jäsenten lisääminen ympäristö tietyn toteutukseen-liittymän alustus **sovelluksen** -luokka.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }

4. Avaa TodoList.xaml kannettavat luokkakirjasto projektista, Lisää **painike** -elementissä *buttonsPanel* asettelu-osaan aiemmin painikkeen jälkeen: 

        <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>

    Tämä painike käynnistää palvelimen hallitun todentaminen mobiilisovelluksen Taustajärjestelmä.

5. Avaa TodoList.xaml.cs kannettavat luokkakirjasto projektista ja valitse Lisää seuraava kenttä `TodoList` luokan:

        // Track whether the user has authenticated. 
        bool authenticated = false;


6. **OnAppearing** menetelmä korvaa seuraava koodi:

        protected override async void OnAppearing()
        {
            base.OnAppearing();
    
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Näin varmistat, että tietoja vain päivittää-palvelusta, kun käyttäjä on tarkistettu.

7. Lisää seuraavat käsittelijä **Clicked** tapahtuman **TodoList** luokan:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }

8. Tallenna muutokset ja muodostaa kannettavat luokkakirjasto projektin tarkistetaan virheitä.


##<a name="add-authentication-to-the-android-app"></a>Käyttöoikeuksien lisääminen Android-sovellukseen

Tässä osassa esitellään ottamisesta käyttöön **IAuthenticate** -liittymän Android-sovelluksen Projectissa. Jos käytössä on ei Android-laitteissa voit ohittaa tämän osan.

1. Visual Studiossa tai Xamarin Studio Napsauta hiiren kakkospainikkeella **droid** projektin **määrittäminen projektin aloitus**.

2. Painamalla F5-näppäintä Käynnistä virheenkorjaus projekti ja valitse Vahvista, että 401 (ei oikeuksia) kanssa tilakoodin käsittelemättömän poikkeuksen käynnistyy, kun sovellus käynnistyy. Tämä johtuu siitä taustassa käyttöä on rajoitettu vain valtuutettujen käyttäjien.

3. Avaa MainActivity.cs Android projektiin ja lisää seuraava `using` lauseet:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Päivitä **MainActivity** luokan toteuttamisesta **IAuthenticate** -liittymää seuraavasti:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate


5. Päivitä **MainActivity** luokan lisäämällä **MobileServiceUser** kentän ja **Tarkista** -menetelmä, jota tarvitaan **IAuthenticate** -liittymän seuraavasti:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }


    Jos käytössäsi on muu kuin Facebook tunnistetietojen toimittaja, valitse toinen arvo [MobileServiceAuthenticationProvider].

6. Lisää seuraava koodi ennen kutsun **MainActivity** -luokan **OnCreate** -menetelmä `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Näin varmistat, että käyttöoikeudet on alustaa ennen sovelluksen Lataa.

7. Sovellus uudelleen, suorittaa ja valitse Kirjaudu sisään käyttöoikeustarkistuspalvelun valinnut ja tarkista olet voivat käyttää todennetun käyttäjän tiedot.

##<a name="add-authentication-to-the-ios-app"></a>Käyttöoikeuksien lisääminen iOS-sovellukseen

Tässä osassa esitellään ottamisesta käyttöön **IAuthenticate** -liittymän iOS-sovelluksen projektissa. Jos käytössä on ei iOS-laitteita, Ohita tämä osa.

1. Visual Studiossa tai Xamarin Studio Napsauta hiiren kakkospainikkeella **iOS** -projektin **määrittäminen projektin aloitus**.

2. Painamalla F5-näppäintä Käynnistä virheenkorjaus projekti ja valitse Vahvista, että 401 (ei oikeuksia), tilakoodi käsittelemättömän poikkeuksen käynnistyy, kun sovellus käynnistyy. Tämä johtuu siitä taustassa käyttöä on rajoitettu vain valtuutettujen käyttäjien.

4. Avaa AppDelegate.cs iOS projektiin ja lisää seuraava `using` lauseet:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Päivitä **AppDelegate** luokan toteuttamisesta **IAuthenticate** -liittymää seuraavasti:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate

5. Päivitä **AppDelegate** luokan lisäämällä **MobileServiceUser** kentän ja **Tarkista** -menetelmä, jota tarvitaan **IAuthenticate** -liittymän seuraavasti:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         

            return success;
        }

    Jos käytössäsi on muu kuin Facebook tunnistetietojen toimittaja, valitse toinen arvo [MobileServiceAuthenticationProvider].

6. Lisää seuraava rivi koodin **FinishedLaunching** menetelmä ennen kutsun `LoadApplication()`: 

        App.Init(this);

    Näin varmistat, että käyttöoikeudet on alustaa, ennen kuin sovellus on ladattu.

7. Sovellus uudelleen, suorittaa ja valitse Kirjaudu sisään käyttöoikeustarkistuspalvelun valinnut ja tarkista olet voivat käyttää todennetun käyttäjän tiedot.


##<a name="add-authentication-to-windows-app-projects"></a>Käyttöoikeuksien lisääminen Windows-sovellus projekteihin

Tässä osassa esitellään **IAuthenticate** -liittymän ottamisesta käyttöön Windows 8.1-ja Windows Phone 8.1 sovelluksen projektit. Samat vaiheet käyttää projektien yleinen Windows Platform (UWP). Jos käytössä on ei ole Windows-laitteet, Ohita tämä osa.

1. Visual Studiossa, napsauta hiiren kakkospainikkeella **WinApp** tai **WinPhone81** projektin **määrittäminen projektin aloitus**.

2. Painamalla F5-näppäintä Käynnistä virheenkorjaus projekti ja valitse Vahvista, että 401 (ei oikeuksia) kanssa tilakoodin käsittelemättömän poikkeuksen käynnistyy, kun sovellus käynnistyy. Tämä johtuu siitä taustassa käyttöä on rajoitettu vain valtuutettujen käyttäjien.

3. Avaa Windows-sovellus projektin MainPage.xaml.cs ja lisää seuraava `using` lauseet:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Korvaa `<your_Portable_Class_Library_namespace>` kannettavat luokkakirjasto nimitilaa kanssa.

4. Päivitä **MainPage** luokan toteuttamisesta **IAuthenticate** -liittymää seuraavasti:

        public sealed partial class MainPage : IAuthenticate


5. Päivitä **MainPage** luokan lisäämällä **MobileServiceUser** kentän ja **Tarkista** -menetelmä, jota tarvitaan **IAuthenticate** -liittymän seuraavasti:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
                
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }


    Jos käytössäsi on muu kuin Facebook tunnistetietojen toimittaja, valitse toinen arvo [MobileServiceAuthenticationProvider].

6. Lisää seuraava rivi koodin **MainPage** luokan ennen kutsun konstruktorissa `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
 
    Korvaa `<your_Portable_Class_Library_namespace>` kannettavat luokkakirjasto nimitilaa kanssa.  
    Jos kyseessä on WinApp projektiin, voit ohittaa vaihe 8 alaspäin. Seuraava vaihe koskee vain WinPhone81 projektin, mihin sinun on suoritettava kirjautuminen takaisinkutsu.

7. (Valinnainen) **WinPhone81** sovelluksen Projectissa, Avaa App.xaml.cs ja lisää seuraava `using` lauseet:

        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;

    Korvaa `<your_Portable_Class_Library_namespace>` kannettavat luokkakirjasto nimitilaa kanssa.

8.  Lisää **sovellus** -luokan **OnActivated** menetelmän-ohitus:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            // We just need to handle activation that occurs after web authentication. 
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Get the client and call the LoginComplete method to complete authentication.
                var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
                client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
        }

    Kun menetelmän ohitus on jo olemassa, Lisää vain yllä koodikatkelman ehdollinen koodi.

7. Sovellus uudelleen, suorittaa ja valitse Kirjaudu sisään käyttöoikeustarkistuspalvelun valinnut ja tarkista olet voivat käyttää todennetun käyttäjän tiedot.

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet suorittanut perustodentamista Tässä opetusohjelmassa, harkitse jatkuvaa voin jompikumpi seuraavista opetusohjelmat:

+ [Sovelluksen lisääminen push-ilmoitukset](app-service-mobile-xamarin-forms-get-started-push.md)  
  Opettele lisäämään push-ilmoitusten tuki, kun sovellus ja määritä Mobile-sovelluksen-taustatietokannan push-ilmoitukset lähetetään Azure ilmoituksen keskittimet avulla.

+ [Sovelluksen offline-synkronoinnin ottaminen käyttöön](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Opettele lisäämään offline-tuki käyttämällä Mobile-sovelluksen Taustajärjestelmä sovelluksen. Offline-synkronoinnin sallii loppukäyttäjien kanssa mobiilisovelluksessa&mdash;tarkasteleminen, lisääminen tai muokkaaminen tietojen&mdash;myös silloin, kun yhteyttä ei ole verkossa.

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx

