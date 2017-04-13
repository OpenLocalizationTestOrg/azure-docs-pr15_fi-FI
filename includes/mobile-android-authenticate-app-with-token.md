
Edellisen esimerkin ilmeni standardi kirjautuminen, joka edellyttää asiakas voi ottaa yhteyttä tunnistetietojen toimittaja ja Taustajärjestelmä Azure-palvelu, joka kerta, kun sovellus käynnistyy. Ei vain tämä menetelmä on tehotonta, voit suorittaa käyttö liittyvät asiat huomioon useille asiakkaille yritä Käynnistä sovellus samanaikaisesti. Vaihtoehto on välimuistin Azure-palvelu palautettu luvan tunnuksen ja yrität käyttää tätä ensimmäistä ennen kuin käytät toimittaja-pohjainen Kirjaudu sisään. 

>[AZURE.NOTE]Voit välimuistiin Taustajärjestelmä Azure palvelun riippumatta siitä, käytätkö asiakkaan hallitun tai palvelua hallita todennus myöntämä tunnuksen. Tässä opetusohjelmassa käyttää palvelua hallita todennusta.


1. Avaa ToDoActivity.java-tiedosto ja lisää tuonti seuraavia komentoja:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

2. Lisää seuraavien jäsenten `ToDoActivity` luokka.

        public static final String SHAREDPREFFILE = "temp"; 
        public static final String USERIDPREF = "uid";  
        public static final String TOKENPREF = "tkn";   


3. Lisää ToDoActivity.java-tiedostoon seuraavat määritelmä `cacheUserToken` menetelmää.
 
        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }   
  
    Tämä menetelmä tallentaa käyttäjätunnus ja tunnuksen asetustiedoston, joka on merkitty yksityinen. Tämä on suojattava access välimuistiin, niin, että muut sovellukset laitteeseen ei ole pääsy tunnuksen, koska tilaksi ei Eristetyn sovelluksen. Jos joku pääsee laitteeseen, se on kuitenkin mahdollista, että he voivat saada suojaustunnuksen välimuistin muulla tavalla. 

    >[AZURE.NOTE]Voit suojata salauksella tunnuksen Jos suojaustunnuksen voi käyttää tietojasi pidetään tallentamattomien ja käyttäjä voi päästä laitteeseen. Täysin suojattu ratkaisu on kuitenkin laajemmin tässä opetusohjelmassa ja määräytyy suojausvaatimuksia.


4. Lisää ToDoActivity.java-tiedostoon seuraavat määritelmä `loadUserTokenCache` menetelmää.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null); 
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null); 
            if (token == null)
                return false;
                
            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);
                
            return true;
        }



5. Korvaa *ToDoActivity.java* -tiedosto `authenticate` seuraavat menetelmä, jota käytetään suojaustunnuksen välimuistin menetelmää. Muuta kirjautuminen palveluntarjoajan, jos haluat kuin Google-tilin.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
        
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();  
                    }
                });
            }
        }

6. Luo sovellus ja testaa todennus kelvollinen-tilillä. Suorita vähintään kaksi kertaa. Ensimmäisen käynnistyksen aikana saat kehotteen kirjautua sisään ja luo suojaustunnuksen välimuistin. Tämän jälkeen jokaisen yrittää ladata todennustavaksi suojaustunnuksen välimuistin ja sinun tulee kirjautuminen edellyttää.



