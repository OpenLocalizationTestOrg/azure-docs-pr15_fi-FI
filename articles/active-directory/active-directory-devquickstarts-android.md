<properties
    pageTitle="Aloittaminen Azure AD-Android | Microsoft Azure"
    description="Miten voit luoda Android sovellus, joka integroituu Azure AD kirjauduttaessa ja soittaa Azure AD suojattu ohjelmointirajapinnan käyttäminen OAuth."
    services="active-directory"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-android-app"></a>Azure AD integroida Android-sovelluksen

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Jos kehität työpöytäsovellus, Azure AD on yksinkertainen ja selkeä voi todentaa käyttäjien käyttämällä niiden Active Directory-tilit.  Ottaa käyttöön myös sovelluksen tarjoaman suojatusti minkä tahansa verkko-Ohjelmointirajapinnan Azure AD, kuten Office 365-ohjelmointirajapinnan tai Azure-Ohjelmointirajapinnan suojattu.

Android-asiakkaat, jotka on suojattu resursseihin Azure AD on Active Directory käyttöoikeuksien kirjaston tai ADAL.  ADAL on aika-ainoa tarkoitus on helpompaa, kun sovellus hakea access tunnuksia.  Osoittamaan vain näin helppoa tähän on muodostaa tehtäväluettelo Android-sovelluksen, seuraavasti:

