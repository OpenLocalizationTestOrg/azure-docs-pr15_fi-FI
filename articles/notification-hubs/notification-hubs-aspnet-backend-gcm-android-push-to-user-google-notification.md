<properties
    pageTitle="Azure ilmoituksen keskittimet Ilmoita käyttäjille for Androidin .NET taustaan"
    description="Lue, miten voit lähettää käyttäjille Azure-tietokannassa push-ilmoitukset. MALLIKOODEJA kirjoitettu Java androidille"
    documentationCenter="android"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-notify-users-for-android-with-net-backend"></a>Azure ilmoituksen keskittimet Ilmoita käyttäjille for Androidin .NET taustaan


[AZURE.INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

##<a name="overview"></a>Yleiskatsaus

Push-ilmoituksen tuki Azure avulla voit helposti käytettävällä, multiplatform ja skaalata ulos push infrastruktuuri, mikä vähentää huomattavasti yksinkertaistaa push-ilmoitukset mobile ympäristöjen kuluttaja- ja enterprise-sovellusten käyttöönoton. Tässä opetusohjelmassa näytetään, miten push-ilmoitukset lähetetään tietyn laitteen tietyn sovelluksen käyttäjän Azure ilmoituksen keskittimet avulla. ASP.NET-WebAPI Taustajärjestelmä käytetään tarkistamiseen asiakkaat ja luo ilmoituksia, ohjeaiheen [rekisteröinti sovelluksen-taustasta](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend)esitetyllä tavalla. Tässä opetusohjelmassa perustuu ilmoitus-toiminto, jonka loit [Ilmoituksen keskittimet (Android) käytön aloittaminen](notification-hubs-android-push-notification-google-gcm-get-started.md) -opetusohjelma.

> [AZURE.NOTE] Tässä opetusohjelmassa oletetaan, että olet luonut ja määrittänyt ilmoitus-toiminnossa kuvatulla tavalla [Ilmoituksen keskittimet (Android) käytön aloittaminen](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="create-the-android-project"></a>Luo Android-projekti

Seuraava vaihe on Android-sovelluksen luominen.

1. Katso [Ilmoituksen keskittimet (Android) käytön aloittaminen](notification-hubs-android-push-notification-google-gcm-get-started.md) -opetusohjelma, voit luoda ja määrittää sovelluksesi push-ilmoitusten vastaanottaminen GCM.

2. **Res/layout/activity_main.xml** tiedosto avaamalla se ja korvata sisällön tarkoitetaan kanssa.

    Tämä Lisää uusi EditText ohjausobjektit käyttäjänä kirjautumisesta. Kenttä lisätään myös käyttäjänimi-tunnisteen, joka on osa ilmoitukset lähetetään:

        <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
            android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">

        <EditText
            android:id="@+id/usernameText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/usernameHint"
            android:layout_above="@+id/passwordText"
            android:layout_alignParentEnd="true" />
        <EditText
            android:id="@+id/passwordText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/passwordHint"
            android:inputType="textPassword"
            android:layout_above="@+id/buttonLogin"
            android:layout_alignParentEnd="true" />
        <Button
            android:id="@+id/buttonLogin"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/loginButton"
            android:onClick="login"
            android:layout_above="@+id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:layout_marginBottom="24dp" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="WNS on"
            android:textOff="WNS off"
            android:id="@+id/toggleButtonWNS"
            android:layout_toLeftOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="GCM on"
            android:textOff="GCM off"
            android:id="@+id/toggleButtonGCM"
            android:checked="true"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="APNS on"
            android:textOff="APNS off"
            android:id="@+id/toggleButtonAPNS"
            android:layout_toRightOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessageTag"
            android:layout_below="@id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_tag_hint" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessage"
            android:layout_below="@+id/editTextNotificationMessageTag"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_hint" />
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/send_button"
            android:id="@+id/sendbutton"
            android:onClick="sendNotificationButtonOnClick"
            android:layout_below="@+id/editTextNotificationMessage"
            android:layout_centerHorizontal="true" />
        </RelativeLayout>



3. **Res/values/strings.xml** -tiedoston avaaminen ja korvaa `send_button` seuraavat rivit, jotka uudelleen merkkijonon määritelmä `send_button` ja merkkijonot muiden ohjausobjektien lisääminen:

        <string name="usernameHint">Username</string>
        <string name="passwordHint">Password</string>
        <string name="loginButton">1. Log in</string>
        <string name="send_button">2. Send Notification</string>
        <string name="notification_message_tag_hint">
            Recipient username tag
        </string>

    Graafisen asettelun main_activity.xml pitäisi näyttää tältä:

    ![][A1]

4. Luo uusi luokka-nimisen **RegisterClient** samaan pakettiin kuin oman `MainActivity` luokka. Käytä alla koodia uuden luokan tiedoston.

        import java.io.IOException;
        import java.io.UnsupportedEncodingException;
        import java.util.Set;

        import org.apache.http.HttpResponse;
        import org.apache.http.HttpStatus;
        import org.apache.http.client.ClientProtocolException;
        import org.apache.http.client.HttpClient;
        import org.apache.http.client.methods.HttpPost;
        import org.apache.http.client.methods.HttpPut;
        import org.apache.http.client.methods.HttpUriRequest;
        import org.apache.http.entity.StringEntity;
        import org.apache.http.impl.client.DefaultHttpClient;
        import org.apache.http.util.EntityUtils;
        import org.json.JSONArray;
        import org.json.JSONException;
        import org.json.JSONObject;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.util.Log;

        public class RegisterClient {
            private static final String PREFS_NAME = "ANHSettings";
            private static final String REGID_SETTING_NAME = "ANHRegistrationId";
            private String Backend_Endpoint;
            SharedPreferences settings;
            protected HttpClient httpClient;
            private String authorizationHeader;

            public RegisterClient(Context context, String backendEnpoint) {
                super();
                this.settings = context.getSharedPreferences(PREFS_NAME, 0);
                httpClient =  new DefaultHttpClient();
                Backend_Endpoint = backendEnpoint + "/api/register";
            }

            public String getAuthorizationHeader() {
                return authorizationHeader;
            }

            public void setAuthorizationHeader(String authorizationHeader) {
                this.authorizationHeader = authorizationHeader;
            }

            public void register(String handle, Set<String> tags) throws ClientProtocolException, IOException, JSONException {
                String registrationId = retrieveRegistrationIdOrRequestNewOne(handle);

                JSONObject deviceInfo = new JSONObject();
                deviceInfo.put("Platform", "gcm");
                deviceInfo.put("Handle", handle);
                deviceInfo.put("Tags", new JSONArray(tags));

                int statusCode = upsertRegistration(registrationId, deviceInfo);

                if (statusCode == HttpStatus.SC_OK) {
                    return;
                } else if (statusCode == HttpStatus.SC_GONE){
                    settings.edit().remove(REGID_SETTING_NAME).commit();
                    registrationId = retrieveRegistrationIdOrRequestNewOne(handle);
                    statusCode = upsertRegistration(registrationId, deviceInfo);
                    if (statusCode != HttpStatus.SC_OK) {
                        Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                        throw new RuntimeException("Error upserting registration");
                    }
                } else {
                    Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                    throw new RuntimeException("Error upserting registration");
                }
            }

            private int upsertRegistration(String registrationId, JSONObject deviceInfo)
                    throws UnsupportedEncodingException, IOException,
                    ClientProtocolException {
                HttpPut request = new HttpPut(Backend_Endpoint+"/"+registrationId);
                request.setEntity(new StringEntity(deviceInfo.toString()));
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                request.addHeader("Content-Type", "application/json");
                HttpResponse response = httpClient.execute(request);
                int statusCode = response.getStatusLine().getStatusCode();
                return statusCode;
            }

            private String retrieveRegistrationIdOrRequestNewOne(String handle) throws ClientProtocolException, IOException {
                if (settings.contains(REGID_SETTING_NAME))
                    return settings.getString(REGID_SETTING_NAME, null);

                HttpUriRequest request = new HttpPost(Backend_Endpoint+"?handle="+handle);
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                HttpResponse response = httpClient.execute(request);
                if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                    Log.e("RegisterClient", "Error creating registrationId: " + response.getStatusLine().getStatusCode());
                    throw new RuntimeException("Error creating Notification Hubs registrationId");
                }
                String registrationId = EntityUtils.toString(response.getEntity());
                registrationId = registrationId.substring(1, registrationId.length()-1);

                settings.edit().putString(REGID_SETTING_NAME, registrationId).commit();

                return registrationId;
            }
        }

    Tämän osan toteuttaa REST-puhelut pakollinen yhteystiedot app taustatietokannan, jotta voit rekisteröidä push-ilmoitukset. Ilmoitus-toiminnossa [rekisteröinti sovelluksen-taustasta](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend)yksilöityjä luoma *registrationIds* tallennetaan myös paikallisesti. Huomaa, että se vastaa luvan-tunnuksen, kun napsautat **Kirjaudu sisään** -painiketta paikallisen tallennustilaan tallennettuja.

5. -Että `MainActivity` luokan poistaminen tai kommentoi pois private-kenttä `NotificationHub`, ja Lisää kenttä `RegisterClient` luokan ja ASP.NET-taustatietokannan päätepisteen merkkijonon. Varmista, että korvaa `<Enter Your Backend Endpoint>` kanssa todellinen Taustajärjestelmä päätepisteen saatu aiemmin. Esimerkiksi `http://mybackend.azurewebsites.net`.


        //private NotificationHub hub;
        private RegisterClient registerClient;
        private static final String BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";


6. -Oman `MainActivity` -luokan `onCreate` menetelmä, poista tai kommentoi pois alustaminen `hub` kenttä ja kutsu `registerWithNotificationHubs` menetelmää. Lisää koodi alustaa esiintymä `RegisterClient` luokka. Menetelmä pitäisi olla seuraavat rivit:

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            MyHandler.mainActivity = this;
            NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);
            gcm = GoogleCloudMessaging.getInstance(this);

            //hub = new NotificationHub(HubName, HubListenConnectionString, this);
            //registerWithNotificationHubs();

            registerClient = new RegisterClient(this, BACKEND_ENDPOINT);

            setContentView(R.layout.activity_main);
        }

