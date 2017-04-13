<properties
    pageTitle="Azure mobiilisovellukset Xamarin.Android-sovellusten käytön aloittaminen"
    description="Katso tämä opetusohjelma Xamarin Android kehittämiseen Azure Mobile-sovellusten käytön aloittaminen"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />

#<a name="create-a-xamarinandroid-app"></a>Xamarin.Android sovelluksen luominen

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa näytetään, miten pilvipohjainen Taustajärjestelmä-palvelun lisääminen Xamarin.Android-sovellukseen. Lisätietoja on artikkelissa [mitä Mobile-sovellukset](app-service-mobile-value-prop.md).

Näyttökuva valmiin sovelluksen on alla:

![][0]

Noudata tässä opetusohjelmassa on Xamarin.Android sovellusten kaikkiin muihin Mobile-sovellusten opetusohjelmiin.

##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat edellytykset:

* Azure active tili. Jos sinulla ei ole tiliä, Azure kokeiluversion rekisteröityminen ja Hae enintään 10 vapaa Mobile-sovellukset. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio Xamarin kanssa. Saat ohjeet [asennus ja asenna Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

>[AZURE.NOTE]Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](https://tryappservice.azure.com/?appServiceName=mobile).  Voit luoda lyhytkestoinen starter Mobile-sovelluksen heti sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="create-an-azure-mobile-app-backend"></a>Luo Azure Mobile-sovelluksen taustaan

Luo Mobile-sovelluksen taustassa seuraavasti.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Voit nyt valmisteltu Azure Mobile-sovelluksen-taustatietokannan, joka voidaan käyttää kaikkia mobiilisovelluksen-sovellusten ominaisuuksia. Lataa seuraavaksi yksinkertainen "todo luettelon" palvelimen projektille Taustajärjestelmä ja julkaista sen Azure.

## <a name="configure-the-server-project"></a>Määritä server-projekti

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Lataa ja suorita Xamarin.Android-sovellus

1. Valitse **Lataa ja suorita Xamarin.Android projektin**-kohdassa **Lataa** -painiketta.

    Pakattu projektitiedoston tallentaminen omaan tietokoneeseesi ja Merkitse muistiin, johon tallennat sen.

2. Painamalla **F5** -näppäintä muodosta projektin ja Käynnistä sovellus.

3. -Sovellukseen Kirjoita kuvaava teksti, kuten _valmiina opetusohjelman_ ja valitse sitten **Lisää** -painiketta.

    ![][10]

    Pyynnön tiedot lisätään TodoItem-taulukkoon. Kohteet, jotka on tallennettu taulukkoon palauttamat mobiilisovelluksen Taustajärjestelmä ja tiedot näkyvät luettelossa.

    > [AZURE.NOTE] Voit tarkastella koodia, joka noutaa mobiilisovelluksen-taustatietokannan kysely ja lisää tiedot, joka löytyy ToDoActivity.cs C#-tiedostossa.

##<a name="next-steps"></a>Seuraavat vaiheet

* [Offline-synkronoinnin lisääminen sovelluksen](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Käyttöoikeuksien lisääminen sovelluksen](app-service-mobile-xamarin-android-get-started-users.md)
* [Push-ilmoitukset lisääminen Xamarin.Android-sovellukseen](app-service-mobile-xamarin-android-get-started-push.md)
* [Miten voit käyttää hallitun asiakkaan Azure-mobiilisovellukset](app-service-mobile-dotnet-how-to-use-client-library.md)


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
