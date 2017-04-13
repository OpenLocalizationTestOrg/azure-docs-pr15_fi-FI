
1. Android Studiossa **Project Explorer** ToDoActivity.java-tiedoston avaaminen ja lisää seuraavat Tuontilauseet.

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. Lisää **ToDoActivity** luokan seuraavalla tavalla: 
    
        private void authenticate() {
            // Login using the Google provider.
            
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
    
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();  
                }
            });     
        }


    Tämä luo uusi menetelmä käsittelemään todennusprosessin. Käyttäjä todennetaan Google-kirjautuminen käyttämällä. Valintaikkuna tulee näkyviin joka näkyy todennetun käyttäjän tunnus. Ei voi jatkaa ilman positiivinen todennusta.

    > [AZURE.NOTE] Jos käytössäsi on muu kuin Google tunnistetietojen toimittaja, muuttaa välitetty **Kirjautuminen** noudattamalla jokin seuraavista toimista arvo: _microsoftaccount käytettävissä_, _Facebook_, _Twitter-_tai _windowsazureactivedirectory_.

3. **OnCreate** tapaa Lisää seuraava rivi koodin jälkeen koodi, joka muodostaa `MobileServiceClient` objekti.

        authenticate();

    Puhelun käynnistää todennusprosessin.

4. Siirrä jälkeen jäljellä olevan koodin `authenticate();` uusi **createTable** menetelmä **onCreate** menetelmä, joka näyttää tältä:

        private void createTable() {
    
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from Azure.
            refreshItemsFromTable();
        }

9. **Suorita** -valikosta valitsemalla **Suorita sovellus** Käynnistä sovellus ja kirjaudu sisään valitun jäsenyyden palveluntarjoajan. 

    Kun olet onnistuneesti kirjautunut sisään, sovellus suoritetaan ilman virheitä ja pitäisi onnistua kyselyn Taustajärjestelmä-palvelun ja tietojen päivittämisen.