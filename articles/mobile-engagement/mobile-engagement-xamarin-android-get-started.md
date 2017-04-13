<properties
    pageTitle="Azure for Xamarin.Android Mobile välitys käytön aloittaminen"
    description="Opettele käyttämään Azure Mobile välitys Xamarin.Android sovellusten Analytics ja Push-ilmoitukset."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Azure Mobile välitys Xamarin.Android-sovellusten käytön aloittaminen

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Tässä aiheessa kerrotaan poistamisesta Azure Mobile välitys avulla voit selvittää sovelluksen käytön ja Lähetä push-ilmoitukset Segmentoitu Xamarin.Android sovelluksen käyttäjille.
Tässä opetusohjelmassa kuvataan käyttämällä Mobile välitys yksinkertaisessa lähetyksen skenaariossa. Ei voit luoda tyhjän Xamarin.Android-sovellusta, joka kerää perustiedot ja vastaanottaa push-ilmoitusten käyttäminen Google Cloud Messaging (GCM).

Tässä opetusohjelmassa edellyttää seuraavaa:

+ [Xamarin Studio](http://xamarin.com/studio). Voit myös käyttää Visual Studio Xamarin kanssa, mutta tässä opetusohjelmassa käyttää Xamarin Studio. Katso asennusohjeet [asennus ja asenna Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ [Mobiili välitys Xamarin SDK-paketissa](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).

##<a id="setup-azme"></a>Määritä Mobile for Android-sovelluksen välitys

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

Tässä opetusohjelmassa näkyy "basic integrointi", joka on vähimmäismäärää, tietojen kerääminen ja Lähetä push-ilmoitus. 

Luodaan basic sovelluksen Xamarin Studiossa osoittamaan integrointi.

###<a name="create-a-new-xamarinandroid-project"></a>Xamarin.Android uuden projektin luominen

1. Julkaisun **Xamarin Studio** valitsemalla **Tiedosto** -> **uuden** -> **ratkaisu** 

    ![][1]

2. Valitse **Android-sovelluksen** ja varmista, että valitulla kielellä on **C#** ja valitse **Seuraava**.

    ![][2]

3. Kirjoita **Sovelluksen nimi** ja **organisaation tunnus**. Varmista, että valintamerkiksi **Google Play-palvelujen** ja valitse sitten **Seuraava**. 

    ![][3]
    
4. Päivitä **Projektinimi**, **Ratkaisun nimi** ja **sijainti** tarvittaessa ja valitse **Luo**.

    ![][4]
 
Xamarin Studio Luo sovellus, jossa on integroida Mobile välitys. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

1. Ratkaisu-Windowsin **pakettien** -kansion hiiren kakkospainikkeella ja valitse **Lisää pakettien...**

    ![][5]

2. **Microsoft Azure Mobile välitys Xamarin SDK** Etsi ja lisää se ratkaisu.  

    ![][6]
   
3. Avaa **MainActivity.cs** ja lisää seuraava lauseiden käyttämisestä:

        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;

4. Valitse `OnCreate` -menetelmä lisää seuraava alustaa Mobile välitys Taustajärjestelmä yhteyttä. Varmista, että voit lisätä oman **ConnectionString**. 

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

###<a name="add-permissions-and-a-service-declaration"></a>Lisää käyttöoikeuksia ja service-ilmoitus

1. Avaa **Manifest.xml** -tiedoston ominaisuudet-kansiossa. Valitse lähde-välilehti, jotta voit päivittää suoraan XML-lähde.
 
2. Lisää nämä oikeudet Manifest.xml (joka löytyy kohdasta **Ominaisuudet** -kansion) projektin välittömästi ennen tai jälkeen `<application>` tunniste:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

3. Lisää seuraava välillä `<application>` ja `</application>` tunnisteen, kun haluat määritellä agent-palvelu:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

4. Vain liittämäsi koodissa korvaa `"<Your application name>"` otsikossa. Tämä näkyy **asetukset** -valikko, jossa käyttäjät voivat tarkastella palveluita laitteeseen. Voit lisätä word-palvelun"merkin esimerkiksi.

###<a name="send-a-screen-to-mobile-engagement"></a>Lähetä näyttöön Mobile välitys

Jotta voit aloittaa lähettävät tietoja ja varmistaa, että käyttäjät ovat käytössä, voit lähettää vähintään yhden näytön Mobile välitys Taustajärjestelmä. Näin saat-varmistaa, että `MainActivity` perii `EngagementActivity` sijaan `Activity`.

    public class MainActivity : EngagementActivity
    
Voit myös jos ei peri `EngagementActivity` sitten lisättävä `.StartActivity` ja `.EndActivity` esitetyssä `OnResume` ja `OnPause` tarpeen mukaan.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }
    
            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

##<a id="monitor"></a>Yhdistä app reaaliaikainen seuranta

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ota käyttöön push-ilmoitukset ja sovelluskohtaisesta viestintä

Mobile välitys voit käsitellä ja SAAVUTTAA käyttäjien push-ilmoitukset ja sovelluksen messaging Kampanjat kontekstissa. Tämä moduuli kutsutaan REACH Mobile välitys-portaalissa.
Seuraavissa osissa määrittää sovelluksen vastaanottamaan ne.

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
