<properties
    pageTitle="Aloita Azure Mobile välitys yhtenäisyyden iOS käyttöönottoa varten"
    description="Opettele käyttämään Azure Mobile välitys Analytics ja Push-ilmoitukset yhtenäisyyden sovellusten käyttöönoton iOS-laitteita."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Aloita Azure Mobile välitys yhtenäisyyden iOS käyttöönottoa varten

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Tässä ohjeaiheessa kerrotaan, miten Azure Mobile välitys avulla voit selvittää sovelluksen käytön ja lähettämisestä push-ilmoituksia, jotka Segmentoitu yhtenäisyyden sovelluksen käyttäjille, kun otat käyttöön iOS-laitteessa.
Tässä opetusohjelmassa käyttää perinteinen yhtenäisyyden aikaisempi pallo opetusohjelma aloituspisteenä. Sinun pitäisi noudattamalla tässä [opetusohjelmassa](mobile-engagement-unity-roll-a-ball.md) ennen jatkamista Mobile välitys integrointi on korostaa alla opetusohjelmassa. 

Tässä opetusohjelmassa edellyttää seuraavaa:

+ [Yhtenäisyyden-editori](http://unity3d.com/get-unity)
+ [Mobiili välitys yhtenäisyyden SDK-paketissa](https://aka.ms/azmeunitysdk)
+ XCode-editori

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).

##<a id="setup-azme"></a>IOS-sovelluksen asennuksen Mobile välitys

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

1. SDK-kansio ja Päivitä **EngagementConfiguration** komentosarja-tiedoston avaamista **IOS\_yhteyden\_MERKKIJONON** hankit aiemmin Azure-portaalista yhteysmerkkijonon kanssa.  

    ![][73]

2. Tallenna tiedosto. 

###<a name="configure-the-app-for-basic-tracking"></a>Määritä sovellus basic seurantaa varten

1. Avaa määrittänyt **PlayerController** komentosarjan muokattavaksi Player-objekti liitetään. 

2. Lisää seuraava lauseella:

        using Microsoft.Azure.Engagement.Unity;

3. Lisää seuraavat `Start()` menetelmä
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Ottaa käyttöön ja suorita sovellus

1. Yhdistä iOS-laite tietokoneeseesi. 

2. Avaa **Tiedosto -> Muodosta asetusten** määrittäminen 

    ![][40]

3. Valitse **iOS** ja valitse sitten **Vaihda** -ympäristössä

    ![][41]

    ![][42]

4. Valitse **toisto-ohjelman asetukset** ja Anna kelvollinen Pikaoppaista tunnus. 

    ![][53]

5. Valitse lopuksi **Luominen ja suorittaminen**

    ![][54]

6. Sinulta pyydetään antamaan tallentamiseen iOS-paketin kansionimi. 

    ![][43]

7. Jos kaikki kunnossa, projektin laaditaan ja kannattaa avata sitä XCode-sovelluksessa. 

8. Varmista, että **Yhdistä tunniste** on oikea projektissa.  

    ![][75]

10. Suorita nyt sovellus XCode niin, että paketin otetaan käyttöön yhdistetyn laitteen ja pitäisi näkyä yhtenäisyyden tietotekniset taitosi puhelimessa! 

##<a id="monitor"></a>Yhdistä app reaaliaikainen seuranta

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ota käyttöön push-ilmoitukset ja sovelluskohtaisesta viestintä

Mobile välitys avulla voit käsitellä käyttäjille ja SAAVUTTAA push-ilmoitukset ja sovelluksen messaging Kampanjat kontekstissa. Tämä moduuli kutsutaan REACH Mobile välitys-portaalissa.
Sinun ei tarvitse tehdä muita määrityksiä sovelluksen saat ilmoituksen, ja jos se on jo sen asetukset.

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