7. Valitse oman `MainActivity` luokan, poistaa tai kommentti kokonaan ulos `registerWithNotificationHubs` menetelmä. Se ei käytetä tässä opetusohjelmassa.

8. Lisää seuraava `import` lauseet **MainActivity.java** -tiedostoon.

        import android.widget.Button;
        import java.io.UnsupportedEncodingException;
        import android.content.Context;
        import java.util.HashSet;
        import android.widget.Toast;
        import org.apache.http.client.ClientProtocolException;
        import java.io.IOException;
        import org.apache.http.HttpStatus;


9. Lisää sitten **Kirjaudu sisään** -painike käsittelemään seuraavista tavoista valitsemalla tapahtuma- ja lähettämisen push-ilmoitukset.

        @Override
        protected void onStart() {
            super.onStart();
            Button sendPush = (Button) findViewById(R.id.sendbutton);
            sendPush.setEnabled(false);
        }

        public void login(View view) throws UnsupportedEncodingException {
            this.registerClient.setAuthorizationHeader(getAuthorizationHeader());

            final Context context = this;
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        String regid = gcm.register(SENDER_ID);
                        registerClient.register(regid, new HashSet<String>());
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to register", e.getMessage());
                        return e;
                    }
                    return null;
                }

                protected void onPostExecute(Object result) {
                    Button sendPush = (Button) findViewById(R.id.sendbutton);
                    sendPush.setEnabled(true);
                    Toast.makeText(context, "Logged in and registered.",
                            Toast.LENGTH_LONG).show();
                }
            }.execute(null, null, null);
        }

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
            return basicAuthHeader;
        }

        /**
         * This method calls the ASP.NET WebAPI backend to send the notification message
         * to the platform notification service based on the pns parameter.
         *
         * @param pns     The platform notification service to send the notification message to. Must
         *                be one of the following ("wns", "gcm", "apns").
         * @param userTag The tag for the user who will receive the notification message. This string
         *                must not contain spaces or special characters.
         * @param message The notification message string. This string must include the double quotes
         *                to be used as JSON content.
         */
        public void sendPush(final String pns, final String userTag, final String message)
                throws ClientProtocolException, IOException {
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {

                        String uri = BACKEND_ENDPOINT + "/api/notifications";
                        uri += "?pns=" + pns;
                        uri += "&to_tag=" + userTag;

                        HttpPost request = new HttpPost(uri);
                        request.addHeader("Authorization", "Basic "+ getAuthorizationHeader());
                        request.setEntity(new StringEntity(message));
                        request.addHeader("Content-Type", "application/json");

                        HttpResponse response = new DefaultHttpClient().execute(request);

                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            DialogNotify("MainActivity - Error sending " + pns + " notification",
                                response.getStatusLine().toString());
                            throw new RuntimeException("Error sending notification");
                        }
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to send " + pns + " notification ", e.getMessage());
                        return e;
                    }

                    return null;
                }
            }.execute(null, null, null);
        }


    `login` **Kirjaudu sisään** -painike käsittelijä Luo basic todennus tunnussanoma käyttäminen syötteen käyttäjänimellä ja salasanalla (Huomaa, että tämä on minkä tahansa tunnuksen todennusmalli käyttää) ja valitse se käyttää `RegisterClient` Soita taustaan rekisteröinnin.

    `sendPush` Menetelmä kutsuu Taustajärjestelmä käynnistettävän suojatun ilmoituksen käyttäjälle käyttäjän tunnisteen perusteella. Ympäristö-ilmoitus-palvelu, joka `sendPush` kohteet, riippuu siitä `pns` merkkijonon välitetty.

