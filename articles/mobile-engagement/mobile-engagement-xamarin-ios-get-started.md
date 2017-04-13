<properties
    pageTitle="Azure for Xamarin.iOS Mobile välitys käytön aloittaminen"
    description="Opettele käyttämään Azure Mobile välitys Xamarin.iOS sovellusten Analytics ja Push-ilmoitukset."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Azure Mobile välitys Xamarin.iOS-sovellusten käytön aloittaminen

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Tässä ohjeaiheessa esitellään käyttämisestä Azure Mobile välitys ymmärtää sovelluksen käytön ja lähettää push-ilmoitukset Segmentoitu käyttäjille Xamarin.iOS-sovelluksessa.
Tässä opetusohjelmassa voit luoda tyhjän Xamarin.iOS-sovellusta, joka kerää perustiedot ja vastaanottaa push-ilmoitusten käyttäminen Apple Push ilmoituksen järjestelmän (APN).

Tässä opetusohjelmassa edellyttää seuraavaa:

+ [Xamarin Studio](http://xamarin.com/studio). Voit myös käyttää Visual Studio Xamarin kanssa, mutta tässä opetusohjelmassa käyttää Xamarin Studio. Katso asennusohjeet [asennus ja asenna Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 
+ [Mobiili välitys Xamarin SDK-paketissa](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).

##<a id="setup-azme"></a>IOS-sovelluksen asennuksen Mobile välitys

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

Tässä opetusohjelmassa näkyy "basic integrointi" on välttämättömät määrittää tietojen kerääminen ja Lähetä push-ilmoitus.

Olemme luodaan basic sovelluksen Xamarin osoittamaan integrointi:

###<a name="create-a-new-xamarinios-project"></a>Xamarin.iOS uuden projektin luominen

1. Käynnistä Xamarin Studio. Valitse **Tiedosto** -> **uuden** -> **ratkaisu** 

    ![][1]

2. Valitse **Yksi näkymä-sovellus**, varmista, että valitulla kielellä on **C#** ja valitse sitten **Seuraava**.

    ![][2]

3. Kirjoita **Sovelluksen nimi** ja **Organisaation tunnus** ja valitse sitten **Seuraava**. 

    ![][3]

    > [AZURE.IMPORTANT] Varmista, että käytät myöhemmin iOS-sovelluksen käyttöönotto julkaisun profiilin käyttää sovelluksen-tunnus, joka vastaa täsmälleen kanssa Pikaoppaista tunniste on tässä. 

4. Päivitä **Projektinimi**, **Ratkaisun nimi** ja **sijainti** tarvittaessa ja valitse **Luo**.

    ![][4]
 
Xamarin Studio Luo esittely-sovellus, jossa on integroida Mobile välitys. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

1. Ratkaisu-Windowsin **pakettien** -kansion hiiren kakkospainikkeella ja valitse **Lisää pakettien...**

    ![][5]

2. **Microsoft Azure Mobile välitys Xamarin SDK** Etsi ja lisää se ratkaisu.  

    ![][6]
   
3. Avaa **AppDelegate.cs** ja lisää seuraava lauseella:

        using Microsoft.Azure.Engagement.Xamarin;

4. Lisää **FinishedLaunching** -menetelmä alustaa Mobile välitys Taustajärjestelmä yhteys seuraavasti. Varmista, että voit lisätä oman **ConnectionString**. Tämä koodi käyttää myös tyhjä **NotificationIcon** joka on lisännyt Mobile välitys SDK-paketissa, jonka haluat korvata. 

        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

##<a id="monitor"></a>Reaaliaikaisen seurannan ottaminen käyttöön

Jotta voit aloittaa lähettävät tietoja ja varmistaa, että käyttäjät ovat käytössä, voit lähettää vähintään yhden näytön Mobile välitys Taustajärjestelmä.

1. Avaa **ViewController.cs** ja lisää seuraava lauseella:

        using Microsoft.Azure.Engagement.Xamarin;

2. Korvaa luokan, josta `ViewController` perii `UIViewController` , `EngagementViewController`. 

##<a id="monitor"></a>Yhdistä app reaaliaikainen seuranta

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ota käyttöön push-ilmoitukset ja sovelluskohtaisesta viestintä

Mobile välitys avulla voit käsitellä käyttäjille ja SAAVUTTAA push-ilmoitukset ja sovelluksen messaging Kampanjat kontekstissa. Tämä moduuli kutsutaan REACH Mobile välitys-portaalissa.
Seuraavissa osissa on määritetty vastaanottamaan ne sovelluksen.

### <a name="modify-your-application-delegate"></a>Muokkaa sovelluksen edustajallasi

1. Avaa **AppDelegate.cs** ja lisää seuraava lauseella:

        using System; 

2. Nyt sisällä `FinishedLaunching` -menetelmä lisää seuraava Rekisteröidy push viestit jälkeen`EngagementAgent.init(...)`

        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }

3. Lopuksi - päivittää tai lisätä seuraavista tavoista:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }

4. **Info.plist** tiedoston ratkaisun Varmista, että **Pikaoppaista tunnus** vastaa **Sovellustunnus** valmistelu-profiili Apple keskihajonta keskelle. 

    ![][7]

5. Varmista, että olet valinnut **Käyttöön tausta-tila** ja **Remote ilmoitukset** **Info.plist** saman tiedoston. 

    ![][8]

6. Suorita sovellus julkaisun profiilia liittämäsi laitteeseen. 

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
