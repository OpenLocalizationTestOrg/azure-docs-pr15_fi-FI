<properties
    pageTitle="Aloita Azure palvelun mobiilisovellukset Xamarin.iOS sovellusten | Microsoft Azure"
    description="Katso tämä opetusohjelma, Aloita Mobile-sovellusten käyttämisestä Xamarin.iOS kehittämiseen."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#<a name="create-a-xamarinios-app"></a>Xamarin.iOS sovelluksen luominen

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa näytetään pilvipohjainen Taustajärjestelmä palvelun lisäämisestä Xamarin.iOS mobiilisovelluksessa käyttämällä Azure mobiilisovelluksen Taustajärjestelmä.  Voit luoda uuden mobiilisovelluksen taustassa ja yksinkertainen _Todo luettelon_ Xamarin.iOS-sovellusta, joka tallentaa tiedot sovelluksen Azure.

Noudata tässä opetusohjelmassa on kaikki muut Xamarin.iOS opetusohjelmat mobiilisovellukset-toiminnon käyttämisestä Azure App palvelun.

##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat edellytykset:

* Azure active tili. Jos sinulla ei ole tiliä, Azure-kokeiluversioon rekisteröityminen ja enintään 10 vapaa-mobiilisovellusten hankkiminen, voit jatkaa senkin jälkeen, kun kokeiluversion päättyy. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio Xamarin kanssa. Saat ohjeet [asennus ja asenna Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

* Mac Xcode v7.0 tai uudempi versio ja Xamarin Studio yhteisön asennettuna. Katso [asennus ja asenna Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) ja [asennuksen ja asennuksen tarkastukset for Mac-käyttäjän](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE]Jos haluat aloittaa Azure App palvelun, ennen kuin kirjaudut Azure-tili, siirry [Yritä App kääntäjä](https://tryappservice.azure.com/?appServiceName=mobile). Voit luoda lyhytkestoinen starter mobiilisovelluksessa heti sovelluksen käytössä, ei ole luottokortti ja ei ole sitoumukset.

## <a name="create-an-azure-mobile-app-backend"></a>Luo Azure Mobile-sovelluksen taustaan

Luo Mobile-sovelluksen taustassa seuraavasti.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Määritä server-projekti

Voit nyt valmisteltu Azure Mobile-sovelluksen-taustatietokannan, joka voidaan käyttää kaikkia mobiilisovelluksen-sovellusten ominaisuuksia. Lataa seuraavaksi yksinkertainen "todo luettelon" palvelimen projektille Taustajärjestelmä ja julkaista sen Azure.

Seuraavat ohjeiden avulla voit määrittää palvelimen Projectin käyttämään Node.js tai .NET Taustajärjestelmä.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Lataa ja suorita Xamarin.iOS-sovellus

1. Avaa [Azure portal] -selainikkunassa.

2. Valitse Mobile-sovelluksen asetukset-sivu, **Aloita** > **Xamarin.iOS**. Valitse vaiheessa 3 **Luo uuden sovelluksen** , jos se ei ole jo avattuna.  Seuraavaksi napsauttamalla **Lataa** -painiketta.

    Asiakassovellus, joka yhdistää mobile Taustajärjestelmä ladataan. Pakattu projektitiedoston tallentaminen omaan tietokoneeseesi ja Merkitse muistiin, johon tallennat sen.

3. Pura projekti, jota olet ladannut ja avaa se sitten Xamarin Studiossa (tai Visual Studio).

    ![][9]

    ![][8]

4. Paina muodosta projektin ja Käynnistä sovellus iPhone emulaattori F5-näppäintä.

5. -Sovellukseen, kirjoita kuvaileva teksti, _Lue Xamarin_, kuten ja valitse sitten **+** painiketta.

    ![][10]

    Pyynnön tiedot lisätään TodoItem-taulukkoon. Kohteet, jotka on tallennettu taulukkoon palauttamat mobiilisovelluksen Taustajärjestelmä ja tiedot näytetään luettelossa.

>[AZURE.NOTE]Voit tarkastella koodia, joka noutaa mobiilisovelluksen-taustatietokannan kysely ja lisää tiedot QSTodoService.cs C#-tiedostossa.

##<a name="next-steps"></a>Seuraavat vaiheet

* [Offline-synkronoinnin lisääminen sovelluksen](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Käyttöoikeuksien lisääminen sovelluksen](app-service-mobile-xamarin-ios-get-started-users.md)
* [Push-ilmoitukset lisääminen Xamarin.Android-sovellukseen](app-service-mobile-xamarin-ios-get-started-push.md)
* [Miten voit käyttää hallitun asiakkaan Azure-mobiilisovellukset](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure portal]: https://portal.azure.com/