10. -Että `MainActivity` luokka-, päivitys `sendNotificationButtonOnClick` menetelmä, jota kutsutaan `sendPush` menetelmä, jolla käyttäjän valittuna ympäristö ilmoituksen services seuraavasti.

        /**
         * Send Notification button click handler. This method sends the push notification
         * message to each platform selected.
         *
         * @param v The view
         */
        public void sendNotificationButtonOnClick(View v)
                throws ClientProtocolException, IOException {

            String nhMessageTag = ((EditText) findViewById(R.id.editTextNotificationMessageTag))
                    .getText().toString();
            String nhMessage = ((EditText) findViewById(R.id.editTextNotificationMessage))
                    .getText().toString();

            // JSON String
            nhMessage = "\"" + nhMessage + "\"";

            if (((ToggleButton)findViewById(R.id.toggleButtonWNS)).isChecked())
            {
                sendPush("wns", nhMessageTag, nhMessage);
            }
            if (((ToggleButton)findViewById(R.id.toggleButtonGCM)).isChecked())
            {
                sendPush("gcm", nhMessageTag, nhMessage);
            }
            if (((ToggleButton)findViewById(R.id.toggleButtonAPNS)).isChecked())
            {
                sendPush("apns", nhMessageTag, nhMessage);
            }
        }



