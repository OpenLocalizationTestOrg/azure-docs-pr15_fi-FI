<properties
    pageTitle="Aloita Mobile-sovellusten avulla Xamarin.Forms"
    description="Katso tämä opetusohjelma Xamarin.Forms kehittämiseen Azure Mobile-sovellusten käytön aloittaminen"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-xamarinforms-app"></a>Xamarin.Forms sovelluksen luominen

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa näytetään pilvipohjainen Taustajärjestelmä-palvelun lisääminen käyttämällä Azure Mobile-sovelluksen Taustajärjestelmä Xamarin.Forms mobiilisovelluksessa. Luo uusi Mobile-sovelluksen taustassa ja yksinkertainen _Todo luettelon_ Xamarin.Forms-sovellusta, joka tallentaa tiedot sovelluksen Azure.

Noudata tässä opetusohjelmassa on kaikki muut mobiilisovellukset opetusohjelmat Xamarin.Forms.

##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

* Azure active tili. Jos sinulla ei ole tiliä, voit Azure kokeiluversion rekisteröityminen ja saada enintään 10 ilmaista Mobile-sovellukset, jotka voit jatkaa senkin jälkeen, kun kokeiluversion päättyy. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio Xamarin kanssa. Saat ohjeet [asennus ja asenna Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) . 

* Mac Xcode v7.0 tai uudempi versio ja Xamarin Studio yhteisön asennettuna. Katso [asennus ja asenna Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) ja [asennuksen ja asennuksen tarkastukset for Mac-käyttäjän](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).
 
