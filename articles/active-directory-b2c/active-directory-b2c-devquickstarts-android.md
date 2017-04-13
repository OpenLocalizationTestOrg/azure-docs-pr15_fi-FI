<properties
    pageTitle="Azure Active Directory-B2C: Soita verkko-Ohjelmointirajapinnan Android-sovelluksen | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan, kuinka voit luoda Android Tehtävät-luettelo-sovellusta, joka soittaa Node.js verkko-Ohjelmointirajapinnan käyttämällä OAuth 2.0 haltijan-tunnukset. Android-sovelluksen ja verkko-Ohjelmointirajapinnan käyttäminen Azure Active Directory-B2C käyttäjätietojen hallinta ja käyttäjien todentamiseen."
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure AD-B2C: Soita verkko-Ohjelmointirajapinnan Android-sovelluksesta

> [AZURE.WARNING] Tässä opetusohjelmassa edellyttää, että tärkeät päivitykset-erityisesti, jos haluat poistaa B2C ADAL Android käyttöä.  Voit julkaista ajan tasalla ohjeet Azure AD B2C käyttäminen Android-sovellukset seuraavalla viikolla tarkastellaan ja suosittelemme pitelevä siihen saakka.  Mutta jos haluat kokeilla kohteita ulos, vapaasti jatkaa artikkelin ohjeiden mukaisesti.



Azure Active Directory (Azure AD) B2C avulla voit lisätä tehokkaita Omatoiminen tunnistetietojen hallintaominaisuudet Android-sovellusten ja web ohjelmointirajapinnan muutaman lyhyt vaiheen avulla. Tässä artikkelissa kerrotaan, kuinka voit luoda Android "tehtäväluettelo"-sovellusta, joka soittaa Node.js verkko-Ohjelmointirajapinnan käyttämällä OAuth 2.0 haltijan-tunnukset. Android-sovelluksen ja verkko-Ohjelmointirajapinnan käyttäminen Azure AD B2C käyttäjätietojen hallinta ja käyttäjien todentamiseen.

Tämä pikaopas edellyttää, että verkko-Ohjelmointirajapinnan, joka on suojattu Azure AD B2C toimimaan täysin kanssa. Microsoft on suunniteltu yhden .NET ja voit käyttää Node.js. Tämä hallintapaketteihin oletetaan, että Node.js verkko-Ohjelmointirajapinnan otosten on määritetty. Lisätietoja on artikkelissa [Azure AD B2C verkko-Ohjelmointirajapinnan Node.js opetusohjelmaan](active-directory-b2c-devquickstarts-api-node.md).

Android-asiakkaat, jotka on suojattu resursseihin Azure AD on Active Directory käyttöoikeuksien kirjaston (ADAL). ADAL ainoana tarkoituksena on sovelluksen access tunnusten pääset helposti. Osoittaa, miten helppoa on tässä oppaassa on luoda sovelluksen Android Tehtävät-luettelo, joka:

