<properties
    pageTitle="Ilmoitus keskittimet avulla voit lähettää uutisia (Windows Phone)"
    description="Azure ilmoituksen keskittimet avulla voit lähettää uutisia Windows Phone-sovelluksen merkintöjen tunnisteiden avulla."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Ilmoitus keskittimet avulla voit lähettää uutisia

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa esitellään käyttämisestä Azure ilmoituksen keskittimet yleislähetys jakautumisen uutisia ilmoitukset Windows Phone 8.0/8.1 Silverlight-sovellukseen. Jos kohdennat Windows-kaupan tai Windows Phone 8.1-sovelluksessa, katso [Windowsin yleinen](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) versioon. Kun olet valmis, jota voi rekisteröidä uudet luokat kiinnostavan purkaa, ja vain kyseisten luokkien push-ilmoitusten vastaanottaminen. Tämä skenaario on useita sovelluksia yleistä mallia, joiden ilmoitukset lähetetään käyttäjäryhmille, joita on aiemmin määritetty korko niitä, kuten RSS lukijan, sovellusten musiikkia tuulettimista jne.

Lähetyksen skenaariot otetaan käyttöön lisäämällä yhden tai useamman _tunnisteita_ luotaessa rekisteröinti ilmoitus-toiminnossa. Ilmoitukset lähetetään tunnisteen, kun kaikki laitteet, joissa olet rekisteröinyt tunnisteen saa ilmoituksen. Koska tunnisteet yksinkertaisesti merkkijonoja, niitä ei tarvitse valmisteltu etukäteen. Lisätietoja tunnisteet lisätietoja [ilmoituksen keskittimet reititys ja tunnisteen lausekkeita](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Edellytykset

Tässä ohjeaiheessa muodostaa luomasi [ilmoituksen keskittimet käytön aloittaminen]-sovellukseen. Ennen kuin aloitat Tässä opetusohjelmassa, olet jo tehnyt [ilmoituksen keskittimet käytön aloittaminen].

##<a name="add-category-selection-to-the-app"></a>Luokka-valinta lisääminen sovellukseen

Ensimmäiseksi on Lisää Käyttöliittymän osat, joiden avulla käyttäjä voi valita luokkia, voit rekisteröidä aiemmin tärkeimmät sivulle. Laitteessa on tallennettu käyttäjän valittuun luokkaan. Kun sovellus käynnistyy-laitteen rekisteröinti luodaan ilmoitus-toiminnossa valitun luokille tunnisteina.

1. Avaa MainPage.xaml projektitiedoston ja korvata nimeltä **ruudukon** osia `TitlePanel` ja `ContentPanel` se seuraavalla koodilla:

        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>

2. Projectissa Luo uusi luokka nimeltä **ilmoitukset**, Lisää **julkisen** määrite luokan määritys ja valitse Lisää seuraavat **käyttämällä** lauseet tiedostoon koodi:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;

3. Kopioi seuraava koodi uusi **ilmoitukset** -luokka.

        private NotificationHub hub;

        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();

            return await SubscribeToCategories();
        }

        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();

            var channel = HttpNotificationChannel.Find("MyPushChannel");

            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;

                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }

            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
            
            return await registrationTask.Task;
        }

        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }

        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";

            // The stored categories tags are passed with the template registration.

            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));

            return await registrationTask.Task;
        }

        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;

            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());

            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);

                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }

            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }


    Tähän luokkaan käyttää eristetyssä tallennuksessa uutisia, tämä laite on vastaanottamaan luokkiin. Se sisältää myös menetelmiä rekisteröidä näiden luokkien avulla [mallia](notification-hubs-templates-cross-platform-push-messages.md) ilmoituksen rekisteröinti.


4. App.xaml.cs project-tiedostoon Lisää seuraavat ominaisuus **App** luokan. Korvaa `<hub name>` ja `<connection string with listen access>` paikkamerkit ilmoitus-toiminnossa nimesi ja *DefaultListenSharedAccessSignature* , jotka olet hankkinut aiemmin yhteysmerkkijonon kanssa.

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    > [AZURE.NOTE] Koska tunnistetiedot, jotka asiakas-sovelluksen jaetaan ei yleensä suojatun, kuunnella access näppäintä pitäisi jakaa vain asiakas-sovelluksessa. Kuuntele access ottaa käyttöön sovelluksen Rekisteröidy ilmoitukset, mutta aiemmin merkintöjä ei voi muokata ja et voi lähettää ilmoitukset. Ilmoitusten lähettäminen ja muuttamalla aiemmin merkintöjen suojatun Taustajärjestelmä palvelun käytetään täydet oikeudet-näppäintä.

