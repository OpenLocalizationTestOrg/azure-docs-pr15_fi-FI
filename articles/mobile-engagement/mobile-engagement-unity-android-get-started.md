<properties
    pageTitle="Aloita Azure Mobile välitys yhtenäisyyden Android käyttöönottoa varten"
    description="Opettele käyttämään Azure Mobile välitys Analytics ja Push-ilmoitukset yhtenäisyyden sovellusten käyttöönoton iOS-laitteita."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Aloita Azure Mobile välitys yhtenäisyyden Android käyttöönottoa varten

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Tässä ohjeaiheessa kerrotaan, miten Azure Mobile välitys avulla voit selvittää sovelluksen käytön ja lähettämisestä push-ilmoituksia, jotka Segmentoitu yhtenäisyyden sovelluksen käyttäjille, kun otat käyttöön Android-laitteessa.
Tässä opetusohjelmassa käyttää perinteinen yhtenäisyyden aikaisempi pallo opetusohjelma aloituspisteenä. Sinun pitäisi noudattamalla tässä [opetusohjelmassa](mobile-engagement-unity-roll-a-ball.md) ennen jatkamista Mobile välitys integrointi on korostaa alla opetusohjelmassa. 

Tässä opetusohjelmassa edellyttää seuraavaa:

+ [Yhtenäisyyden-editori](http://unity3d.com/get-unity)
+ [Mobiili välitys yhtenäisyyden SDK-paketissa](https://aka.ms/azmeunitysdk)
+ Google-Android SDK-paketissa

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).

##<a id="setup-azme"></a>Määritä Mobile for Android-sovelluksen välitys

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

###<a name="import-the-unity-package"></a>Tuo yhtenäisyyden-paketti

1. Lataa [Mobile välitys yhtenäisyyden paketin](https://aka.ms/azmeunitysdk) ja tallentaa sen paikallisessa tietokoneessa. 

2. Siirry **kalusto-pakkauksen tuominen > mukautettu paketti ->** ja valitse paketti, voit ladata edellä vaiheessa. 

    ![][70] 

3. Varmista, että kaikki tiedostot ovat valittuina, ja valitse sitten **Tuo** -painike. 

    ![][71] 

4. Kun tuonti on tarkistettu, näet tuodut SDK-tiedostot projektin.  

    ![][72] 

###<a name="update-the-engagementconfiguration"></a>Päivitä EngagementConfiguration

1. SDK-kansio ja Päivitä **EngagementConfiguration** komentosarja-tiedoston avaamista **ANDROID\_yhteyden\_MERKKIJONON** hankit aiemmin Azure-portaalista yhteysmerkkijonon kanssa.  

    ![][73]

2. Tallenna tiedosto 

3. Suorita **Tiedosto -> välitys -> Luo Android luettelo**. Tämä on Mobile välitys SDK lisäämät laajennus ja valitsemalla se päivittyy automaattisesti project-asetukset. 

    ![][74]

> [AZURE.IMPORTANT] Varmista, että voit suorittaa tämän aina, kun päivität **EngagementConfiguration** tiedoston muutoin tekemäsi muutokset eivät näy-sovelluksessa. 

###<a name="configure-the-app-for-basic-tracking"></a>Määritä sovellus basic seurantaa varten

1. Avaa määrittänyt **PlayerController** komentosarjan muokattavaksi Player-objekti liitetään. 

2. Lisää seuraava lauseella:

        using Microsoft.Azure.Engagement.Unity;

3. Lisää seuraavat `Start()` menetelmä
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Ottaa käyttöön ja suorita sovellus
Varmista, että sinulla on asennettu tietokoneeseesi, ennen kuin yrität yhtenäisyyden sovelluksen käyttöön laitteen Android SDK. 

1. Yhdistä Android-laite tietokoneeseesi. 

2. Avaa **Tiedosto -> Muodosta asetusten** määrittäminen 

    ![][40]

3. Valitse **Android** ja valitse sitten **Vaihda** -ympäristössä

    ![][51]

    ![][52]

4. Valitse **toisto-ohjelman asetukset** ja Anna kelvollinen Pikaoppaista tunnus. 

    ![][53]

5. Valitse lopuksi **Luominen ja suorittaminen**

    ![][54]

6. Sinulta pyydetään antamaan tallentamiseen Android paketin kansionimi. 

7. Jos kaikki kunnossa, paketti otetaan käyttöön yhdistetyn laitteen ja pitäisi näkyä yhtenäisyyden tietotekniset taitosi puhelimessa! 

##<a id="monitor"></a>Yhdistä app reaaliaikainen seuranta

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ota käyttöön push-ilmoitukset ja sovelluskohtaisesta viestintä

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###<a name="update-the-engagementconfiguration"></a>Päivitä EngagementConfiguration

1. SDK-kansio ja Päivitä **EngagementConfiguration** komentosarja-tiedoston avaamista **ANDROID\_GOOGLE\_numero** **Google projektinumero** hankit aiemmin Google Cloud Kehitystyökalut-portaalista. Tämä on merkkijonon arvon joten varmista, että se on kirjoitettava lainausmerkkeihin. 

    ![][75]

2. Tallenna tiedosto. 

3. Suorita **Tiedosto -> välitys -> Luo Android luettelo**. Tämä on Mobile välitys SDK lisäämät laajennus ja valitsemalla se päivittyy automaattisesti project-asetukset. 

    ![][74]

###<a name="configure-the-app-to-receive-notifications"></a>Määritä sovellus-ilmoitusten vastaanottotilanteiden

1. Avaa määrittänyt **PlayerController** komentosarjan muokattavaksi Player-objekti liitetään. 

2. Lisää seuraavat `Start()` menetelmä

        EngagementReachAgent.Initialize();

3. Nyt, kun sovellus päivitetään, ottaminen käyttöön ja suorita sovellus laitteessa kohti jäljempänä annettuja ohjeita. 

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
