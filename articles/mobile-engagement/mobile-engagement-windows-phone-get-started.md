<properties
    pageTitle="Azure Mobile välitys for Windows Phone Silverlight-sovellusten käytön aloittaminen"
    description="Opettele käyttämään Azure Mobile välitys Windows Phone Silverlight-sovellusten kanssa analyysin ja push-ilmoitukset."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Azure Mobile välitys for Windows Phone Silverlight-sovellusten käytön aloittaminen

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Tässä ohjeaiheessa esitellään käyttämisestä Azure Mobile välitys ymmärtää sovelluksen käytön ja Lähetä push-ilmoitukset Segmentoitu Windows Phone Silverlight-sovelluksen käyttäjille.
Tässä opetusohjelmassa kuvataan käyttämällä Mobile välitys yksinkertaisessa lähetyksen skenaariossa. Ei voit luoda tyhjän Windows Phone Silverlight-sovellusta, joka kerää perustiedot ja vastaanottaa push-ilmoitukset käyttämällä Microsoftin Push ilmoituksen Service (MPNS).

> [AZURE.NOTE] Jos kohdennat Windows Phone 8.1 (-Silverlight), tutustu [Windows yleinen opetusohjelma](mobile-engagement-windows-store-dotnet-get-started.md).

Tässä opetusohjelmassa edellyttää seuraavaa:

+ Visual Studio 2013
+ [MicrosoftAzure.MobileEngagement] Nuget paketti

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).

##<a id="setup-azme"></a>Asennuksen Mobile välitys, että Windows Phone-sovelluksen

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

Tässä opetusohjelmassa näkyy "basic integrointi", joka on vähimmäismäärää, tietojen kerääminen ja Lähetä push-ilmoitus. Täydellinen integrointi asiakirjat löytyvät [Mobile välitys Windows Phone SDK-integrointi](mobile-engagement-windows-phone-sdk-overview.md)

Luodaan basic app Visual Studiossa osoittamaan integrointi.

###<a name="create-a-new-windows-phone-silverlight-project"></a>Windows Phone Silverlight uuden projektin luominen

Seuraavissa vaiheissa oletetaan Visual Studio 2015 käyttöä, vaikka vaiheet ovat samanlaiset Visual Studio aiemmissa versioissa. 

1. Käynnistä Visual Studio ja valitse **Aloitus** -ruudussa **Uusi projekti**.

2. Valitse Ponnahdusvalikossa kohdan **Windows 8: n** -> **Windows Phone** -> **Tyhjästä sovelluksesta (Windows Phone Silverlight)**. Kirjoita sovelluksen **nimi** ja **ratkaisun nimeä**ja valitse sitten **OK**.

    ![][1]

3. Voit kohdistaa **Windows Phone 8.0** tai **Windows Phone 8. 1**.

Olet nyt luonut uuden Windows Phone Silverlight-sovelluksen, johon olemme integroida Azure Mobile välitys SDK-paketissa.

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

1. Asenna [MicrosoftAzure.MobileEngagement] nuget paketti projektin.

2. Avaa `WMAppManifest.xml` (Valitse ominaisuudet-kansio) ja varmista, että seuraavat ilmoitetaan (lisää ne jos ne eivät ole)- `<Capabilities />` tunniste:

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. Liitä yhteysmerkkijonon, joka on kopioitu aiemmin välitys Mobile-sovelluksen ja liitä se nyt `Resources\EngagementConfiguration.xml` tiedostojen välillä `<connectionString>` ja `</connectionString>` tunnisteet:

    ![][3]

4. Valitse `App.xaml.cs` tiedosto:

    a. Lisää `using` lause:

            using Microsoft.Azure.Engagement;

    b. Alusta-SDK `Application_Launching` menetelmää:

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c-näppäinyhdistelmää. Lisää seuraavat toimet `Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a id="monitor"></a>Ota reaaliaikainen seuranta

Jotta voit aloittaa lähettävät tietoja ja varmistaa, että käyttäjät ovat käytössä, voit lähettää vähintään yksi näyttö (tehtävä) Mobile välitys Taustajärjestelmä.

1. Lisää MainPage.xaml.cs, `using` lause:

        using Microsoft.Azure.Engagement;

2. Korvaa **MainPage**, joka on ennen **PhoneApplicationPage**, perus luokan **EngagementPage**.

        class MainPage : EngagementPage 
    
3. Valitse oman `MainPage.xml` tiedosto:

    a. Lisää nimitilan ilmoitukset:

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b. Korvaa `phone:PhoneApplicationPage` XML-tunnisteen nimi Valitse `engagement:EngagementPage`.

##<a id="monitor"></a>Yhdistä app reaaliaikainen seuranta

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ota käyttöön push-ilmoitukset ja sovelluskohtaisesta viestintä

Mobile välitys voit olla vuorovaikutuksessa ja saavuttaa käyttäjien Push-ilmoitukset ja sovelluskohtaisesta viestit Kampanjat kontekstissa. Tämä moduuli kutsutaan REACH Mobile välitys-portaalissa.
Seuraavissa osissa on määritetty vastaanottamaan ne sovelluksen.

###<a name="enable-your-app-to-receive-mpns-push-notifications"></a>Ottaa käyttöön sovelluksen MPNS Push-ilmoitusten vastaanottotilanteiden

Lisää uusia ominaisuuksia, jotka oman `WMAppManifest.xml` tiedosto:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

###<a name="initialize-the-reach-sdk"></a>Alusta REACH SDK-paketissa

1. Valitse `App.xaml.cs`, soita `EngagementReach.Instance.Init();` **Application_Launching** -funktiossa oikealle agentti alustamisen jälkeen:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. Valitse `App.xaml.cs`, soita `EngagementReach.Instance.OnActivated(e);` **Application_Activated** -funktiossa oikealle agentti alustamisen jälkeen:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Olet valmis. On nyt tarkistamaan, että sinulla on oikein Pukki basic integrointi ulos.

##<a id="send"></a>Lähettää ilmoituksen sovelluksen

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Pitäisi tulla näkyviin ilmoituksen laitteessasi, jossa näkyy muodossa-sovelluksen ilmoituksella Jos sovellus on avoinna muussa ilmoitusruudun, kuten jompikumpi seuraavista: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
