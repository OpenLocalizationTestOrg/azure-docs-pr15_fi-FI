<properties
    pageTitle="Ilmoitus keskittimet purkaa opetusohjelman - uutisia Android"
    description="Opettele käyttämään Azure palvelun Bus ilmoituksen keskittimet jakautumisen uutisia ilmoitusten lähettämisen Android-laitteisiin."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>


# <a name="use-notification-hubs-to-send-breaking-news"></a>Ilmoitus keskittimet avulla voit lähettää uutisia

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa esitellään käyttämisestä Azure ilmoituksen keskittimet yleislähetys jakautumisen uutisia Android-sovelluksen ilmoitukset. Kun olet valmis, jota voi rekisteröidä uudet luokat kiinnostavan purkaa, ja vain kyseisten luokkien push-ilmoitusten vastaanottaminen. Tämä skenaario on useita sovelluksia yleistä mallia, joiden ilmoitukset lähetetään käyttäjäryhmille, joita on aiemmin määritetty korko niitä, kuten RSS lukijan, sovellusten musiikkia tuulettimista jne.

Lähetyksen tilanteita, joissa on otettu sisällyttämällä vähintään yksi _tunnisteita_ luotaessa rekisteröinti ilmoitus-toiminnossa. Ilmoitukset lähetetään tunnisteen, kun kaikki laitteet, joissa olet rekisteröinyt tunnisteen saa ilmoituksen. Koska tunnisteet yksinkertaisesti merkkijonoja, niitä ei tarvitse valmisteltu etukäteen. Lisätietoja tunnisteet lisätietoja [ilmoituksen keskittimet reititys ja tunnisteen lausekkeita](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Edellytykset

Tässä ohjeaiheessa muodostaa luomasi [ilmoituksen keskittimet käytön aloittaminen]-sovellukseen[get-started]. Ennen kuin aloitat Tässä opetusohjelmassa, olet jo tehnyt [ilmoituksen keskittimet käytön aloittaminen][get-started].

##<a name="add-category-selection-to-the-app"></a>Luokka-valinta lisääminen sovellukseen

Ensimmäiseksi on Käyttöliittymän osat lisääminen oman aiemmin pääasiallista, joiden avulla käyttäjä voi valita rekisteröidä luokkia. Laitteessa on tallennettu käyttäjän valittuun luokkaan. Kun sovellus käynnistyy-laitteen rekisteröinti luodaan ilmoitus-toiminnossa valitun luokille tunnisteina.

1. Avaa res/layout/activity_main.xml-tiedosto ja korvaa sisällön seuraavasti:

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">

                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>

2. Avaa res/values/strings.xml-tiedosto ja lisää seuraavista riveistä:

        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>

    Graafisen asettelun main_activity.xml pitäisi näyttää tältä:

    ![][A1]

3. Luo luokan **ilmoitukset** nyt samaan pakettiin kuin **MainActivity** -luokka.

        import java.util.HashSet;
        import java.util.Set;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;

        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;

            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
        
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }

            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }

            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }

            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
        
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
        
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
        
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }

        }

    Tähän luokkaan käyttää paikallisen säilön uutisia, laite on vastaanottamaan luokkiin. Se sisältää myös menetelmiä Rekisteröidy näitä luokkia.


4. **MainActivity** luokan Poista **NotificationHub** ja **GoogleCloudMessaging**private-kentät ja kentän lisääminen **ilmoitukset**:

        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;