>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](https://tryappservice.azure.com/?appServiceName=mobile), jossa lyhytkestoinen starter Mobile-sovelluksen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="create-a-new-azure-mobile-app-backend"></a>Luo uusi Azure Mobile-sovelluksen taustassa

Luo uusi Mobile-sovelluksen taustassa seuraavasti.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


Voit nyt valmisteltu Azure Mobile-sovelluksen-taustatietokannan, joka voidaan käyttää kaikkia mobiilisovelluksen-sovellusten ominaisuuksia. Seuraavaksi lataat yksinkertainen "todo luettelon" palvelimen projektille Taustajärjestelmä ja julkaista sen Azure.

## <a name="configure-the-server-project"></a>Määritä server-projekti

Noudata seuraavia ohjeita voit määrittää palvelimen projektin käyttämään Node.js tai .NET Taustajärjestelmä.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-xamarinforms-solution"></a>Lataa ja suorita Xamarin.Forms ratkaisu

Tässä on muutama vaihtoehtojen. Voit ladata ratkaisun Mac-tietokoneeseen ja avaa se Xamarin Studiossa tai voit ladata ratkaisun Windows-tietokoneeseen ja Avaa Visual Studiossa käyttäminen verkossa Mac etsimisen iOS-sovellukseen. Katso [asennus ja asenna Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) , jos tarvitset Xamarin asennuksen käyttötavoista tarkempia ohjeita.

Oletetaan, että jompaankumpaan:

 1. Mac-tietokoneeseen tai Windows-tietokoneessa Avaa [Azure Portal] -selainikkunassa.
 2. Valitse Mobile-sovelluksen asetukset-sivu (Matkapuhelin)-kohdassa **Aloita** > **Xamarin.Forms**. Valitse vaiheessa 3 **Luo uuden sovelluksen** , jos se ei ole jo avattuna.  Seuraavaksi napsauttamalla **Lataa** -painiketta.

    Tämä Lataa projekti, joka sisältää asiakassovellus, joka on liitetty mobile-sovellus. Pakattu projektitiedoston tallentaminen omaan tietokoneeseesi ja Merkitse muistiin, johon tallennat sen.

 3. Pura projekti, jota olet ladannut ja avaa se sitten Xamarin Studiossa tai Visual Studio.

    ![][9]

    ![][8]


##<a name="optional-run-the-ios-project"></a>(Valinnainen) Suorita iOS-projekti

Tässä osassa on käynnissä iOS-laitteiden Xamarin iOS-projekti. Voit ohittaa tämän osan, jos käsittelet ei iOS-laitteita.

####<a name="in-xamarin-studio"></a>Xamarin Studiossa

1. IOS-projektin hiiren kakkospainikkeella ja valitse sitten **Määritä kuin käynnistys projektin**.
2. Valitse **Suorita** projektin luominen ja Käynnistä sovellus iPhone emulaattori **Käynnistä virheenkorjaus** .

####<a name="in-visual-studio"></a>Visual Studiossa
1. IOS-projektin hiiren kakkospainikkeella ja valitse sitten **Aseta projektin aloitus**.
2. Valitse **Muodosta** **Määritysten hallinta**.
3. Valitse **Määritysten hallinta** -valintaikkunassa iOS-projektin **Muodosta** ja **Ota käyttöön** -valintaruudut.
4. Paina muodosta projektin ja Käynnistä sovellus iPhone emulaattori **F5** -näppäintä.

    >[AZURE.NOTE] Jos sinulla on ongelmia luomisesta, suorita NuGet pakettien hallinta ja Xamarin uusimman päivityksen tukevat paketit. Joskus pikaopas projektien voi esittää on päivitetty uusimmat.    

-Sovellukseen, kirjoita kuvaava teksti, kuten _Tietoja Xamarin_ ja valitse sitten **+** painiketta.

![][10]

Kirjaa pyyntö lähetetään ylläpidettävä Azure uusi mobiilisovelluksen taustaan. Pyynnön tiedot lisätään TodoItem-taulukkoon. Kohteet, jotka on tallennettu taulukkoon palauttamat mobiilisovelluksen Taustajärjestelmä ja tiedot näytetään luettelossa.

>[AZURE.NOTE]
> Löydät koodi, joka noutaa mobiilisovelluksen-taustatietokannan kannettavat luokan kirjaston project-ratkaisun TodoItemManager.cs C#-tiedostoon.

##<a name="optional-run-the-android-project"></a>(Valinnainen) Suorita Android projekti

Tässä osassa on käynnissä Xamarin droid projektin androidille. Voit ohittaa tämän osan, jos käsittelet ei Android-laitteisiin.

####<a name="in-xamarin-studio"></a>Xamarin Studiossa

1. Android projektin hiiren kakkospainikkeella ja valitse sitten **Määritä kuin käynnistys projektin**.
2. Valitse **Suorita** projektin luominen ja Käynnistä sovellus Android emulaattorin **Käynnistä virheenkorjaus** .

####<a name="in-visual-studio"></a>Visual Studiossa
1. Android (Droid)-projekti hiiren kakkospainikkeella ja valitse sitten **Aseta projektin aloitus**.
4. Valitse **Muodosta** **Määritysten hallinta**.
5. Valitse **Määritysten hallinta** -valintaikkunassa Android projektin **Muodosta** ja **Ota käyttöön** -valintaruudut.
6. Painamalla **F5** -näppäintä, voit luoda projektin ja aloittaa sovelluksen Android emulaattorin.

    >[AZURE.NOTE] Jos sinulla on ongelmia luomisesta, suorita NuGet pakettien hallinta ja Xamarin uusimman päivityksen tukevat paketit. Joskus pikaopas projektien voi esittää on päivitetty uusimmat.    


-Sovellukseen, kirjoita kuvaava teksti, kuten _Tietoja Xamarin_ ja valitse sitten **+** painiketta.

![][11]

Kirjaa pyyntö lähetetään ylläpidettävä Azure uusi mobiilisovelluksen taustaan. Pyynnön tiedot lisätään TodoItem-taulukkoon. Kohteet, jotka on tallennettu taulukkoon palauttamat mobiilisovelluksen Taustajärjestelmä ja tiedot näytetään luettelossa.

> [AZURE.NOTE]
> Löydät koodi, joka noutaa mobiilisovelluksen-taustatietokannan kannettavat luokan kirjaston project-ratkaisun TodoItemManager.cs C#-tiedostoon.


##<a name="optional-run-the-windows-project"></a>(Valinnainen) Suorita Windows-projekti


Tässä osassa on käynnissä Windows laitteet Xamarin WinApp projektin. Voit ohittaa tämän osan Jos eivät toimi Windows-laitteiden kanssa.


####<a name="in-visual-studio"></a>Visual Studiossa
1. Mitä tahansa Windows-projektien hiiren kakkospainikkeella ja valitse sitten **Aseta projektin aloitus**.
4. Valitse **Muodosta** **Määritysten hallinta**.
5. Valitse **Määritysten hallinta** -valintaikkunassa Windows-projekti, jonka valitsit **Muodosta** ja **Ota käyttöön** -valintaruudut.
6. Paina muodosta projektin ja Käynnistä sovellus Windows-emulaattorin **F5** -näppäintä.

    >[AZURE.NOTE] Jos sinulla on ongelmia luomisesta, suorita NuGet pakettien hallinta ja Xamarin uusimman päivityksen tukevat paketit. Joskus pikaopas projektien voi esittää on päivitetty uusimmat.    


-Sovellukseen, kirjoita kuvaava teksti, kuten _Tietoja Xamarin_ ja valitse sitten **+** painiketta.

Kirjaa pyyntö lähetetään ylläpidettävä Azure uusi mobiilisovelluksen taustaan. Pyynnön tiedot lisätään TodoItem-taulukkoon. Kohteet, jotka on tallennettu taulukkoon palauttamat mobiilisovelluksen Taustajärjestelmä ja tiedot näytetään luettelossa.

![][12]

> [AZURE.NOTE]
> Löydät koodi, joka noutaa mobiilisovelluksen-taustatietokannan kannettavat luokan kirjaston project-ratkaisun TodoItemManager.cs C#-tiedostoon.

##<a name="next-steps"></a>Seuraavat vaiheet

* [Käyttöoikeuksien lisääminen sovelluksen](app-service-mobile-xamarin-forms-get-started-users.md)  
Opettele todennetaan Liittoutuminen sovelluksesi käyttäjät.

* [Sovelluksen lisääminen push-ilmoitukset](app-service-mobile-xamarin-forms-get-started-push.md)  
Opettele lisäämään push-ilmoitusten tuki, kun sovellus ja määritä Mobile-sovelluksen-taustatietokannan push-ilmoitukset lähetetään Azure ilmoituksen keskittimet avulla.

* [Sovelluksen offline-synkronoinnin ottaminen käyttöön](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Opettele lisäämään offline-tuki käyttämällä Mobile-sovelluksen Taustajärjestelmä sovelluksen. Offline-synkronoinnin sallii loppukäyttäjien kanssa mobiilisovelluksessa&mdash;tarkasteleminen, lisääminen tai muokkaaminen tietojen&mdash;myös silloin, kun yhteyttä ei ole verkossa.

* [Miten voit käyttää hallitun asiakkaan Azure-mobiilisovellukset](app-service-mobile-dotnet-how-to-use-client-library.md)  
Lue, miten hallitun asiakkaan SDK Xamarin-sovelluksen käyttöä varten. 


<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure Portal]: https://portal.azure.com/

