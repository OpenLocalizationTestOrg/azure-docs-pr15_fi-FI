<properties
    pageTitle="Azure Mobile välitys Android SDK-integrointi"
    description="Uusimmat päivitykset ja Azure Mobile välitys Android SDK ohjeet"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-android"></a>Miten voit integroida välitys saatavilla Android-laitteeseen

> [AZURE.IMPORTANT] Sinun on noudatettava integrointi kuvatulla tavalla miten voit integroida välitys Android asiakirjan ennen seuraavan tässä oppaassa.

##<a name="standard-integration"></a>Vakio-integrointi

Yhteys-SDK edellyttää **Android tuki kirjaston (v4)**.

Nopein tapa lisätä kirjaston **Pimennys** projektin on `Right click on your project -> Android Tools -> Add Support Library...`.

Jos et käytä Pimennys, voit lukea ohjeet [tähän].

Kopioi Reach resurssitiedostojen projektin SDK:

-   Kopioi tiedostot kohteesta `res/layout` SDK mukana toimitettu kansion `res/layout` sovelluksen-kansioon.
-   Kopioi tiedostot kohteesta `res/drawable` SDK mukana toimitettu kansion `res/drawable` sovelluksen-kansioon.

Muokkaa yhteyttä `AndroidManifest.xml` tiedosto:

-   Lisää seuraavan osan (välillä `<application>` ja `</application>` tunnisteet):

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
              <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
              </intent-filter>
            </receiver>

-   Tarvitset nämä oikeudet toista järjestelmäilmoituksia, joita ei ole napsautettu käynnistyksen (muuten ne säilyvät levyllä, mutta eivät enää näy, sinulla on todella haluat sisällyttää tämän).

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

-   Määritä ilmoitukset (molemmat sovelluksen ja järjestelmän niistä) kopioiminen ja muokkaamalla seuraavassa osassa käytettävää kuvaketta (välillä `<application>` ja `</application>` tunnisteet):

            <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [AZURE.IMPORTANT] Tässä osassa on **pakollinen** , jos aiot käyttää järjestelmäilmoituksia, kun luot Reach kampanjat. Android estää järjestelmäilmoituksia ilman kuvakkeet näytetä. Niin Jos sisältö, käyttäjiesi voi vastaanottaa niitä.

