<properties
    pageTitle="Luo yleinen Windows Platform (UWP), joka käyttää Mobile-sovellusten | Microsoft Azure"
    description="Noudata tässä opetusohjelmassa yleinen Windows Platform (UWP)-sovelluksen kehittämiseen C#, Visual Basic tai JavaScript Azure mobiilisovelluksen backends avulla pääset alkuun."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-windows-app"></a>Windows-sovelluksen luominen

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa näytetään, miten voit lisätä pilvipohjainen Taustajärjestelmä palvelu yleinen Windows Platform (UWP)-sovelluksen. Lisätietoja on artikkelissa [mitä Mobile-sovellukset](app-service-mobile-value-prop.md). Näyttökuvien valmiin sovelluksen ovat seuraavat:

![Valmiit työpöytäsovellus](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Käynnissä työpöydän. 

![Valmiit phone-sovellus](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Käynnissä puhelimeen

Noudata tässä opetusohjelmassa on UWP sovellusten kaikkiin muihin Mobile-sovelluksen opetusohjelmiin. 

##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

* Azure active tili. Jos sinulla ei ole tiliä, voit Azure-kokeiluversioon rekisteröityminen ja enintään 10 vapaa-mobiilisovellusten hankkiminen, voit jatkaa senkin jälkeen, kun kokeiluversion päättyy. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

* [Visual Studio yhteisön 2015] tai sitä uudempi versio.

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun, ennen kuin kirjaudut Azure-tili, siirry [Yritä App kääntäjä](https://tryappservice.azure.com/?appServiceName=mobile). Siellä voit luoda lyhytkestoinen starter mobiilisovelluksessa heti App palvelussa – ei luottokortti ja ei ole sitoumukset.

##<a name="create-a-new-azure-mobile-app-backend"></a>Luo uusi Azure Mobile-sovelluksen taustassa

Luo uusi Mobile-sovelluksen taustassa seuraavasti.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Voit nyt valmisteltu Azure Mobile-sovelluksen-taustatietokannan, joka voidaan käyttää kaikkia mobiilisovelluksen-sovellusten ominaisuuksia. Seuraavaksi lataat yksinkertainen "todo luettelon" palvelimen projektille Taustajärjestelmä ja julkaista sen Azure.

## <a name="configure-the-server-project"></a>Määritä server-projekti

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-client-project"></a>Lataa ja suorita asiakkaan projekti

Kun olet määrittänyt Mobile-sovelluksen taustatietokannan, voit luoda uuden asiakas-sovelluksen tai olemassa olevan sovelluksen muodostaa Azure muokkaaminen. Tässä osassa voit ladata UWP sovelluksen malliprojekti, joka on mukautettu muodostaa Mobile-sovelluksen Taustajärjestelmä.

1. Takaisin sisään for Mobile-sovelluksen Taustajärjestelmä **Pika-aloitusopas** -sivu, valitse **Luo uusi sovellus** > **Lataa**ja valitse Pura pakattu projektitiedostoja paikalliseen tietokoneeseen.

    ![Lataa Windows pikaopas project](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

3. (Valinnainen) Lisää UWP app project Serverin projektiksi samaan ratkaisuun. Tämä on helppo korjata ja esikatsella sekä sovellus että taustaan samaan Visual Studio-ratkaisuun, jos haluat tehdä. Voit lisätä UWP sovelluksen projektin ratkaisun, sinun on käytettävä Visual Studio 2015 tai sitä uudempi versio.

4. UWP-sovelluksen käynnistys-projektin painamalla F5-näppäintä ja suorita sovellus.

5. -Sovellukseen Kirjoita kuvaileva teksti *valmiina opetusohjelman*, kuten **lisätä TodoItem** -tekstiruutuun ja valitse sitten **Tallenna**.

    ![Pikaopas koko työpöydän](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Kirjaa pyyntö lähetetään uusi mobiilisovelluksen taustatietokannan, joka isännöi Azure-tietokannassa.

6. (Valinnainen) Lopeta sovellus ja käynnistä se toisen laitteen tai matkapuhelimen emulaattorin.

    ![Pikaopas valmis online](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Huomaa, että edellisessä vaiheessa tallennetut tiedot on ladattu Azure, kun UWP sovellus käynnistyy. 

##<a name="next-steps"></a>Seuraavat vaiheet

* [Käyttöoikeuksien lisääminen sovelluksen](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Opettele todennetaan Liittoutuminen sovelluksesi käyttäjät.

* [Sovelluksen lisääminen push-ilmoitukset](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Opettele lisäämään push-ilmoitusten tuki, kun sovellus ja määritä Mobile-sovelluksen-taustatietokannan push-ilmoitukset lähetetään Azure ilmoituksen keskittimet avulla.

* [Sovelluksen offline-synkronoinnin ottaminen käyttöön](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Opettele lisäämään offline-tuki käyttämällä Mobile-sovelluksen Taustajärjestelmä sovelluksen. Offline-synkronoinnin sallii loppukäyttäjien kanssa mobiilisovelluksessa&mdash;tarkasteleminen, lisääminen tai muokkaaminen tietojen&mdash;myös silloin, kun yhteyttä ei ole verkossa.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio yhteisön 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