## <a name="run-the-application"></a>Suorita sovellus


1. Suorita sovellus laitteen tai emulaattorin Android Studiossa.

2. Kirjoita Android-sovelluksen käyttäjänimeä ja salasanaa. Niiden on oltava sama merkkijonoarvo ja ne eivät saa sisältää välilyöntejä tai erikoismerkkejä.

3. Android-sovelluksessa valitsemalla **Kirjaudu sisään**. Saapuvan viestin, joka ilmoittaa **kirjattu ja rekisteröity**odota. Tämä ottaa käyttöön **Lähetä ilmoitus** -painiketta.

    ![][A2]

4. Valitsemalla-asetus käyttöön kaikissa ympäristöissä, jossa on painikkeet suoritettiin sovellus ja rekisteröity käyttäjä.
5. Kirjoita käyttäjän nimi, joka vastaanottaa ilmoitusviesti. Käyttäjän on rekisteröitävä ilmoitusten kohde-laitteissa.

6. Kirjoita viesti, käyttäjä voi vastaanottaa ilmoituksen push-viestinä.
7. Valitse **Lähetä ilmoitus**.  Kunkin laitteissa, joissa on rekisteröinti vastaavia käyttäjänimi-tunniste saavat push-ilmoituksen.


[A1]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users.png
[A2]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users-enter-password.png
