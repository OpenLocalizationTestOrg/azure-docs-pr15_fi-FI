<properties
    pageTitle="Ilmoitus keskittimet avulla voit lähettää uutisia (Windows yleinen)"
    description="Käytä Azure ilmoituksen lihavoituna rekisteröinnin tunnisteet uutisia lähettäminen yleinen Windows-sovellus."
    services="notification-hubs"
    documentationCenter="windows"
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

# <a name="use-notification-hubs-to-send-breaking-news"></a>Ilmoitus keskittimet avulla voit lähettää uutisia


[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa esitellään käyttämisestä Azure ilmoituksen keskittimet yleislähetys jakautumisen uutisia ilmoitukset Windows-kaupan tai Windows Phone 8.1 (-Silverlight)-sovelluksen. Jos kohdennat Windows Phone 8.1 Silverlight, tutustu [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) -versio. Kun olet valmis, jota voi rekisteröidä uudet luokat kiinnostavan purkaa, ja vain kyseisten luokkien push-ilmoitusten vastaanottaminen. Tässä skenaariossa on useita sovelluksia yleistä mallia kohtaa, johon ilmoitukset lähetetään käyttäjäryhmille, jotka on aiemmin määritetty korko niitä, kuten RSS reader musiikkia tuulettimista sovellukset ja niin edelleen. 

Lähetyksen tilanteita, joissa on otettu sisällyttämällä vähintään yksi _tunnisteita_ luotaessa rekisteröinti ilmoitus-toiminnossa. Ilmoitukset lähetetään tunnisteen, kun kaikki laitteet, joissa olet rekisteröinyt tunnisteen saa ilmoituksen. Koska tunnisteet yksinkertaisesti merkkijonoja, niitä ei tarvitse valmisteltu etukäteen. Lisätietoja tunnisteet lisätietoja [ilmoituksen keskittimet reititys ja tunnisteen lausekkeita](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Edellytykset

Tässä ohjeaiheessa muodostaa luomasi [ilmoituksen keskittimet käytön aloittaminen]-sovellukseen[get-started]. Ennen kuin aloitat Tässä opetusohjelmassa, olet jo tehnyt [ilmoituksen keskittimet käytön aloittaminen][get-started].

##<a name="add-category-selection-to-the-app"></a>Luokka-valinta lisääminen sovellukseen

Ensimmäiseksi on Käyttöliittymän osat lisääminen aiemmin luotu pää-sivu, joiden avulla käyttäjä voi valita rekisteröidä luokkia. Laitteessa on tallennettu käyttäjän valittuun luokkaan. Kun sovellus käynnistyy-laitteen rekisteröinti luodaan ilmoitus-toiminnossa valitun luokille tunnisteina.

1. Avaa MainPage.xaml project-tiedosto ja valitse Kopioi seuraava koodi **Ruudukko** -osaan:

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>


2. Hiiren kakkospainikkeella **jaettu** -projekti ja lisätä uuden luokan nimi **ilmoitukset**, Lisää **julkisen** määrite luokan määritys ja lisää sitten seuraavat **käyttämällä** lauseet tiedostoon koodi:

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Kopioi seuraava koodi uusi **ilmoitukset** -luokka.

        private NotificationHub hub;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }

        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }

    Tähän luokkaan käyttää paikallisen säilön uutisia, laite on vastaanottamaan luokkiin. Huomaa, että sen sijaan, että *RegisterNativeAsync* kutsumista olemme Soita *RegisterTemplateAsync* Rekisteröi käyttäen mallia rekisteröiminen luokkien. 
    
    Annamme myös nimi mallin ("simpleWNSTemplateExample"), koska haluamme voi rekisteröidä useita malleja (esimerkiksi yksi ilmoitukseen ilmoituksia ja) yksi osalta ja annettava nimeät ne haluat, että voit päivittää tai poistaa niitä.

    Huomaa, että jos laitteen Rekisteröi useita malleja samaa tunnistetta, saapuvan viestin kohdistamisen, että tunnisteen johtaa useita ilmoituksia toimitettu laitteen (yksi kullekin mallille). Tämä ongelma on hyötyä, kun samaa looginen viestiin on useita visuaaliset ilmoitukset, näkyvät esimerkiksi Windows-kaupan sovelluksen merkin luominen ja ilmoitukseen johtaa.

    Saat lisätietoja mallien [malleja](notification-hubs-templates-cross-platform-push-messages.md).




