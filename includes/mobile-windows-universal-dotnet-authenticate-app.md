
1. Avaa jaettu projektitiedoston MainPage.cs ja lisää seuraavat koodikatkelman MainPage luokkaan:
    
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

3. Kommentti-kohtaa tai poistaa aiemmin **OnNavigatedTo** menetelmän ohitus **RefreshTodoItems** menetelmä puhelu.

    Tämä estää tietojen lataamisen, ennen kuin käyttäjä todennetaan. Lisää seuraavaksi **Kirjaudu sisään** -painike, joka käynnistää todennus-sovellukseen.

4. Lisää seuraavat koodikatkelman MainPage luokkaan:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Hide the login button and load items from the mobile app.
                ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Windows-kaupan sovelluksen Projectissa Avaa MainPage.xaml projektitiedoston ja lisää seuraava **painike** elementti, juuri ennen elementtiä, joka määrittää **Tallenna** -painiketta:

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. Windows Phone-kaupan sovelluksen Projectissa Lisää seuraava **painike** -elementissä **ContentPanel** **tekstiruudun** elementin jälkeen:

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
            Margin="10, 0, 0, 0" Visibility="Visible">Sign in</Button>

8. Avaa jaettu App.xaml.cs projektitiedoston ja lisää seuraava koodi:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    Jos **OnActivated** menetelmä on jo luotu, voit lisätä `#if...#endif` koodin estä.

9. Painamalla F5-näppäintä, suorita Windows-kaupan sovellus ja valitse **Kirjaudu sisään** -painike valittu tunnistetietojen palvelussa sovellukseen kirjautuminen. 

    Kun olet onnistuneesti kirjautunut sisään, sovellus suoritetaan ilman virheitä ja pitäisi onnistua kyselyn oman Taustajärjestelmä ja tietojen päivittämisen.

10. Windows Phone-kaupan sovelluksen projektin hiiren kakkospainikkeella, valitse **Käynnistys Project asettaa**sitten toista edellinen vaihe, varmista, että Windows Phone-kaupan sovelluksen myös toimii oikein.  

 