<properties
    pageTitle="Android sovellusten Azure Mobile välitys käytön aloittaminen"
    description="Opettele käyttämään Azure Mobile välitys Android-sovellusten kanssa analyysin ja push-ilmoitukset."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Android-sovellusten Azure Mobile välitys aloittaminen

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Tässä aiheessa kerrotaan Azure Mobile välitys käyttämisestä saat lisätietoja sovelluksen käytön ja lähettäminen push-ilmoitukset Segmentoitu Android-sovelluksen käyttäjille.
Tässä opetusohjelmassa kuvataan käyttämällä Mobile välitys yksinkertaisessa lähetyksen skenaariossa. Ei voit luoda tyhjän Android-sovelluksen, joka kerää perustiedot ja vastaanottaa push-ilmoitusten käyttäminen Google Cloud Messaging (GCM).

## <a name="prerequisites"></a>Edellytykset

Olet suorittanut tämän opetusohjelman edellyttää [Android Kehitystyökalut](https://developer.android.com/sdk/index.html), joka sisältää Android Studiossa integroitua ja uusimmat Android-ympäristössä.

Se vaatii myös [Mobile välitys Android SDK-paketissa](https://aka.ms/vq9mfn).

> [AZURE.IMPORTANT] Viimeistele Tässä opetusohjelmassa, sinun on aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Välitys Mobile for Android-sovelluksen määrittäminen

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

Tässä opetusohjelmassa näkyy "basic integrointi", joka on vähimmäismäärää, tietojen kerääminen ja Lähetä push-ilmoitus. Luot tavallisen-sovelluksen Android Studiossa osoittamaan integrointi.

Täydellinen integrointi asiakirjat löytyvät [Mobile välitys Android SDK integrointi](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Android projektin luominen

1. Käynnistä **Android Studio**ja valitse Ponnahdusvalikossa kohdan **Android Studio uuden projektin aloittaminen**.

    ![][1]

2. Anna sovelluksen nimen ja yrityksen toimialueen. Pane merkille mitä täytettävien, sillä tarvitset sitä myöhemmin. Valitse **Seuraava**.

    ![][2]

3. Valitse kohde lomakkeessa kerroin ja API taso ja valitse **Seuraava**.

    >[AZURE.NOTE] Mobile välitys tarvitaan vaatii tason vähintään 10 (Android 2.3.3).

    ![][3]

4. Valitse **Tyhjä tehtävän** , joka on tämän sovelluksen vain näytössä ja valitse **Seuraava**.

    ![][4]

5. Jätä oletusarvot ja valitse **Valmis**.

    ![][5]

Android Studio luoma esittely-sovellus, johon on integroida Mobile välitys.

### <a name="include-the-sdk-library-in-your-project"></a>Sisällytä projektin SDK-kirjasto

1. Lataa [Mobile välitys Android SDK](https://aka.ms/vq9mfn).
2. Pura arkistotiedoston tietokoneessa olevaan kansioon.
3. Määritä SDK nykyisen version .jar kirjasto ja kopioi se Leikepöydälle.

      ![][6]

4. Siirry osaan, **Project** (1) ja liittää .jar kirjastoa-kansio (2).

      ![][7]

5. Jos haluat ladata kirjastoon, synkronoida projektin.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Sovelluksen yhdistäminen yhteysmerkkijonolla Taustajärjestelmä Mobile välitys

1. Kopioi seuraavat koodin rivit (täytyy tehdä vain yhdessä paikassa sovelluksesi yleensä tärkeimmät tehtävän) tehtävän luominen. Esimerkki-sovelluksen MainActivity src-kohdassa voit avata -> main -> java-kansio ja lisää seuraava teksti:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. Ratkaise viittaukset painamalla Alt + Enter tai tuo seuraavista väittämistä lisääminen:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Siirry Azure perinteinen portaalin sinua sovelluksen **Yhteyden tietojen** sivulla ja kopioi **Yhteysmerkkijono**.

      ![][9]

4. Liittää sen sovellukseen `setConnectionString` parametrin korvaaminen koko merkkijonon näkyy seuraava koodi:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Lisää käyttöoikeuksia ja service-ilmoitus

1. Lisää nämä oikeudet projektin Manifest.xml välittömästi ennen tai jälkeen `<application>` tunniste:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Määritellä agent-palvelu, lisää koodi välillä `<application>` ja `</application>` tunnisteet:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. Korvaa liittämäsi koodia `"<Your application name>"` otsikossa, jossa näkyy **asetukset** -valikossa kohtaa näet palveluita laitteeseen. Voit lisätä word-palvelun"merkin esimerkiksi.

### <a name="send-a-screen-to-mobile-engagement"></a>Lähetä näyttöön Mobile välitys

Jos haluat lisätä tietoja ja varmistaa, että käyttäjät ovat käytössä, voit lähettää vähintään yksi näyttö (tehtävä) Mobile välitys Taustajärjestelmä.

Siirry **MainActivity.java** ja lisää korvaa **MainActivity** **EngagementActivity**, perus luokan seuraavasti:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Jos peruste luokan ei ole *tehtävä*, ota yhteyttä siitä, miten eri luokkien periytyvät [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) .


Lisää kommentti tämän yksinkertaisen otoksen skenaarion seuraavan rivin pois:

    // setSupportActionBar(toolbar);

Jos haluat säilyttää `ActionBar` sovelluksessa, katso [Advanced Android raportointia](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes).

## <a name="connect-app-with-real-time-monitoring"></a>Yhdistä app reaaliaikainen seuranta

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Ota käyttöön push-ilmoitukset ja sovelluskohtaisesta viestintä

Markkinointikampanja-aikana Mobile välitys avulla voit käsitellä ja SAAVUTTAA push-ilmoitukset ja sovelluskohtaisesta messaging käyttäjiä. Tämä moduuli kutsutaan REACH Mobile välitys-portaalissa.
Seuraavassa osassa määrittää sovelluksen vastaanottamaan ne.

### <a name="copy-sdk-resources-in-your-project"></a>Kopioi projektin SDK-resurssit

1. Siirry takaisin SDK-Lataa sisältö ja kopioi **tarkkuus** -kansio.

    ![][10]

2. Siirry takaisin Android Studiossa, valitse **projektin tiedostojen päähakemisto** ja liitä se resurssien lisääminen projektiin.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Siirry [Android SDK](mobile-engagement-android-sdk-overview.md) saat yksityiskohtaiset tietosi SDK-integrointi.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