5. Lisää seuraava rivi oman MainPage.xaml.cs:

        using Windows.UI.Popups;

6. Lisää MainPage.xaml.cs project-tiedostoon seuraavasti:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
    
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
    
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }

    Tämä menetelmä luo luokkaluettelo ja käyttää **ilmoitukset** -luokan tallentamista paikalliseen muistiin ja rekisteröidä vastaavan tunnisteet ilmoitus-toiminnossa. Muuttuessa luokat rekisteröinnin luodaan uusi luokille.

Sovellus on nyt voi tallentaa luokkien laitteeseen paikallisen tallennustilan ja voit rekisteröidä ilmoitus-toiminnossa aina, kun käyttäjä muuttaa valintaa luokat.

##<a name="register-for-notifications"></a>Rekisteröidy ilmoitukset

Näin voit rekisteröidä käynnistyksen avulla luokat, jotka on tallennettu paikallisen tallennustilaan ilmoitus-toiminnossa.

> [AZURE.NOTE] Koska kanavan URI määritetty mukaan Microsoftin Push-ilmoituksen Service (MPNS) voit muuttaa milloin tahansa, Rekisteröi ilmoitusten usein tapahtuvien virheiden ilmoitus. Tässä esimerkissä Rekisteröi ilmoitukset aina, kun sovellus käynnistyy. Sovellusten, jotka suoritetaan usein useammin kuin kerran päivässä, voit todennäköisesti ohittaa rekisteröinti Jos edellisen rekisteröinti on kulunut alle yhden päivän kaistanleveyden säilyttää.


1. App.xaml.cs-tiedoston avaaminen ja lisää **asynkroninen** -määrite **Application_Launching** menetelmä ja korvaa ilmoituksen keskittimet rekisteröintikoodin, jotka olet lisännyt [aloittaminen ilmoituksen keskittimet] se seuraavalla koodilla:

        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();

            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }

    Näin varmistat, että aina, kun sovellus käynnistyy se hakee luokat paikallisesta tallennussijainnista ja pyytää rekisteröinti näiden luokkien.

2. MainPage.xaml.cs project-tiedostoon Lisää seuraava koodi, joka sisältää **OnNavigatedTo** -menetelmää:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
        }

    Tämä päivittää pääsivulta aiemmin tallennetut luokat tilan perusteella.

Sovellus on nyt valmis, ja voit tallentaa luokkien avulla voit rekisteröidä ilmoitus-toiminnossa aina, kun käyttäjä muuttaa valintaa luokat laitteen paikallisen muistiin. Olemme Määritä seuraavaksi taustatietokannan, voit lähettää ilmoituksen: sovellukseen luokan ilmoitukset.

##<a name="sending-tagged-notifications"></a>Merkitty ilmoitusten lähettäminen

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Suorita sovellus ja luo ilmoituksia

1. Visual Studiossa painamalla F5 Käännä ja Käynnistä sovellus.

    ![][1]

    Huomautus sovelluksen Käyttöliittymän on joukko, jonka avulla voit näyttää tai piilottaa Valitse tilata luokat.

2. Yhden tai useamman luokat vaihtaa ottaminen käyttöön ja valitse sitten **tilaa**.

    Sovelluksen muuntaa valittujen luokkien tunnisteet ja pyytää uusi laitteen rekisteröinti valitun tunnisteiden ilmoitus-toiminnosta. Rekisteröity luokat palautti ja valintaikkuna näkyvät.

    ![][2]

3. Saatuaan luokat on valmis tilauksen vahvistus suorittamalla console-sovelluksen kunkin luokkien ilmoitukset lähetetään. Tarkista, näyttöön tulee ilmoitus olet tilannut luokat.

    ![][3]

Olet suorittanut tämän ohjeaiheen.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Ilmoitus keskittimet käytön aloittaminen]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

