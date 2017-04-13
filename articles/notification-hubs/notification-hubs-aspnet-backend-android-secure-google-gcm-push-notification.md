<properties
    pageTitle="Suojatun Push-ilmoitukset ja Azure ilmoituksen keskittimet lähettäminen"
    description="Lue, miten voit lähettää suojatun push-ilmoitukset Azure Android-sovelluksen. MALLIKOODEJA kirjoitettu Java- ja C#."
    documentationCenter="android"
    keywords="Push-ilmoituksen, push-ilmoitukset push-viestien android push-ilmoitukset"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

#<a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Suojatun Push-ilmoitukset ja Azure ilmoituksen keskittimet lähettäminen

> [AZURE.SELECTOR]
- [Windowsin Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android-laitteeseen](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

##<a name="overview"></a>Yleiskatsaus

> [AZURE.IMPORTANT] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Push-ilmoituksen tuki Microsoft Azure avulla voit käyttää helposti käytettävällä, useita, skaalata ulos push viestin infrastruktuuri, mikä vähentää huomattavasti yksinkertaistaa push-ilmoitukset mobile ympäristöjen kuluttaja- ja enterprise-sovellusten käyttöönoton.

Vuoksi säädösten tai suojauksen rajoitukset, joskus sovelluksen halutessasi lisätä jotain ilmoituksessa, joka voidaan lähettää vakio push-ilmoituksen infrastruktuurin kautta. Tässä opetusohjelmassa kerrotaan, miten saavuttamiseksi on sama kuvaus lähettämällä luottamuksellisia tietoja asiakkaan Android-laitteen ja sovelluksen Taustajärjestelmä suojatun, todennetun-yhteyden kautta.

Korkean tason kulun on seuraavanlainen:

1. -Sovelluksen taustatietokantaan:
    - Säilöjen suojatun paketti-taustatietokannan.
    - Lähettää ilmoituksen tunnus (suojattu mitään tietoja ei lähetetä) Android-laitteessa.
2. Sovelluksen laitteeseen, kun saat ilmoituksen:
    - Android-laitteen yhteystietojen pyytää suojatun sisällön taustatietokannan.
    - Sovellus voidaan näyttää tiedot ilmoituksen laitteeseen.

On tärkeää muistaa, että edellisen kulun (ja tässä opetusohjelmassa) oletetaan, että laitteen tallentaa todennus-tunnuksen paikalliseen säilöön sen jälkeen, kun käyttäjä kirjautuu. Tämä takaa laitteen voit hakea ilmoituksen suojattu tietojen käyttäminen tunnuksessa kokonaan sujuvan kokemuksen. Jos sovellusta ei Tallenna todennus tunnusten laitteeseen tai näiden tunnusten on päättynyt, laite-sovelluksen, kun saat ilmoituksen push pitäisi näkyä yleinen ilmoituksen käyttäjältä, Käynnistä sovellus. Valitse sovellus todentaa käyttäjän sekä näkyy ilmoitus-paketti.

Tässä opetusohjelmassa näytetään lähettäminen suojatun push-ilmoitukset. Se perustuu [Ilmoita käyttäjille](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) , opetusohjelman niin olisi vaiheiden kyseisen opetusohjelmassa ensimmäisen kerran, ellet ole jo.

> [AZURE.NOTE] Tässä opetusohjelmassa oletetaan, että olet luonut ja määrittänyt ilmoitus-toiminnossa kuvatulla tavalla [Ilmoituksen keskittimet (Android) käytön aloittaminen](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Muokkaa Android projektin

Nyt kun olet muokannut oman sovelluksen taustatietokantaan lähetetään vain *tunnus* push-ilmoituksen, on muutettava Android-sovelluksen Käsittele kyseiseen ilmoituksen ja soittaa takaisin hakemiseen suojatun viestin näytetään oman taustatietokannan.
Tämän tavoitteen saavuttamiseksi on Varmista, että Android-sovelluksen tietää, kuinka todentaa itse oman taustatietokannan, kun se saa push-ilmoitukset.

Olemme Muokkaa *Kirjautuminen* työnkulku nyt jotta todennus otsikon arvo tallennetaan sovelluksen jaetun asetukset. Paperimuotoon järjestelmiä voidaan tallentaa mitä tahansa todennus tunnuksen (kuten OAuth tunnuksiksi), sovellus on käytettävä tarvitsematta käyttäjän tunnistetietoja.

1. Lisää projektiin Android-sovelluksen **MainActivity** luokan yläreunassa seuraavia vakioita:

        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";

2. Edelleen päivittää **MainActivity** -luokka `getAuthorizationHeader()` menetelmä sisältää seuraava koodi:

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

            return basicAuthHeader;
        }

3. Lisää seuraava teksti `import` lauseet **MainActivity** tiedoston yläosassa:

        import android.content.SharedPreferences;

Nyt muutamme käsittelijä, jota kutsutaan, kun ilmoitus vastaanotetaan.

4. **MyHandler** -luokan muutos `OnReceive()` menetelmä sisältää:

        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }

5. Lisää `retrieveNotification()` -menetelmä Korvaa paikkamerkin `{back-end endpoint}` saatu otettaessa käyttöön oman taustatietokantaan taustatietokantaan päätepisteissä:

        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }


Tämä menetelmä kutsuu oman sovelluksen taustatietokantaan hakemiseen ilmoituksen sisältöä jaettuun asetukset tallennettuja tunnistetietoja avulla ja näyttää sen Normaali ilmoituksena. Ilmoitus näyttää sovelluksen täsmälleen kuten push-ilmoituksen käyttäjälle.

Huomaa, että se on suositeltavampaa käsittelemään ominaisuus puuttuu todennus-otsikon tai hylättäväksi tapauksissa sitten taustatietokantatiedosto mukaan. Tällaisissa tapauksissa käsittely määräytyvät enimmäkseen kohde käyttökokemusta. Yksi vaihtoehto on yleinen kehote ilmoituksen, että käyttäjä todennetaan hakemiseen ilmoituksen näyttämiseen.

## <a name="run-the-application"></a>Suorita sovellus

Suorita sovellus, toimi seuraavasti:

1. Varmista, että **AppBackend** on otettu käyttöön Azure. Jos Visual Studiossa, **AppBackend** verkko-Ohjelmointirajapinnan-sovelluksen käyttämiseen. ASP.NET-web-sivu tulee näkyviin.

2. Suorita sovellus Pimennys, fyysinen Android-laitteen tai emulaattori.

3. Android-sovelluksessa Käyttöliittymän Kirjoita käyttäjänimi ja salasana. Nämä voi olla mikä tahansa merkkijono, mutta niiden on oltava sama arvo.

4. Android-sovelluksessa Käyttöliittymän Valitse **Kirjaudu sisään**. Valitse **Lähetä push**.