- Saa access-tunnuksia, Soita tehtäväluettelo API [OAuth 2.0 todennus-protokollan](https://msdn.microsoft.com/library/azure/dn645545.aspx)avulla.
- Saa käyttäjien tehtäväluetteloita.
- Käyttäjät, merkit.

> [AZURE.NOTE] Tässä artikkelissa ei käsitellä käyttämällä Azure AD B2C ottamisesta käyttöön kirjautumisnimi, kirjautuminen ja Profiilin hallinta. Se keskitytään soittaminen verkko-ohjelmointirajapinnan, kun käyttäjä todennetaan. Jos et ole jo, olisi aloitetaan [.NET web app aloitusopas](active-directory-b2c-devquickstarts-web-dotnet.md) lisätietoja Azure AD B2C perusteet.

## <a name="get-an-azure-ad-b2c-directory"></a>Hae Azure AD B2C-kansio

Ennen kuin voit käyttää Azure AD B2C, voit luoda hakemiston tai vuokraaja. Kansio on kaikkien käyttäjien, sovellukset, ryhmät ja lisää säilö. Jos sitä ei ole vielä ole, ennen kuin voit [luoda hakemiston B2C](active-directory-b2c-get-started.md) edelleen tässä oppaassa.

## <a name="create-an-application"></a>Sovelluksen luominen

Seuraavaksi haluat luoda sovelluksen B2C hakemistossa. Näin pitää yhteyttä suojatusti sovelluksen tarvitsemiaan Azure AD-tietoja. Sovelluksen ja verkko-Ohjelmointirajapinnan ilmaistaan yksi **Sovellustunnus** tässä tapauksessa koska ne muodostavat yhteen looginen-sovellukseen. Voit luoda sovelluksen noudattamalla [näitä ohjeita](active-directory-b2c-app-registration.md). Muista:

- Sisällytä **web Appin**/**Verkko-Ohjelmointirajapinnan** -sovelluksessa.
- Kirjoita `urn:ietf:wg:oauth:2.0:oob` **vastaa URL**-osoitteeksi. Tässä esimerkissä koodin oletusarvoisen URL-osoite on.
- **Sovelluksen salaisuus** sovelluksen luominen ja kopioiminen. Tarvitset sen myöhemmin. Huomaa, että tämä arvo on oltava [XML-ohitettuja](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) , ennen kuin käytät sitä.
- Kopioi **Tunnus** , jolla määritetään sovelluksen. Tarvitset myös tämän myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Luoda oman käytäntöjä

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Azure AD B2C jokaisen käyttäjäkokemus on määritetty [käytännön](active-directory-b2c-reference-policies.md)avulla. Sovelluksen sisältää kolme tunnistetietojen kokemukset: Rekisteröidy, kirjaudu sisään ja kirjaudu sisään käyttämällä Facebook.  Haluat yhden käytännön kunkin tyypin [käytännön viitata artikkelissa](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kuvatulla tavalla. Kun olet luonut kolme käytäntöjä, muista:

- Valitse kirjautumisen käytännön **Näyttönimi** ja ilmoittautuminen määritteet.
- Valitse jokaisen käytännön **Näyttönimi** ja **Objektitunnus** sovelluksen saatavat. Voit valita muita saatavat.
- Kopioi kunkin käytännön **nimen** , sen luomisen jälkeen. Tunnisteen pitäisi olla etuliite `b2c_1_`.  Tarvitset seuraavat käytännön nimet myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kun olet luonut kolme käytäntöjä, olet valmis luomaan sovelluksen.

Huomaa, että tässä artikkelissa ei käsitellä käyttämisestä juuri luomasi käytännöt. Lisätietoja siitä, kuinka käytännöt toimivat Azure AD B2C, aloita [.NET web app aloitusopas](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Lataa koodi

Tämän opetusohjelman [määritetään GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android)koodi. Voit luoda otosten kuitenkin, [Lataa rakenne projektin .zip-tiedosto](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip). Voit myös kopioida rakenne:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **Lataa rakenne suorittamiseen tässä opetusohjelmassa tarvitaan.** Käyttöönoton täysin Android-sovelluksen monimutkaisuuden vuoksi rakenne on UX-koodi, joka suoritetaan, kun olet suorittanut tämän opetusohjelman. Tämä on sovelluskehittäjille aikaa säästäviä mitta. UX-koodia ei ole germane B2C lisääminen Android-sovelluksen ohjeaiheeseen.

Valmis-sovellus on myös [saatavilla .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip) tai `complete` saman säilö haaraa.

Voit muodostaa maven-testi, käyttämällä `pom.xml` ylimmällä tasolla.

  1. Ohjeiden [määrittäminen androidiin maven-testi edellytykset](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)-osassa.
  2. Määritä emulaattorin SDK 21 kanssa.
  3. Siirry mihin repo monistaa pääkansio.
  4. Suorita-komento `mvn clean install`.
  5. Vaihda kansio pikaopas otosten `cd samples\hello`.
  6. Suorita-komento `mvn android:deploy android:run`.

Raportissa pitäisi näkyä Käynnistä sovellus. Kirjoita testin käyttäjätietoja, voit kokeilla sitä.

Java-arkisto (JAR)-paketit lähetetään myös vieressä Android arkisto (AAR)-paketti.

## <a name="download-the-android-adal-and-add-it-to-your-android-studio-workspace"></a>Lataa Android ADAL ja lisää se Android Studio lisääminen työtilaan

Sinulla on käyttämisestä Android projektin kirjaston asetukset:

* Voit tuoda kirjaston Pimennys ja linkki sovelluksen lähdekoodin.
* Jos käytät Android Studio, voit käyttää AAR paketti-muodon ja viitata binaaritiedostoja.

### <a name="option-1-binaries-via-gradle-recommended"></a>Vaihtoehto 1: Binaaritiedostoja kautta Gradle (suositus)

Saat binaaritiedostoja maven-testi keskitetyn repo. Projektin Android Studiossa voidaan lisätä AAR-paketti (esimerkiksi `build.gradle`) näin:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

### <a name="option-2-aar-via-maven"></a>Vaihtoehto 2: AAR kautta maven-testi

Jos käytät `m2e` laajennuksen Pimennys, voit määrittää riippuvuuden oman `pom.xml` tiedosto:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

### <a name="option-3-source-via-git-last-resort"></a>Vaihtoehto 3: Tietolähteen Git kautta (viimeksi kädessä)

Saat lähdekoodi kautta Git SDK-Kirjoita:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

Käytä haaran **lähentää.**

## <a name="set-up-your-configuration-file"></a>Oman kokoonpanotiedosto määrittäminen

Määritys, joka määritetään aiemmissa määrittämistä Android projektin B2C-portaalissa.

Avaa `helpes/Constants.java` ja täytä seuraavat arvot:

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}


```
- `SCOPES`: Alueet, jotka siirtävät palvelimeen, jonka haluat pyytää takaisin palvelimelta, kun käyttäjä kirjautuu. Välittää B2C esikatselun `client_id`. Kuitenkin tämä on tarkoitus muuttaa `read scopes` myöhemmin. Tämä asiakirja päivitetään, joka on yhteydessä.
- `ADDITIONAL_SCOPES`: Lisää alueet, jota haluat käyttää sovelluksen. Ne on tarkoitus käyttää jatkossa.
- `CLIENT_ID`Sovellustunnus: olet saanut-portaalista.
- `REDIRECT_URL`: Uudelleenohjaus, jossa oletat kirjataan Edellinen tunnuksen.
- `EXTRA_QP`: Mitään ylimääräisiä haluat siirtää palvelimen URL-koodatun-muodossa.
- `FB_POLICY`Käytäntö: keskustelunäkymässä. Tämä on tämän hallintapaketteihin tärkeimmät osa.
- `EMAIL_SIGNIN_POLICY`Käytäntö: keskustelunäkymässä. Tämä on tämän hallintapaketteihin tärkeimmät osa.
- `EMAIL_SIGNUP_POLICY`Käytäntö: keskustelunäkymässä. Tämä on tämän hallintapaketteihin tärkeimmät osa.

## <a name="add-references-to-android-adal-to-your-project"></a>Viittaukset Android ADAL lisääminen projektiin

> [AZURE.NOTE]  ADAL androidille käyttää palveluita perustuva mallin käynnistää todennusta. Sovituksen "asettelu-sovellus toimii. Koko tässä esimerkissä ja kaikki ADAL for Android-keskukset siitä, miten voit hallita tätä ominaisuutta ja välittää tietoja niiden välillä.

Android kerro ensin sovellus, kuten sovituksen, jota haluat käyttää asettelua. Nämä sovituksen selitetään tarkemmin jäljempänä tässä opetusohjelmassa olevassa.

Päivitä projektin `AndroidManifest.xml` tiedosto, joka sisältää kaikki että tätä ominaisuutta:

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

Kuten näet, voit määrittää viisi toimintoja. Voit käyttää kaikkia seuraavasti.

- `AuthenticationActivity`: Tämä on peräisin ADAL ja se sisältää kirjautumisen web-näkymä.
- `LoginActivity`: Tämä näyttää Kirjaudu sisään käytännöt ja kunkin käytännön painikkeet.
- `SettingsActivity`: Voit käyttää suorituksen app-asetusten muuttaminen.
- `AddTaskActivity`: Voit tämän toiminnon avulla voit lisätä tehtäviä oman REST API, jotka on suojattu Azure AD.
- `ToDoActivity`: Tämä on ensisijainen toiminto, jossa näkyvät tehtävät.

## <a name="create-the-sign-in-activity"></a>Kirjaudu sisään-aktiviteetin luominen

Luo pää tehtävä ja anna sille `LoginActivity`.

Luo tiedosto nimeltä `LoginActivity.java`.

Sinun täytyy alustaa tehtävän ja lisätä joitakin painikkeita, jotka ohjaavat oman UI. Tämä on tuttuja, jos olet kirjoittanut Android koodia.

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}


```
Olet nyt luonut painikkeita, jotka Soita oman `ToDoActivity` palveluita (joka soittaa ADAL, kun tarvitset tunnuksen). Ne tehdään käyttämällä toimintosi viittaus ja ylimääräistä parametria. Tämä ylimääräinen parametri välitetään `intent.putExtra()` menetelmää. Voit määrittää `"thePolicy"` käyttämällä on määritetty `Constants.java`. Tämä kertoo käyttötarkoitukseen käytäntö käynnistää todennuksen aikana.

## <a name="create-the-settings-activity"></a>Luo tehtävän asetukset

Tämä on toiminto, joka täyttää asetusten Käyttöliittymän.

Luo tiedosto nimeltä `SettingsActivity.java` varten yksinkertainen luominen, lukeminen, päivittäminen ja poistaminen (CRUD) toimintoja.

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

## <a name="create-the-add-task-activity"></a>Lisää tehtävän aktiviteetin luominen

Tämän toiminnon avulla voit lisätä tehtävän REST API-päätepiste.

Luo tiedosto nimeltä `AddTaskActivity.java` ja kirjoita seuraava.

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}

```

## <a name="create-the-to-do-list-activity"></a>Tehtävät-luettelossa tehtävän luominen

Tämä on tärkeimmät tehtävän. Voit käyttää sen tunnuksen saaminen Azure AD käytännön ja kutsua tehtävän REST API-palvelinta, tunnuksen avulla.

Luo tiedosto nimeltä `ToDoActivity.java` ja kirjoita seuraava. (Kutsut myöhemmin selitetään.)

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}

```


 Olet ehkä jo huomannut, että tämä on riippuvainen menetelmiä, joilla ei ole kirjoitettu vielä. Tällöin käytettävissä ovat `updateLoggedInUser()`, `clearSessionCookie()`, ja `getTasks()`. Kirjoittaa ne tämän oppaan. Voit ohittaa virheet Android Studiossa turvallisesti nyt.

SELITYS parametrien:

  - `SCOPES`: Pakollinen, yrität pyytää käytön alueet. B2C esikatselussa, tämä on sama kuin `client_id`, mutta tämä on tarkoitus muuttaa myöhemmin.
  - `POLICY`:-Käytäntö, kun haluat tarkistaa.
  - `CLIENT_ID`: Tarvitaan, tulee Azure AD-portaalissa.
  - `redirectUri`: Voidaan määrittää paketin nimeksi. Sitä ei tarvita toimitettavat `acquireToken` Soita.
  - `getUserInfo()`Tarkista onko käyttäjä on jo välimuistin: tavalla. Parametrin käsitellään myös käyttäjää, jos käyttäjä ei löydy tai käyttäjän käyttöoikeustietue on virheellinen. Tämä menetelmä kirjoitetaan tämän oppaan.
  - `PromptBehavior.always`: Voit kysyä tunnistetiedot, voit ohittaa välimuistin ja eväste.
  - `Callback`: Kutsua jälkeen todennus-koodi on vaihdetaan tunnusta. Se on objektin `AuthenticationResult`, joka sisältää käyttöoikeustietue, erääntymispäivä ja ID-tunnuksen tietoja.

> [AZURE.NOTE]  Microsoft Intune yritysportaalisovellus on broker-osan, ja se on asennettu käyttäjän laitteessa. Sovellus on kertakirjautumisportaaliin (SSO) access kaikkien sovellusten laitteeseen. Kehittäjien tulee valmisteltu Intune varten. ADAL androidille käyttää broker-tili, jos on yksi käyttäjätili luodaan käyttöoikeudet. Jos haluat käyttää välittäjän, kehittäjä on Rekisteröi erityinen `redirectUri` käyttämään broker varten. `redirectUri`on msauth://packagename/Base64UrlencodedSignature muodossa. Voit hankkia `redirectUri` , kun sovellus käyttämällä komentosarjaa `brokerRedirectPrint.ps1` tai käyttämällä API-kutsu `mContext.getBrokerRedirectUri()`. Allekirjoituksen liittyy allekirjoitetun varmenteet Google Play-kaupasta.

 Voit ohittaa broker käyttäjän avulla:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```
> [AZURE.NOTE] Ovat valinneet on yksinkertaista B2C-pikaopas, jotta voit ohittaa broker töihin.

Seuraavaksi luodaan helper menetelmiä, jotka saat tunnuksen yksin todennus kutsujen tehtävään API aikana.

Saman `ToDoActivity.java` tiedoston, kirjoita seuraava.

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

Lisää myös menetelmiä, joka "Aseta" ja "Hanki- `AuthenticationResult` (joka on kohdassa tunnuksen) yleiseen `Constants`. Vaikka `ToDoActivity.java` käyttää `sResult` sen työnkulkuja ssä, sinun on lisättävä seuraavista tavoista. Jos et, muita toimintoja ei voi käyttää toimimaan tunnuksen (esimerkiksi lisäämällä tehtävän `AddTaskActivity.java`).

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }


```
## <a name="create-a-method-to-return-a-user-identifier"></a>Luo menetelmä palauttaa käyttäjätunnus

ADAL varten Android edustaa muodossa käyttäjä `UserIdentifier` objekti. Tämä hallitsee käyttäjä. Voit tunnistaa, onko sama käyttäjä käytetään puhelut-objekti. Näiden tietojen avulla voit luottaa välimuistin, sijaan soittaa uuden puhelun palvelimeen. Helpompi luomaamme `getUserInfo()` menetelmä, joka palauttaa `UserIdentifier`. Voit käyttää tätä kanssa `acquireToken()`. Myös luomaasi `getUniqueId()` menetelmä, joka palauttaa tunnus `UserIdentifier` välimuistin.

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }

```

### <a name="write-helper-methods"></a>Kirjoita helper menetelmät

Kirjoita seuraavaksi jotkin helper menetelmät Poista evästeet ja anna `AuthenticationCallback`. Näistä tavoista käytetään laskettuna varmistaaksesi, että olet SIIVOA-tilaan, kun soitat otosten oman `ToDo` tehtävän.

Saman tiedoston nimi `ToDoActivity.java`, kirjoita seuraava.

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }

```   

## <a name="call-the-task-api"></a>Soita tehtävän Ohjelmointirajapinta

Kun olet valmis käytetty tunnusten toimiessasi, voit kirjoittaa tehtävän-palvelimen yhteyttä API.

`getTasks`on taulukko, joka edustaa palvelimellesi tehtävät.

Aloita `getTasks`.

Saman tiedoston nimi `ToDoActivity.java`, kirjoita seuraava.

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }

```

Myös kirjoittaa menetelmän, joka alusta ensimmäisen käynnistyksen taulukoissa.

Saman tiedoston nimi `ToDoActivity.java`, kirjoita seuraava.

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }

```

Näet, että tämä koodi edellyttää joitakin muita tapoja käyttää. Kirjoita ne Seuraava.

### <a name="create-an-endpoint-url-generator"></a>Luo päätepiste-URL-Osoitteen luominen

Sinun täytyy luoda päätepisteen URL-osoite, johon muodostamassa yhteyttä. Tehdä luokan samaan tiedostoon.

**Samaan** kutsutaan `ToDoActivity.java`, kirjoita seuraava.

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }

 ```


Huomautus: access-tunnuksen lisääminen pyyntö käsitellään seuraavan osion koodissa.

## <a name="write-some-ux-methods"></a>Kirjoita jotkin UX menetelmät

Android edellyttää käsitellään joitakin takaisinkutsuja toimimaan sovellus. Nämä ovat `createAndShowDialog` ja `onResume()`. Tämä on tuttuja, jos olet kirjoittanut Android koodia.

Saman tiedoston nimi `ToDoActivity.java`, kirjoita seuraava.

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }


```

Seuraavaksi voit hallita valintaikkunan takaisinkutsuja.

Saman tiedoston nimi `ToDoActivity.java`, kirjoita seuraava.

```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }

```

Pitäisi nyt olla `ToDoActivity.java` tiedosto, joka kääntää. Koko projektin pitäisi myös Käännä tässä vaiheessa.

## <a name="run-the-sample-app"></a>Suorita malli-sovellus

Lopuksi muodosta ja suorita sovellus Android Studiossa tai Pimennys. Rekisteröitynyt tai kirjautunut-sovelluksessa. Luo kirjautunut sisään käyttäjän tehtävät. Kirjaudu ulos ja kirjaudu takaisin sisään eri käyttäjänä ja luo sitten kyseisen käyttäjän tehtävät.

Huomaa, että tehtäviä on tallennettu käyttäjäkohtainen-Ohjelmointirajapinnan, koska Ohjelmointirajapinnan poimii käyttäjän tunnistetiedot käyttöoikeustietue, se saa.

Viitteen, valmis malli [on annettu .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip). Voit myös Kloonaa se GitHub kohteesta:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-Android```


## <a name="important-information"></a>Tärkeitä tietoja


### <a name="encryption"></a>Salaus

ADAL salaa tunnusten ja säilytetään `SharedPreferences` oletusarvoisesti. Voit tarkistaa `StorageHelper` luokan tietojasi. Android-sovelluksen käyttöön **AndroidKeyStore 4.3(API18)** suojattuun säilöön, yksityiset avaimet. ADAL käyttää sitä API18 ja edellä. Jos haluat käyttää ADAL alemman SDK-versiot, sinun on antamaan salausavaimen osoitteessa `AuthenticationSettings.INSTANCE.setSecretKey`.

### <a name="session-cookies-in-web-view"></a>Istunnon evästeet web-näkymässä

Android-web-näkymä ei poista istunnon evästeet, kun sovellus suljetaan. Voit käsitellä seuraavaa kanssa.
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
[Lisätietoja evästeitä](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).
