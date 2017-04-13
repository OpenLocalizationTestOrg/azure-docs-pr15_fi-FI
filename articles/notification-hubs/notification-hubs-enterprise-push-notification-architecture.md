<properties
    pageTitle="Ilmoitus keskittimet - yrityksen Push-arkkitehtuuri"
    description="Ohjeita Azure ilmoituksen keskittimet käyttäminen yritysympäristössä"
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="enterprise-push-architectural-guidance"></a>Yrityksen push arkkitehtuuri ohjeet

Yrityksille tänään on vähitellen siirtäminen kohti mobile-sovellusten joko luomisesta niiden loppukäyttäjät (ulkoinen) tai (sisäinen) työntekijöiden. Heillä on aiemmin Taustajärjestelmä järjestelmien paikassa olla se, keskustietokoneita tai joitakin Toimialasovellusten, joka on integroitu mobiilisovelluksen arkkitehtuuri. Tässä oppaassa keskustella parhaista tekemään suositellaan mahdollinen ratkaisu Yleisiä tilanteita, joissa integrointi.

Usein käytetyt vaatimus koskee push ilmoituksen lähettäminen niiden mobiilisovelluksen käyttäjiin tapahtuman halutut toteutuessa Taustajärjestelmä järjestelmien. Esimerkiksi pankin asiakas, joka on pankin pankin app hänen iPhonen haluaa ilmoituksen, kun pankki tehdään tietyn henkilön tili tai intranet-skenaario, jossa työntekijää budjetin hyväksyntä-sovellus Windows Phone hänen omistavan talousosaston haluaa ilmoituksen, kun hän saa hyväksyntäpyyntö summan yläpuolella.

Pankkitilin tai hyväksynnän käsittely todennäköisesti on tehtävä joitakin Taustajärjestelmä, joka on aloitettava push käyttäjälle. Voi olla useita tällaisia Taustajärjestelmä-järjestelmiä, jossa on kaikki luominen logiikan toteuttamisesta push, kun tapahtuman ilmoituksen samanlaisia. Monimutkainen tähän virheellisesti integrointi useita Taustajärjestelmä järjestelmien kanssa yksittäisen push-järjestelmän jossa loppukäyttäjille olet tilannut eri ilmoitusten ja myös ehkä useita mobile sovelluksia, kuten intranet-mobiilisovellukset kyseessä jossa yksi mobiilisovelluksen kannattaa ilmoituksia useista tällaisten Taustajärjestelmä järjestelmistä. Taustajärjestelmä järjestelmien ei tiedä tai tarvitsee tietää push semantiikkaan liittyvien/tekniikan, jotta tähän yleisiä ratkaisu on perinteisesti ottamaan osa, joka tekee kyselyn halutut tapahtumat Taustajärjestelmä järjestelmiä ja vastaava push-viestien lähettäminen asiakkaalle.
Seuraavassa käsittelemme näkyisivät paremmin ratkaisu, joka on Azure-palvelu Bus - aiheen/tilauksen malli, joka vähentää monimutkaisuus muodostettaessa ratkaisun skaalattava avulla.

Seuraavassa on yleinen arkkitehtuuri-ratkaisun (generalized useita mobiilisovellukset kanssa, mutta tasaisesti käytettävissä, kun kyseessä on vain yksi mobiilisovelluksen)

## <a name="architecture"></a>Arkkitehtuuri

![][1]

