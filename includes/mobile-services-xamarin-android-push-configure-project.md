
1. Ratkaisu näkymä (tai **Ratkaisunhallinnassa** Visual Studiossa) Napsauta **osat** -kansiota hiiren kakkospainikkeella, valitsemalla **Hae lisää osia...**, Etsi **Google Cloud Messaging asiakas** -osan ja lisääminen projektiin.

2. Avaa projektitiedoston ToDoActivity.cs ja lisää seuraava lauseella luokkaan:

        using Gcm.Client;

3. Lisää **ToDoActivity** -luokan uusi seuraava koodi: 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    Näin voit käyttää mobiilisovelluksen esiintymän push käsittelijä service-prosessin.

4.  Lisää seuraava koodi **OnCreate** -menetelmä **MobileServiceClient** luomisen jälkeen:

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Oman **ToDoActivity** on nyt valmis lisäämiseen push-ilmoitukset.