<properties
    pageTitle="Todennus Xamarin Android Mobile-sovellusten käytön aloittaminen"
    description="Opettele todentaa käyttäjät Xamarin Android-sovelluksen kautta tunnistetietojen toimittajat, mukaan lukien AAD, Google, Facebookiin, Twitteriin ja Microsoft erilaisia Mobile-sovellusten avulla."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinandroid-app"></a>Käyttöoikeuksien lisääminen Xamarin.Android-sovellukseen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Tässä ohjeaiheessa esitellään, miten-asiakassovellus Mobile-sovelluksen käyttäjien todentamiseen. Tässä opetusohjelmassa käyttöoikeuksien lisääminen pikaopas projektin identity-palvelun, joka tue Azure Mobile-sovellusten avulla. Kun olet parhaillaan todennettu onnistuneesti ja valtuutettuja Mobile-sovelluksesta, näkyy käyttäjän tunnus-arvo.

Tässä opetusohjelmassa perustuu Mobile-sovelluksen pikaopas. Sinun on suoritettava myös ensin opetusohjelman [Xamarin.Android sovelluksen luominen]. Jos et käytä ladatut pikaopas server-projekti-todennus tunniste-paketti on lisättävä projektin. Katso lisätietoja server tunniste-paketit [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register"></a>Rekisteröi todennustavaksi sovelluksen ja sovelluksen palvelujen määrittäminen

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Todennettujen käyttäjien käyttöoikeuksien rajaamiseen

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Visual Studiossa tai Xamarin Studio suorittaa asiakkaan projektin laitteen tai emulaattorin. Varmista, että 401 (ei oikeuksia), tilakoodi käsittelemättömän poikkeuksen käynnistyy, kun sovellus käynnistyy. Tämä johtuu siitä, sovellus yrittää käyttää Mobile-sovelluksen-taustatietokannan Todentamattomille käyttäjänä. Nyt *TodoItem* taulukon edellyttää käyttöoikeuden tarkistusta.

Seuraavaksi voit päivittää asiakas-sovelluksen pyynnön resurssien Mobile-sovelluksen taustasta todennetun käyttäjän kanssa.

##<a name="add-authentication"></a>Käyttöoikeuksien lisääminen sovellukseen

Sovellus päivitetään kehottavan käyttäjiä todennuksen, ennen kuin tiedot näytetään ja **Kirjaudu sisään** -painiketta.

1. Lisää seuraava koodi **TodoActivity** luokkaan:

        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");

                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }

        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;

                // Load the data.
                OnRefreshItemsSelected();
            }
        }

    Tämä luo uusi menetelmä tarkistamiseen käyttäjän ja menetelmä käsittelytoiminto uusi **Kirjaudu sisään** -painiketta. Yllä esimerkkikoodi käyttäjä todennetaan käyttämällä Facebook-kirjautuminen. Keskustelun käytetään näyttämiseen kerran todennettu Käyttäjätunnus.

    > [AZURE.NOTE] Jos käytössäsi on muu kuin Facebook tunnistetietojen toimittaja, muuttaa välitetty **LoginAsync** jokin seuraavista toimista arvo: _microsoftaccount käytettävissä_, _Twitter-_, _Google_tai _WindowsAzureActiveDirectory_.

3. **OnCreate** -menetelmä poistaminen tai kommentti-kohtaa koodi seuraava rivi:

        OnRefreshItemsSelected ();

4. Lisää seuraavan *LoginUser* painike määritelmän ennen *AddItem* painikkeeseen Activity_To_Do.axml-tiedostoon:

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. Lisää seuraava elementti Strings.xml resurssit-tiedosto:

        <string name="login_button_text">Sign in</string>

6. Visual Studiossa tai Xamarin Studio Suorita asiakkaan projekti laitteen tai emulaattorin ja kirjaudu sisään valitun jäsenyyden palveluntarjoajan.

    Kun olet onnistuneesti kirjautunut sisään, sovellus näkyy Käyttäjätunnus ja todo kohteet ja tekemäsi päivitykset tietoihin.


<!-- URLs. -->
[Xamarin.Android sovelluksen luominen]: app-service-mobile-xamarin-android-get-started.md