Tärkein arkkitehtuuri kaaviossa on Azure palvelun Bus, joka sisältää aiheet/tilaukset ohjelmointi mallin (Lisätietoja sen osoitteessa [palvelun Bus kirja ja Sub ohjelmointi]). Vastaanottajan, joka on tässä tapauksessa Mobile Taustajärjestelmä (yleensä [Azure mobiilipalvelun], joka käynnistää push mobiilisovellusten avulla) vastaanota viestit suoraan Taustajärjestelmä Systemsin, mutta sen sijaan [Azure palvelun Bus] joka mahdollistaa mobile taustaan haluat vastaanottaa viestejä Taustajärjestelmä-järjestelmistä myöntämä keskitason abstrahointikerros on. Palvelun Bus aiheen on luotava kullekin Taustajärjestelmä järjestelmien esimerkiksi tilin HR, rahoitus, jotka ovat lähinnä "aiheet" halutut, joka käynnistää viestejä voidaan lähettää kuin push-ilmoitus. Taustajärjestelmä järjestelmien lähettää viestejä näitä ohjeita. Yhden tai useamman tällaisen aiheet tilata Mobile taustassa luomalla palvelun Bus tilauksen. Tämä oikeuta mobile taustaan haluat vastaanottaa ilmoituksen vastaavan Taustajärjestelmä. Mobiili Taustajärjestelmä jatkuu kuunnella niiden tilaukset-viestejä ja heti, kun viesti saapuu, se muuttuu takaisin ja lähettää ilmoituksen sen ilmoitus-keskittimeen-muodossa. Ilmoitus keskittimet sitten myöhemmin toimittaa viestin mobile-sovellus. Niin Yhteenveto tärkeimmät komponentit on:

1. Taustajärjestelmä järjestelmien (LoB/vanha systems)
    - Luo palvelun Bus aiheen
    - Lähettää viestin
2. Mobiili taustaan
    - Luo ylläpitosopimus
    - Vastaanottaa viestin (mistä Taustajärjestelmä)
    - Lähettää ilmoituksen asiakkaille (joko Azure ilmoitus-toiminnossa)
3. Mobiilisovelluksen
    - Vastaanottaa ja näyttää ilmoituksen

###<a name="benefits"></a>Edut:

1. Lisää Taustajärjestelmä järjestelmien on integroitu mahdollisimman vähän muuta irtautus vastaanotin (sovelluksen/mobiilipalvelu ilmoituksen keskittimeen kautta) – lähettäjän (Taustajärjestelmä systems) avulla.
2. Näin voit myös useita mobiilisovellukset ei voi vastaanottaa tapahtumien Taustajärjestelmä-Systemsin skenaario.  

## <a name="sample"></a>Esimerkki:

###<a name="prerequisites"></a>Edellytykset
On suoritettava seuraavat opetusohjelmat tutustua käsitteitä sekä yleisiä luominen ja määritysvaiheet:

1. [Palvelun Bus kirja ja Sub ohjelmointi] - tämä kerrotaan tiedot toimimasta palvelun Bus aiheet ja tilaukset-luomisesta nimitila sisältävät aiheet/tilaukset, voit lähettää ja vastaanottaa viestejä ne.
2. [Ilmoitus keskittimet - Windows yleinen opetusohjelma] – tämä kerrotaan, kuinka Windows-kaupan sovelluksen määrittäminen ja käyttäminen ilmoituksen keskittimet Rekisteröi ja vastaanottaa ilmoituksia.

###<a name="sample-code"></a>Esimerkki koodi

Otoksen koko-koodi on osoitteessa [Ilmoituksen keskittimeen objektit]. Jaettu kolme osaa:

1. **EnterprisePushBackendSystem**

    a. Projektin käyttää *WindowsAzure.ServiceBus* Nuget paketin ja [palvelun Bus kirja ja Sub ohjelmointi]perusteella.

    b. Tämä on yksinkertainen C# konsolisovellus, voit simuloida LoB-järjestelmästä, joka käynnistää viestin toimitetaan mobile-sovellus.

        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the topic where we will send notifications
            CreateTopic(connectionString);

            // Send message
            SendMessage(connectionString);
        }

    c-näppäinyhdistelmää. `CreateTopic`käytetään palvelun Bus aiheen luominen Jos lähetämme viestejä.

        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already

            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }

    d. `SendMessage`viestien lähettäminen ohjeaihe Bus palvelua käytetään. Olemme tähän riittää, että lähettää satunnaisia viestien joukko aiheeseen säännöllisesti otosten varten. Tavallisesti ole Taustajärjestelmä, joka lähettää viestejä tapahtuman yhteydessä.

        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };

            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);

                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }

2. **ReceiveAndSendNotification**

    a. Projektin käyttää *WindowsAzure.ServiceBus* ja *Microsoft.Web.WebJobs.Publish* Nuget-paketit ja [palvelun Bus kirja ja Sub programming]perusteella.

    b. Tämä on toiseen C# konsolisovellus, joka on suoritetaan [Azure WebJob] koska se on jatkuvasti LoB/Taustajärjestelmä järjestelmistä viestien kuunteleminen. Tämä on osa Mobile Taustajärjestelmä.

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c-näppäinyhdistelmää. `CreateSubscription`käytetään palvelun Bus tilauksen aiheen luomiseen, jossa Taustajärjestelmä lähettää viestejä. Liiketoiminnan toimintamallin tämän osan luoda yhden tai useamman tilaukset vastaavan aiheisiin (kuten joitakin voi vastaanottaa viestejä HR järjestelmästä, jotkin rahoitus-järjestelmän ja niin edelleen)

        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }

    d. ReceiveMessageAndSendNotification käytetään viestin lukea sen tilauksen käyttäminen aihetta ja jos luku on tarkistettu sitten luoda ilmoituksen (Valitse esimerkkitilanne Windows alkuperäisen ilmoitusruudun) käyttämällä Azure ilmoituksen keskittimet mobiilisovelluksen lähettämisen.

        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");

            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);

            Client.Receive();

            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");

                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);

                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }

    e. Julkaiseminen tämä **WebJob**-ratkaisun käyttöön Visual Studiossa napsauttamalla hiiren kakkospainikkeella ja valitse **Julkaise WebJob**

    ![][2]

    f. Valitse julkaisun profiilisi ja luoda uuden Azure-sivuston, jos se ei ole jo joka isännöi tämän WebJob ja kun sivuston sitten **Julkaise**.

    ![][3]

    g. Määritä työ on "Suorittaa jatkuvasti", niin, että silloin, kun kirjaudut [Azure perinteinen Portal] pitäisi näkyä seuraavanlaiselta:

    ![][4]


3. **EnterprisePushMobileApp**

    a. Tämä on Windows-kaupan sovellus, joka vastaanottaa ilmoitukseen ilmoitukset-käynnissä Mobile Taustajärjestelmä osana WebJob ja tuo se näyttöön. Tämä perustuu [Ilmoituksen keskittimet - Windows yleinen opetusohjelma].  

    b. Varmista, että sovelluksesi on määritetty vastaanottamaan ilmoitukseen ilmoitukset.

    c-näppäinyhdistelmää. Varmista, että seuraava ilmoitus keskittimet rekisteröintikoodin kutsutaan osoitteessa sovelluksen käynnistäminen (jälkeen korvaaminen *HubName* ja *DefaultListenSharedAccessSignature*:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Malli on käynnissä:

1. Varmista, että WebJob suoritetaan onnistuneesti ja aikataulun mukaan "Suorita jatkuvasti".
2. Suorita **EnterprisePushMobileApp** , jotka alkavat Windows-kaupan sovelluksen.
3. Suorita **EnterprisePushBackendSystem** konsolin sovellusta, jolla simuloida LoB Taustajärjestelmä ja aloittaa viestien lähettäminen ja pitäisi näkyä ilmoitukseen ilmoitukset näkyvä seuraavalla tavalla:

    ![][5]

4. Viestien alun perin lähetettiin palvelun Bus aiheisiin, joka on seurataan työtäsi WWW-palvelun Bus-tilauksissa. Kun viesti on vastaanotettu, ilmoituksen on luotu ja lähettää mobile-sovellus. Voit selata WebJob lokit Vahvista käsittely, kun siirryt lokit linkki [Azure perinteinen] portaalissa Web-työ:

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Ilmoitus keskittimeen objektit]: https://github.com/Azure/azure-notificationhubs-samples
[Azure mobiilipalvelu]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure palvelun Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Palvelun Bus kirja ja Sub-ohjelmoinnin]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Ilmoitus keskittimet - Windows yleinen opetusohjelma]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure perinteinen Portal]: https://manage.windowsazure.com/