4. App.xaml.cs project-tiedostoon Lisää seuraava ominaisuus **App** luokan:

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    Tätä ominaisuutta käytetään voivat luoda ja käyttää **ilmoitukset** -esiintymä.

    Korvaa yllä koodi `<hub name>` ja `<connection string with listen access>` paikkamerkit, joissa ilmoitus-toiminnossa nimesi ja *DefaultListenSharedAccessSignature* , jotka olet hankkinut aiemmin yhteysmerkkijono.

    > [AZURE.NOTE] Koska tunnistetiedot, jotka asiakas-sovelluksen jaetaan ei yleensä suojatun, kuunnella access näppäintä pitäisi jakaa vain asiakas-sovelluksessa. Kuuntele access ottaa käyttöön sovelluksen Rekisteröidy ilmoitukset, mutta aiemmin merkintöjä ei voi muokata ja et voi lähettää ilmoitukset. Ilmoitusten lähettäminen ja muuttamalla aiemmin merkintöjen suojatun Taustajärjestelmä palvelun käytetään täydet oikeudet-näppäintä.

5. Lisää seuraava rivi oman MainPage.xaml.cs:

        using Windows.UI.Popups;

6. Lisää MainPage.xaml.cs project-tiedostoon seuraavasti:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }

    Tämä menetelmä luo luokkaluettelo ja käyttää **ilmoitukset** -luokan tallentamista paikalliseen muistiin ja rekisteröidä vastaavan tunnisteet ilmoitus-toiminnossa. Muuttuessa luokat rekisteröinnin luodaan uusi luokille.

Sovellus on nyt voi tallentaa luokkien laitteeseen paikallisen tallennustilan ja voit rekisteröidä ilmoitus-toiminnossa aina, kun käyttäjä muuttaa valintaa luokat.

##<a name="register-for-notifications"></a>Rekisteröidy ilmoitukset

Näin voit rekisteröidä käynnistyksen avulla luokat, jotka on tallennettu paikallisen tallennustilaan ilmoitus-toiminnossa.

> [AZURE.NOTE] Koska kanavan URI määritetty Windowsin ilmoituksen Service (WNS) avulla voit muuttaa milloin tahansa, Rekisteröi ilmoitusten usein tapahtuvien virheiden ilmoitus. Tässä esimerkissä Rekisteröi joka kerta, kun sovellus käynnistyy-ilmoituksen. Sovellusten, jotka suoritetaan usein useammin kuin kerran päivässä, voit todennäköisesti ohittaa rekisteröinti Jos edellisen rekisteröinti on kulunut alle yhden päivän kaistanleveyden säilyttää.

1. Avaa App.xaml.cs-tiedosto ja Päivitä **InitNotificationsAsync** menetelmä `notifications` luokkiin perustuvan luokan tilaa.

        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
    
        var result = await notifications.SubscribeToCategories();

    Näin varmistat, että aina, kun sovellus käynnistyy se hakee luokat paikallisesta tallennussijainnista ja pyytää näiden luokkien registeration. **InitNotificationsAsync** menetelmä on luotu osana [aloittaminen ilmoituksen keskittimet] [ get-started] opetusohjelma.

3. Lisää seuraava koodi MainPage.xaml.cs-projektitiedoston *OnNavigatedTo* tapaan:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
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

    ![][19]

4. Lähetä uusi ilmoitus taustasta jollakin seuraavista tavoista:

    + **Console-sovellus:** Käynnistä console-sovellus.

    + **Java/PHP:** sovelluksen tai komentosarjan suorittaminen.

    Valittujen luokkien ilmoitukset näkyvät ilmoitukseen ilmoitukset.

    ![][14]

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa on oppinut yleislähetys uutisia luokittain. Harkitse Viimeistellään jompikumpi seuraavista opetusohjelmat, jotka korostavat muiden ilmoituksen keskittimet tilanteita:

+ [Lähetä lokalisoitu uutisia ilmoituksen keskittimet avulla]

    Opettele Laajenna jakautumisen uutiset sovelluksen käyttöön lähettämisen lokalisoitu ilmoitukset.



<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Lähetä lokalisoitu uutisia ilmoituksen keskittimet avulla]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
