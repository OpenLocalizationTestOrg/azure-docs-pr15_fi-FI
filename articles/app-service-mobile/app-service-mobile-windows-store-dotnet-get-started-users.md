<properties
    pageTitle="Käyttöoikeuksien lisääminen yleinen Windows Platform (UWP)-sovellukseen | Azure-mobiilisovellukset"
    description="Opettele käyttämään Azure palvelun mobiilisovellukset tarkistamiseen tunnistetietojen toimittajat, mukaan lukien erilaisilla yleinen Windows Platform (UWP)-sovelluksesi käyttäjät: AAD, Google, Facebookiin, Twitteriin ja Microsoft."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-windows-app"></a>Käyttöoikeuksien lisääminen Windows-sovellus

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Tässä ohjeaiheessa esitellään pilvipohjainen käyttöoikeuksien lisääminen mobile-sovellus. Tässä opetusohjelmassa voit lisätä todennus yleinen Windows Platform (UWP) pikaopas projektiin Mobile-sovellusten, jota tuetaan Azure App palvelun identity-palvelun avulla. Kun olet parhaillaan todennettu onnistuneesti ja Mobile-sovelluksen Taustajärjestelmä myöntänyt, käyttäjän tunnus-arvon tulee näkyviin.

Tässä opetusohjelmassa perustuu mobiilisovellukset pikaopas. Ensin sinun on suoritettava opetusohjelman [Mobile-sovellusten käytön aloittaminen](app-service-mobile-windows-store-dotnet-get-started.md).

##<a name="register"></a>Rekisteröi todennustavaksi sovelluksen ja sovelluksen kertakirjauspalvelun asetusten määrittäminen

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Todennettujen käyttäjien käyttöoikeuksien rajaamiseen

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nyt voit varmistaa, että Taustajärjestelmä anonyymi käyttö on poistettu käytöstä. UWP sovelluksen Projectissa pysäyttämisestä projektin määrittäminen, käyttöönotto ja suorita sovellus; Varmista, että 401 (ei oikeuksia), tilakoodi käsittelemättömän poikkeuksen käynnistyy, kun sovellus käynnistyy. Tämä johtuu siitä, kun sovellus yrittää käyttää Mobile App koodin Todentamattomille käyttäjänä, mutta *TodoItem* taulukon nyt edellyttää käyttöoikeuden tarkistusta.

Seuraavaksi voit päivittää sovelluksen todentaa käyttäjät ennen pyytää resurssien sovelluksen-palvelusta.

##<a name="add-authentication"></a>Käyttöoikeuksien lisääminen sovellukseen

1. UWP sovelluksen projektin tiedoston MainPage.cs ja lisää seuraavat koodikatkelman MainPage luokkaan:
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    Tämä koodi todentaa käyttäjä, jolla Facebook-kirjautuminen. Jos käytössäsi on muu kuin Facebook tunnistetietojen toimittaja, muuta **MobileServiceAuthenticationProvider** arvo arvoksi palveluntarjoajan.

3. Kommentti-kohtaa tai poistaa aiemmin **OnNavigatedTo** menetelmän ohitus puhelun **ButtonRefresh_Click** -menetelmän (tai **InitLocalStoreAsync** -menetelmä). Tämä estää tietojen lataamisen, ennen kuin käyttäjä todennetaan. Lisää seuraavaksi **Kirjaudu sisään** -painike, joka käynnistää todennus-sovellukseen.

4. Lisää seuraavat koodikatkelman MainPage luokkaan:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. MainPage.xaml projektitiedoston avaaminen, Etsi elementti, joka määrittää **Tallenna** -painiketta ja korvaa se seuraava koodi:

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. Painamalla F5-näppäintä, suorita sovellus ja valitse **Kirjaudu sisään** -painike valittu tunnistetietojen palvelussa sovellukseen kirjautuminen. Kun käyttäjän kirjautuminen on tarkistettu, sovellus suoritetaan ilman virheitä ja olet kyselyn oman Taustajärjestelmä ja tietojen päivittämisen.


##<a name="tokens"></a>Tallenna todennus-tunnuksen asiakas

Edellisen esimerkin ilmeni standardi kirjautuminen, joka edellyttää asiakas voi ottaa yhteyttä tunnistetietojen toimittaja ja sovelluksen palvelu, joka kerta, kun sovellus käynnistyy. Ei vain on tämä menetelmä tehotonta, voit suorittaa kyselyjä käyttö-liittyvät ongelmat useille asiakkaille yritä Käynnistä sovellus samanaikaisesti. Vaihtoehto on välimuistin luvan tunnuksen sovelluksen palvelun palauttama ja yrität käyttää tätä ensimmäistä ennen kuin käytät palvelu-pohjainen Kirjaudu sisään.

>[AZURE.NOTE]Voit välimuistiin myöntämä App Services riippumatta siitä, käytätkö asiakkaan hallitun tai palvelua hallita todennus tunnuksen. Tässä opetusohjelmassa käyttää palvelua hallita todennusta.

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet suorittanut perustodentamista Tässä opetusohjelmassa, harkitse jatkuvaa voin jompikumpi seuraavista opetusohjelmat:

+ [Sovelluksen lisääminen push-ilmoitukset](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Opettele lisäämään push-ilmoitusten tuki, kun sovellus ja määritä Mobile-sovelluksen-taustatietokannan push-ilmoitukset lähetetään Azure ilmoituksen keskittimet avulla.

+ [Sovelluksen offline-synkronoinnin ottaminen käyttöön](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Opettele lisäämään offline-tuki käyttämällä Mobile-sovelluksen Taustajärjestelmä sovelluksen. Offline-synkronoinnin sallii loppukäyttäjien kanssa mobiilisovelluksessa&mdash;tarkasteleminen, lisääminen tai muokkaaminen tietojen&mdash;myös silloin, kun yhteyttä ei ole verkossa.


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

