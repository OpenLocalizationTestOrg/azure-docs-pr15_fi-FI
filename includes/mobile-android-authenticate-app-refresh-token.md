Tutustu suojaustunnuksen välimuistin pitäisi toimia yksinkertainen tapaus, mutta mitä tapahtuu, kun tunnuksen päättynyt tai se on kumottu? Tunnuksen voitu päättyy, kun sovellus ei ole käynnissä. Tämä tarkoittaa suojaustunnuksen välimuisti on virheellinen. Tunnuksen voi myös päättyy, kun sovellus on käynnissä. Tulos on HTTP-tila-koodiin 401-ei oikeuksia". 

Annettava voi tunnistaa vanhentuneet tunnuksen ja Päivitä. Voit tehdä tämän Käytämme [ServiceFilter](http://dl.windowsazure.com/androiddocs/com/microsoft/windowsazure/mobileservices/ServiceFilter.html) [Android asiakkaan kirjastoon](http://dl.windowsazure.com/androiddocs/).

Tässä osassa määritetään ServiceFilter, joka tunnistaa HTTP tila koodin 401 vastauksen ja tunnuksen ja suojaustunnuksen välimuistin päivityksen käynnistäminen. Lisäksi tässä ServiceFilter estää muita lähtevien pyyntöjen todennuksen aikana niin, että nämä pyynnöt käyttää päivitetyt tunnuksen.

1. Avaa ToDoActivity.java-tiedosto ja lisää tuonti seuraavia komentoja:
 
        import java.util.concurrent.atomic.AtomicBoolean;
        import java.util.concurrent.ExecutionException;

        import com.microsoft.windowsazure.mobileservices.MobileServiceException;
 
2. Lisää seuraavien jäsenten `ToDoActivity` luokka. 

        public boolean bAuthenticating = false;
        public final Object mAuthenticationLock = new Object();

    Nämä käytetään helpottavat synkronointia käyttäjän todennusta. Vain haluat todentaa kerran. Puhelujen todennuksen aikana olisi Odota ja käyttää uuden tunnuksen-todennus on meneillään.

3. ToDoActivity.java-tiedostossa, jota käytetään estää muita viestiketjuissa siirtyminen lähtevien puhelujen todennus ollessa käynnissä ToDoActivity luokan lisääminen seuraavalla tavalla.

        /**
         * Detects if authentication is in progress and waits for it to complete. 
         * Returns true if authentication was detected as in progress. False otherwise.
         */
        public boolean detectAndWaitForAuthentication()
        {
            boolean detected = false;
            synchronized(mAuthenticationLock)
            {
                do
                {
                    if (bAuthenticating == true)
                        detected = true;
                    try
                    {
                        mAuthenticationLock.wait(1000);
                    }
                    catch(InterruptedException e)
                    {}
                }
                while(bAuthenticating == true);
            }
            if (bAuthenticating == true)
                return true;
            
            return detected;
        }
        

4. ToDoActivity.java-tiedoston lisääminen ToDoActivity luokan seuraavalla tavalla. Tämä menetelmä käynnistää tämän ja Päivitä lähtevä pyynnöt-tunnuksen todennus päätyttyä. 

        
        /**
         * Waits for authentication to complete then adds or updates the token 
         * in the X-ZUMO-AUTH request header.
         * 
         * @param request
         *            The request that receives the updated token.
         */
        private void waitAndUpdateRequestToken(ServiceFilterRequest request)
        {
            MobileServiceUser user = null;
            if (detectAndWaitForAuthentication())
            {
                user = mClient.getCurrentUser();
                if (user != null)
                {
                    request.removeHeader("X-ZUMO-AUTH");
                    request.addHeader("X-ZUMO-AUTH", user.getAuthenticationToken());
                }
            }
        }


5. ToDoActivity.java-tiedostossa, Päivitä `authenticate` ToDoActivity maksutavan luokan niin, että se hyväksyy totuusarvo parametrin sallimaan pakottaminen tunnuksen ja suojaustunnuksen välimuistin päivitys. Myös annettava minkä tahansa estetyn viestiketjuissa siirtyminen Ilmoita todennus on valmis, joten he voivat uuden tunnuksen.

        /**
         * Authenticates with the desired login provider. Also caches the token. 
         * 
         * If a local token cache is detected, the token cache is used instead of an actual 
         * login unless bRefresh is set to true forcing a refresh.
         * 
         * @param bRefreshCache
         *            Indicates whether to force a token refresh. 
         */
        private void authenticate(boolean bRefreshCache) {
            
            bAuthenticating = true;
            
            if (bRefreshCache || !loadUserTokenCache(mClient))
            {
                // New login using the provider and update the token cache.
                mClient.login(MobileServiceAuthenticationProvider.MicrosoftAccount,
                        new UserAuthenticationCallback() {
                            @Override
                            public void onCompleted(MobileServiceUser user,
                                    Exception exception, ServiceFilterResponse response) {
    
                                synchronized(mAuthenticationLock)
                                {
                                    if (exception == null) {
                                        cacheUserToken(mClient.getCurrentUser());
                                        createTable();
                                    } else {
                                        createAndShowDialog(exception.getMessage(), "Login Error");
                                    }
                                    bAuthenticating = false;
                                    mAuthenticationLock.notifyAll();
                                }
                            }
                        });
            }
            else
            {
                // Other threads may be blocked waiting to be notified when 
                // authentication is complete.
                synchronized(mAuthenticationLock)
                {
                    bAuthenticating = false;
                    mAuthenticationLock.notifyAll();
                }
                createTable();
            }
        }   



6. Lisää tämä koodi ToDoActivity.java-tiedostoon uuden `RefreshTokenCacheFilter` luokan ToDoActivity luokan sisällä:

        /**
        * The RefreshTokenCacheFilter class filters responses for HTTP status code 401. 
         * When 401 is encountered, the filter calls the authenticate method on the 
         * UI thread. Out going requests and retries are blocked during authentication. 
         * Once authentication is complete, the token cache is updated and 
         * any blocked request will receive the X-ZUMO-AUTH header added or updated to 
         * that request.   
         */
        private class RefreshTokenCacheFilter implements ServiceFilter {
         
            AtomicBoolean mAtomicAuthenticatingFlag = new AtomicBoolean();                     
            
            @Override
            public ListenableFuture<ServiceFilterResponse> handleRequest(
                    final ServiceFilterRequest request, 
                    final NextServiceFilterCallback nextServiceFilterCallback
                    )
            {
                // In this example, if authentication is already in progress we block the request
                // until authentication is complete to avoid unnecessary authentications as 
                // a result of HTTP status code 401. 
                // If authentication was detected, add the token to the request.
                waitAndUpdateRequestToken(request);
     
                // Send the request down the filter chain
                // retrying up to 5 times on 401 response codes.
                ListenableFuture<ServiceFilterResponse> future = null;
                ServiceFilterResponse response = null;
                int responseCode = 401;
                for (int i = 0; (i < 5 ) && (responseCode == 401); i++)
                {
                    future = nextServiceFilterCallback.onNext(request);
                    try {
                        response = future.get();
                        responseCode = response.getStatus().getStatusCode();
                    } catch (InterruptedException e) {
                       e.printStackTrace();
                    } catch (ExecutionException e) {
                        if (e.getCause().getClass() == MobileServiceException.class)
                        {
                            MobileServiceException mEx = (MobileServiceException) e.getCause();
                            responseCode = mEx.getResponse().getStatus().getStatusCode();
                            if (responseCode == 401)
                            {
                                // Two simultaneous requests from independent threads could get HTTP status 401. 
                                // Protecting against that right here so multiple authentication requests are
                                // not setup to run on the UI thread.
                                // We only want to authenticate once. Requests should just wait and retry 
                                // with the new token.
                                if (mAtomicAuthenticatingFlag.compareAndSet(false, true))                                                                                                      
                                {
                                    // Authenticate on UI thread
                                    runOnUiThread(new Runnable() {
                                        @Override
                                        public void run() {
                                            // Force a token refresh during authentication.
                                            authenticate(true);
                                        }
                                    });
                                }
    
                                // Wait for authentication to complete then update the token in the request. 
                                waitAndUpdateRequestToken(request);
                                mAtomicAuthenticatingFlag.set(false);                                                  
                            }
                        }
                    }
                }
                return future;
            }
        }


    Tässä suodatin tarkastaa kunkin vastauksen HTTP-tilakoodi 401-ei oikeuksia". Jos 401 kirjautuminen uuden kokouspyynnön voit hankkia uuden tunnuksen on käyttöliittymäsäikeen asetukset. Vastaamatta tuleviin puheluihin estetään, kunnes kirjautuminen on valmis, tai 5 yritykset epäonnistui. Jos uuden tunnuksen saadaan pyynnön, joka on käynnistänyt 401 yritetään uuden tunnuksen ja estettyjen puhelujen yritetään uuden tunnuksen. 

7. Lisää tämä koodi ToDoActivity.java-tiedostoon uuden `ProgressFilter` luokan ToDoActivity luokan sisällä:
        
        /**
        * The ProgressFilter class renders a progress bar on the screen during the time the App is waiting for the response of a previous request.
        * the filter shows the progress bar on the beginning of the request, and hides it when the response arrived.
        */
        private class ProgressFilter implements ServiceFilter {
            @Override
            public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback nextServiceFilterCallback) {

                final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();

                runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
                    }
                });

                ListenableFuture<ServiceFilterResponse> future = nextServiceFilterCallback.onNext(request);

                Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
                    @Override
                    public void onFailure(Throwable e) {
                        resultFuture.setException(e);
                    }

                    @Override
                    public void onSuccess(ServiceFilterResponse response) {
                        runOnUiThread(new Runnable() {

                            @Override
                            public void run() {
                                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.GONE);
                            }
                        });

                        resultFuture.set(response);
                    }
                });

                return resultFuture;
            }
        }
        
    Tämä suodatin näkyy tilanneilmaisin pyynnön alkuun ja piilottaa sen kun vastauksen saapunut.

8. ToDoActivity.java-tiedostossa, Päivitä `onCreate` menetelmä seuraavasti:

        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            
            setContentView(R.layout.activity_to_do);
            mProgressBar = (ProgressBar) findViewById(R.id.loadingProgressBar);
        
            // Initialize the progress bar
            mProgressBar.setVisibility(ProgressBar.GONE);
        
            try {
                // Create the Mobile Service Client instance, using the provided
                // Mobile Service URL and key
                mClient = new MobileServiceClient(
                        "https://<YOUR MOBILE SERVICE>.azure-mobile.net/",
                        "<YOUR MOBILE SERVICE KEY>", this)
                           .withFilter(new ProgressFilter())
                           .withFilter(new RefreshTokenCacheFilter());
            
                // Authenticate passing false to load the current token cache if available.
                authenticate(false);
                    
            } catch (MalformedURLException e) {
                createAndShowDialog(new Exception("Error creating the Mobile Service. " +
                    "Verify the URL"), "Error");
            }
        }


       In this code, `RefreshTokenCacheFilter` is used in addition to `ProgressFilter`. Also during `onCreate` we want to load the token cache. So `false` is passed in to the `authenticate` method.


