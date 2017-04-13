<properties
    pageTitle="Ilmoitus keskittimet lokalisoitu jakautumisen uutisia opetusohjelma"
    description="Opettele käyttämään Azure ilmoituksen keskittimet, lokalisoidut jakautumisen uutisia ilmoitukset lähetetään."
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

# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Ilmoitus keskittimet avulla voit lähettää lokalisoitu uutisia

> [AZURE.SELECTOR]
- [Windows-kaupan C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

##<a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa esitellään, miten broadcast jakautumisen uutisia ilmoituksia, jotka on lokalisoitu kieli-ja laitteen Azure ilmoituksen keskittimet **malli** -toiminnon avulla. Tässä opetusohjelmassa aloitetaan Windows-kaupan sovelluksen [Käytön ilmoituksen keskittimet lähettää uutisia]luotu. Kun olet valmis, osaat Rekisteröidy kiinnostavan luokat, Määritä kieli, jolla haluat vastaanottaa ilmoituksia ja vain valitun luokkien push-ilmoitusten vastaanottaminen kyseisellä kielellä.


Tässä skenaariossa kaksi osaa on:

- Windows-kaupan sovelluksen sallii laitteet kielen ja eri jakautumisen uudet luokat; tilaaminen

- sitten taustatietokantatiedosto lähettää käyttävät Azure ilmoituksen keskittimet **tunnisteen** ja **malli** -feautres ilmoitukset.



##<a name="prerequisites"></a>Edellytykset

Jos olet jo tehnyt [Käytä ilmoituksen keskittimet lähettää uutisia] opetusohjelman ja on käytettävissä, koodi, koska tässä opetusohjelmassa rakentuu suoraan koodia.

Tarvitset myös Visual Studio 2012 tai uudempi versio.


##<a name="template-concepts"></a>Mallin käsitteitä

[Käytä ilmoituksen keskittimet lähettää uutisia] olet luonut sovellus, jonka eri uudet luokat-ilmoitusten tilaaminen **tunnisteiden** avulla.
Useita sovelluksia on kuitenkin kohteen useita markkina-alueet ja edellyttävät lokalisoinnin. Tämä tarkoittaa, että ilmoitukset itse sisältö on lokalisoitu ja toimitetaan laitteiden oikea joukko.
Tässä ohjeaiheessa on näkyy ilmoitus keskittimet **malli** -toiminnon käyttämisestä aikana helposti lokalisoitu jakautumisen uutisia ilmoitukset.

Huomautus: lokalisoitu ilmoitukset lähetetään yksi tapa on luoda kutakin useita versioita. Esimerkiksi englanti, ranska ja Mandariini tukemaan annettava kolme eri tunnisteita, uutisia: "world_en", "world_fr" ja "world_ch". Olemme olisi maailmanlaajuisesti uutisryhmät lokalisoitu lähettäminen kunkin tunnisteet. Tässä ohjeaiheessa Käytämme malleja tunnisteita koskevia ja vaatimus useiden viestien lähettäminen välttämiseksi.

Korkean tason mallit ovat voi määrittää, miten tietyn laitteen saavat ilmoituksen. Mallin määrittää tarkat tiedot viittaamalla ominaisuudet, jotka ovat osa sovellus että taustatietokantaa lähettämä viesti. Tässä tapauksessa lähetämme kaikki tukemilla kielillä sisältävä alue-ympäristöstä riippumattomalla tavalla viesti:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Valitse Microsoft varmistat, että laitteet rekisteröidä malli, joka viittaa oikea-ominaisuuteen. Esimerkiksi Windows-kaupan sovellus, joka vastaanottaa yksinkertainen saapuvan viestin rekisteröidä seuraavat mallin vastaavan tunnisteita kanssa:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Mallit ovat hyvin tehokas ominaisuus, voit lukea lisää [malleja](notification-hubs-templates-cross-platform-push-messages.md) tämän artikkelin. 


##<a name="the-app-user-interface"></a>Sovelluksen käyttöliittymässä

On nyt muokata purkaa uutisia sovellus, jonka loit artikkelissa [Käytä ilmoituksen keskittimet lähettää uutisia] lähettää lokalisoitu mallien avulla uutisia.

Windows-kaupan-sovelluksessa:

Muuta oman MainPage.xaml sisällytettävien kielen yhdistelmäruutu:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

##<a name="building-the-windows-store-client-app"></a>Windows-kaupan asiakas-sovelluksen luominen

1. Lisää kieli-parametri ilmoitukset-luokka, *StoreCategoriesAndSubscribe* ja *SubscribeToCateories* menetelmiä.

        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }

        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }

    Huomaa, että sen sijaan, että *RegisterNativeAsync* kutsumista olemme Soita *RegisterTemplateAsync*: emme rekisteröityvät tietyn ilmoitus-muodossa, jossa malli määräytyy aluekohtaisia asetuksia. Annamme myös nimi mallin ("localizedWNSTemplateExample"), koska haluamme voi rekisteröidä useita malleja (esimerkiksi yksi ilmoitukseen ilmoituksia ja) yksi osalta ja annettava nimeät ne haluat, että voit päivittää tai poistaa niitä.

    Huomaa, että jos laitteen Rekisteröi useita malleja samaa tunnistetta, saapuvan viestin kohdistamisen, että tunnisteen johtaa useita ilmoituksia toimitettu laitteen (yksi kullekin mallille). Tämä ongelma on hyötyä, kun samaa looginen viestiin on useita visuaaliset ilmoitukset, näkyvät esimerkiksi Windows-kaupan sovelluksen merkin luominen ja ilmoitukseen johtaa.

2. Lisää hakemiseen tallennetut aluekohtaisia asetuksia seuraavasti:

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. Päivittää oman MainPage.xaml.cs painikkeen valitsemalla käsittelijä kielen yhdistelmäruudun nykyisen arvon hakeminen ja ilmoitukset-luokan puhelu antaminen esitetyllä:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;

            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);

            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }


4. Lopuksi App.xaml.cs tiedostossa, muista päivittää oman `InitNotificationsAsync` noutaa aluekohtaisia asetuksia ja käytä tätä, kun tilaat menetelmää:

        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }


##<a name="send-localized-notifications-from-your-back-end"></a>Lokalisoitu ilmoitusten lähettäminen Edellinen + end

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]






<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Ilmoitus keskittimet avulla voit lähettää uutisia]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
