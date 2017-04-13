1. **Sovelluksen** projektissasi, Avaa tiedosto `AndroidManifest.xml`. Seuraavat kaksi vaihetta koodia, korvaa _`**my_app_package**`_ projektin sovelluspaketin nimi, joka on arvo `package` -määrite `manifest` tunniste.

2. Lisää seuraavat uudet käyttöoikeudet, kun olet nykyisen `uses-permission` elementti:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Lisää seuraava koodi jälkeen `application` tunnisteen avaaminen:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                        android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>


4. Avaa tiedosto *ToDoActivity.java*ja lisää tuonti-lause:

        import com.microsoft.windowsazure.notifications.NotificationsManager;


5. Lisää seuraavan yksityinen muuttujan luokan: Korvaa _`<PROJECT_NUMBER>`_ on määritetty Google mukaan edellisen menettelyn sovelluksen projektinumero:

        public static final String SENDER_ID = "<PROJECT_NUMBER>";

6. Määritysten muuttaminen *MobileServiceClient* **yksityisestä** **julkinen staattinen**, jotta se näyttää nyt tältä:

        public static MobileServiceClient mClient;

7. Lisää uusi luokka käsittelee ilmoitusten annettava Seuraava. Avaa Project Explorer **src** => **tärkeimmät** => **java** solmujen ja hiiren kakkospainikkeen napsauttaminen paketin nimi-solmu: **Uusi**ja valitse sitten **Java-luokka**.

8. Kirjoita **nimi** `MyHandler`, valitse **OK**.


    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)


9. Korvaa MyHandler-tiedosto, luokkailmoitus

        public class MyHandler extends NotificationsHandler {


10. Lisää seuraavat Tuontilauseet, saat `MyHandler` luokan:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;


11. Lisää seuraavaksi, `MyHandler` luokan:

        public static final int NOTIFICATION_ID = 1;


12. Valitse `MyHandler` luokan, Lisää seuraava koodi korvaavat **onRegistered** -menetelmä, jota Rekisteröi laite mobiilipalvelun ilmoitus-toiminnossa.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
            super.onRegistered(context, gcmRegistrationId);

            new AsyncTask<Void, Void, Void>() {

                protected Void doInBackground(Void... params) {
                    try {
                        ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                        return null;
                    }
                    catch(Exception e) {
                        // handle error             
                    }
                    return null;            
                }
            }.execute();
        }


13. Valitse `MyHandler` luokan, Lisää seuraava koodi korvaavat **onReceive** menetelmä, joka aiheuttaa ilmoituksen tulee näkyviin, kun se vastaanotetaan.

        @Override
        public void onReceive(Context context, Bundle bundle) {
                String msg = bundle.getString("message");

                PendingIntent contentIntent = PendingIntent.getActivity(context,
                        0, // requestCode
                        new Intent(context, ToDoActivity.class),
                        0); // flags

                Notification notification = new NotificationCompat.Builder(context)
                        .setSmallIcon(R.drawable.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                        .setContentText(msg)
                        .setContentIntent(contentIntent)
                        .build();

                NotificationManager notificationManager = (NotificationManager)
                        context.getSystemService(Context.NOTIFICATION_SERVICE);
                notificationManager.notify(NOTIFICATION_ID, notification);
        }


14. Takaisin sisään TodoActivity.java-tiedosto Päivitä Rekisteröi ilmoituksen käsittelijä luokan *ToDoActivity* -luokan **onCreate** -menetelmää. Varmista, että voit lisätä koodin, kun *MobileServiceClient* esiintymää.


        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Sovellus on nyt päivitetty tukemaan push-ilmoitukset.