-   Jos luot Kampanjat järjestelmäilmoituksia ylemmän tason kanssa, sinun on lisättävä seuraavat oikeudet (kun `</application>` tunniste) Jos puuttuu:

            <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
            <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

  -   Android M ja jos sovelluksesi kohteena Android API tason 23 tai suurempi ``WRITE_EXTERNAL_STORAGE`` käyttöoikeus edellyttää käyttäjän hyväksyntää. Lue [Tämä osa](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

-   Järjestelmän ilmoitusten voit myös määrittää Reach markkinointikampanjan Jos laite on soi ja/tai värinätoiminnot. Sen toimimaan edellyttää Varmista, että seuraavat oikeudet tietueeksi (kun `</application>` tunniste):

            <uses-permission android:name="android.permission.VIBRATE" />

    Nämä oikeudet ilman Android estää seuraavilta järjestelmäilmoitukset näkyvät, jos valittuna soi tai saavuttaa markkinointikampanjan hallintaan vibrate-vaihtoehto.

-   Jos luominen käyttämällä **ProGuard** sovelluksen ja on Android tuki-kirjaston tai välitys purkkiin liittyvät virheet, Lisää seuraavat rivit oman `proguard.cfg` tiedosto:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

## <a name="native-push"></a>Alkuperäisen Push

Nyt kun olet määrittänyt Reach moduuli, sinun täytyy määrittää alkuperäisen push voivat tulla Kampanjat laitteeseen.

Sovellus tukee kahta palvelua Android:

  - Google Play-laitteet: Käytä [Google Cloud Messaging] noudattamalla [Voit integroida GCM välitys apuviivaan](mobile-engagement-android-gcm-integrate.md) opas.
  - Amazon laitteet: Käytä [Amazon laitteen Messaging] noudattamalla [Voit integroida ADM välitys apuviivaan](mobile-engagement-android-adm-integrate.md) opas.

Jos haluat kohdistaa Amazon ja Google Play-laitteet, sen mahdollista, jos haluat, että kaikki 1 AndroidManifest.xml/APK kehittämiseen sisällä. Mutta lähetettäessä Amazon ne voivat hylkää sovelluksesi, jos ne löytävät GCM koodi.

Tässä tapauksessa kannattaa käyttää useita APKs.

**Sovellus on nyt valmis ja näyttämiseen reach kampanjat!**

##<a name="how-to-handle-data-push"></a>Tietoja push käsittelemisestä

### <a name="integration"></a>Integrointi

Jos haluat, että sovelluksesi voi vastaanottaa Reach tietojen työntää, sinun on luotava aliluokka `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` ja sen viittaus `AndroidManifest.xml` tiedosto (välillä `<application>` ja/tai `</application>` tunnisteet):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Sitten voit ohittaa `onDataPushStringReceived` ja `onDataPushBase64Received` takaisinkutsuja. Tässä on esimerkki:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Luokka

Luokka-parametri on valinnainen, kun luot tietojen Push markkinointikampanjan ja avulla voit suodattaa tietoja Vie. Tämä on kätevä, jos sinulla on useita erityyppisiä tietoja työntää käsittely lähetyksen vastaanottajia tai jos haluat push eri, `Base64` tiedot ja haluat niiden tyypin selvittäminen ennen jäsennyksen ne.

### <a name="callbacks-return-parameter"></a>Palauta parametria takaisinkutsuja'

Seuraavassa on joitakin ohjeita, käsittele oikein, palauta parametria `onDataPushStringReceived` ja `onDataPushBase64Received`:

-   Lähetyksen vastaanottaja tulee palauttaa `null` takaisinkutsussa, jos se ei tiedä, miten voit käsitellä tietoja push. Luokan avulla määrittää, onko lähetetyn vastaanotin käsitellä tietoja push vai ei.
-   Yksi lähetyksen vastaanottaja tulee palauttaa `true` takaisinkutsussa, jos se hyväksyy tietojen push.
-   Yksi lähetyksen vastaanottaja tulee palauttaa `false` takaisinkutsussa, jos se tunnistaa tietojen push, mutta hylkää jostakin syystä. Esimerkiksi palauttaa `false` kun vastaanotettu tiedot ovat virheellisiä.
-   Jos jokin broadcast vastaanotin palauttaa `true` aikana toiseen jokin palauttaa `false` saman tietojen push-ongelma ei ole määritetty, ei koskaan muuta se.

Palautustyyppi käytetään vain Reach tilastotiedot:

-   `Replied`suurenee, jos jokin lähetyksen vastaanottajia joko `true` tai `false`.
-   `Actioned`arvo kasvaa vain, jos jokin lähetyksen vastaanottajia palautti `true`.

##<a name="how-to-customize-campaigns"></a>Kampanjat mukauttamisesta

Jos haluat mukauttaa kampanjat, voit muokata saavuttaa SDK annetuista kaavoista.

Tulisi säilyttää kaikki asettelut käytetyt tunnukset ja pidä Näkymätyypit, jotka käyttävät tunniste, erityisesti tekstiä ja kuva näkymiä. Joissakin näkymissä käytetään vain piilottaminen tai näyttäminen alueet, jotta tyypin voi muuttaa. Tarkista lähdekoodi, jos haluat muuttaa näkymän annettu asetteluissa.

### <a name="notifications"></a>Ilmoitukset

Ilmoitusten kahdenlaisia: Järjestelmä- ja sovelluskohtaisesta ilmoitukset, joiden asettelua tiedostojen käyttäminen.

#### <a name="system-notifications"></a>Järjestelmäilmoituksia

Voit mukauttaa järjestelmäilmoituksia haluat **Luokat**. Voit siirtyä [Luokat](#categories).

#### <a name="in-app-notifications"></a>Sovellusten ilmoitukset

Oletusarvon mukaan-sovelluksen ilmoituksella on näkymä, joka lisätään dynaamisesti nykyisen tehtävän käyttöliittymän ansiosta Android menetelmä `addContentView()`. Tätä kutsutaan ilmoituksen päällekkäin. Ilmoitus päällekkäiset soveltuvat erinomaisesti nopea integrointi, koska ne eivät edellytä voit muokata mitä tahansa asettelua sovelluksessa.

Voit muokata ilmoituksen päällekkäiset ulkoasua, voit yksinkertaisesti muokata tiedostoa `engagement_notification_area.xml` tarpeitasi.

> [AZURE.NOTE] Tiedoston `engagement_notification_overlay.xml` on se, jota käytetään ilmoitus-kerros sisältää tiedoston `engagement_notification_area.xml`. Voit myös mukauttaa sen tarpeisiisi (kuten sijoittelu ilmaisinalueelle sisällä kerros).

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Sisällytä ilmoituksen asettelun osana tehtävän-asettelu

Päällekkäiset soveltuvat erinomaisesti nopea integrointi mutta voidaan tällainen tai tietyissä tapauksissa laidassa tehosteet eivät ole. Kerros-järjestelmän voi mukauttaa tehtävän tasolla henkilöistä ja estää erityinen toiminta puoli tehosteet.

Voit päättää, Sisällytä Microsoftin ilmoitusta asettelu **sisältää** Android-lauseen alkuun aiemmin rakenne. Seuraavassa on esimerkki muokattu `ListActivity` asettelun sisältävä vain `ListView`.

**Ennen välitys integrointia:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Kun olet välitys integrointi:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

Tässä esimerkissä on lisätty ylemmän tason säilön jälkeen alkuperäisessä asettelussa käytetään luettelonäkymän ylimmän tason elementti. Myös lisätä `android:layout_weight="1"` voivat näkymän alla luettelonäkymän, jolle on määritetty `android:layout_height="fill_parent"`.

Välitys saavuttaa SDK tunnistaa automaattisesti ilmoituksen asettelun sisältyy tässä aktiviteetti ja ei lisää kyselynopeustiedot tämän toiminnon.

> [AZURE.TIP] Jos käytät ListActivity sovelluksessa, näkyvissä Reach kerros saattavat estää reagoi napsautettaessa luettelonäkymän kohteiden enää. Tämä on tunnettu ongelma. Tämä ongelma voidaan ratkaista Suosittelemme voit upottaa ilmoituksen asettelun oman luettelon tehtävän asettelu, kuten edellisessä esimerkissä.

##### <a name="disabling-application-notification-per-activity"></a>Sovelluksen ilmoituksen aktiviteettia kohden poistaminen käytöstä

Jos et halua näkyvän toimiessasi kerros ja jos ilmoituksen asettelun ei sisällyttää uuden asettelun, voit poistaa tämän toiminnon, kerros `AndroidManifest.xml` lisäämällä `meta-data` osa, kuten seuraavassa esimerkissä:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Luokat

Kun muokkaat annettu asetteluja, voit muokata kaikki ilmoitukset ulkoasua. Luokkien avulla voit määrittää ilmoitusten eri kohdennettujen näyttää (mahdollisesti toiminnan). Luokan voidaan määrittää Reach markkinointikampanjan luodessasi. Ota huomioon, että luokat mahdollistavat myös ilmoitukset ja kyselyiden, mukauttaminen, joka on kuvattu jäljempänä tässä asiakirjassa.

Jos haluat rekisteröidä luokan käsittelijä ilmoituksissa, Lisää kutsu, kun sovellus alustaminen.

> [AZURE.IMPORTANT] Lue android: prosessi-määritteen varoittaa \<android-sdk-välitys-prosessin\> Valitse miten haluat integroida välitys Android aiheeseen, ennen kuin jatkat.

Seuraavassa esimerkissä oletetaan kuitata edellisen varoitus ja käyttää aliluokka `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

`MyNotifier` Objekti on ilmoitus luokan käsittelijä soveltaminen. Se on joko `EngagementNotifier` käyttöliittymän tai sub-luokan niiden oletusarvoisen: `EngagementDefaultNotifier`.

Huomaa, että sama ilmoitus voi käsitellä useita luokkia, voit rekisteröidä tältä:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Korvaa oletusarvoisen luokan käyttöönoton, voit rekisteröidä käyttöympäristösi, kuten seuraavassa esimerkissä:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

Nykyisen luokan käytetään käsittelytoiminto välitetään parametrina useimmat menetelmiä, voit ohittaa- `EngagementDefaultNotifier`.

Välitetään joko `String` parametrin tai epäsuorasti `EngagementReachContent` objekti, joka on `getCategory()` menetelmää.

Voit muuttaa ilmoituksen luontiprosessi useimmat menetelmiä uudelleen käyttöön `EngagementDefaultNotifier`, entistä enemmän vapaasti Tutustu teknisten ja lähdekoodin.

##### <a name="in-app-notifications"></a>Sovellusten ilmoitukset

Jos haluat käyttää muita asetteluja tietyssä luokassa, voit ottaa tämän seuraavan esimerkin mukaisesti:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Esimerkki `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Kuten näet, kerros näkymän tunnus on eri kuin vakio. On tärkeää, että jokaista asettelua käyttää yksilöllinen päällekkäiset.

**Esimerkki `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Kuten näet, ilmoitus alueen Näytä tunniste on eri kuin vakio. On tärkeää, että kunkin asettelussa käytetään yksilöllinen ilmoituksen aihealueiden.

Tässä yksikertaisessa esimerkissä luokan tekee sovelluksen (tai sovelluskohtaisesta) ilmoitukset näkyy näytön ylälaidassa. Olemme muuttanut käytetään ilmaisinalueella itse vakio tunnukset.

Jos haluat muuttaa, sinun on uudelleen `EngagementDefaultNotifier.prepareInAppArea` menetelmää. On suositeltavaa tarkistettava teknisten ja lähdekoodia `EngagementNotifier` ja `EngagementDefaultNotifier` Jos haluat tämän tason enemmän.

##### <a name="system-notifications"></a>Järjestelmäilmoituksia

Pidentämällä `EngagementDefaultNotifier`, voit ohittaa `onNotificationPrepared` muuttaa ilmoitus, joka on valmistanut oletusarvo-käyttöönotto.

Esimerkki:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

Tässä esimerkissä on järjestelmän ilmoituksen sisältöä, näytetty jatkuvaa tapahtumana "jatkuvaa" luokan käytettäessä.

Jos haluat luoda `Notification` objektin alusta, voit palauttaa `false` menetelmä ja puhelun `notify` itse käyttöön `NotificationManager`. Tässä tapauksessa on tärkeää, että pidät `contentIntent`, `deleteIntent` ja ilmoitus-tunnus, jota `EngagementReachReceiver`.

Seuraavassa on esimerkiksi toteutusta oikea Esimerkki:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Ilmoitus vain ilmoitukset

Valitse vain ilmoitus voi mukauttaa ohittaminen ilmoituksen hallinta `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` muokata valmisteltu `Intent`. Tällä menetelmällä voit hienosäätää liput helposti.

Voit lisätä esimerkiksi `SINGLE_TOP` merkintä:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

Aiempien versioiden välitys käyttäjille Huomaa, että järjestelmäilmoituksia ilman toimintoa URL-Osoitteen nyt käynnistää sovelluksen jos se on avattuna, jotta tätä menetelmää voit kutsuttava ilmoituksen ilman toiminnon URL-osoite. Sinun kannattaa harkita, kun mukautat käyttötarkoitukseen.

Voit myös ottaa käyttöön `EngagementNotifier.executeNotifAnnouncementAction` alusta alkaen.

##### <a name="notification-life-cycle"></a>Ilmoitus elinkaari

Oletus-luokan käytettäessä jotkin elinkaaren menetelmät kutsutaan `EngagementReachInteractiveContent` objektin raportin tilastoja ja Päivitä markkinointikampanjan tila:

-   Kun ilmoitus näkyy sovelluksen tai sijoittaa tilarivillä `displayNotification` menetelmää kutsutaan (joka ilmoittaa tilastotiedot) mukaan `EngagementReachAgent` Jos `handleNotification` palauttaa `true`.
-   Jos ilmoituksessa, joka on piilotettu, `exitNotification` menetelmää kutsutaan, Tilasto ilmoitetaan ja seuraavan Kampanjat nyt voidaan käsitellä.
-   Jos ilmoituksen napsautetaan, `actionNotification` on nimeltä tilasto ilmoitetaan ja liittyvän palveluita käynnistyy.

Jos käyttöönoton `EngagementNotifier` ohittaa oletustoiminnon, tarvitsemat elinkaaren näistä tavoista itse. Seuraavissa esimerkeissä kuvataan joissakin tapauksissa, jossa oletustoiminnan on ohitettu:

-   Älä laajentaa `EngagementDefaultNotifier`, kuten luokan käsittely alusta alkaen on käytössä.
-   Järjestelmän ilmoituksissa, overrode `onNotificationPrepared` ja muokkaamasi `contentIntent` tai `deleteIntent` - `Notification` objekti.
-   Sovelluksen-ilmoituksissa, overrode `prepareInAppArea`, varmista, että voit yhdistää vähintään `actionNotification` johonkin oman U.I ohjausobjektit.

> [AZURE.NOTE] Jos `handleNotification` aiheuttaa poikkeuksen sisältö poistetaan ja `dropContent` kutsutaan. Tämä on näkyvissä tilastoja ja seuraavan Kampanjat nyt voidaan käsitellä.

### <a name="announcements-and-polls"></a>Ilmoitukset ja kyselyiden

#### <a name="layouts"></a>Asettelut

Voit muokata `engagement_text_announcement.xml`, `engagement_web_announcement.xml` ja `engagement_poll.xml` tiedostojen tekstin ilmoitukset, web-ilmoitukset ja kyselyiden mukauttamiseen.

Nämä tiedostot jakaa kaksi yleisiä rakennetta otsikkoalue ja painikkeiden päälle. Asettelu-otsikko on `engagement_content_title.xml` ja käyttää eponymous drawable tiedoston taustan. Asettelu-toiminto ja Lopeta-painikkeet on `engagement_button_bar.xml` ja käyttää eponymous drawable tiedoston taustan.

Kyselyn, valitse kysymys-asettelun ja niiden valinnat ovat dynaamisesti sellainen käyttämällä useita kertoja `engagement_question.xml` asettelun tiedoston kysymysten ja `engagement_choice.xml` vaihtoehtojen tiedosto.

#### <a name="categories"></a>Luokat

##### <a name="alternate-layouts"></a>Vaihtoehtoinen asettelut

Ilmoitukset, kuten markkinointikampanja luokan voidaan on vaihtoehtoinen asettelut ilmoitukset ja kyselyjä.

Esimerkiksi luominen tekstin ilmoitus luokka, voit laajentaa `EngagementTextAnnouncementActivity` ja se viittaa `AndroidManifest.xml` tiedosto:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Huomaa, että käyttötarkoitukseen luokassa suodattaa käytetään ilmoitus oletustoiminto ero.

Saavuttaa SDK käyttää käyttötarkoitusta järjestelmän ratkaisemiseksi oikean tehtävän tietyssä luokassa, ja se kuuluu takaisin oletusarvo-luokan Jos ratkaisu epäonnistui.

Valitse toteuttamisesta on `MyCustomTextAnnouncementActivity`, jos haluat vain asettelun muuttaminen (mutta säilyttää samassa näkymässä tunnukset), sinulla on Määritä luokka, kuten seuraavassa esimerkissä:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Jos haluat korvata tekstin ilmoitukset oletusarvo-luokka, riittää, että korvaa `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` käyttöympäristösi mukaan.

Web-ilmoitukset ja kyselyiden voi mukauttaa samalla tavalla.

Web-ilmoitukset voit laajentaa `EngagementWebAnnouncementActivity` ja määritellä toimiessasi `AndroidManifest.xml` , kuten seuraavassa esimerkissä:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

Voit laajentaa kyselyiden `EngagementPollActivity` ja määritellä yhteyttä- `AndroidManifest.xml` , kuten seuraavassa esimerkissä:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Toteutuksen jälkeisen alusta alkaen

Voit ottaa luokat ilmoitus (ja kysely) toimintojen ilman laajentaminen jokin `Engagement*Activity` saavuttaa SDK myöntämä luokat. Tästä on hyötyä esimerkiksi jos haluat määrittää asettelu, joka ei käytä samaa näkymien vakio asetteluja.

Kuten kehittyneet ilmoituksen mukauttaminen, on suositeltavaa tarkasteltavan vakio käyttöönoton lähdekoodia.

Seuraavat seikat kannattaa pitää mielessä: Reach käynnistää tietyn palveluita, (koskien käyttötarkoitusta suodatin) tehtävää plus ylimääräinen parametri, joka on sen sisällön tunnus.

Noutaa sisällön objekti, joka sisältää kentät, markkinointikampanja sivustossa luotaessa määritetty voit tehdä tämän:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

Tilastotietoja, tulee raportoida sisältö näkyy `onResume` tapahtuman:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Älä unohda sitten Soita joko `actionContent(this)` tai `exitContent(this)` sisällön objektin ennen tehtävän siirtyy tausta.

Jos et Soita joko `actionContent` tai `exitContent`, tilastotiedot ei lähetetä (eli ei analytics-markkinointikampanja) ja lisäksi seuraava Kampanjat ei lähetetä ennen hakeminen uudelleenkäynnistämistä.

Suuntaa tai muita määritysmuutoksia voi tehdä koodin vaikeaa määrittääksesi, onko tehtävä siirtyy taustan tai vakio toteutusta varmistetaan, että sisältö on valmiiksi poistunut, jos käyttäjä jättää tehtävän (joko painamalla `HOME` tai `BACK`), mutta ei, jos suunta muuttuu.

Näin käyttöönoton kiinnostava osa:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Kuten näet, jos soitat `actionContent(this)` sitten -tehtävän valmiiksi `exitContent(this)` voidaan turvallisesti kutsua ilman, että vaikutusta.

[Täällä]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud viestintä]:http://developer.android.com/guide/google/gcm/index.html
[Amazon laitteen Messaging]:https://developer.amazon.com/sdk/adm.html
