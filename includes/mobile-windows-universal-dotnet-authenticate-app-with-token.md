
1. Lisää MainPage.xaml.cs-projektitiedoston **käyttämällä** seuraavia komentoja:

        using System.Linq;      
        using Windows.Security.Credentials;

2. **AuthenticateAsync** menetelmä korvaa seuraava koodi:

        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;

            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;

            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }

            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;

                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;

                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.

                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);

                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);

                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
            
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();

            return success;
        }

    Tässä versiossa **AuthenticateAsync**sovellus yrittää käyttää palvelun käyttämiseen **PasswordVault** tallennettuja tunnistetietoja. Tavallinen kirjautumisvirheiden myös suoritetaan, kun ei ole tallennettu tunnistetiedon.

    >[AZURE.NOTE]Välimuistiin tallennetut tunnuksen vanhentua ja tunnuksen vanhentumisen voi myös ilmetä todennuksen jälkeen, kun sovellus on käytössä. Tietoja sen selvittämisestä, jos tunnus on vanhentunut-kohdassa [Tarkista vanhentuneet todennus tunnusten](http://aka.ms/jww5vp). Lisätietoja luvan vanhentuvien tunnusten liittyvien virheiden käsittelyä ratkaisuja, on artikkelissa viestin [Hallittujen SDK välimuisti ja käsittelemisen vanhentuneet tunnusten Azure Mobile Services-palveluissa](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 

3. Käynnistä sovellus uudelleen kahdesti.

    Huomaa, että ensimmäinen aikana, valitse Kirjaudu sisään palvelu uudelleen tarvitaan. Kuitenkin toisen uudelleenkäynnistyksen välimuistissa olevia käytetään ja kirjaudu sisään on ohitettu. 