5. Poista sitten **onCreate** -menetelmä alustus **keskittimeen** -kentän ja **registerWithNotificationHubs** -menetelmää. Lisää seuraavat rivit, joka alustaa **ilmoitukset** -luokan esiintymän. 


        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;
    
            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);
    
            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);
    
            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`ja `HubListenConnectionString` jo on asetettava kanssa `<hub name>` ja `<connection string with listen access>` paikkamerkit ilmoitus-toiminnossa nimesi ja *DefaultListenSharedAccessSignature* , jotka olet hankkinut aiemmin yhteysmerkkijonon kanssa.

    > [AZURE.NOTE] Koska tunnistetiedot, jotka asiakas-sovelluksen jaetaan ei yleensä suojatun, kuunnella access näppäintä pitäisi jakaa vain asiakas-sovelluksessa. Kuuntele access ottaa käyttöön sovelluksen Rekisteröidy ilmoitukset, mutta aiemmin merkintöjä ei voi muokata ja et voi lähettää ilmoitukset. Ilmoitusten lähettäminen ja muuttamalla aiemmin merkintöjen suojatun Taustajärjestelmä palvelun käytetään täydet oikeudet-näppäintä.


6. Lisää sitten seuraavat tuonti ja `subscribe` tapa käsitellä tilaa-painiketta, valitse tapahtuma:
        
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;

        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");

            notifications.storeCategoriesAndSubscribe(categories);
        }

    Tämä menetelmä luo luokkaluettelo ja käyttää **ilmoitukset** -luokan tallentamista paikalliseen muistiin ja rekisteröidä vastaavan tunnisteet ilmoitus-toiminnossa. Muuttuessa luokat rekisteröinnin luodaan uusi luokille.

Sovellus on nyt voi tallentaa luokkien laitteeseen paikallisen tallennustilan ja voit rekisteröidä ilmoitus-toiminnossa aina, kun käyttäjä muuttaa valintaa luokat.

##<a name="register-for-notifications"></a>Rekisteröidy ilmoitukset

Näin voit rekisteröidä käynnistyksen avulla luokat, jotka on tallennettu paikallisen tallennustilaan ilmoitus-toiminnossa.

> [AZURE.NOTE] Koska määritetty mukaan Google Cloud Messaging (GCM) registrationId voi muuttaa milloin tahansa, Rekisteröi ilmoitusten usein tapahtuvien virheiden ilmoitus. Tässä esimerkissä Rekisteröi joka kerta, kun sovellus käynnistyy-ilmoituksen. Sovellusten, jotka suoritetaan usein useammin kuin kerran päivässä, voit todennäköisesti ohittaa rekisteröinti Jos edellisen rekisteröinti on kulunut alle yhden päivän kaistanleveyden säilyttää.


1. Lisää seuraava koodi **MainActivity** luokan **onCreate** menetelmää lopussa:

        notifications.subscribeToCategories(notifications.retrieveCategories());

    Näin varmistat, että aina, kun sovellus käynnistyy se hakee luokat paikallisesta tallennussijainnista ja pyytää näiden luokkien registeration. 

2. Päivitä `onStart()` menetelmää `MainActivity` luokan seuraavasti:

    @Overridesuojattu void onStart() {super.onStart();      isVisible = tosi.

        Set<String> categories = notifications.retrieveCategories();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }

    Tärkeimmät tehtävän tilan aiemmin tallennetut luokkien perusteella päivittyy.

Sovellus on nyt valmis, ja voit tallentaa luokkien avulla voit rekisteröidä ilmoitus-toiminnossa aina, kun käyttäjä muuttaa valintaa luokat laitteen paikallisen muistiin. Olemme Määritä seuraavaksi taustatietokannan, voit lähettää ilmoituksen: sovellukseen luokan ilmoitukset.

##<a name="sending-tagged-notifications"></a>Merkitty ilmoitusten lähettäminen

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Suorita sovellus ja luo ilmoituksia

1. Android Studiossa muodosta sovellus ja Käynnistä laite tai emulaattorin.

    Huomautus sovelluksen Käyttöliittymän on joukko, jonka avulla voit näyttää tai piilottaa Valitse tilata luokat.

2. Yhden tai useamman luokat vaihtaa ottaminen käyttöön ja valitse sitten **tilaa**.

    Sovelluksen muuntaa valittujen luokkien tunnisteet ja pyytää uusi laitteen rekisteröinti valitun tunnisteiden ilmoitus-toiminnosta. Rekisteröity luokat palautti ja saapuvan ilmoitus näytetään.

4. Lähetä uusi ilmoitus mukaan .NET Console-sovellusta.  Vaihtoehtoisesti voit lähettää merkittynä mallien ilmoitukset [Azure perinteinen Portal]-ilmoitus-toiminnossa virheenkorjaus-välilehden avulla.

    Valittujen luokkien ilmoitukset näkyvät ilmoitukseen ilmoitukset.

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa on oppinut yleislähetys uutisia luokittain. Harkitse Viimeistellään jompikumpi seuraavista opetusohjelmat, jotka korostavat muiden ilmoituksen keskittimet tilanteita:

+ [Lähetä lokalisoitu uutisia ilmoituksen keskittimet avulla]

    Opettele Laajenna jakautumisen uutiset sovelluksen käyttöön lähettämisen lokalisoitu ilmoitukset.





<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Lähetä lokalisoitu uutisia ilmoituksen keskittimet avulla]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure perinteinen Portal]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