-   Saa käyttää tunnusten soitettavien Tehtävät luettelon API- [OAuth 2.0 todennus-protokollan](https://msdn.microsoft.com/library/azure/dn645545.aspx)avulla.
-   Saa käyttäjän Tehtävät-luettelo
-   Etumerkki käyttäjät ulos.

Aluksi sinun on Azure AD-vuokraajan, jossa voit luoda käyttäjille ja sovelluksen rekisteröiminen.  Jos sinulla ei vielä ole vuokraajan, [Lue, miten voit hankkia sen](active-directory-howto-tenant.md).

> [AZURE.TIP] Kokeile Microsoftin, joka auttaa sinua lisämääritykset Azure Active Directory-hakemistosta muutaman minuutin kuluttua uuden [developer-portaalissa](https://identity.microsoft.com/Docs/Android) esikatselu!  Developer-portaalissa opastaa sovellus rekisteröidään ja Azure AD integroiminen koodisi.  Kun olet valmis, käytössäsi on yksinkertainen sovellus, joka voi todentaa alihallintaan ja taustassa käyttäjät, jotka voit hyväksyä tunnusten ja suorittaa vahvistus. 

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Vaihe 1: Lataa ja suorita Node.js REST API TODO otoksen palvelimeen

Tässä esimerkissä on kirjoitettu erityisesti toimimaan etsimisen yhtä vuokraajan tehtävät REST API Microsoft Azure Active Directory Microsoftin aiemmin otoksen perusteella. Tämä on vanhat tarvittavat varten Pika-aloitus.

Lisätietoja siitä, miten voit määrittää tämän käy Microsoftin aiemmin näytteiden tähän:

* [Microsoft Azure Active Directory-Esimerkki REST API palvelun Node.js varten](active-directory-devquickstarts-webapi-nodejs.md)

## <a name="step-2-register-your-web-api-with-your-microsoft-azure-ad-tenant"></a>Vaihe 2: Voit rekisteröidä verkko-Ohjelmointirajapinnan Microsoft Azure AD-vuokraajan

**Mitä voin voimme tehdä?**

*Microsoft Active Directory tukee kahdentyyppisiä sovellusten lisääminen. Verkko-ohjelmointirajapinnan, jotka tarjoavat palveluita käyttäjille ja sovelluksia (joko verkossa tai laitteessa sovelluksen), jotka käyttävät näitä verkko-ohjelmointirajapinnan. Tässä vaiheessa voit rekisteröityvät sinulla on käytössä paikallisesti testikäyttöön tässä esimerkissä verkko-Ohjelmointirajapinnan. Tavallisesti tämä verkko-Ohjelmointirajapinnan olisi REST-palvelun, joka tarjoaa haluat käyttää sovelluksen toimintoja. Microsoft Azure Active Directory voit suojata kaikki päätepisteen!*

*Seuraavassa on ovat olettaen TODO REST-Ohjelmointirajapinnalla edellä mainitut rekisteröityvät, mutta tämä koskee minkä tahansa verkko-Ohjelmointirajapinnan haluat ehkä Azure Active Directory suojaaminen.*

Verkko-Ohjelmointirajapinnan rekisteröityä Microsoft Azure AD vaiheet

1. Kirjautuminen [Azure hallinta-portaalin](https://manage.windowsazure.com).
2. Valitse Active Directory vasemmalla olevasta NAV-ohjelmassa.
3. Napsauta kohtaa, johon haluat rekisteröidä sovelluksen malli directory vuokraajan.
4. Valitse sovellukset-välilehti.
5. Laatikko Valitse Lisää.
6. Valitse "Lisää organisaation kehittää sovellus".
7. Valitse sovellus, esimerkiksi "TodoListService", "Web Application ja/tai Web API" helpossa muodossa nimi ja valitse Seuraava.
8. Kirjoita Sign-URL-Perusosoitteen otoksen, joka on oletusarvoisesti `https://localhost:8080`.
9. Kirjoita sovelluksen tunnus-URI `https://<your_tenant_name>/TodoListService`, korvaaminen `<your_tenant_name>` Azure AD-vuokraajan nimi.  Voit suorittaa rekisteröinnin valitsemalla OK.
10. Kun edelleen Azure-portaaliin, valitse-sovelluksen määrittäminen-välilehti.
11. **Etsi Asiakastunnus ja kopioi se varaa**, sinun on tämä myöhemmin sovelluksen määritettäessä.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Vaihe 3: Rekisteröi Android Native Client-sovelluksen malli

Web-sovelluksen on ensimmäinen vaihe. Seuraavaksi tarvitset Azure Active Directory Kerro sovelluksesi paikan päällä. Näin vain rekisteröity verkko-Ohjelmointirajapinnan kanssa kommunikoimiseen sovelluksen

**Mitä voin voimme tehdä?**  

*Edellä ilmoitetulla Microsoft Azure Active Directory tukee lisääminen kahdentyyppisiä sovellukset. Verkko-ohjelmointirajapinnan, jotka tarjoavat palveluita käyttäjille ja sovelluksia (joko verkossa tai laitteessa sovelluksen), jotka käyttävät näitä verkko-ohjelmointirajapinnan. Tässä vaiheessa voit rekisteröityvät tässä esimerkissä sovelluksen. Sinun on tehtävä, jotka voivat käyttää verkko-Ohjelmointirajapinnan pyyntö vain rekisteröity tämän sovelluksen järjestyksessä. Azure Active Directory kieltäytyä avulla voit myös pyytää kirjautumista varten, ellei se on rekisteröity sovelluksen! Joka on osa mallin suojaus.*

*Seuraavassa on ovat olettaen rekisteröityvät edellä mainitut otoksen sovelluksen, mutta tämä koskee kaikkia sovelluksia on kehittäminen.*

**Miksi minua voin osoittamassa sovelluksen ja verkko-Ohjelmointirajapinnan yhdestä vuokraajasta?**

*Kun sinulla voi olla arvata, voit luoda sovelluksen, joka käyttää ulkoisen API, joka on rekisteröity Azure Active Directoryn kohteesta toiseen. Jos teet näin, asiakkaiden pyydetään API-sovelluksen käytön hyväksyminen. Nice osa on Active Directory käyttöoikeuksien kirjasto iOS kestää varoen tämän luvan puolestasi! Lähestyessä kehittyneempiä ominaisuuksia, näyttöön tulee tämä on tärkeä osa, voit käyttää Microsoft APIs suite Azure ja Officen sekä muita palveluntarjoajan tarvittavaa työmäärää. Nyt, koska rekisteröity verkko-Ohjelmointirajapinnan ja sovelluksen kohdassa samassa alihallinnassa eivät näy kehotteita suostumusta. Tämä on yleensä Jos kehität hakemus vain oman yrityksesi käyttää.*

1. Kirjautuminen [Azure hallinta-portaalin](https://manage.windowsazure.com).
2. Valitse Active Directory vasemmalla olevasta NAV-ohjelmassa.
3. Napsauta kohtaa, johon haluat rekisteröidä sovelluksen malli directory vuokraajan.
4. Valitse sovellukset-välilehti.
5. Laatikko Valitse Lisää.
6. Valitse "Lisää organisaation kehittää sovellus".
7. Kirjoita kutsumanimi-sovelluksen, esimerkiksi "TodoListClient kuin Android-, valitse"alkuperäisen asiakassovellus", ja valitse sitten Seuraava.
8. Määritä uudelleenohjaus URI-tunnus `http://TodoListClient`.  Valitse valmis.
9. Valitse sovelluksen määrittäminen-välilehti.
10. Etsi Asiakastunnus ja kopioi se alkutoimia, sinun on tämä myöhemmin sovelluksen määritettäessä.
11. Valitse "Oikeudet ja muut sovellukset", "Lisää sovellus."  Valitse "Muut" "Näytä-valikko ja valitse ylemmässä valintamerkki.  Etsi ja valitse TodoListService ja valitse ala-valintaruudun, voit lisätä sovelluksen.  Valitse avattavasta valikosta "Valtuutetun käyttöoikeudet" "Access TodoListService" ja Tallenna määritykset.



Voit muodostaa maven-testi, voit käyttää pom.xml ylimmällä tasolla

  * Kloonaa tämän repo-kansioon valittua:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  

  * Noudattamalla [Prerequests osan määrittäminen oman maven-testi androidille](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)
  * Asennuksen emulaattorin SDK 19 kanssa
  * Siirry mihin repo monistaa pääkansio
  * Suorita-komento: mvn SIIVOA asentaminen
  * Vaihda kansio otosten Pika-aloitus: cd samples\hello
  * Suorita-komento: mvn android: android: Suorita käyttöönotto
  * Näkyviin tulee sovelluksen käynnistäminen
  * Kirjoita testin käyttäjätietoja, kokeile!

Purkki pakettien lähetetään myös aar paketin vieressä.

### <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Vaihe 4: Lataa Android ADAL ja lisää se Pimennys lisääminen työtilaan

On tehty sen helposti on useita vaihtoehtoja Android projektin tämän kirjaston käyttäminen:

* Voit tuoda tämän kirjaston Pimennys ja linkki sovelluksen lähdekoodin.
* Jos Android Studiossa, voit käyttää *aar* paketin muoto ja viitata binaaritiedostoja.

####<a name="option-1-source-zip"></a>Vaihtoehto 1: Lähteen Zip

Lataa kopio lähdekoodin napsauttamalla "Lataa ZIP-sivun oikeassa reunassa tai [tähän](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

####<a name="option-2-source-via-git"></a>Vaihtoehto 2: Tietolähteen Git kautta

Saat kautta git SDK lähdekoodia Kirjoita vain:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

####<a name="option-3-binaries-via-gradle"></a>Vaihtoehto 3: Binaaritiedostot Gradle kautta

Saat binaaritiedostoja maven-testi keskitetyn repo. AAR paketti voidaan lisätä projektin AndroidStudio seuraavasti:

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
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

####<a name="option-4-aar-via-maven"></a>Vaihtoehto 4: aar kautta maven-testi

Jos käytät m2e laajennus Pimennys, voit määrittää riippuvuuden pom.xml-tiedosto:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


####<a name="option-5-jar-package-inside-libs-folder"></a>Vaihtoehto 5: jar paketin kirjastoa kansioon
Voit saada purkki tiedoston maven-testi repo ja pudota *kirjastoa* kansioon projektin. Haluat kopioida tarvittavat resurssit lisääminen projektiin, koska purkki paketit eivät sisällä niitä.


### <a name="step-5-add-references-to-android-adal-to-your-project"></a>Vaihe 5: Viittauksia Android ADAL lisääminen projektiin


2. Lisää projektiin viittaus ja määrittää Android kirjastoon. Jos et ole varma siitä, miten voit tehdä tämän [napsauttamalla tätä saat lisätietoja] (http://developer.android.com/tools/projects/projects-eclipse.html)

3. Lisää virheiden poistamiseen projektin asetukset project-riippuvuus

4. Päivitä projektin AndroidManifest.xml-tiedosto, joka sisältää:

    ```Java
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
      ....
      <application/>
    ```

7. Luoda oman pääasiallista AuthenticationContext esiintymä. Puhelun tiedot ovat tämän Lueminut-tiedosto laajemmin, mutta saat hyvän Käynnistä katsomalla [Android Native Client-malli](https://github.com/AzureADSamples/NativeClient-Android). Alla on esimerkki:

    ```Java
    // Authority is in the form of https://login.windows.net/yourtenant.onmicrosoft.com
    mContext = new AuthenticationContext(MainActivity.this, authority, true); // This will use SharedPreferences as            default cache
    ```
  * Huomautus: mContext on toimiessasi-kenttä

8. Kopioi tämän koodin lohkon käsittelemään AuthenticationActivity loppuun, kun käyttäjä lisää tunnistetiedot ja vastaanottaa luvan koodi:

    ```Java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         super.onActivityResult(requestCode, resultCode, data);
         if (mContext != null) {
             mContext.onActivityResult(requestCode, resultCode, data);
         }
     }
    ```

9. Kysy tunnistetta, voit määrittää takaisinkutsun

    ```Java
    private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };
    ```
10. Kysy-tunnuksen, avulla, että:

    ```Java
     mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                    callback);
    ```

SELITYS parametrien:

  * Resurssi on pakollinen ja yrität käyttää resurssi.
  * Clientid tarvitaan ja AzureAD-portaalissa on peräisin.
  * Voit asennuksen redirectUri kuin oman pakkauksen nimi. Ei ole pakollinen toimitettavat acquireToken puhelun.
  * PromptBehavior avulla voidaan kysyä tunnistetiedot, voit ohittaa välimuistin ja eväste.
  * Takaisinsoitto kutsutaan, kun luvan koodi on vaihdetaan tunnusta.

  Takaisinkutsu on objektin AuthenticationResult, jossa on accesstoken, päivämäärä on vanhentunut, ja idtoken tiedot.

Valinnainen: **acquireTokenSilent**

Voit soittaa **acquireTokenSilent** käsittelemään välimuistin ja suojaustunnuksen Päivitä. Siinä on myös synkronointi-versio. Se hyväksyy käyttäjätunnus parametrina.

    ```java
     mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

11. **Broker**: Microsoft Intune yritysportaalisovellus päivitystavasta broker-osa. ADAL käyttäminen broker-tili, jos yksi käyttäjätili luodaan tämän todentaja ja Developer valitsee ei, jos haluat ohittaa sen. Kehittäjä voi ohittaa broker käyttäjälle:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

 Kehittäjä on Rekisteröi erityinen redirectUri broker käytön. RedirectUri on msauth://packagename/Base64UrlencodedSignature muoto. Voit hankkia oman redirecturi, kun sovellus komentosarjan "brokerRedirectPrint.ps1" avulla tai käyttää API puhelun mContext.getBrokerRedirectUri. Allekirjoituksen liittyy allekirjoitetun varmenteet.

 Nykyinen broker malli on yhdelle käyttäjälle. AuthenticationContext avulla API saat broker käyttäjä.

 ```java
 String brokerAccount =  mContext.getBrokerUser();
 ```
 Broker käyttäjä palautetaan, jos tilisi on voimassa.

 Sovellus-luettelossa pitäisi olla oikeudet käyttää AccountManager tilejä: http://developer.android.com/reference/android/accounts/AccountManager.html

 * GET_ACCOUNTS
 * USE_CREDENTIALS
 * MANAGE_ACCOUNTS


Käytä tätä vaiheittaista, pitäisi olla haluat integroida onnistuneesti Azure Active Directory-hakemistosta. Lisää esimerkkejä tämän työpäivän päivityksistä AzureADSamples / säilöön-GitHub.

## <a name="important-information"></a>Tärkeitä tietoja

### <a name="customization"></a>Mukauttaminen

Kirjaston projektiresurssit voidaan korvata sovelluksen resurssien mukaan. Tämä tapahtuu sovelluksen luotaessa. Tästä syystä voit mukauttaa todennus tehtävän asettelua niin kuin haluat. Sinun täytyy tehdä Säilytä tunnus valvonnasta, että ADAL uses(Webview).

### <a name="broker"></a>Broker

Broker osan toimitetaan Microsoft Intune yritysportaalisovellus kanssa. Tilin luodaan tilin hallinta. Valitse tilin tyyppi on "com.microsoft.workaccount". Vain yhden SSO-tilin avulla. Se luo SSO eväste kyseisen käyttäjän laitteen todennus johonkin sovellukset suorittamisen jälkeen.

### <a name="authority-url-and-adfs"></a>Myöntäjä URL-osoite ja ADFS

ADFS ei tunnisteta tuotannon STS-ympäristö, niin, että haluat poistaa esiintymän etsiminen ja välittää EPÄTOSI AuthenticationContext konstruktoria.

Myöntäjä URL-osoite on STS esiintymän ja vuokraajan nimi: https://login.windows.net/yourtenant.onmicrosoft.com

### <a name="querying-cache-items"></a>Kyselyt välimuistin kohteet

ADAL sisältää oletusarvon välimuistin SharedPreferences joitakin yksinkertaisia välimuistin ja kysely-funktioita. Saat nykyisen välimuistin AuthenticationContext kanssa:
```Java
 ITokenCacheStore cache = mContext.getCache();
```
Voit myös lisätä käyttöympäristösi välimuistin, jos haluat mukauttaa sitä.
```Java
mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);
```

### <a name="promptbehavior"></a>PromptBehavior

ADAL on vaihtoehto, jos haluat määrittää kehotteen toiminnan. PromptBehavior.Auto ponnahdusikkuna Käyttöliittymän käyttäjän tunnistetiedot ovat pakollisia, jos päivityksen tunnus on virheellinen. PromptBehavior.Always ohittaa välimuistin käyttö ja Näytä aina UI.

### <a name="silent-token-request-from-cache-and-refresh"></a>Hiljainen suojaustunnuksen pyytää välimuistin ja päivittäminen

Tämä menetelmä ei käytä Käyttöliittymän pop määrittäminen ja edellytä aktiviteetin. Palauttaa suojaustunnuksen välimuistista, jos se on käytettävissä. Jos tunnus on päättynyt, se yrittää päivittää tiedot. Jos päivityksen tunnus on vanhentunut tai ei onnistunut, se palauttaa AuthenticationException.

    ```Java
    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

Voit tehdä synkronoinnin Soita tällä menetelmällä. Voit määrittää takaisinsoiton null tai käyttää acquireTokenSilentSync.

### <a name="diagnostics"></a>Vianmääritys

Lisätietoja ohjelmistossa ongelmat ensisijainen lähteitä ovat seuraavat:

+ Poikkeukset
+ Lokit
+ Verkon jälkitiedot

Huomaa myös, korrelaatio tunnukset on keskitetyn diagnostiikka-kirjastossa. Voit määrittää oman korrelaatio tunnukset kohti pyynnön välein, jos haluat yhdistää ADAL pyytää koodissa muiden toimintojen kanssa. Jos et määritä korrelaatioita tunnus sitten ADAL Luo satunnainen jokin and kaikki lokin viestejä ja verkko-puhelut leimata korrelaatiotunnus. Itse tehtyä muutoksia jokaisen pyynnön.

#### <a name="exceptions"></a>Poikkeukset

Tämä on selvästi ensimmäisen Diagnostiikka. Pyrimme tarjoamaan hyödyllisiä virhesanomia. Jos löydät sellaisen, joka ei ole hyödyllisiä tiedoston Tarkista ongelma ja kerro meille, mistä. Anna laitteen tiedot, kuten mallin ja SDK # myös.

#### <a name="logs"></a>Lokit

Voit määrittää kirjaston luo lokitiedoston viestit, joiden avulla voit vianmääritys. Kirjauksen määrittäminen tekemällä seuraavat puhelun takaisinsoiton, jota ADAL käyttää toimita log jokaisen viestin käytöstä, kun se luodaan.


 ```Java
 Logger.getInstance().setExternalLogger(new ILogger() {
     @Override
     public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
      ...
      // You can write this to logfile depending on level or errorcode.
      writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
     }
 }
 ```
Viestit voidaan kirjoittaa mukautetun lokitiedoston tarkastelu alapuolella. Valitettavasti tällä ei vakio voi saada lokit laitteesta. Jotkin palvelut, jotka auttavat sinua tällä on. Voit myös kuvaava Omat, kuten lähettämällä tiedosto palvelimeen.

```Java
private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
}
```

##### <a name="logging-levels"></a>Kirjaamisen tasot

+ Error(Exceptions)
+ Warn(Warning)
+ Tiedot (tiedoksi)
+ Yksityiskohtainen (Lisätietoja)

Voit määrittää lokiin tason tältä:
```Java
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);
 ```

 Log-viestit lähetetään logcat lisäksi kaikki mukautetut log takaisinkutsuja.
Saat tiedoston lomake-logcat lokin näkyvän belog:

 ```
  adb logcat > "C:\logmsg\logfile.txt"
 ```
 Lisää esimerkkejä tietoja adb cmds: https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat

#### <a name="network-traces"></a>Verkon Jälkitiedot

Voit ottaa HTTP-liikenne, joka luo ADAL eri työkaluja.  Tämä on hyödyllinen, jos olet aiemmin OAuth-protokollan kanssa tai jos haluat antaa vianmääritystiedot Microsoft tai tuki muiden lähteiden.

Fiddler on helpoin HTTP seurantatyökalu.  Seuraavissa linkeissä avulla voit asetukset oikein tietueen ADAL verkkoliikennettä ylöspäin.  Hyödylliset on määritettävä fiddler tai muuta työkalua, kuten Charles, voit tallentaa salaamaton SSL-liikenne.  Huomautus: Tällä tavalla luodut jäljittää voi olla erittäin sellaisten tietoja, kuten access-tunnuksia, käyttäjänimet ja salasanat.  Jos käytössäsi on tuotannon tilit, Jaa nämä jäljittää 3 osapuolten kanssa.  Jos haluat antaa toiselle jäljityksen jotta saat tuki tilapäinen tilillä käyttäjänimet ja salasanat, muistat, jakaminen ja ongelman luominen uudelleen.

+ [Fiddler For Android-sovelluksen määrittäminen](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
+ [ADAL Fiddler sääntöjen määrittäminen](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)


### <a name="dialog-mode"></a>Valintaikkunan tila
acquireToken menetelmä ilman tehtävän tukee valintaikkuna tulee näkyviin.

### <a name="encryption"></a>Salaus

ADAL salaa tunnusten ja SharedPreferences säilytetään oletusarvoisesti. Voi tarkastella tietojasi StorageHelper-luokka. Android-sovelluksen käyttöön AndroidKeyStore 4.3(API18) suojattuun säilöön, yksityiset avaimet. ADAL käyttää sitä API18 ja edellä. Jos haluat käyttää ADAL alemman SDK-versiot, sinun on antamaan salausavaimen osoitteessa AuthenticationSettings.INSTANCE.setSecretKey

### <a name="oauth2-bearer-challenge"></a>Oauth2 haltijan todennus

Toiminnot authorization_uri käyttämistä Oauth2 haltijan todennus tarjoaa AuthenticationParameters luokka.

### <a name="session-cookies-in-webview"></a>Istunnon evästeet-näkymä

Android näkymä ei poista istunnon evästeet, kun sovellus suljetaan. Voit käsitellä otoksen koodilla alla:
```java
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
Lisätietoja evästeet: http://developer.android.com/reference/android/webkit/CookieSyncManager.html

### <a name="resource-overrides"></a>Resurssin ohittaa

ADAL kirjasto sisältää seuraavat kaksi ProgressDialog viestit englanninkielisiä.

Sovelluksen pitäisi korvata lokalisoidut merkkijonot tarvittaessa.

```Java
<string name="app_loading">Loading...</string>
<string name="broker_processing">Broker is processing</string>
<string name="http_auth_dialog_username">Username</string>
<string name="http_auth_dialog_password">Password</string>
<string name="http_auth_dialog_title">Sign In</string>
<string name="http_auth_dialog_login">Login</string>
<string name="http_auth_dialog_cancel">Cancel</string>
```

=======

### <a name="ntlm-dialog"></a>NTLM-valintaikkuna
Adal versio 1.1.0 tukee NTLM-valintaikkuna, joka käsitellään onReceivedHttpAuthRequest tapahtuman WebViewClient kautta. Valintaikkunan asettelu ja merkkijonojen voi mukauttaa.

### <a name="cross-app-sso"></a>Usean sovelluksen SSO
Opettele [rajat-sovelluksen SSO Android-käyttämällä ADAL ottaminen käyttöön](active-directory-sso-android.md)  


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
